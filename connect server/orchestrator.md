
sudo nano /etc/systemd/system/orchestrator.service

```
[Service]
User=orchestrator
Group=orchestrator
WorkingDirectory=/opt/orchestrator/publish
ExecStart=/opt/orchestrator/publish/Orchestrator

# Куда распаковывать self-contained bundle
Environment=DOTNET_BUNDLE_EXTRACT_BASE_DIR=/var/tmp/orch-bundle

# Слушаем прод-порт
Environment=ASPNETCORE_URLS=https://0.0.0.0:8444

# --- Сертификаты (как в твоём appsettings для Linux) ---
Environment=Certificates_Tls_Path=/etc/apigw/certs/orch-server.pfx
Environment=Certificates_Tls_Password=OrchPass123

Environment=Certificates_Signing_Path=/etc/apigw/certs/orch-client.pfx
Environment=Certificates_Signing_Password=OrchPass123

Environment=Certificates_Client_UseSeparateClientCert=true
Environment=Certificates_Client_Path=/etc/apigw/certs/orch-client.pfx
Environment=Certificates_Client_Password=OrchPass123

Environment=Tls__CustomRootPath=/etc/apigw/certs/ca.crt
Environment=Tls__CustomClientRootPath=/etc/apigw/certs/ca.crt
Environment=Orchestrator_Jwt_TrustedKeysPath=/etc/apigw/trusted-keys

# Базовый URL сервиса
Environment=Orchestrator__BaseUrl=https://10.100.191.25:8444


Restart=always
RestartSec=5
# логирование в journald
StandardOutput=journal
StandardError=journal

[Install]
WantedBy=multi-user.target

```



