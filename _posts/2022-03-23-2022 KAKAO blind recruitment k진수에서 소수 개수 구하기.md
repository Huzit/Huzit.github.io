---
layout: post 
title: 2022 KAKAO blind recruitment k진수에서 소수 개수 구하기
subtitle: 프로그래머스
categories: 코딩문제[프로그래머스]
tags: [코딩테스트, 자바, 코틀린]
---
### 문제설명
양의 정수 n이 주어집니다. 이 숫자를 k진수로 바꿨을 때, 변환된 수 안에 아래 조건에 맞는 소수(Prime number)가 몇 개인지 알아보려 합니다.

- 0P0처럼 소수 양쪽에 0이 있는 경우
- P0처럼 소수 오른쪽에만 0이 있고 왼쪽에는 아무것도 없는 경우
- 0P처럼 소수 왼쪽에만 0이 있고 오른쪽에는 아무것도 없는 경우
- P처럼 소수 양쪽에 아무것도 없는 경우
- 단, P는 각 자릿수에 0을 포함하지 않는 소수입니다.
  - 예를 들어, 101은 P가 될 수 없습니다.

예를 들어, 437674을 3진수로 바꾸면 211020101011입니다. 여기서 찾을 수 있는 조건에 맞는 소수는 왼쪽부터 순서대로 211, 2, 11이 있으며, 총 3개입니다. (211, 2, 11을 k진법으로 보았을 때가 아닌, 10진법으로 보았을 때 소수여야 한다는 점에 주의합니다.) 211은 P0 형태에서 찾을 수 있으며, 2는 0P0에서, 11은 0P에서 찾을 수 있습니다.

정수 n과 k가 매개변수로 주어집니다. n을 k진수로 바꿨을 때, 변환된 수 안에서 찾을 수 있는 위 조건에 맞는 소수의 개수를 return 하도록 solution 함수를 완성해 주세요.
### 제한사항
- 1 ≤ n ≤ 1,000,000
- 3 ≤ k ≤ 10

### 입출력 예 

|n|k| result |
|---|---|--------|
|437674|	3| 	3     |
|110011|	10	| 2      |

### 입출력 예 설명

입출력 예 #1

문제 예시와 같습니다.

입출력 예 #2

110011을 10진수로 바꾸면 110011입니다. 여기서 찾을 수 있는 조건에 맞는 소수는 11, 11 2개입니다. 이와 같이, 중복되는 소수를 발견하더라도 모두 따로 세어야 합니다.

### 풀이
이 문제는 2파트로 나눠서 생각해볼 수 있다.
1. 진수변환함수
2. 소수판단함수

진수변환은 정수n을 k진수로 변경해야되고 변경된 결과에서 소수판단함수로 소수인지 여부를 파악해야 한다.    
P가 될 수 있는 경우를 나열했는데, 쉽게 요약하자면 "0"으로 split했을 때 1보다 큰 값이다. 

진수변환함수부터 구현해보자.
```java
public String convert(int digit, int num){
  StringBuilder sb = new StringBuilder();
  if(num == 0) return "0";

  while(num != 0){
      int remain = num % digit; //지수변환시작
      if(remain>9)
          sb.insert(0, Character.toChars(remain+55)); 
      else
          sb.insert(0, remain); 
      num/=digit;
  }
  return sb.toString();
}
```
눈여겨봐야할 부분은 while문 이다.

```java
while(num != 0){
  int remain = num % digit; //지수변환시작
  if(remain>9)
      sb.insert(0, Character.toChars(remain+55)); //ASCII코드에서 65는 A, 65에서 두자릿수 최소값 10을빼면 55, 따라서 55를 더해줌
  else
      sb.insert(0, remain); //아니면 맨앞에 그냥 추가
  num/=digit;
}
```
remain값에 따라 stringBuilder에 다른 값을 넣고있다. 16진수를 생각해보면 9다음 수는 A, B, C, D....로 적는다. 마찬가지로 9가 넘어가면 영어 대문자가 나와야 되는데 ASCII코드에서 65는 A이다. 변환되는 기준의 되는 10과 55가 차이나므로 55를 더하고 문자로 바꿔서 넣어준다.

그 다음은 소수판단함수이다.
```java
public Boolean getPrime(Long prime){
    for (long i = 2L; i <= Math.sqrt(prime); i++) {
        if(prime%i==0L)
            return false;
    }
    return true;
}
```
소수판단함수의 키포인트는 2가지다.
- Long타입 패러미터
- Math.sqrt(x)함수

첫 번째    
패러미터 타입을 int로 설정하면 들어오는 n의 값이 적을 때는 문제없이 작동된다. 하지만 최대값인 백만을 3진수로 변경하면 int의 최대범위인 2,147,483,647가 넘는 1,212,210,202,001이 나온다. 이는 unsigned Integer로 해도 크기때문에 Long타입으로 해야한다.

