
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
#include <stdio.h>
#include <stdlib.h>

int binarySearch(int arr[], int val, int first, int last)
{
    if(first > last) return 0;
    int mid = (first+last)/2;
    if(val==arr[mid]) return 1;
    else if(val < arr[mid]) return binarySearch(arr, val, first, mid-1);
    else return binarySearch(arr, val, mid+1, last);
}

int compare(const void* a, const void* b){
    if((*(int*)a > *(int*)b)) return 1;
    else return -1;
}

int main()
{
    int n, zero=0;
    int arr[100001] = {0};
    scanf("%d", &n);
    for(int i=0;i<n;++i) scanf("%d", &arr[i]);
    qsort(arr, n, sizeof(int), compare);
    
    int m;
    scanf("%d", &m);
    for(int i=0;i<m;++i){
        int k;
        scanf("%d",&k);
        if(k==0 && zero==1) printf("1\n");
        else printf("%d\n", binarySearch(arr, k, 0, n-1));
    }
    return 0;
}
```

참고 : 백준 1920 수 찾기
https://www.acmicpc.net/problem/1920

### 피보나치 수열

피보나치 수열이란? 
- F(0) = 0, F(1) = 1, F(n) = F(n-1) + F(n-2)를 만족하는 수열
- 0 1 1 2 3 5 8 11 13 21 34 55 89 ...

#### 재귀(Recursive) 구현

```C
#include <stdio.h>

int fib(int n){
    if(n<=1) return n; 
    else return fib(n-1) + fib(n-2); 
}

int main(){
    int n;
    scanf("%d", &n);
    printf("%d", fib(n));
    return 0;
}
```

참고 : 백준 2747 피보나치 수 1
https://www.acmicpc.net/problem/2747

그러나 이 재귀 알고리즘은 아래 그림의 모든 노드를 지난다.  
이 것이 효율적인가? -> 아니요 시간초과 나요
![[Pasted image 20260404021555.png]]
#### 반복(Iterative) 구현

배열을 왼쪽부터 오른쪽으로 채우면 아주 효율적이다!  
F(n) = F(n-1) + F(n-2);
![[Pasted image 20260404022447.png]]

```C
#include <stdio.h>

long long f[91]
long long fib2(int n)
{
    int i;
    f[0] = 0;
    if(n>0){
        f[1] = 1;
        for(i=2;i<=n;++i){
            f[i] = f[i-1] + f[i-2];
        }
    }
    return f[n];
}

int main(){
    int n;
    scanf("%d", &n);
    printf("%lld", fib2(n));
    return 0;
}
```

참고 : 백준 2748 피보나치 수 2
https://www.acmicpc.net/problem/2748
## 알고리즘 분석(Analysis)

시간 복잡도(Time complexity)
- 알고리즘이 입력 데이터 크기 n의 함수로서 수행하는 작업의 양, 즉 n의 함수  
효율성(Efficiency)
- 만약 동일한 시간 복잡도를 가진 다른 두 알고리즘을 같은 데이터 셋에 대해 실행시킨다면 실행 시간은 아마도 다를 것이다. (실제 컴퓨터에서 돌려보면 걸리는 시간은 다를 수 있다!)
- 더 빠른 알고리즘이 다른 것보다 더 효율적이다.

### 시간 복잡도(Time complexity)

#### 예시 1
```C
function1(A[], n){
    k = n/2;
    return A[k];
}
```
n에 상관없이 시간 복잡도는 상수항인 O(1)이다.
#### 예시 2
```C
function2(A[], n){
    sum = 0;
    for(int i=0;i<n;++i){
        sum = sum + A[i];
    }
    return sum;
}
```
알고리즘의 시간은 n에 비례한다. 따라서 시간 복잡도는 O(n)이다.
#### 예시 3
```C

function3(A[], n){
    sum = 0;
    for(int i=0;i<n;++i){
        for(int j=0;j<n;++j){
            sum = sum + A[i]*A[j];
        }
    }
    return sum;
}

