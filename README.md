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

## 763. Partition Labels (Medium)
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
## Best Time to Buy and Sell Stock II (Easy)
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


## 最小覆盖子串
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


## 680. 验证回文字符串
```cpp
bool isVaild(string s){
    int l = 0;
    int r = s.length() -1;
    while(l < r){
        if(s[l] != s[r]) return false;
        l++;
        r--;
    }
    return true;
}
bool validPalindrome(string s) {
    int l =0;
    int r = s.length() -1;
    while( l < r && s[l] == s[r]){
            l++;
            r--;
    }
    return l >= r || isVaild(s.substr(l+1, (r - l))) || isVaild(s.substr(l, (r-l)));
}
```


## 524 Longest Word in Dictionary through Deleting (Medium)
```cpp
判断这种是不是相等可以通过双指针来判断，如果target串的指针在末尾证明是符合的。（可以思考一下它普通字符串匹配的区别）
  bool isMatch(string &a, string &b){
        int l=0,r=0;
        while(l < a.length() &&  r < b.length()){
            if(a[l] == b[r]){
                r++;
            }
            l++;
        }
        return r == b.length();
    }
    string findLongestWord(string s, vector<string>& dictionary) {
        sort(dictionary.begin(), dictionary.end(), [](string &a, string &b){
            if(a.length() != b.length()) return a.length() > b.length();
            else return a < b;
        });
        for(auto ss:dictionary){
            if(isMatch(s, ss)){
                return ss;
            }
        }
        return "";
    }
```


## 二分搜索
根据while循环条件不同分两种情况:
1.while(low < high)
此时的循环不包含low=high,所以每次循环的查找区间为[low, high),左闭右开，则high指向的位
置是不被包含在查找区间的，所以此时high的初始值 取length,循环里，high=mid。
2.while(low<=high)
此时的循环包含low=high,查找区间为[low, high]，左闭右闭，则high指向的位置被包含在查找区
间中，所以此时的high初始值取length-1,循环里，high=mid-1。

```cpp
 int mySqrt(int x) {
        if(x == 0) return 0;
        int l =1, r=x;
        while(l <= r){
            int mid = l + (r-l)/2;
            if(mid == (x / mid)){
                return mid;
            }else if(mid > (x / mid)){
                r = mid - 1;
            }else{
                l = mid + 1;
            }
        }
        return r;
    }
```

## 34. 在排序数组中查找元素的第一个和最后一个位置
```cpp
  vector<int> searchRange(vector<int>& nums, int target) {
        if(nums.empty()) return vector<int>{-1, -1};
        int startIndex = lower_bound(nums, target);
        int endIndex = upper_bound(nums, target) - 1;
        if(startIndex == nums.size() || nums[startIndex] != target)
            return vector<int>{-1, -1};
        else
            return vector<int>{startIndex, endIndex};
    }
    int lower_bound(vector<int>&nums, int target){
        int l = 0, r = nums.size();
        while(l < r){
            int mid = l + (r - l) /2;
            if(nums[mid] >= target){
                r = mid;
            }else{
                l = mid + 1;
            }
        }
        return l;
    }

     int upper_bound(vector<int>&nums, int target){
        int l = 0, r = nums.size();
        while(l < r){
            int mid = l + (r - l) /2;
            if(nums[mid] <= target){
                l = mid + 1; //把答案排除
            }else{
                r = mid;
            }
        }
        return l;
    }

```