[Human Interface Guidelines - Icons and Images - System Icons](https://developer.apple.com/design/human-interface-guidelines/ios/icons-and-images/system-icons/)

# 시스템 아이콘*System Icons*

시스템은 일반적인 작업과 다양한 유스케이스의 컨텐츠 타입을 나타내는 내장 아이콘을 제공합니다.

- [내비게이션 바 및 툴바 아이콘](https://developer.apple.com/design/human-interface-guidelines/ios/icons-and-images/system-icons/#navigation-bar-and-toolbar-icons)
- [탭 바 아이콘](https://developer.apple.com/design/human-interface-guidelines/ios/icons-and-images/system-icons/#tab-bar-icons)
- [홈 화면 빠른 액션 아이콘](https://developer.apple.com/design/human-interface-guidelines/ios/icons-and-images/system-icons/#home-screen-quick-action-icons)

그것들은 사람들에게 익숙하기 때문에 가능한 한 많은 내장 아이콘을 사용하는 것은 좋은 생각입니다.

### 의도된 대로 시스템 아이콘을 사용하십시오.

시스템이 제공하는 모든 이미지는 특정한, 잘 알려진 의미를 갖습니다. 사용자들을 혼란스럽게 하지 않기 위하여, 각각의 이미지가 그 의미와 추천되는 사용법에 따라 사용되는 것은 필수적입니다.

### 아이콘에 대한 대안 텍스틀 레이블을 제공하십시오.

대안 텍스트 레이블은 화면 상에 보이지 않으나, VoiceOver가 화면 상에 무엇이 있는지 청각적으로 묘사할 수 있게 하여, 시각 장애가 있는 사람들이 더 쉽게 탐색할 수 있게 합니다.

### 당신의 필요를 충족시키지는 시스템이 제공하는 아이콘을 찾지 못했다면 커스텀 아이콘을 디자인 하십시오.

시스템이 제공하는 이미지를 오용하는 것보다는 당신 자신의 아이콘을 디자인하는 것이 낫습니다. [커스텀 아이콘](https://developer.apple.com/design/human-interface-guidelines/ios/icons-and-images/custom-icons/)을 참고하십시오.

## 내비게이션 바 및 툴바 아이콘*Navigation Bar and Toolbar Icons*

[내비게이션 바](https://developer.apple.com/design/human-interface-guidelines/ios/bars/navigation-bars/) 및 [툴바](https://developer.apple.com/design/human-interface-guidelines/ios/bars/toolbars/) 내에서 다음의 아이콘을 사용하십시오. 개발자 가이드에서, [UIBarButtonSystemItem](https://developer.apple.com/documentation/uikit/uibarbuttonsystemitem)을 참고하십시오.

**참고.** 내비게이션 바나 툴바에 있는 아이템을 나타내기 위해 아이콘 대신 텍스트를 사용할 수 있습니다. 예를 들어, 캘린더 앱은 툴바에 "오늘*Today*", "캘린더*Calendars*", "초대*Inbox*"를 사용합니다. 내비게이션과 툴바 아이콘 사이에 패딩을 제공하기 위하여 고정된 공간 요소*fixed space element*를 사용할 수도 있습니다.

| 아이콘 이름    | 의미                                                         | API                                                          |
| -------------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| Action (Share) | 현재 컨텍스트에서 유용한, 복사*Copy*, 즐겨찾기*Favorite* 또는 찾기*Find*와 같은 공유 확장, 액션 확장 및 작업을 포함하는 모달 뷰를 보여주기. | [action](https://developer.apple.com/documentation/uikit/uibarbuttonsystemitem/1617116-action) |
| Add            | 새로운 아이템을 생성하기.                                    | [add](https://developer.apple.com/documentation/uikit/uibarbuttonsystemitem/1617115-add) |
| Bookmarks      | 애플리케이션에 특화된 북마크를 보여주기.                     | [bookmarks](https://developer.apple.com/documentation/uikit/uibarbuttonsystemitem/1617144-bookmarks) |
| Camera         | 사진이나 비디오를 촬영하거나, 사진 라이브러리를 보여주기.    | [camera](https://developer.apple.com/documentation/uikit/uibarbuttonsystemitem/1617159-camera) |
| Cancel         | 현재 뷰를 닫거나 변화 저장 없이 편집 모드를 종료하기.        | [cancel](https://developer.apple.com/documentation/uikit/uibarbuttonsystemitem/1617133-cancel) |
| Compose        | 편집 모드에서 새로운 뷰를 열기.                              | [compose](https://developer.apple.com/documentation/uikit/uibarbuttonsystemitem/1617156-compose) |
| Done           | 상태를 저장하고 현재 뷰를 닫거나, 편집 모드를 종료하기.      | [done](https://developer.apple.com/documentation/uikit/uibarbuttonsystemitem/1617131-done) |
| Edit           | 현재 컨텍스트에서 편집 모드에 진입하기.                      | [edit](https://developer.apple.com/documentation/uikit/uibarbuttonsystemitem/1617168-edit) |
| Fast Forward   | 미디어 재생이나 슬라이드를 통해 빨리 감기.                   | [fastForward](https://developer.apple.com/documentation/uikit/uibarbuttonsystemitem/1617136-fastforward) |
| Organize       | 아이템을 폴더와 같은 새로운 목적지로 이동하기.               | [orgainze](https://developer.apple.com/documentation/uikit/uibarbuttonsystemitem/1617121-organize) |
| Pause          | 미디어 재생이나 슬라이드를 일시 정지하기. 일시 정지할 때 항상 현재 위치를 저장하여, 재생이 나중에 재개될 수 있게 하기. | [pause](https://developer.apple.com/documentation/uikit/uibarbuttonsystemitem/1617119-pause) |
| Play           | 미디어 재생이나 슬라이드를 시작하거나 재개하기.              | [play](https://developer.apple.com/documentation/uikit/uibarbuttonsystemitem/1617147-play) |
| Redo           | 실행 취소된 마지막 액션을 실행 복귀하기.                     | [redo](https://developer.apple.com/documentation/uikit/uibarbuttonsystemitem/1617137-redo) |
| Refresh        | 컨텐츠를 새로고침하기. 애플리케이션은 가능할 때마다 컨텐츠를 자동으로 새로고침 해야 하므로 이 아이콘은 아껴서 사용하기. | [refresh](https://developer.apple.com/documentation/uikit/uibarbuttonsystemitem/1617123-refresh) |
| Reply          | 아이템을 또다른 사람이나 위치로 전송하기.                    | [reply](https://developer.apple.com/documentation/uikit/uibarbuttonsystemitem/1617150-reply) |
| Rewind         | 미디어 재생이나 슬라이드를 통해 뒤로 감기.                   | [rewind](https://developer.apple.com/documentation/uikit/uibarbuttonsystemitem/1617112-rewind) |
| Save           | 현재 상태를 저장하기.                                        | [save](https://developer.apple.com/documentation/uikit/uibarbuttonsystemitem/1617126-save) |
| Search         | 검색 필드를 표시하기.                                        | [search](https://developer.apple.com/documentation/uikit/uibarbuttonsystemitem/1617146-search) |
| Stop           | 미디어 재생이나 슬라이드를 멈추기.                           | [stop](https://developer.apple.com/documentation/uikit/uibarbuttonsystemitem/1617142-stop) |
| Trash          | 현재 아이템 또는 선택된 아이템 삭제하기.                     | [trash](https://developer.apple.com/documentation/uikit/uibarbuttonsystemitem/1617167-trash) |
| Undo           | 마지막 액션을 실행 취소하기.                                 | [undo](https://developer.apple.com/documentation/uikit/uibarbuttonsystemitem/1617130-undo) |

## 탭 바 아이콘*Tab Bar Icons*

[탭 바](https://developer.apple.com/design/human-interface-guidelines/ios/bars/tab-bars/)에는 다음의 아이콘을 사용하십시오. 개발자 가이드에서, [UITabBarSystemItem](https://developer.apple.com/documentation/uikit/uitabbarsystemitem)을 참고하십시오.

| 아이콘 이름 | 의미                                                 | API                                                          |
| :---------- | ---------------------------------------------------- | ------------------------------------------------------------ |
| Bookmarks   | 애플리케이션에 특화된 북마크 보여주기.               | [bookmarks](https://developer.apple.com/documentation/uikit/uitabbarsystemitem/1617052-bookmarks) |
| Contacts    | 개인 연락처를 보여주기.                              | [contacts](https://developer.apple.com/documentation/uikit/uitabbarsystemitem/1617068-contacts) |
| Downloads   | 활성화되었거나 최근 다운로드를 보여주기.             | [downloads](https://developer.apple.com/documentation/uikit/uitabbarsystemitem/1617060-downloads) |
| Favorites   | 개인 즐겨찾기 아이템을 보여주기.                     | [favorites](https://developer.apple.com/documentation/uikit/uitabbarsystemitem/1617061-favorites) |
| Featured    | 애플리케이션에 의해 다루어진 컨텐츠 표시하기.        | [featured](https://developer.apple.com/documentation/uikit/uitabbarsystemitem/1617073-featured) |
| History     | 최근 액션이나 액티비티를 표시하기.                   | [history](https://developer.apple.com/documentation/uikit/uitabbarsystemitem/1617054-history) |
| More        | 추가적인 탭 바 아이템 표시하기.                      | [more](https://developer.apple.com/documentation/uikit/uitabbarsystemitem/1617069-more) |
| Most Recent | 특정 기간 내 최근에 접근된 컨텐츠나 아이템 표시하기. | [mostRecent](https://developer.apple.com/documentation/uikit/uitabbarsystemitem/1617064-mostrecent) |
| Most Viewed | 가장 유명한 아이템 표시하기.                         | [mostViewed](https://developer.apple.com/documentation/uikit/uitabbarsystemitem/1617058-mostviewed) |
| Search      | 검색 모드에 진입하기.                                | [search](https://developer.apple.com/documentation/uikit/uitabbarsystemitem/1617062-search) |
| Top Rated   | 가장 높은 평가를 받은 아이템 보여주기.               | [topRated](https://developer.apple.com/documentation/uikit/uitabbarsystemitem/1617059-toprated) |

## 홈 화면 빠른 액션 아이콘*Home Screen Quick Action Icons*

[홈 화면 빠른 액션]() 메뉴에는 다음의 아이콘을 사용하십시오. 개발자 가이드에서, [UIApplicationShortcutIconType](https://developer.apple.com/documentation/uikit/uiapplicationshortcuticontype)을 참고하십시오.

| 아이콘 이름    | 의미                                                         | API                                                          |
| -------------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| Add            | 새로운 아이템 생성하기.                                      | [add](https://developer.apple.com/documentation/uikit/uiapplicationshortcuticontype/1623363-add) |
| Alarm          | 알람을 설정하거나 표시하기.                                  | [alarm](https://developer.apple.com/documentation/uikit/uiapplicationshortcuticontype/1623369-alarm) |
| Audio          | 오디오를 나타내거나 조절하기.                                | [audio](https://developer.apple.com/documentation/uikit/uiapplicationshortcuticontype/1623391-audio) |
| Bookmark       | 북마크를 생성하거나 보여주기.                                | [bookmark](https://developer.apple.com/documentation/uikit/uiapplicationshortcuticontype/1623365-bookmark) |
| Capture Photo  | 사진 촬영하기.                                               | [capturePhoto](https://developer.apple.com/documentation/uikit/uiapplicationshortcuticontype/1623366-capturephoto) |
| Capture Video  | 비디오 촬영하기.                                             | [captureVideo](https://developer.apple.com/documentation/uikit/uiapplicationshortcuticontype/1623390-capturevideo) |
| Cloud          | 클라우드 기반 서비스를 나타내고, 표시하거나, 초기화하기.     | [cloud](https://developer.apple.com/documentation/uikit/uiapplicationshortcuticontype/1623347-cloud) |
| Compose        | 새로운 편집 가능한 컨텐츠를 작성하기.                        | [compose](https://developer.apple.com/documentation/uikit/uiapplicationshortcuticontype/1623388-compose) |
| Confirmation   | 액션이 완료되었음을 나타내기.                                | [confirmation](https://developer.apple.com/documentation/uikit/uiapplicationshortcuticontype/1623385-confirmation) |
| Contact        | 연락처를 선택하거나 표시하기.                                | [contact](https://developer.apple.com/documentation/uikit/uiapplicationshortcuticontype/1623383-contact) |
| Date           | 캘린더나 이벤트를 표시하거나, 관련 액션을 수행하기.          | [date](https://developer.apple.com/documentation/uikit/uiapplicationshortcuticontype/1623344-date) |
| Favorite       | 즐겨찾기 아이템을 나타내거나 표시하기.                       | [favorite](https://developer.apple.com/documentation/uikit/uiapplicationshortcuticontype/1623364-favorite) |
| Home           | 홈 화면을 가리키거나 표시하기. 물리적인 집을 가리키고, 표시하거나, 길을 안내하기. | [home](https://developer.apple.com/documentation/uikit/uiapplicationshortcuticontype/1623374-home) |
| Invitation     | 초대를 나타내거나 표시하기.                                  | [invitation](https://developer.apple.com/documentation/uikit/uiapplicationshortcuticontype/1623357-invitation) |
| Location       | 위치의 개념을 나타내거나 현재 지리학적 위치에 접근하기.      | [location](https://developer.apple.com/documentation/uikit/uiapplicationshortcuticontype/1623379-location) |
| Love           | 좋아하는 아이템을 나타내거나 표시하기.                       | [love](https://developer.apple.com/documentation/uikit/uiapplicationshortcuticontype/1623380-love) |
| Mail           | 메일 앱 메세지를 생성하기.                                   | [mail](https://developer.apple.com/documentation/uikit/uiapplicationshortcuticontype/1623350-mail) |
| Mark Location  | 지리학적 위치를 나타내고, 표시하거나, 저장하기.              | [markLocation](https://developer.apple.com/documentation/uikit/uiapplicationshortcuticontype/1623359-marklocation) |
| Message        | 새로운 메세지를 생성하거나 메세지의 사용을 나타내기.         | [message](https://developer.apple.com/documentation/uikit/uiapplicationshortcuticontype/1623378-message) |
| Pause          | 미디어 재생을 일시 정지하기. 일시 정지할 때 현재 상태를 항상 저장하여 나중에 재개될 수 있게 하기. | [pause](https://developer.apple.com/documentation/uikit/uiapplicationshortcuticontype/1623386-pause) |
| Play           | 미디어 재생을 시작하거나 재개하기.                           | [play](https://developer.apple.com/documentation/uikit/uiapplicationshortcuticontype/1623377-play) |
| Prohibit       | 무언가가 허용되지 않음을 나타내기.                           | [prohibit](https://developer.apple.com/documentation/uikit/uiapplicationshortcuticontype/1623358-prohibit) |
| Search         | 검색 모드에 진입하기.                                        | [search](https://developer.apple.com/documentation/uikit/uiapplicationshortcuticontype/1623360-search) |
| Share          | 다른 사람이나 소셜 미디어에 컨텐츠를 공유하기.               | [share](https://developer.apple.com/documentation/uikit/uiapplicationshortcuticontype/1623353-share) |
| Shuffle        | 셔플 모드를 가리키거나 개시하기.                             | [shuffle](https://developer.apple.com/documentation/uikit/uiapplicationshortcuticontype/1623381-shuffle) |
| Task           | 완료되지 않은 작업을 나타내거나 완료된 것으로 작업을 표시하기. | [task](https://developer.apple.com/documentation/uikit/uiapplicationshortcuticontype/1623349-task) |
| Task Completed | 완료된 작업을 나타내거나 완료되지 않은 것으로 작업을 표시하기. | [taskCompleted](https://developer.apple.com/documentation/uikit/uiapplicationshortcuticontype/1623368-taskcompleted) |
| Time           | 시계나 타이머를 나타내거나 표시하기.                         | [time](https://developer.apple.com/documentation/uikit/uiapplicationshortcuticontype/1623345-time) |
| Update         | 컨텐츠 갱신하기.                                             | [update](https://developer.apple.com/documentation/uikit/uiapplicationshortcuticontype/1623346-update) |