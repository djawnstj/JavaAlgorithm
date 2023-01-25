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

# 1. 구간 합
## 1차원 배열에서의 합배열
```kotlin
// arr: 주어진 배열, sumArr: 합 배열
sumArr[i] = sumArr[i-1] + arr[i]
```
## 1차원 배열에서의 구간 합
```kotlin
// i번째 수 부터 j번째 수까지의 합
sumArr[j] - sumArr[i-1]
```
예제: 백준 11659([코드(kotlin)](https://github.com/djawnstj/Algorithm/blob/learned/%EB%B0%B1%EC%A4%80/Silver/11659.%E2%80%85%EA%B5%AC%EA%B0%84%E2%80%85%ED%95%A9%E2%80%85%EA%B5%AC%ED%95%98%EA%B8%B0%E2%80%854/%EA%B5%AC%EA%B0%84%E2%80%85%ED%95%A9%E2%80%85%EA%B5%AC%ED%95%98%EA%B8%B0%E2%80%854.kt))

## 2차원 배열에서의 합배열
```kotlin
// arr: 주어진 배열, sumArr: 합 배열
sumArr[i][j] = sumArr[i][j-1] - sumArr[i-1][j] - sumArr[i-1][j-1] + arr[i][j]
```
## 2차원 배열에서의 구간 합
```kotlin
// (startX, startY)번째 수 부터 (endX, endY)번째 수까지의 합
sumArr[endX][endY] - sumArr[startX-1][endY] - sumArr[endX][startY-1] + sumArr[startX-1][startY-1]
```
예제: 백준 11660([코드(kotlin)](https://github.com/djawnstj/Algorithm/blob/learned/%EB%B0%B1%EC%A4%80/Silver/11660.%E2%80%85%EA%B5%AC%EA%B0%84%E2%80%85%ED%95%A9%E2%80%85%EA%B5%AC%ED%95%98%EA%B8%B0%E2%80%855/%EA%B5%AC%EA%B0%84%E2%80%85%ED%95%A9%E2%80%85%EA%B5%AC%ED%95%98%EA%B8%B0%E2%80%855.kt))

---

# 2. 투 포인터

## 연속된 자연수의 합 구하기
자연수를 입력받고 연속된 자연수의 합으로 해당 자연수를 만들 수 있는 경우의 수를 구하기
```kotlin
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

예제: 백준 2018([코드(kotlin)](https://github.com/djawnstj/Algorithm/blob/learned/%EB%B0%B1%EC%A4%80/Silver/2018.%E2%80%85%EC%88%98%EB%93%A4%EC%9D%98%E2%80%85%ED%95%A9%E2%80%855/%EC%88%98%EB%93%A4%EC%9D%98%E2%80%85%ED%95%A9%E2%80%855.kt))

## 연속되지 않은 자연수 배열에서 합 조건 찾기
숫자를 합하여 찾을 수 M과 불연속 자연수 개수 N, N개만큼 자연수를 입력받은 후 N개의 자연수에서 M을 만들 수 있는 합 조건 개수 구하기
```kotlin
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
예제: 백준 1940([코드(kotlin)](https://github.com/djawnstj/Algorithm/blob/learned/%EB%B0%B1%EC%A4%80/Silver/1940.%E2%80%85%EC%A3%BC%EB%AA%BD/%EC%A3%BC%EB%AA%BD.kt))

## 중복이 허용된 정수의 배열에서 합 조건 찾기
N개의 정수를 받은 후 해당 배열에서 서로 다른 두 수의 합으로 표현되는 숫자의 개수 구하기
```kotlin
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
예제: 백준 1253([코드(kotlin)](https://github.com/djawnstj/Algorithm/blob/learned/%EB%B0%B1%EC%A4%80/Gold/1253.%E2%80%85%EC%A2%8B%EB%8B%A4/%EC%A2%8B%EB%8B%A4.kt))

# 3. 슬라이딩 윈도우
슬라이딩 윈도우는 O(n^2)의 시간복잡도를 O(n)으로 줄이기 위해 2중 반복이 아닌 **범위 앞/뒤의 원소를 추가/제거** 하는 방식의 알고리즘

## 주어진 문자열에서 조건을 만족하는 부분 문자열 구하기
길이가 P개인 문자열을 받아 길이가 S개인 연속 부분 문자열들 중에서 알파벳 { A, C, G, T } 의 최소 개수를 만족하는 부분 문자열 개수 구하기
```kotlin
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
예제: 백준 12891([코드(kotlin)](https://github.com/djawnstj/Algorithm/blob/learned/%EB%B0%B1%EC%A4%80/Silver/12891.%E2%80%85DNA%E2%80%85%EB%B9%84%EB%B0%80%EB%B2%88%ED%98%B8/DNA%E2%80%85%EB%B9%84%EB%B0%80%EB%B2%88%ED%98%B8.kt))

---

