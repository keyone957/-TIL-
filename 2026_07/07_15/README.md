# 07/15

## 잔디 심기

[Sliding Window Maximum - LeetCode](https://leetcode.com/problems/sliding-window-maximum/description/)

### 단조 큐

[Sliding Window Maximum - LeetCode](https://leetcode.com/problems/sliding-window-maximum/)

```cpp
class Solution {
public:
    vector<int> maxSlidingWindow(vector<int>& nums, int k) {
        int arrSize=nums.size();
        vector<int> answer;
        priority_queue<pair<int,int>> window;
        int left=0;
        for(int i=0;i<k;i++)
        {
            window.push({nums[i],i});
        }

        answer.push_back(window.top().first);
        left++;   


        for(int i=1;i<arrSize-k+1;i++)
        {
            window.push({nums[i+k-1],i+k-1});
            // 다음 인덱스의 값을 먼저 push
            while(!window.empty()&&window.top().second<left)
            {
                window.pop();
                //윈도우 크기의 범위를 벗어난 값을 pop시킴
            }
            answer.push_back(window.top().first);

            left++;
        }
        return answer;
    }
};
```

이 문제를 푸는데 나는 우선순위 큐 + 슬라이딩 윈도우를 사용해서 문제를 풀었음

→ 우선순위 큐에 값을 삽입하거나 제거할 때 마다 힙을 다시 다 정리 하는 과정때문에 

시간 복잡도 ⇒ `$O(nlogn)$`


근데 태그를 보니 단조 큐가 있길래 단조 큐에 대해서 한번 찾아봄

<aside>

### 단조 큐

> 내부 원소들이 단조 증가 OR 감소 상태를 유지하는 자료구조  / 보통 덱 사용(양끝에서 원소 제거 가능하니)

윈도우를 벗어난 원소를 앞에서 제거

필요 없어진 작은 원소를 뒤에서 제거.

⇒ 주로 이런 슬라이딩 윈도우처럼 일정 범위 내의 유효한 값만 저장하는 그런 문제에 사용한다 함 (유효 하지 않으면 pop해버림 (문제에서는 현재 보고 있는 값보다 작은 값이겠죠) 

#### 모노톤 큐를 이용한 구간의 최댓값 구하는 방법

큐 or 덱에 pair로 <값, 값의 인덱스>를 관리함. 

→ 윈도우 안의 모든 값을 저장하는 것이 아닌 앞으로 최댓값이 될 가능성이 있는 값만 저장

ex)

기존 덱

8 6 3 

들어올 값 5

3은 5보다 작으므로 제거, 이후 5를 추가.

1. 현재 윈도우를 벗어난 원소 제거
2. 현재 들어오는 값보다 작거나 같은 원소는 후보에서 제거
3. 필요 없는 원소들을 모두 제거한 후 현재 들어오는 값을 덱의 뒤에 추가
4. 덱의 맨 앞 값을 정답 배열에 저장 

→ 위 단계를 윈도우의 단계마다 다시 진행함

예시)

1 삽입: [1]

3 삽입:
1은 3보다 작으므로 제거
[3]

5 삽입:
3은 5보다 작으므로 제거
[5]

현재 값보다 작으면서 더 먼저 들어온 값은 앞으로 최댓값이 될 수 없으므로 즉시 제거한다.

따라서 단조 큐에는 최댓값 후보만 남는다.

</aside>

```cpp
class Solution {
public:
    vector<int> maxSlidingWindow(vector<int>& nums, int k) {
        vector<int> answer;

        // {값, 인덱스}
        deque<pair<int, int>> dq;

        for (int i = 0; i < nums.size(); i++) {
            int left = i - k + 1;

            // 1. 현재 윈도우의 범위를 벗어난 원소 제거
            while (!dq.empty() && dq.front().second < left) {
                dq.pop_front();
            }

            // 2. 현재 값보다 작거나 같은 값 제거
            while (!dq.empty() && dq.back().first <= nums[i]) {
                dq.pop_back();
            }

            // 3. 현재 값과 인덱스를 덱에 추가
            dq.push_back({nums[i], i});

            // 4. 크기가 k인 윈도우가 완성되면 최댓값 저장
            if (i >= k - 1) {
                answer.push_back(dq.front().first);
            }
        }

        return answer;
    }
};
```

### 단조 큐와 내가 사용했던 우선순위 큐 방법의 차이점

#### 우선순위 큐

<img width="840" height="383" alt="Image" src="https://github.com/user-attachments/assets/81b8a016-3e66-4da4-b255-3514b5c7a9b6" />

→ 들어온 값들을 저장하고 가장 큰 값이 윈도우를 벗어났을 때 제거함. 범위를 벗어난 값도 내부에 남을 수 있음 ⇒ 값을 모두 저장

#### 단조 큐

<img width="857" height="436" alt="Image" src="https://github.com/user-attachments/assets/b819dada-aea9-494e-89c4-52772a1a32cb" />

→ 현재 윈도우 안에서 앞으로 최댓값이 될 가능성이 있는 값만 감소하는 순서로 저장함. 절대 최댓값 후보가 될 수 없는 값은 바로 제거함 ⇒ 최댓값 후보만 보관





## 블로그 정리

[LeetCode 239. Sliding Window Maximum](https://keyone957.tistory.com/31)



## 프로젝트

- 페리또 정기 회의 

- 각 페이즈 구조 설계 및 OpenFieldSystem.cs 구성  


