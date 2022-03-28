---
layout: post 
title: 2022 KAKAO blind recruitment 신고결과받기
subtitle: 프로그래머스
categories: 코딩문제[프로그래머스]
tags: [코딩테스트, 자바, 코틀린]
---
### 문제설명
신입사원 무지는 게시판 불량 이용자를 신고하고 처리 결과를 메일로 발송하는 시스템을 개발하려 합니다. 무지가 개발하려는 시스템은 다음과 같습니다.

- 각 유저는 한 번에 한 명의 유저를 신고할 수 있습니다.
  - 신고 횟수에 제한은 없습니다. 서로 다른 유저를 계속해서 신고할 수 있습니다.
  - 한 유저를 여러 번 신고할 수도 있지만, 동일한 유저에 대한 신고 횟수는 1회로 처리됩니다.
- k번 이상 신고된 유저는 게시판 이용이 정지되며, 해당 유 저를 신고한 모든 유저에게 정지 사실을 메일로 발송합니다.
  - 유저가 신고한 모든 내용을 취합하여 마지막에 한꺼번에 게시판 이용 정지를 시키면서 정지 메일을 발송합니다.
다음은 전체 유저 목록이 ["muzi", "frodo", "apeach", "neo"]이고, k = 2(즉, 2번 이상 신고당하면 이용 정지)인 경우의 예시입니다.

|유저 ID	|유저가 신고한 ID|	설명|
|---|---|---|
|"muzi"|	"frodo"|	"muzi"가 "frodo"를 신고했습니다.|
|"apeach"|	"frodo"|	"apeach"가 "frodo"를 신고했습니다.|
|"frodo"|	"neo"	|"frodo"가 "neo"를 신고했습니다.|
|"muzi"	|"neo"	|"muzi"가 "neo"를 신고했습니다.|
|"apeach"|	"muzi"|	"apeach"가 "muzi"를 신고했습니다.|

각 유저별로 신고당한 횟수는 다음과 같습니다.

|유저 ID|신고당한 횟수|
|---|---|
|"muzi"|	1|
|"frodo"|	2|
|"apeach"|	0|
|"neo"	|2|

위 예시에서는 2번 이상 신고당한 "frodo"와 "neo"의 게시판 이용이 정지됩니다. 이때, 각 유저별로 신고한 아이디와 정지된 아이디를 정리하면 다음과 같습니다.

|유저 ID	|유저가 신고한 ID|	정지된 ID|
|---|---|---|
|"muzi"|	["frodo", "neo"]|	["frodo", "neo"]|
|"frodo"|	["neo"]	|["neo"]|
|"apeach"|	["muzi", "frodo"]|	["frodo"]|
|"neo"	|없음	|없음|

따라서 "muzi"는 처리 결과 메일을 2회, "frodo"와 "apeach"는 각각 처리 결과 메일을 1회 받게 됩니다.

이용자의 ID가 담긴 문자열 배열 id_list, 각 이용자가 신고한 이용자의 ID 정보가 담긴 문자열 배열 report, 정지 기준이 되는 신고 횟수 k가 매개변수로 주어질 때, 각 유저별로 처리 결과 메일을 받은 횟수를 배열에 담아 return 하도록 solution 함수를 완성해주세요.

### 제한사항

- 2 ≤ id_list의 길이 ≤ 1,000
  - 1 ≤ id_list의 원소 길이 ≤ 10
  - id_list의 원소는 이용자의 id를 나타내는 문자열이며 알파벳 소문자로만 이루어져 있습니다.
  - id_list에는 같은 아이디가 중복해서 들어있지 않습니다.
- 1 ≤ report의 길이 ≤ 200,000
  - 3 ≤ report의 원소 길이 ≤ 21
  - report의 원소는 "이용자id 신고한id"형태의 문자열입니다.
  - 예를 들어 "muzi frodo"의 경우 "muzi"가 "frodo"를 신고했다는 의미입니다.
  - id는 알파벳 소문자로만 이루어져 있습니다.
  - 이용자id와 신고한id는 공백(스페이스)하나로 구분되어 있습니다.
  - 자기 자신을 신고하는 경우는 없습니다.
- 1 ≤ k ≤ 200, k는 자연수입니다.
- return 하는 배열은 id_list에 담긴 id 순서대로 각 유저가 받은 결과 메일 수를 담으면 됩니다.

### 입출력 예 
|id_list| 	report                                                                         | 	k  | 	result |
|---|---------------------------------------------------------------------------------|-----|---------|
|["muzi", "frodo", "apeach", "neo"]| 	["muzi frodo","apeach frodo","frodo neo","muzi neo","apeach muzi"]	| 2   | 	[2,1,1,0] |
|["con", "ryan"]	| ["ryan con", "ryan con", "ryan con", "ryan con"]	| 3   | 	[0,0]  |

