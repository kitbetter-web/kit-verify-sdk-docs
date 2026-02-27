# KiTverify SDK for iOS

## 시작하기
이 문서는 KiTverify SDK를 사용하기 위한 기본적인 방법을 안내합니다.

### 요구사항
- iOS 14.0 이상

## 설치
KiTverify SDK를 SPM(Swift Package Manager)을 통해 설치할 수 있습니다.

### Swift Package Manager 설정
Xcode에서 프로젝트를 열고 File > Swift Packages > Add Package Dependency… 메뉴를 선택합니다.

아래 주소를 입력 후 `Add Package` 버튼을 눌러서 프로젝트에 추가합니다.
```text
https://github.com/kitbetter-web/muzlive-kit-verify-sdk-ios
```

## 프로젝트 설정

### 앱 프라이버시 설정

KiTverify SDK를 사용하기 위해서는 마이크 권한이 필요합니다.

`Info.plist` 파일에 아래 항목을 추가하세요:

```xml title="마이크 권한"
<key>NSMicrophoneUsageDescription</key>
<string>키트 인식을 위해 마이크 권한이 필요합니다.</string>
```

## SDK 사용하기

### 1. 초기화

KiTverify SDK를 사용하기 위해서는 앱이 시작될 때 SDK를 초기화해야 합니다.
이 작업은 `AppDelegate`의 `application(_:didFinishLaunchingWithOptions:)` 메서드에서 수행됩니다.

```swift
import UIKit
import KiTverifySDK

@main
class AppDelegate: UIResponder, UIApplicationDelegate {
    func application(
        _ application: UIApplication,
        didFinishLaunchingWithOptions launchOptions: [UIApplication.LaunchOptionsKey: Any]?
    ) -> Bool {
        let clientId = "YOUR_CLIENT_ID"
        let secretKey = "YOUR_SECRET_KEY"
        KiTverify.initialize(with: clientId, secretKey: secretKey)
        return true
    }
}
```

> 🚨 주의
> `SECRET_KEY`는 앱의 보안을 위해 외부에 노출되지 않도록 주의해야 합니다. 따라서, `SECRET_KEY`는 소스코드에 직접 입력하지 않고, 별도의 파일에 저장하여 사용하는 것이 좋습니다.

### 2. 델리게이트 등록

SDK 이벤트를 수신하기 위해 `KiTverifyDelegate`를 구현합니다.

```swift
import KiTverifySDK

class ViewController: UIViewController {
    override func viewDidLoad() {
        super.viewDidLoad()
        KiTverify.shared.addDelegate(self)
    }
}

extension ViewController: KiTverifyDelegate {
    func kitVerifyDidSuccess(_ result: KiTverifyResult) {
        print("인식 성공 - token: \(result.token), albumID: \(result.albumID)")
    }

    func kitVerifyDidFail(_ error: KiTverifyError) {
        print("인식 실패 - \(error.localizedDescription)")
    }

    func kitVerifyDidDismiss() {
        print("SDK 화면 종료")
    }
}
```

| 메서드 | 설명 |
|--------|------|
| `kitVerifyDidSuccess(_:)` | 태그 인식 성공 시 호출. `KiTverifyResult`에 `token`과 `albumID`가 포함됩니다. |
| `kitVerifyDidFail(_:)` | 에러 발생 시 호출. [에러 코드 레퍼런스](error-code.md)를 참고하세요. |
| `kitVerifyDidDismiss()` | SDK UI가 종료될 때 호출. 기본 구현이 제공되므로 선택적으로 구현할 수 있습니다. |

### 3. 태그 인식 시작 / 종료

`start()` 메서드를 호출하여 태그 인식을 시작하고, `stop()` 메서드로 종료할 수 있습니다.

```swift
import KiTverifySDK

class ViewController: UIViewController {
    @IBAction func startButtonTapped(_ sender: UIButton) {
        KiTverify.shared.start()
    }

    @IBAction func stopButtonTapped(_ sender: UIButton) {
        KiTverify.shared.stop()
    }
}
```

### 4. 상태 확인

SDK의 현재 상태를 확인할 수 있습니다.

```swift
// SDK 초기화 여부 확인
if KiTverify.shared.isInitialized {
    print("SDK 초기화 완료")
}

// SDK 실행 여부 확인
if KiTverify.shared.isRunning {
    print("SDK 실행 중")
}
```
