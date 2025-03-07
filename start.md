# SberPaySdkAndroidDoc

# Начало работы
> Версии SDK, которые работают с *API>=24* - поддерживается всеми версиями  
> Версии SDK, которые работают с *API<24* - до *2.0.x* включительно

## Подключение SDK
Подключите SDK одним из удобных Вам способов: Maven(https://sdkpay.github.io/SberPaySdkAndroidDoc/start#maven) / Aar(https://sdkpay.github.io/SberPaySdkAndroidDoc/start#aar)  

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
В приведенном выше примере указан путь до aar-файлов, находящихся в директории *libs* проекта. Если вы разместили их в другом месте, то нужно будет указать ваш путь к файлам. Подробнее смотрите в [Документации для Android](https://developer.android.com/studio/projects/android-library#psd-add-aar-jar-dependency)

## Инициализация SDK
Для инициализации SDK необходимо вызвать метод `initialize` и передать в него [SPayInitSdkConfig]()
