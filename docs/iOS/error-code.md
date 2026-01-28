# 에러 코드 레퍼런스

## KiTverifyError

KiTverify SDK 사용 중 발생할 수 있는 에러 코드 목록입니다.

### 초기화

| 에러 코드 | 설명 |
|-----------|------|
| `initializeFailed` | SDK 초기화에 실패했습니다. |
| `notInitialized` | SDK가 초기화되지 않았습니다. |

### 네트워크

| 에러 코드 | 설명 |
|-----------|------|
| `networkError` | 네트워크 오류가 발생했습니다. |
| `micPositionFetchFailed` | 마이크 위치 정보를 가져오는데 실패했습니다. |
| `tagVerificationFailed` | 태그 검증에 실패했습니다. |

### 권한

| 에러 코드 | 설명 |
|-----------|------|
| `microphonePermissionDenied` | 마이크 권한이 거부되었습니다. |

### 인식

| 에러 코드 | 설명 |
|-----------|------|
| `recognitionTimeout` | 인식 시간이 초과되었습니다. |
| `invalidTag` | 잘못된 태그입니다. |
| `notSupportedTag` | 지원하지 않는 태그입니다. |

### 상태

| 에러 코드 | 설명 |
|-----------|------|
| `alreadyRunning` | 이미 실행 중입니다. |
| `notRunning` | 실행 중이 아닙니다. |

## 에러 처리 예제

```swift
extension ViewController: KiTverifyDelegate {
    func kitVerifyDidFail(_ error: KiTverifyError) {
        switch error {
        case .initializeFailed, .notInitialized:
            print("초기화 오류: \(error.localizedDescription)")

        case .networkError, .micPositionFetchFailed, .tagVerificationFailed:
            print("네트워크 오류: \(error.localizedDescription)")

        case .microphonePermissionDenied:
            print("권한 오류: \(error.localizedDescription)")

        case .recognitionTimeout, .invalidTag, .notSupportedTag:
            print("인식 오류: \(error.localizedDescription)")

        case .alreadyRunning, .notRunning:
            print("상태 오류: \(error.localizedDescription)")
        }
    }
}
```
