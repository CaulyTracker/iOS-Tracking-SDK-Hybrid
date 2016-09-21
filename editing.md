# Cauly 리타겟팅 iOS Hybrid 연동 가이드

### 개요
하이브리드 앱은 앱 안에서 Webview 를 이용하여 컨텐츠를 보여주는 형식의 앱입니다. 
Naver 앱, Daum 앱 등이 이 경우에 해당하며, 이 경우 Cauly Tracker Native SDK 의 일부 기능을 사용하여 앱 안의 Webview 에 Cauly 의 Tracking Web SDK 와 통신할 수 있는 기능을 추가해야 합니다. 
본 문서는 광고주의 앱이 하이브리드 앱일 경우 Cauly 와 리타겟팅 연동을 하는 방법에 대해서 설명하는 문서입니다.

### 문서 버전
| 문서 버전 | 작성 날짜 | 작성자 및 내용 | 
| ---------- | ----------- | ---------------- |
| 1.0.0 | 2016.4.28 | 권순국(nezy at fsn.co.kr) - 초안작성 |

### 목차
- [연동 절차](#연동-절차)
- [연동 상세](#연동-상세)

### 연동 절차
1. Cauly 담당자 혹은 cauly@fsn.co.kr로 연락하여 Cauly 리타겟팅 연동에 대해서 협의합니다.
1. 협의 완료 후, track_code 를 발급받습니다.
1. track_code 를 포함한 tracker 코드를 아래 ‘연동상세’을 참고하여 Android 앱의 필요한 곳에 삽입합니다.
1. 코드 삽입 이후 연동이 되었는지 Cauly 에게 테스트를 요청합니다.
1. Cauly에서 테스트를 완료하면 APP을 마켓에 업데이트하고 업데이트 내용을 Cauly와 공유합니다.
1. 마켓 업데이트 완료 후 7일 뒤 (주말 및 공휴일 포함) 광고 라이브 가능합니다. (단, 모수가 너무 적을 경우 모수 수집을 위해 추가 시간이 소요될 수 있습니다.)

### 연동 상세
아래의 순서대로 연동을 시작합니다.

#### Native APP SDK 기본 연동
- Setting
 - info.plist: https://github.com/CaulyTracker/iOS-Tracking-SDK#infoplist
 - Library Import
   - header, .so 추가: https://github.com/CaulyTracker/iOS-Tracking-SDK#static-library-import
    - Framework 추가: https://github.com/CaulyTracker/iOS-Tracking-SDK#depedency
- 초기화: https://github.com/CaulyTracker/iOS-Tracking-SDK#cauly-tracker-초기화
- Session: https://github.com/CaulyTracker/iOS-Tracking-SDK#session-start--close
 
#### Webview 연동
WebView 의 Load가 끝나는 시점에 JavaScript Interface 를 등록합니다.
```objc
#pragma mark - UIWebViewDelegate
...
- (BOOL)webView:(UIWebView *)webView shouldStartLoadWithRequest:(NSURLRequest *)request navigationType:(UIWebViewNavigationType)navigationType {
    ...
    [CaulyTracker callJSInterface:webView request:request];
    ...
    return YES;
}

- (void)webViewDidFinishLoad:(UIWebView *)webView {
    ...
    [CaulyTracker setJSInterface:webView];
    ...
}
...
```

#### Mobile Web 연동
아래 3가지 캠페인 중 집행 예정인 캠페인에 맞게 Mobile WEB에 tracker 스크립트를 삽입합니다.
- [A. Feed 캠페인](https://github.com/CaulyTracker/Retargeting#a-feed-%EC%BA%A0%ED%8E%98%EC%9D%B8)
- [B. Static 캠페인](https://github.com/CaulyTracker/Retargeting#b-static-%EC%BA%A0%ED%8E%98%EC%9D%B8)
- [C. Install 캠페인](https://github.com/CaulyTracker/Retargeting#c-install-%EC%BA%A0%ED%8E%98%EC%9D%B8)
