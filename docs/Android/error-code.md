# 에러 코드 레퍼런스

## KitErrorEvent

KiTverify SDK 사용 중 `KitVerifier.errorEvent` Flow를 통해 수신할 수 있는 에러 이벤트 목록입니다.

### 초기화

| 클래스 | 설명 |
|--------|------|
| `InitializeFailed` | SDK 초기화에 실패했습니다. |
| `NotInitialized` | SDK가 초기화되지 않았습니다. |

### 네트워크

| 클래스 | 설명 |
|--------|------|
| `NetworkError` | 네트워크 오류가 발생했습니다. (`originalError`로 원인 확인 가능) |
| `MicPositionFetchFailed` | 마이크 위치 정보를 가져오는데 실패했습니다. |
| `TagVerificationFailed` | 태그 검증에 실패했습니다. |

### 권한

| 클래스 | 설명 |
|--------|------|
| `MicrophonePermissionDenied` | 마이크 권한이 거부되었습니다. |

### 인식

| 클래스 | 설명 |
|--------|------|
| `RecognitionTimeout` | 인식 시간이 초과되었습니다. |
| `InvalidTag` | 잘못된 태그입니다. |
| `NotSupportedTag` | 지원하지 않는 태그입니다. |

### 상태

| 클래스 | 설명 |
|--------|------|
| `AlreadyRunning` | 이미 실행 중입니다. |
| `NotRunning` | 실행 중이 아닙니다. |

## 에러 처리 예제

`KitVerifier.errorEvent`를 구독하여 각 에러 상황에 맞게 처리할 수 있습니다.

```kotlin
lifecycleScope.launch {
    KitVerifier.errorEvent.collect { event ->
        when (event) {
            is KitErrorEvent.InitializeFailed,
            is KitErrorEvent.NotInitialized -> {
                Log.e("KitVerifier", "초기화 오류")
            }
            is KitErrorEvent.NetworkError -> {
                Log.e("KitVerifier", "네트워크 오류: ${event.originalError?.message}")
            }
            is KitErrorEvent.MicrophonePermissionDenied -> {
                Log.e("KitVerifier", "권한 오류")
            }
            is KitErrorEvent.RecognitionTimeout -> {
                Log.e("KitVerifier", "인식 시간 초과")
            }
            // 그 외 에러 처리...
            else -> {
                Log.e("KitVerifier", "기타 오류: $event")
            }
        }
    }
}
```
