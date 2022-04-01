---
layout: post 
title: 2022 KAKAO blind recruitment 주차 요금 계산
subtitle: 프로그래머스
categories: 코딩문제[프로그래머스]
tags: [코딩테스트, 자바, 코틀린]
---
### 문제설명
주차장의 요금표와 차량이 들어오고(입차) 나간(출차) 기록이 주어졌을 때, 차량별로 주차 요금을 계산하려고 합니다. 아래는 하나의 예시를 나타냅니다.

- 요금표   

|기본 시간(분)|	기본 요금(원)|	단위 시간(분)|	단위 요금(원)|
|---|---|---|---|
|180|	5000|	10|	600|

- 입/출차 기록

 |시각(시:분)	|차량 번호	|내역|
 |---|---|---|
 |05:34	      |5961	    |입차|
 |06:00	      |0000	    |입차|
 |06:34	      |0000	    |출차|
 |07:59	      |5961	    |출차|
 |07:59	      |0148	    |입차|
 |18:59	      |0000	    |입차|
 |19:09	      |0148	    |출차|
 |22:59	      |5961	    |입차|
 |23:00	      |5961	    |출차|

- 자동차별 주차 요금

| 차량 번호 | 	누적 주차 시간(분)|	주차 요금(원) |
|---|---|---|
| 0000 | 	34 + 300 = 334|	5000 + ⌈(334 - 180) / 10⌉ x 600 = 14600 |
| 0148 | 	670	|5000 +⌈(670 - 180) / 10⌉x 600 = 34400 |
| 5961 | 	145 + 1 = 146|	5000 |

- 어떤 차량이 입차된 후에 출차된 내역이 없다면, 23:59에 출차된 것으로 간주합니다.
  - 0000번 차량은 18:59에 입차된 이후, 출차된 내역이 없습니다. 따라서, 23:59에 출차된 것으로 간주합니다.
- 00:00부터 23:59까지의 입/출차 내역을 바탕으로 차량별 누적 주차 시간을 계산하여 요금을 일괄로 정산합니다.
- 누적 주차 시간이 기본 시간이하라면, 기본 요금을 청구합니다.
- 누적 주차 시간이 기본 시간을 초과하면, 기본 요금에 더해서, 초과한 시간에 대해서 단위 시간 마다 단위 요금을 청구합니다.
  - 초과한 시간이 단위 시간으로 나누어 떨어지지 않으면, 올림합니다.
  - ⌈a⌉ : a보다 작지 않은 최소의 정수를 의미합니다. 즉, 올림을 의미합니다.

주차 요금을 나타내는 정수 배열 fees, 자동차의 입/출차 내역을 나타내는 문자열 배열 records가 매개변수로 주어집니다. 차량 번호가 작은 자동차부터 청구할 주차 요금을 차례대로 정수 배열에 담아서 return 하도록 solution 함수를 완성해주세요.
### 제한사항
- fees의 길이 = 4
  - fees[0] = 기본 시간(분)
  - 1 ≤ fees[0] ≤ 1,439
  - fees[1] = 기본 요금(원)
  - 0 ≤ fees[1] ≤ 100,000
  - fees[2] = 단위 시간(분)
  - 1 ≤ fees[2] ≤ 1,439
  - fees[3] = 단위 요금(원)
  - 1 ≤ fees[3] ≤ 10,000
