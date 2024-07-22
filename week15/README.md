# week15

## 1

### 문제 - <code>161989_덧칠하기</code>



### 알고리즘 설계

```cpp
/* 아이디어
    개념: 거리 계산
    로직:
        마지막으로 페인트칠 한 부분을 기억하여 이를 기준으로 칠해야 하는 부분을 칠한다.
*/
```



### 풀이 코드

```cpp
#include <string>
#include <vector>

using namespace std;

int solution(int n, int m, vector<int> section)
{
    int count = 0;
    int lastPaintedEnd = 0;
    for (int i : section)
    {
        if (i <= lastPaintedEnd) continue;
        
        count++;
        lastPaintedEnd = i + m - 1;
    }
    
    return count;
}
```

### 개인적인 회고와 다른 풀이

아이디어만으로 깔끔하게 구현 가능한 (제가 개인적으로 좋아하는) 문제였던 것 같습니다! 거리 계산이라는 아이디어가 떠올라서, 마지막으로 페인트칠 한 위치를 기억할 변수를 토대로 알고리즘을 구현했습니다😁😁


### 느낀 점
폰켓몬에 이어서 이렇게 아이디어만으로 풀 수 있는 문제가 가장 깔끔하고 풀기 편해서 좋은 것 같아요!!

## 2

### 문제 - <code>42889_실패율</code>

### 알고리즘 설계

```cpp
/* flow
    1. 스테이지를 통과하지 못한 플레이어의 수를 센다.
    2. 이를 토대로 실패율을 산정한다.
    3. 실패율을 토대로 내림차순 정렬을 한다.
    4. 실패율이 0이라면 오름차순으로 0인 스테이지들만 정렬한다.
    5. 정렬이 모두 끝난 배열을 반환한다.
*/
```



### 풀이 코드

```cpp
#include <string>
#include <vector>
#include <algorithm>
#include <utility>

using namespace std;

vector<int> solution(int N, vector<int> stages)
{
    vector<int> ans(N);
    vector<pair<unsigned, float>> failureRates(N);
    for (int i = 0; i < N; ++i)
    {
        int cnt = count(stages.begin(), stages.end(), i + 1);
        
        failureRates[i].first = i + 1;
        failureRates[i].second = cnt == 0 ? 0 : float(cnt) / count_if(stages.begin(), stages.end(),
                                        [&i] (const int stage) { return stage >= i + 1; });
    }
    
    sort(failureRates.begin(), failureRates.end(),
        [] (const pair<unsigned, float>& r1, const pair<unsigned, float>& r2)
         {
        	if (r1.second == r2.second)
        		return r1.first < r2.first;
        		
            return r1.second > r2.second;
         });
    
    for (int i = 0; i < N; ++i)
        ans[i] = failureRates[i].first;
    
    return ans;
}
```

### 개인적인 회고와 다른 풀이

사실 처음에는 샘플 테스트 케이스는 모두 통과했지만 제출했을 때 정답률이 100%가 되질 않아 무엇이 잘못됐는지 고민을 했었습니다. 근데 생각해보니 마지막 스테이지(N)를 아예 모든 플레이어들이 도달하지도 못한 경우도 있겠더라구요! 다시 말해 **분모로 계산되는 값이 0인 경우**가 존재하므로, 이런 경우를 방지하고자 삼항연산자를 통해 애초에 cnt가 0인 경우에는 실패율 계산을 하지 않고 바로 0을 할당하도록 코드를 수정했습니다🙃🙃

### 느낀 점

이번에 스터디원분들처럼 함수 지향적으로 프로그래밍 해보고자 람다함수를 사용해봤는데, 너무 편하고 좋은 것 같습니다!! 앞으로도 애용할 것 같아요 ㅎㅎ




## 3

### 문제 - <code>160586_대충만든자판</code>

### 알고리즘 설계

```cpp
/* flow
    1. keymap들을 순회하며 target 문자열의 문자들을 입력가능한 지 확인한다.
    1-1. 이 때, 하나라도 입력할 수 없는 문자가 발견되면 해당 target 문자열은 넘어가며 -1을 담아둔다.
    1-2. 그게 아니라면 keymap들을 모두 순회하며 버튼을 누르는 최소 횟수를 담아둔다.
    2. 순회가 모두 끝나면 최소 입력값들을 담은 배열을 반환한다.
*/
```



### 풀이 코드

```cpp
#include <string>
#include <vector>
#include <climits>

#define NOT_FOUND -1

using namespace std;

vector<int> solution(vector<string> keymap, vector<string> targets)
{
    vector<int> minTyping;
    for (string t : targets)
    {
        int count = 0;
        bool found = true;
        for (char c : t)
        {
            int minIndex = INT_MAX;
            for (string k : keymap)
            {
                int currentIndex = k.find(c);
                
                if (currentIndex == string::npos) continue;
                
                minIndex = min(minIndex, currentIndex);
            }
            
            if (minIndex == INT_MAX)
            {
                minTyping.push_back(NOT_FOUND);
                found = false;
                break;
            }
                
            count += (minIndex + 1);
        }
        
        if (found) minTyping.push_back(count);
    }
    
    return minTyping;
}
```

### 개인적인 회고와 다른 풀이

개인적으로 애먹었던 문제였습니다. 모든 로직을 다 구현하고, 테스트를 해봐도 계속 실패가 떠서, 무엇이 문제인지 찾느라 고생을 했습니다😭😭 알고보니 minIndex에 초기값으로 0을 할당했었는데, 이렇게 되면 min(minIndex, currentIndex) 코드에서 논리대로 제대로 동작하지 않는 경우가 발생하므로 minIndex의 초기값으로는 해당 자료형의 최대값인 INT_MAX를 할당해주었습니다! 그래서 결국 작성한 논리 흐름대로 잘 작동했습니다🥲


### 느낀 점

이 문제를 풀면서 최대값과 최소값을 구할 때, 초기값으로 무엇을 할당하는 지에 대해 복습을 하게 됐습니다🫨
