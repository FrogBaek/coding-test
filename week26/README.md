# week26

## 1

### 문제 - <code>12945_피보나치수</code>



### 알고리즘 설계

```cpp
/* 아이디어:
	피보나치 수를 dp로 풀이한다. 이 때, hash code를 구현했을 때처럼 모듈러 연산을 미리 해줌으로써 overflow를 방지한다
*/
```



### 풀이 코드

```cpp
#define MAX 100'000

using namespace std;

int solution(int n)
{
    int memo[MAX + 1];
    memo[0] = 0;
    memo[1] = 1;
    
    for (int i = 2; i <= n; ++i)
        memo[i] = (memo[i - 2] + memo[i - 1]) % 1234567;
    
    return memo[n];
}
```



### 개인적인 회고와 다른 풀이

피보나치 수를 dp로 풀고, `1234567` 로 나머지 연산을 **그때 그때마다** 해줌으로써 overflow가 나지 않도록 합니다😁

이게 가능한 이유는 hash code를 구현했을 때 봤던 것처럼 나머지 연산의 성질 덕분입니다!

```cpp
(A + B) mod C = (A mod C + B mod C) mod C
```



### 느낀 점

저번 주 dp 복습이 되어서 좋았습니다!!





## 2

### 문제 - <code>42839_소수찾기</code>

### 알고리즘 설계

```cpp
/* 아이디어:
	1. numbers로 만들 수 있는 최대값을 구한다
	2. 1번에서 구한 최대값까지 가능한 소수를 모두 구한다 => 에라토스테네스의 체
	3. numbers로 만들 수 있는 순열을 완전탐색하여 만들 수 있는 소수를 판별한다
*/
```



### 풀이 코드

```cpp
#include <string>
#include <algorithm>
#include <cmath>

#define MAX 10'000'000

using namespace std;

int solution(string numbers)
{
    // 1. numbers로 만들 수 있는 최대값을 구한다
    sort(numbers.begin(), numbers.end(), greater<>());
    
    int maxNumber = stoi(numbers);
    
    // 2. 1번에서 구한 최대값까지 가능한 소수를 모두 구한다 => 에라토스테네스의 체
    int memo[MAX];
    memo[0] = memo[1] = 0;
    
    for (int i = 2; i <= maxNumber; ++i)
        memo[i] = i;
    
    for (int i = 2; i <= sqrt(maxNumber); ++i)
    {
        if (memo[i] == 0) continue;
        
        for (int j = i * i; j <= maxNumber; j += i)
            memo[j] = 0;
    }
    
    // 3. numbers로 만들 수 있는 순열을 완전탐색하여 만들 수 있는 소수를 판별한다
    sort(numbers.begin(), numbers.end());
    
    int cnt = 0;
    do
    {
        for (int len = 1; len <= numbers.length(); ++len)
        {
            int num = stoi(numbers.substr(0, len));
            if (memo[num])
            {
                cnt++;
                memo[num] = 0;
            }
        }
    } while (next_permutation(numbers.begin(), numbers.end()));
    
    return cnt;
}
```



### 개인적인 회고와 다른 풀이

사실 마지막에 `3. 순열을 완전탐색하여 소수 판별하기` 에서 c++에서 제공해주는 순열 알고리즘 덕분에 손쉽게 풀었던 것 같습니다!

처음에 재귀로 구현해보고자 코드를 구상 중이었는데, 마냥 떠오르지 않더라구요😢

문제를 다 풀고 나서 다른 분들의 풀이를 보니 **백트랙킹** 기법으로 재귀로 푸셨더라구요! 다음 주에 백트랙킹을 배우니, 그 때 열심히 배워보겠습니다



### 느낀 점

c++이 순열 알고리즘을 제공해줘서 편하다는 얘길 들었던 것 같은데 이번에 확실히 체감됐습니다🫨






## 3

### 문제 - <code>181187_두원사이의정수쌍</code>

### 알고리즘 설계

```cpp
/* 아이디어:
	r1^2 <= x^2 + y^2 <= r2^2
	이 때, 시간 초과를 막기 위해 하나의 사분면에서 구한 정수 쌍의 개수에 4를 곱해줌으로써 최종 정수 쌍을 구한다
*/
```



### 풀이 코드

```cpp
#include <cmath>

using namespace std;

long long solution(int r1, int r2)
{
    long long cnt = 0;

    for (int x = 1; x <= r2; ++x)
    {
        long long minY = ceil(sqrt((long long)r1 * r1 - (long long)x * x));
        long long maxY = floor(sqrt((long long)r2 * r2 - (long long)x * x));
        
        cnt += (maxY - (minY < 0 ? 0 : minY) + 1);
    }

    return cnt * 4;
}
```



### 개인적인 회고와 다른 풀이

처음에는 식을 세훈 후, 아래와 같이 모든 사분면을 한 번에 탐색했었습니다

```cpp
#include <cmath>

using namespace std;

long long solution(int r1, int r2)
{
    long long cnt = 0;

    for (int x = -r2; x <= r2; ++x)
    {
        for (int y = -r2; y <= r2; ++y)
        {
            long long dist = (long long)x * x + (long long)y * y;
            if ((long long)r1 * r1 <= dist && dist <= (long long)r2 * r2)
                ++cnt;
        }
    }

    return cnt;
}

```

하지만 시간 초과가 나게 되었고, 이를 해결하기 위해 원의 특성인 **대칭성** 을 이용하여 하나의 사분면에서 구한 정수 쌍에 4를 곱해줌으로써 최종 정수 쌍을 구했습니다😀




### 느낀 점

까다롭지만 재밌는 문제였습니다!!
