# [SberPaySdkAndroidDoc](https://sdkpay.github.io/SberPaySdkAndroidDoc/)

#### [Бординг](https://sdkpay.github.io/SberPaySdkAndroidDoc/boarding) | [Регистрация заказов в платежном шлюзе Сбера](https://sdkpay.github.io/SberPaySdkAndroidDoc/order_registration) | [Начало работы](https://sdkpay.github.io/SberPaySdkAndroidDoc/start) | [Сценарии оплаты через SDK](https://sdkpay.github.io/SberPaySdkAndroidDoc/payment_script) | [Работа в режиме посочницы](https://sdkpay.github.io/SberPaySdkAndroidDoc/sandbox_mode) | [Вспомогательные структуры данных](https://sdkpay.github.io/SberPaySdkAndroidDoc/data_structures) | [Актуальная версия SDK](https://sdkpay.github.io/SberPaySdkAndroidDoc/version) | [Поддержка](https://sdkpay.github.io/SberPaySdkAndroidDoc/support) | [FAQ](https://sdkpay.github.io/SberPaySdkAndroidDoc/faq)

<br>

# Сценарии оплаты

> Возможные сценарии оплаты описаны в структуре [SPayMethod](https://sdkpay.github.io/SberPaySdkAndroidDoc/data_structures#spaymethod)

Вне зависимости от конкретного сценария оплаты, Вам необходимо будет использовать один и тот же метод запуска SDK

<br>

## Способы авторизации

В SDK на данный момент существует 4 способа авторизации для проведения оплаты:
- По рефреш токену
- Бесшовная (используя авторизационный токен)
- Через мобильное приложение Банка
- По номеру телефона

Очередность попыток авторизации будет такой же, как описано выше.
Т.е. сначала SDK будет пробовать авторизоваться по рефреш токену, затем бесшовным способом и так далее, пока не пройдет успешно, либо все способы не закончатся. Естественно, условие выше актуально в случае, когда все способы доступны для конкретного пользователя и способа оплаты

> Для Пользователя и Партнера визуальное отличие будет только между авторизацией через мобильное приложение Банка (произойдет переход в МП Банка с авторизацией и редирект обратно в приложение Партнера) и другими способами (все происходит в рамках открытой шторки SDK)
>
> В большинстве случаев первая авторизация для Пользователя будет через мобильное приложение Банка

<br>

## Запуск сценария оплаты

Метод `pay` является входной точкой в SDK и должен быть вызван только по клику на кнопку «Оплатить»

 > Обязательно корректно укажите значение параметра `appPackage`.<br>В противном случае при возврате из МП Банка после авторизации поднимется шторка с вариантами приложений для продолжения оплаты. Таким образом сценарий разорвется

### Kotlin

```
SPaySdkApp.getInstance().pay(
    method = SPayMethod.Default,
    request = SPaymentRequest(
        context = requireContext(),
        apiKey = API_KEY,
        merchantLogin = MERCHANT_LOGIN,
        bankInvoiceId = BANK_INVOICE_ID,
        orderNumber = ORDER_NUMBER,
        appPackage = APP_PACKAGE,
        phoneNumber = PHONE_NUMBER
    ) { paymentResult -> }
)
```

### Java

```
SPaySdkApp.getInstance().pay(
    SPayMethod.Default,
    SPaymentRequest(
        context,
        API_KEY,
        MERCHANT_LOGIN,
        BANK_INVOICE_ID,
        ORDER_NUMBER,
        APP_PACKAGE,
        PHONE_NUMBER,
        paymentResult -> {
            if (paymentResult instanceof PaymentResult.Success) {
		        //do something on success
            } else if (paymentResult instanceof PaymentResult.Error) {
		        //do something on error
            } else if (paymentResult instanceof PaymentResult.Processing) {
            //do something on processing
            }
            return null;
        }
    )
);
```