```
알고리즘의 시간은 n^2에 비례한다. 따라서 시간 복잡도는 O(n^2)이다.

복잡도의 종류
- 최악의 경우 (수업의 핵심 주제)
- 최고의 경우
- 평균의 경우

### 평균의 경우 시간복잡도

순차 탐색(Sequential search)
- n개의 항목을 가진 배열이 있고 순차 탐색을 통해 값 x를 찾는다고 하자.
- 또한 값 x가 동일한 확률로 어떤 위치에서든 있을 수 있다고 가정하자. (즉, 1/n)

순차 탐색의 평균 시간 복잡도 (기대값)
![[Pasted image 20260405200734.png]]

1번 위치에 있으면 1회 비교  
2번 위치에 있으면 2회 비교  
...  
n번 위치에 있으면 n회 비교  

따라서 각 경우의 확률과 횟수를 곱하여 더함  
![[Pasted image 20260405211957.png]]

### n의 차수에 따른 시간 복잡도 분류

n을 데이터 셋의 크기라고 하자.
- 상수 시간 O(1) : (1, 3, 9, 232, ...)
- 선형 O(n) : (n, 2n, 3n-2, 21n+100, ...)
- 제곱 O(n^2) : (n^2, 2n^2-3, 4n^2-3n+23, ...)
- O(n^3) : (n^3, 4n^3+3n^2-2n+7, ...)

3차 다항식의 시간복잡도 분류는 Ɵ(n^3)로 나타내진다.  
Ɵ(n^3)는 함수의 집합을 나타낸다.  
시간복잡도에 대한 일반적인 표기법 : Ɵ(log n), Ɵ(2^n), Ɵ(n), Ɵ(n!), Ɵ(n log n), Ɵ(n^2), ...

### 몇 가지 일반적인 시간복잡도 함수의 성장률

n이 증가함에 따라 그래프 사이의 간격이 확실하게 차이난다.
![[Pasted image 20260405213719.png]]

만약 알고리즘의 시간복잡도가 Ɵ(n^2)이고, 데이터 셋이 두배가 된다면 실행 시간에 무슨 변화가 일어날까?  
- n개의 크기에 대해서, 데이터 셋의 크기가 2n이라고 하자.
- Ɵ((2n)^2) = Ɵ(4n^2) = 4Ɵ(n^2) -> 시간이 4배로 증가한다!
- 그렇다면, 2^n은 얼마나 증가할까? 즉, 시간복잡도에서 n의 차수가 클수록 데이터 크기에 비례하여 시간 또한 훨신 더 빠르게 증가한다.
## 점근 표기법

### 빅오(Big O) 표기법

정의
- 주어진 시간복잡도 함수 f(n)에 대하여, O(f(n))은 어떤 함수 g(n)의 집합이다.
- g(n)은 어떤 양의 실수 c와 음이 아닌 정수 N이 존재하여, N보다 크거나 같은 모든 n에 대하여 아래 식을 만족한다.
![[Pasted image 20260405235148.png]]
즉, 충분히 큰 n에 대해서는 g(n)은 f(n)의 c배 이하이다. = f(n)이 g(n)의 상한이다.
#### 예시 1

##### 5n^2 +3n - 6 ∈ O(n^2)임을 보여라.
- 우리는 다음 부등식을 만족할 수 있는 c와 N을 찾아야 한다.
![[Pasted image 20260406000419.png]]
- 어떤 값이 c가 될 수 있을까? 우리는 그냥 풀 수도 있고 추측해 볼 수도 있다. 
- c의 값을 6으로 해보자!
![[Pasted image 20260406000816.png]]
![[Pasted image 20260406000840.png]]
따라서 c=6, N=1로 잡으면 모든 n>=1에 대하여 부등식이 성립하므로 5n^2 +3n - 6 ∈ O(n^2)는 증명된다.

```C++
#include <iostream>

using namespace std;

