# 챕터 1 첫 주제

## charactor

C언어에서 문자를 표현하기 위해 사용하는 기본적인 자료형은 char이다. char는 "character(문자)"의 약어로, 일반적으로 문자 하나를 표현할 때 사용된다.

### char 자료형의 크기와 저장 방식
`char` 자료형은 **1바이트(8비트)** 크기를 가지며, 이는 최대 256가지(2⁸)의 값을 표현할 수 있다는 뜻이다. 대부분의 시스템에서는 이 256개의 값 중 일부(0~127)를 **아스키(ASCII)** 문자 코드에 매핑하여 사용한다.

예를 들어, 다음과 같이 `char` 변수에 문자를 저장할 수 있다.

이때 변수 `ch`에는 문자 `'A'`가 저장되는 것이 아니라, **'A'에 해당하는 아스키 코드 값인 정수 65**가 저장된다. 즉, `char`는 본질적으로 정수형 자료형 중 하나이며, 단지 이를 문자로 해석하여 출력하거나 비교하는 것이다.

### char의 출력 방식
다음은 `char` 자료형의 값을 문자와 숫자 형태로 각각 출력하는 예이다.

```c
char ch = 'A';
printf("%c\n", ch);  // 출력: A
printf("%d\n", ch);  // 출력: 65
```

- `%c`는 해당 값을 문자로 출력한다.
- `%d`는 해당 값을 정수로 출력한다.

이처럼 `char`는 내부적으로 숫자를 저장하고, 필요한 상황에 따라 문자로 해석하여 사용할 수 있다.

### char의 메모리 구조
`char` 자료형은 메모리상에 1바이트(8비트)로 저장된다. 예를 들어, `'A'`는 아스키 코드 값으로 65이며, 2진수로 표현하면 `01000001`이다. 따라서 다음과 같이 메모리에 저장된다.
```c
ch = 'A'

메모리 (2진수): 01000001
메모리 (16진수): 0x41
```
자세한 아스키 코드는 아래 이미지를 참조하자.
<img src="https://i.namu.wiki/i/PBslb_v1iy979265LgcwUjiNS211qSQis50dqxKPy7zOsNmLSM4QDdGeOuf-N3KCE-jfKYNN5gxdyrmcZgHxqFOK9oyTj7qHSIxNZPNabKuwD3-yvbJFfjdqMGQSUFpz_Zdcbv9GkCUPHBvGCtEP5g.gif">