- 1 ≤ records의 길이 ≤ 1,000
  - records의 각 원소는 "시각 차량번호 내역" 형식의 문자열입니다.
  - 시각, 차량번호, 내역은 하나의 공백으로 구분되어 있습니다.
  - 시각은 차량이 입차되거나 출차된 시각을 나타내며, HH:MM 형식의 길이 5인 문자열입니다.
    - HH:MM은 00:00부터 23:59까지 주어집니다.
    - 잘못된 시각("25:22", "09:65" 등)은 입력으로 주어지지 않습니다.
  - 차량번호는 자동차를 구분하기 위한, `0'~'9'로 구성된 길이 4인 문자열입니다.
  - 내역은 길이 2 또는 3인 문자열로, IN 또는 OUT입니다. IN은 입차를, OUT은 출차를 의미합니다.
  - records의 원소들은 시각을 기준으로 오름차순으로 정렬되어 주어집니다.
  - records는 하루 동안의 입/출차된 기록만 담고 있으며, 입차된 차량이 다음날 출차되는 경우는 입력으로 주어지지 않습니다.
  - 같은 시각에, 같은 차량번호의 내역이 2번 이상 나타내지 않습니다.
  - 마지막 시각(23:59)에 입차되는 경우는 입력으로 주어지지 않습니다.
  - 아래의 예를 포함하여, 잘못된 입력은 주어지지 않습니다.
    - 주차장에 없는 차량이 출차되는 경우
    - 주차장에 이미 있는 차량(차량번호가 같은 차량)이 다시 입차되는 경우

### 입출력 예

|fees	|records	|result|
|---|---|---|
|[180, 5000, 10, 600]|	["05:34 5961 IN", "06:00 0000 IN", "06:34 0000 OUT", "07:59 5961 OUT", "07:59 0148 IN", "18:59 0000 IN", "19:09 0148 OUT", "22:59 5961 IN", "23:00 5961 OUT"]	|[14600, 34400, 5000]|
|[120, 0, 60, 591]|	["16:00 3961 IN","16:00 0202 IN","18:00 3961 OUT","18:00 0202 OUT","23:58 3961 IN"]	|[0, 591]|
|[1, 461, 1, 10]|	["00:00 1234 IN"]|	[14841]|

### 풀이
매번 자바로 먼저풀고 코틀린으로 다시 풀었는데 이번에는 코틀린을 먼저 풀어봤다. 그래서 설명할 때도 코틀린 쓸예정이다.   
이번 문제는 뭔가 새로웠다. 매일 수학같은 문제만 보다가 현실생활에 쓰일 법한 문제를 봐서 그런지 너무 좋았다.   
문제의 내용과 제한사항이 정~~~~~말 길지만 3줄로 정리하면

>입차와 출차내역이 주어지고   
누적 시간에 따라 요금이 매겨진다.   
입차되고 출차가 안되면 23:59에 자동 출차된다.

진행 단계는 총 3단계이다.
1. IN, OUT으로 누적시간 계산
2. OUT하지 않은 차량을 23:59 기준으로 누적시간 계산
3. 누적시간으로 요금 측정
 
이런 문제에서 가장 쓰기 좋은 자료구조는 Map이라고 생각한다. 필요한 Map은 2개이다.

1. 차량번호와 들어온 시간을 저장할 맵
2. 차량번호와 주차요금을 저장할 맵

1번 맵을 recordMap이라 짓고 2번 맵을 accrueMap로 짓겠다.

```kotlin
//key : 차량번호 , value : 들어온 시간(분)
val recordMap = HashMap<String, Int>()
//keh : 차량번호 , value : 주차요금
val accrueMap = HashMap<String, Int>()
```
그 다음 진행해야할 단계는 출차기록을 바탕으로 차량이 들어온 시간과 나간 시간으로 누적 시간을 계산하는 것이다.
```kotlin
records.forEach {
    val s = it.split(" ")//0 : 시간 | 1 : 차량번호 | 2 : IN, OUT
    val min = s[0].split(":") // 시 : 분 -> 분
    //IN
    if(s[2].equals("IN"))
        recordMap.put(s[1], min[1].toInt() + min[0].toInt()*60)
    //OUT
    else{
        val parkingMin = min[1].toInt() + min[0].toInt()*60 - recordMap.remove(s[1])!! //주차시간(분)
        accrueMap.put(s[1], accrueMap.getOrDefault(s[1], 0) + parkingMin) //주차 누적시간
    }
}
```
차근차근 보면 records.forEach{} 람다를 이용해서 records루프를 실행한다.

```kotlin
val s = it.split(" ")//0 : 시간 | 1 : 차량번호 | 2 : IN, OUT
val min = s[0].split(":") // 시 : 분 -> 분
```
출차기록은 "05:34 5961 IN" 형태로 주어지기 때문에 1차적으로 공백기준으로 끊을 필요가 있다.   
끊은 문자열중 시:분 구조에서 분 구조로 바꿔야 이후 누적시간계산할 때 편하기 때문에 min이라는 List<String>을 만들어준다.   

조건 분기를 걸어 IN 일때는 recordMap의 key : 차량번호, value : 들어온 시간(min) 으로 넣어준다.   
OUT 일 경우 나간시간(분) - 들어온시간(분) 계산 결과를 accrueMap에 차량번호 키값에 주차 누적시간을 맞게 넣어준다.

>recordMap.remove(s[1])!!    

여기서 NotNull키워드를 붙여주는 이유는 remove()를 통한 결과가 Int?로 널이 될 수 있는 타입으로 오기 때문이다.   
그대로 두면 컴파일시 Type misMatch 에러를 뱉으므로 Null이 아님을 증명할 필요가 있다.   
위 예제처럼 !!를 통해 Null이 아님을 명시하거나 Null Check로 Null이 올 가능성을 막으면 된다.

이제 recordMap을 출력시켜보면 23:59분 까지 출차하지 않은 사람만 남을 것이다. 따라서 이 사람들을 계산해주면 누적시간계산 과정은 끝이난다.

```kotlin
//23:59분까지 출차 안한경우
if(!recordMap.isEmpty()){
    recordMap.forEach{
        val parkingMin = 1439 - it.value//주차시간(분)
        accrueMap.put(it.key, accrueMap.getOrDefault(it.key, 0) + parkingMin)
    }
}
```
23:59을 분으로 치환하면 1439분이다. 람다에서 it.value를 빼주면 주차시간이 나오고 accrueMap에 key값에 맞춰서 넣어주면 된다.
여기서 이전 루프에서 짰던 누적시간 계산처럼 짜면 안된다. ~~내가 한 실수~~   

>val parkingMin = min[1].toInt() + min[0].toInt()*60 - recordMap.remove(s[1])!!

이전 루프에선 remove키워드를 사용하고 있다. records루프안에서 remove를 사용해서 문제 없지만 recordMap루프에서 remove를 해버리면 키를 사용하고 있는 동시에 삭제해서 동시성 오류를 뱉어낸다. 따라서 런타임오류가 3번부터 12번까지 시뻘겋게 뜬다.

이제 우리는 고객들에게 요금 정산만 해주면 된다.
```kotlin
//요금정산
accrueMap.toSortedMap().forEach {
    //누적시간이 기본시간이하
    if(it.value < fees[0]){
        answer.add(fees[1])//기본요금
    }
    else{
        answer.add(fees[1] + (ceil((it.value.toDouble()-fees[0]) / fees[2])).toInt()*fees[3]) //기본요금 + ⌈(누적시간-기본요금)/단위시간⌉ * 단위요금
    }
}
return answer.toIntArray()
```
누적 시간이 기본시간 이하면 기본 요금만 주면 된다.
그 외엔 <span style="color:#20C997">기본요금 + ⌈(누적시간-기본요금)/단위시간⌉ * 단위요금]</span>식을 써주면 된다. 여기서 봐야될 것은 ceil()함수이다.   
Math클래스에서 호출되는 함수로 주어진 패러미터의 소수점 첫째자리를 올림해준다. 


마지막으로 전체코드와 자바코드를 올리면서 마무~리 하겠다.
```kotlin
import kotlin.math.ceil

