# 알고리즘

# 목차
- [목차](#목차)
- [1. 구간 합 구하기](#1-구간-합)
    - [1차원 배열에서의 합배열](#1차원-배열에서의-합배열)
    - [1차원 배열에서의 구간 합](#1차원-배열에서의-구간-합)
- [2. 투 포인터](#2-투-포인터)
  - [연속된 자연수의 합 구하기](#연속된-자연수의-합-구하기)
  - [연속되지 않은 자연수 배열에서 합 조건 찾기](#연속되지-않은-자연수-배열에서-합-조건-찾기)

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



---

