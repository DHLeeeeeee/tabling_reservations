﻿# 예약 목록 관리 App
JSON 형식으로 된 예약 데이터를 받아와서 Java script를 활용해 예약 목록을 구성하고 관리가 가능한 App을 구현하였습니다.
 
 
## 페이지 주소
https://dhleeeeeee.github.io/tabling_reservations/


## App의 동작 순서


1. 화면이 load될 때 시스템을 초기화 합니다.
2. 초기화 함수 내에 API 호출 함수가 있어 지정된 API 경로에서 데이터를 받아옵니다.
3. 받아 온 데이터를 로컬 스토리지에 저장합니다.
4. 로컬 스토리지에 있는 데이터를 바탕으로 예약 목록 리스트를 구성합니다.
5. 데이터에서 가장 상위에 있는 예약 데이터의 내용을 예약 상세란에 표시합니다.


## 예약 리스트의 구성


fetch 메소드를 사용해 데이터를 받아왔고, 그 안에는 여러개의 예약 데이터들이 배열로 구성되어 있었습니다.
예약 데이터의 상태값이 done이 아닌 예약 데이터만을 리스트에 표시하기 위해서 filter 메소드를 사용하여 데이터를 선별했습니다.
그 다음 forEach 메소드를 이용해 각각의 예약 데이터를 미리 작성해둔 리스트 아이템 생성함수로 리스트 아이템으로 생성했습니다.


### 문제점과 해결
1. 데이터를 호출하는 기능은 비동기적으로 작동해서 HTML 요소가 데이터를 구성하기 전에 생성되어 내용이 빈 상태가 됩니다.
그래서 async/await 문법을 이용해서 데이터 구성이 끝난 다음 리스트 아이템 생성 함수를 실행하게 했습니다.
2. 예약 리스트의 예약시간, 접수시간의 데이터가 YYYY-MM-DD HH:mm:ss 형식이였는데, 표시할때에는 HH:mm 형식으로 표시해야 했습니다.
시간을 표시해야하는 부분이 여러곳이 있어서 재사용성을 위해 시간 데이터를 표시하는 코드에는 시간 포맷을 재구성하는 함수를 따로 만들어서 실행시켜 항상 동일한 포맷으로 표시하도록 했습니다.

## 예약 리스트의 기능


1. 예약 리스트의 아이템을 클릭하면 해당 아이템의 내용이 예약 상세란에 표시됩니다.
2. 예약 리스트 아이템의 좌측에는 예약의 상태를 나타내고 상태에 따라 내용과 글자의 색상을 다르게 표시합니다 (reserved는 “예약", seated는 “착석 중”)
3. 예약 리스트 아이템의 우측에 있는 버튼을 클릭해 해당 데이터의 상태를 변경하여 관리 할 수 있습니다. ([착석] 버튼을 클릭하면, [퇴석]으로 버튼을 변경,  [퇴석] 버튼을 클릭하면, 예약 목록에서 제거)
 


### 문제점과 해결


1. 예약 상세란 표시기능이 동작할때 예약 데이터에 있는 여러개의 데이터 중 클릭된 아이템의 데이터를 표시해야 했습니다.
그래서 HTML을 구성할때 리스트 아이템 요소의 ID 속성에 해당 데이터의 ID값을 입력했고, 그 ID값과 데이터의 ID값이 일치하는 데이터를 가져와 표시되게 했습니다.
2. 예약 리스트 아이템의 버튼 기능은 동적으로 데이터의 상태 값을 변경하고 그 변경 된 상태에 따라서 예약 리스트를 구성해야 했습니다.
그래서 리스트 아이템이 구성될때 버튼에도 해당 데이터의 ID값을 ID속성에 입력했고(리스트 아이템의 ID와의 중복을 피하기 위해 'btn'이라는 문자열을 추가),
버튼을 클릭했을때 해당 ID값의 데이터 상태값을 변경하고 리스트 업데이트 함수를 만들어 변경된 데이터로 다시 HTML을 생성하게 했습니다.(replace 메소드로 ID속성의 문자열을 제거)
3. 예약 리스트 아이템의 버튼을 클릭했을때 해당 데이터의 상태값이 done으로 변경되어 리스트에서 제거된다면, 예약 상세란 표시를 리스트 최상단 아이템의 데이터로 표시하는게 사용자경험을 개선 할 것이라 생각했습니다.
그래서 버튼 클릭 이벤트 함수에 클릭 된 아이템의 데이터 상태값이 done이 됐을때 상태값이 done이 아닌 데이터 중 최상단의 데이터를 표시하게 하고,
모든 예약 데이터의 상태값이 done이 되어 예약 리스트에 아무것도 남지 않았을 경우에는 예약 상세란을 비워두는 초기화 함수를 작성했습니다.

