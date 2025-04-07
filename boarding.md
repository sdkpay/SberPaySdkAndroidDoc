# [SberPaySdkAndroidDoc](https://sdkpay.github.io/SberPaySdkAndroidDoc/)

#### [Бординг](https://sdkpay.github.io/SberPaySdkAndroidDoc/boarding) | [Регистрация заказов в платежном шлюзе Сбера](https://sdkpay.github.io/SberPaySdkAndroidDoc/order_registration) | [Начало работы](https://sdkpay.github.io/SberPaySdkAndroidDoc/start) | [Сценарии оплаты через SDK](https://sdkpay.github.io/SberPaySdkAndroidDoc/payment_script) | [Работа в режиме посочницы](https://sdkpay.github.io/SberPaySdkAndroidDoc/sandbox_mode) | [Вспомогательные структуры данных](https://sdkpay.github.io/SberPaySdkAndroidDoc/data_structures) | [Актуальная версия SDK](https://sdkpay.github.io/SberPaySdkAndroidDoc/version) | [Поддержка](https://sdkpay.github.io/SberPaySdkAndroidDoc/support) | [FAQ](https://sdkpay.github.io/SberPaySdkAndroidDoc/faq)

# Бординг

## Схемы получения учетных записей

![Схема взаимодействия](/docs/assets/img/scheme.png)

Варианты использования учетных данных для удобного распределения финансовых потоков на стороне партнера

![Учетные данные](/docs/assets/img/scheme-creds.png)

Описание получения параметров авторизации (apikey, merchantLogin, deeplink), необходимых для интеграции с SDK SberPay

![Процесс оплаты](/docs/assets/img/scheme-payment.png)

## Настройка доступов для проведения оплаты

### Получения учетных записей для проведения оплаты﻿

Запрос в support на адрес support@ecom.sberbank.ru

## Как получить apikey

### Получение apiKey через API

[Ссылка](https://ecomtest.sberbank.ru/doc#tag/changePasswordServices/operation/generateApiKey) на иструкцию по использованию метода

### Получение apiKey через СберБизнес

[Инструкция](https://developers.sber.ru/docs/assets/files/apikey-2712ab7639c827586f0304625dcddef5.pdf) для получения apiKey

## Настройка доступов для тестирования оплаты

Параметры для авторизации через тестовую среду

### Параметры для регистрации тестового заказа

- мерчант 781000037371-20278739
- API: 781000037371-20278739-api
- Оператор: 781000037371-20278739-operator
- Пароль на оба логина: 781000037371-20278739

### Параметры для оплаты в SDK

- Api-key: AN+1efbOj0gRlvKsOcKqPNwAAAAAAAAADA75DovvlpSfYAVNL2/CkvTa+i/B7Rn9+uZ62ufbZWJZEfdST/orBr2U6pwjJKnpKrEiYQvT2Q7JJWzLZMTbQh4IMBt1q2kIM9C9mEFU+cCm9rg2JCLGcUJ9kxct2qYn5iMzYsNxKkD5seYzYJZnKh+fHd1WF5ScQTGTPlskLbnu8DGbxtv/n6rmvQ==
- MerchantLogin: 781000037371-20278740-ecomSdk
 