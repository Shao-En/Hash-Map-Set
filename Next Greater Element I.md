# Next Greater Element I
題目解析:
給定一個數組 nums1 和另一個數組 nums2，求出在 nums1 中每個元素的下一個比它大的數字。

https://www.delftstack.com/zh-tw/howto/cpp/find-in-vector-in-cpp/
注意find()和begin()回傳的是甚麼
使用 STL 庫中的 std::find 演算法。
此函式將迭代器返回到滿足條件的第一個元素
如果未找到任何元素，***則演算法將返回範圍的最後一個元素***
***請注意，返回的迭代器應按 begin 迭代器遞減以計算位置。***
```C++=
class Solution {
public:
    vector<int> nextGreaterElement(vector<int>& nums1, vector<int>& nums2) {
        vector<int> res;
        //找到nums1的值在nums2的哪裡
        for(int i = 0; i < nums1.size(); i++){
            int index = 0;
            for(int j = 0; j < nums2.size(); j++){
                if(nums1[i] == nums2[j]){
                    index = j;
                    //找到nums1的值在nums2的哪
                    break;
                }
            }
        //找nums1的值在nums2中右邊大的值
        int index2 = -1;
            for(int k = index; k < nums2.size(); k++){
                if(nums2[k] > nums1[i]){
                    index2 = k;
                    //找到nums1的值在nums2中右邊第一個大的值
                    break;
            }
        }
        if(index2 != -1){
            res.push_back(nums2[index2]);
        }else{
            res.push_back(index2);
        }
        }
        return res;
    }
};
```
以下是find和begin互動的範例

```
#include <iostream>
#include <vector>
#include <algorithm>
#include <unordered_map>
#include <numeric>
using namespace std;
int main()
{
	vector<int> nums1({ 4,1,2 });
	vector<int> nums2({ 1,3,4, 2 });
	int index;
	auto a = find(nums2.begin(), nums2.end(), nums1[0]);
	index = a - nums2.begin();
	cout << index;
}
```
印出 2

法二:
直接用stack去掃nums2 有出現右邊第一個大的值就做成hash
stack完成後拿nums1的元素去查表即可
用stack和hash
https://home.gamer.com.tw/artwork.php?sn=

```c++=
class Solution {
public:
    vector<int> nextGreaterElement(vector<int>& nums1, vector<int>& nums2) {
        vector <int> ans;
        if(!nums1.size()){           //如果nums1為空
            return {};
        }
        stack <int> st;
        unordered_map <int, int> hash;
        //先用stack遍歷nums2
        for(int i = 0; i < nums2.size(); i++){
            while(!st.empty() && st.top() < nums2[i]){
                //如果stack不為空 且 新的n值比stack.top還大的話 代表找到右邊第一個比stack.top大的值
                hash[st.top()] = nums2[i];
                st.pop();
            }
            //如果新的n值不是右邊第一個比較大的值就放入
            st.push(nums2[i]);
        }
        //遍歷nums1的全部元素
        for(int i = 0; i < nums1.size(); i++){
            if(hash.find(nums1[i]) != hash.end()){  //如果find回傳的不是end就代表有
                ans.push_back(hash[nums1[i]]);
            }else{
                ans.push_back(-1);
            }
        }
        return ans;    
    }
};
```   
以上程式碼使用 stack 和 unordered_map 主要是為了解決如下問題：

對於給定的 nums2 數組中的每個元素，要找到其在 nums2 中右邊第一個比它大的數，如果沒有這樣的數則返回 -1。

具體來說，程序從右往左遍歷 nums2 中的元素，**並使用堆疊 stack 暫存還未找到右邊第一個比它大的元素的元素**。當遍歷到第 i 個元素時，先從堆疊中找到所有比 nums2[i] 小的元素，這些元素在 nums2 中的位置都在 i 右側，且其右邊第一個比它大的元素即為 nums2[i]。程序將這些元素從堆疊中彈出，**同時在哈希表 unordered_map 中記錄下每個已彈出的元素及其右邊第一個比它大的元素**，以便之後在 nums1 中查詢。

最後，程序遍歷 nums1 中的元素，查找每個元素在哈希表中對應的右邊第一個比它大的元素，如果有則添加到結果向量 greater 中，否則添加 -1。
因此，以上程式碼需要使用 stack 和 unordered_map 來解決如上問題。stack 用於暫存還未找到右邊第一個比它大的元素的元素，unordered_map 用於記錄已彈出的元素及其右邊第一個比它大的元素，以便之後在 nums1 中查詢。


解題思路:
nums1 = [4, 1, 2]
nums2 = [1, 3, 4, 2]
尋找nums1元素在nums2中右邊第一個比他大的值
先用stack掃過nums2, 一直持續判斷n和stack.top的關係
如果n大於stack.top則將stack.top pop出來並且映射到hash (代表stack.top這個值有比他大的值)
```c++=
for (int i = 0; i != nums2.size(); ++i) {
   while (!decrease.empty() && decrease.top() < nums2[i])
        nextGreats[decrease.top()] = nums2[i], decrease.pop();
   decrease.push(nums2[i]);
        }
```
利用上面求得的映射關係，我們掃過 nums1 陣列。每遇到一個數字就去找它有沒有對應的 n 值。如果有，則 n 值就是所求；如果沒有則代表它下一個較大，所以輸出 -1 。
```
for (int i = 0; i != nums1.size(); ++i)
   greater.push_back(nextGreats.find(nums1[i]) != nextGreats.end() ? nextGreats[nums1[i]] : -1);
return greater;
    }
```
在hash中找到nums的元素
如果找不到就會 find == end 回傳-1
如果找到就會成立不等於 回傳hash中存的那個第一個大的值
```
nextGreats.find(nums1[i]) != nextGreats.end() ?  nextGreats[nums1[i]] : -1)
```
C++中的unordered_map的end()函数返回一个迭代器，指向容器中的超出范围的位置。这意味着，当您遍历unordered_map容器时，使用end()函数返回的迭代器来判断是否已经遍历到了容器的末尾。