int main()
{
    int a1, a0;
    cin >> a1 >> a0;
    int c, n0;
    cin >> c >> n0;
    
    if(a1==0){
        if(c*n0 >= a0){
            cout << 1;
        }else{
            cout << 0;
        }
    }else{
        if(a1 > 0){
            if(a1<c){
                if(c*n0 >= a1*n0 + a0){
                    cout << 1;
                }else{
                    cout << 0;
                }
            }else if(a1>c){
                cout << 0;
            }else{
                if(a0 > 0){
                    cout << 0;
                }else{
                    cout << 1;
                }
            }
        }else{
            cout << 1;
        }
    }
    return 0;
}
```

참고 : 백준 24313 점근적 표기 1
https://www.acmicpc.net/problem/24313
##### 5n^2 +3n - 6 ∈ Ω(n^2) 임을 보여라
- 위와 마찬가지로 다음 부등식을 만족할 수 있는 c와 N을 찾아야 한다.
![[Pasted image 20260406001709.png]]
- 어떤 값이 c가 될 수 있을까? 빅오메가는 빅오와는 반대로 하한을 찾아야 한다.
- 간단히 c의 값을 5로 해보자.
![[Pasted image 20260406003322.png]]
- 따라서 c=5, N=2로 잡으면 모든 n>=2에 대하여 부등식이 성립하므로 5n^2 +3n - 6 ∈ Ω(n^2)는 증명된다.

5n^2 +3n - 6 ∈ Ω(n^2)와 5n^2 +3n - 6 ∈ Ω(n^2)를 만족하므로 5n^2 +3n - 6 ∈ Θ(n^2)를 만족한다.
#### 빅오의 또 다른 증명 방법
- 만약 모든 n에 대하여 f(n) > g(n)이어도, f ∈ O(g)일 수 있다.
- f는 g의 빅오에 속한다. (f ∈ O(g))

보조 정리(Lemma):
![[Pasted image 20260406004253.png]]

예시 1
![[Pasted image 20260406004341.png]]
![[Pasted image 20260406004432.png]]
- 따라서 g ∈ O(f)이고, f ∉ O(g)이다.

예시 2
![[Pasted image 20260406004622.png]]
- 따라서 g ∈ O(f)이고, f ∉ O(g)이다.
- 로피탈 정리에 의해 미분해줌
### 빅오, 빅오메가, 빅세타
2차 함수에 대해 시간복잡도 분류 시각화
![[Pasted image 20260406002701.png]]
![[Pasted image 20260406005312.png]]
- 모든 2차 함수는 Θ(n^2)에 포함된다. n^2보다 느리거나 같은 함수는 O(n^2)에 포함되며, n^2보다 빠르거나 같은 함수는 Ω(n^2)에 포함된다.
![[Pasted image 20260406005226.png]]
- 그림으로 나타내면 다음과 같다.
![[Pasted image 20260406005240.png]]
- n이 충분히 커지면 n^2+10n은 2n^2아래에 머물게 된다. 즉, 차수가 높은 식이 전체를 지배하게 된다.
### 로그
#### 로그의 정의
![[Pasted image 20260406011325.png]] ![[Pasted image 20260406011548.png|130]]
- 밑이 b인 x에 대한 로그 y는 $y = log_b (x)$와 같이 나타낸다.
#### 로그 법칙
$$
\log_b(xy) = \log_b x + \log_b y
$$
$$
\log_b\left(\frac{x}{y}\right) = \log_b x - \log_b y
$$
$$
\log_b\left(\frac{x}{y}\right) = \log_b x - \log_b y
$$
$$  
\log_b a = \frac{\log_c a}{\log_c b}  
$$
$$  
\log_b\left(\frac{1}{a}\right) = -\log_b a  
$$
$$  
\log_b a = \frac{1}{\log_a b}  
$$
$$
\log_b a = \frac{1}{\log_a b}
$$
### 정리 : log(n!) ∈ Θ(n log n)

#### 하한 증명 : n log n ∈ Ω(n log n)
![[Pasted image 20260406012947.png]]
- n, n-1, n-2, ... 은 n/2보다 크기 때문에 log(n/2) x n/2로 대체할 수 있다.
#### 상한 증명 : n log n ∈ O(n log n)
![[Pasted image 20260406013152.png]]
- n-1, n-2, ... 1은 n보다 작으므로 다음과 같은 부등식이 만족한다.
따라서 n log n ∈ Ω(n^2)이고 n log n ∈ O(n^2)이므로 최종적으로 n log n ∈ Θ(n log n)이다.
# 참고자료

[1] 이진탐색 / 나무위키
https://namu.wiki/w/%EC%9D%B4%EC%A7%84%20%ED%83%90%EC%83%89