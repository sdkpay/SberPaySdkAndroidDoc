# [SberPaySdkAndroidDoc](https://sdkpay.github.io/SberPaySdkAndroidDoc/)

#### [Бординг](https://sdkpay.github.io/SberPaySdkAndroidDoc/boarding) | [Регистрация заказов в платежном шлюзе Сбера](https://sdkpay.github.io/SberPaySdkAndroidDoc/order_registration) | [Начало работы](https://sdkpay.github.io/SberPaySdkAndroidDoc/start) | [Сценарии оплаты через SDK](https://sdkpay.github.io/SberPaySdkAndroidDoc/payment_script) | [Работа в режиме посочницы](https://sdkpay.github.io/SberPaySdkAndroidDoc/sandbox_mode) | [Вспомогательные структуры данных](https://sdkpay.github.io/SberPaySdkAndroidDoc/data_structures) | [Актуальная версия SDK](https://sdkpay.github.io/SberPaySdkAndroidDoc/version) | [Поддержка](https://sdkpay.github.io/SberPaySdkAndroidDoc/support) | [FAQ](https://sdkpay.github.io/SberPaySdkAndroidDoc/faq)

# Регистрация заказов в платежном шлюзе Сбера

#### [Для платежного шлюза с эндпоинтом 3dsec.sberbank.ru]()
#### [Для платежного шлюза с эндпоинтом ecommerce.sberbank.ru]()
#### [Для подключения на платежном шлюзе ЮМани]()

## Для платежного шлюза с эндпоинтом 3dsec.sberbank.ru и ecommerce.sberbank.ru

В сценариях автоматической и ручной оплаты с SDK необходимо передавать значение атрибута `sbolBankInvoiceId` – это уникальный идентификатор заказа, сгенерированный Банком. Его значение можно получить в ответе на запрос создания заказа (*register.do* или *registerPreAuth.do*) в Платежном Шлюзе Сбербанка и только при условии, что вы корректно указали дополнительные параметры в теле своего запроса

Вне зависимости от того, используете ли вы одностадийные (*register.do*) или двухстадийные запросы (*registerPreAuth.do*), необходимо добавить дополнительные параметры в теле запроса в структуру `jsonParams`:

|Название|Тип|Обязательно|Описание|
|---|:---:|:---:|---|
|app2app|Boolean|Да|Для работы с SDK необходимо передавать значение true. Для использования этого параметра должна быть включена пермиссия|
|app.osType|ANS..32|Да|Тип ОС. Возможные значения: ios, android|
|app.deepLink|ANS..255|Да|Ссылка на приложение мерчанта. Не влияет на использование SDK. Рекомендуется указывать схему своего приложения|

<br>

> При корректном вызове создания заказа ожидается, что в ответе будет получено необходимо значение `sbolBankInvoiceId` в структуре `externalParams`

### Пример запроса

```
{
  "userName": "testUserName",
  "password": "testPassword",
  "orderNumber": "e2574f1785324f1592d9029cb05adbbd",
  "amount": 19900,
  "returnUrl": "https://testmerchant.ru/return",
  "jsonParams": {
    "app2app": true,
    "app.deepLink": "android-app://ru.testbankmobile/main"
  }
}
```

### Пример запроса в curl

```
curl --location --request POST 'ecommerce.sberbank.ru/ecomm/gw/partner/api/v1/register.do' \
--header 'Content-Type: application/json' \
--data-raw '{
    "userName": "sbertest_0244",
    "password": "sbertest_024412345",
    "orderNumber": "AL1223201123223",
    "amount": 100,
    "currency": "643",
    "returnUrl": "https://ya.ru",
    "failUrl": "http://example.com/fail",
    "description": "регистрация заказа с максимальным набором полей",
    "language": "ru",
    "pageView": "DESKTOP",
    "clientId": "1080735968",
    "merchantLogin": "sbertest_0244",
    "jsonParams": {
        "osType": "ios",
        "app.deepLink": "yoomoney://checkout-info/v3",
        "app2app": "true",
        "web2app": "false"
    },
    "sessionTimeoutSecs": 6000,
    "features": "FORCE_SSL",
    "expirationDate": "2023-12-16T23:59:59",
    "phone": "79959977757",
    "email": "test@example.com"
}
```

