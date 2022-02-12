## 605. Can Place Flowers(Easy)
```cpp
 bool canPlaceFlowers(vector<int>& flowerbed, int n) {
        int ans = 0;
        for(int i =0;i<flowerbed.size() ;i++){
           if(flowerbed[i] == 0 && 
            (i + 1 == flowerbed.size() || flowerbed[i+1] == 0) 
             && (i-1 < 0 || flowerbed[i-1] == 0))
           flowerbed[i]=1, ans ++;
           //判断左右边界和判断左右是否合法的写法可以学习
        }
        return ans >= n;
    }
```

## 452 Minimum Number of Arrows to Burst Balloons (Medium)
```cpp
 int findMinArrowShots(vector<vector<int>>& points) {
        sort(points.begin(), points.end(),[](vector<int> &a, vector<int> &b){
            return a[1] < b[1];
        });
        int ans = 1;
        int prev = points[0][1];
        for(int i =1;i<points.size();i++){
            if(prev < points[i][0]){ //这里是不同点
                ans += 1;
                prev = points[i][1];
            }
        }
        return ans;
    }
```

