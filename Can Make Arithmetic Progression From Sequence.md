# Can Make Arithmetic Progression From Sequence
題目解析:
判斷一個序列能否形成等差數列，可以透過排序後檢查相邻元素的差值是否相同來完成。
   
法一:先排序後驗證
```
class Solution {
public:
    bool canMakeArithmeticProgression(vector<int>& arr) {
        //先排序(nlogn)
        sort(arr.begin(), arr.end());
        //公差
        int diff = arr[1] - arr[0];
        for(int i = 0; i < arr.size()-1; i++){
            if((arr[i] + diff) != arr[i+1]){
                return false;
            }
        }
        return true;
    }
};
```
法二: Using Unordered Map 找到最大值最小值 根據關係式判斷
學習unorder_map和auto

-------
If the initial term of an arithmetic progression is a and the common difference of successive members is d, then the n-th term of the sequence is given by:
an = a + (n-1) d
Let the last term of AP be maxi and the first term is mini.
So, maxi = mini + (n-1)d
Hence, d = (maxi - mini ) / (n-1)

-----

在不排序的情況下，找到最大和最小值，並算出它們之間的差值，然後檢查序列中是否存在差值數量的整數倍

解題思路：
1. 找出最大值和最小值，並用一個unordered_map將所有數存下來。
1. 判斷最大值和最小值的差值是否能被數列長度-1整除，若不能，則不是等差數列。
1. 計算出等差數列的差值，並判斷每一個數是否存在，若不存在則不是等差數列。
```
class Solution {
public:
    bool canMakeArithmeticProgression(vector<int>& arr) {
        unordered_map <int, int> hash;
        int n = arr.size();
        //找最大最小值
        int minV = INT_MAX;
        int maxV = INT_MIN;
        //掃過一次arr 計算最大和最小值 將arr壓入hash
        for(int i = 0; i < n; i++){
            minV = min(arr[i], minV);
            maxV = max(arr[i], maxV);
            hash[arr[i]]++;         //紀錄每個值在hash出現過幾次，如果用=1會沒有這個效果
        }
        //計算公差 如果不能整除就回傳不是
        int diff;
        if((maxV - minV) % (n-1) != 0){
            return false;
        }else{
            diff = (maxV - minV) / (n-1);
        }
        //檢查hash內有無值 minV, minV+diff, ...,maxV
        while(n--){
            if(! hash[minV]){
                return false;
            }
            minV += diff;
        }

        return true;

    }
};
```


範例：

假設有一個向量arr = [3, 5, 1, 2]，我們要判斷它是否可以形成等差數列。

我們可以使用unordered_map<int, int> mp來存儲每一個數字的出現次數，代碼如下：

unordered_map<int, int> mp;
for(auto &it : arr){
mp[it]++;
}

那麼hash表中的值將會是：
mp = {
3: 1,
5: 1,
1: 1,
2: 1
}

我們可以使用mp[minV]來判斷minV是否在向量中存在，如果mp[minV]為0，則代表minV在向量中不存在，將返回false。



###### tags: `hash`