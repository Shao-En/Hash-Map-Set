思考
1.達成條件要素 兩個單字的組成相同 兩個單字的出現次數陣列相同
2.用set or vector去判斷組成問題
3.用vector排序後去判斷出現次數陣列相同問題

```c++=
class Solution {
public:
    bool closeStrings(string word1, string word2) {
        vector<int> v1(26, 0);
        vector<int> v2(26, 0);
        unordered_set<char> set1;
        unordered_set<char> set2;
        int m = word1.size();
        int n = word2.size();
        if(m != n){
            return false;
        }
        for(int i = 0; i < m; i++){
            set1.insert(word1[i]);
            set2.insert(word2[i]);
        }
        for (char ch : word1) {
            v1[ch - 'a']++;
        }
        
        for (char ch : word2) {
            v2[ch - 'a']++;
        }
        
        sort(v1.begin(), v1.end());
        sort(v2.begin(), v2.end());
        
        return v1 == v2 && set1 == set2;        
    }
};
```

```c++=
class Solution {
public:
    bool closeStrings(string word1, string word2) {
        if (word1.length() != word2.length()) {
            return false;
        }
        
        vector<int> count1(26, 0);
        vector<int> count2(26, 0);
        vector<int> freq1(26, 0);
        vector<int> freq2(26, 0);
        
        for (char ch : word1) {
            count1[ch - 'a']++;
            freq1[ch - 'a'] = 1;
        }
        
        for (char ch : word2) {
            count2[ch - 'a']++;
            freq2[ch - 'a'] = 1;
        }
        
        sort(count1.begin(), count1.end());
        sort(count2.begin(), count2.end());
        
        return count1 == count2 && freq1 == freq2;
    }
};


```

```c++=
class Solution {
public:
    bool closeStrings(string word1, string word2) {
        //元素組成相同 用set判斷組成
        //出現次數相同 用array判斷出現次數
        //符合兩個條件就代表兩個字是Close
        unordered_set <char> set1;
        unordered_set <char> set2;
        for(char i : word1){
            set1.insert(i);
        }
        for(char i : word2){
            set2.insert(i);
        }
        unordered_map <char, int> hash1;
        unordered_map <char, int> hash2;
        for(char i : word1){
            hash1[i]++;
        }
        for(char i : word2){
            hash2[i]++;
        }
        vector<int> v1;
        vector<int> v2;
        //建立儲存重複次數的array
        for(pair<char, int> i : hash1){
            v1.push_back(i.second);
        }
        for(pair<char, int> i : hash2){
            v2.push_back(i.second);
        }
        sort(v1.begin(), v1.end());
        sort(v2.begin(), v2.end());
        return set1 == set2 && v1 == v2;
    }
};

```