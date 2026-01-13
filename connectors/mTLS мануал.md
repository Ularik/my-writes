Создаем файл и вставляем этот скрипт, запускаем его
```
#!/bin/bash
set -e

# Папка для сертификатов
CERTS_DIR=certs
mkdir -p $CERTS_DIR
cd $CERTS_DIR

echo "👉 Создаём CA (Certificate Authority)..."
openssl genrsa -out ca.key 4096
openssl req -x509 -new -nodes -key ca.key -sha256 -days 3650 \
  -out ca.crt -subj "/CN=MyCA"

echo "👉 Создаём серверный ключ и сертификат..."
openssl genrsa -out server.key 2048
openssl req -new -key server.key -out server.csr -subj "/CN=172.20.35.133"
openssl x509 -req -in server.csr -CA ca.crt -CAkey ca.key \
  -CAcreateserial -out server.crt -days 365 -sha256

echo "👉 Создаём клиентский ключ и сертификат..."
openssl genrsa -out client.key 2048
openssl req -new -key client.key -out client.csr -subj "/CN=test-client"
openssl x509 -req -in client.csr -CA ca.crt -CAkey ca.key \
  -CAcreateserial -out client.crt -days 365 -sha256

echo "✅ Готово! Все файлы лежат в папке $CERTS_DIR:"
ls -l
```

cp ca.pem /mnt/c/Users/SAMURAI/Downloads/

### Создали СЕРТИФИКАТ
```
# 1. Приватный ключ CA
openssl genrsa -out ca.key 4096
chmod 600 ca.key
```
### Создали конфиг файл для сертификата
```
# 2. Конфиг CA
cat > ca.cnf <<'EOF'
[ req ]
distinguished_name = dn
x509_extensions    = v3_ca
prompt             = no
[ dn ]
CN = CyberBars Internal CA
O  = CyberBars
C  = KG
[ v3_ca ]
basicConstraints       = critical, CA:true, pathlen:1
keyUsage               = critical, keyCertSign, cRLSign
subjectKeyIdentifier   = hash
authorityKeyIdentifier = keyid:always,issuer
EOF
```

### Создали публичный сертификат из предыдущих сертификатов
```
# 3. Самоподписанный сертификат CA (на 10 лет)
openssl req -x509 -new -key ca.key -out ca.crt -days 3650 -config ca.cnf
chmod 644 ca.crt
```

## Для стороны с оркестратором

### Создали привытный ключ оркестратора
```
Серверный сертификат Оркестратора (109)

→ orch-server.key + orch-server.crt + orch-server.pfx

👉 Используется Kestrel на 109. SAN обязательно должен содержать IP 192.168.0.109.

# 1. Приватный ключ
openssl genrsa -out orch-client.key 2048
```

### Создали из него публичный ключ
```
# 2. CSR
openssl req -new -key orch-client.key -out orch-client.csr -subj "/CN=10.100.191.25"
```

**Примечание**. Обязательно указать в CN=IP - свой ip на котором будет лежать оркестратор 
### Создали конфиг для публичного ключа
```
# 3. Расширения
cat > orch-client_ext.cnf <<'EOF'
basicConstraints = CA:FALSE
keyUsage = critical, digitalSignature, keyEncipherment
extendedKeyUsage = clientAuth
subjectAltName = @alt_names
[alt_names]
IP.1 = 10.100.191.25
DNS.1 = orchestrator.local
EOF
```

### Подписываем публичный ключ с помощью конфига и сертификатов
```
# 4. Подписываем CA
openssl x509 -req -in orch-client.csr -CA ca.crt -CAkey ca.key -CAcreateserial \
  -out orch-client.crt -days 1095 -sha256 -extfile orch-client_ext.cnf
```

### (Опционально) создали из него pfx
```
# 5. Создаём PFX для Windows (с цепочкой CA)
openssl pkcs12 -export -inkey orch-client.key -in orch-client.crt -certfile ca.crt \
  -out orch-client.pfx -password pass:OrchPass123

```

### Для коннектора то же самое 

```
→ connector-server.key + connector-server.crt

👉 Для HTTPS браузеров (порт 443) на 111. Можно использовать IP или домен.
Предположим, IP = 10.100.123.156

# 1. Приватный ключ
openssl genrsa -out client-orch-local.key 2048

# 2. CSR
openssl req -new -key client-orch-local.key -out client-orch-local.csr -subj "/CN=10.100.123.156"

# 3. Расширения
cat > client-orch-local.cnf <<'EOF'
basicConstraints = CA:FALSE
keyUsage = critical, digitalSignature, keyEncipherment
extendedKeyUsage = clientAuth
subjectAltName = @alt_names
[alt_names]
IP.1 = 10.100.123.156
DNS.1 = client-orch-local.local
EOF

# 4. Подписываем CA
openssl x509 -req -in client-orch-local.csr -CA ../ca.crt -CAkey ../ca.key -CAcreateserial \
  -out client-orch-local.crt -days 1095 -sha256 -extfile client-orch-local.cnf

chmod 600 connector.key
chmod 644 connector.crt

```


```
→ connector-server.key + connector-server.crt

# 1. Приватный ключ
openssl genrsa -out back-client.key 2048
openssl genrsa -out el.cert.gov.key 2048

# 2. CSR
openssl req -new -key back-server.key -out back-server.csr -subj "/CN=10.100.191.25"
openssl req -new -key el.cert.gov.key -out el.cert.gov.csr -subj "/CN=213.109.67.185"

# 3. Расширения
cat > el.cert.gov.cnf <<'EOF'
basicConstraints = CA:FALSE
keyUsage = critical, digitalSignature, keyEncipherment
extendedKeyUsage = serverAuth
subjectAltName = @alt_names
[alt_names]
IP.1 = 213.109.67.185
DNS.1 = el.cert.gov.local
EOF

cat > back-client.cnf <<'EOF'
basicConstraints = CA:FALSE
keyUsage = critical, digitalSignature, keyEncipherment
extendedKeyUsage = clientAuth
subjectAltName = @alt_names
[alt_names]
IP.1 = 10.100.191.25
DNS.1 = back-client.local
EOF

# 4. Подписываем CA
openssl x509 -req -in el.cert.gov.csr -CA ca.crt -CAkey ca.key -CAcreateserial \
  -out el.cert.gov.crt -days 1095 -sha256 -extfile el.cert.gov.cnf

openssl x509 -req -in back-client.csr -CA ../ca.crt -CAkey ../ca.key -CAcreateserial \
  -out back-client.crt -days 1095 -sha256 -extfile back-client.cnf
  

openssl pkcs12 -export -inkey client-orch-local.key -in client-orch-local.crt -certfile ../ca.crt \
  -out client-orch-local.pfx -password pass:Orch123!
  
openssl pkcs12 -export -inkey server-orch-local.key -in server-orch-local.crt -certfile ../ca.crt \
  -out server-orch-local.pfx -password pass:Orch123!


```