使用end()函数返回的迭代器不能解引用，因为它指向的是超出容器范围的位置。如果您尝试对end()函数返回的迭代器进行解引用，将会导致未定义的行为。因此，您应该仅将end()函数返回的迭代器用于判断是否已经到达容器的末尾。


練習

```c++=
#include <iostream>
#include <vector>
#include <algorithm>
#include <unordered_map>
#include <numeric>
#include <stack>
using namespace std;
int main()
{
	vector<int> nums1({ 4,1,2 });
	vector<int> nums2({ 1,3,4, 2 });
	stack <int> st;
	unordered_map <int, int> hash;
	vector <int> ans({});
	if (!nums1.size()) return {};

	for (int i = 0; i < nums2.size(); i++) {
		while (!st.empty() && st.top() < nums2[i]) {
			hash[st.top()] = nums2[i];
			st.pop();
		}
		st.push(nums2[i]);
	}
	while (!st.empty())
	{
		//打印栈顶元素
		cout << st.top() << " ";
		//弹出栈顶元素
		st.pop();
	}
	for (int i = 0; i < nums1.size(); i++) {
		if (hash.find(nums1[i]) != hash.end()) {
			ans.push_back(hash[nums1[i]]);
		}
		else {
			ans.push_back(-1);
		}
	}
	for (int i = 0; i < ans.size(); i++) {
		cout << endl;
		cout << ans[i] << endl;
	}
	for (auto const& pair : hash)
	{
		//打印键和对应的值
		cout << pair.first << " : " << pair.second << endl;
	}
	return 0;
}
```


```c++=
class Solution {
public:
    vector<int> nextGreaterElement(vector<int>& nums1, vector<int>& nums2) {
        stack <int> st;
        unordered_map <int, int> hash;
        vector <int> ans;
        //用st和hash掃過nums2
        for(int i = 0; i < nums2.size(); i++){
            while(!st.empty() && st.top() < nums2[i]){
                //如果stack非空和下一個元素大於top，代表有右邊大的值
                hash[st.top()] = nums2[i];
                //紀錄進hash
                st.pop();
                //stack pop掉top值之後stack還有值會繼續while有沒有右邊比較大的值
            }
            //壓入stack中
            st.push(nums2[i]);
        }
        //掃過nums1檢查有無值
        for(int i = 0; i < nums1.size(); i++){
            if(hash.find(nums1[i]) != hash.end()){
                //如果有值
                ans.push_back(hash[nums1[i]]);
            }else{
                ans.push_back(-1);
            }
        }
        return ans;
    }
};
```

用for解決
```c++=
class Solution {
public:
    vector<int> nextGreaterElement(vector<int>& nums1, vector<int>& nums2) {
        //找到下一個較大的值
        //給兩個從零開始的array nums1和nums2,nums1是nums2的subarray
        //找到nums1的值在nums2位置的右邊第一個比較大的值, 如果沒有就回傳-1
        //回傳一個array長度和nums1相同
        vector<int> res;
        //找到nums1的值在nums2的哪裡
        for(int i = 0; i < nums1.size(); i++){
            int index = 0;
            for(int j = 0; j < nums2.size(); j++){
                if(nums1[i] == nums2[j]){
                    index = j;
                    //找到nums1的值在nums2的哪
                    break;
                }
            }
        //找nums1的值在nums2中右邊大的值
        int index2 = -1;
            for(int k = index; k < nums2.size(); k++){
                if(nums2[k] > nums1[i]){
                    index2 = k;
                    //找到nums1的值在nums2中右邊第一個大的值
                    break;
            }
        }
        if(index2 != -1){
            res.push_back(nums2[index2]);
        }else{
            res.push_back(index2);
        }
        }
        return res;
    }
};

```
```c++=
class Solution {
public:
    vector<int> nextGreaterElement(vector<int>& nums1, vector<int>& nums2) {
        //找到下一個較大的值
        //給兩個從零開始的array nums1和nums2,nums1是nums2的subarray
        //找到nums1的值在nums2位置的右邊第一個比較大的值, 如果沒有就回傳-1
        //回傳一個array長度和nums1相同
        vector<int> ans;
        stack <int> st;
        unordered_map <int, int> hash;
        //先用stack和hash掃過nums2紀錄nums2中每一個值的右邊第一個大值
        for(int i = 0; i < nums2.size(); i++){
            while(!st.empty()&& st.top() < nums2[i]){
                //如果stack不為空, 且下一個要進來的值大於stack中的值
                //則壓入hash中
                //用while可以檢查到st中下面值是否符合條件
                hash[st.top()] = nums2[i];
                st.pop();
            }
        st.push(nums2[i]);
        } 
        //掃描hash得到nums1的值在nums2中的右邊第一個比較大的值
        for(int i = 0; i < nums1.size(); i++){
            if(hash.find(nums1[i]) != hash.end()){
                ans.push_back(hash[nums1[i]]);
            }else{
                ans.push_back(-1);
            }
        }
        return ans;
    }
};

```

###### tags: `hash and stack`

