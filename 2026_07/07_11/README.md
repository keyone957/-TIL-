# 07/11

## 잔디 심기

[Rotate Array - LeetCode](https://leetcode.com/problems/rotate-array/)

```cpp
class Solution {
public:
    void rotate(vector<int>& nums, int k) {
        deque<int> dq;
        dq.assign(nums.begin(),nums.end());

        for(int i=0;i<k;i++)
        {
            int back=dq.back();
            dq.pop_back();
            dq.push_front(back);
        }

        nums.assign(dq.begin(),dq.end());
    }
};
```

<img title="" src="image1" alt="image1" width="810" height="487">

→ 추가 메모리를 사용한 문제풀이



```cpp
class Solution {
public:
    void rotate(vector<int>& nums, int k) {
        k=k%nums.size();
        int left =0;
        int right=nums.size()-1;
        while(left<right)
        {
            swap(nums[left], nums[right]);
            left++;
            right--;
        }
        int left2=0;
        int right2=k-1;
        while(left2<right2)
        {
           swap(nums[left2], nums[right2]);
            left2++;
            right2--;
        }
        int left3=k;
        int right3=nums.size()-1;
        while(left3<right3)
        {
           swap(nums[left3], nums[right3]);
            left3++;
            right3--;
        }
    }
 };
```

<img title="" src="image2" alt="image2" width="811" height="478"> 

→ 투 포인터와 배열 뒤집기를 이용한 최적



## 블로그 정리

[LeetCode 121 Rotate Array](https://keyone957.tistory.com/29)
