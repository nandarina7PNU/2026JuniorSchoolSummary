
# 알고리즘 : 효율성(Efficiency), 분석(Analysis), 차수(Order)

## 문제(Problem)란 무엇인가?

문제(Problem) : 우리가 답을 찾고자 하는 질문(question)

예시
- 숫자가 나열되어 있는 리스트를 크기 순서대로 재 배열 : 정렬(sort)
- n개의 원소를 가진 리스트 S에 숫자 x가 있는지 확인
- 25번째 피보나치 수 구하기 : F(25) = 75,025

## 알고리즘(Algorithm)이란 무엇인가?

주어진 입력으로부터 출력을 만들어내기 위해, 대상 컴퓨터에서 효과적으로 작동하도록 소프트웨어 개발자가 소프트웨어로 작성한 논리의 실제 구현체(instance)

알고리즘의 중요한 요소(factor)
- 정확성(Correctness)
- 복잡도(Complexity)
- 최적성(Optimality)
- 명확성과 효율성(Clarity and Efficiency)

### 순차 탐색

Problem : n개의 원소를 가진 배열 S에 key x는 존재하는가?  
Inputs : 정수 n개, 1부터 n까지 배열의 위치에 key값이 대응됨(index)  
Outputs : 위치, 만약 그 값이 없다면 0을 반환.

C 구현

```C
#include <stdio.h>

void seqsearch(int n, const int S[], int x, int* location){
    *location = 0;
    while(*location <= n && S[*location]!=x) (*location)++;
    if(*location > n) *location = 0;
}

int main()
{
    int n, f, loc;
    int arr[100];
    scanf("%d", &n);
    for(int i=0;i<n;++i) scanf("%d", &arr[i]);
    scanf("%d", &f);
    seqsearch(n, arr, f, &loc);
    printf("%d", loc);
    return 0;
}

// 입력
/*
5
1 2 3 4 5
3
*/
// 출력
/*
2
*/
```

C++ 구현
```C++
#include <iostream>
using namespace std;

void seqsearch(int n, const int S[], int x, int& location){
    location = 0;
    while(location <= n && S[location]!=x) location++;
    if(location > n) location = 0;
}

int main()
{
    int n, f, loc;
    int arr[100];
    cin >> n;
    for(int i=0;i<n;++i) cin >> arr[i];
    cin >> f;
    seqsearch(n, arr, f, loc);
    cout << loc;
    return 0;
}
```
### 행렬 곱셈

2차원 for문으로 r1 x c2 행렬의 모든 원소를 순회  
이 때, 배열 C의 (i, j) 위치는 A배열의 (i, k) 원소 x B배열의 (k, j)의 합으로 나타냄.
```C
#include <stdio.h>

void matrixmult(int r1, int c1, int c2, int A[][101], int B[][101], int C[][101])
{
    int i, j, k;
    for(i=0; i<r1; i++){
        for(j=0; j<c2; j++){
            C[i][j] = 0;
            for(k=0; k<c1; k++){
                C[i][j] += A[i][k] * B[k][j];
            }
        }
    }
}

int main()
{
    int mat_a[101][101], mat_b[101][101], mat_c[101][101];
    int a_n, a_m;
    scanf("%d %d", &a_n, &a_m);
    
    for(int i=0; i<a_n; i++){
        for(int j=0; j<a_m; j++){
            scanf("%d", &mat_a[i][j]);
        }
    }

    int b_n, b_m;
    scanf("%d %d", &b_n, &b_m);
    
    for(int i=0; i<b_n; i++){
        for(int j=0; j<b_m; j++){
            scanf("%d", &mat_b[i][j]);
        }
    }

    matrixmult(a_n, a_m, b_m, mat_a, mat_b, mat_c);

    for(int i=0; i<a_n; i++){
        for(int j=0; j<b_m; j++){
            printf("%d ", mat_c[i][j]);
        }
        printf("\n");
    }

    return 0;
}
```

참고 : 백준 2740 행렬 곱셈
https://www.acmicpc.net/problem/2740
### 배열 탐색

#### 순차 탐색(Sequential search)
- 최악의 경우에는 n번의 비교가 필요함 (정렬된 n개의 원소를 가진 리스트가 주어졌을 때)

#### 이진 탐색(Binary search)
- 재귀
- 임의의 k에 대하여 n = 2^k라고 하자. 그러면 비교 횟수는 k=log n이다.

[1] 이진 탐색 시각화
![[Pasted image 20260403224422.png|216]]

```C

```

참고 : 백준 1920 수 찾기
https://www.acmicpc.net/problem/1920

### 피보나치



#### 재귀(Recursive)

```C
```

#### 반복(Iterative)

```C
```

## 알고리즘 분석(Analysis)

### 시간 복잡도(Time complexity)

### 



# 참고자료

[1] 이진탐색 / 나무위키
https://namu.wiki/w/%EC%9D%B4%EC%A7%84%20%ED%83%90%EC%83%89