### Пример ответа

```
{
  "errorCode": "0",
  "errorMessage": "Обработка запроса прошла без системных ошибок",
  "orderId": "a67b0ced-c9a4-4cfb-bce3-b9595afaafc1",
  "formUrl": "https://ecomtest.sberbank.ru/pp/pay_ru?orderId=a67b0ced-c9a4-4cfb-bce3-b9595afaafc1",
  "externalParams": {
    "sbolDeepLink": "sberpay://invoicing/v2?bankInvoiceId=a67b0cedc9a44cfbbce3b9595afaafc1&operationType=Web2App&option=Connect",
    "sbolBankInvoiceId":"a67b0cedc9a44cfbbce3b9595afaafc1"
  }
}
```

Параметры `externalParams`:

|Название|Тип|Обязательно|Описание|
|---|:---:|:---:|---|
|sbolDeepLink|ANS..1024|Да|Ссылка на приложение Банка для завершения оплаты. При внедрении SDK не используется|
|sbolBankInvoiceId|ANS..1024|Да|Уникальный идентификатор заказа, сгенерированный Банком. **Это значение необходимо передавать при обращении к SDK**|

<br>

> По всем возникшим вопросам по регистрации обращайтесь на почтовый адрес: support@ecom.sberbank.ru

### Регистрация заказов в платежном шлюзе Сбера в песочнице

В сценариях автоматической и ручной оплаты с SDK необходимо передавать значение атрибута `sbolBankInvoiceId` – это уникальный идентификатор заказа, сгенерированный Банком. Его значение можно получить в ответе на запрос создания заказа (`register.do` или `registerPreAuth.do`) в Платежном Шлюзе Сбербанка и только при условии, что вы корректно указали дополнительные параметры в теле своего запроса

Для регистрации заказа в песочнице для эндпоинта *3dsec.sberbank.ru* использовать: https://3dsec.sberbank.ru/payment/rest/register.do

> Общая документация по работе со шлюзом находится по адресу: https://securepayments.sberbank.ru/wiki/doku.php/main_page

<br>

Для регистрации заказа в песочнице для эндпоинта *ecommerce.sberbank.ru* использовать:
https://ecomift.sberbank.ru/ecomm/gw/partner/api/v1/register.do

> Общая документация по работе со шлюзом находится по адресу: https://ecomtest.sberbank.ru/doc#tag/basicServices/operation/register

## Для подключения на платежном шлюзе ЮМани

### Требования

Для проведения оплаты необходимо:
- Знать свой api key для текущего shop id Юмани
- Знать merchantLogin для инициации библиотеки SDK
- Иметь возможность подключения по api к шлюзу ЮМани

> При отстутсвии одного из требований необходимо обратиться в [чат поддержки](https://yookassa.ru/yooid/signin/step/login) пользователей в личном кабинете ЮМани

### Регистрация заказа и оплата

Создаем платеж с помощью [API](https://yookassa.ru/developers/payment-acceptance/getting-started/payment-process#create-payment) с обязательным указанием и заполнением следующих реквизитов в теле запроса:

```
"payment_method_data": {
    "type": "sberbank"
},
"confirmation": {
    "type": "mobile_application",
    "return_url": "https://www.example.com/return_url"
}

```

Пример ответа:

```
"created_at": "2024-06-24T09:12:03.438Z",
"confirmation": {
    "type": "mobile_application",
    "confirmation_url": "sberpay://invoicing/v2?bankInvoiceId=f87c3f08938f48d58ad97b22c939d0d8&operationType=App2App&payment_id=2e0b4c23-000f-5000-a000-1da834a56598"
}
```

Далее необходимо инициировать оплату в SDK с помощью `apikey`, `merchantLogin` и полученного из запроса выше `bankInvoiceId`

### Информация о статусе платежа

Можно получить с помощью:
- C помощью [webhook от платежного шлюза ЮМани](https://yookassa.ru/developers/using-api/webhooks)
- C помощью самостоятельной проверки статуса оплаты по [API](https://yookassa.ru/developers/payment-acceptance/getting-started/payment-process#payment-statuses)