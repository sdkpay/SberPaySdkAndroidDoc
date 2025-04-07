# [SberPaySdkAndroidDoc](https://sdkpay.github.io/SberPaySdkAndroidDoc/)

##### [Начало работы](https://sdkpay.github.io/SberPaySdkAndroidDoc/start) | [Сценарии оплаты через SDK](https://sdkpay.github.io/SberPaySdkAndroidDoc/payment_script) | [Работа в режиме посочницы](https://sdkpay.github.io/SberPaySdkAndroidDoc/sandbox_mode) | [Вспомогательные структуры данных](https://sdkpay.github.io/SberPaySdkAndroidDoc/data_structures) | [Актуальная версия SDK](https://sdkpay.github.io/SberPaySdkAndroidDoc/version)

# Сценарии оплаты
#### [Автоматическая оплата](https://sdkpay.github.io/SberPaySdkAndroidDoc/payment_script#)
#### [Оплата без рефреш-токена](https://sdkpay.github.io/SberPaySdkAndroidDoc/payment_script#)
#### [Оплата частями с комиссией](https://sdkpay.github.io/SberPaySdkAndroidDoc/payment_script#)
#### [Оплата со списанием бонусов «Спасибо»](https://sdkpay.github.io/SberPaySdkAndroidDoc/payment_script#)

## Автоматическая оплата

### Сценарий автоматической оплаты

Если оплата происходит через эквайринг Сбербанка и уже известен **sbolBankInvoiceId**, тогда следует воспользоваться автоматической оплатой. Для этого необходимо воспользоваться методом `payWithBankInvoiceId`. Ниже представлен список параметров метода