두 번째 
Math.sqrt(x)는 x의 제곱근을 구하는 함수다. ~~갑자기 웬 제곱근?~~ 우리가 한 수의 약수를 구하게되면 제곱근을 제외하고 짝수개의 약수가 나온다. 짝이되는 약수는 소수판단에 동일한 결과를 배출하므로 시간을 아끼기 위해선 2부터 제곱근까지의 수를 나눠서 소수인지 판단하는게 좋다.

마지막으로 메인이되는 soluation함수이다.
```java
public int solution(int n, int k) {
  final int[] answer = {0};
  Arrays.stream(convert(k, n)
    .split("0"))
    .filter(x -> !x.equals("") && Long.parseLong(x) > 1L)
    .forEach(x -> {
      if (getPrime(Long.parseLong(x)))
          answer[0]++;
    });

  return answer[0];
}
```
람다를 이용해서 풀었는데 차례대로 설명하면 0으로 쪼개기 -> ""가 아니면서 1보다 큰 수인 원소 -> 반복문을 통해 소수판단
이다. !x.equals("")를 적은 이유는 NumberForamtException때문이다. 왜인지 모르겠는데 쪼갤때 공백값이 포함되서 타입 캐스팅시 예외가 발생한다.
테스트케이스 전체가 다 예외가 발생하지않는것으로 봐선 일부 테스트케이스의 n값에 공백문자가 들어가있는거같다.
   
이렇게 세 파트로 나눠서 풀어봤는데 카카오...역시 어렵다. 아래는 전체 코드와 코틀린 코드를 남기면서 마무리하겠다.   
~~람다쓸때는 확실히 코틀린이 편해~~

```java
import java.util.Arrays;

public class Main {
    public int solution(int n, int k) {
        final int[] answer = {0};
        Arrays.stream(convert(k, n)
                .split("0"))
                .filter(x -> !x.equals("") && Long.parseLong(x) > 1L)
                .forEach(x -> {
                    if (getPrime(Long.parseLong(x)))
                        answer[0]++;
                });

        return answer[0];
    }

    public String convert(int digit, int num){
        StringBuilder sb = new StringBuilder();
        if(num == 0) return "0";

        while(num != 0){
            int remain = num % digit; //지수변환시작
            if(remain>9)
                sb.insert(0, Character.toChars(remain+55)); //ASCII코드에서 65는 A, 65에서 두자릿수 최소값 10을빼면 55, 따라서 55를 더해줌
            else
                sb.insert(0, remain); //아니면 맨앞에 그냥 추가
            num/=digit;
        }
        return sb.toString();
    }

    public Boolean getPrime(Long prime){
        for (long i = 2L; i <= Math.sqrt(prime); i++) {
            if(prime%i==0L)
                return false;
        }
        return true;
    }
}
```
#### 코틀린
```kotlin
import kotlin.math.sqrt

fun main() {
    println(solution(997244, 3))
}

fun solution(n: Int, k: Int): Int{
    var answer: Int = 0
    convert(k, n) //진수 변환
        .split("0") //조건에 맞게 0을 기점으로 소수 판별할 수 컷팅
        .filter { it.toLongOrNull() != null && it.toLong() > 1L} //""을 Long타입 변환인한 예외가 없거나 소수조건에 포함되지않는 1은 제외
        .forEach {
        if(getPrime(it.toLong()))
            answer++
        }
    return answer
}

//digit : 진수
//num : 정수
fun convert(digit: Int, num: Int): String{
    var number = num //num이 immutable하므로 number에 할당
    val sb = StringBuilder()
    if(number == 0) return "0" //0이면 반환
    while (number!=0){
        var remain = number % digit //지수변환시작
        if(remain > 9) //한자리 이상일경우
            sb.insert(0, (remain+55).toChar()) //ASCII코드에서 65는 A, 65에서 두자릿수 최소값 10을빼면 55, 따라서 55를 더해줌
        else
            sb.insert(0, remain) //아니면 맨앞에 그냥 추가
        number/=digit
    }
    return sb.toString()
}

//소수 여부
//패러미터가 Long타입인 이유(부제 : 내가 오답이 나왔던 이유) 2147483647을 넘는 수가 1000000을 3진수로 변환하면 충분히 넘기때문
fun getPrime(prime: Long): Boolean{
    //범위가 제곱근인 이유(부제 : 내가 오답이 나왔던 두 번째 이유)
    //prime은 제곱근을 넘어가면 짝이되는 약수로 넘어가기전에 연산을 했기 때문에 줄임
    for(it in 2L..sqrt(prime.toDouble()).toLong() ){
        if( prime % it == 0L)
            return false
    }
    return true
}
```