### 입출력 예 설명

입출력 예 #1

문제의 예시와 같습니다.

입출력 예 #2

"ryan"이 "con"을 4번 신고했으나, 주어진 조건에 따라 한 유저가 같은 유저를 여러 번 신고한 경우는 신고 횟수 1회로 처리합니다. 따라서 "con"은 1회 신고당했습니다. 3번 이상 신고당한 이용자는 없으며, "con"과 "ryan"은 결과 메일을 받지 않습니다. 따라서 [0, 0]을 return 합니다.

### 풀이

반복으로 땡치려고 하면 작동은 되지만 루프가 많아서 테스트를 통과못한다. ~~그리고 코드가 너무 길어져..~~
지금 문제에서 요구되는 것은 1. 중복제거 2. 이름과 신고리스트를 저장   
이럴 때 우린 아주 좋은걸 배웠다. Set, Map

기본적인 단계는 세가지다.
1. 신고목록 reportMap과 메시지 받을사람 countMap 부터 만들어 준다.
2. 신고목록만큼 반복해서 신고목록map에 넣어주는데, 중복 배재해야하므로 Set으로 넣어준다.
3. id_list만큼 반복해서 신고횟수가 k회 이상이고 신고를 당한적 있는 사람만 countMap에 숫자를 증가시켜준다.

이걸 코드로 짜면 아래처럼 나온다.
```java
import java.util.HashMap;
import java.util.LinkedHashMap;
import java.util.HashSet;
import java.util.Map;

class Solution {
    public int[] solution(String[] id_list, String[] report, int k) {
        String[] s = {};    //나눌 문자를 저장할 배열
        //1. 신고목록 reportMap과 메시지 받을사람 countMap 부터 만들어 준다.
        Map<String, HashSet<String>> reportList = new HashMap<>(); //피 신고자, 신고자
        Map<String, Integer> countMap = new LinkedHashMap<>(); //순서대로 나오는 맵
        int[] answer = new int[id_list.length]; //각 유저별 처리결과 메시지를 받은 횟수

        for(String id : id_list)
            countMap.put(id, 0);
        
        //2. 신고목록만큼 반복해서 신고목록map에 넣어주는데, 중복 배재해야하므로 Set으로 넣어준다.
        for(int i = 0; i< report.length; i++){
            s = report[i].split(" ");//신고자, 피신고자
            HashSet<String> a = reportList.getOrDefault(s[1], new HashSet<>()); //신고한놈 집합
            a.add(s[0]);    //신고한놈 추가
            reportList.put(s[1], a); //피신고자, 신고자
        }
        //3. id_list만큼 반복해서 신고횟수가 k회 이상이고 신고를 당한적 있는 사람만 countMap에 숫자를 증가시켜준다.
        for (String id : id_list){
          //신고 된적 있고 신고 누적횟수가 k회 이상인 피신고자
            if(reportList.containsKey(id) && reportList.get(id).size() >= k) {
                for (String s1 : reportList.get(id)) {
                    //신고한 사람들
                    countMap.put(s1, countMap.getOrDefault(s1, 0)+1);
                }
            }
        }
        answer = countMap.values().stream().mapToInt(Integer::intValue).toArray();
        return answer;
    }
}
```

코틀린 코드로도 만들 수 있다.
```kotlin
class Solution {
    fun solution(id_list: Array<String>, report: Array<String>, k: Int): IntArray {
        var splitString : List<String>
    val reportList = HashMap<String, HashSet<String>>() //피 신고자, 신고자
    val countMap = LinkedHashMap<String, Int>() //신고한 사람이 받은 문자 갯수
    var answer = IntArray(id_list.size) //각 유저별 처리결과 메시지를 받은 횟수


    for (i in id_list)
        countMap.put(i, 0)

    for(s in report){
        splitString = s.split(" ")
        var a = reportList.getOrDefault(splitString[1], HashSet())
        a.add(splitString[0])
        reportList.put(splitString[1], a)
    }
    //신고 누적횟수가 k회 이상인 피신고자
    for (id in id_list) {
        //신고 횟수초과 필터
        if (reportList.containsKey(id) && reportList[id]!!.size >= k) {
            for (s1 in reportList[id]!!) {
                //신고한 사람들
                countMap[s1] = countMap.getOrDefault(s1, 0) + 1
            }
        }
    }
    answer = countMap.values.toIntArray()
    return answer
    }
}```
