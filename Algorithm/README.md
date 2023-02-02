# 알고리즘

# 목차
- [목차](#목차)
- [1. 구간 합 구하기](#1-구간-합)
    - [1차원 배열에서의 합배열](#1차원-배열에서의-합배열)
    - [1차원 배열에서의 구간 합](#1차원-배열에서의-구간-합)
- [2. 투 포인터](#2-투-포인터)
  - [연속된 자연수의 합 구하기](#연속된-자연수의-합-구하기)
  - [연속되지 않은 자연수 배열에서 합 조건 찾기](#연속되지-않은-자연수-배열에서-합-조건-찾기)
- [3. 슬라이딩 윈도우](#3-슬라이딩-윈도우)
  - [주어진 문자열에서 조건을 만족하는 부분 문자열 구하기](#주어진-문자열에서-조건을-만족하는-부분-문자열-구하기)
  - [최소값 찾기](#최소값-찾기)
- [4. 스택과 큐](#4-스택과-큐)
  - [스택으로 오름차순 수열 만들기](#스택으로-오름차순-수열-만들기)
  - [오큰수 구하기](#오큰수-구하기)
  - [카드 게임](#카드-게임)
  - [절댓값 힙 구현하기](#절댓값-힙-구현하기)
- [5. 정렬](#5-정렬)
  - [정렬 알고리즘](#정렬-알고리즘)
  - [버블 정렬](#버블-정렬)

# 1. 구간 합
## 1차원 배열에서의 합배열
```Kotlin
// arr: 주어진 배열, sumArr: 합 배열
sumArr[i] = sumArr[i-1] + arr[i]
```
## 1차원 배열에서의 구간 합
```Kotlin
// i번째 수 부터 j번째 수까지의 합
sumArr[j] - sumArr[i-1]
```
예제: 백준 11659([코드(Kotlin)](https://github.com/djawnstj/Algorithm/blob/learned/%EB%B0%B1%EC%A4%80/Silver/11659.%E2%80%85%EA%B5%AC%EA%B0%84%E2%80%85%ED%95%A9%E2%80%85%EA%B5%AC%ED%95%98%EA%B8%B0%E2%80%854/%EA%B5%AC%EA%B0%84%E2%80%85%ED%95%A9%E2%80%85%EA%B5%AC%ED%95%98%EA%B8%B0%E2%80%854.kt))

## 2차원 배열에서의 합배열
```Kotlin
// arr: 주어진 배열, sumArr: 합 배열
sumArr[i][j] = sumArr[i][j-1] - sumArr[i-1][j] - sumArr[i-1][j-1] + arr[i][j]
```
## 2차원 배열에서의 구간 합
```Kotlin
// (startX, startY)번째 수 부터 (endX, endY)번째 수까지의 합
sumArr[endX][endY] - sumArr[startX-1][endY] - sumArr[endX][startY-1] + sumArr[startX-1][startY-1]
```
예제: 백준 11660([코드(Kotlin)](https://github.com/djawnstj/Algorithm/blob/learned/%EB%B0%B1%EC%A4%80/Silver/11660.%E2%80%85%EA%B5%AC%EA%B0%84%E2%80%85%ED%95%A9%E2%80%85%EA%B5%AC%ED%95%98%EA%B8%B0%E2%80%855/%EA%B5%AC%EA%B0%84%E2%80%85%ED%95%A9%E2%80%85%EA%B5%AC%ED%95%98%EA%B8%B0%E2%80%855.kt))

---

# 2. 투 포인터

## 연속된 자연수의 합 구하기
자연수를 입력받고 연속된 자연수의 합으로 해당 자연수를 만들 수 있는 경우의 수를 구하기
```Kotlin
// num: 입력받은 자연수 
// sum: 루프를 돌며 연속된 자연수의 합을 담는 변수(0으로 초기화)
// lt: 왼쪽 포인터로 연속된 자연수를 합하는 한 싸이클이 돌면 1씩 증가(1로 초기화)
// rt: 오른쪽 포인터로 1씩 증가하며 연속된 자연수를 합하는데 사용(1로 초기화)
// result: 연속된 자연수의 합을 구한 횟수(0으로 초기회)
while (lt <= num) {
    sum += rt
    if (sum >= num) {
        if (sum == num) result++
        lt++
        sum = 0
        rt = lt
    }
    else rt++
}

또는

for (lt in 1 .. num) {
    sum = 0
    for (rt in i .. num) {
        sum += rt
        if (sum > num) break
        else if (sum == num) {
            result++
            break
        }
    }
}
```
위 두 식중 위에 식이 시간복잡도, 공간복잡도 측면에서 좋은 결과가 나왔다.

예제: 백준 2018([코드(Kotlin)](https://github.com/djawnstj/Algorithm/blob/learned/%EB%B0%B1%EC%A4%80/Silver/2018.%E2%80%85%EC%88%98%EB%93%A4%EC%9D%98%E2%80%85%ED%95%A9%E2%80%855/%EC%88%98%EB%93%A4%EC%9D%98%E2%80%85%ED%95%A9%E2%80%855.kt))

## 연속되지 않은 자연수 배열에서 합 조건 찾기
숫자를 합하여 찾을 수 M과 불연속 자연수 개수 N, N개만큼 자연수를 입력받은 후 N개의 자연수에서 M을 만들 수 있는 합 조건 개수 구하기
```Kotlin
// M: 합하여 찾을 목표 숫자
// N: 불연속 자연수 개수
// arr: N개만큼 입력받은 수를 담은 배열(오름차순 정렬 필요)
// lt: 왼쪽 포인터(0으로 초기화)
// rt: 오른쪽 포인터(arr.size-1로 초기화)
// result: M을 만들 수 있는 경우 총 합
while (lt < rt) {
    val sum = arr[lt] + arr[rt]
    if (sum < M) lt++
    else if (sum > M) rt--
    else {
        result++
        lt++
        rt--
    }
}
```
예제: 백준 1940([코드(Kotlin)](https://github.com/djawnstj/Algorithm/blob/learned/%EB%B0%B1%EC%A4%80/Silver/1940.%E2%80%85%EC%A3%BC%EB%AA%BD/%EC%A3%BC%EB%AA%BD.kt))

## 중복이 허용된 정수의 배열에서 합 조건 찾기
N개의 정수를 받은 후 해당 배열에서 서로 다른 두 수의 합으로 표현되는 숫자의 개수 구하기
```Kotlin
// N: 중복이 혀용된 정수의 개수
// arr: N개만큼 입력받은 수를 담은 배열(오름차순 정렬 필요)
// lt: 왼쪽 포인터(0으로 초기화)
// rt: 오른쪽 포인터(arr.size-1로 초기화)
// result: M을 만들 수 있는 경우 총 합
for (i in 0 until N) {
    val find = arr[i]
    lt = 0
    rt = N - 1
    while (lt < rt) {
        if (arr[lt] + arr[rt] == find) {
            // 자기 자신과 0이 합해진 경우는 제외해야함
            if (lt != i && rt != i) {
                result++
                break
            } else if (lt == i) lt++
            else if (rt == i) rt--
        } else if (arr[lt] + arr[rt] < find) lt++
        else rt--
    }
}
```
예제: 백준 1253([코드(Kotlin)](https://github.com/djawnstj/Algorithm/blob/learned/%EB%B0%B1%EC%A4%80/Gold/1253.%E2%80%85%EC%A2%8B%EB%8B%A4/%EC%A2%8B%EB%8B%A4.kt))

---

# 3. 슬라이딩 윈도우
슬라이딩 윈도우는 O(n^2)의 시간복잡도를 O(n)으로 줄이기 위해 2중 반복이 아닌 **범위 앞/뒤의 원소를 추가/제거** 하는 방식의 알고리즘

## 주어진 문자열에서 조건을 만족하는 부분 문자열 구하기
길이가 P개인 문자열을 받아 길이가 S개인 연속 부분 문자열들 중에서 알파벳 { A, C, G, T } 의 최소 개수를 만족하는 부분 문자열 개수 구하기
```Kotlin
// checkArr: 유효한 비밀번호인지 체크하는 조건이 담긴 배열
// myArr: 돌고있는 슬라이딩 윈도우의 문자열에서 체크한 필요 알파벳의 개수를 담은 배열
// checkSecret: 돌고있는 슬라이딩 윈도우의 문자열에서 조건을 만족하는 알파벳 개수
// S: DNA 문자열 개수
// P: 암호로 만들 문자열 개수
// A: DNA 알파벳을 담은 문자열
// result: 조건을 만족하는 암호 개수

// 암호 입력받기
for (i in 0 until 4) {
    checkArr[i] = sc.nextInt()
    if (checkArr[i] == 0) checkSecret++
}

// 입력받은 문자열의 앞 P개만큼 먼저 체크
for (i in 0 until P) add(A[i])

// 알파벳 네개가 모두 조건을 만족하면 암호 개수 +1
if (checkSecret == 4) result++

// 2번째 알파벳부터 슬라이딩 윈도우를 돌며 조건 체크
for (i in P until S) { 
    // 이전 슬라이딩 윈도우에서 가장 첫 인덱스를 찾아 제거 
    val j = i - P 
    // 새로운 슬라이딩 윈도우에서 마지막에 해당하는 인텍스 알파벳 추가 
    add(A[i]) 
    // 이전 슬라이딩 윈도우에서 첫번째 인덱스 알파벳 제거 
    remove(A[j]) 
    // 알파벳 네개가 모두 조건을 만족하면 암호 개수 +1 
    if (checkSecret == 4) result++
}

/**
 * 새로 추가된 알파벳이 암호 조건을 만족하는지 확인하는 함수
 * 새로 알파벳이 추가되어 해당 알파벳이 조건을 만족하게 된 경우 +1
 */
private fun add(c: Char) {
    when (c) {
        'A' -> {
            myArr[0]++
            if (myArr[0] == checkArr[0]) checkSecret++
        }
        'C' -> {
            myArr[1]++
            if (myArr[1] == checkArr[1]) checkSecret++
        }
        'G' -> {
            myArr[2]++
            if (myArr[2] == checkArr[2]) checkSecret++
        }
        'T' -> {
            myArr[3]++
            if (myArr[3] == checkArr[3]) checkSecret++
        }
    }
}

/**
 * 지나간 알파벳을 제거하는 함수
 * 기존에 암호 조건을 만족하고 있었다면 암호 조건 만족하는 알파벳 개수에서 -1
 */
private fun remove(c: Char) {
    when (c) {
        'A' -> {
            if (myArr[0] == checkArr[0]) checkSecret--
            myArr[0]--
        }
        'C' -> {
            if (myArr[1] == checkArr[1]) checkSecret--
            myArr[1]--
        }
        'G' -> {
            if (myArr[2] == checkArr[2]) checkSecret--
            myArr[2]--
        }
        'T' -> {
            if (myArr[3] == checkArr[3]) checkSecret--
            myArr[3]--
        }
    }
}
```
예제: 백준 12891([코드(Kotlin)](https://github.com/djawnstj/Algorithm/blob/learned/%EB%B0%B1%EC%A4%80/Silver/12891.%E2%80%85DNA%E2%80%85%EB%B9%84%EB%B0%80%EB%B2%88%ED%98%B8/DNA%E2%80%85%EB%B9%84%EB%B0%80%EB%B2%88%ED%98%B8.kt))

## 최소값 찾기
N개의 주어진 숫자들 중 L개 연속된 숫자 중 최소값 찾기

```Kotlin
// N: 숫자의 개수
// L; 최소값을 찾을 범위
// Node: index와 value를 가진 클래스
// deq: Node를 가지고 있을 배열(ArrayDeque or LinkedList(자료형을Deque로 함))
// Deque - 앞/뒤 양쪽으로 값을 추가하거나 삭제할 수 있음, 
// st: BufferedReader로 읽은 한 줄을 공백을 기준으로 쪼개서 문자열 토큰으로 리턴해주는 StringTokenizer

// N개의 수만큼 받아서 바로 비교함으로써, 시간복잡도를 줄일 수 있음
for (i in 0 until N) {
    val cur = st.nextToken().toInt()

    // 최소값을 구하는 문제이므로, 현재 deque 내에서 입력받은 값보다 큰 값이 존재하면 삭제하면서 정렬 효과를 냄
    while (deq.isNotEmpty() && deq.last.value > cur) deq.removeLast()

    // deque의 최대값으로 최근 입력받은 값 추가
    deq.addLast(Node(i, cur))

    // 가장 첫번째 값이 현재 index로부터 L 이상 멀어졌으면 슬라이딩 윈도우를 벗어난것이므로 가장 첫번째 값 삭제
    if (deq.first.index <= i - L) deq.removeFirst()

    // 가장 첫번째 값이 최소값이므로 첫번째 값을 공백과 함께 문자열에 추가
    bw.write("${deq.first.value} ")
}
```
예제: 백준 11003 - [코드(Java)](https://github.com/djawnstj/Algorithm/blob/learned/%EB%B0%B1%EC%A4%80/Platinum/11003.%E2%80%85%EC%B5%9C%EC%86%9F%EA%B0%92%E2%80%85%EC%B0%BE%EA%B8%B0/%EC%B5%9C%EC%86%9F%EA%B0%92%E2%80%85%EC%B0%BE%EA%B8%B0.java), [코드(Kotlin)](https://github.com/djawnstj/Algorithm/blob/learned/%EB%B0%B1%EC%A4%80/Platinum/11003.%E2%80%85%EC%B5%9C%EC%86%9F%EA%B0%92%E2%80%85%EC%B0%BE%EA%B8%B0/%EC%B5%9C%EC%86%9F%EA%B0%92%E2%80%85%EC%B0%BE%EA%B8%B0.kt)

---

# 4. 스택과 큐
- stack: LIFO(Last In First Out, 선입후출)로 push(), pop(), peek() 등 함수가 있다.
- queue: FIFO(First In First Out, 선입선출)로 add(), poll(), peek() 등 함수가 있다.

## 스택으로 오름차순 수열 만들기
stack을 이용해서 N개의 숫자를 오름차순 수열로 만들 수 있는지 확인

```kotlin
// N: 입력받을 숫자들의 개수
// arr: 입력받은 숫자들을 담을 배열
// stack: 오름차순 숫자를 담을 스택
// temp: stack에 담을 숫자(한번 담을때 +1씩)
for (i in 0 until N) {

    // 수열을 만들어야하는 숫자보다 temp가 같아질때까지 계속 반복(수열을 만들 수랑 같은 수가 stack에 들어갈때까지)
    while (temp < arr[i]) {
        // stack의 마지막 수가 수열을 만들 수보다 큰경우 제거
        if (stack.isNotEmpty() && stack.peek() > arr[i]) {
            answer.append("-").append("\n")
            stack.pop()
        } 
        // stack의 마지막 수가 수열을 만들 수보다 작은경우 temp+1을 stack에 추가
        else if (stack.isNotEmpty() && stack.peek() < arr[i]) {
            answer.append("+").append("\n")
            stack.push(++temp)
        } 
        // stack이 비어있는 경우 temp+1을 stack에 추가
        else if (stack.isEmpty()) {
            answer.append("+").append("\n")
            stack.push(++temp)
        }
    }

    // stack의 마지막 수가 수열을 만들 수와 같으면 stack에서 제거
    if (stack.isNotEmpty() && stack.peek() == arr[i]) {
        answer.append("-").append("\n")
        stack.pop()
    } 
    // stack이 비어있거나, stack의 마지막 수와 수열을 만들 수가 다르면 NO 출력하고 return
    else {
        println("NO")
        return
    }

}
```
예제: 백준 1874([코드(Kotlin)](https://github.com/djawnstj/Algorithm/blob/learned/%EB%B0%B1%EC%A4%80/Silver/1874.%E2%80%85%EC%8A%A4%ED%83%9D%E2%80%85%EC%88%98%EC%97%B4/%EC%8A%A4%ED%83%9D%E2%80%85%EC%88%98%EC%97%B4.kt))

## 오큰수 구하기
크기가 N인 수열이 주어지고, 각 원소의 오른쪽에 있으면서 해당 원소보다 값이 크면서 가장 왼쪽에 있는 값(오큰수)을 구하기

```kotlin
// N: 수열의 크기
// arr: 입력받은 수열을 담은 배열
// answer: 인덱스별로 오큰수를 담을 배열
// stack: 인덱스를 담을 Stack

// 1번 인덱스의 수가 0번 인덱스의 수의 오큰수가 될 수 있는지 확인하면 되기 때문에 1부터 시작
for (i in 1 until N) {
    // 현재(i)번째 수와 i번째 이전의 수들의 크기를 비교하여 i번째 수가 이전 수들의 오큰수가 될 수 있는지 확인하며 시간복잡도에서 이득
    // 스택의 top에 들어있는 인덱스번째 수보다 i 번째 수가 작으면 스택에 들어있는 다른 인덱스번째 수들에도 오큰수가 될 수 없음
    // 스택의 top에 들어있는 인덱스번째 수와 i번째 수를 계속 비교하며 i 번째 수가 더 크다면 스택의 top에 들어있는 인덱스 번째 수의 오큰수는 i번째 수가 됨
    while (stack.isNotEmpty() && arr[stack.peek()] < arr[i]) answer[stack.pop()] = arr[i]
    // 현재(i)번째 수의 오큰수는 다음(i+1)번째 수부터 확인해야하기 때문에 stack의 top부분에 i를 추가
    stack.push(i)
}

// 오큰수를 찾지 못한 인덱스(stack에 남아있는 인덱스)번째는 -1을 넣어줌
while (stack.isNotEmpty()) {
    answer[stack.pop()] = -1
}
```
예제: 백준 17298([코드(kotlin)](https://github.com/djawnstj/Algorithm/blob/learned/%EB%B0%B1%EC%A4%80/Gold/17298.%E2%80%85%EC%98%A4%ED%81%B0%EC%88%98/%EC%98%A4%ED%81%B0%EC%88%98.kt))

## 카드 게임
자연수 N을 입력받고 N이하의 오름차순 자연수 수열에서 가장 앞의 수를 버린 후 다음 수를 마지막 순서로 보내기를 숫자가 하나 남을때까지 반복하면, 마지막으로 남는 수를 구하기
```kotlin
// N: 입력받은 자연수
// queue: 자연수 수열을 담을 Queue(구현체 - LinkedList)
while (queue.size > 1) {
    queue.poll()
    queue.add(queue.poll())
}
```
예제: 백준 2164([코드(kotlin)](https://github.com/djawnstj/Algorithm/blob/learned/%EB%B0%B1%EC%A4%80/Silver/2164.%E2%80%85%EC%B9%B4%EB%93%9C2/%EC%B9%B4%EB%93%9C2.kt))

## 절댓값 힙 구현하기
수열에서 가장 작은 절대값을 리턴하는 절댓값 힙을 구현하기
```kotlin
// N: 입력받은 수열의 크기

// 절대값 크기별 오름차순으로 정렬시킬 우선순위 Queue (우선순위를 지정해줘야함)
val queue: Queue<Int> = PriorityQueue { o1, o2 ->
    val f = Math.abs(o1) // 비교할 값의 절대값 구해주기
    val s = Math.abs(o2) // 비교할 값의 절대값 구해주기
    // 절대값이 같으면 음수먼저, 다르면 절대값을 기준으로 정렬
    return@PriorityQueue if (f == s && o1 > o2) 1 else if (f == s && o1 <= o2) -1 else f-s
}
for (i in 0 until N) {
    val x = br.readLine().toInt()
    if (x == 0) {
        if (queue.isEmpty()) println(0) else println(queue.poll().toString())
    } else queue.add(x)
}
```
예제: 백준 11286([코드(kotlin)](https://github.com/djawnstj/Algorithm/blob/learned/%EB%B0%B1%EC%A4%80/Silver/11286.%E2%80%85%EC%A0%88%EB%8C%93%EA%B0%92%E2%80%85%ED%9E%99/%EC%A0%88%EB%8C%93%EA%B0%92%E2%80%85%ED%9E%99.kt))

---

# 5. 정렬

## 정렬 알고리즘
- [버블 정렬](#버블-정렬) - 데이터의 인접 요소끼리 비교하고, swap 연산을 수행하며 정렬하는 방식
- [선택 정렬](#) - 대상에서 가장 크거나 작은 데이터를 찾아가 선택을 반복하면서 저열하는 방식
- [삽입 정렬](#) - 대상을 선택해 정렬된 영역에서 선택 데이터의 적절한 위치를 찾아 삽입하면서 정렬하는 방식
- [퀵 정렬](#) - pivot 값을 선정해 해당 값을 기준으로 정렬하는 방식
- [병합 정렬](#) - 이미 정렬된 부분 집합들을 효율적으로 병합해 전체를 정렬하는 방식
- [기수 정렬](#) - 데이터의 자릿수를 바탕으로 비교해 데이터를 정렬하는 방식

## 버블 정렬
인접한 두 데이터의 크기를 비교해 정렬하는 방법으로,
간단하게 구현 가능하지만 시간복잡도는 O(n^2)으로 다른 정렬 알고리즘보다 속도가 느린편

### 수 정렬하기 1
N개의 수가 주어졌을 때 이를 오름차순 정렬하는 프로그램을 작성하시오
```kotlin
// N: 수열의 크기
// arr: 입력받은 수열
for (i in 0 until N-1) {
    // 정렬할 때 오른쪽으로 보낼 수를 먼저 정렬시키기 때문에 -i 를 해주면 됨.
    for (j in 0 until N-1-i) {
        // 현재 인덱스 수보다 배열의 오른쪽 값이 크면 두 수 바꿔주기
        if (arr[j] > arr[j+1]) {
            val temp = arr[j]
            arr[j] = arr[j+1]
            arr[j+1] = temp
        }
    }
}
```
예제: 백준 2750([코드(kotlin)](https://github.com/djawnstj/Algorithm/blob/learned/%EB%B0%B1%EC%A4%80/Bronze/2750.%E2%80%85%EC%88%98%E2%80%85%EC%A0%95%EB%A0%AC%ED%95%98%EA%B8%B0/%EC%88%98%E2%80%85%EC%A0%95%EB%A0%AC%ED%95%98%EA%B8%B0.kt))


---