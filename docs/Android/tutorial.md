# KiTverify SDK for Android

## 시작하기
이 문서는 KiTverify SDK를 Android 프로젝트에 통합하고 사용하는 방법을 안내합니다.

### 요구사항
- Android 7.0 (API Level 24) 이상
- Kotlin 1.9.22 이상

## 설치

### Gradle 설정
`settings.gradle.kts` 또는 Project level `build.gradle`에 `mavenCentral()`이 포함되어 있는지 확인합니다.

```kotlin title="settings.gradle.kts"
dependencyResolutionManagement {
    repositories {
        google()
        mavenCentral()
    }
}
```

App level `build.gradle.kts`에 의존성을 추가합니다.

```kotlin title="build.gradle.kts"
dependencies {
    implementation("io.github.kitbetter-web:kit-verify-sdk-android:1.0.0") // 최신 버전을 확인하세요
}
```

## SDK 사용하기

### 1. 초기화 (Application Class 권장)
`Application` 클래스에서 `initialize()`를 호출하여 SDK를 미리 초기화할 수 있습니다.

```kotlin
class SampleApplication : Application() {
    override fun onCreate() {
        super.onCreate()

        val sdkKey = "your-sdk-key"
        val secretKey = "your-secret-key"
        val packageName = "your.package.name"

        KitVerifier.initialize(
            sdkKey,
            secretKey,
            packageName,
            this
        ) { isSuccess ->
            if (isSuccess) {
                // 초기화 성공
            } else {
                // 초기화 실패
            }
        }
    }
}
```

### 2. 실행
`KiTVerifyContract`를 사용하여 결과를 수신할 런처를 등록하고, `KitVerifier.start()`로 인증을 시작합니다.

```kotlin
class SampleActivity : AppCompatActivity() {

    // 결과 수신을 위한 ActivityResultLauncher 등록
    private val resultLauncher = registerForActivityResult(KiTVerifyContract()) { result ->
        if (result != null) {
            // 인증 성공: result.albumId, result.token 등 사용 가능
            Log.d("KiTVerify", "Success: ${result.albumId}")
        } else {
            // 사용자가 취소했거나 뒤로가기로 종료한 경우
            Log.d("KiTVerify", "Cancelled")
        }
    }

    override fun onCreate(savedInstanceState: Bundle?) {
        super.onCreate(savedInstanceState)
        
        // 버튼 클릭 시 인증 시작
        binding.verifyButton.setOnClickListener {
            KitVerifier.start(this, resultLauncher)
        }
    }
}
```

### 3. 에러 처리
인증 과정에서 발생할 수 있는 에러를 처리하기 위해 리스너를 등록할 수 있습니다.

```kotlin
KitVerifier.setOnErrorListener { kitError ->
    Log.e("KiTVerify", "Error: ${kitError.errorCode}, Cause: ${kitError.cause}")
}
```

### 4. 결과 수신 (Flow 사용)
`KiTVerifyContract`를 사용하는 방법 외에도, `KitVerifier.verifyResult`를 통해 `SharedFlow`로 검증 결과를 구독할 수 있습니다.

```kotlin
lifecycleScope.launch {
    KitVerifier.verifyResult.collect { result ->
        // 인증 성공: result.albumId, result.token 등 사용 가능
        Log.d("KiTVerify", "Success via Flow: ${result.albumId}")
    }
}
```