|Объект|Тип|Формат|Обязательный|Описание|
|---|:---:|:---:|:---:|---|
|context|Context|-|Да|Любой контекст Вашего приложения который может запускать новую Activity|
|apiKey|String|ANS..512|Да|Ключ Клиента для работы с сервисами платежного шлюза через SDK|
|merchantLogin|String|ANS..512|Да|Логин партнера для работы с сервисами платежного шлюза|
|bankInvoiceId|String|ANS..512|Да|Уникальный номер (идентификатор) заказа в Платежном шлюзе Банка.Необходимо передавать значение sbolBankInvoiceId (передается в externalParams) из ответа на Запрос регистрации заказа|
|orderNumber|String|ANS..36|Да|Уникальный номер (идентификатор) заказа в системе Клиента. Используется для упрощенного поиска заказа в системе банка при выявлении дефектов|
|appPackage|String|ANS..512|Да|Package приложения (он же BuildConfig.APPLICATION_ID) Вашего приложения, по которому необходимо вернуть Плательщика после аутентификации в СберБанк Онлайн|
|language|String|A..2|Нет|Язык локализации интерфейсов.  Пример: RU|
|callback|(PaymentResult) -> Unit|-|Да|Результат выполнения метода оплаты.<br>Структура [PaymentResult](https://sdkpay.github.io/SberPaySdkAndroidDoc/data_structures#payemntresult)|

### Вызов метода `payWithBankInvoiceId`

Метод `payWithBankInvoiceId` является входной точкой в SDK и должен быть вызван только по клику на кнопку «Оплатить»

> Обязательно корректно укажите значение параметра `appPackage`. В противном случае при возврате из СБОЛа после оплаты, если будет допущена ошибка в схеме, поднимется шторка с вариантами приложений для продолжения оплаты. Таким образом, сценарий разорвется

### Kotlin

```
SPaySdkApp.getInstance().payWithBankInvoiceId(
    context = context,
    apiKey = API_KEY,
    merchantLogin = CLIENT_NAME,
    bankInvoiceId = BANK_INVOICE_ID,
    orderNumber = ORDER_NUMBER,
    appPackage = APP_PACKAGE,
    language = LANGUAGE,
) { paymentResult ->
    when (paymentResult) {
        is PaymentResult.Success -> {
            //do something on success
	        doOnSuccess()	
        }
        is PaymentResult.Error -> {
            //do something on error
	        doOnError(paymentResult.merchantError)
        }
        is PaymentResult.Processing -> {
            //do something on processing
             doOnProcessing();
        }
    }
}
```

### Java

```
SPaySdkApp.getInstance().payWithBankInvoiceId(
    context,
    API_KEY,
    CLIENT_NAME,
    BANK_INVOICE_ID,
    ORDER_NUMBER,
    APP_PACKAGE,
    LANGUAGE,
    paymentResult -> {
        if (paymentResult instanceof PaymentResult.Success) {
		    //do something on success
            doOnSuccess();
        } else if (paymentResult instanceof PaymentResult.Error) {
		    //do something on error
            doOnError(
               ((PaymentResult.Error) paymentResult).getMerchantErrors()
            );
        } else if (paymentResult instanceof PaymentResult.Processing) {
            //do something on processing
            doOnProcessing();
        }
        return null;
    }
);
```

## Оплата без рефреш-токена

### Сценарий оплаты без рефреш-токена

Для автоматической оплаты необходимо воспользоваться методом `payWithoutRefresh`. Ниже представлен список параметров метода. Ниже представлен список параметров метода

|Объект|Тип|Формат|Обязательный|Описание|
|---|:---:|:---:|:---:|---|
|context|Context|-|Да|Любой контекст Вашего приложения который может запускать новую Activity|
|apiKey|String|ANS..512|Да|Ключ Клиента для работы с сервисами платежного шлюза через SDK|
|merchantLogin|String|ANS..512|Да|Логин партнера для работы с сервисами платежного шлюза|
|bankInvoiceId|String|ANS..512|Да|Уникальный номер (идентификатор) заказа в Платежном шлюзе Банка.Необходимо передавать значение sbolBankInvoiceId (передается в externalParams) из ответа на Запрос регистрации заказа|
|orderNumber|String|ANS..36|Да|Уникальный номер (идентификатор) заказа в системе Клиента. Используется для упрощенного поиска заказа в системе банка при выявлении дефектов|
|appPackage|String|ANS..512|Да|Package приложения (он же BuildConfig.APPLICATION_ID) Вашего приложения, по которому необходимо вернуть Плательщика после аутентификации в СберБанк Онлайн|
|language|String|A..2|Нет|Язык локализации интерфейсов.  Пример: RU|
|callback|(PaymentResult) -> Unit|-|Да|Результат выполнения метода оплаты.<br>Структура [PaymentResult](https://sdkpay.github.io/SberPaySdkAndroidDoc/data_structures#payemntresult)|

### Вызов метода `payWithoutRefresh`

Метод `payWithoutRefresh` является входной точкой в SDK и должен быть вызван только по клику на кнопку «Оплатить»

> Обязательно корректно укажите значение параметра `appPackage`. В противном случае при возврате из СБОЛа после оплаты, если будет допущена ошибка в схеме, поднимется шторка с вариантами приложений для продолжения оплаты. Таким образом, сценарий разорвется

### Kotlin

```
SPaySdkApp.getInstance().payWithoutRefresh(
    context = context,
    apiKey = API_KEY,
    merchantLogin = CLIENT_NAME,
    bankInvoiceId = BANK_INVOICE_ID,
    orderNumber = ORDER_NUMBER,
    appPackage = APP_PACKAGE,
    language = LANGUAGE,
) { paymentResult ->
    when (paymentResult) {
        is PaymentResult.Success -> {
            //do something on success
	        doOnSuccess()	
        }
        is PaymentResult.Error -> {
            //do something on error
	        doOnError(paymentResult.merchantError)
        }
        is PaymentResult.Processing -> {
            //do something on processing
             doOnProcessing();
        }
         is PaymentResult.Cancel -> {
         //do something on processing
             doOnCancel();
        }
    }
}
```

### Java

```
SPaySdkApp.getInstance().payWithoutRefresh(
    context,
    API_KEY,
    CLIENT_NAME,
    BANK_INVOICE_ID,
    ORDER_NUMBER,
    APP_PACKAGE,
    LANGUAGE,
    paymentResult -> {
        if (paymentResult instanceof PaymentResult.Success) {
		    //do something on success
            doOnSuccess();
        } else if (paymentResult instanceof PaymentResult.Error) {
		    //do something on error
            doOnError(
               ((PaymentResult.Error) paymentResult).getMerchantErrors()
            );
        } else if (paymentResult instanceof PaymentResult.Processing) {
            //do something on processing
            doOnProcessing();
        }
        return null;
    }
);
```

## Оплата частями с комиссией

### Сценарий оплаты частями с комиссией

Для выбора сценария оплаты только частями с комиссией необходимо использовать метод SDK `payWithPartPay`. Ниже представлен список параметров метода

|Объект|Тип|Формат|Обязательный|Описание|
|---|:---:|:---:|:---:|---|
|context|Context|-|Да|Любой контекст Вашего приложения который может запускать новую Activity|
|apiKey|String|ANS..512|Да|Ключ Клиента для работы с сервисами платежного шлюза через SDK|
|merchantLogin|String|ANS..512|Да|Логин партнера для работы с сервисами платежного шлюза|
|bankInvoiceId|String|ANS..512|Да|Уникальный номер (идентификатор) заказа в Платежном шлюзе Банка.Необходимо передавать значение sbolBankInvoiceId (передается в externalParams) из ответа на Запрос регистрации заказа|
|orderNumber|String|ANS..36|Да|Уникальный номер (идентификатор) заказа в системе Клиента. Используется для упрощенного поиска заказа в системе банка при выявлении дефектов|
|appPackage|String|ANS..512|Да|Package приложения (он же BuildConfig.APPLICATION_ID) Вашего приложения, по которому необходимо вернуть Плательщика после аутентификации в СберБанк Онлайн|
|language|String|A..2|Нет|Язык локализации интерфейсов.  Пример: RU|
|callback|(PaymentResult) -> Unit|-|Да|Результат выполнения метода оплаты.<br>Структура [PaymentResult](https://sdkpay.github.io/SberPaySdkAndroidDoc/data_structures#payemntresult)|

### Вызов метода `payWithPartPay`

Метод `payWithPartPay` является входной точкой в SDK и должен быть вызван только по клику на кнопку «Оплатить»

> Обязательно корректно укажите значение параметра `appPackage`. В противном случае при возврате из СБОЛа после оплаты, если будет допущена ошибка в схеме, поднимется шторка с вариантами приложений для продолжения оплаты. Таким образом, сценарий разорвется

### Kotlin

```
SPaySdkApp.getInstance().payWithoutRefresh(
    context = context,
    apiKey = API_KEY,
    merchantLogin = CLIENT_NAME,
    bankInvoiceId = BANK_INVOICE_ID,
    orderNumber = ORDER_NUMBER,
    appPackage = APP_PACKAGE,
    language = LANGUAGE,
) { paymentResult ->
    when (paymentResult) {
        is PaymentResult.Success -> {
            //do something on success
	        doOnSuccess()	
        }
        is PaymentResult.Error -> {
            //do something on error
	        doOnError(paymentResult.merchantError)
        }
        is PaymentResult.Processing -> {
            //do something on processing
             doOnProcessing();
        }
         is PaymentResult.Cancel -> {
         //do something on processing
             doOnCancel();
        }
    }
}
```

### Java

```
SPaySdkApp.getInstance().payWithoutRefresh(
    context,
    API_KEY,
    CLIENT_NAME,
    BANK_INVOICE_ID,
    ORDER_NUMBER,
    APP_PACKAGE,
    LANGUAGE,
    paymentResult -> {
        if (paymentResult instanceof PaymentResult.Success) {
		    //do something on success
            doOnSuccess();
        } else if (paymentResult instanceof PaymentResult.Error) {
		    //do something on error
            doOnError(
               ((PaymentResult.Error) paymentResult).getMerchantErrors()
            );
        } else if (paymentResult instanceof PaymentResult.Processing) {
            //do something on processing
            doOnProcessing();
        }
        return null;
    }
);
```

## Оплата со списанием бонусов «Спасибо»

### Сценарий оплаты со списанием бонусов «Спасибо»

Для выбора сценария оплаты с бонусами «Спасибо» необходимо использовать метод SDK `payWithBonuses`. Ниже представлен список параметров метода

|Объект|Тип|Формат|Обязательный|Описание|
|---|:---:|:---:|:---:|---|
|context|Context|-|Да|Любой контекст Вашего приложения который может запускать новую Activity|
|apiKey|String|ANS..512|Да|Ключ Клиента для работы с сервисами платежного шлюза через SDK|
|merchantLogin|String|ANS..512|Да|Логин партнера для работы с сервисами платежного шлюза|
|bankInvoiceId|String|ANS..512|Да|Уникальный номер (идентификатор) заказа в Платежном шлюзе Банка.Необходимо передавать значение sbolBankInvoiceId (передается в externalParams) из ответа на Запрос регистрации заказа|
|orderNumber|String|ANS..36|Да|Уникальный номер (идентификатор) заказа в системе Клиента. Используется для упрощенного поиска заказа в системе банка при выявлении дефектов|
|appPackage|String|ANS..512|Да|Package приложения (он же BuildConfig.APPLICATION_ID) Вашего приложения, по которому необходимо вернуть Плательщика после аутентификации в СберБанк Онлайн|
|language|String|A..2|Нет|Язык локализации интерфейсов.  Пример: RU|
|callback|(PaymentResult) -> Unit|-|Да|Результат выполнения метода оплаты.<br>Структура [PaymentResult](https://sdkpay.github.io/SberPaySdkAndroidDoc/data_structures#payemntresult)|

### Вызов метода `payWithBonuses`

Метод `payWithBonuses` является входной точкой в SDK и должен быть вызван только по клику на кнопку «Оплатить»

> Обязательно корректно укажите значение параметра `appPackage`. В противном случае при возврате из СБОЛа после оплаты, если будет допущена ошибка в схеме, поднимется шторка с вариантами приложений для продолжения оплаты. Таким образом, сценарий разорвется

### Kotlin

```
SPaySdkApp.getInstance().payWithoutRefresh(
    context = context,
    apiKey = API_KEY,
    merchantLogin = CLIENT_NAME,
    bankInvoiceId = BANK_INVOICE_ID,
    orderNumber = ORDER_NUMBER,
    appPackage = APP_PACKAGE,
    language = LANGUAGE,
) { paymentResult ->
    when (paymentResult) {
        is PaymentResult.Success -> {
            //do something on success
	        doOnSuccess()	
        }
        is PaymentResult.Error -> {
            //do something on error
	        doOnError(paymentResult.merchantError)
        }
        is PaymentResult.Processing -> {
            //do something on processing
             doOnProcessing();
        }
         is PaymentResult.Cancel -> {
         //do something on processing
             doOnCancel();
        }
    }
}
```

### Java

```
SPaySdkApp.getInstance().payWithoutRefresh(
    context,
    API_KEY,
    CLIENT_NAME,
    BANK_INVOICE_ID,
    ORDER_NUMBER,
    APP_PACKAGE,
    LANGUAGE,
    paymentResult -> {
        if (paymentResult instanceof PaymentResult.Success) {
		    //do something on success
            doOnSuccess();
        } else if (paymentResult instanceof PaymentResult.Error) {
		    //do something on error
            doOnError(
               ((PaymentResult.Error) paymentResult).getMerchantErrors()
            );
        } else if (paymentResult instanceof PaymentResult.Processing) {
            //do something on processing
            doOnProcessing();
        }
        return null;
    }
);
```
