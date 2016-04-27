---
layout: post
title: 맥에서 카카오톡 테마 만들 때 압축 문제
tag: 
 - OSX
published: true
---
# 들어가기에 앞서

저는 개발에 대해서는 쥐뿔도 모르고 깨작깨작 여기저기서 찾아 깊는 평범한 일반인입니다. 요점인즉슨 포스트에 적혀있는 게 제가 아는 전부이며 다른 걸 물어보셔도 *찾아서 나온다면 대답해드릴 수 있을지언정 그 자리에서 대답해드리기는 어렵습니다*. 물론 그 정도면 직접 찾아보셔도 금방 해답을 찾으실 수 있을 거라 장담합니다만... 

- - -

사람들이 카카오톡에 테마를 입혀서 사용한 건 벌써 4년이 넘었습니다. 2012년 무렵에 카카오톡이 정식으로 지원하기 전에도 윈터보드나 직접 내부 파일을 교체하는 방식으로 테마를 입혀서[^1] 사용했으니까요. 저도 윈도우 컴퓨터를 사용하던 시절에 제 테마를 만들어 사용하기도 했기에 어떻게 돌아가는 지 대략은 알고 있었습니다. 문제는 2012년도 무렵에 맥으로 넘어오면서부터였습니다. 윈도우에서는 멀쩡하게 적용되던 테마 파일이 맥에서 컴파일[^2]만 하면 적용이 안 되는 일이 생겼습니다.

[^1]: 또는 *변형을 해서*. 당연히 이건 무지 귀찮은 게 업데이트 할 때 마다 내부 파일을 교체해주어야 했으므로... 지금 맥에서 카카오톡 아이콘을 교체해서 사용하는 것도 이 방식이기 때문에 ***매우*** 귀찮습니다.

[^2]: 압축하고 적용가능한 포맷으로 변환하는 걸 편의상 컴파일이라고 부를 참입니다.

다행이라고 해야 할 지 저만 그런 게 아니더군요. [아사모 글1](http://cafe.naver.com/appleiphone/3437410), [아사모 글2](http://cafe.naver.com/appleiphone/3453202), [맥쓰사 글](http://cafe.naver.com/inmacbook/966891). 다만 국내 맥 사용자 수가 *비교적* 많지 않은 탓인지 명쾌한 해결책을 제시하는 글이 없었어요. 그렇게 4년 동안 고민하다 오늘 점심에 한 가지를 빼먹었다는 것을 깨달았습니다. 압축 구조 문제였죠.

탈옥 사용자들이 Springboard에 테마를 입힐 때 가장 많이 쓰는 툴 중에 Winterboard라는 게 있습니다. 폴더 구조대로 배열된 리소스를 가져와서 해당하는 요소 위에 덧입히는 건데요, 처음 테마를 적용해보는 분들이 많이 범하는 실수 중에 하나가 최상위 폴더를 하나 더 생성하는 겁니다. 알집이나 다른 압축 해제 프로그램으로 압축을 해제할 때 설정에 따라 최상위 폴더를 생성하고 그 안에 내용물을 풀어놓는 경우가 있는데 이걸 그대로 가져다 리소스로 올리는 거죠. 당연히 인식하지 못합니다.

카카오톡 테마 파일도 마찬가지입니다. ***폴더를 압축하면서 최상위 폴더를 하나 더 만든 셈입니다.***

문제의 원인이 파악되었으니 해결책도 나온 것과 다름없습니다. 최상위 폴더를 압축하는 것이 아닌 컨텐츠 자체를 압축하는 것입니다. 여기에는 세 가지 방법이 있습니다.

## Finder를 이용하는 방법[^3]

1. 만들어 둔 테마 폴더를 열고 KakaoTalk.css와 Images를 선택 해줍니다.
2. 선택한 상태에서 우클릭 또는 Ctrl + 클릭하여 메뉴를 열고 '2개 항목 압축'을 클릭합니다.  
![](/Resources/2016-04-27/2개항목압축.png){: .center-image :}
3. 생성된 압축파일의 확장자를 .ktheme으로 바꿔줍니다.

[^3]: 다만 이 경우에는 .DS_Store 라든가 기타 불필요한 숨겨진 파일들이 함께 압축될 수 있으므로 `defaults write com.apple.finder AppleShowAllFiles TRUE;killall Finder` 등으로 숨김 파일을 표시하고 사용하는 게 바람직합니다.[BackToTheMac 포스트, 9. 터미널 명령어로 숨겨진 파일 표시하기 부분 참조](http://macnews.tistory.com/203)

## Terminal을 이용하는 방법

![](/Resources/2016-04-27/navtothemefiles.png){: .center-image :}

1. 기본 앱인 Terminal.app을 실행하고 KakaoTalk.css가 위치한 디렉터리로 이동합니다.[^4]  
![](/Resources/2016-04-27/zipcommand.png){: .center-image :}*예시. 파일이름을 `KakaoTheme`으로 한 경우*
2. `zip -X {파일이름} *` 을 입력하고 return을 눌러줍니다. ***대소문자, 띄어쓰기 모두 준수하셔야 합니다.*** -X 옵션은 .DS_Store 와 같은 부스러기 파일들을 포함하지 않고 압축하는 옵션이고, 해당 명령어는 디렉터리 내부의 파일을 모두 압축하는 명령어입니다.  
![](/Resources/2016-04-27/final.png)
3. 생성된 압축파일의 확장자를 .ktheme으로 바꿔줍니다.

[^4]: 저 같은 경우는 데스크탑에 downloads 폴더를 만들어두고 그 안에 KakaoTheme이라는 폴더를 만들어서 작업하였으므로 `cd desktop/downloads/Kakaotheme`을 입력하고 return을 눌러줍니다. 여기서는 대소문자 구별을 하지 않습니다.

## 서드파티 앱을 사용하는 방법

일반 사용자들에게 가장 추천할만한 방법이 아닐까 *개인적으로* 생각합니다.

1. CleanArchiver를 설치합니다.  
![](/Resources/2016-04-27/cleanarchiversetup.png){: .center-image :}
2. 위와 같이 CleanArchiver를 설정하고 KakaoTalk.css 와 Images 를 선택하여 CleanArchiver 창으로 던져넣습니다. 아래 사진처럼요.  
![](/Resources/2016-04-27/dragintocleanarchiver.png){: .center-image :}
3. 생성된 압축파일의 확장자를 .ktheme으로 바꿔줍니다.

- - -