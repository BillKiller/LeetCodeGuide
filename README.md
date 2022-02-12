# Range Problem 
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

## Partition Labels (Medium)
```cpp

   vector<int> partitionLabels(string s) {
        map<char,int> endIndex;
        vector<int>result;
        s = s + " ";
        for(int i=s.length()  -1; i >= 0; i--){
             if(endIndex.count(s[i]) == 0) endIndex[s[i]] = i;
        }
        int prev = endIndex[s[0]];
        int cnt = 1;
        for(int i =1; i<s.length(); i++){
            if(prev < i){
                result.push_back(cnt);
                cnt = 0;
            }
            cnt ++;
            prev = max(endIndex[s[i]], prev);
        }
        return result;
    }
```
##Best Time to Buy and Sell Stock II (Easy)
只要在低点买，高点卖就好了，贪心下去一定是最优的。
```cpp
   int maxProfit(vector<int>& prices) {
         //
         int ans = 0;
         int WAITTING_STATE = 0, HODLING_STATE = 1;
         int state = 0;
         int buy_price = 0;
         for(int i = 0;i<prices.size();i++){
            if(state == WAITTING_STATE){
                if(i + 1 == prices.size() || prices[i] < prices[i+1]){
                    buy_price = prices[i];
                    state = HODLING_STATE;
                }
            }else{
                if(i  + 1== prices.size() || prices[i] > prices[i + 1]){
                    ans += (prices[i] - buy_price);
                    state = WAITTING_STATE;
                }
            }
         }
         return ans;
    }

```


## 
```cpp
string minWindow(string s, string t) {
       vector<int>chars(128, 0);
       vector<bool>flags(128, false);
       for(int i =0; i<t.length(); i++){
           flags[t[i]] = true;
           chars[t[i]]++;
       }
       int l=0, min_l = 0, min_size = s.size() + 1;
       int cnt = 0;
       for(int r = 0; r<s.length(); r++){
           if(flags[s[r]]){
               //push
                if( -- chars[s[r]] >= 0){
                   cnt++;
                }
           }
           while(cnt == t.length()){
               if(r - l + 1 < min_size){
                   min_l = l;
                   min_size = r - l + 1;
               }
               if(flags[s[l]] && ++chars[s[l]] > 0){
                  cnt--;
               }
               l++;
           }
       }
       return min_size > s.size()? "" : s.substr(min_l, min_size);
    }
```