# [SberPaySdkAndroidDoc](https://sdkpay.github.io/SberPaySdkAndroidDoc/)

#### [Бординг](https://sdkpay.github.io/SberPaySdkAndroidDoc/boarding) | [Регистрация заказов в платежном шлюзе Сбера](https://sdkpay.github.io/SberPaySdkAndroidDoc/order_registration) | [Начало работы](https://sdkpay.github.io/SberPaySdkAndroidDoc/start) | [Сценарии оплаты через SDK](https://sdkpay.github.io/SberPaySdkAndroidDoc/payment_script) | [Работа в режиме посочницы](https://sdkpay.github.io/SberPaySdkAndroidDoc/sandbox_mode) | [Вспомогательные структуры данных](https://sdkpay.github.io/SberPaySdkAndroidDoc/data_structures) | [Актуальная версия SDK](https://sdkpay.github.io/SberPaySdkAndroidDoc/version) | [Поддержка](https://sdkpay.github.io/SberPaySdkAndroidDoc/support) | [FAQ](https://sdkpay.github.io/SberPaySdkAndroidDoc/faq)

<br>

# Вспомогательные структуры данных

## SPaySdkInitConfig

#### Конфиг для инициализации SDK

|Параметр|Тип|Дефолтное значение|Обязательный|Описание|
|---|:---:|:---:|:---:|---|
|enableBnpl|Boolean|false|Нет|Параметр отвечает за функционал оплаты частями. Значение true не гарантирует, что оплата частями будет доступна, но значение false гарантирует, что оплата частями будет не доступна|
|stage|SPayStage|-|Да|Стенд с которым будет взаимодействовать SDK.<br>Структура [SPayStage](https://sdkpay.github.io/SberPaySdkAndroidDoc/data_structures#spaystage)|
|helperConfig|SPayHelperConfig|-|Да|Функционал "Помогашек".<br>Структура [SPayHelperConfig](https://sdkpay.github.io/SberPaySdkAndroidDoc/data_structures#spayhelperconfig)|
|spasiboBonuses|Boolean|true|Нет|Флаг активации функционала бонусов "Спасибо"|
|resultViewNeeded|Boolean|true|Нет|Отображение экранов статусов завершения работы SDK|
|enableLogging|Boolean|false|Нет|Отображение логов SDK в консоль|
|enableOutsideTouchCancelling|Boolean|true|Нет|Включить завершение работы SDK по тапу вне шторки|
|initializationResult|Callback<InitializationResult>|-|Да|Блок результата инициализации SDK|

<br>

## SPayMethod

#### Доступные способы оплаты в SDK

|Параметр|Описание|
|---|---|
|Default|Оплата с использованием 6-ти частей и обычной оплатой (без paymentToken и paymentOrder)|
|WithBankInvoiceId|Оплата с параметрами для проведения оплаты (с paymentToken и paymentOrder)|
|WithBonuses|Оплата со списанием бонусов «Спасибо»|
|WithoutRefresh|Оплата без рефреша с использованием 6-ти частей и обычной оплатой (без paymentToken и paymentOrder)|
|WithPaymentAccount|Оплата с использованием платежного счета|
|WithPartPay|Оплата с использованием платежа на 6 частей|
|WithBinding|Оплата по связке<br>Требует обязательный параметр `bindingId` — Уникальный идентификатор связки|
|WithPhoneNumber|Оплата с авторизацией только по номеру телефона|

<br>

## SPaymentRequest

#### Параметры для проведения сценария оплаты

|Объект|Тип|Формат|Обязательный|Описание|
|---|:---:|:---:|:---:|---|
|context|Context|-|Да|Любой контекст Вашего приложения который может запускать новую Activity|
|apiKey|String|ANS..512|Да|Ключ для работы с сервисами платежного шлюза через SDK|
|bankInvoiceId|String|ANS..512|Да|Уникальный номер (идентификатор) заказа в Платежном шлюзе Банка.Необходимо передавать значение sbolBankInvoiceId (передается в externalParams) из ответа на Запрос регистрации заказа|
|orderNumber|String|ANS..36|Да|Уникальный номер (идентификатор) заказа в Вашей системе. Используется для упрощенного поиска заказа в системе банка при выявлении дефектов|
|merchantLogin|String|ANS..512|Да|Логин для работы с сервисами платежного шлюза|
|appPackage|String|ANS..512|Да|Package приложения (он же BuildConfig.APPLICATION_ID) Вашего приложения, по которому необходимо вернуть Плательщика после аутентификации в СберБанк Онлайн|
|phoneNumber|String?|71231231212|Нет|Мобильный телефон Клиента для сценария авторизации по номеру телефона|
|callback|(PaymentResult) -> Unit|-|Да|Результат выполнения метода оплаты.<br>Структура [PaymentResult](https://sdkpay.github.io/SberPaySdkAndroidDoc/data_structures#paymentresult)|

<br>

## SPayStage

#### Стенды для работы с SDK

|Параметр|Дефолтное значение|Описание|
|---|:---:|---|
|Prod|Да|Стандартное значение, все сервисы в SDK работают в продуктовом режиме|
|SandboxRealBankApp|Нет|Режим песочницы, для авторизации пользователя происходит редирект в приложение Сбербанка. Позволяет протестировать оплату в максимально близких к продуктовым условиях|
|SandboxWithoutBankApp|Нет|Режим песочницы, при авторизации пользователя не осуществляется перехода в приложение Сбербанка. Позволяет производить тестирование на устройствах без Сбербанк-онлайн|

<br>

## SPayHelperConfig

#### Помогашки

|Параметр|Тип|Дефолтное значение|Обязательный|Описание|
|---|:---:|:---:|:---:|---|
|sbp|Boolean|true|Нет|Разрешить пополнение карты через СБП|
|creditCard|Boolean|true|Нет|Разрешить выпуск кредитной карты|
|debitCard|Boolean|true|Нет|Разрешить выпуск дебетовой карты|

<br>

## MerchantError

#### Описание ошибок SDK

```
package spay.sdk.api

import android.os.Parcelable
import kotlinx.parcelize.Parcelize

/**
 * Класс с возможными ошибками SDK
 */
@Parcelize
sealed class MerchantError(
    open val description: String? = null
) : Parcelable {

    /**
     * Ошибка, если не переданы необходимые данные
     */
    data class RequiredDataNotSent(override val description: String) : MerchantError(description)

    /**
     * Ошибка при взаимодействии с SPayApi
     */
    data class SPayApiError(override val description: String) : MerchantError(description)

    /**
     * Ошибка, возникающая если время ожидания ответа от сервера привысило заданный лимит
     */
    data class TimeoutException(override val description: String) : MerchantError(description)

    /**
     * Ошибка при отсутствии подключния к интернету
     */
    data class NoInternetConnection(override val description: String = "Отсутствует подключение к интернету") : MerchantError(description)

    /**
     * Ошибка при оплате бонусами Спасибо
     */
    data class PayWithBonusesError(override val description: String) : MerchantError(description)

    /**
     * Ошибка при оплате по связке
     */
    data class PayWithBindingError(override val description: String) : MerchantError(description)

    /**
     * Непредвиденная ошибка при работе SPaySDK
     */
    data class UnexpectedError(override val description: String) : MerchantError(description)

    /**
     * Ошибка при проверке готовности к проведению оплаты
     */
    data class IsReadyCheckHasNotBeenCalled(override val description: String = "Перед проведением оплаты необходимо вызвать метод isReadyForSPaySdk()") : MerchantError(description)

    /**
     * Ошибка внутренних компонентов SPaySdk
     */
    data class InnerSdkComponentsError(override val description: String = "Ошибка внутренних компонентов SPaySdk") : MerchantError(description)
}
```

<br>

## InitializationResult

#### Результат выполнения метода `initialize`

```
/** Объект результата инициализации SPaySdk */
sealed class InitializationResult {

    /** SPaySdk инициализирована успешно */
    object Success : InitializationResult()

    /**
     * Произошел сбой при получении или обработке конфига
     *
     * @param message сообщение об ошибке
     */
    data class ConfigError(val message: String) : InitializationResult()
}
```

<br>

## PaymentResult

#### Результат выполнения любого из методов `pay`

```
package spay.sdk.api

import android.os.Parcelable
import kotlinx.parcelize.Parcelize

/**
 * Класс обёртка для предоставления результата оплаты
 * */
@Parcelize
sealed class PaymentResult : Parcelable {

    /**
     * @object [Success] возвращается в случае, если оплата была произведена успешно
     *
     * @param localSessionId [String] localSessionId в рамках операции
     */
    data class Success(val localSessionId: String) : PaymentResult()

    /**
     * @class [Error] возвращается в случае, если во время выполнения операции оплаты произошла ошибка
     *
     * @param localSessionId [String] localSessionId в рамках операции
     * @param merchantError - объект класса, описывающий ошибки для предоставления мерчанту
     * @see MerchantError
     */
    data class Error(val localSessionId: String, val merchantError: MerchantError? = null) : PaymentResult()

    /**
     * @object [Processing] возвращается в случае, если оплата находится в процессе выполнения
     *
     * @param localSessionId [String] localSessionId в рамках операции
     */
    data class Processing(val localSessionId: String) : PaymentResult()

    /**
     * @object [Cancel] возвращается в случае, если пользователь прервал процесс оплаты
     *
     * @param localSessionId [String] localSessionId в рамках операции
     */
    data class Cancel(val localSessionId: String) : PaymentResult()
}
```

<br>

## PaymentTokenResult

#### Результат выполнения метода `getPaymentToken`

```
package spay.sdk.api

/**
 * Класс обёртка для результата запроса на получение PaymentToken
 * */
sealed class PaymentTokenResult {

    /**
     * @class [Success] возвращается в случае, если токен был успешно получен
     *
     * @param paymentToken - платежный токен
     */
    data class Success(val paymentToken: String) : PaymentTokenResult()

    /**
     * @class [Error] возвращается в случае, если при получении токена произошла ошибка
     *
     * @param merchantError [MerchantError] - ошибка
     */
    data class Error(val merchantError: MerchantError) : PaymentTokenResult()
}
```

<br>

## SdkReadyCheckResult

#### Состояние SDK, возвращаемое на вызов метода `isReadyForSPaySdk`

```
/**
 * Структура состояния готовности SDK к работе. Результат вызова метода isReadyForSPaySdk
 */
sealed class SdkReadyCheckResult {
    /**
     * Состояние SDK готово к использованию
     */
    object Ready : SdkReadyCheckResult()

    /**
     * Состояние SDK не готово к использованию
     *
     * @param cause [String] Причина не готовности SDK
     */
    data class NotReady(val cause: String) : SdkReadyCheckResult()
}
```
