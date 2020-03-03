---
title: "Flutter iOS 13.3.1 빌드 에러"
date: 2020-03-03T20:30:00+09:00
lastmod: 2020-03-03T20:30:00+09:00
draft: false
show_in_homepage: true
show_description: false
license: 'Tyler Grey'

tags: ['Flutter']
categories: ['Flutter']

featured_image: '/images/flutter_logo.png'
featured_image_preview: ''

comment: true
toc: true
autoCollapseToc: true
math: true
---
최근 Flutter로 개인 프로젝트를 진행하다 iphone 디바이스에 앱이 빌드가 안돼 한참 애먹었습니다.  
그러다 4일 전 Flutter 컨트리뷰터인 [@jmagman](https://github.com/jmagman)가 해당 증상에 대해 코멘트를 달아 공유합니다👻

평소와 같이 앱을 테스트 디바이스 iPhone 6S에 빌드하려 했는데 시작시 충돌이 발생했습니다.

```bash
Launching lib/main.dart on Tyler Grey의 iPhone in debug mode...
Automatically signing iOS for device deployment using specified development team in Xcode project: 785U498M97
Running Xcode build...
Xcode build done.                                           28.8s
Installing and launching...
```

install and launching 과정에서 디바이스에 앱이 실행하자마자 죽어버렸습니다. 그리곤 계속 저 상태가 유지됐습니다.

좀 더 명확한 이유를 알기 위해 `flutter run -debug -v` 명령어로 로그를 확인해 봤습니다.

```bash
...

[ +224 ms] success
[        ] (lldb)     safequit
[ +118 ms] Process 363 detached
[  +65 ms] Application launched on the device. Waiting for observatory port.
[   +5 ms] Checking for advertised Dart observatories...
[+5026 ms] No pointer records found.
[   +3 ms] mDNS lookup failed, attempting fallback to reading device log.
[        ] Waiting for observatory port.
```

이 로그를 보고, 처음에 방향을 잘못 잡았습니다..😂 Hot reload에서 프록시가 잘못 된 줄 알고 시간을 한참 더 쓴 것 같아요. 관련해서 스택오버플로우에 방화벽을 열어라 등 다양한 답변이 있었지만 저에겐 별로 도움이 되지 않았습니다.

그러다 마지막으로 Xcode에서 빌드를 해보자고 마음을 먹고, 빌드를 해보니

```bash
dyld: Library not loaded: @rpath/Flutter.framework/Flutter
  Referenced from: /private/var/containers/Bundle/Application/D05EC662-6EB2-47C0-BE00-7248DFE0A1DF/Runner.app/Runner
  Reason: no suitable image found.  Did find:
 /private/var/containers/Bundle/Application/D05EC662-6EB2-47C0-BE00-7248DFE0A1DF/Runner.app/Frameworks/Flutter.framework/Flutter: code signature invalid for '/private/var/containers/Bundle/Application/D05EC662-6EB2-47C0-BE00-7248DFE0A1DF/Runner.app/Frameworks/Flutter.framework/Flutter'

...
```

이번엔 이런 에러가 달렸습니다. OS X에서 동적 라이브러리 로더인 `dyld`에서 플러터를 로드하지 못했다는 로그였습니다. 이 로그 중 `code signature invalide for ..`로 구글링 한 결과 저와 동일한 증상의 [이슈](https://github.com/flutter/flutter/issues/49504)를 발견했습니다.

[Flutter 멤버의 코멘트](https://github.com/flutter/flutter/issues/49504#issuecomment-592767041)에서 **iOS 13.3.1 부터 해당 증상이 나타났고, iOS 13.4 beta 3 에서는 해결 됐다고 합니다.** 아직 해당 이슈가 오픈 되어 있지만 다음 OS 버전이 배포 되면 closed 됩니다.

얼마 전 테스트 디바이스 OS 업데이트를 했는데 그게 영향이 있었나 보네요.  
저는 디바이스에 빌드하는건 포기하고 시뮬레이터로 테스트하고 있지만, 급하신 분은 iOS 13.4 beta 3 으로 먼저 테스트하는 것도 좋을 것 같아요.
