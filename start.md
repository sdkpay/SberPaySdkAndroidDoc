# [SberPaySdkAndroidDoc](https://sdkpay.github.io/SberPaySdkAndroidDoc/)

##### [Начало работы](https://sdkpay.github.io/SberPaySdkAndroidDoc/start) | [Сценарии оплаты через SDK](https://sdkpay.github.io/SberPaySdkAndroidDoc/payment_script) | [Работа в режиме посочницы](https://sdkpay.github.io/SberPaySdkAndroidDoc/sandbox_mode) | [Вспомогательные структуры данных](https://sdkpay.github.io/SberPaySdkAndroidDoc/data_structures) | [Актуальная версия SDK](https://sdkpay.github.io/SberPaySdkAndroidDoc/version)

# Начало работы
> Версии SDK, которые работают с *API>=24* - поддерживается всеми версиями  
> Версии SDK, которые работают с *API<24* - до *2.0.x* включительно

## Подключение SDK
Подключите SDK одним из удобных Вам способов: [Maven](https://sdkpay.github.io/SberPaySdkAndroidDoc/start#maven) / [Aar](https://sdkpay.github.io/SberPaySdkAndroidDoc/start#aar)  

### Maven
Для получения зависимости из *maven* репозитория необходимо добавить его в **settings.gradle** файл вашего приложения
```
dependencyResolutionManagement {
    ...
    repositories {
        google()
        mavenCentral()
        ...
        maven {
            ⁣name = "GitHubPackages"
            url = uri("*URL из договора*")
            credentials {
                username = "*username для репозитория*"
                password = "*password для репозитория*"
            }
        }
    }
}
```
Далее нужно перейти в **build.gradle** вашего модуля и добавить зависимости внутрь блока `dependencies { ... }`
```
dependencies {
    ...
    implementation("*название зависимости из договора*:x.y.z")
    ...
}
```

### Aar
Пакет дистрибуции состоит из двух файлов: *SDK-VERSION.aar* и *bms-sdk-fingerprint_VERSION_release.aar*, которые необходимо разместить в вашем проекте (например в директории *libs* в корне проекта). Далее нужно перейти в **build.gradle** вашего модуля и добавить зависимости от *.aar-файлов* внутрь блока `dependencies { ... }`
> Также здесь необходимо явно добавить транзитивные зависимости библиотек.
```
// SPaySdk
implementation(files("../libs/SPaySDK-version.aar"))

// Bi.Zone
implementation(files("../libs/bms-sdk-fingerprint-version.aar"))

// Activity
implementation("androidx.activity:activity-ktx:1.6.1")

// Coroutines
implementation("androidx.lifecycle:lifecycle-runtime-ktx:2.6.1")
implementation("androidx.lifecycle:lifecycle-viewmodel-ktx:2.6.1")

// OkHttp
implementation("com.squareup.okhttp3:okhttp:4.10.0")
implementation("com.squareup.okhttp3:logging-interceptor:4.10.0")

// Retrofit
implementation("com.squareup.retrofit2:retrofit:2.9.0")
implementation("com.squareup.retrofit2:converter-gson:2.3.0")

// Jsoup
implementation("org.jsoup:jsoup:1.13.1")

// Dagger2
implementation("com.google.dagger:dagger:2.44")
annotationProcessor("com.google.dagger:dagger-compiler:2.44")

// Play services
implementation("com.google.android.gms:play-services-auth-api-phone:18.0.1")

// Timber
implementation("com.jakewharton.timber:timber:5.0.1")

// Three Ten BP
implementation("com.jakewharton.threetenabp:threetenabp:1.2.1")
testImplementation("org.threeten:threetenbp:1.2.1") {
    exclude("com.jakewharton.threetenabp:threetenabp:1.2.0")
}

// Coil
implementation("io.coil-kt:coil-base:2.4.0")
implementation("io.coil-kt:coil-svg:2.4.0")

// Shimmer
implementation("com.facebook.shimmer:shimmer:0.5.0")

// Firebase Database
implementation("com.google.firebase:firebase-database-ktx:20.2.0")

// Biometric
implementation("androidx.biometric:biometric:1.1.0")

// Encrypted Shared Preferences
implementation("androidx.security:security-crypto:1.1.0-alpha06")

// Lottie
implementation("com.airbnb.android:lottie:6.2.0")

// Serialization Json
implementation("org.jetbrains.kotlinx:kotlinx-serialization-json:1.4.1")
```
В приведенном выше примере указан путь до aar-файлов, находящихся в директории *libs* проекта. Если вы разместили их в другом месте, то нужно будет указать ваш путь к файлам. Подробнее смотрите в [документации для Android](https://developer.android.com/studio/projects/android-library#psd-add-aar-jar-dependency)

## Инициализация SDK
Для инициализации SDK необходимо вызвать метод `initialize` и передать в него [SPayInitSdkConfig](https://sdkpay.github.io/SberPaySdkAndroidDoc/data_structures#spayinitsdkconfig)

```
import spay.sdk.SPaySdkApp
import spay.sdk.api.SPayStage

class YourApplication : Application() {
    override fun onCreate() {
        super.onCreate()

    val config = SPayInitSdkConfig(
        application = requireActivity().application,
        enableBnpl = true,
        stage = stage,
        resultViewNeeded = true,
        enableLogging = true,
        helperConfig = SPayHelperConfig(
            isHelperEnabled = true,
            disabledHelpers = listOfHelpers
        ),
        initializationResult = { initializationResult -> ... }
    )

    SPaySdkApp.getInstance().initialize(config)

    }
 }
```

> Если вы подключили сервис *Плати частями* и пользователь выбрал этот способ для оплаты заказа, то выполняя через back расширенный запрос состояния заказа *getOderStatusExtended.do*, в ответе вы получите значение параметра `paymentWay` равное *BNPL*. Подключить параметр `paymentWay` в callback-уведомлениях возможно в личном кабинете партнера интернет-эквайринга. Это можно сделать в *настройках -> основные настройки -> callback-уведомления*. При заполнении доп. параметров выйдет список всех возможных. Для того, чтобы передавался способ оплаты заказа необходимо выбрать *paymentWay*.  
>Оплаченные частями заказы в личном кабинете партнера интернет-эквайринга будут отмечаться признаком *BNPL* в поле «Платежное средство».  
>При этом денежные средства по заказам, оплаченным частями, поступят от ООО «ЦНФС» («Центр новых финансовых сервисов»), предоставляющей сервис

### Помогашки

Helpers или помогашки - функционал, позволяющий клиенту с недостаточным количеством средств быстро пополнить счет или выпустить новые продукты для оплаты.  
Структура [SPayHelperConfig](https://sdkpay.github.io/SberPaySdkAndroidDoc/data_structures#spayhelperconfig)

> При вызове данного метода подгружается конфиг, который содержит строковые ресурсы и картинки. Если метод был вызван не при старте приложения, то есть шанс, что SDK не успеет получить конфиг, в результате чего кнопка SDK может быть отрисована некорректно или не отрисована вовсе

## Добавление кнопки

Для вызова метода оплаты можно использовать готовый класс кнопки `SPayButton` или отрисовать кнопку самостоятельно в соответсвии с [гайдланами](https://cdn-app.sberdevices.ru/misc/0.0.0/assets/bsm-docs/button-guideline.pdf)

Инициализация кнопки оплаты: [.xml](https://sdkpay.github.io/SberPaySdkAndroidDoc/start#xml) / [Composable](https://sdkpay.github.io/SberPaySdkAndroidDoc/start#composable)

### xml
```
<spay.sdk.view.SPayButton
    android:id="@+id/s_pay_button"
    android:layout_width="match_parent"
    android:layout_height="wrap_content">
```

### Composable

```
@Composable
private fun SPButton() {
    val context = LocalContext.current
    val customView = SberPayButton(
        context = context,
        attributeSet = null
    )

    AndroidView(
        modifier = Modifier.fillMaxSize(),
        factory = { customView }
    )
}
```

## Проверка готовности к оплате

Для проверки готовности сервисов SberPay к оплате, и проверки наличия установленного мобильного приложения банка на устройстве необходимо вызывать метод `isReadyForSPaySdk`

> Кнопка должна быть отрисована только в том случае, если метод `isReadyForSPaySdk` вернет true

## Запрос разрешений

Для работы SDK также запрашивает определенные разрешения, которые не являются обязательными но повышают шанс успешной оплаты

```
<uses-permission android:name="android.permission.ACCESS_COARSE_LOCATION" />
<uses-permission android:name="android.permission.ACCESS_COARSE_UPDATES" />
<uses-permission android:name="android.permission.ACCESS_FINE_LOCATION" />
```

Вызов диалогового окна на запрос разрешений от пользователя реализован при обращении к методу запуска сценария оплаты, при условии, что как минимум одно разрешение не было выдано пользователем

> Независимо от результата запроса разрешений от пользователя сценарий оплаты будет продолжен