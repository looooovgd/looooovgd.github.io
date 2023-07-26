---
title: JS es6 비구조화 할당
date: 2023-07-26 12:18:13 +0900
categories: [Bloggings, Javascript]
tags: [js],[es6],[자바스크립트],[문법]    # TAG names should always be lowercase
---
## Js의 특이한 문법, 비구조화 할당 (destructing assignment)
자바스크립트를 리액트에서 처음 접하게 되어서, 문법공부를 한다기보다는 읽으면서 공부하자는 마인드로 시작하였습니다.
읽다가 이런 스타일의 문법을 접하게 되어, 나중에 찾기 쉽게 하기위해 기록하게 되었습니다.

> _비구조화 할당_ 이란?
: {} 스코프 안에 있는 값을 스코프 밖에서도 쓸 수 있게 꺼내어주는 것.

밑의 예시와 같은 스타일입니다.

```javascript
const jayce = {
	a: 7,
    b: 6,
}
const {a:alpha,b} = jayce //비구조화 할당
console.log(`출력결과: ${alpha}*${b} = ${alpha*b}`)
```
출력결과
`출력결과: 7*6 = 42`

-
비구조화 할당: a와b는 james안에서만 사용될 수 있었으나,비구조화 할당 후 괄호 밖에서도 사용 할 수 있습니다.
-
**`a:alpha`**라는 부분은 jayce의 a를 가져와서 선언 할 때 alpha로 이름을 바꾸겠다는 말 입니다.

***

> 실제로 쓰일 때에는...

```javascript
const jinx = {
	a: 10,
  	b: 6,
  fire: function (){
    console.log(`${this.a}%의 추가데미지 사격`)
  }
}

function attack(aa,bb){
	console.log(`${aa}+${bb}만큼의 피해를 입힙니다`)
}

const {a:aaa,b:bbb} = jinx
attack(aaa,bbb)
```
출력결과
`10+6만큼의 피해를 입힙니다`

---
> es6 **화살표 함수**에서는 보통 쓰는 방식이 다릅니다.

```javascript
const jinx = {
	a: 90,
  	b: 50,
}

const looose = (ad,bb) => {
    console.log(`${ad}+${bb}%의 추가데미지 사격`)
}
const {a:ad,b:bb} = jinx
looose(ad,bb)

```
실행결과: `90+50%의 추가데미지 사격`

- 이렇게 이름을 바꾸어서 전달해 줄 수 있으며,

### 주로 쓰이는 방식은,
```javascript
const bilibili = ({a,b}) => {
    console.log(`${a}+${b}%의 피해를 입히는 덫 설치`)
}

bilibili(jinx)
```
실행결과: `90+50%의 피해를 입히는 덫 설치`

이렇게 같은 형태로 변수를 바로 사용하고 싶은 경우입니다.
- 화살표 함수 인자받는 부분에 저렇게 {} 형태의 비구조화 할당을 하는 것으로 가독성을 높일 수 있습니다.
 

