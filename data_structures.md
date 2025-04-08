# [SberPaySdkAndroidDoc](https://sdkpay.github.io/SberPaySdkAndroidDoc/)

#### [Бординг](https://sdkpay.github.io/SberPaySdkAndroidDoc/boarding) | [Регистрация заказов в платежном шлюзе Сбера](https://sdkpay.github.io/SberPaySdkAndroidDoc/order_registration) | [Начало работы](https://sdkpay.github.io/SberPaySdkAndroidDoc/start) | [Сценарии оплаты через SDK](https://sdkpay.github.io/SberPaySdkAndroidDoc/payment_script) | [Работа в режиме посочницы](https://sdkpay.github.io/SberPaySdkAndroidDoc/sandbox_mode) | [Вспомогательные структуры данных](https://sdkpay.github.io/SberPaySdkAndroidDoc/data_structures) | [Актуальная версия SDK](https://sdkpay.github.io/SberPaySdkAndroidDoc/version) | [Поддержка](https://sdkpay.github.io/SberPaySdkAndroidDoc/support) | [FAQ](https://sdkpay.github.io/SberPaySdkAndroidDoc/faq)

<br>

# Вспомогательные структуры данных

## SPaySdkInitConfig

#### Конфиг для инициализации SDK

|Параметр|Тип|Дефолтное значение|Обязательный|Описание|
|---|:---:|:---:|:---:|---|
|application|Application|-|Да|Инстанс App класса|
|enableBnpl|Boolean|false|Нет|Параметр отвечает за функционал оплаты частями. Значение true не гарантирует, что оплата частями будет доступна, но значение false гарантирует, что оплата частями будет не доступна|
|stage|SPayStage|-|Да|Стенд с которым будет взаимодействовать SDK. Структура [SPayStage](https://sdkpay.github.io/SberPaySdkAndroidDoc/data_structures#spaystage)|
|helperConfig|SPayHelperConfig|-|Да|Функционал "Помогашек". Структура [SPayHelperConfig](https://sdkpay.github.io/SberPaySdkAndroidDoc/data_structures#spayhelperconfig)|
|spasiboBonuses|Boolean|true|Нет|Флаг активации функционала бонусов "Спасибо"|
|resultViewNeeded|Boolean|true|Нет|Отображение экранов статусов завершения работы SDK|
|enableLogging|Boolean|false|Нет|Отображение логов SDK в консоль|
|enableOutsideTouchCancelling|Boolean|true|Нет|Включить завершение работы SDK по тапу вне шторки|
|initializationResult|Callback<InitializationResult>|-|Да|Блок результата инициализации SDK|

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
