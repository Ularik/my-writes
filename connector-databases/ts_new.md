
### **Владельцы и участники (Owners & Entities)**

- `relation_entity` - универсальная сущность (физическое/юридическое лицо)
- `person_info` - персональные данные физ. лиц (ПИН, ФИО, документы)
- `organization` - организации (ИНН, название)
- `car_co_owners` - совладельцы автомобилей

### **Автомобили (Cars)**

- `car` - основные данные автомобиля (VIN, год, страна, статус)
- `car_history` - история регистраций автомобиля (связь с владельцами, тех. характеристики)
- `car_checkup` - техосмотры
- `el_passport_car` - электронные паспорта

### **Гос. номера (License Plates)**

- `car_gov_plate` - выданные гос. номера
- `gov_plate` - справочник гос. номеров
- `pre_car_gov_plate` - предварительные номера
- `used_gov_plate` - использованные номера
- `foreigner_gov_plate` - иностранные номера

### **Регистрационные действия (Registration)**

- `registration_record` - основная сущность регистрации
- `record_log` - лог изменений регистраций
- `record_delete` - отмененные регистрации
- `record_user_confirms` - подтверждения пользователей

### **Документы (Documents)**

- `certificate` - свидетельства (ТС, регистрации)
- `document_info` - документы удостоверяющие личность
- `reg_document` - документы для регистрации
- `tinting_window_documents` - документы на тонировку

### **Платежи (Payments)**

- `reg_record_payment` - платежи за регистрацию
- `payment_type` - типы платежей
- `payment_type_cost` - стоимость платежей
- `receive_payment` - полученные платежи

### **Тонировка (Tinting)**

- `tinting_window` - разрешения на тонировку
- `tinting_window_payment` - платежи за тонировку
- `cloud_tinting_window_sign` - ЭЦП для тонировки

### **Аресты и залоги (Arrests & Pledges)**

- `arest_pledge` - аресты и залоги ТС
- `arest_pledge_issuance` - выдача арестов

### **Справочники (References)**

- `ref_car_brand`, `ref_car_model` - марки и модели
- `ref_car_color` - цвета
- `ref_ate` - административно-территориальные единицы
- `ref_document_type` - типы документов