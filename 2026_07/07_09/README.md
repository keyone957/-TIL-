# 07/09

### 잔디 심기

https://leetcode.com/problems/max-area-of-island/description/

- 코드
    
    ```cpp
    class Solution {
    int dx[4]={1,-1,0,0};
    int dy[4]={0,0,1,-1};
    public:
        int maxAreaOfIsland(vector<vector<int>>& grid) {
            int y=grid.size();
            int x=grid[0].size();
            int answer=0;
    
            for(int i=0;i<y;i++)
            {
                for(int j=0;j<x;j++)
                {
                    if(grid[i][j]==1)
                    {
                        queue<pair<int,int>> q;
                        q.push({i,j});
                        grid[i][j]=0;
    
                        int cnt=0;
    
                        while(!q.empty())
                        {
                            pair<int,int> cur=q.front();
                            q.pop();
                            cnt++;
    
                            for(int dir=0;dir<4;dir++)
                            {
                                int nextY=cur.first+dy[dir];
                                int nextX=cur.second+dx[dir];
    
                                if(nextY<0||nextX<0||nextY>=y||nextX>=x) continue;
                                if(grid[nextY][nextX]==0) continue;
    
                                grid[nextY][nextX]=0;
                                q.push({nextY,nextX});
                            }
                        }
    
                        answer=max(answer,cnt);
                    }
                }
            }
    
            return answer;
        }
    };
    ```