fun main(){
    val fees = intArrayOf(180, 5000, 10, 600)
    val records = arrayOf("05:34 5961 IN", "06:00 0000 IN", "06:34 0000 OUT", "07:59 5961 OUT", "07:59 0148 IN", "18:59 0000 IN", "19:09 0148 OUT", "22:59 5961 IN", "23:00 5961 OUT")
    solution(fees, records).forEach {
        println(it)
    }
}

//fee{기본 시간(분), 기본요금, 단위시간(분), 단위요금}
//요금의 정의 : 기본시간 이하일 경우 기본요금만, 이상 일 경우 단위시간 당 단위요금 +
//해결 records배열을 map에 넣고 차량번호를 key, 시간을 value로 놓고 IN일때 add, out일 때 계산하는 방식으로 갈것
fun solution(fees: IntArray, records: Array<String>): IntArray {
    var answer = ArrayList<Int>() //차량 번호가 작은 자동차부터 청구할 요금
    //key : 차량번호 , value : 들어온 시간(분)
    val recordMap = HashMap<String, Int>()
    //keh : 차량번호 , value : 주차요금
    val accrueMap = HashMap<String, Int>()

    records.forEach {
        val s = it.split(" ")//0 : 시간 | 1 : 차량번호 | 2 : IN, OUT
        val min = s[0].split(":") // 시 : 분 -> 분
        //IN
        if(s[2].equals("IN"))
            recordMap.put(s[1], min[1].toInt() + min[0].toInt()*60)
        //OUT
        else{
            val parkingMin = min[1].toInt() + min[0].toInt()*60 - recordMap.remove(s[1])!! //주차시간(분)
            accrueMap.put(s[1], accrueMap.getOrDefault(s[1], 0) + parkingMin) //주차 누적시간
        }
    }
    //23:59분까지 출차 안한경우
    if(!recordMap.isEmpty()){
        recordMap.forEach{
            val parkingMin = 1439 - it.value//주차시간(분)
            accrueMap.put(it.key, accrueMap.getOrDefault(it.key, 0) + parkingMin)
        }
    }
    //요금정산
    accrueMap.toSortedMap().forEach {
        //누적시간이 기본시간이하
        if(it.value < fees[0]){
            answer.add(fees[1])//기본요금
        }
        else{
            answer.add(fees[1] + (ceil((it.value.toDouble()-fees[0]) / fees[2])).toInt()*fees[3]) //기본요금 + ⌈(누적시간-기본요금)/단위시간⌉ * 단위요금
        }
    }
    return answer.toIntArray()
}
```

### JAVA
자바 꿀팁! map을 키값으로 정렬하고 싶다면 TreeMap을 쓰면 알아서 오름차순으로 정렬된다.
```java
public int[] solution(int[] fees, String[] records) {
        ArrayList<Integer> answer = new ArrayList<>();
        Map<String, Integer> recordMap = new HashMap<>(); //차량번호 : 들어온 시간
        Map<String, Integer> accrueMap = new TreeMap<>(); //차량번호 : 주차요금

        for(String e : records){
            String[] s = e.split(" ");
            String[] min = s[0].split(":");
            //IN
            if(s[2].equals("IN"))
                recordMap.put(s[1], Integer.parseInt(min[1]) + Integer.parseInt(min[0]) * 60);
            //OUT
            else{
                int parkingMin = Integer.parseInt(min[1]) + Integer.parseInt(min[0]) * 60 - recordMap.remove(s[1]);
                accrueMap.put(s[1], accrueMap.getOrDefault(s[1], 0) + parkingMin);
            }
        }

        if(!recordMap.isEmpty()){
            recordMap.forEach((a, b) -> {
                    int parkingMin = 1439 - b;
                    accrueMap.put(a, accrueMap.getOrDefault(a, 0) + parkingMin);
            });
        }

        accrueMap.forEach((a, b) -> {
            if(b < fees[0])
                answer.add(fees[1]);
            else {
                Double c = Math.ceil((Double.parseDouble(b.toString()) - fees[0]) / fees[2]);
                answer.add(fees[1] + c.intValue() * fees[3]);
            }
        });
        return answer.stream().mapToInt(i -> i).toArray();
    }
```
