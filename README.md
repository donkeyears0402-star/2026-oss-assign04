# 오픈소스 스튜디오 과제 04

컴퓨터공학과 이준형 (21901037)

이번 주는 HTML Form을 직접 만들고, CSS로 모양을 잡은 다음 JavaScript로 제출을 막아 보며 입력값을 검사했다.

## 페이지

- [form1.html](form1.html) : Form 구조
- [form1_css.html](form1_css.html) : CSS 적용
- [form1_js.html](form1_js.html) : Validation + JavaScript

Clone Coding 원본: https://getbootstrap.com/docs/5.2/examples/checkout/

디자인을 똑같이 베끼기보다, 주문자 / 배송지 / 결제 / 추가 옵션으로 나뉜 checkout form 구조를 가져왔다. 결제는 실제로 이루어지지 않는다.

## Key Learning

1. `form` 안에 `input`, `select`, `textarea`를 넣고 `name`을 줘야 어떤 값이 제출되는지 구분된다.
2. CSS는 구조를 바꾸지 않고 여백, 테두리, 색, 정렬만 바꿔도 입력 화면처럼 보인다.
3. `required`만으로는 submit 이벤트가 막힐 수 있어서, JavaScript에서 `checkValidity()`로 다시 확인해야 알림을 직접 띄울 수 있다.

## Form Elements

| 요소 | 용도 |
| --- | --- |
| `input type="text"` | 이름, 주소, 카드 번호처럼 짧은 글 |
| `input type="email"` | 이메일 형식 검사 |
| `input type="password"` | 주문 확인용 비밀번호, `minlength="6"` |
| `input type="radio"` | 배송 방법, 결제 수단 중 하나 선택 |
| `input type="checkbox"` | 주소 저장, 다음에도 사용처럼 켜고 끄는 항목 |
| `input type="date"` | 희망 배송일 |
| `input type="color"` | 선물 포장 리본 색 |
| `select` + `optgroup` | 국가, 지역(시/도와 광역시를 묶어서) |
| `datalist` | 건물명 후보를 보여 주고 직접 입력도 가능하게 |
| `textarea` | 배송 메모 |
| `fieldset` + `legend` | 주문자, 배송지, 결제, 추가 옵션 묶음 |

입력 칸은 10개가 넘는다.

## HTML vs CSS

`form1.html`은 태그와 속성만 있는 파일이다. 브라우저 기본 모양이라 칸이 세로로 늘어지고 버튼도 작다.

`form1_css.html`은 같은 form을 복사한 뒤 `color`, `background-color`, `border`, `border-radius`, `padding`, `margin`, `width`, `display`로 정리했다. label은 블록으로 두고, 입력 칸은 가로를 꽉 채웠다. 마우스를 올리면 테두리 색이 바뀌고, focus되면 파란 테두리가 보이게 했다.

## Validation & JS

`form1_js.html`에 넣은 조건은 아래와 같다.

- 성, 이름, 사용자 이름, 이메일, 비밀번호, 주소, 국가, 카드 명의, 카드 번호에 `required`
- 이메일은 `type="email"`
- 비밀번호는 `minlength="6"`, 사용자 이름은 `minlength="4"`, 카드 번호는 `minlength="8"`

제출 버튼을 누르면 `addEventListener("submit")`이 실행되고, 맨 앞에서 `event.preventDefault()`로 페이지 이동을 막는다. 이메일이 틀리면 알림 후 `focus()`하고 `return`한다. 비밀번호가 6자보다 짧아도 같이 처리한다. 그 다음 `form.checkValidity()`가 거짓이면 처음 틀린 칸으로 포커스를 옮기고 함수를 끝낸다. 전부 통과하면 `alert("등록이 완료되었습니다.")`가 뜬다.

확인한 경우:

- 실패: 이메일에 `abc`만 입력하면 "유효한 이메일을 입력하세요."
- 실패: 비밀번호 `123`은 6자 미만이라 알림
- 성공: 필수 칸을 형식에 맞게 채우면 "등록이 완료되었습니다."

## Problem & Solution

`required`를 넣고 제출하면 브라우저 말풍선이 먼저 뜨고, 내가 만든 `alert`는 실행되지 않았다. 찾아보니 기본 검사를 통과하지 못하면 `submit` 이벤트 자체가 발생하지 않는다. form에 `novalidate`를 붙여 기본 말풍선을 끄고, JavaScript의 `checkValidity()`로 같은 검사를 하게 바꿨다.

## Reflection

`name`이 같으면 radio가 한 그룹이 된다는 점은 직접 눌러 보고 알았다. 지역 `select`에 `optgroup`을 넣으니 목록이 덜 헷갈렸다. `datalist`는 선택지가 보여도 다른 글자를 칠 수 있어서 `select`와 다르다는 점이 신기했다. 카드 번호는 아직 숫자만 들어가게 막지는 못했고, 길이만 보고 있다. 다음엔 `pattern`도 써 보고 싶다.

## Weekly Question

Google Form 제출용으로 두 문제를 적어 두었다. https://forms.gle/QoxoyWP8ZiJTyJu67

1. 단답형. 폼 제출의 기본 동작(페이지 이동)을 막으려면 어떤 메서드를 호출해야 하는가?
   - 정답: `preventDefault()`
   - 해설: `submit` 이벤트 객체에서 `event.preventDefault()`를 호출하면 브라우저가 action 주소로 넘어가지 않는다. 그래서 알림을 띄운 뒤에도 입력 내용이 남아 있다.

2. OX. `checkValidity()`가 `false`를 반환하면 그 입력값은 유효하지 않으므로, `focus()`로 그 칸에 커서를 두고 `return`으로 함수를 끝내면 된다.
   - 정답: O
   - 해설: `checkValidity()`는 `required`, `type="email"`, `minlength` 같은 HTML 제약 조건을 검사한다. 거짓이면 아직 등록이 끝나면 안 되므로 해당 요소에 포커스를 주고 함수를 종료한다.
