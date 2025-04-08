# [SberPaySdkAndroidDoc](https://sdkpay.github.io/SberPaySdkAndroidDoc/)

#### [Бординг](https://sdkpay.github.io/SberPaySdkAndroidDoc/boarding) | [Регистрация заказов в платежном шлюзе Сбера](https://sdkpay.github.io/SberPaySdkAndroidDoc/order_registration) | [Начало работы](https://sdkpay.github.io/SberPaySdkAndroidDoc/start) | [Сценарии оплаты через SDK](https://sdkpay.github.io/SberPaySdkAndroidDoc/payment_script) | [Работа в режиме посочницы](https://sdkpay.github.io/SberPaySdkAndroidDoc/sandbox_mode) | [Вспомогательные структуры данных](https://sdkpay.github.io/SberPaySdkAndroidDoc/data_structures) | [Актуальная версия SDK](https://sdkpay.github.io/SberPaySdkAndroidDoc/version) | [Поддержка](https://sdkpay.github.io/SberPaySdkAndroidDoc/support) | [FAQ](https://sdkpay.github.io/SberPaySdkAndroidDoc/faq)

# Работа в режиме песочницы

> В данном режиме не отображаются настоящие данные клиента

Песочница - режим работы SDK, который необходим для отладки процесса оплаты. В режиме песочницы есть возможность пройти весь процесс оплаты через SDK в Вашем приложении без регистрации реальных заказов. Это можно реализовать двумя способами.  
Рассмотрите первый простой вариант, он подойдет для тех, кто не интегрирован с платежным шлюзом Банка и ищет способ протестировать визуальную составляющую продукта. Реализуете сценарий «Оплата вне SDK».  
Если вы интегрированы с платежным шлюзом Сбера, то мы рекомендуем второй вариант - использовать идентификаторы заказов в Платежном шлюзе Банка из тестового окружения. Обратите внимание, что в зависимости от того, с какой именно версией протокола вы интегрированы могут потребоваться отдельные учетные данные для регистрации заказов

## Необходимые данные для тестирования

Для получения доступа к песочнице Вам необходимо отправить запрос на почту *support@ecom.sberbank.ru*. В теме письма обязательно указать "Получение доступа к песочнице Sandbox SDK SberPay In-App"

## Реализация в коде

В объект класса [SPaySdkInitConfig](https://sdkpay.github.io/SberPaySdkAndroidDoc/data_structures#spaysdkinitconfig) передайте в параметр `stage` одно из возможных значений enum класса **[SPayStage](https://sdkpay.github.io/SberPaySdkAndroidDoc/data_structures#spaystage)**
