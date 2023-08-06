https://leetcode.cn/u/nettee/

# 牛客ACM模式





## string

### 字符串输入getline()

第一种重载，在\<istream>的getline函数有两种重载形式：

getline函数可以从输入流中读取一行字符串，包括空格和特殊字符，并将其存储在字符串对象中。**你可以指定一个分隔符来终止读取**，如果不指定，默认是换行符’\n’（**但不会保存这个换行符**）。

```c++
// 这是cin的成员函数
istream& getline (char* s, streamsize n); // 读到换行符、或者读了n个字符，停止读取
// 自定义终止符，会读取终止符，但是不会保存到字符串中（不管终止符是什么，都不会保存到字符串中）
// 但是不能用delim不是换行符的情况下去读取一行，因为这样的话，遇到换行符也会返回，并且也会把换行符存到s中
istream& getline (char* s, streamsize n, char delim); 
```

```c++
#include<iostream>
using namespace std;
int main()
{
	char s1[100],s2[100];
	//输入的数据：aa,bb,cc
	//第一种重载
	cin.getline(s1,100);
	cout<<s1<<endl;//aa,bb,cc
	//第二种重载
	cin.getline(s2,100,',');
	cout<<s2<<endl;//aa
	cin.getline(s2,100,',');
	cout<<s2<<endl;//bb
	return 0;
}
```

第二种用法
在\<string>中的getline函数有四种重载形式：

```c++
/*
is：表示一个输入流，例如cin。
str：string类型的引用，用来存储输入流中的流信息。
delim：char类型的变量，所设置的截断字符；在不自定义设置的情况下，遇到’\n’，则终止输入。
*/
istream& getline (istream&  is, string& str, char delim);
istream& getline (istream&& is, string& str, char delim);
istream& getline (istream&  is, string& str);
istream& getline (istream&& is, string& str);
```

## stringstream

stringstream是C++标准库中的一个类，用于将字符串转换为各种数据类型，例如整数、浮点数和布尔值等，并支持将这些数据类型转换回字符串。使用stringstream需要包含头文件。

```c++
#include <iostream>
#include <sstream>
#include <string>
#include <cassert>
using namespace std;
// 将一个整形变量转化为字符串，存储到string类对象中
int main()
{
	int a = 12345678;
	string sa;
	stringstream ss;
	ss << a;
	ss >> sa;
    // ss一定读到文件尾，所以返回false
    if(!ss.good()) std::cout << "ss is false." << std::endl;
	ss.str("");
	ss.clear();// 重置标志位
    // 如果不重置标志位，这里还返回false，重置了就为true
	if(!ss.good()) std::cout << "ss is false." << std::endl;
	double d = 12.34;
    ss << d;
	ss >> sa;
	string sValue;
	//这个方法会把管理的整个string对象返回，不会受读/写指针的影响
	sValue = ss.str();
	if(!ss.fail()) // 判断最后一次转换操作是否成功
		cout << sValue << endl;
	else cout << "转换失败" << endl;
	return 0;
}

```

**实现字符串分割**

```c++
#include <iostream>
#include <string>
#include <sstream>
#include <vector>
using namespace std;

// 实现一行的字符串分割，分隔符为空格
void spilt1(){
    vector<string> vec;
    string line, tmp;
    getline(cin, line); // 读取一行, 不会包含末尾换行符
    stringstream ss(line);
    while(ss >> tmp){ // 一旦ss读取到末尾，就会返回false
        vec.push_back(std::move(tmp));
    }
    for(auto& str: vec)
        cout << str << " " << str.size() << endl;
}

// 实现一行的字符串分割，分隔符为 #   但是如果前后有空格的话要小心
void spilt2(){
    vector<string> vec;
    string line, tmp;
    getline(cin, line); // 读取一行, 不会包含末尾换行符
    stringstream ss(line);
    while(getline(ss, tmp, '#')){ // 一旦ss读取到末尾，就会返回false
            vec.push_back(std::move(tmp));
    }
    for(auto& str: vec)
        cout << str << " " << str.size() << endl;  
}

// 实现一行的数字分割，分隔符为空格
void spilt3(){
    vector<int> vec;
    string line;
    int tmp;
    getline(cin, line); // 读取一行, 不会包含末尾换行符
    stringstream ss(line);
    while(ss >> tmp){ // 一旦ss读取到末尾，就会返回false
        vec.push_back(tmp);
    }
    for(auto& e: vec)
        cout << e << endl;
}

// 实现一行的数字分割，分隔符为#
void spilt4(){
    vector<int> vec;
    string line, tmp;
    getline(cin, line); // 读取一行, 不会包含末尾换行符
    stringstream ss(line);
    while(getline(ss, tmp, '#')){ // 一旦ss读取到末尾，就会返回false
        vec.push_back(atoi(tmp.c_str()));
    }
    for(auto& e: vec)
        cout << e << endl;
}

int main(){
    spilt4();
}
```

## [HJ1字符串最后一个单词的长度](https://www.nowcoder.com/practice/8c949ea5f36f422594b306a2300315da?tpId=37&tqId=21224&rp=1&ru=/exam/oj/ta&qru=/exam/oj/ta&sourceUrl=%2Fexam%2Foj%2Fta%3FtpId%3D37&difficulty=undefined&judgeStatus=undefined&tags=&title=) ×

```c++
#include <iostream>
#include <string>
#include<sstream>
using namespace std;

/*
在C++中，使用cin >> s时，输入运算符（>>）会根据空白字符（包括空格、制表符和换行符）来分割输入。每次输入运算符会读取并存储一个字符串（不包含空白字符），直到遇到下一个空白字符为止。
*/
void ans1(){
    string s;
    while(cin >> s); // s会保存最后一个单词
    cout << s.size();
    return;
}

void ans2(){
    string s;
    getline(cin, s); // getline会读取一行，不读取最后一个换行符
    stringstream ss(s);
    while(ss >> s); // 仍然会按照空格、制表符、换行符分开，
    cout << s.size();
    return;
}

void ans3(){
    string s;
    while(getline(cin, s, ' '));
    cout << s.size();
}

int main() {
    ans3();
    return 0;
}
```





# XXXXXXXXXXXXXXXXXX

##  三星 没做

113 64 48 



dp子序列问题：

![](https://pic.leetcode-cn.com/1625312260-JHtQFs-image.png)

回溯是真记不住！！



# 🔥 LeetCode 热题 HOT 100

## [1. 两数之和](https://leetcode.cn/problems/two-sum/)（简单-哈希表）√√√

```c++
class Solution {
public:
    vector<int> twoSum(vector<int>& nums, int target) {
        unordered_map<int, int> umap;
        for(int i = 0; i < nums.size(); ++i){
            if(umap.find(target - nums[i]) != umap.end())
                return {umap[target - nums[i]], i};
            umap[nums[i]] = i;
        }
        return {};
    }
};
```

## [2. 两数相加](https://leetcode.cn/problems/add-two-numbers/)（中等-模拟题-数学）√√√

重点是当有一个链表为空了不单独处理，按节点为0处理。

```c++
class Solution {
public:
    ListNode* addTwoNumbers(ListNode*l1, ListNode* l2){
			ListNode* preHead = new ListNode(-1), *r = preHead;
			int flag = 0;
			while(l1 || l2){
				int s = (l1 ? l1->val : 0) + (l2 ? l2->val : 0) + flag;
				ListNode* node = new ListNode(s % 10);
				r->next = node;
				r = r->next;
				flag = s / 10;
				if(l1) l1 = l1->next;
				if(l2) l2 = l2->next;
			}
			if(flag){
				ListNode* node = new ListNode(1);
				r->next = node;
			}
			return preHead->next;
		}
		
};
```

## [3. 无重复字符的最长子串](https://leetcode.cn/problems/longest-substring-without-repeating-characters/)（中等-滑动窗口-哈希表） ×√×√！

滑动窗口三步走：

+ 更新右边界，for中自动就更新了；
+ 判断是否需要更新左边界；
+ 更新res;

![](https://pic.leetcode-cn.com/8b7cac826e572c65f8b77e0f380eaa93ab665857a8e916bc4ea36b7765eafc55-%E5%9B%BE%E7%89%87.png)

```c++
class Solution {
public:
    int lengthOfLongestSubstring(string s) {
        int res = 0;
        unordered_map<char, int> umap;
        for(int slow(0),fast(0); fast < s.size(); ++fast){
            // 如果当前字符出现了第二次，那么就要更新到不重复的位置，那么为什么要取最大？因为slow已经去后面了，如果出现一个很久之前的字符，不可能往回更新
            slow = max(umap[s[fast]], slow);
            res = max(res, fast - slow + 1);
            umap[s[fast]] = fast + 1;
        }
        return res;
    }
};
```



## [4. 寻找两个正序数组的中位数](https://leetcode.cn/problems/median-of-two-sorted-arrays/)（困难）？

```c++

```

## [5. 最长回文子串](https://leetcode.cn/problems/longest-palindromic-substring/) （中等-双指针）×√√

关键点就在于回文串中点可能是一个字符，可能是两个字符。关键处见代码标注。

```c++
class Solution {
public:
    int left = 0, maxLength = 0; // 只需要更新左端点和最大长度就可以
    string longestPalindrome(string s) {
        for(int i = 0; i < s.size(); ++i){
            extend(s, i, i); // ！！！！关键在这里！默认以两个字符为中点，单个字符为中点时都传i！
            extend(s, i, i + 1);
        }
        return s.substr(left, maxLength);
    }
    void extend(string& s, int i, int j){
        for(; i >= 0 && j < s.size() && s[i] == s[j]; --i, ++j){
            if(j - i + 1 > maxLength){ // 如果出现更大的，更新左端点和长度
                left = i;
                maxLength = j - i + 1;
            }
        }
    }
};
```

## [10. 正则表达式匹配](https://leetcode.cn/problems/regular-expression-matching/)（困难）×

https://leetcode.cn/problems/regular-expression-matching/solution/python3-ju-jue-rong-yu-fen-lei-chao-jian-onu9/

```c++
class  Solution {
public:
 bool isMatch(string  s, string  p) {
        int ns = s.size(), np = p.size();
        vector<vector<bool>> dp(np + 1, vector<bool>(ns + 1));
        dp[0][0] = true;
        for(int i = 2; i <= np; ++i){
            if(p[i - 1] == '*')
                dp[i][0] = dp[i - 2][0];
        }
        auto match = [](char p, char s){ return p == '.' || p == s; };
        for(int i = 1; i <= np; ++i){
            for(int j = 1; j <= s.size(); ++j){
    if(p[i - 1] == '*'){
        if(match(p[i - 2], s[j - 1])) // 不可能以 * 开头  不用担心p越界
      dp[i][j] = dp[i - 2][j] || dp[i][j - 1];
                    else
                        dp[i][j] = dp[i - 2][j];
     
    }else
     dp[i][j] = (match(p[i - 1], s[j - 1]))  && dp[i-1][j-1];
            }
        }
        
  return dp[np][ns];
    }

};
/* 
两种情况：
1 如果当前字符为*， p：abb*    s：abbb

1.1 使用 X*（*不干掉X） ，也就是p可能是abb、abbb：   
如果p[i - 2]能匹配s[j - 1]， 也就是p[2]的b == s[3]的b => 那么dp[i][j] = dp[i-2][j](X* -》 X) or dp[i][j-1]（因为前提是p[2] == s[3]  == b, 如果p(abb*)匹配了s的（abb）,那么*一定可以再扩展一个b。或者说abb*匹配了abb，只要s的下一个字符等于p*前面的字符，那么*号就能扩展）

1.2 如果p[i - 2]不能匹配s[j - 1]：
那么用*把前面的字符干掉，看能否匹配。

2 如果当前字符不为*，就很简单了。

*/
```

## [11. 盛最多水的容器](https://leetcode.cn/problems/container-with-most-water/)（中等-双指针）√√√

**缩减搜索空间**：[](https://leetcode.cn/problems/container-with-most-water/solution/on-shuang-zhi-zhen-jie-fa-li-jie-zheng-que-xing-tu/)

如果height[left] == height[right]，比如 3 1 10 3：

那么最终的最大值一定跟3有关系，因为不管时3 10  和 1 3 ，宽都变小了，高的最大值还是3，所以只要一边的高仍是3，面积一定会减少。

```c++
class Solution {
public:
    int maxArea(vector<int>& height) {
        int l = 0, r = height.size() - 1, res = 0;
        while(l < r){
            res = max(res, (r - l) * min(height[l], height[r]));
            if(height[l] > height[r]) --r;
            else ++l;
        }
        return res;
    }
};
```

## [15. 三数之和](https://leetcode.cn/problems/3sum/)（中等-双指针） ××√×√！

```
if(i && nums[i] == nums[i - 1]) continue; // 去重
比如 -1 -1 0 1 2不能允许第二个-1再凑一个 -1 0 1 ，但是第一个允许，可以凑-1 -1 2

if(s < 0) ++slow; // 这里不能是while，因为每次fast/slow 变化了，s 要重新计算
如果写成while(slow < fast && s < 0) ++slow; 没有任何意义，因为s没机会重新计算，slow最终只会等于fast

for(++slow; slow < fast && nums[slow] == nums[slow - 1]; ++slow);
假设  -1 -1 -1 0 1 2;    -1 -1 2 是一个元组，slow前移，如果发现nums[slow] == nums[slow-1] 那必须继续前移，因为x不变，如果nums[slow]还不变，那nums[fast]肯定也不能变，就会产生重复三元组。
```

```c++
class Solution {
public:
    vector<vector<int>> threeSum(vector<int>& nums) {
       sort(nums.begin(), nums.end());
       vector<vector<int>> res;
       int n(nums.size());
       for(int i = 0; i < n - 2; ++i){
           if(nums[i] > 0) return res;// 剪枝，如果nums[i]>0，说明后面都大于0
           if(i && nums[i] == nums[i - 1]) continue; // 去重
           int x = nums[i];
           for(int slow(i + 1), fast(n - 1); slow < fast;){
               int s = x + nums[slow] + nums[fast];
               if(s < 0) ++slow;           // 这里不能是while，因为每次fast/slow 变化了，s 要重新计算
               if(s > 0) --fast;
               if(s == 0){ // 注意更新slow和fast也要在这里面更新，不是每次迭代都要更新
                   res.push_back({x, nums[slow], nums[fast]});
                   for(++slow; slow < fast && nums[slow] == nums[slow - 1]; ++slow);
                   for(--fast; slow < fast && nums[fast] == nums[fast + 1]; --fast);
               }
           }
       } 
       return res;
    }
};
```

## [17. 电话号码的字母组合](https://leetcode.cn/problems/letter-combinations-of-a-phone-number/)（中等-回溯）×√

这道题的关键是，不是从一个序列里面找k个字符的组合，而是从不同的字符串中寻找k个字符的排列。

```c++
class Solution {
public:
    string path;
    vector<string> res;
    vector<string> tel{"", "", "abc", "def", "ghi", "jkl", 
                            "mno", "pqrs", "tuv", "wxyz"};
    vector<string> letterCombinations(string digits) {
        if(!digits.size()) return res;
        DFS(digits, 0);
        return res;
    }
	// 关键在于 startIndex是digits的下标
    void DFS(string &digits, int startIndex){
        // 得到数字对应的字符串在tel中的下标
        int index = digits[startIndex] - '0';
        // 核心在这，常规回溯
        for(int i = 0; i < tel[index].size(); ++i){
            path.push_back(tel[index][i]);
            if(path.size() == digits.size())
                res.push_back(path);
            else if(startIndex + 1 < digits.size())
                DFS(digits, startIndex + 1);
            path.pop_back(); 
        }
    }
};
 
```

## [19. 删除链表的倒数第 N 个结点](https://leetcode.cn/problems/remove-nth-node-from-end-of-list/)（中等-链表）√√√

1 2 3 4 5  n = 2

+ slow fast 同时指向preHead；
+ fast先走两步到2；
+ fast到5，slow到3；
+ 注意循环结束条件是fast->next != nullptr！ 才能让fast停在5。

```c++
class Solution {
public:
    ListNode* removeNthFromEnd(ListNode* head, int n) {
        ListNode* preHead = new ListNode(-1, head), *fast = preHead, *slow = preHead;
        for(int i = 0; i < n; ++i)
            fast = fast->next;
        while(fast->next){
            fast = fast->next;
            slow = slow->next;
        }
        slow->next = slow->next->next;
        return preHead->next;
    }
};
```

## [20. 有效的括号](https://leetcode.cn/problems/valid-parentheses/)（简单-栈） √√√

```c++
class Solution {
public:
    bool isValid(string s) {
        unordered_map<char, char> umap{{')', '('},
                                       {']', '['},
                                       {'}', '{'}};
        stack<char> stk;
        for(auto c: s){
            if(stk.empty() || umap[c] != stk.top())
                stk.push(c);
            else stk.pop();
        }
        return stk.empty();
    }
};
```

## [21. 合并两个有序链表](https://leetcode.cn/problems/merge-two-sorted-lists/)（简单-链表） √√√

```c++
class Solution {
public:
    ListNode* mergeTwoLists(ListNode* list1, ListNode* list2) {
        ListNode* preHead = new ListNode, *r = preHead;
        while(list1 && list2){
            if(list1->val < list2->val){
                r->next = list1;
                list1 = list1->next;
            }else{
                r->next = list2;
                list2 = list2->next;
            }
            r = r->next;
        }
        r->next = list1 ? list1 : list2;
        return preHead->next;
    }
};
```

## [22. 括号生成](https://leetcode.cn/problems/generate-parentheses/)（中等-DFS/回溯）×！×

**好像这种问有多少种组合的题，都应该往DFS/回溯上想！**

动态规划也可以解，再说吧。

通过观察我们可以发现，生成的任何括号组合中都有两个规律：

+ 括号组合中左括号的数量等于右括号的数量；
+ 括号组合中任何位置左括号的数量都是大于等于右括号的数量。

第一条很容易理解，我们来看第二条，比如有效括号"（（））（）"，**在任何一个位置右括号的数量都不大于左括号的数量**，所以他是有效的。如果像这样"（））（）"第3个位置的是右括号，那么他前面只有一个左括号，而他和他前面的右括号有2个，所以无论如何都不能组合成有效的括号。

![](https://pic.leetcode-cn.com/1607265041-lcwBdE-image.png)

```c++
class Solution {
public:
    vector<string> res;
    vector<string> generateParenthesis(int n) {
        dfs("", n, n);
        return res;
    }
    void dfs(string path, int l, int r){

      if(l == 0 && r == 0){
          res.push_back(path);
          return;
      }
      if(l > 0) dfs(path + '(', l - 1, r);
      if(r > l) dfs(path + ')', l, r - 1);
    }
};
```

## [23. 合并K个升序链表](https://leetcode.cn/problems/merge-k-sorted-lists/)（困难-优先队列）×！×√

注意function的写法，注意priority_queue用自定义cmp时需要传入cmp。

模拟题，用优先队列优化。

简单来说先把所有链表的表头依次进入优先队列（小顶堆），此时堆顶就是最小值，然后r->next = node; 如果堆顶节点的next不空，那么就把next节点继续入队，堆顶还是最小的。

因为每构建一个节点，都要判断当前k个链表哪个头节点的值是最小的，用优先队列可以把这个过程的时间复杂度降到O(1).

```c++
/*
1：这里要注意，当头节点不为空时才能入队
*/
class Solution {
public:
    ListNode* mergeKLists(vector<ListNode*>& lists) {
        ListNode* preHead = new ListNode, *r = preHead;
        priority_queue<ListNode*, vector<ListNode*>, function<bool(ListNode*, ListNode*)>> pque([](ListNode* l, ListNode* r){
            return l->val > r->val;
        });
        for(auto l: lists)
            if(l) pque.push(l); // 1
        while(!pque.empty()){
            ListNode* node = pque.top(); pque.pop();
            r->next = node; r = r->next;
            if(node->next) pque.push(node->next);
        }
        return preHead->next;
    }
};
```

## [31. 下一个排列](https://leetcode.cn/problems/next-permutation/)（中等）？

https://leetcode.cn/problems/next-permutation/solution/xia-yi-ge-pai-lie-suan-fa-xiang-jie-si-lu-tui-dao-/

## [32. 最长有效括号](https://leetcode.cn/problems/longest-valid-parentheses/)（困难）？

https://leetcode.cn/problems/longest-valid-parentheses/solution/shou-hua-tu-jie-zhan-de-xiang-xi-si-lu-by-hyj8/

## [33. 搜索旋转排序数组](https://leetcode.cn/problems/search-in-rotated-sorted-array/)（中等-二分）××√

这道题的关键在于：

+ 不要先去判断target 和nums[mid]的关系，进而去修改l 和 r ! 假设用例为[4,5,6,7,8,1,2,3] 8，那么nums[mid]是7，那么8大于7，但是又大于3：反正以下这么写是不对的：

  ```c++
  class Solution {
  public:
      int search(vector<int>& nums, int target) {
          int n = nums.size();
          int l = 0, r = n - 1;
          while(l <= r){
              int mid = l + (r - l) / 2;
              if(nums[mid] == target) return mid;
              if(nums[mid] > target){
                  if(nums[l] <= target) r = mid - 1;
                  else l = mid + 1;
              }
              if(nums[mid] < target){
                  if(nums[r] >= target) l = mid + 1;
                  else r = mid - 1;
              }
          }
          return -1;
      }
  };
  ```

+ 正确做法是，每当得到nums[mid]，先去判断前半部分是正序还是后半部分是正序，一定有一部分是正序的！然后再根据target和和nums[mid]的关系去修改l或r。

```c++
class Solution {
public:
    int search(vector<int>& nums, int target) {
        int l = 0, r = nums.size() - 1, mid = 0;
        while (l <= r) {
            mid = l + (r - l) / 2;
            if (nums[mid] == target) {
                return mid;
            }
            // 如果nums[mid] >= nums[l] 说明left-mid是正序
            if (nums[mid] >= nums[l]) {
                // 如果target在left-mid内
                //必须左右都要判断，否则就算target比nums[mid]小，也有可能在nums[mid]右面
                if (target >= nums[l] && target < nums[mid]) {
                    r = mid - 1;
                } else { // 如果不在
                    l = mid + 1;
                }
            } else { // 说明left-mid不是正序， 那么mid-right肯定是正序
                if (target > nums[mid] && target <= nums[r]) {
                    l = mid + 1;
                } else {
                    r = mid - 1;
                }
        }
    }
    return -1;
    }
};
```

## [34. 在排序数组中查找元素的第一个和最后一个位置](https://leetcode.cn/problems/find-first-and-last-position-of-element-in-sorted-array/)（中等-二分）×√

```c++
/*
1: 只有这里要注意，当target 大于数组内所有元素时，或者当nums.size() == 0时，l会等于nums.size()；
   当数组内不存在target时，nums[left] != target。
这两种情况都要考虑到。
*/
class Solution {
public:
    vector<int> searchRange(vector<int>& nums, int target) {
        int n = nums.size(), l = 0, r = n - 1, mid = 0;
        while(l <= r){
            mid = l + (r - l) / 2;
            if(nums[mid] >= target) r = mid - 1;
            else l = mid + 1;
        }
        int left = l;
        if(left == nums.size() || nums[left] != target) return {-1, -1}; // 1
        r = n - 1;
        while(l <= r){
            mid = l + (r - l) / 2;
            if(nums[mid] <= target) l = mid + 1;
            else r = mid - 1;
        }
        int right = r;
        return {left, right};
    }
};
```



## [39. 组合总和](https://leetcode.cn/problems/combination-sum/)（中等-回溯）√×

```c++
class Solution {
public:
    vector<vector<int>> res;
    vector<int> path;
    vector<vector<int>> combinationSum(vector<int>& candidates, int target) {
        backTracking(candidates, target, 0);
        return res;
    }
    void backTracking(vector<int>& candidates, int target, int startIndex){
        for(int i = startIndex; i < candidates.size(); ++i){
            // 先进来
            path.push_back(candidates[i]); 
            target -= candidates[i]; 
			
            // 由于元素可以重复使用
            if(target > 0) backTracking(candidates, target, i); // 有没有必要继续递归
            if(target == 0) res.push_back(path); // 符合条件，加入结果集
            // 不管是递归结束，还是符合条件加入结果集，都要弹出尾元素
            target += candidates[i];
            path.pop_back();
        }
    }
};
```



## [42. 接雨水](https://leetcode.cn/problems/trapping-rain-water/)（困难-单调栈）×！

```c++
class Solution {
public:
    int trap(vector<int>& height) {
        int res = 0;
        stack<int> stk; // 单调递减栈 （注意存的不是高度是下标!）
        for(int cur = 0; cur < height.size(); ++cur){
            while(!stk.empty() && height[cur] > height[stk.top()]){ // 1
                int bottomIndex = stk.top(); stk.pop(); // 2
                if(stk.empty()) break; // 3
                int l = stk.top(), r = cur; // 4
                int h = min(height[l], height[r]) - height[bottomIndex]; // 5
                res = res + (r - l - 1) * h; // 6
            }
            stk.push(cur);
        }
        return res; 
    }
};
/*
单调递减栈中存的是元素下标，不是下标单调递减，而是下标对应的元素单调递减。
整体思路：首先我们用的是单调递减栈，所以当 当前柱子高度比栈顶要大时，说明一定可以接水。
因为是 1 0 2 的关系
1: 如果栈不空，且当前柱子高度大于栈顶，说明出现了水沟
2: bottomIndex是水沟所在的下标, 然后出栈
3: 如果栈空了，说明没有左边的柱子，或者说当前是单调增的趋势，那么就把栈顶pop，然后break结束while，然后把当前的柱子下标入栈，维持单调递减栈的性质。
4: 由于刚才的bottomIndex已经出栈，所以现在栈顶元素是左柱子元素的下标
5: 水沟的高度就是左右柱子高度的最小值减去水沟的高度
6：这里要强调的是，由于计算了一个水沟之后，会出栈，因此，比如倒着的“土”，当中间的块出栈之后，
会变成“一”块，此时是不符合单调递减栈的，但是下一个柱子入栈时会一起计算这个“一”的面积。
*/
```

## [46. 全排列](https://leetcode.cn/problems/permutations/)(中等-回溯) ××√

```c++
class Solution {
public:
    vector<vector<int>> res;
    vector<int> path;
    vector<vector<int>> permute(vector<int>& nums) {
        vector<bool> visited(nums.size());
        backTracking(nums, visited);
        return res;
    }
    void backTracking(vector<int>& nums, vector<bool>& visited){
        for(int i = 0; i < nums.size(); ++i){
            if(visited[i]) continue;
            path.push_back(nums[i]);
            visited[i] = true;
            if(path.size() == nums.size()){
                res.push_back(path);
            }else 
                backTracking(nums, visited);
            path.pop_back();
            visited[i] = false;
        }
        return;
    }
};
```

## [48. 旋转图像](https://leetcode.cn/problems/rotate-image/)（中等-模拟题（矩阵））×√√

副对角线没做过，下次找一道试一试。

对于n * n 的矩阵旋转题，首先要掌握四种翻转转的转移式，然后各种旋转都能转换成这四种翻转的组合：

```
上下翻转：matrix[i][j] = matrix[n - i - 1][j]；
左右翻转：matrix[i][j] = matrix[i][n-j-1]；
主对角线翻转：matrix[i][j] = matrix[j][i]；
副对角线翻转：matrix[i][j] = matrix[n-j-1][n-i-1]；
```

```
对于本题旋转90°，转移式为：matrix[i][j] = matrix[j][n-i-1]
根据上面四个转移式，可以观察到，只要把i变成n-i-1，再进行主对角线翻转，就实现了旋转90°的效果。
所以我们可以先对matrix[i][j]进行上下翻转 -> matrix[n-i-1][j];       再进行主对角线旋转，就可以旋转90°啦。
```

所以不管题目千变万化，只要先想出转移式，再找到翻转的组合，就ok了。

```c++
class Solution{
public:
	void rotate(vector<vector<int>>& matrix){
		upDownRotate(matrix);
		mainDiagRotate(matrix);
	}
	
	void upDownRotate(vector<vector<int>>& matrix){ // 上下翻转
		int n = matrix.size();
		for(int i = 0; i < n / 2; ++i){
			for(int j = 0; j < n; ++j)
				swap(matrix[i][j], matrix[n - i - 1][j]);
		}
	}
	
	void leftRightRotate(vector<vector<int>>& matrix){ // 左右翻转
		int n = matrix.size();
		for(int j = 0; j < n / 2; ++j){
			for(int i = 0; i < n; ++j)
				swap(matrix[i][j], matrix[i][n - j - 1]);
		}
	}
	
	void mainDiagRotate(vector<vector<int>>& matrix){ // 主对角线翻转
		int n = matrix.size();
		for(int i = 0; i < n - 1; ++i){
			for(int j = i + 1; j < n; ++j)
				swap(matrix[i][j], matrix[j][i]);
		}
	}
	void subDiagRotate(vector<vector<int>>& matrix){ // 副对角线翻转
		int n = matrix.size();
		for(int i = 0; i < n - 1; ++i){
			for(int j = 0; j < n-i-1; ++j)
				swap(matrix[i][j], matrix[n-j-1][n-i-1]); 
		}
	}
};
// 1 2 3
// 4 5 6
// 7 8 9
```

```cpp
// second：
class Solution {
public:
    void rotate(vector<vector<int>>& matrix) {
        rotateUpDown(matrix);
        rotateMainDjx(matrix);
    }
    void rotateUpDown(vector<vector<int>>& matrix){
        int r = matrix.size();
        int c = matrix[0].size();
        for(int i = 0; i < r / 2; ++i){
            for(int j = 0; j < c; ++j)
                swap(matrix[i][j], matrix[r - 1 - i][j]);
        }
    }
    void rotateMainDjx(vector<vector<int>>& matrix){
        int r = matrix.size();
        int c = matrix[0].size();
        for(int i = 0; i < r - 1; ++i){
            for(int j = i + 1; j < c; ++j)
                swap(matrix[i][j], matrix[j][i]);
        }
    }


};
/*
自己分析一手：
updown：
matrix[i][j] = matrix[n - 1 - i][j]
main_djx：
matrix[i][j] = matrix[j][i]
leftright:
matrix[i][j] = matrix[i][n - 1 - j]


本题：
matrix[i][j] = matrix[j][n - 1 - i]
matrix[i][j]先上下翻转->matrix[n - 1 - i][j]
再主对角线翻转就ok了

*/
```

## [49. 字母异位词分组](https://leetcode.cn/problems/group-anagrams/)（中等-哈希）××

用for遍历unordered_map，返回的竟然不是迭代器是pair？

for遍历unordered_set，也是直接返回值，可能是我记错了。

```c++
for(auto it = uset.begin(); it != uset.end(); ++it){
            cout<< *it <<" "<<endl;
}
// 这样才是迭代器
```

```c++
class Solution {
public:
    vector<vector<string>> groupAnagrams(vector<string>& strs) {
        vector<vector<string>> res;
        unordered_map<string, vector<string>> umap;
        for(auto& str: strs){
            string tmp = str; // ！！！！！！！
            sort(tmp.begin(), tmp.end());
            umap[tmp].push_back(str);
        }
        for(auto& it: umap){
            res.push_back(std::move(it.second));
        }
        return res;
    }
};
```

第二遍又没做出来，我想到排序后用哈希了，但是我不知道给每个单词排序后，放进哈希，往vector<string>里插入时怎么找到原来的单词。

关键是遍历每个原始strs的但此时，在循环内定义一个tmp来排序就好了。脑子瓦特了。

## [53. 最大子数组和](https://leetcode.cn/problems/maximum-subarray/)（中等-动态规划/分治）√√√

**最后一遍看看分治法！！！**

只需要注意dp[i]是截止i之前的最大子数组和，所以res是dp中的最大值。

```c++
class Solution {
public:
    int maxSubArray(vector<int>& nums) {
        int n = nums.size();
        vector<int> dp(n); // （包含）下标为i前的最大子数组和
        dp[0] = nums[0];
        int res = dp[0];
        for(int i = 1; i < n; ++i){
            dp[i] = max(nums[i], nums[i] + dp[i - 1]);
            res = max(res, dp[i]);
        }
        return res;
    }
};
```

## [55. 跳跃游戏](https://leetcode.cn/problems/jump-game/)（中等-动态规划）××√

```c++
/*
很简单啊，dp[i]表示在下标i能到达的最远下标，dp[0]自然就等于nums[0]。
1 如果从dp[i-1]到不了i，那直接返回false
2 如果dp[i-1]可以直接到达最后一个下标，直接返回true
*/
class Solution {
public:
    bool canJump(vector<int>& nums) {
        int n = nums.size();
        vector<int> dp(n);
        dp[0] = nums[0];
        for(int i = 1; i < n; ++i){
            if(dp[i - 1] < i) return false; // 1
            if(dp[i - 1] >= n - 1) return true; // 2
            dp[i] = max(dp[i - 1], i + nums[i]);
        }
        return true;
    }
};

class Solution {
public:
    bool canJump(vector<int>& nums) {
        int n = nums.size();
        int dp = nums[0];
        for(int i = 1; i < n; ++i){
            if(dp < i) return false;
            if(dp >= n - 1) return true;
            dp = max(dp, i + nums[i]);
        }
        return true;
    }
};
```

## [56. 合并区间](https://leetcode.cn/problems/merge-intervals/)（中等-区间问题）××√

```c++
/*
1：最后一个区间，无论是重叠，还是没重叠，在for循环中都没有处理，因此要处理一下。
2：注意，当冲突时，r要用max取最大
*/
class Solution {
public:
    vector<vector<int>> merge(vector<vector<int>>& intervals) {
        sort(intervals.begin(), intervals.end());
        vector<vector<int>> res;
        int l = intervals[0][0], r = intervals[0][1];
        for(int i = 1; i < intervals.size(); ++i){
            if(intervals[i][0] <= r) r = max(r, intervals[i][1]); // 2
            else{
                res.push_back({l, r});
                l = intervals[i][0];
                r = intervals[i][1]; // 2
            }
        }
        res.push_back({l, r}); // 1
        return res;
    }
};
```

## [62. 不同路径](https://leetcode.cn/problems/unique-paths/)(中等-动态规划) √√√

```c++
class Solution {
public:
    int uniquePaths(int m, int n) {
        vector<vector<int>> dp(m, vector<int>(n, 1));
        for(int i = 1; i < m; ++i){
            for(int j = 1; j < n; ++j)
                dp[i][j] = dp[i - 1][j] + dp[i][j - 1];
        }
        return dp[m - 1][n - 1];
    }
};
```

## [64. 最小路径和](https://leetcode.cn/problems/minimum-path-sum/)（中等-动态规划）√

下次尝试用DFS/BFS做一下。

```C++
class Solution {
public:
    int minPathSum(vector<vector<int>>& grid) {

        int m = grid.size(), n = grid[0].size();
        vector<vector<int>> dp(m, vector<int>(n, grid[0][0]));
        for(int j = 1; j < n; ++j)
            dp[0][j] = grid[0][j] + dp[0][j-1];
        for(int i = 1; i < m; ++i)
            dp[i][0] = grid[i][0] + dp[i-1][0];   
        for(int i = 1; i < m; ++i){
            for(int j = 1; j < n; ++j)
                dp[i][j] = grid[i][j] + min(dp[i-1][j], dp[i][j-1]);
        }
        return dp[m-1][n-1];
    }
};
```



## [70. 爬楼梯](https://leetcode.cn/problems/climbing-stairs/)（简单-动态规划）√√√

```c++
/*
n = 1, 需要1阶到楼顶，说明当前在第0层。
*/
class Solution {
public:
    int climbStairs(int n) {
        if(n <= 2) return n;
        vector<int> dp(n + 1);
        dp[1] = 1;
        dp[2] = 2;
        for(int i = 3; i < dp.size(); ++i){
            dp[i] = dp[i - 1] + dp[i - 2];
        }
        return dp[n];
    }
};

class Solution {
public:
    int climbStairs(int n) {
        if(n <= 2) return n;
        int a = 1, b = 2, c = 0;
        for(int i = 3; i < n + 1; ++i){
            c = a + b;
            a = b;
            b = c;
        }
        return c;
    }
};
```

##  [72. 编辑距离](https://leetcode.cn/problems/edit-distance/)（困难-动态规划）×

![](https://pic.leetcode-cn.com/7bf05e42bb31e224d06701cf72a96cfa4ac0c4cd5fdc927ec0b6fa5198449b66-%E5%9B%BE%E7%89%87.png)

|      | x    | a    | b    |
| ---- | ---- | ---- | ---- |
| x    | 0    | 1    | 2    |
| a    | 1    | 0    |      |

dp[i]\[j]表示word1前i个字符转换成word2前j个字符所用的最少操作数。

对于红框里的2，要么从他左面来，hro匹配r（hro已经变成r了）需要操作两次，hro再想匹配ro只能再操作一次添加一个o，也就是3次；

要么从左上方来，ho匹配r操作了2次，再操作一次添加一个o，一共操作3次；

要么从上方来，ho匹配r操作了一次，hor想匹配ro只需要把r改成o（操作1次）；(!如果word1[i] = word2[j]，那么显然不需要修改。)

```c++
word1[i - 1] == word2[j - 1];
对于这一行，因为在word1和word2前面都加了个空字符，所以在dp[i][j]里其实判断的是word1[i-]和word2[j-1].
dp[0][0]是空字符，dp[1][1]判断是才是word里的第一个字符。
```

```c++


class Solution {
public:
    int minDistance(string word1, string word2) {
        int m = word1.size(), n = word2.size();
        vector<vector<int>> dp(m + 1, vector<int>(n + 1));
        for (int j = 0; j <= n; j++) {
            dp[0][j] = j;
        }
        for (int i = 0; i <= m; i++) {
            dp[i][0] = i;
        }
        
        for (int i = 1; i <= m; i++) {
            for (int j = 1; j <= n; j++) {
                dp[i][j] = min(dp[i - 1][j - 1], min(dp[i][j-1], dp[i - 1][j])) + 1;
                if (word1[i - 1] == word2[j - 1]) {
                    dp[i][j] = min(dp[i][j], dp[i - 1][j - 1]);
                }
            }
        }
        return dp.back().back();
    }
};

```

## [75. 颜色分类](https://leetcode.cn/problems/sort-colors/)（中等-循环不变量）×！××

[剑指 Offer 21. 调整数组顺序使奇数位于偶数前面](https://leetcode.cn/problems/diao-zheng-shu-zu-shun-xu-shi-qi-shu-wei-yu-ou-shu-qian-mian-lcof/)

```c++
class Solution {
public:
    void sortColors(vector<int>& nums) {
       
        int n = nums.size() - 1;
        int zero = -1, two = n + 1;
        for(int i = 0; i < two;){
            if(nums[i] == 1){
                ++i;
                continue;
            }
            if(nums[i] == 0){
                ++zero;
                swap(nums[zero], nums[i]);
                ++i;
            }else{
                --two;
                swap(nums[i], nums[two]);
            }
        }
    }
};
```

## [76. 最小覆盖子串](https://leetcode.cn/problems/minimum-window-substring/)（困难-滑动窗口）×！×

```c++
/*
1 umap_t存储t中字符出现的个数，也是s中需要出现的字符的个数。
2 cnt维护的是s字符串[l, r]区间中出现在t中的字符的个数。
3 如果--umap_t[s[r]] >= 0，说明该字符是必须的
4 如果umap_t[s[l]] < 0， 说明已经超了，窗口左面可以缩小；但如果umap_t[s[l]] >= 0, 说明现在是正好的或者还不够，窗口不能缩小。
5 当cnt == t.size()之后，说明t中的字符在窗口中都出现了，cnt不会再变大也不会变小了。
6 如果cnt始终没有== t.size(), 说明没有符合条件的窗口。

对于滑动窗口来说，首先都是要让窗口符合条件，再更新最终结果。
*/
class Solution {
public:
    string minWindow(string s, string t) {
        string res;
        int start = 0, length = INT_MAX;
        unordered_map<char, int> umap_t; // 1
        int cnt = 0; // 2
        for(auto c: t)  ++umap_t[c];
        for(int l = 0, r = 0; r < s.size(); ++r){
            if(--umap_t[s[r]] >= 0) ++cnt; // 3
            while(umap_t[s[l]] < 0) ++umap_t[s[l++]]; // 4
            if(cnt == t.size() && length > r - l + 1){ // 5
                length = r - l + 1;
                start = l;
            }
        }
        if(cnt == t.size()) res = s.substr(start, length); // 6
        return res;
    }
};
```



## [78. 子集](https://leetcode.cn/problems/subsets/)（中等-回溯）×

https://leetcode.cn/problems/subsets/solution/c-zong-jie-liao-hui-su-wen-ti-lei-xing-dai-ni-gao-/

```c++
class Solution {
public:
    vector<vector<int>> res;
    vector<int> path;
    vector<vector<int>> subsets(vector<int>& nums) {
        backTracking(nums, 0);
        return res;
    }

    void backTracking(vector<int>& nums, int startIndex){
        res.push_back(path);
        for(int i = startIndex; i < nums.size(); ++i){
            path.push_back(nums[i]);
            backTracking(nums, i + 1);
            path.pop_back();
        }
    }
};
```

## [79. 单词搜索](https://leetcode.cn/problems/word-search/)（中等-DFS）×！××

```c++
/*
这个题很怪异，dfs是当i，j已经匹配了某个前缀，用index是否超过word.size()作为递归结束条件
1 在这里，是i，j已经匹配，然后进dfs，i, j已经匹配自然就不用判断了。
2 在这里要判断各种和new_i, new_j相关的条件。
*/
class Solution {
public:
    bool flag = false;
    int x[4] = {-1, 0, 1, 0};
    int y[4] = {0, 1, 0, -1};
    bool exist(vector<vector<char>>& board, string word) {
        int m = board.size(), n = board[0].size();
        vector<vector<bool>> visited(m, vector<bool>(n));
        for(int i = 0; i < m; ++i){
            for(int j = 0; j < n; ++j){
                if(board[i][j] == word[0])
                    dfs(board, word, visited, i, j, 1); // 1
                if(flag) return true;
            }
        }
        return false;
    }
    void dfs(vector<vector<char>>& board, string word, vector<vector<bool>>& visited, int i, int j, int index){
        if(index >= word.size()){
            flag = true;
            return;
        }
        visited[i][j] = true;
        
        for(int t = 0; t < 4; ++t){
            int new_i = i + x[t];
            int new_j = j + y[t];
            if(inArea(board, new_i, new_j) && board[new_i][new_j] == word[index] && !visited[new_i][new_j]) // 2
                dfs(board, word, visited, new_i, new_j, index + 1);
        }
        visited[i][j] =false;

    }

    bool inArea(vector<vector<char>>& board, int i, int j){
        return i >= 0 && i < board.size() && j >= 0 && j < board[0].size();
    }
};
```

## [84. 柱状图中最大的矩形](https://leetcode.cn/problems/largest-rectangle-in-histogram/)（困难-单调栈）×！×

```cpp
对于2 1 2  来说  添加了边界变成 0 2 1 2 0 （下面为了简化说明，指的都是元素对应的元素，而不是下标）
当栈内有 0 2 时， 1 准备入栈，发现不符合单调递增栈的性质；所以对于2 来说，以2为高度的最大矩形就已经确定，就是2；
然后1入栈，2入栈；栈内有0 1 2 ；
0入栈，2为高度的矩形确定；出栈； 
此时栈内还有0， 1 ；此时需要确定1为高度的矩形；
int curHeight = heights[st.top()]; st.pop();  // 1的下标出栈，这里能说明为什么必须获得了高度之后紧接着就要出栈，因为这样才能获得以1为高度的左端点的下标（因为现在栈内的0 1，原先它们之间是有个2的，只是以2为高度的矩形已经算完了，所以2出去了，但它比1大，所以可以用它作为左端点）
int left = st.top() + 1; // （因为是单调递增栈，如果0 1 是紧挨着，那么left就是1的下标（st.top() + 1）。如果原来它们之间还有元素，要么比1大，但是肯定已经出栈了，st.top() + 1 就是第一个大于1的元素的下标）

```

```c++
class Solution {
public:
    int largestRectangleArea(vector<int>& heights) {
        heights.insert(heights.begin(), 0);
        heights.push_back(0);
        int res = 0;
        stack<int> stk;
        for(int i = 0; i < heights.size(); ++i){
            while(!stk.empty() && heights[i] < heights[stk.top()]){
                int height = heights[stk.top()]; stk.pop();
                int left = stk.top() + 1;
                res = max(res, (i - left) * height);
            }
            stk.push(i);
        }
        return res;
    }
};
```

## [85. 最大矩形](https://leetcode.cn/problems/maximal-rectangle/)（困难-单调栈）×

![](https://pic.leetcode-cn.com/1662522483-OZdKKO-image.png)

```c++
/*
跟84解法一样，只是要将84的解法用m - 1次求出最大值。主要还是要夯实84题。
就是以每一行为x轴算出一个柱状图的vector<int>， 然后算出最大面积就可以了。
1：这里要注意，虽然从图里看最高的一层在顶上，但是从矩阵看它就是第一行，所以i,j都从0遍历。
2：如果当前是1，那么可以加上之前的高度，只要是0，那么就是0。
3：这里要注意不能用引用，因为解法中要对heights增加左右边界，所以要传值，不能改变heights的大小。
*/
```

```c++
class Solution {
public:
    int maximalRectangle(vector<vector<char>>& matrix) {
        int m = matrix.size(), n = matrix[0].size();
        int res = 0;
        vector<int> heights(n);
        for(int i = 0; i < m; ++i){ // 1
            for(int j = 0; j < n; ++j){
                if(matrix[i][j] == '1') heights[j] += matrix[i][j] - '0'; // 2
                else heights[j] = 0;
            }
            res = max(res, largestRectangleArea(heights));
        }
        return res;
    }

    int largestRectangleArea(vector<int> heights) { // 3
        heights.insert(heights.begin(), 0);
        heights.push_back(0);
        int res = 0;
        stack<int> stk;
        for(int i = 0; i < heights.size(); ++i){
            while(!stk.empty() && heights[i] < heights[stk.top()]){
                int height = heights[stk.top()]; stk.pop();
                int left = stk.top() + 1;
                res = max(res, (i - left) * height);
            }
            stk.push(i);
        }
        return res;
    }
};
```

## [94. 二叉树的中序遍历](https://leetcode.cn/problems/binary-tree-inorder-traversal/)（简单-二叉树）√√√

```c++
class Solution {
public:
    vector<int> res;
    vector<int> inorderTraversal(TreeNode* root) {
        inorder(root);
        return res;
    }
    void inorder(TreeNode* root){
        if(!root) return;
        inorder(root->left);
        res.push_back(root->val);
        inorder(root->right);
    }
};
```

## [96. 不同的二叉搜索树](https://leetcode.cn/problems/unique-binary-search-trees/)（中等）×！×

![](https://pic.leetcode-cn.com/a4d9d01db1e7abfcc3a047723b17bcb69ab9085cdf22d49955a34ba9d054ae85-image.png)

```c++
/*
动态规划！
思路：
如果整数1~n中的k作为根节点的值，则1~k-1会去构建左子树，k+1~n回去构建右子树，k作为根节点；
左子树出来的形态有a种，右子树出来的形态有b种，则整个树的形态有a ∗ b种；
以k为根节点的BST种类数 = 左子树BST种类数 * 右子树BST种类数

状态转移方程：用i个节点构建 BST，除去根节点，剩i−1个节点构建左、右子树，左子树分配0个，则右子树分配到i-1个...以此类推。

*/

class Solution {
public:
    int numTrees(int n) {
        vector<int> dp(n + 1);
        dp[0] = 1;
        dp[1] = 1;
        for(int i = 2; i <= n; ++i){
            for(int j = 0; j < i; ++j){
                dp[i] += dp[j] * dp[i - j - 1];
            }
        }
        return dp[n];
    } 
};
```

## [98. 验证二叉搜索树](https://leetcode.cn/problems/validate-binary-search-tree/)（中等-二叉树）××

```c++
class Solution {
public:
    TreeNode* preNode = nullptr;
    bool isValidBST(TreeNode* root) {
        if(!root) return true;
        if(!isValidBST(root->left)) return false; // ！！！！！！！！！！！！
        if(preNode && (root->val <= preNode->val)) return false;
        preNode = root;
        return isValidBST(root->right);

    }
};
```

## [101. 对称二叉树](https://leetcode.cn/problems/symmetric-tree/)（简单-二叉树）×√

递归

```c++
class Solution {
public:
    bool isSymmetric(TreeNode* root) {
        if(!root) return true;
        return DFS(root->left, root->right);
    }
    bool DFS(TreeNode* left, TreeNode* right){
        if(!left && !right) return true;
        if(!left || !right) return false;
        if(left->val != right->val) return false;
        return DFS(left->left, right->right) && DFS(left->right, right->left);
    }
};
```

迭代

```c++
class Solution {
public:
    bool isSymmetric(TreeNode* root) {
        if(!root->left && !root->right) return true;
        deque<TreeNode*> dque;
        dque.push_back(root->left);
        dque.push_back(root->right);
        while(!dque.empty()){
            TreeNode* first = dque.front();
            dque.pop_front();
            TreeNode* second = dque.front();
            dque.pop_front();
            if(!first && !second) continue;
            if(!first || !second) return false;
            if(first->val != second->val) return false;
            dque.push_back(first->left); 
            dque.push_back(second->right);
            dque.push_back(first->right);
            dque.push_back(second->left); 
        }
        return true;
    }
};
```

## [102. 二叉树的层序遍历](https://leetcode.cn/problems/binary-tree-level-order-traversal/)（中等-二叉树）×√√√

二叉树-层序遍历！
**就是忘了用dque.size()去判断每次出队的数量！！！！！！**
只需要定义一个vector<int> tmp; 然后每次循环后移动出去，**下次循环可以复用。有空看下移动时发生了什么。**

```c++
class Solution{
public:
	vector<vector<int>> levelOrder(TreeNode* root){
		vector<vector<int>> res;
		if(!root) return res;
		deque<TreeNode*> dque;
		dque.push_back(root);
		vector<int> tmp;
		while(!dque.empty()){
			int n = dque.size(); // !!!!!!!!!!!!!!!!!!!!
			for(int i = 0; i < n; ++i){
				TreeNode* node = dque.front();
				dque.pop_front();
				tmp.push_back(node->val);
				if(node->left) dque.push_back(node->left);
				if(node->right) dque.push_back(node->right);
			}	
			res.push_back(std::move(tmp));
		}
		return res;
	}
};
```

## [104. 二叉树的最大深度](https://leetcode.cn/problems/maximum-depth-of-binary-tree/)（简单-二叉树）√√√

```c++
class Solution {
public:
    int maxDepth(TreeNode* root) {
        if(!root) return 0;
        return max(maxDepth(root->left), maxDepth(root->right)) + 1;
    }
};
```

## [105. 从前序与中序遍历序列构造二叉树](https://leetcode.cn/problems/construct-binary-tree-from-preorder-and-inorder-traversal/)（中等-二叉树）×？

```c++
class Solution{
	TreeNode* traversal(vector<int>& inorder, vector<int>& preorder){
		if(preorder.size() == 0) return nullptr; // 递归出口
		// 前序遍历数组的第一个节点就是当前的根节点
		int rootValue = pretorder[0];
		TreeNode* root = new TreeNode(rootValue);
		if(preorder.size() == 1) return root; // 叶子节点  递归出口
		//找到中序遍历数组的切割点
		int delimiterIndex;
		for(delimiterIndex = 0; delimiterIndex < inorder.size(); ++delimiterIndex){
			if(inorder[delimiterIndex] == rootValue) break;
		}
		
		// 切割中序数组
		vector<int> leftInorder(inorder.begin(), inorder.begin() + delimiterIndex);
		vector<int> rightInorder(inorder.begin() + delimiterIndex + 1, inorder.end());

		// 舍弃前序数组的第一个元素，为了切割的左右前序数组的size与左右中序数组size一样。
		preorder.erase(preorder.begin());
		vector<int> leftPreorder(preorder.begin(), preorder.begin() + leftInorder.size());
		vector<int> rightPreorder(preorder.begin() + leftInorder.size(), preorder.end());
		root->left = traversal(leftInorder, leftPreorder);
		root->right = traversal(rightInorder, rightPreorder);
		return root;
	}
public:
	TreeNode* buildTree(vector<int>& preorder, vector<int>& inorder){
		return traversal(inorder, preorder);
	}
};
```

```c++
class Solution {
public:
    TreeNode* buildTree(vector<int>& preorder, vector<int>& inorder) {
        int pn = preorder.size(), in = inorder.size();
        TreeNode* root = build(preorder, inorder, 0, pn - 1, 0, in - 1);
        return root;
    }
    TreeNode* build(vector<int>& preorder, vector<int>& inorder, int ps, int pe, int is, int ie){
        if(ps > pe) return nullptr;
        TreeNode* root = new TreeNode(preorder[ps]);
        if(pe - ps == 0) return root;
        int midIndex = find(inorder.begin() + ps, inorder.begin() + pe + 1, preorder[ps]) - inorder.begin();
        
        int preLeftStartIndex = ps + 1, preLeftEndIndex = midIndex;
        int preRightStartIndex = midIndex + 1, preRightEndIndex = pe;

        int inLeftStartIndex = is, inLeftEndIndex = midIndex - 1;
        int inRightStartIndex = midIndex + 1, inRightEndIndex = ie;
        root->left = build(preorder, inorder, preLeftStartIndex, preLeftEndIndex, inLeftStartIndex, inLeftEndIndex);
        root->right = build(preorder, inorder, preRightStartIndex, preRightEndIndex, inRightStartIndex, inRightEndIndex);
        return root;
    }
};
```

## [114. 二叉树展开为链表](https://leetcode.cn/problems/flatten-binary-tree-to-linked-list/)（中等-二叉树）？

```c++

```

## [121. 买卖股票的最佳时机](https://leetcode.cn/problems/best-time-to-buy-and-sell-stock/)（简单-贪心）√√√

用贪心法就好，始终维护一个**今天之前**的最低股价，那么每天能获得最大利润就是在最低股价的那天买，在今天卖。（马后炮？是到了今天前才知道哪天的股价最低）。

最终结果就是每一天能获得的最大利润中的最大值。

```c++
class Solution {
public:
    int maxProfit(vector<int>& prices) {
        int res = 0;
        int minPrice = prices[0];
        for(int i = 1; i < prices.size(); ++i){
            res = max(res, prices[i] - minPrice);
            minPrice = min(minPrice, prices[i]);
            
        }
        return res;
    }
};
```

## [124. 二叉树中的最大路径和](https://leetcode.cn/problems/binary-tree-maximum-path-sum/)（困难-二叉树） ×

https://leetcode.cn/problems/binary-tree-maximum-path-sum/solution/shou-hui-tu-jie-hen-you-ya-de-yi-dao-dfsti-by-hyj8/

我不会的点在于，从哪儿出发都可以，这怎么解决？实在没想出来。

```c++

```

## [128. 最长连续序列](https://leetcode.cn/problems/longest-consecutive-sequence/)（中等-哈希）×！×√√

1. 首先肯定要用到集合，一个是去重，一个是计数（哈希）。
2. 统计连续元素的数量时，先判断一下e - 1 在不在集合中，在的话就continue；很好理解， 2 3 4 5 ； 当前是3的话，根本没必要统计，因为正确答案是2345。

**第二遍做，忘干净了完全没思路。废物**

```c++
class Solution {
public:
    int longestConsecutive(vector<int>& nums) {
        unordered_set<int> uset(nums.begin(), nums.end()); // 去重，因为后面想计数，重复就麻烦了
        int res = 0;
        for(auto e: uset){
            int c = 1; // 记录连续元素的数量
            if(!uset.count(e - 1)){
                while(uset.count(++e)) ++c;
            } 
            res = max(res, c);
        }
        return res;
    }
};
```

## [136. 只出现一次的数字](https://leetcode.cn/problems/single-number/)（简单-位运算） √√

很简单运用了异或的性质：

+ A^A = 0; 
+ 0和任何数字异或结果都是那个数字。

```c++
class Solution {
public:
    int singleNumber(vector<int>& nums) {
        int res = 0;
        for(auto e: nums){
            res ^= e;
        }
        return res;
    }
};
```

## [139. 单词拆分](https://leetcode.cn/problems/word-break/)（中等-回溯）×

```
if(wordSet.find(s.substr(startIndex, i-startIndex+1)) != wordSet.end() && backtracking(s, i + 1)){
这一样的意思是，假如是这样的用例：
"aaaa"  
["aaaa","aaa"]
当分离出"aaa"时，虽然在set里找到了aaa，但同时还要判断剩下的"a"行不行，行才能用当前的分割方式。
不行就i++，选中"aaaa"。
但遇到复杂的用例会超时，所以要用记忆化递归。
如下图：在1 - 3 时已经判断过以3为某个单词开头能否遍历完，但 2 - 3又要重复遍历，导致大量重复计算。
```

![](https://pic.leetcode-cn.com/5cba31457da78b75f3d593ef6f3c64c34e80db00c5e619f7e03affb1d6b829f0-image.png)



```cpp
/*
1：方便查找
2：记忆数组，dp[i]表示以i下标对应的字母作为某个单词的开头，直到字符串结束能否被正确拼接出来。0表示无法拼接出来，1表示能拼接出来，-1表示还未遍历。
3：startIndex表示起始位置，当 == s.size()时，说明0 ~ s.size() - 1都被正确拼接了。
4：如果被遍历过了，直接返回结果
5：以startIndex开始直到末尾都无法被拼接
*/
class Solution {
public:
    bool wordBreak(string s, vector<string>& wordDict) {    
        unordered_set<string> uset(wordDict.begin(),wordDict.end());  // 1
        vector<int> dp(s.size(), -1); // 2
        return backTracking(s, uset, dp, 0);
    }

    bool backTracking(string& s, unordered_set<string>& uset, vector<int>& dp, int startIndex){
        if(startIndex >= s.size()) return true; // 3
        if(dp[startIndex] != -1) return dp[startIndex]; // 4
        for(int i = startIndex; i < s.size(); ++i){
            if(uset.find(s.substr(startIndex, i - startIndex + 1)) != uset.end() && backTracking(s, uset, dp, i + 1)){
                dp[startIndex] = 1;
                return true;
            }
        }
        dp[startIndex] = 0; // 5
        return false;
    }
};
```



## [141. 环形链表](https://leetcode.cn/problems/linked-list-cycle/?favorite=2ckc81c)（简单-链表-快慢双指针）√√√

![环形链表](E:\拯救者\study\leetcode\img\环形链表.jpg)

**证明链表有环：**

假设链表有环，链表头节点到环入口节点口有m个节点，环的入口标号为0，环上一共有k + 1个节点，那么环的最后一个节点（指入口节点的上一个节点）标号为k，假设现在慢指针第一次来到了环的0号位置，那快指针必然已经停在环的某个节点上，那么则有：

x % （k + 1） = [2 * x + m]  % （k  + 1）

x % （k + 1） = [m + x  + x] % （k  + 1）

解释一下公式，背景是慢指针再环的0位置处，再走x步快慢指针相遇，等号左面好理解，等号右面，因为快指针是慢指针速度的两倍，因此当慢指针走到环的0位置处，快指针已经走了2 * m，因此快指针此时是停在m % (k + 1)位置处的。

然后看结论，只要存在m + x 是（k + 1）的整数倍，那么两个指针就可以相遇。

**再证明如何找到环的入口节点：**

由上面的结论可以得知，**相遇的条件是m + x 是（k + 1）的整数倍**，因此快慢指针相遇的时候，慢指针是在环的x位置处的，再走m步会到达 (x + m) % （k + 1） == 0的位置，也就是还的入口处。但是我们不知道m是几，因此我们就再定义一个慢指针2从链表的头节点跟环中的慢指针同时出发，当相遇的时候，慢指针2已经走了m步到达了入口节点，慢指针1也会停在在环的入口节点。

```c++
class Solution {
public:
    bool hasCycle(ListNode *head) {
        ListNode* slow = head, *fast = head;
        while(fast && fast->next != nullptr){
            slow = slow->next;
            fast = fast->next->next;
            if(slow == fast) return true;
        }
        return false;
    }
};
```

## [142. 环形链表 II](https://leetcode.cn/problems/linked-list-cycle-ii/)（中等-链表）×

```C++
class Solution {
public:
    ListNode *detectCycle(ListNode *head) {
        ListNode* slow1 = head, *slow2 = head, *fast = head;
        while(true){
            if(fast == nullptr || fast->next == nullptr) return nullptr;
            fast = fast->next->next;
            slow1 = slow1->next;
            if(fast == slow1) break;
        } 
        while(slow1 != slow2){
            slow1 = slow1->next;
            slow2 = slow2->next;
        }
        return slow2;
    }
};
```





## [146. LRU 缓存](https://leetcode.cn/problems/lru-cache/)（中等-模拟题(算法)）××

- 首先，要实现一个双向链表，只是链表节点不只是一个node，要带key、val;
- 其次，要实现一个删除表尾（因为缓存有大小，超过了要掉最久没使用的）的功能、将任意节点放到链表头的功能（刚用过的要往前放）；
- 实现一个put、get，这两个都是针对哈希表的；
- 哈希表的val是DLinkNode*类型，因为无论是插入节点还是修改节点，都涉及移动该节点在原链表的位置。所以把真正的val存储在链表节点中；

**为什么要用双向链表?**

+ 根据题目要求，每次put都往链表头插（**满足时间复杂度O(1)的要求**），这样链表尾节点就是最近最久未使用的；
+ 最近使用过某节点（也就是get()）后，就把该节点移动到链表头；这样链表尾节点就是最近最久未使用的；
+ put()正常往链表头插时间复杂度就是O(1)，当超过capacity时，直接删掉最后一个节点再插入，复杂度也是O(1)。
+ 因为如果插入节点后达到了缓存容量，那么就要删除尾节点，此时必须要找到倒数第二个节点并修改它的next指针，所以必须是双向的。（用单链表并且定义一个指针一直指向倒数第二个节点也不行，因为删了最后一个节点还是需要再去找上一个节点，所以必须要用双向链表）。

**为什么要使用unordered_map?**

+ 因为根据题意，get()的时间复杂度要为O(1)，所以很自然的想到用哈希表。

+ 无论用get()还是push访问一个节点时，都涉及移动节点，因此val为DLinkedNode*；

**其他细节**

+ 链表节点既要存储val，也要存储key。因为当删除尾节点时，unordered_map也要删除对应的key-value，所以要通过存储在链表节点中的key来删除。
+ 尾节点也是个节点，不能简单定义成指针，要不然**当链表为空的时候添加新节点**还要特殊处理去修改尾指针的指向，得不偿失。

```c++
#include <iostream>
#include <unordered_map>
using namespace std;

struct DLinkNode{
    int key_, val_;
    DLinkNode* next_;
    DLinkNode* prev_;
    DLinkNode(int key = 0, int val = 0): key_(key), val_(val), next_(nullptr), prev_(nullptr){}
};

class LRUCache{
public:
    LRUCache(int cap): m_cap(cap), m_size(0){
        head_ = new DLinkNode(-1);
        tail_ = new DLinkNode(-1);
        head_->next_ = tail_;
        tail_->prev_ = head_;
    }
    void push_front(DLinkNode* node){
        node->next_ = head_->next_;
        node->next_->prev_ = node;
        head_->next_ = node;
        node->prev_ = head_;
    }
    void rm_node(DLinkNode* node){
        node->prev_->next_ = node->next_;
        node->next_->prev_ = node->prev_;
    }
    DLinkNode* rm_tail(){
        DLinkNode* tail = tail_->prev_;
        rm_node(tail);
        return tail;
    }
    void put(int key, int val){
        if(m_umap.find(key) == m_umap.end()){
            DLinkNode* node = new DLinkNode(key, val);
            m_umap[key] = node;
            push_front(node);
            if(++m_size > m_cap){
                DLinkNode* removed = rm_tail();
                m_umap.erase(removed->key_);
                delete removed;
            }
        }else{
            DLinkNode* node = m_umap[key];
            m_umap[key]->val_ = val;
            rm_node(node);
            push_front(node);
        }
    }

    int get(int key){
        if(m_umap.find(key) == m_umap.end()) return -1;
        else{
            DLinkNode* node = m_umap[key];
            rm_node(node);
            push_front(node);
            return node->val_;
        }
    }

    DLinkNode* head_;
    DLinkNode* tail_;
    int m_cap;
    int m_size;
    unordered_map<int, DLinkNode*> m_umap;
};
```



## [148. 排序链表](https://leetcode.cn/problems/sort-list/)（中等-排序）×

归并递归：

![](https://pic.leetcode-cn.com/8c47e58b6247676f3ef14e617a4686bc258cc573e36fcf67c1b0712fa7ed1699-Picture2.png)

```c++
//有个疑问就是为什么找中点不能这么写：
ListNode* findMid(ListNode* head){
    ListNode* slow = head, *fast = head;
    while(fast != nullptr && fast->next != nullptr){
        fast = fast->next->next;
        slow = slow->next;
    }
    return slow;
}
// 因为当只剩4，2的时候，fast还能向前移动，slow指向2，那么就永远也分不完了
// 所以只要涉及到找链表的中点，那就永远记住就用那种办法。
```

```c++
class Solution {
public:
    ListNode* sortList(ListNode* head) {
        return mergeSort(head);
    }
    ListNode* mergeSort(ListNode* head){
        // 终止条件 空节点或者只有一个节点
        if(!head || !head->next) return head;
        // 1 寻找切割点 也就是找中点
        ListNode* mid = findMid(head);
        ListNode* nextHead = mid->next;
        mid->next = nullptr;
        // 2 递归
        ListNode* left = mergeSort(head); 
        ListNode* right = mergeSort(nextHead);
        // 3 合并
        ListNode* newHead = merge(left, right);
        return newHead;
    }
    
    ListNode* findMid(ListNode* head){
        ListNode* slow = head, *fast = head;
        while(fast->next != nullptr && fast->next->next != nullptr){
            fast = fast->next->next;
            slow = slow->next;
        }
        return slow;
    }
    ListNode* merge(ListNode* l1, ListNode* l2){
        ListNode* preHead = new ListNode, *tail = preHead;
        while(l1 && l2){
            if(l1->val < l2->val){
                tail->next = l1;
                l1 = l1->next;
            }else{
                tail->next = l2;
                l2 = l2->next;
            }
            tail = tail->next;
        }
        if(l1) tail->next = l1;
        else tail->next = l2;
        return preHead->next;
    }

};
```

归并排序，采取迭代的方法：

![](https://pic.leetcode-cn.com/1659328927-oGEPtf-%E6%8E%92%E5%BA%8F%E9%93%BE%E8%A1%A81.png)

![](https://pic.leetcode-cn.com/1659328938-LIpnHX-%E6%8E%92%E5%BA%8F%E9%93%BE%E8%A1%A82.png)

```c++
class Solution {
public:
    ListNode* sortList(ListNode* head) {
        ListNode* preHead = new ListNode(-1, head);
        // 1 获取链表的长度
        int len = 0;
        ListNode* curr = head;
        while(curr != nullptr){
            ++len;
            curr = curr->next;
        }
        // 假设 1 2 3 4 
        // step == 1 时， 
        for(int step = 1; step < len; step *= 2){
            ListNode* tail = preHead;
            curr = preHead->next;
            while(curr != nullptr){
                ListNode* left = curr;
                ListNode* right = cut(left, step); // left 指向 1 ；right指向2 3 4 
                curr = cut(right, step); // right指向2 ； curr指向3 4 
                tail->next = merge(left, right); // 将1 2 合并
                while(tail->next != nullptr) tail = tail->next; // tail指向2
            }
        }
        return preHead->next;
    }
    // 将链表从start开始切掉前step个元素，返回后一个元素
    // 举例  1 2 3 4 假设start = 1， step = 2，那么将返回3
    ListNode* cut(ListNode* start, int step){
        --step;
        while(start != nullptr && step > 0){ 
            start = start->next;
            --step;
        }
        if(start == nullptr) return nullptr;
        else{
            ListNode* next = start->next;
            start->next = nullptr;
            return next;
        }
    }
    ListNode* merge(ListNode* l1, ListNode* l2){
        ListNode* preHead = new ListNode, *tail = preHead;
        while(l1 && l2){
            if(l1->val < l2->val){
                tail->next = l1;
                l1 = l1->next;
            }else{
                tail->next = l2;
                l2 = l2->next;
            }
            tail = tail->next;
        }
        if(l1) tail->next = l1;
        else tail->next = l2;
        return preHead->next;
    }
};
```

**快排：**

```c++
/**
思路
1 为了防止恶心的用例，将取链表的中心为基准点，将mid->val 和 head->val 交换；
2 lhead作为小于pivot的链表的头插法指针 utail作为大于等于pivot部分的尾插法的指针
3 因为utail不一定指向的是原链表的表尾节点，所以要修改它的next指向
4 递归处理前半部分
5 递归处理后半部分
6 每次返回的是排好序的链表的头节点
*/

class Solution {
public:
    ListNode* sortList(ListNode* head) {
        return quickSort(head, nullptr);
    }
    ListNode* quickSort(ListNode* head, ListNode* end){
        if(head == end || head->next == end) return head;
        ListNode* mid = findMid(head, end); 
        swap(mid->val, head->val); // 1
        ListNode* lhead = head, *utail = head, *curr = head->next; // 2
        while(curr != end){
            ListNode* next = curr->next;
            if(curr->val < head->val){
                curr->next = lhead;
                lhead = curr;
            }else{
                utail->next = curr;
                utail = curr;
            }
            curr = next;
        }
        utail->next = end; // 3 
        ListNode* node = quickSort(lhead, head); // 4
        head->next = quickSort(head->next, end); // 5
        return node; // 6
    }
    ListNode* findMid(ListNode* head, ListNode* end){
        ListNode* slow = head, *fast = head;
        while(fast->next != end && fast->next->next != end){
            fast = fast->next->next;
            slow = slow->next;
        }
        return slow;
    }
};
```







## [152. 乘积最大子数组](https://leetcode.cn/problems/maximum-product-subarray/)（中等-动态规划）√√√

因为存在正负号，所以要维护两个dp状态。

```c++
class Solution {
public:
    int maxProduct(vector<int>& nums) {
        // dp[i] 不能单纯由dp[i-1]推出，因为有正负号，所以要有最大最小两个状态
        // dpMax[i] 表示包括nums[i]在内的最大乘积
        vector<int> dpMax(nums.size());
        vector<int> dpMin(nums.size());
        dpMax[0] = nums[0];
        dpMin[0] = nums[0];
        int res = dpMax[0];
        for(int i = 1; i < nums.size(); ++i){
            // 有可能和之前乘积大，也有可能自己大
            dpMax[i] = max(max(nums[i] * dpMax[i-1], nums[i] * dpMin[i-1]), nums[i]);
            dpMin[i] = min(min(nums[i] * dpMax[i-1], nums[i] * dpMin[i-1]), nums[i]);
            res = max(res, dpMax[i]);
        }
        return res;
    }
};
```

## [155. 最小栈](https://leetcode.cn/problems/min-stack/)（中等-设计题（栈））×

[k神写的好](https://leetcode.cn/problems/min-stack/solution/min-stack-fu-zhu-stackfa-by-jin407891080/)

主要是维护一个单调不减栈。

比如 2 1 3 1   

minStk:2 1 3(跳过) 1 .

当stack出栈时，先判断一下主栈的栈顶元素是不是等于minStk的栈顶元素，是的话同时出栈。

```c++
class MinStack {
public:
    stack<int> stk;
    stack<int> minStk;
    MinStack() {}
    void push(int val) {
        stk.push(val);
        if(minStk.empty() || val <= minStk.top())
            minStk.push(val);
    }
    
    void pop() {
        int top = stk.top();
        stk.pop();
        if(top ==  minStk.top()) minStk.pop();
    }
    
    int top() {
        return stk.top();
    }
    
    int getMin() {
        return minStk.top();
    }
};


```

## [160. 相交链表](https://leetcode.cn/problems/intersection-of-two-linked-lists/)（简单-链表）××√√

注意循环条件：while(ra != rb)， 因为有交点那么就会提前相遇，没有交点也会同时为空节点，所以返回ra就是正确答案。	

```
不能写成:
ra = ra->next ? ra->next : headB;
rb = rb->next ? rb->next : headA;
因为ra rb 永远不会都等于nullptr，无限循环
```

```C++
class Solution {
public:
    ListNode *getIntersectionNode(ListNode *headA, ListNode *headB) {
        ListNode* ra = headA, *rb = headB;
        while(ra != rb){
            ra = ra ? ra->next : headB;
            rb = rb ? rb->next : headA;
        }
        return ra;
    }
};
```

## [169. 多数元素](https://leetcode.cn/problems/majority-element/)（简单-投票法）×√√√

同归于尽法，也叫投票法？
假设有100个士兵（nums），他们分属不同的阵营（阵营编号为nums[i]），他们依次冲向一个阵地并想占领阵地。

1. 首先第一个冲上阵地的是nums[0]阵营的士兵，阵地上士兵数量count为1；
2. 如果第i个士兵冲上阵地发现没有士兵，那么他所属的阵营就占领了阵地，count = 1；
3. 如果阵地已经被占领，如果后面冲上来的士兵还是属于阵营nums[0]的；那么++count（占领士兵+1）;
4. 如果第i个士兵冲上阵地发现有其他阵营的士兵，那么他会冲上去与其中一个士兵同归于尽（--count；阵地上的士兵数量-1）。

```c++
​```c++
class Solution {
public:
    int majorityElement(vector<int>& nums) {
        int res = nums[0]; // 1
        int cnt = 1;
        for(int i = 1; i < nums.size(); ++i){
            if(cnt == 0){ // 2
                res = nums[i];
                ++cnt;
            }else{
                if(nums[i] == res) ++cnt; // 3
                else --cnt;
            }
        }
        return res;
    }
};
```

## 198. [打家劫舍](https://leetcode.cn/problems/house-robber/)（中等-动态规划）√√√

```CPP
class Solution {
public:
    int rob(vector<int>& nums) {
        if(nums.size() == 1) return nums[0];
        vector<int> dp(nums.size());
        dp[0] = nums[0];
        dp[1] = max(nums[0], nums[1]);
        for(int i = 2; i < nums.size(); ++i){
            dp[i] = max(dp[i-2] + nums[i], dp[i-1]);
        }
        return dp[nums.size() - 1];
    }
};
```





## [200. 岛屿数量](https://leetcode.cn/problems/number-of-islands/)(中等-DFS) √×√

就是之前做对了现在又错了我有啥招。逻辑搞清啊，什么时候能进递归，逻辑要清晰。

```c++
class Solution {
public:
    int dx[4] = {1, 0, -1, 0};
    int dy[4] = {0, 1, 0, -1};
    int m, n;
    int numIslands(vector<vector<char>>& grid) {
        int res = 0;
        m = grid.size(), n = grid[0].size();
        vector<vector<bool>> visited(m, vector<bool>(n));
        for(int i = 0; i < m; ++i){
            for(int j = 0; j < n; ++j){
                if(grid[i][j] == '1' && !visited[i][j]){
                    ++res;
                    dfs(grid, visited, i, j);
                }
            }
        }
        return res;
    }
    void dfs(vector<vector<char>>& grid, vector<vector<bool>>& visited, int i, int j){
        if(!inArea(grid, i, j) || grid[i][j] != '1' || visited[i][j]) return;
        visited[i][j] = true;
        for(int t = 0; t < 4; ++t)
            dfs(grid, visited, i + dx[t], j + dy[t]);
    
    }

    bool inArea(vector<vector<char>>& grid, int i, int j){
        return i >= 0 && i < m && j >= 0 && j < n;
    }
};
```

## [207. 课程表](https://leetcode.cn/problems/course-schedule/)（中等-拓扑排序-DFS）×!×

```c++
/*
主旨思路就是：
    首先生成一个邻接表。注意邻接表的结构，下标是前置课程，数组是学了前置课程才能学的后置课程。
    首先就是要以每门课为起始，去DFS判断是否到能学到的任意一门课程都有没有环，为什么要以每门课为起始都判断一遍？因为假设有10门课，可能其中3门有环，另外7门无环，如果只从7门中的一门判断出无环，是无法得出那3门课也无环的，所以必须以每门课为起点都DFS一遍。
1 邻接表
2 作为标志。默认：0； 1：已经遍历过； -1：从当前课程各种递归都是无环的；
3 传入邻接表， flags, 起始课程
4 flags[course] == 1， 已遍历过，说明有环， 返回false
5 flags[course] == -1，说明从当前课程各种递归都是无环的
6 标记为遍历过
7 DFS正确返回了， 说明从当前课程各种递归都是无环的，flags置为-1
*/
class Solution {
public:
    bool canFinish(int numCourses, vector<vector<int>>& prerequisites) {
        vector<vector<int>> adjcent(numCourses);
        for(int i = 0; i < prerequisites.size(); ++i)
            adjcent[prerequisites[i][1]].push_back(prerequisites[i][0]); // 1
        vector<int> flags(numCourses); // 2
        for(int i = 0; i < numCourses; ++i){
            if(!DFS(adjcent, flags, i)) return false; // 3
        }
        return true;
    }

    bool DFS(vector<vector<int>>& adjcent, vector<int>& flags, int course){
        if(flags[course] == 1) return false; // 4
        if(flags[course] == -1) return true; // 5
        flags[course] = 1; // 6
        for(auto e: adjcent[course]){
            if(!DFS(adjcent, flags, e)) return false; 
        }
        flags[course] = -1; // 7
        return true;
    }   
};
```

## [208. 实现 Trie (前缀树)](https://leetcode.cn/problems/implement-trie-prefix-tree/)（中等）×

https://leetcode.cn/problems/implement-trie-prefix-tree/solution/trie-tree-de-shi-xian-gua-he-chu-xue-zhe-by-huwt/

```c++
class Trie {
public:
    // 不是节点存储值，而是"边"代表值
    // 没有边通向根节点，也不是一个单词的结束
    bool isEnd = false;
    //当某一个next不为空，说明某个字母存在
    vector<Trie*> next;
    Trie():next(26, nullptr) {}
    
    void insert(string word) {
        Trie *node = this;
        for(auto c: word){
            if(node->next[c - 'a'] == nullptr)
                node->next[c - 'a'] = new Trie();
            // 这里要注意！！不能写成node = next[c - 'a'];
            // 因为这样一直用的是根节点的next！
            node = node->next[c - 'a'];
        }
        node->isEnd = true;
    }
    
    bool search(string word) {
        Trie *node = this;
        for(auto c: word){
            if(!node->next[c - 'a']) return false;
            node = node->next[c - 'a'];
        }
        return node->isEnd;
    }
    
    bool startsWith(string prefix) {
        Trie *node = this;
        for(auto c: prefix){
            if(!node->next[c - 'a']) return false;
            node = node->next[c - 'a'];
        }
        return true;
    }
    // 拓展一手，实现一个代码补全：感觉就是一个简单回溯
    void complete(string prefix){
        trie *node = this;
        for(auto c: prefix){
            if(node->next[c - 'a'] == nullptr) return;
            node = node->next[c - 'a'];
        }
    }
};

/**
 * Your Trie object will be instantiated and called as such:
 * Trie* obj = new Trie();
 * obj->insert(word);
 * bool param_2 = obj->search(word);
 * bool param_3 = obj->startsWith(prefix);
 */
```

## [215. 数组中的第K个最大元素](https://leetcode.cn/problems/kth-largest-element-in-an-array/)（中等-快排）×

```C++
/*
说下借助partiton+减治这种办法的时间复杂度：
首先，每次借助partition我们都能找到序列中第k个数，其中在它前面的都小于他，在它后面的都大于他，每次划分的时间复杂度是T(n);
然后我们使用随机选择的办法，使之后的划分的平均时间复杂度为T(n)、T(n/4)。什么意思呢？假设有3个元素，那么随机选取到每个数字作为pivot的概率就是1/3，那么数学期望(随机变量的平均取值)就是1 * 1/3 + 2 * 1/3 + 3 * 1/3 = 2，也就是多次随机取pivot，最终取到的平均值就是中间的元素。因此每次partition都能将数组分为两半，所以下一次划分的时间复杂度是上一次的一半。
T(n) + T(n/2) + ... 1,当n为无穷大时，极限为2T(n), 时间复杂度为O(n)。
这种快排的一次递归的时间复杂度为：T(n) = n + T(n/2)
正常快排的时间复杂度为：T(n) = n + 2T(n/2)
*/
class Solution {
public:
    int partition(vector<int>& nums, int l, int r){
        int pivotIndex = l + rand() % (r - l + 1);
        swap(nums[l], nums[pivotIndex]);
        int i = l, j = r;
        while(i < j){
            while(i < j && nums[j] >= nums[l]) --j;
            while(i < j && nums[i] <= nums[l]) ++i;
            swap(nums[i], nums[j]);
        }
        swap(nums[i], nums[l]);
        return i;
    }
    int findKthLargest(vector<int>& nums, int k) {
        int n = nums.size(), l = 0, r = n - 1;
        int mid = partition(nums, l, r);
        while(mid != n - k){
            if(mid > n - k){
                r = mid - 1;
                mid = partition(nums, l, r);
            } 
            else{
                l = mid + 1;
                mid = partition(nums, l, r);
            } 
        }
        return nums[mid];
    }
};
```

## [221. 最大正方形](https://leetcode.cn/problems/maximal-square/)（中等-动态规划）×

![](https://pic.leetcode.cn/1666660261-smlLFj-image.png)

```c++
/*
求正方形面积，只要求出正方形的边长即可，因为s = n * n;
故转移方程为dp[i][j] = min(min(dp[i-1][j], dp[i][j-1]), dp[i - 1][j - 1]) + 1;
1：dp[i][j]表示以matrix[i][j]为正方形右下角点，最大边长是多大
2: 边界初始化；这题不好这么初始化，因为res的初值不方便设定，如果1只存在第一行或第一列，那么res都要特殊置1，所以不方便，故循环变量都从0开始，在循环中处理边界。

*/

class Solution {
public:
    int maximalSquare(vector<vector<char>>& matrix) {
        int res = 0, m = matrix.size(), n = matrix[0].size();
        vector<vector<int>> dp(m, vector<int>(n)); // 1
        /* 2
        for(int j = 0; j < n; ++j) dp[0][j] = matrix[0][j] == '1' ? 1 : 0;
        for(int i = 0; i < m; ++i) dp[i][0] = matrix[i][0] == '1' ? 1 : 0; 
        */
        for(int i = 0; i < m; ++i){
            for(int j = 0; j < n; ++j){
                if(matrix[i][j] == '1'){
                    if(i == 0 || j == 0) dp[i][j] = 1; // 2
                    else dp[i][j] = min(min(dp[i-1][j], dp[i][j-1]), dp[i - 1][j - 1]) + 1;
                }
                res = max(res, dp[i][j]);
            }
        }
        return res * res;
    }
};


```



## [226. 翻转二叉树](https://leetcode.cn/problems/invert-binary-tree/)（简单-二叉树）√√√

```c++
class Solution {
public:
    TreeNode* invertTree(TreeNode* root) {
        if(!root) return root;
        swap(root->left, root->right);
        invertTree(root->left);
        invertTree(root->right);
        return root;
    }
};
```

## [234. 回文链表](https://leetcode.cn/problems/palindrome-linked-list/)（简单-链表）√√

找中点->翻转后半部分->对比。

用back作为截至条件：假设1221，mid = 3 ；

前半段：1 2 2 ，后半段：1 2 

```c++
class Solution {
public:
    bool isPalindrome(ListNode* head) {
        if(!head) return true;
        ListNode* mid = findMidNode(head);
        ListNode* back = reverse(mid);
        while(back){
            if(head->val != back->val) return false;
            back = back->next;
            head = head->next;
        }
        return true;
    }

    ListNode* findMidNode(ListNode* head){
        ListNode* slow = head, *fast = head;
        while(fast && fast->next){
            slow = slow->next;
            fast = fast->next->next;
        }
        return slow;
    }
    ListNode* reverse(ListNode* head){
        ListNode* pre = nullptr;
        while(head){
            ListNode* next = head->next;
            head->next = pre;
            pre = head;
            head = next;
        }
        return pre;
    }
};
```

## [236. 二叉树的最近公共祖先](https://leetcode.cn/problems/lowest-common-ancestor-of-a-binary-tree/)（中等-二叉树-后序遍历）××

后序遍历！

1.	如果left和right都空，说明当前root不是p、q的祖先；
2.	如果left空 但right不空，说明right的子树中可能只有p或者只有q ；或者p、q都有；
3.	如果right空 但left不空，说明left的子树中可能只有p或者只有q；或者p、q都有；
4.	如果left和right都不空，说明root正好就是最近公共祖先。

```c++
class Solution {
public:
    TreeNode* lowestCommonAncestor(TreeNode* root, TreeNode* p, TreeNode* q) {
        if(!root || root == p || root == q) return root;
        TreeNode* left = lowestCommonAncestor(root->left, p, q);
        TreeNode* right = lowestCommonAncestor(root->right, p, q);
        if(!left && !right) return nullptr; // 1
        if(!left) return right; // 2
        if(!right) return left; // 3
        return root; // 4
    }
};
```

## [238. 除自身以外数组的乘积](https://leetcode.cn/problems/product-of-array-except-self/)（中等-双指针）××

双指针，解法很妙又很朴素！

```java
原数组：       [1       2       3       4]
左部分的乘积：   1       1      1*2    1*2*3
右部分的乘积： 2*3*4    3*4      4      1
结果：        1*2*3*4  1*3*4   1*2*4  1*2*3*1
```

```c++
class Solution {
public:
	vector<int> productExceptSelf(vector<int>& nums) {
		vector<int> res(nums.size(), 1);
		int left = 1, right = 1;
		for(int i = 0, j = nums.size() - 1; i < nums.size(); ++i, --j){
			res[i] *= left;
            res[j] *= right;
            left *= nums[i];
            right *= nums[j];
		}
		return res;
	}
};
```



## [239. 滑动窗口最大值](https://leetcode.cn/problems/sliding-window-maximum/)（困难-滑动窗口-单调队列）×××

```c++
/*
1 用一个单调递减队列（应该是单调不增），也就是que[i] >= que[i + 1]    4 3 3 2 为什么不能是严格单调递减呢？ 因为第二个3可能是下一个窗口里的最大值，不能随便不入队。
2 因为每次窗口移动的时候，要把l出队，把r入队。
    比如 1 3 -1 -3，一开始队列中有3 -1
    然后窗口准备右移，要把1出队，但是1是不在队列里的，3才是队头
    所以就不用出队了。
    
*/
class Solution {
public:
    // 单调递减队列 1 
    template<typename T>
    struct myQueue{
        deque<T> dque;
        T front(){ return dque.front(); }
        void push(T x){
            while(!dque.empty() && x > dque.back()) dque.pop_back();
            dque.push_back(x);
        }
        void pop(T x){ // 2
            if(!dque.empty() && dque.front() == x)
                dque.pop_front();
        }
    };
    vector<int> maxSlidingWindow(vector<int>& nums, int k) {
        vector<int> res;
        myQueue<int> que;
        for(int i = 0; i < k; ++i)
            que.push(nums[i]);
        res.push_back(que.front());
        for(int i = k; i < nums.size(); ++i){
            que.pop(nums[i - k]);
            que.push(nums[i]);
            res.push_back(que.front());
        }
        return res;
    }
};
```

[240. 搜索二维矩阵 II](https://leetcode.cn/problems/search-a-2d-matrix-ii/)（中等-二分）×√√√

先用一下抽象成BST的方法。第二次做再用一下二分（就是对每一行用一次二分）。

![](https://pic.leetcode-cn.com/1617066993-AyRIiF-image.png)

```c++
class Solution {
public:
    bool searchMatrix(vector<vector<int>>& matrix, int target) {
        int m = matrix.size(), n = matrix[0].size();
        int r = 0, c = n - 1;
        while(r < m && c >= 0){
            if(matrix[r][c] > target) --c;
            else if(matrix[r][c] < target) ++r;
            else return true;
        }
        return false;
    }
};
```

二分：

每行用一次二分，找到了就返回true。

```c++
class Solution {
public:
    bool searchMatrix(vector<vector<int>>& matrix, int target) {
        int m = matrix.size(), n = matrix[0].size();
        bool flag = false;
        for (int i = 0; i < m; i++) {
            int l = 0, r = n - 1;
            while (l <= r) {
                int mid = l + (r - l) / 2;
                if (matrix[i][mid] == target){
                    flag = true;
                    break; // 这里要break！！！！！要不然会无限循环！！！
                }
                else if (matrix[i][mid] < target) l = mid + 1;
                else r = mid - 1;
            }
            if (flag) return true;
        }
        return false;
    }
};
```

## [253. 会议室 II](https://leetcode.cn/problems/meeting-rooms-ii/)（中等）××

**核心思路：所需的最少的会议室数量，可以转化为同一时间内举办的会议数量的最大值。用一个优先队列记录同时进行的会议数量。**

为什么一定要先按start排序？举个例子[[2, 4], [7, 10]]:

如果排了序，那么能保证1会议一定比0会议开的晚，如果7再大于4，那么两个会议一定不冲突，也就是2会议后开，并且还是在1结束之后开的。

如果不排序呢？[[7, 10]，[2, 4] ]因为我们是假定2会议比1晚开的，所以当2 < 10时；我们误以为2会议比1会议晚开，然后2又小于10，我们又误以为会议2跟会议1冲突了，实际上根本没冲突，导致错误。

```c++
/*
1：首先要按照会议的起始时间从大到小排序
2：优先队列保存的是排序后会议的下标，并且是按会议结束时间从小到大排序的小根堆；优先队列中保存的都是时间冲突的会议，
    也就是需要会议室的数量。
3：因为排了序，所以intervals[i]一定是晚开始的，如果当前会议开始时，队头的会议已经结束，那么队头出队，会议室被intervals[i]接替。队列里只记录同时举行的会议的数量（某一时刻至少所需会议室的数量）。

intervals = [[0,30],[5,10],[15,20]]
1(会议)一定比0晚开始，但是1开始时0还没结束，所以冲突了，入队；
此时2又开始了，2应该跟0去对比还是跟1去对比(这就涉及到用大根堆还是用小根堆的问题)？
很明显2应该跟1去对比，因为你开会的话肯定先去看哪个会议室的会议更早结束！因为跟0冲突了不代表跟1也冲突；跟1不冲突说明2可以接替1的会议室；
！终于理解了为什么会议1可以出队，因为当会议2入队时，说明此时的时刻就为15-20，会议1已经结束了，后面的会议无需跟会议1去对比！

*/
class Solution {
public:
    int minMeetingRooms(vector<vector<int>>& intervals) {
        int res = 0;
        sort(intervals.begin(), intervals.end(), [](vector<int>& lhs, vector<int>& rhs){
            return lhs[0] < rhs[0];
        }); // 1
        priority_queue<int, vector<int>, function<bool(int, int)>> pque([&intervals](int l, int r){
            return intervals[l][1] > intervals[r][1];
        }); // 2
        for(int i = 0; i < intervals.size(); ++i){
            while(!pque.empty() && intervals[pque.top()][1] <= intervals[i][0]) // 3
                pque.pop();
            pque.push(i);
            res = max(res, (int)pque.size());
        }
        return res;
    }
};
```

## [279. 完全平方数](https://leetcode.cn/problems/perfect-squares/)（中等-背包问题）?

动态规划-背包问题

什么是背包问题？简单来说，就是dp[i]不能由dp[i-1]推出，而是和dp[0 -  i-1]都有关系。



```C++

class Solution {
public:
    int numSquares(int n) {
        vector<int> m_vec;
        for(int i = 1; i * i <= n; ++i)
            m_vec.push_back(i * i);
        int m = m_vec.size();
        vector<vector<int>> dp(m, vector<int>(n + 1));
        for(int j = 1; j <= n; ++j) 
            dp[0][j] = j;
        for(int i = 1; i < m; ++i){
            for(int j = 0; j <= n; ++j){
                if(m_vec[i] > j) dp[i][j] = dp[i-1][j];
                else dp[i][j] = dp[i- 1][j - m_vec[i]] + 1;
            }
        }
        return dp[m - 1][n];

    }
};
```



```c++
class Solution {
public:
    int numSquares(int n) {
        vector<int> dp(n + 1, INT_MAX); // dp[i]表示和为i的完全平方数的最少数量
        dp[0] = 0; // 0的最少数量为0
        for(int i = 1; i <= n; ++i){
            for(int j = 1; j * j <= i; ++j){
                dp[i] = min(dp[i], dp[i - j * j] + 1);
            }
        }
        return dp[n];
    }
};
```

## [283. 移动零](https://leetcode.cn/problems/move-zeroes/)（简单-循环不变量）√√√

就是规定[0, notZero]为非0数。为了满足条件，notZero初始化为-1。

去遍历不等于0的元素，往前换。

```c++
class Solution {
public:
    void moveZeroes(vector<int>& nums) {
        int notZero = -1;
        for(int i = 0; i < nums.size(); ++i){
            if(nums[i] != 0)
                swap(nums[++notZero], nums[i]);
        }
    }
};
```

## [287. 寻找重复数](https://leetcode.cn/problems/find-the-duplicate-number/)（中等）×

原地哈希：

```c++
class Solution {
public:
    int findDuplicate(vector<int>& nums) {
        for(int i = 0; i < nums.size(); ++i){
            while(nums[i] != nums[nums[i] - 1]) swap(nums[i], nums[nums[i] - 1]);
        }
        for(int i = 0; i < nums.size(); ++i){
            if(nums[i] != i + 1) return nums[i];
        }
        return 0;
    }
};
```

不修改数组的办法：

```c++
/*
假设数组有n + 1个数，都在[1, n]范围内，假设没有重复数，那么数组就只有n个数，与假设矛盾，因此至少有一个重复数。
假设数组有n个数，都在[1, n]范围内，假设没有重复数，那么用例为 n = 4, nums = {2, 4, 3, 1}
    那么用寻找环形链表的入口节点的办法来模拟，我们从下标0出发，根据nums(i)计算出一个值，以这个值为新的下标，再用nums(i)计算，以此类推产生一个类似链表一样的序列。
    那么从下标0开始 2 -> 3 -> 1 -> 4 -> 越界
假设数组有n + 1个数，都在[1, n]范围内，那么就必定存在环，用例为n = 4, nums = {2, 4, 3, 1, 2}
    那么从下标0开始 2 -> 3 -> 1 -> 4 -> 3 -> 1 ... 
*/

class Solution {
public:
    int findDuplicate(vector<int>& nums) {
        int n = nums.size(), slow1 = 0, slow2 = 0, fast = 0;
        while(true){
            fast = nums[nums[fast]];
            slow1 = nums[slow1];
            if(fast == slow1) break;
        }
        while(slow1 != slow2){
            slow1 = nums[slow1];
            slow2 = nums[slow2];
        }
        return slow1;
    }
};
```







## [300. 最长递增子序列](https://leetcode.cn/problems/longest-increasing-subsequence/)（中等-动态规划-子序列问题）×√√

这个题不是背包问题，因为并没有一个背包大小。

时间复杂度为n*logn的还没想。

https://leetcode.cn/problems/longest-increasing-subsequence/solution/zui-chang-shang-sheng-zi-xu-lie-dong-tai-gui-hua-2/

```C++
class Solution {
public:
    int lengthOfLIS(vector<int>& nums) {
        int res = 1;
        vector<int> dp(nums.size(), 1);
        for(int i = 1; i < nums.size(); ++i){
            for(int j = 0; j <= i; ++j){
                if(nums[i] > nums[j]){
                    dp[i] = max(dp[i], dp[j] + 1);
                }
                res = max(res, dp[i]);
            }
        }
        return res;
    }
};
```

## [309. 最佳买卖股票时机含冷冻期](https://leetcode.cn/problems/best-time-to-buy-and-sell-stock-with-cooldown/)（中等-动态规划）？



## [322. 零钱兑换](https://leetcode.cn/problems/coin-change/)（中等-背包问题）？



## [337. 打家劫舍 III](https://leetcode.cn/problems/house-robber-iii/)（中等-动态规划）？



## [338. 比特位计数](https://leetcode.cn/problems/counting-bits/)（简单-动态规划）√√

```c++
class Solution {
public:
    vector<int> countBits(int n) {
        if(!n) return {0};
        vector<int> dp(n + 1, 0);
        dp[1] = 1;
        for(int i = 2; i <= n; ++i){
            if(i % 2 == 0) dp[i] = dp[i / 2];
            else dp[i] = dp[i - 1] + 1;
        }
        return dp;
    }
};
```





## [347. 前 K 个高频元素](https://leetcode.cn/problems/top-k-frequent-elements/)（中等-优先队列）×√

```c++
class Solution {
public:
    vector<int> topKFrequent(vector<int>& nums, int k) {
        vector<int> res;
        unordered_map<int, int> umap;
        for(auto e: nums) ++umap[e];
        priority_queue<int, vector<int>, function<bool(int, int)>> pque([&umap](int l, int r){
            return umap[l] > umap[r];
        });
        for(auto &p: umap){
            pque.push(p.first);
            if(pque.size() > k) pque.pop();
        }
        while(k--){
            res.push_back(pque.top());
            pque.pop();
        }
        return res;
    }
};
```





## [394. 字符串解码](https://leetcode.cn/problems/decode-string/)（中等-栈）××

```c++
/*
1：cnt是[]内字符串重复的个数，res是 num[] 的前缀；
2：当遇到‘[’时，前缀和cnt入栈；cnt清0重新记录；
3：当遇到‘]’时，出栈一对cnt和前缀，加上cnt个res，因为res记录的就是要重复的字符串。
*/

class Solution {
public:
    string decodeString(string s) {
        stack<pair<int, string>> stk;
        int cnt = 0; string res; // 1
        for(int i = 0; i < s.size(); ++i){
            if(s[i] >= 'a' && s[i] <= 'z') res += s[i];
            if(s[i] >= '0' && s[i] <= '9') cnt = cnt * 10 + s[i] - '0';
            if(s[i] == '['){ // 2
                stk.push(make_pair(cnt, move(res)));
                cnt = 0;
            }
            if(s[i] == ']'){ // 3
                int n = stk.top().first;
                string prefix = move(stk.top().second);
                stk.pop();
                for(int i = 0; i < n; ++i) prefix += res;
                res = move(prefix);
            }

        }
        return res;
    }
};
```

## [399. 除法求值](https://leetcode.cn/problems/evaluate-division/)（中等-模拟题）？







## [406. 根据身高重建队列](https://leetcode.cn/problems/queue-reconstruction-by-height/)（贪心）×

先简单说一下本题是个啥意思，就是people这个数组中，某个人的ki，也就是它前面比他高的人数，和他的下标是不对应的， 因此我们要将这些人重新排队。

本题有两个维度，h和k，**看到这种题目一定要想如何确定一个维度，然后在按照另一个维度重新排列**。

其实如果大家认真做了135. 分发糖果，就会发现和此题有点点的像。

在135. 分发糖果我就强调过一次，**遇到两个维度权衡的时候，一定要先确定一个维度，再确定另一个维度**。



所以在本题中，显然应该先对身高进行从大到小排序，然后只需要按照k为下标重新插入队列就可以，以图中{5 2}为例：

![](https://pic.leetcode-cn.com/1631929525-ZFbGfU-file_1631929522702)

而对于{6 1}来说，就算他被{5 2}挤到后面的位置，也不影响，因为{5 2}比他矮，不影响{6 1}前面比他高的人数。

另外，用vector频繁的在中间插入会非常耗时，因此先选用list存储。

```c++
class Solution {
public:
    vector<vector<int>> reconstructQueue(vector<vector<int>>& people) {
        sort(people.begin(), people.end(), [](vector<int>& lhs, vector<int>& rhs){
            return lhs[0] == rhs[0] ? lhs[1] < rhs[1] : lhs[0] > rhs[0];
        });
        list<vector<int>> que;
        for(int i = 0; i < people.size(); ++i){
            int position = people[i][1];
            auto it = que.begin();
            while(position--)
                ++it;
            que.insert(it, people[i]);
        }
        
        return vector<vector<int>>(que.begin(), que.end());
    }
};

```



## [416. 分割等和子集](https://leetcode.cn/problems/partition-equal-subset-sum/)（中等-动态规划-背包问题）√√×

给你一个 **只包含正整数** 的 **非空** 数组 `nums` 。请你判断是否可以将这个数组分割成两个子集，使得两个子集的元素和相等。

```
输入：nums = [1,5,11,5]
输出：true
解释：数组可以分割成 [1, 5, 5] 和 [11] 。
```

```c++
/* 
自己分析：假设第一个子集的总和为target，那么第二个自己的总和就是total - target
那么 target = total / 2
total / 2 一定是整数，否则无法分开
进而，就转化为了0-1背包问题。物品的重量为nums[i]，背包容量为target，问背包能否被正好装满。
dp[i][j] 从前i个物品里任选某几个物品，能否正好容量为j的背包。
*/

class Solution {
public:
    bool canPartition(vector<int>& nums) {
        int m = nums.size(), total = accumulate(nums.begin(), nums.end(), 0);
        if(total % 2 == 1) return false;
        int target = total / 2;
        // dp[i][j] 容量为j的背包，从前i个物品里任选某几个物品，能装载的最大重量。当j==target时，如果dp[i][j] == target，返回true
        vector<vector<bool>> dp(m, vector<bool>(target + 1));
        for(int j = nums[0]; j < dp[0].size(); ++j) // 注意这里j的初始值！！！！！要从第一个物品的重量开始才能放下，不能放下的话，dp值都为0！！！
            if(j == nums[0]) dp[0][j] = true;
        for(int i = 1; i < m; ++i){
            for(int j = 0; j <= target; ++j){
                if(nums[i] > j)
                    dp[i][j] = dp[i - 1][j];
                else
                    dp[i][j] = dp[i - 1][j] || dp[i - 1][j - nums[i]];
            }
        }
        return dp[m - 1][target];
    }
};
```

**滚动数组** √

```C++
/*
1 这一行不能有，假设用例为[100],那么bag为50，但是dp[100]就已经越界了，所以不用这么初始化第一行，dp[0] = true; 其实就已经初始化了第一行。
2 注意啊不是大于i，是大于nums[i]。是大于物品，不是大于物品的下标。
3 注意啊如果dp[j]已经是true了说明已经有能装满的方案了不需要再置为false了。
*/
class Solution {
public:
    bool canPartition(vector<int>& nums) {
        int n = nums.size(), total = accumulate(nums.begin(), nums.end(), 0);
        if(total % 2) return false;
        int bag = total / 2;
        vector<bool> dp(bag + 1);
        dp[0] = true;
        // dp[nums[0]] = true;  1
        for(int i = 0; i < n; ++i){ // 遍历物品
            for(int j = bag; j >= nums[i]; --j){ // 遍历背包 2 
                dp[j] = dp[j] || dp[j - nums[i]];  // 3
            }
        }
        return dp[bag];
    }
};
```



## [437. 路径总和 III](https://leetcode.cn/problems/path-sum-iii/)（中等-二叉树路径）×

就是递归遍历每个节点，在每个节点上再用一次dfs。

```c++
class Solution {
public:
    int res = 0;
    int pathSum(TreeNode* root, int targetSum) {
        if(!root) return 0;
        dfs(root, targetSum);
        pathSum(root->left, targetSum);
        pathSum(root->right, targetSum);
        return res;
    }
    void dfs(TreeNode* root, long targetSum){
        if(!root) return;
        // if(root->val > targetSum) return;  这一行不能有，因为节点的值有正有负，就算当前节点值>target,后面也有可能被负数给抵消
        targetSum -= root->val;
        if(targetSum == 0) ++res;
        dfs(root->left, targetSum);
        dfs(root->right, targetSum);
    }
};
```





## [438. 找到字符串中所有字母异位词](https://leetcode.cn/problems/find-all-anagrams-in-a-string/)（中等-滑动窗口-哈希表）×√

跟567题差不多，只是这个多了一个条件：记录每个排列的起始坐标。

这道题有两个关键点：

+ 用一个哈希表记录在s中需要出现的字符的次数；
+ 如果用两个哈希表，那就判断两个哈希表是否相等；如果就一个哈希表，那就判断所有键对应的值是否为0（也就是都出现了）；
+ 另外由于是找和p对应的子串，因此本题的滑动窗口大小是固定的。 

```c++
class Solution {
public:
    vector<int> findAnagrams(string s, string p) {
        int ns = s.size(), np = p.size();
        vector<int> res;
        unordered_map<char, int> umap;
        for(int i = 0; i < np; ++i){
            ++umap[p[i]];
            --umap[s[i]];
        }
        function<bool(unordered_map<char, int>&)> zero([](unordered_map<char, int>& umap)->bool{
            for(auto p: umap)
                if(p.second != 0) return false;
            return true;
        });
        for(int l = 0, r = np - 1; r < s.size();){
            if(zero(umap)) res.push_back(l);
            ++umap[s[l]];
            ++l;
            ++r;
            --umap[s[r]];
        }
        return res;
    }
};
```



## [448. 找到所有数组中消失的数字](https://leetcode.cn/problems/find-all-numbers-disappeared-in-an-array/)（简单-原地哈希）××

```c++
/*
while(nums[i] != i + 1){swap(nums[i], nums[nums[i] - 1]);} 
这样写是错的，举个例子：{1, 2, 3, 4, 5, 2, 7, 8}
当i == 6时，nums[6](2) != i + 1 (7), 所以就会和nums[1]交换，但nums[1]也是2，所以会无限循环。
因此，我们要这样判断：
while(nums[i] != nums[nums[i] - 1]) swap(nums[i], nums[nums[i] - 1]);
	判断nums[nums[i] - 1](下标==1这个位置)放的是不是nums[i]（2），如果不是，就把这个2换过去。
	然后一定要用while，直到换过来的数字就是当前位置想要的，就不换了。
	
1 注意这里题目说要返回在范围内但没有出现在nums中的数字，所以要返回i + 1.
*/
class Solution {
public:
    vector<int> findDisappearedNumbers(vector<int>& nums){
        vector<int> res;
        for(int i = 0; i < nums.size(); ++i){
            while(nums[i] != nums[nums[i] - 1]) swap(nums[i], nums[nums[i] - 1]);
        }
        for(int i = 0; i < nums.size(); ++i){
            if(nums[i] != i + 1) res.push_back(i + 1); // 1
        }
        return res;
    }
};
```





## [461. 汉明距离](https://leetcode.cn/problems/hamming-distance/)（简单-位运算）√

我的错误解法，不用两个一起右移再去比，直接先x和y异或，再判断1的个数就好了，因为异或为1就说明不相同。

```cpp
class Solution {
public:
    int hammingDistance(int x, int y) {
        int res = 0;
        while(x || y){
            if((x & 1) ^ (y & 1)){
                ++res;
            }
            x = x >> 1;
            y = y >> 1;
        }
        return res;
    }
};
```

正解：

```cpp
class Solution {
public:
    int hammingDistance(int x, int y) {
        int z = x ^ y, res = 0;
        while(z){
            res += z & 1;
            z >>= 1;
        }
        return res;
    }
};
```



## [494. 目标和](https://leetcode.cn/problems/target-sum/)（中等-动态规划-背包问题）××

初始化时一定要注意！！！！！！！！！！！！！！！！！因为当nums[0]为0时，去初始化背包大小为0的dp，初始化为2！！！！！！！！

因为+0，-0，都是0！！！！上来就是两种！！

```cpp
/*
假设加法的总和为x，那么减法的总和就是total - x,因为x + (total - x) = total。
进而有，x - (total - x) = target -> x = (total + target) / 2
(total + target) / 2 必须是整数。
此时问题就转化为，背包为容量为x，石头重量为nums[i]， dp[i][j]表示从前i个数字中选某几个石头装满容量为j的背包有几种方法。

*/

class Solution {
public:
    int findTargetSumWays(vector<int>& nums, int target) {
        int m = nums.size(), total = accumulate(nums.begin(), nums.end(), 0);
        if(abs(target) > total) return 0;
        if((total + target) % 2 == 1) return 0;
        int bagSize = (total + target) / 2;
        vector<vector<int>> dp(m, vector<int>(bagSize + 1, 0));
        for(int i = 0; i < dp.size(); ++i)
            dp[i][0] = 1;
        for(int j = 0; j < dp[0].size(); ++j)
            if(j == nums[0])
                dp[0][j] = j == 0 ? 2 : 1;
        for (int i = 1; i < m; ++i) {
            for(int j = 0; j <= bagSize; ++j) {
                if(j < nums[i]) dp[i][j] = dp[i - 1][j];
                else
                    dp[i][j] = dp[i - 1][j] + dp[i - 1][j - nums[i]];
            }
        }
        return dp[m - 1][bagSize];
    }
};
```

**滚动数组**

```c++
class Solution {
public:
    int findTargetSumWays(vector<int>& nums, int target) {
        int m = nums.size(), total = accumulate(nums.begin(), nums.end(), 0);
        if(abs(target) > total) return 0;
        int tmp = total + target;
        if(tmp % 2 == 1) return 0;
        int bagSize = tmp / 2;
        vector<int> dp(bagSize + 1); // dp[j]表示和为j的表达式数量
        dp[0] = 1;
        for(int i = 0; i < m; ++i){
            for(int j = bagSize; j >= 0; --j){
                if(j >= nums[i])
                    dp[j] += dp[j - nums[i]]; // 注意这个转移方程，dp[j] = 不用nums[i]的组合数，以及用huns[i]的组合数
            }
        }
        return dp[bagSize];
    }
};
```





## [538. 把二叉搜索树转换为累加树](https://leetcode.cn/problems/convert-bst-to-greater-tree/)（中等-二叉树）？





## [543. 二叉树的直径](https://leetcode.cn/problems/diameter-of-binary-tree/)（简单-二叉树）？

```c++

```









## [560. 和为 K 的子数组](https://leetcode.cn/problems/subarray-sum-equals-k/) （中等-前缀和）×！×

一开始想用滑动窗口去做，但是发现行不通，如果都是正数的话是没问题的，但是因为有负数，所以当用例为[-1, -1, 1], k = 0时；当窗口慢慢变大，sum却在变小；所以窗口只能一直变大，**left没有机会右移**，找不到[-1, 1]这个子数组。

```C++
class Solution {
public:
    int subarraySum(vector<int>& nums, int k) {
        if(k == 0) return 0;
        int res = 0;
        int sum = 0;
        for(int l = 0, r = 0; r < nums.size(); ++r){
            sum += nums[r];
            while(sum > k){
                sum -= nums[slow];
                ++l;
            }
            if(sum == k){
                ++res;
            }
            else
                continue;
        }
        return res;
    }
};
```

所以我认为（不一定对）：当滑动窗口变大时，sum必须单调递增（或者说结果集必须变大），才能使用滑动窗口来解。否则尝试使用**前缀和**。

**前缀和-普通解法：**

```c++
class Solution {
public:
    int subarraySum(vector<int>& nums, int k) {
        vector<int> preSum(nums.size() + 1); // preSum[i]为前i个数的和
        int count = 0;
        for(int i = 0; i < nums.size(); ++i){
            preSum[i+1] = preSum[i] + nums[i];
        }
        for(int j = 1; j <= nums.size(); ++j){
            for(int i = 0; i < j; ++i){
                if(preSum[j] - preSum[i] == k)
                    count++;
            }
        }
        return count;
    }
};
```

超时了，因为时间复杂度是n方，用哈希表优化。

**用哈希表优化**

```c++
// 每新获得一个前缀和presum[j]，那就从presum[0:j]中查找presum[i] == presum[j] - k 的个数
for (int j = 1; j <= N; j++) {
    for (int i = 0; i < j; i++) {
        if (k == presum[j] - presum[i]) {
            count++;
        }
    }
}
```

```c++
/*
1 前缀和-前缀和出现的次数;
2 必须有 preSum[0] = 1;
3 关键就是这里 如果找到了，说明 存在一个前缀和a 使得 sum - k = a也就是sum - a = k; 那么就找到了一个符合条件的子数组，在a和sum之间。
4 更新前缀和的次数；这行不能放在3前面，否则如果要求k = 0 的话，那presum[j] - presum[i] if i == j 肯定等于0；
或者说如果先更新了sum的次数，那么就相当于上面普通解法的k == presum[j] - presum[i]；假设k==0，那得出的也是空的子数组，不符合题意的。
*/
class Solution{
public:
    int subarraySum(vector<int>& nums, int k) {
        int count = 0;
        int sum = 0;
        unordered_map<int, int> presum; // 1
        presum[0] = 1; // 2
        for(int e: nums){
            sum += e;
             // 其实就是在找sum - a == k ?
            if(presum.find(sum - k) != presum.end()) // 3
                count += presum[sum - k];            
            ++presum[sum]; // 4
        }
        return count;
    }
};
```



## [581. 最短无序连续子数组](https://leetcode.cn/problems/shortest-unsorted-continuous-subarray/)（双指针）×

![](https://pic.leetcode-cn.com/1600691648-ZCYlql-%E5%BE%AE%E4%BF%A1%E6%88%AA%E5%9B%BE_20200921203355.png)

我们可以假设把这个数组分成三段，`左段`和`右段`是标准的升序数组，`中段`数组虽是无序的，但满足最小值大于`左段`的最大值，最大值小于`右段`的最小值。

那么我们目标就很明确了，找中段的左右边界，我们分别定义为begin 和 end;分两头开始遍历:

+ 从左到右维护一个最大值max,在进入右段之前，那么遍历到的nums[i]都是小于max的，我们要求的end就是遍历中最后一个小于max元素的位置；
+ 同理，从右到左维护一个最小值min，在进入左段之前，那么遍历到的nums[i]也都是大于min的，要求的begin也就是最后一个大于min元素的位置。

```c++
class Solution {
public:
    int findUnsortedSubarray(vector<int>& nums) {
        int n = nums.size();
        int max = nums[0], min = nums[n - 1];
        int end = -1, start = 0;

        for(int i = 0; i < n; ++i){
            // 这个if else 是有说法的，必须这么写，因为当数组原本就有序的时候，要确保end和start不变
            if(nums[i] < max) end = i; 
            else max = nums[i];
        }
        for(int i = n - 1; i >= 0; --i){
            if(nums[i] > min) start = i;
            else min = nums[i];
        }
        return end - start + 1;
    }
};

```



## [617. 合并二叉树](https://leetcode.cn/problems/merge-two-binary-trees/)（简单-二叉树）？



## [621. 任务调度器](https://leetcode.cn/problems/task-scheduler/)（中等）？







## [647. 回文子串](https://leetcode.cn/problems/palindromic-substrings/)（中等-双指针）√

```c++
class Solution {
public:
    int res = 0;
    int countSubstrings(string s) {
        int n = s.size();
        for(int i = 0; i < n; ++i){
            func(s, i, i);
            func(s, i, i + 1);    
        }
        return res;
    }
    void func(string& s, int i, int j){
        while(i >= 0 && j < s.size()){
            if(s[i] == s[j]){
                ++res;
                --i;
                ++j;
            }
            else break;
        }
    }
};
```





## [739. 每日温度](https://leetcode.cn/problems/daily-temperatures/)（中等-单调栈）√×

说来搞笑，这个题一开始我是只能想到暴力解的，然后我去看题解，先看到一个栈字，我在想跟栈有什么关系？又看到单调栈，然后一瞬间就会做了。
主要是脑子里没建立起一个哈希映射，到底什么样的题能用到单调栈？
**现在我还想不到答案，按我自己做过几道单调栈的题目的经验来说，一般题目是一维数组，需要用O(n2)的暴力解来做，都可以试试单调栈（不一定能用上，但是要往这想想）。**

**思路：**
用个单调递减栈，假设temperatures为[3,2,1,4];

+ 3，2，1符合单调递减，肯定依次入栈，当i为3时，发现4大于栈顶，那么对于1来说，它的下一个最高温度就是第i = 3天，1出栈；
+ 此时栈顶为1，4仍然大于2；因为是单调递减的，2和4之间要么原来没有元素，要么有元素也肯定比2小。所以2的下一个最大温度就是3 - 1 = 2天后。
+ 最后栈内剩下的元素都是递减的，并且没有比他们高的温度的日期，又因为res的所有元素默认初始化为0，所以不用处理，直接返回。
+ **需要强调的是栈内存储的是元素的下标！不是元素！切记！**

```cpp
/*
第二遍做的时候突然忘了怎么维护栈是单调递减的了，后来才反应过来：
假设用例是5 3 4；那么到4的时候发现大于栈顶，所以就要进while处理一下把3出栈；然后发现4小于5，所以4就可以入栈了。也就是说，每次入栈的时候一定确保当前元素是小于栈顶的，过程是动态的。
1 这里千万要注意，单调栈存的是下标！！！！
*/

class Solution {
public:
    vector<int> dailyTemperatures(vector<int>& temperatures) {
        vector<int> res(temperatures.size());
        stack<int> stk;
        for(int i = 0; i < temperatures.size(); ++i){
            while(!stk.empty() && temperatures[i] > temperatures[stk.top()]){ // 1
                int dayIndex = stk.top(); stk.pop();
                res[dayIndex] = i - dayIndex;
            }
            stk.push(i);
        }
        return res;
    }
};
```





## 👨‍💻 LeetCode 精选 TOP 面试题

https://leetcode.cn/problem-list/2ckc81c/?sorting=W3sic29ydE9yZGVyIjoiREVTQ0VORElORyIsIm9yZGVyQnkiOiJGUkVRVUVOQ1kifV0%3D





## [3. 无重复字符的最长子串](https://leetcode.cn/problems/longest-substring-without-repeating-characters/)（中等） ×！√

滑动窗口！

1. 首先要判断slow需不需要更新，怎么判断？slow = max(umap[s[fast]], slow);什么意思，就是说：**umap[s[fast]]的意义是，为了在slow和fast之间不出现重复字符，slow能处于的最左位置。**如图，当j第一次到d，将umap[s[j]] = j + 1; 当再次遇到d时，slow就要判断一下，slow是不是比umap[s[j]]大，如果不是，就更新到它。
2. 必须要先处理slow，也就是说，假定umap[s[fast]]已经设置好了。当fast走到重复子串的位置时，更新一下slow，怎么更新呢？就是更新到fast给slow提前安排好的位置。

1. **fast到一个位置，就判断一下是否要更新slow，如何更新？用它给slow提前安排好的位置更新；**
2. **计算slow和fast的距离；**
3. **更新umap[s[fast]];**

不可能先更新umap[s[fast]]，再更新slow。本质上就是fast先往前走，走一步判断一下slow是否要前移。

![](https://pic.leetcode-cn.com/8b7cac826e572c65f8b77e0f380eaa93ab665857a8e916bc4ea36b7765eafc55-%E5%9B%BE%E7%89%87.png)

```c++
class Solution {
public:
    int lengthOfLongestSubstring(string s) {
        int res = 0;
        unordered_map<char, int> umap;
        for(int slow(0),fast(0); fast < s.size(); ++fast){
            slow = max(umap[s[fast]], slow);
            res = max(res, fast - slow + 1);
            umap[s[fast]] = fast + 1;
        }
        return res;
    }
};
```

## [18. 四数之和](https://leetcode.cn/problems/4sum/)（中等）√

双指针。

关键在于去重，i要去一次重，j也要去一次重。

```c++
class Solution {
public:
    vector<vector<int>> fourSum(vector<int>& nums, int target) {
        int n = nums.size();
        sort(nums.begin(), nums.end());
        vector<vector<int>> res;
        for(int i = 0; i < n - 3; ++i){
            if(i && nums[i] == nums[i-1]) continue; // ！！！！！
            for(int j = i + 1; j < n - 2; ++j){
                int x = nums[i], y = nums[j];
                if(j != i + 1 && nums[j] == nums[j - 1]) continue; // ！！！
                for(int slow(j + 1), fast(n - 1); slow < fast;){
                    long s = (long)x + y + nums[slow] + nums[fast];
                    if(s < target) ++slow;
                    if(s > target) --fast;
                    if(s == target){
                        res.push_back({x, y, nums[slow], nums[fast]});
                        for(++slow; slow < fast && nums[slow] == nums[slow - 1]; ++slow);
                        for(--fast; slow < fast && nums[fast] == nums[fast + 1]; --fast);
                    }
                }
            }
        }
        return res;
    }
};
```





## [14. 最长公共前缀](https://leetcode.cn/problems/longest-common-prefix/)（简单-模拟题）×

+ 令最长公共前缀 ans 的值为第一个字符串，进行初始化；
+ 遍历后面的字符串，依次将其与 ans 进行比较，两两找出公共前缀，最终结果即为最长公共前缀；
+ 如果查找过程中出现了 ans 为空的情况，则公共前缀不存在直接返回；

![](https://pic.leetcode-cn.com/08a7d664e59901fc8c66a7cb7272838e3989bc6fdd250e37479dfef084e25925-frame_00001.png)

```c++
class Solution {
public:
    string longestCommonPrefix(vector<string>& strs) {
        string res = strs[0];
        for(int i = 1; i < strs.size(); ++i){
            int j = 0;
            for(; j < res.size() && j < strs[i].size(); ++j){
                if(res[j] != strs[i][j]) break;
            }
            res = res.substr(0, j);
            if(res.size() == 0) return res;
        }
        return res;
    }
};
```

## [7. 整数反转](https://leetcode.cn/problems/reverse-integer/) ××

```c++
/*
不能统一成正数处理，因为INT_MAX 和INT_MIN的绝对值不一样；
res == INT_MAX / 10 && e > INT_MAX % 10 其实这个是不用判断的。假设这个成立，那么说明x有那么多位数且高位是大于2的，那么x就溢出了，与题意不相符。
*/
class Solution {
public:
    int reverse(int x) {
        int res = 0;
        while(x){
            int e = x % 10;
            if(res > INT_MAX / 10 || (res == INT_MAX / 10 && e > INT_MAX % 10) || res < INT_MIN / 10 || (res == INT_MIN / 10 && e < INT_MIN % 10))
                return 0;
            res = res * 10 + e;
            x /= 10; // 不要忘了这里！！！！！！
        }
        return res;
    }
};
```



## [217. 存在重复元素](https://leetcode.cn/problems/contains-duplicate/)（简单） √

```c++
class Solution {
public:
    bool containsDuplicate(vector<int>& nums) {
        sort(nums.begin(), nums.end());
        for(int i = 0; i < nums.size() - 1; ++i){
            if(nums[i] == nums[i + 1]) return true;
        }
        return false;
    }
};
```



## [13. 罗马数字转整数](https://leetcode.cn/problems/roman-to-integer/)（简单）×

summary：就是没想到处理4、9、40、90这些特殊情况。其实很简单，就是遍历s的时候，如果当前数字比下一个数字小，那么就减去当前数字，否则就加。

```c++
class Solution {
public:
    int romanToInt(string s) {
        unordered_map<char, int> umap{{'I', 1}, {'V', 5},{'X', 10}, {'L', 50}, {'C', 100}, {'D', 500}, {'M', 1000}};
        int res = 0;
        int i = 0, n = s.size();
        while(i < n - 1){
            if(umap[s[i]] < umap[s[i + 1]])
                res -= umap[s[i]];
            else res += umap[s[i]];
            ++i;
        }
        res += umap[s[i]];
        return res;
    }
};
```

## [88. 合并两个有序数组](https://leetcode.cn/problems/merge-sorted-array/)（简单）√√√

```c++
class Solution {
public:
    void merge(vector<int>& nums1, int m, vector<int>& nums2, int n) {
        int ns = m + n - 1;
        for(--m, --n; m >= 0 && n >= 0; --ns){
            if(nums1[m] > nums2[n]){
                nums1[ns] = nums1[m];
                --m;
            }
            else{
                nums1[ns] = nums2[n];
                --n;
            }
        }
        for(int i = 0; i <= n; ++i)
            nums1[i] = nums2[i];
    }
};
```



## [8. 字符串转换整数 (atoi)](https://leetcode.cn/problems/string-to-integer-atoi/)（中等）×

summary：

判断正负号那里，只能用if-else，不能用if-if，因为只能取一个符号，读入的下一个如果还是正负号那么直接忽略所有后面的字符并返回0；

res == INT_MAX / 10 && num > INT_MAX % 10  这个判断，如果num是7，没溢出；

是8，正数溢出了，截断，INT_MIN个位数是8，没溢出，但是直接截断也正好。

```c++
class Solution {
public:
    int myAtoi(string s) {
        int res = 0, index = 0, n = s.size();
        bool flag = true;
        while(index < n && s[index] == ' ') ++index; // 消除前导空格
        if(index < n && s[index] == '-'){ // 判断正负号 
            flag = false;
            ++index;
        }else if(index < n && s[index] == '+') ++index; // 如果有正有负 那么就跳过一个 确保异常情况能输出0

        while(index < n &&  s[index] >= '0' && s[index] <= '9'){
            int num = s[index] - '0';
            if(res > INT_MAX / 10 || res == INT_MAX / 10 && num > INT_MAX % 10)  // 不能只判断res > INT_MAX，因为有可能再*10直接溢出，必须提前截断
                return flag ? INT_MAX : INT_MIN;
            res = res * 10 + num;
            ++index;
        }
        return flag ? res : -res;
    }
};
```

## [26. 删除有序数组中的重复项](https://leetcode.cn/problems/remove-duplicates-from-sorted-array/)（简单）√√

```c++
class Solution {
public:
    int removeDuplicates(vector<int>& nums) {
        int slow = 0, fast = 0, n = nums.size();
        for( ; fast < n; ++fast){
            if(nums[fast] != nums[slow]){
                ++slow;
                nums[slow] = nums[fast];
            }
        }
        return slow + 1;
    }
};
```



## [131. 分割回文串](https://leetcode.cn/problems/palindrome-partitioning/)（中等）×！

回溯中中的i，就是数组的字符串的下标。

一开始是 a b b       开始回溯 变成 a bb 

![](https://pic.leetcode-cn.com/1615074249-jcNHLL-image.png)

```c++
class Solution {
public:
    vector<string> path; 
    vector<vector<string>> res; 
    void backtracking(string &s, int startIndex){
        if(startIndex >= s.size()){ // 一条分割的路径搜索完毕，分割的结果都是回文串
            res.push_back(path);
            return;
        }
        for(int i = startIndex; i < s.size(); ++i){
            if(isPalindrome(s, startIndex, i)){
                path.push_back(s.substr(startIndex, i - startIndex + 1));
                backtracking(s, i + 1);
                path.pop_back(); 
                //比如['a', 'a', 'b'], 此时start=2，i=2; backtracking(s, 3), 说明当前path中的结果都是回文串，直接return；
                // 回溯之前要把'b'弹出， 回到start=1，++i -> 2，判断‘ab’是不是回文串。
            }
            // 如果不是回文串，整个分支直接跳过，不搜索了，不会继续往下递归            
        }
    }
   // 记忆化搜索中，f[i][j] = 0 表示未搜索，1 表示是回文串，-1 表示不是回文串
    bool isPalindrome(const string& s, int l, int r) {
        while(l < r) if(s[l++] != s[r--]) return false;
        return true;
    }
    vector<vector<string>> partition(string s) {
        backtracking(s, 0); // 回溯
        return res;
    }
};
```

## [54. 螺旋矩阵](https://leetcode.cn/problems/spiral-matrix/)（中等-模拟-矩阵）××√√

```c++
/*
1：注意往下时x是增大的；
2：转弯是由t来控制的， 当tmpX和tmpY越界或者遍历已经遍历过的位置时，t就更新，x和y也更新；
*/
class Solution {
public:
    vector<int> spiralOrder(vector<vector<int>>& matrix) {
        vector<int> res;
        int m = matrix.size(), n = matrix[0].size();
        int ns = m * n;
        int dx[] = {0, 1, 0, -1}; // 1
        int dy[] = {1, 0, -1, 0};
        int i = 0;
        int x = 0, y = 0, tmpX = 0, tmpY = 0, t = 0;
        for(int i = 0; i < ns; ++i){
            res.push_back(matrix[x][y]);
            matrix[x][y] = INT_MAX;
            tmpX = x + dx[t];
            tmpY = y + dy[t];
            if(tmpX < 0 || tmpX >= m || tmpY < 0 || tmpY >= n || matrix[tmpX][tmpY] == INT_MAX){ // 2
                t = (t + 1) % 4;
                tmpX = x + dx[t];
                tmpY = y + dy[t]; 
            }
            x = tmpX;
            y = tmpY;
        }
        return res;
    }
};
```

## [202. 快乐数](https://leetcode.cn/problems/happy-number/)（简单）×

```c++
class Solution {
public:
    int func(int n){
        int res = 0;
        while(n != 0){
            res += (n % 10) * (n % 10);
            n /= 10;
        }
        return res;
    }
    bool isHappy(int n) {
        unordered_set<int> umap;
        int res = func(n);
        while(umap.count(res) == 0){
            if(res == 1) return true;
            umap.insert(res);
            res = func(res);
        }
        return false;
    }
};
```







## [41. 缺失的第一个正数](https://leetcode.cn/problems/first-missing-positive/)（困难）×！

1. 最关键的一点是，n = nums.size()，res只能是[1， n+1]中的一个，因为假设n为3，数组中的元素为1，2，3；那么res就是4；否则就是缺失的那一个。

2. 所以说，只要把每个元素i，放在nums[i-1]的位置，然后再遍历一遍，找到nums[i] != i + 1；就找到缺失的正数了。

3. 解释一下为什么是while：

   ```
   nums[i] >= 1 && nums[i] <= n && nums[i] != nums[nums[i] - 1
   比如用例是3，4，-1，1；如果是if：
   首先判断3(nums[0]) 在不在 nums[2],不在的话就把3换到nums[2]的位置上；
   -1， 4， 3， 1； 然后判断4
   -1，1，3，4；这个时候如果是if，那么1的判断就结束了，但是1应该是在nums[0]的位置上，所以必须是while保证当前元素在正确的位置上。
   
   第一次-1换到了nums[0], 因为-1不在[1, n+1]的范围内，所以不用管，等着被换就行了！
   
   
   ```

```c++
class Solution {
public:
    int firstMissingPositive(vector<int>& nums) {
        int n = nums.size();
        for(int i = 0; i < nums.size(); ++i){
            while(nums[i] >= 1 && nums[i] <= n && nums[i] != nums[nums[i] - 1]){
                swap(nums[i], nums[nums[i] - 1]);
            }
        }
        for(int i = 0; i < n; ++i)
            if(nums[i] != i + 1) return i + 1;
        return n + 1;
    }
};
```

## [122. 买卖股票的最佳时机 II](https://leetcode.cn/problems/best-time-to-buy-and-sell-stock-ii/)（中等单调栈）×√

```c++
class Solution {
public:
    int maxProfit(vector<int>& prices) {
        stack<int> st;
        int res = 0;
        prices.emplace_back(0); // 末尾要加个0，因为要判断最后一天的股价能否盈利
        for(int i = 0; i < prices.size(); ++i){
            if(!st.empty() && prices[i] < st.top()){ // 如果新元素比栈顶小，说明栈内的元素是单调递增的，可盈利
                int high = st.top(), slow = high; // 定义最高价与最低价，slow=high是处理栈内只有一个元素的情况
                st.pop(); // 栈顶出栈
                while(!st.empty()){ // 一直出到栈空，找到最低价，因为栈内是单调递增的，最后肯定是栈空
                    slow = st.top();
                    st.pop();
                }
                res += high - slow; // 记录一次利润
            } 
            st.push(prices[i]); // 把新元素入栈
        } 
        return res;
    }
};
```





## [179. 最大数](https://leetcode.cn/problems/largest-number/)（中等）×！×

模拟题。

就是忘了应该怎么比了。直接看代码。

```c++
class Solution {
public:
    string largestNumber(vector<int>& nums) {
        string res;
        sort(nums.begin(), nums.end(), [](int& lhs, int& rhs){
                string sl = to_string(lhs);
                string sr = to_string(rhs);
                return sl + sr > sr + sl;
        });
        // 如果排序后第一个数字是0，说明所有数字都是0
        if(nums[0] == 0) return "0";
        for(auto e: nums){
            res += to_string(e);
        }
        return res;
    }
};
```

## [118. 杨辉三角](https://leetcode.cn/problems/pascals-triangle/)（简单）×

模拟、动态规划。

Vector的resize(n, e = T()); 第二个参数并不能改变容器中所有元素，只有当n大于当前size时，追加的元素才会赋值e。

```c++
class Solution{
public:
    vector<vector<int>> generate(int numRows){
		vector<vector<int>> res(numRows);
        for(int i = 0; i < numRows; ++i){
			res[i].resize(i + 1);
			res[i][0] = res[i][i] = 1;
			for(int j = 1; j < i; ++j){
				res[i][j] = res[i-1][j] + res[i-1][j-1];
			}
		}
		return res;
    }
};
```







## [50. Pow(x, n)](https://leetcode.cn/problems/powx-n/)（中等）×！！

模拟——快速幂！有两种办法：https://leetcode.cn/problems/powx-n/solution/50-powx-n-kuai-su-mi-qing-xi-tu-jie-by-jyd/

第一种：二进制角度：主要是把幂转换成二进制，然后再将幂的二进制展开，具体看题解。

**有个个问题：就算n < 0 ;每次用 n >> 1   为什么会超时呢？ 应该可以算完啊**

[正数 负数 左移＜＜ 右移＞＞](https://blog.csdn.net/Jia_Feng_/article/details/116495063?spm=1001.2101.3001.6650.1&utm_medium=distribute.pc_relevant.none-task-blog-2%7Edefault%7ECTRLIST%7ERate-1-116495063-blog-8038728.pc_relevant_multi_platform_whitelistv3&depth_1-utm_source=distribute.pc_relevant.none-task-blog-2%7Edefault%7ECTRLIST%7ERate-1-116495063-blog-8038728.pc_relevant_multi_platform_whitelistv3&utm_relevant_index=2)

对于正数来说，左移右移都补0；

对于负数来说，左移添0，右移添1；

负数的右移：负数右移的话，由于要保持它是负数，所以负数的二进制的右边补1。如果一直右移的话，最后就就变成0xFFFFFFFF 即-1
如： -4>>1 为-2 ；-4>>2为-1（除二）
2)负数的左移：跟正整数左移一样，右边补0，一直左移的话，最后就是0啦。-2<<2 为-4 ； -2<<31为0（×2，负数越×越小）。

因此对于本题来说，如果使用n >> 1，那么n最终会变成-1且无限循环。

```c++
class Solution {
public:
    double myPow(double x, int n) {
        double res = 1.0;

        if(x == 0.0) return 0.0;

        if(n < 0) x = res / x; // 如果 n < 0; 令x = 1 / x
        while(n){
            if(n & 1) res *= x;
            x *= x;
            n /= 2;
        }
        return res;
    }
};
```

**第二种解法：分治法！！！！！**

假设b = 13，1101

```c++
class Solution {
    public double myPow(double x, int n) {
        if(x == 0.0f) return 0.0d;
        long b = n;
        double res = 1.0;
        if(b < 0) {
            x = 1 / x;
            b = -b;
        }
        while(b > 0) {
            if((b & 1) == 1) res *= x;
            x *= x;
            b >>= 1;
        }
        return res;
    }
}

```

## [69. x 的平方根 ](https://leetcode.cn/problems/sqrtx/)（简单）×！√

二分-寻找最右边的值。

为什么是寻找最右值？由于x平方根的整数部分res是**满足 k * k <= x的最大k值，最大即最右！ **

注意，(long)mid * mid <= x这个是条件，小于或者等于，由于是找最右值，所以l = mid + 1;

而不是(long)mid * mid >= x；去找最左值，3*3 是大与8的最左值，这明显是错误的。

```c++
/*
二分的条件：mid * mid <= x
要找符合条件的最右值，所以只要符合条件，就向右更新

*/
class Solution {
public:
    int mySqrt(int x) {
        if(x == 1) return 1;
        int l = 0, r = x / 2;
        long mid;
        auto flag = [&](){return mid * mid <= x;};
        while(l <= r){
            mid = l + (r - l) / 2;
            if(flag()) l = mid + 1;
            else r = mid - 1;
        }
        return r;
    }
};
```



## [28. 找出字符串中第一个匹配项的下标](https://leetcode.cn/problems/find-the-index-of-the-first-occurrence-in-a-string/)（中等）×！

KMP！！！！

```c++

```

## [454. 四数相加 II](https://leetcode.cn/problems/4sum-ii/)（中等）×

```c++
class Solution {
public:
    int fourSumCount(vector<int>& A, vector<int>& B, vector<int>& C, vector<int>& D) {
        unordered_map<int, int> umap; //key:a+b的数值，value:a+b数值出现的次数
        // 遍历大A和大B数组，统计两个数组元素之和，和出现的次数，放到map中
        for (int a : A) {
            for (int b : B) {
                umap[a + b]++;
            }
        }
        int count = 0; // 统计a+b+c+d = 0 出现的次数
        // 在遍历大C和大D数组，找到如果 0-(c+d) 在map中出现过的话，就把map中key对应的value也就是出现次数统计出来。
        for (int c : C) {
            for (int d : D) {
                if (umap.find(0 - (c + d)) != umap.end()) {
                    count += umap[0 - (c + d)];
                }
            }
        }
        return count;
    }
};
```



## [134. 加油站](https://leetcode.cn/problems/gas-station/)（中等）×！！

贪心算法！

好好解释一下：

1. 首先计算出rest[i] = gas[i] - cost[i]; 意思是从i出发到达i+1的加油站剩余的油量；如果rest[i]的总和 < 0 说明不管从哪个加油站出发，肯定无法走完一圈，大于0则肯定存在从某个加油站出发能走完一圈。
2. curSum计算rest[i]的累加和，当小于0时，重新计算。为什么？当到i时累加和小于0了，说明从之前的出发点出发到不了i+1加油站，所以将start = i + 1，从i + 1重新出发。
3. 如果restSum（rest[i]的和）大于0，那么start一定是有效的。小于0则start无效。

**为什么是贪心算法**：因为当curSum小于0时，说明从之前的出发点出发到不了i+1加油站，那就选择从i+1个加油站出发，即**局部最优**（你可能会问从出发点到i+1之间的加油站出发不行吗？我想了一下如果先减后增（<0），那start会直接更新；如果先增后减，那从i+1之前出发就没有意义）。

最终的答案是从局部最优中推出来的，因此，符合贪心算法。（因为只要restSum > 0，之前的局部最优start就一定是 有效的。）

如果频繁出现curSum小于0，那start就一直往后更新。为什么？因为如果restSum>0，也就是肯定能走完一圈的话，前面有多少小于的，后面就有多少大于0，所以从越后面的加油站出发剩余的油量越多，来抵消前面cost大的路段。

```c++
class Solution {
public:
    int canCompleteCircuit(vector<int>& gas, vector<int>& cost) {
        int n = gas.size();
        int start(0), curSum(0), restSum(0);

        for(int i = 0; i < n; ++i){
            curSum += gas[i] - cost[i];
            if(curSum < 0){
                start = i + 1;
                curSum = 0;
            }
            restSum += gas[i] - cost[i];
        }
        if(restSum < 0) return -1;
        return start;
    }
};
```





## [149. 直线上最多的点数](https://leetcode.cn/problems/max-points-on-a-line/)（困难）×！！！

哈希表！！！这种本来是O(n3)的时间复杂度，通过哈希表降成O(n2)的操作一定好好好看好好学！！

https://leetcode.cn/problems/max-points-on-a-line/solution/xiang-xi-tong-su-de-si-lu-fen-xi-duo-jie-fa-by--35/  好解法！！到时候用这个！！

**为什么 * 1.0 就可以解决分母是0的问题？？？？？？？？？？？**

```c++
/*
这种解法的思路是，对于每一个点，将它与其他所有点的斜率都放在哈希表中统计，同一斜率最大的value就是一条直线上最多的点数
形象点说就是遍历每一个点，向所有其他点发出射线，看哪一条射线上的点数最多！！
*/
class Solution {
public:
    int maxPoints(vector<vector<int>>& points) {
        int ans = 0;
        int n = points.size();
        
        for (int i = 0; i < n; i++) {
            unordered_map<long double, int> ma;
            int thisAns = 0;
            for (int j = 0 ; j < n; j++) {
                if (i == j)
                    continue;
                long double k = 1.0 * (points[j][1] - points[i][1]) / (points[j][0] - points[i][0]);
                ma[k]++;
                thisAns = max(thisAns, ma[k]);
            }
            ans = max(ans, thisAns + 1);
        }

        return ans;
    }
};

```

## [350. 两个数组的交集 II](https://leetcode.cn/problems/intersection-of-two-arrays-ii/)（简单）×

哈希、双指针！

进阶三问：

1. 如果是排好序的数组，那么显然应该使用双指针，时间复杂度为O(n);
2. 如果nums1的size较小，那么应该使用nums1进行哈希计数，然后在另一个数组中遍历，根据哈希来确定交集。
3. 如果数组很大内存很小，显然不能使用哈希。应该先对数组进行外部排序，归并排序就是天然适合外部排序的算法。
   可以将分割后的子数组写入单个文件进行排序，归并时将小文件合并为大文件。当两个数组均排序完成生成两个大文件后，即可使用双指针遍历两个文件，如此可以使空间复杂度降为最低。

**哈希：注意选择size较小的nums存到哈希表中的技巧，很舒服！！！**

```c++
class Solution{
public:
	vector<int> intersect(vector<int>& nums1, vector<int>& nums2){
		// 好技巧！！！！！
		if(nums1.size() > nums2.size()) return intersect(nums2, nums1);
		vector<int> res;
		unordered_map<int, int> umap;
		for(auto e: nums1)
			++umap[e];
		for(auto e: nums2){
			if(umap[e] > 0){
				res.push_back(e);
				--umap[e];
			}
		}
		return res;
	}
};
```

双指针：

```c++
class Solution{
public:
	vector<int> intersect(vector<int>& nums1, vector<int>& nums2){
		sort(nums1.begin(), nums1.end());
		sort(nums2.begin(), nums2.end());
		vector<int> res;
		for(int p1(0), p2(0); p1 < nums1.size() && p2 < nums2.size();){
			if(nums1[p1] < nums2[p2]) ++p1;
			else if(nums1[p1] > nums2[p2]) ++p2;
			else{
				res.push_back(nums1[p1]);
				++p1;
				++p2;
			}
		}
		return res;
		
	}
};
```



## [106. 从中序与后序遍历序列构造二叉树](https://leetcode.cn/problems/construct-binary-tree-from-inorder-and-postorder-traversal/)（中等）×

看一眼图片一目了然。
1.后序数组的最后一个元素就是根节点。
2.然后用后续数组的最后一个元素去切割中序数组，左半部分就是左子树的中序数组，右半部分就是右子树的中序数组；
3.因为后序数组是先遍历左子树，再遍历右子树，最后遍历根节点，所以把后序数组的根节点删除。后序数组的左子树的后序数组 和  右子树的后序数组  数量是一样的， 只是顺序不一样。 因此可以直接用左 / 右 子树的中序数组的size来切割后序数组。

```c++
class Solution{
	TreeNode* traversal(vector<int>& inorder, vector<int>& postorder){
		if(postorder.size() == 0) return nullptr; // 递归出口
		// 后序遍历数组的最后一个节点就是当前的根节点
		int rootValue = postorder[postorder.size() - 1];
		TreeNode* root = new TreeNode(rootValue);
		if(postorder.size() == 1) return root; // 叶子节点  递归出口
		//找到中序遍历数组的切割点
		int delimiterIndex;
		for(delimiterIndex = 0; delimiterIndex < inorder.size(); ++delimiterIndex){
			if(inorder[delimiterIndex] == rootValue) break;
		}
		
		// 切割中序数组
		vector<int> leftInorder(inorder.begin(), inorder.begin() + delimiterIndex);
		vector<int> rightInorder(inorder.begin() + delimiterIndex + 1, inorder.end());

		// 舍弃后序数组的末尾元素，为了切割的左右后序数组size与左右中序数组size一样。
		postorder.resize(postorder.size() - 1);
		// 这里注意：postorder.begin() + leftInorder.size() 不是 + delimiterIndex
		vector<int> leftPostorder(postorder.begin(), postorder.begin() + leftInorder.size());
		vector<int> rightPostorder(postorder.begin() + leftInorder.size(), postorder.end());
		root->left = traversal(leftInorder, leftPostorder);
		root->right = traversal(rightInorder, rightPostorder);
		return root;
	}
public:
	TreeNode* buildTree(vector<int>& inorder, vector<int>& postorder){
		return traversal(inorder, postorder);
	}
};
```



## [29. 两数相除](https://leetcode.cn/problems/divide-two-integers/)（中等）×！?

二分 + 快速乘！(只有允许用long才行，因为在 0 和 被除数之间取 mid ，再去乘除数，很容易就溢出了，但long不会。)

**更优：逐渐逼近法。**



```c++
//假设：divedend - 10  divisor - 3;  先都映射成负数：divedend - （-10）  divisor （-3）;
while(dividend  <= divisor){   // -10 <= -3
    int s = divisor, d = -1;
    while(s >= LIMIT && d >= LIMIT && s >= dividend - s){ // s = -6 d = -2时 -6 < -10 - (-6) = -4 
    s += s; d += d; 								   // 更新dividend = -4 res = 0 + (-2) = -2
	}                                                    // s = -3 < -4 - (-3) = -1
    dividend -= s;                                       // res += -1 = -3 
    res += d;
}
```



```c++
class Solution{
public:
	int divide(int dividend, int divisor){
		int res = 0;
		// 由于操作数都是负数，因此自倍增过程中如果操作数小于INT_MIN / 2，则结果会发生溢出
		int LIMIT = INT_MIN / 2;
		// INT_MIN / -1 会溢出， 因此直接返回INT_MAX
		if(dividend == INT_MIN && divisor == -1) return INT_MAX;
		// 记录是否异号
		bool flag = ((dividend > 0 && divisor < 0) || (dividend < 0 && divisor > 0)) ? true : false;
		dividend = dividend > 0 ? -dividend : dividend;
		divisor = divisor > 0 ? -divisor : divisor;
		// 因为都变成了负号，因此除数比被除数大
		while(dividend  <= divisor){
			int s = divisor, d = -1;
			while(s >= LIMIT && d >= LIMIT && s >= dividend - s){
				s += s; d += d; 
		    }
		dividend -= s;
		res += d;
	   }
	   return flag ? res : -res;
	}
};

```

## 

```c++
class Solution {
    int INF = Integer.MAX_VALUE;
    public int divide(int _a, int _b) {
        long a = _a, b = _b;
        boolean flag = false;
        if ((a < 0 && b > 0) || (a > 0 && b < 0)) flag = true;
        if (a < 0) a = -a;
        if (b < 0) b = -b;
        long l = 0, r = a;
        while (l < r) {
            long mid = l + r + 1 >> 1;
            if (mul(mid, b) <= a) l = mid;
            else r = mid - 1;
        }
        r = flag ? -r : r;
        if (r > INF || r < -INF - 1) return INF;
        return (int)r;
    }
    long mul(long a, long k) {
        long ans = 0;
        while (k > 0) {
            if ((k & 1) == 1) ans += a;
            k >>= 1;
            a += a;
        }
        return ans;
    }
}
```



## [44. 通配符匹配](https://leetcode.cn/problems/wildcard-matching/)（困难）×！！？？？

表格动态规划！千万注意第一行第一列一般都是要初始化的！！太累了下次再看！

题目拓展
最长公共子串： 718. 最长重复子数组 （类似题目，只是由字符串变为数组）

72. 编辑距离
1143. 最长公共子序列
10. 正则表达式匹配
583. 两个字符串的删除操作
727. 最小窗口子序列

```c++
class Solution {
    public boolean isMatch(String s, String p) {
        int m = s.length(), n = p.length();
        boolean[][] f = new boolean[m + 1][n + 1];
        
        f[0][0] = true;
        for(int i = 1; i <= n; i++){
            f[0][i] = f[0][i - 1] && p.charAt(i - 1) == '*';
        }
        
        for(int i = 1; i <= m; i++){
            for(int j = 1; j <= n; j++){
                if(s.charAt(i - 1) == p.charAt(j - 1) || p.charAt(j - 1) == '?'){
                    f[i][j] = f[i - 1][j - 1];
                }
                if(p.charAt(j - 1) == '*'){
                    f[i][j] = f[i][j - 1] || f[i - 1][j];
                }
            }
        }
        return f[m][n];
    }
}
 
```

## [91. 解码方法](https://leetcode.cn/problems/decode-ways/)（中等）××

```c++
/*
1 边界不好处理，那就构造一个边界
2 空字符的解码出来为空，方法数为1，其实就是构造边界。
3、4   假设用例为"112", a就是1，b就是11，分别计算是否符合解码条件。
5 对于i = 1时， b = ' ' - '0'  + 1 - '0',   ' '对应32， '0' 对应48 所以结果是 -161，不会成立。
*/

class Solution {
public:
    int numDecodings(string s) {
        int n = s.size();
        s = ' ' + s; // 1
        vector<int> dp(n + 1);
        dp[0] = 1; // 2
        for(int i = 1; i < n + 1; ++i){
            int a = s[i] - '0'; // 3
            int b = (s[i - 1] - '0') * 10 + s[i] - '0'; // 4
            if(a > 0 && a <= 9) dp[i] = dp[i - 1];
            if(b >= 10 && b <= 26) dp[i] += dp[i - 2]; // 5
        }
        return dp[n];
    }
};
```

## [395. 至少有 K 个重复字符的最长子串](https://leetcode.cn/problems/longest-substring-with-at-least-k-repeating-characters/)（中等-分治-滑动窗口）×！×

**学一下到底什么是分治？什么时候能用滑动窗口？**
先说一个令我迷惑的点：就是用哈希统计了每个字符出现的次数之后，直接用个start和num遍历一下不就能得到最长子串了吗？其实不是，举个例子：
s = "xyaabbcabhhh", k = 3
能说s[2:5] = "aabb"是个符合条件的子串吗？不能！因为虽然他们的计数都=3，但是a和b的第k次出现都被c隔开了。所以不能这么简单的梭哈。
故使用分治法：统计了每个字符出现的次数后，依次遍历字符串，当start = 2, num = 4时，他们的count都=3，我们就把这个子串递归处理，判断一下a和b的3次重复是不是都在这个子串里。如果不是，那G了。

 再捋一遍思路：s = "xyaabbcabhhh", k = 3：
 1.先统计每个字符出现的次数；
 2.判断是否所有字符都大于k，都大于返回s.size();
 3.否则遍历s，截取大于k的连续子串，递归处理，判断子串中的所有字符的出现次数是否都大于k，还是说出现的k次被中间出现次数小于k的字符隔开了。
 4.返回子串的数量。

```c++
class Solution{
public:
	int longestSubstring(string s, int k){
		unordered_map<char, int> umap;
		for(auto c: s) ++umap[c];
		bool isAll = true;
		for(auto& kv: umap){
			if(kv.second < k){
				isAll = false;
				break;
			}
		}
		if(isAll) return s.size();
		int maxCount = 0;
		for(int i = 0; i < s.size(); ++i){
			int start = i, num = 0;
			while(i < s.size() && umap[s[i]] >= k){ // 这里i不符合循环条件了说明出现次数<k，for再++i跳过去是没问题的
				++i;
				++num;
			}
			maxCount = max(longestSubstring(s.substr(start, num), k), maxCount);		
		}
		return maxCount;
	}
};
```

## [138. 复制带随机指针的链表](https://leetcode.cn/problems/copy-list-with-random-pointer/)（中等）×！

```c++
/*
// Definition for a Node.
class Node {
public:
    int val;
    Node* next;
    Node* random;
    
    Node(int _val) {
        val = _val;
        next = NULL;
        random = NULL;
    }
};
*/
/*
我们就看示例一：主要是构建一个map，构建原节点和新节点的关系，这样所有新节点就已经都存在了；
再去更新每个新节点的next和random，因为所有新节点都已经构建好了，所以用umap就可以找到对应的新的next和random节点。
*/
class Solution {
public:
    Node* copyRandomList(Node* head) {
        if(!head) return head;
        Node* cur = head;
        unordered_map<Node*, Node*> umap;
        while(cur != nullptr){
            umap[cur] = new Node(cur->val);
            cur = cur->next;
        }
        cur = head;
        while(cur != nullptr){
            umap[cur]->next = umap[cur->next];
            umap[cur]->random = umap[cur->random];
            cur = cur->next;
        }
        return umap[head];
    }
};
```

## [412. Fizz Buzz](https://leetcode.cn/problems/fizz-buzz/)（简单）×

挺简单的，就是注意一下简洁的写法，在看一下string移动时发生了什么。 
```c++
class Solution{
public:
	vector<string> fizzBuzz(int n){
		vector<string> res;
		string tmp;
		for(int i = 1; i <= n; ++i){
			if(i % 3 == 0) tmp += "Fizz";
			if(i % 5 == 0) tmp += "Buzz";
			if(tmp.size() == 0) tmp += to_string(i);
			res.push_back(std::move(tmp));
		}
		return res;
	}
};
```

## [315. 计算右侧小于当前元素的个数](https://leetcode.cn/problems/count-of-smaller-numbers-after-self/)（困难）×？？
归并排序及其应用！！！！？

```c++
class Solution {
public:
    vector<int> countSmaller(vector<int>& nums) {

    }
};
```

## [295. 数据流的中位数](https://leetcode.cn/problems/find-median-from-data-stream/?favorite=2ckc81c)（困难-设计题（优先队列））×
**整理一下排序和堆的仿函数！**
设计题-优先队列！
为了能在O(1)的复杂度内取到中位数，需要两个优先队列，令lpque为大顶堆，rpque为小顶堆（即lpque中的元素都比rque中的元素小）；
并且人为规定：

+ 当数据流元素数量为偶数时，lpque和rpque大小相同，此时中位数为两者堆顶的平均值；
+ 当数据流中元素数量为奇数时，lpque比rpque的size大1，此时中位数就是lque的堆顶元素。

+ 为了满足上述人为规定，在进行addNum时，应当分情况处理：
  + 1 当插入前两者大小相同，说明插入前数据流元素个数为偶数，我们期望达到插入后[lpque的size比rpque的size大1，且两个堆维持有序]，进一步分情况讨论：
    + 如果rpque为空，说明当前插入的是首个元素，直接添加到rpque;
    + 如果r不空，继续分情况讨论：
      + 如果num <= rpque.top();  那么num直接添加到lpque就好(就算先添加到pque, 因为规定lpque的size比rpque大1，也会再放到lpque中)
      + 如果num > rpque.top()； 那么num先添加到rpque中，再把rpque.top()放到lpque中。
  + 2 当插入前两者大小不同，说明插入前lpque比rpque的size大1，我们期望插入后lpque和rpque大小相等，进一步分情况讨论：
    + 如果num >= lpque.top(); 直接放进rpque；(就算先放进lpque，最后也要放到rpque里，所以要优先判断能否放了之后就不用动了)
    + 如果num < lpque.top(); 先放进lpque，再把lpque.top()放到rpque中。


```c++
class MedianFinder {
public:
		priority_queue<int> lpque;
		priority_queue<int, vector<int>, std::greater<int>> rpque; // 注意这里是小顶堆！！！！！！
    MedianFinder() {}
    
    void addNum(int num) {
			int nl = lpque.size(), nr = rpque.size();
			if(nl == nr){
				if(rpque.empty() || num <= rpque.top()) lpque.push(num);
				else{
					lpque.push(rpque.top());
					rpque.pop();
					rpque.push(num);
				}
			}
			else{
				if(num >= lpque.top()) rpque.push(num);
				else{
					rpque.push(lpque.top());
					lpque.pop();
					lpque.push(num);
				}
			}
			
    }
    
    double findMedian() {
			int nl = lpque.size(), nr = rpque.size();
			if(nl == nr) return (lpque.top() + rpque.top()) / 2.0;
			else return lpque.top();
    }
};
```

## [162. 寻找峰值](https://leetcode.cn/problems/find-peak-element/)（中等-二分）×！×
这道题如果暴力解的话很简单，但是要达到log(n)那肯定是要用2分。
为什么不是有序数组也能用二分？这个问题很关键。
首先，题中给了一个条件，nums[-1] = nums[n] = -∞。
**所以 if(nums[mid] > nums[mid + 1]) r = mid; 此时r的右侧是一个上坡，令r = mid；继续寻找看能再找到一个从左往右的上坡。**

**(题目说了对于所有有效的i, nums[i] != nums[i+1])**如果nums[mid] < nums[mid + 1]，此时l的左侧是一个上坡‘/’，继续寻找看能再找到一个从右往左的上坡‘\’。

5   4   3   6    5
l         m        r
6     5 
l       r

6

l,r

当l == r，循环结束。

就算整个序列是单调递减的，r一直向右更新，一直是这样‘\’，直到r==l为0， 因为l[-1]是-∞，所以也是个山峰。  -∞ 3 2 1 -∞ 
你可能会想更新了r(r为5)之后 假设是这样 8 7 6 5 4，不怕呀，因为l[-1]=-∞，8就是个峰值。

```c++
class Solution {
public:
    int findPeakElement(vector<int>& nums) {
        int l = 0, r = nums.size() - 1, mid = 0;
        while(l < r){
            mid = l + (r - l) / 2;
            if(nums[mid] > nums[mid + 1]) r = mid;
            else l = mid + 1;
        }
        return l;
    }
};
```

## [297. 二叉树的序列化与反序列化](https://leetcode.cn/problems/serialize-and-deserialize-binary-tree/)（困难-二叉树）？

```c++
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class Codec {
public:

    // Encodes a tree to a single string.
    string serialize(TreeNode* root) {
        
    }

    // Decodes your encoded data to tree.
    TreeNode* deserialize(string data) {
        
    }
};

// Your Codec object will be instantiated and called as such:
// Codec ser, deser;
// TreeNode* ans = deser.deserialize(ser.serialize(root));
```

## [242. 有效的字母异位词](https://leetcode.cn/problems/valid-anagram/)（简单-哈希表）√
我这个脑子一开始只能想到用两个哈希表，去比
明明可以只用一个，然后一加一减就解决了。下次注意。

```c++
class Solution {
public:
    bool isAnagram(string s, string t) {
        unordered_map<char, int> umap;
				for(auto c: s) ++umap[c];
				for(auto c: t) --umap[c];
				for(auto& p: umap){
					if(p.second != 0) return false;
				}
				return true;
    }
};
```



## [189. 轮转数组](https://leetcode.cn/problems/rotate-array/)（中等- 模拟）×√

```c++
class Solution {
public:
    void rotate(vector<int>& nums, int k) {
        int n = nums.size();
        k = k % n;
        auto reverse = [&](int start, int end){ while(start < end) swap(nums[start++], nums[end--]);};
        reverse(0, n - 1);
        reverse(0, k - 1);
        reverse(k, n - 1);
    }

};
```

## [125. 验证回文串](https://leetcode.cn/problems/valid-palindrome/)（简单-字符串）××

```c++
/*
注意两点
1 isalnum()判断是否是字母|数字 tolower()转换成小写
2 这里必须是i < j, 不能是i <= j，因为如果是".,", i <= j，那么最终i会等于2，j = 1，得出错误答案，因为正常.,都是要被删除的，空字符串就是回文串。
    所以要返回false，但由于i ！= j 导致得出错误答案。
*/
class Solution {
public:
    bool isPalindrome(string s) {
        int i= 0, j = s.size() - 1;
        while(i < j){
            while(!isalnum(s[i]) && i < j) ++i; // 2
            char l = tolower(s[i]);
            while(!isalnum(s[j]) && i < j) --j;
            char r = tolower(s[j]);
            if(l != r) return false;
            ++i;
            --j;
        }
        return true;
    }
};
```





## [130. 被围绕的区域](https://leetcode.cn/problems/surrounded-regions/)（中等）？

```c++

```





## [66. 加一](https://leetcode.cn/problems/plus-one/)（简单）√

直接像两数之和那样做，so easy。假设一开始需要进一位。

```c++
class Solution {
public:
    vector<int> plusOne(vector<int>& digits) {
        int flag = 1;
        for(int i = digits.size() - 1; i >= 0; --i){
            digits[i] += flag;
            flag = digits[i] / 10;
            digits[i] %= 10; 
        }
        if(flag) digits.emplace(digits.begin(), 1);
        return digits;
    }
};
```

## [204. 计数质数](https://leetcode.cn/problems/count-primes/)（中等）？

```c++

```

## [237. 删除链表中的节点](https://leetcode.cn/problems/delete-node-in-a-linked-list/)（中等-二叉树）×

删不了自己，那就先暂存一下儿子，把自己伪装成儿子照顾孙子，再把儿子弄死，自己就不存在了。

```c++
class Solution {
public:
    void deleteNode(ListNode *node) {
        ListNode* tmp = node->next;
        node->val = node->next->val;
        node->next = node->next->next; // 这里没有回收内存，有需要的同学可自行补充
        delete tmp;
    }
};
```

## [108. 将有序数组转换为二叉搜索树](https://leetcode.cn/problems/convert-sorted-array-to-binary-search-tree/)（简单-二叉树构造）×

+ 高度平衡的二叉树是指一棵树满足 [每个节点的两个左右子树的高度差的绝对值不超过1]的二叉树。

+ 因为题目给的是升序数组，需要构建二叉搜索树，为了满足高度平衡的条件，可以让根节点是排序数组的中点，这样左右子树的高度差不会超过1。然后每次都递归处理。

+ 因为先确定根节点，所以自然的是用谦虚遍历，先构造中间节点，然后递归左子树和右子树。

+ **注意类似用数组构造二叉树的题目，每次分隔尽量不要定义新的数组，而是通过下标索引直接在原数组上操作，这样可以节约时间和空间上的开销。**

  ![](https://pic.leetcode-cn.com/1650516580-YpdrYb-image.png)

```c++
class Solution {
public:
    TreeNode* sortedArrayToBST(vector<int>& nums) {
        int l = 0, r = nums.size();
        return preorder(nums, 0, r);
    }
    // 使用左闭右开区间，[l, r)，这样 int mid = l + (r -  l) / 2 都正好可以作为根节点
    TreeNode* preorder(vector<int>& nums, int l, int r){
        if(l >= r) return nullptr;
        int mid = l + (r - l) / 2;
        TreeNode* root = new TreeNode(nums[mid]);
        root->left = preorder(nums, l, mid); // 注意是左闭右开区间！！！ 不是0 是l！！！
        root->right = preorder(nums, mid + 1, r);
        return root;
    }
};

```

##  [654. 最大二叉树](https://leetcode.cn/problems/maximum-binary-tree/)（中等-类似题）×

一开始想到排序那去了，但是排序就不知道根节点左右有哪些节点了。

所以只要每次遍历找到[l, r]范围内的最大值作为根节点，再递归处理左右子树就可以了。

![](https://pic.leetcode-cn.com/1649941354-HRhrmy-image.png)

```c++
class Solution {
private:
    /* 在左闭右开区间[left, right)，构造二叉树 */
    TreeNode* traversal(vector<int>& nums, int left, int right) {
        /* 递归截止条件 */
        if (left >= right) return NULL;

        /* 求取分割点下标：maxValueIndex */
        int maxValueIndex = left;// 赋值很巧妙可以更新 新数组的起点
        for (int i = left + 1; i < right; i++)
            if (nums[i] > nums[maxValueIndex]) maxValueIndex = i;

        /* 更新节点最大值 */
        TreeNode* root = new TreeNode(nums[maxValueIndex]);
        /* 左闭右开：[left, maxValueIndex) */
        root->left = traversal(nums, left, maxValueIndex);
        /* 左闭右开：[maxValueIndex + 1, right)*/
        root->right = traversal(nums, maxValueIndex + 1, right);
        return root;
    }
public:
    TreeNode* constructMaximumBinaryTree(vector<int>& nums) {
        if(nums.size() == 0) return NULL;
        return traversal(nums, 0, nums.size());
    }
};
```



## [227. 基本计算器 II](https://leetcode.cn/problems/basic-calculator-ii/)（中等-模拟题）×！

还需要多写几遍。

```c++
class Solution {
public:
    int calculate(string s) {
        stack<int> operands;  //存储表达式中的数值
        stack<int> operators; //存储表达式中的运算符
        int index = 0;
        /* 移除用户输入表达式中包含的无用的空格 */
        s.erase(remove(s.begin(), s.end(), ' '), s.end());
        while(index < s.size()){
            if(isdigit(s[index])){
                /* 将字符串转化为数字 */
                int digit = 0;
                for(;index < s.size() && isdigit(s[index]);index++)
                    digit = 10 * digit + (s[index] - '0');
                operands.push(digit);
            } else {
                /* 取运算符 */ // 2 + 3 * 4 + 5 栈顶的优先级高，那就先算一下
                // 2 + 12 + 5 栈顶优先级仍然高，还可以再算，所以要用while
                while(operators.size() && priority(operators.top(), s[index]))
                    calc(operands,operators);
                operators.push(s[index]);
                index++;    
            }    
        }
        /* 保存最后一次运算 */ // 3 + 2 * 2 这要用while
        while(operators.size())
            calc(operands,operators);
        return operands.top();
    }

    /* a 的优先级是否高于等于 b */
    bool priority(int a, int b){
        if(b == '+' || b == '-')
            return true;   
        else if(b=='*' || b=='/')
            return (a == '*' || a == '/');
        return true;
    }

    void calc(stack<int>& operands,stack<int>& operators){
        int result;
        int rhs = operands.top();operands.pop();//右操作数
        int lhs = operands.top();operands.pop();//左操作数
        switch(operators.top()){
            case '+':result = lhs + rhs; break;
            case '-':result = lhs - rhs; break;
            case '*':result = lhs * rhs; break;
            case '/':result = lhs / rhs; break; 
            default: break;
        }
        operators.pop();         //计算完成后，该运算符要弹栈
        operands.push(result);   //将新计算出来的结果入栈
        return;
    }

    
};

```



## [341. 扁平化嵌套列表迭代器](https://leetcode.cn/problems/flatten-nested-list-iterator/)（中等）？

```c++

```

## [344. 反转字符串](https://leetcode.cn/problems/reverse-string/)（简单-字符串）√

```c++
class Solution {
public:
    void reverseString(vector<char>& s) {
        int l = 0, r = s.size() - 1;
        while(l < r) swap(s[l++], s[r--]);
    }
};
```

## [289. 生命游戏](https://leetcode.cn/problems/game-of-life/)（中等）×

为了采用较低的时间复杂度的算法，采用原地修改矩阵的办法。
定义三种状态，对于一个节点：
如果原来是活细胞，当前仍是活细胞，那么仍为1；
如果原来是活细胞，当前变成了死细胞，那么设置为-1；
如果原来是死细胞，当前变成了活细胞，那么设置为2。
最终把-1还原会0，把2还原成1。

```c++
class Solution{
public:
	void gameOfLife(vector<vector<int>>& board){
		int m = board.size();
		int n = board[0].size();
		for(int i = 0; i < m; ++i){
			for(int j = 0; j < n; ++j){
				int num = alive(board, i, j);
				if((board[i][j] == 1) && (num > 3 || num < 2)) board[i][j] = -1;
				if(board[i][j] == 0 && num == 3) board[i][j] = 2;
			}
		}
		// 还原
		for(int i = 0; i < m; ++i){
			for(int j = 0; j < n; ++j){
				if(board[i][j] == 2) board[i][j] = 1;
				if(board[i][j] == -1) board[i][j] = 0;
			}
		}
	}
	// 计算某个细胞周围的细胞存货数量，对i，j的越界情况做特殊处理。
	int alive(vector<vector<int>>& board, int row, int col){
		int res = 0;
		int m = board.size(), n = board[0].size();
		int r_min = row > 0 ? row - 1 : row, c_min = col > 0 ? col - 1 : col;
		int r_max = row == m - 1 ? row : row + 1, c_max = col == n - 1 ? col : col + 1;

		for(int i = r_min; i <= r_max; ++i){
			for(int j = c_min; j <= c_max; ++j){
				if(i == row && j == col) continue;
				if(abs(board[i][j]) == 1) res += 1;
			}
		}
		return res;
	}
};
```

## [103. 二叉树的锯齿形层序遍历](https://leetcode.cn/problems/binary-tree-zigzag-level-order-traversal/)(中等-二叉树) √√√

没什么问题，就是模拟的时候思路要清晰！

+ flag为true，**正向遍历**，从尾部出，从头部插入**（先插left）**；
+ flag为false，**逆向遍历**，从头部出，从尾部插入**（注意要先插right）**。

```c++
class Solution {
public:
    vector<vector<int>> zigzagLevelOrder(TreeNode* root) {
        if(!root) return {};
        vector<vector<int>> res;
        deque<TreeNode*> dque;
        dque.push_back(root);
        bool flag = 1;
        while(!dque.empty()){
            int n = dque.size();
            vector<int> tmp;
            for(int i = 0; i < n; ++i){
                if(flag){ //  <- 从左往右，后进先出
                    TreeNode* node = dque.back();
                    dque.pop_back();
                    tmp.push_back(node->val);
                    if(node->left) dque.push_front(node->left);
                    if(node->right) dque.push_front(node->right);
                }else{ // -> 从右往左，后进先出
                    TreeNode* node = dque.front();
                    dque.pop_front();
                    tmp.push_back(node->val);
                    if(node->right) dque.push_back(node->right);
                    if(node->left) dque.push_back(node->left);
                }
            }
            flag = !flag; 
            res.push_back(move(tmp));
        } 
        return res;
    }
};
```

## [73. 矩阵置零](https://leetcode.cn/problems/set-matrix-zeroes/)（中等）×

思路分析：
1.首先，不能遇到一个0就把行列刷0，因为刷了一个行列之后，再遇到其他的0，你不知道是原数组中本来就有0，还是被刷出来的0。
2.暴力思路：复制出一个原数组，然后遍历新数组，如果原数组当前位置为0，那么就把新数组的行列刷0。空间复杂度O(m*n);
3.用一个数组+pair记录0出现的位置的话，因为最坏情况是所有位置都是0，所以空间复杂度还是O(m*n);
4.用两个数组分别代表行和列记录0出现的行列坐标，空间复杂度为O(m + n);
5.最优方法：首先记录一下第一行和第一列有没有出现0，然后用第一行和第一列记录当前行或列有没有出现0，再刷。空间复杂度O(1)。补充一点：假设第一行没有0，那么本来的1和被刷出来的0不用变，如果第一行出现过0，那么整行全刷成0。

```c++
class Solution{
public:
	void setZeroes(vector<vector<int>>& matrix){
		int m = matrix.size(), n = matrix[0].size();
		bool r = false, c = false;
		for(int i(0); i < n; ++i){
			if(!matrix[0][i]) r = true;
		}
		for(int i(0); i < m; ++i){
			if(!matrix[i][0]) c = true;
		}
		for(int i(1); i < m; ++i){
			for(int j(1); j < n; ++j){
				if(matrix[i][j] == 0){
					matrix[i][0] = matrix[0][j] = 0;
				}
			}
		}
		for(int i(1); i < m; ++i){
			for(int j(1); j < n; ++j){
				if(!matrix[i][0] || !matrix[0][j])
					 matrix[i][j] = 0;
			}
		}
		if(r){
			for(int i(0); i < n; ++i) matrix[0][i] = 0;
		}
		if(c){
			for(int i(0); i < m; ++i) matrix[i][0] = 0;
		}
	}
};
```

## [329. 矩阵中的最长递增路径](https://leetcode.cn/problems/longest-increasing-path-in-a-matrix/)（困难-DFS）×

其实已经算是做出来了，就是超时了。

关键这里要注意：

```c++
// 如果非要传pre： 假设[[1,1]]
int DFS(vector<vector<int>>& matrix, vector<vector<int>>& memo, int i, int j, int pre){
        int cur = matrix[i][j];
        if(cur <= pre) return 0; // 必须要写在这
        if(memo[i][j] != 0) return memo[i][j]; // 否则以第二个1为起点时，那他可能获得memo[0][1]
        int dx[] = {-1, 0, 1, 0};
        int dy[] = {0, 1, 0, -1};
	    // if(cur <= pre) return 0; 那么这一行一定不能写在这
        int res = 1;
        for(int t = 0; t < 4; ++t){
            int r = i + dx[t];
            int c = j + dy[t];
            if(inArea(matrix, r, c)){ // && cur > matrix[r][c]
                res = max(res, 1 + DFS(matrix, memo, r, c, cur));
            }
        }
        memo[i][j] = res;
        return res;
    }
```



```c++
class Solution {
public:
    int longestIncreasingPath(vector<vector<int>>& matrix) {
        int m = matrix.size(), n = matrix[0].size();
        vector<vector<int>> memo(m, vector<int>(n, 0));
        int res = 0;
        for(int i = 0; i < m; ++i){
            for(int j = 0; j < n; ++j){
                if(memo[i][j] == 0){ // 不等于0，说明被以其他数为起点遍历过，以当前这个更大的数为起点遍历，长度只会更小。
                    int len = DFS(matrix, memo, i, j);
                    res = max(res, len); 
                }
                   
            }
        }
        return res;
    }

    int DFS(vector<vector<int>>& matrix, vector<vector<int>>& memo, int i, int j){
        int cur = matrix[i][j];
        if(memo[i][j] != 0) return memo[i][j];
        int dx[] = {-1, 0, 1, 0};
        int dy[] = {0, 1, 0, -1};

        int res = 1; // 这不能是0，万一四周都比自己小，那自己也算长度1返回。
        for(int t = 0; t < 4; ++t){
            int r = i + dx[t];
            int c = j + dy[t];
            if(inArea(matrix, r, c) && cur > matrix[r][c]){ 
                res = max(res, 1 + DFS(matrix, memo, r, c));
            }
        }
        memo[i][j] = res;
        return res;
    }
    bool inArea(vector<vector<int>>& matrix, int i, int j){
        return i >= 0 && i < matrix.size() && j >= 0 && j < matrix[0].size();
    }
};  
```



## [387. 字符串中的第一个唯一字符](https://leetcode.cn/problems/first-unique-character-in-a-string/)（简单-字符串）√

```c++
class Solution {
public:
    int firstUniqChar(string s) {

    }
};
```



## [210. 课程表 II](https://leetcode.cn/problems/course-schedule-ii/)（中等-DFS/BFS）？
DFS/BFS太不熟练，脑子里框架一点都不清晰！
```c++
class Solution {
public:
	vector<int> tmp;
	vector<int> res;
	vector<int> isSelected;
	bool flag = false;
    vector<int> findOrder(int numCourses, vector<vector<int>>& prerequisites) {
		vector<vector<int>> adjacent(numCourses);
		for(int i = 0; i < prerequisites.size(); ++i)
			adjacent[prerequisites[i][1]].push_back(prerequisites[i][0]);
		for(int i = 0; i < numCourses; ++i){
			if(flag) break;
			DFS(adjacent, isSelected, numCourses, i);
		}
		return res;
    }
    void DFS(vector<vector<int>>& adjacent, vector<int>& isSelected,int numCourses, int i){
		if(isSelected[i]) return;
		isSelected[i] = 1;
		tmp.push_back(i);
		if(tmp.size() == numCourses){
			res = tmp;
			flag = true;
			return;
		}
		for(auto e: adjacent[i]) DFS(adjacent, isSelected, numCourses, e);
		tmp.pop_back();
		isSelected[i] = 0;
		return;
	}
};
```

## [163. 缺失的区间](https://leetcode.cn/problems/missing-ranges/)（简单-模拟题）×！

简单题，我他妈就是没写对：

```c++
class Solution {
public:
    vector<string> findMissingRanges(vector<int>& nums, int lower, int upper) {
        vector<string> res;
        string s;
        nums.insert(nums.begin(), lower);
        nums.push_back(upper + 1);
        for(int i = 1; i < nums.size(); ++i){
            if(nums[i] - nums[i - 1] == 2) res.push_back(to_string(nums[i] - 1));
            if(nums[i] - nums[i - 1] > 2) res.push_back(to_string(nums[i - 1] + 1) + "->" + to_string(nums[i] - 1));
        }
        return res;
    }
};
```

正解：

```c++
class Solution {
public:
    vector<string> findMissingRanges(vector<int>& nums, int lower, int upper) {
        // 关键就是这两行 如果是 1 1  放进vector是  0 2 可以返回1
        nums.insert(nums.begin(),lower-1);
        nums.push_back(upper+1);
        vector<string> ans;
        for(int i = 1;i<nums.size();i++)
            if(nums[i]-nums[i-1]==1) continue;
            else if(nums[i]-nums[i-1]==2) ans.push_back(to_string(nums[i]-1));
            else ans.push_back(to_string(nums[i-1]+1)+"->"+to_string(nums[i]-1));
        return ans;
    }
};
```



## [140. 单词拆分 II](https://leetcode.cn/problems/word-break-ii/)（困难-回溯）？

回溯/DFS/BFS都很不熟练！

```c++
class Solution {
public:
    vector<string> wordBreak(string s, vector<string>& wordDict) {

    }
};
```

## [150. 逆波兰表达式求值](https://leetcode.cn/problems/evaluate-reverse-polish-notation/)（中等-模拟题（栈））×

string转数字：atoi(s.c_str());

我一开始没做出来的原因：

```c++
class Solution {
public:
    int evalRPN(vector<string>& tokens) {
		stack<int> stk;
		for(auto s: tokens){ // 我想单独判断每个字符串的第一个字符是不是运算符，但是tokens里会有负数，那么负号会被当成减号导致错误
			if(isoptor(s[0])){
				int rhs = stk.top(); stk.pop();
				int lhs = stk.top(); stk.pop();
				int res = cal(lhs, rhs, s[0]);
				stk.push(res);
			}
			else stk.push(atoi(s.c_str()));
		}
		return stk.top();
    }
    bool isoptor(char c){
		return c == '+' || c == '-' || c == '/' || c == '*';
	}
};
```

```c++
class Solution {
public:
    int evalRPN(vector<string>& tokens) {
		stack<int> stk;
		for(auto s: tokens){
			if(isoptor(s)){
				int rhs = stk.top(); stk.pop();
				int lhs = stk.top(); stk.pop();
				int res = cal(lhs, rhs, s[0]);
				stk.push(res);
			}
			else stk.push(atoi(s.c_str()));
		}
		return stk.top();
    }
    bool isoptor(string& s){
		return s == "+" || s == "-" || s == "*" || s == "/";
	}
	int cal(int lhs, int rhs, char optor){
		switch(optor){
			case '+': return lhs + rhs;
			case '-': return lhs - rhs;
			case '*': return lhs * rhs;
			case '/': return lhs / rhs;
		}
		return 0;
	}
	
};
```





## [268. 丢失的数字](https://leetcode.cn/problems/missing-number/)（简单-原地哈希）×

https://leetcode.cn/problems/missing-number/solution/gong-shui-san-xie-yi-ti-wu-jie-pai-xu-ji-te3s/

原地哈希的思路一定要掌握！！经常用！！

```c++
// 假设现在是 2 0 1 
while(nums[i] != i && nums[i] < n) swap(nums[i], nums[nums[i]]);
// 好好解释下这一行
// 首先nums[0]<2> != 0 然后 swap(nums[0]<2>, nums[nums[i]<2>]<1>); 也就是把2换到它应该在的位置
// 为什么用while？因为1被换过来了，但是1也没在它应该在的位置，所以继续换，直到把0换过来
// 那如果0不存在呢？ 假如是 2 3 1  最终3被换过来 然后结束当前次循环

 for (int i = 0; i < n; i++) {
            if (nums[i] != i) return i;
        }
// 最后遍历一下，如果0 - n-1都在，那么缺失的数字是n。
```



```C++
class Solution {
public:
    int missingNumber(vector<int>& nums) {
        int n = nums.size();
        for (int i = 0; i < n; ++i) {
            while(nums[i] != i && nums[i] < n) swap(nums[i], nums[nums[i]]);
        }
        for (int i = 0; i < n; i++) {
            if (nums[i] != i) return i;
        }
        return n;
    }
};
```

## [348. 设计井字棋](https://leetcode.cn/problems/design-tic-tac-toe/)（中等）？

```c++

```



## [230. 二叉搜索树中第K小的元素](https://leetcode.cn/problems/kth-smallest-element-in-a-bst/)（中等-二叉树-优先队列）√×

```c++
/*
找第k小，那么用大顶堆就完事了，始终让优先队列的size为k，另外priority_queue默认是大顶堆

 */ 
class Solution {
public:
    priority_queue<int> pque;
    int cnt;
    int kthSmallest(TreeNode* root, int k) {
        cnt = k;
        preorder(root);
        return pque.top();
    }
    void preorder(TreeNode* root){
        if(!root) return;
        preorder(root->left);
        pque.push(root->val);
        if(pque.size() > cnt) pque.pop();
        preorder(root->right);
    }
};
```

## [127. 单词接龙](https://leetcode.cn/problems/word-ladder/)（困难-DFS/BFS）？
DFS/BFS 还得多做。
```c++
class Solution {
public:
	int res = INT_MAX;
	int tmp = 1;
    int ladderLength(string& beginWord, string& endWord, vector<string>& wordList) {
		vector<bool> visited(wordList.size(), false);
		DFS(beginWord, endWord, wordList, visited);
		return res;
    }
    void DFS(string& beginWord, string & endWord, vector<string>& wordList, vector<bool>& visited){
		if(beginWord == endWord){
			++tmp;
			res = min(res, tmp);
			return;
		}
		for(int i = 0; i < wordList.size(); ++i){
			string word = wordList[i];
			if(replace(beginWord, word) && !visited[i]){
				visited[i] = true;
				DFS(word, endWord, wordList, visited);
				visited[i] = false;
				--tmp;
			}
		}
		return;
	}
	bool replace(string& beginWord, string& word){
		if(beginWord.size() != word.size()) return false;
		int num = 0;
		for(int i = 0; i < word.size(); ++i){
			if(beginWord[i] != word[i]) ++num;
		}
		return num <= 1;
	}
};
```

## [378. 有序矩阵中第 K 小的元素](https://leetcode.cn/problems/kth-smallest-element-in-a-sorted-matrix/)（中等-优先队列、二分、归并）√
这道题还可以用二分（最优）、归并排序来解，下次做再看看**矩阵中应用二分的思路和代码，感觉会考。**

优先队列：
```c++
class Solution {
public:
    int kthSmallest(vector<vector<int>>& matrix, int k) {
		int m = matrix.size(), n = matrix[0].size();
		priority_queue<int> pque;
		for(int i(0); i < m; ++i){
			for(int j(0); j < n; ++j){
				pque.push(matrix[i][j]);
				if(pque.size() > k) pque.pop();
			}
		}
		return pque.top();
    }
};
```

## [171. Excel 表列序号](https://leetcode.cn/problems/excel-sheet-column-number/)（简单-模拟题）√
挺简单的，就是26进制加法。但是注意一点，res = res * 26 + c - 'A' + 1; 如果不给c - 'A' + 1加括号先计算，res * 26 + c 可能会溢出。解决办法有两个，要么res定义成unsigned，要么加括号。
```c++
class Solution {
public:
    int titleToNumber(string columnTitle) {
		int res = 0;
		for(auto c: columnTitle)
			res = res * 26 + （c - 'A' + 1）;
		return res;
    }
};
```

## [166. 分数到小数](https://leetcode.cn/problems/fraction-to-recurring-decimal/)（中等-模拟题-除法）？

```c++
class Solution {
public:
    string fractionToDecimal(int numerator, int denominator) {

    }
};
```

## [371. 两整数之和](https://leetcode.cn/problems/sum-of-two-integers/)（中等 -位运算）？

a ^ b就是不进位的加法

![](https://pic.leetcode-cn.com/1643784567-MMSjVx-image-20211119214842165.png)

a & b << 1正好可以算出进位

![](https://pic.leetcode-cn.com/1643784633-JdKjuw-image-20211119215714833.png)

​    1 0  0  1 0

\+     0  1  1 0

​    1  1  0 0 0

https://leetcode.cn/problems/sum-of-two-integers/solution/li-yong-wei-cao-zuo-shi-xian-liang-shu-qiu-he-by-p/

```c++
class Solution {
public:
    int getSum(int a, int b) {
        while(b != 0){
            int temp = a ^ b;
            b = (unsigned)(a & b) << 1;
            a = temp;
        }
        return a;
    }
};

```



## [38. 外观数列](https://leetcode.cn/problems/count-and-say/)（中等-双指针）√

这道题我做的没啥问题。题解都是这么做的。

就是用双指针遍历，slow ~ fast - 1 之间的数字是一样的。

```c++
class Solution {
public:
    string countAndSay(int n) {
        string s = "1";
        for(int i = 0; i < n - 1; ++i){
            string tmp;
            for(int slow(0), fast(0); fast <= s.size(); ++fast){
                if(s[fast] != s[slow]){
                    tmp = tmp + to_string(fast - slow) + to_string(s[slow] - '0');
                    slow = fast;
                }
            }
            s = std::move(tmp);
        }
        return s;
    }
};
```

## [190. 颠倒二进制位](https://leetcode.cn/problems/reverse-bits/)（简单-位运算）×

位运算不太熟练！

简单做法，重新定义一个res，每次把n的末尾取出来给res，res再左移，就实现了翻转。时间复杂度是O(n)。

```c++
//错误写法：不能用while判n是否不是0为结束条件，假设n是5，那么右移3次就是0了，但是res明明应该左移32次
class Solution {
public:
    uint32_t reverseBits(uint32_t n) {
        uint32_t res = 0;
        while(n){
            res = res << 1 | (n & 1);
            n >>= 1;
        }
        return res;
    }
};
```

```c++
// 正确写法
class Solution {
public:
    uint32_t reverseBits(uint32_t n) {
        uint32_t res = 0;
        for(int i = 0; i < 32; ++i){
            res = res << 1 | (n & 1);
            n >>= 1;
        }
        return res;
    }
};
```

更优解法：分治！O(1)的办法，没看懂。

![](https://pic.leetcode-cn.com/1616982968-vXsJSf-190.001.jpeg)

```c++
class Solution {
public:
    uint32_t reverseBits(uint32_t n) {
        n = (n >> 16) | (n << 16);
        n = ((n & 0xff00ff00) >> 8) | ((n & 0x00ff00ff) << 8);
        n = ((n & 0xf0f0f0f0) >> 4) | ((n & 0x0f0f0f0f) << 4);
        n = ((n & 0xcccccccc) >> 2) | ((n & 0x33333333) << 2);
        n = ((n & 0xaaaaaaaa) >> 1) | ((n & 0x55555555) << 1);
        return n;
    }
};
```



## [191. 位1的个数](https://leetcode.cn/problems/number-of-1-bits/)（简单-位运算）√

```c++
class Solution {
public:
    int hammingWeight(uint32_t n) {
        int res = 0;
        while(n != 0){ // 这里可以用while，因为如果n是0说明所有比特位都是0
            if(n & 1) ++res;
            n = n >> 1;
        }
        return res;
    }
};
```

## [328. 奇偶链表](https://leetcode.cn/problems/odd-even-linked-list/)（中等-链表）×
有必要多练习！
![](https://pic.leetcode.cn/1672974277-auUVVZ-LeetCode328.%20%E5%A5%87%E5%81%B6%E9%93%BE%E8%A1%A8.png)

```c++
class Solution {
public:
    ListNode* oddEvenList(ListNode* head) {
        if(!head) return nullptr;
	    // 只需要一个preHead处理偶数节点
		ListNode* preHead = new ListNode(-1), *slow= preHead;
		ListNode* fast = head;
		while(fast){
			// 先处理偶数节点
			slow->next = fast->next;
			slow = slow->next;
			// 1 2 3 
			// f s
			// 只有3存在（slow->next != nullptr），才有必要更改fast->next,否则倒数第二行就会报错（fast->next = preHead->next;）
			// fast必须指向最后一个不空的奇数节点，因为它还要指向第一个偶数节点
			if(slow && slow->next){
				fast->next = slow->next;
				fast = fast->next;
			}else 
				break;
		}
		fast->next = preHead->next;
		return head;
    }
};
```



## [116. 填充每个节点的下一个右侧节点指针](https://leetcode.cn/problems/populating-next-right-pointers-in-each-node/)（中等-二叉树）√

**注意！！！！！！！！！！！！！！！！！！！！！！**

```c++
// dque.size(); 千万不能写在for循环里！！！！因为队列的size会变化！
int n = dque.size();
Node* pre = nullptr;
for(int i = 0; i < n; ++i){
```



```c++
class Solution {
public:
    Node* connect(Node* root) {
        if(!root) return nullptr;
        deque<Node*> dque;
        dque.push_front(root);
        while(!dque.empty()){
            int n = dque.size();
            Node* pre = nullptr;
            for(int i = 0; i < n; ++i){
                Node* node = dque.back(); dque.pop_back();
                node->next = pre;
                if(node->right) dque.push_front(node->right);
                if(node->left) dque.push_front(node->left);
                pre = node;
            }
        }
        return root;
    }
};
```

## [218. 天际线问题](https://leetcode.cn/problems/the-skyline-problem/)（困难）？

```c++
class Solution {
public:
    vector<vector<int>> getSkyline(vector<vector<int>>& buildings) {

    }
};
```



## [340. 至多包含 K 个不同字符的最长子串](https://leetcode.cn/problems/longest-substring-with-at-most-k-distinct-characters/)（中等-滑动窗口）√
```c++
class Solution {
public:
    int lengthOfLongestSubstringKDistinct(string s, int k) {
        int res = 0;
        unordered_map<char, int> umap;
        for(int slow(0), fast(0); fast < s.size(); ++fast){
	        ++umap[s[fast]];
			while(umap.size() > k){
				--umap[s[slow]];
				if(!umap[s[slow]]) umap.erase(s[slow]);
				++slow;
			}
			res = max(res, fast - slow + 1);
		}
		return res;
    }
};
```





## [172. 阶乘后的零](https://leetcode.cn/problems/factorial-trailing-zeroes/)（中等-数学）？

```c++
class Solution {
public:
    int trailingZeroes(int n) {

    }
};
```



## [380. O(1) 时间插入、删除和获取随机元素](https://leetcode.cn/problems/insert-delete-getrandom-o1/)（中等）？
我不理解为什么不能用集合？
插入，删除都是O(1)。
对于随机取元素，用一个随机数加上起始迭代器不能实现吗？
为什么非要用map存储元素在数组中的下标，然后用随机数去数组中随机存取呢？

```c++
class RandomizedSet {
public:
    RandomizedSet() {

    }
    
    bool insert(int val) {

    }
    
    bool remove(int val) {

    }
    
    int getRandom() {

    }
};

/**
 * Your RandomizedSet object will be instantiated and called as such:
 * RandomizedSet* obj = new RandomizedSet();
 * bool param_1 = obj->insert(val);
 * bool param_2 = obj->remove(val);
 * int param_3 = obj->getRandom();
 */
```

## [326. 3 的幂](https://leetcode.cn/problems/power-of-three/)（简单-数学）×

朴素做法:

```c++
class Solution {
public:
    bool isPowerOfThree(int n) {
        if(!n) return 0;
        while(n % 3 == 0) n /= 3;
        return n == 1;
    }
};
```

O(1)做法：先分析出int内最大的3次幂。

为什么能这样做？举例，3是质数，因此27的因子只有27，9，3，1。

所以只要n能整除INT_MAX内最大的3次幂，说明n就是3的幂。

但是不是质数不能用这个办法，比如4，16  % 8 = 0；但是8不是4的幂。 	

```c++
class Solution {
public:
    bool isPowerOfThree(int n) {
        return n > 0 && 1162261467 % n == 0;
    }
};
```



## [324. 摆动排序 II](https://leetcode.cn/problems/wiggle-sort-ii/)（中等-双指针、桶排序）× ？桶排序
https://leetcode.cn/problems/wiggle-sort-ii/solution/by-jiang-hui-4-5qdr/
O(n)的桶排序我可以不会，但是排序+双指针我总得会吧？

```cpp
class Solution {
public:
    void wiggleSort(vector<int>& nums) {
	    vector<int> arr(nums);
        sort(arr.begin(), arr.end());
        // 偶数：2233  奇数：112  
        int left = (arr.size() - 1) /  2;
        int right = arr.size() - 1;
        for(int i = 0; i < nums.size(); ++i){
			if(i % 2 == 0){
				nums[i] = arr[left];
				--left;
			}else{
				nums[i] = arr[right];
				--right;
			}
		}
    }
};
```



# 字节零基础算法100例

https://leetcode.cn/leetbook/read/basic-algorithm-100-li/wlja1i/



# Last

## [146. LRU 缓存](https://leetcode.cn/problems/lru-cache/)

## [912. 排序数组](https://leetcode.cn/problems/sort-an-array/)



# 动态规划

## 背包问题

[背包九讲](https://blog.nowcoder.net/n/3ae2828dac364e9f86ff1b39ae87dd77)

背包问题 (Knapsack problem) 是一种组合优化的 NP (NP-Complete) 完全问题。**问题可以描述为：给定一组物品，每种物品都有自己的重量和价格，在限定的总重量内，我们如何选择，才能使得物品的总价格最高。问题的名称来源于如何选择最合适的物品放置于给定背包中。**

**一般问题：** 我们有n件物品和一个容量 (capacity)为C的背包，记第i件物品的重量为weight[i]，价值 为value[i] ，求将哪些物品装入背包可使价值总和最大。

###  0-1背包问题

**0-1背包：** 有n件物品和一个最多能背重量为w的背包。第i件物品的重量是weight[i]，得到的价值是value[i] 。**每件物品只能用一次**，求解将哪些物品装入背包里物品价值总和最大。

一般地，我们认为：`dp[i][j]`表示前 i 件物品放入一个容量为 j 的背包**可以获得的最大价值**（每件物品最多放一次），则状态转移过程可表示为： 	

+ 不选第 i 件物品，则问题转化为前 i-1 件物件放入容量为 j 的背包中所获得的价值：`dp[i][j]`=`dp[i-1][j]`;
+ 选第i件物品，则问题转化为前i-1件物件放入容量为(j-w_i)容量的背包中所获得的价值：`dp[i][j]`=`dp[i-1][j-w_i] + v_i`;注意，能放入第i件物品的前提为w_i <= j。

具体看 [代码随想录 0-1背包](https://programmercarl.com/%E8%83%8C%E5%8C%85%E7%90%86%E8%AE%BA%E5%9F%BA%E7%A1%8001%E8%83%8C%E5%8C%85-1.html#_01-%E8%83%8C%E5%8C%85)，这里只给出代码：

![](https://code-thinking.cdn.bcebos.com/pics/%E5%8A%A8%E6%80%81%E8%A7%84%E5%88%92-%E8%83%8C%E5%8C%85%E9%97%AE%E9%A2%9810.jpg)

```c++
// 1 确定dp数组以及下标的含义 ：dp[i][j] 表示从下标为[0-i]的物品里任意取，放进容量为j的背包，价值总和最大是多少。
vector<vector<int>> dp(weight.size(), vector<int>(bagweight + 1, 0));
// 2 dp数组初始化
// 第一行，初始化dp[0][j],存放编号0的物品的时候，各个容量的背包所能存放的【最大价值】。
// 第一列，是初始化dp[i][0](背包容量为0)。无论是选取哪些物品，背包价值总和一定为0。
for (int j = weight[0]; j <= bagweight; j++) 
    dp[0][j] = value[0];
// 3 确定遍历顺序以及转移方程
for(int i = 1; i < weight.size(); i++) { // 遍历物品(编号为0的物品初始化了)
    for(int j = 0; j <= bagweight; j++) { // 遍历背包容量
        if (j < weight[i]) dp[i][j] = dp[i - 1][j];  // 如果当前背包容量小于物品重量，那么放不下
        // 要么不用当前物品，要么就是当前物品的价值 + 当前背包容量-当前物品重量的最大价值
        else dp[i][j] = max(dp[i - 1][j], dp[i - 1][j - weight[i]] + value[i]);
    }
}
return dp[weight.size() - 1][bagweight]；
```

**一维dp数组**

动规五部曲分析如下：

1. **确定dp数组的定义**

   **在一维dp数组中，dp[j]表示：容量为j的背包，所背的物品价值可以最大为dp[j]。**

2. **一维dp数组的递推公式**

   dp[j]为容量为j的背包所背的最大价值，那么如何推导dp[j]呢？

   dp[j]可以通过dp[j - weight[i]]推导出来，dp[j - weight[i]]表示容量为j - weight[i]的背包所背的最大价值。

   **dp[j - weight[i]] + value[i]** 表示**容量为 j - 物品i重量的背包加上物品i的价值**。（也就是容量为j的背包，放入物品i了之后的价值即：dp[j]）

   此时dp[j]有**两个选择**，**一个是取自己dp[j] 相当于 二维dp数组中的dp\[i-1][j]，即不放物品i**，**一个是取dp[j - weight[i]] + value[i]，即放物品i，指定是取最大的，毕竟是求最大价值，**

   所以递归公式为：

   ```c++
   dp[j] = max(dp[j], dp[j - weight[i]] + value[i]);
   ```

   可以看出相对于二维dp数组的写法，就是把dp\[i][j]中i的维度去掉了。

3. **一维dp数组如何初始化**

   **关于初始化，一定要和dp数组的定义吻合，否则到递推公式的时候就会越来越乱**。

   dp[j]表示：容量为j的背包，所背的物品价值可以最大为dp[j]，那么dp[0]就应该是0，因为背包容量为0所背的物品的最大价值就是0。

   那么dp数组除了下标0的位置，初始为0，其他下标应该初始化多少呢？

   看一下递归公式：dp[j] = max(dp[j], dp[j - weight[i]] + value[i]);

   dp数组在推导的时候一定是取价值最大的数，如果题目给的价值都是正整数那么非0下标都初始化为0就可以了。

4. **一维dp数组遍历顺序**

   代码如下：

   ```c++
   for(int i = 0; i < weight.size(); i++) { // 遍历物品
       for(int j = bagWeight; j >= weight[i]; j--) { // 遍历背包容量
           dp[j] = max(dp[j], dp[j - weight[i]] + value[i]);
       }
   }
   ```

   **这里发现和二维dp的写法中，遍历背包的顺序是不一样的！二维dp遍历的时候，背包容量是从小到大，而一维dp遍历的时候，背包是从大到小。为什么呢？**

   **倒序遍历是为了保证物品i只被放入一次！。但如果一旦正序遍历了，那么物品0就会被重复加入多次！**

   举一个例子：物品0的重量weight[0] = 1，价值value[0] = 15。如果正序遍历：

   ```c++
   dp[1] = dp[1 - weight[0]] + value[0] = 15
   dp[2] = dp[2 - weight[0]] + value[0] = 30
   ```

   此时dp[2]就已经是30了，意味着物品0，被放入了两次，所以不能正序遍历。

   **为什么倒序遍历，就可以保证物品只放入一次呢？**

   ```c++
   dp[2] = dp[2 - weight[0]] + value[0] = 15 （dp数组已经都初始化为0）
   dp[1] = dp[1 - weight[0]] + value[0] = 15
   ```

   所以从后往前循环，每次取得状态不会和之前取得状态重合，这样每种物品就只取一次了。

   **那么问题又来了，为什么二维dp数组历的时候不用倒序呢？**!!!

   因为对于二维dp，dp\[i][j]都是通过上一层即dp\[i - 1][j]计算而来，本层的dp\[i][j]并不会被覆盖！

   ```C++
   dp[1][1] = dp[0][1 - weight[1]] + value[1]
   dp[1][2] = dp[0][2 - weight[1]] + value[1] 
   ```

   

   **再来看看两个嵌套for循环的顺序，代码中是先遍历物品嵌套遍历背包容量，那可不可以先遍历背包容量嵌套遍历物品呢？**

   不可以！

   因为一维dp的写法，背包容量一定是要倒序遍历（原因上面已经讲了），如果遍历背包容量放在上一层，那么每个dp[j]就只会放入一个物品，即：背包里只放入了一个物品。

   倒序遍历的原因是，本质上还是一个对二维数组的遍历，并且右下角的值依赖上一层左上角的值，因此需要保证左边的值仍然是上一层的，从右向左覆盖。

   （这里如果读不懂，就再回想一下dp[j]的定义，或者就把两个for循环顺序颠倒一下试试！）

   **所以一维dp数组的背包在遍历顺序上和二维其实是有很大差异的！**，这一点大家一定要注意。

   1. 举例推导dp数组

   一维dp，分别用物品0，物品1，物品2 来遍历背包，最终得到结果如下：



完整代码：

```c++
void test_1_wei_bag_problem() {
    vector<int> weight = {1, 3, 4};
    vector<int> value = {15, 20, 30};
    int bagWeight = 4;

    // 初始化
    vector<int> dp(bagWeight + 1, 0);
    for(int i = 0; i < weight.size(); i++) { // 遍历物品
        for(int j = bagWeight; j >= weight[i]; j--) { // 遍历背包容量
            dp[j] = max(dp[j], dp[j - weight[i]] + value[i]);
        }
    }
    cout << dp[bagWeight] << endl;
}

int main() {
    test_1_wei_bag_problem();
}
```

#### [416. 分割等和子集](https://leetcode.cn/problems/partition-equal-subset-sum/)（中等-动态规划-背包问题）×

#### [1049. 最后一块石头的重量 II](https://leetcode.cn/problems/last-stone-weight-ii/) （中等-背包问题）×

跟416几乎一模一样，就是将石头重量尽可能均分，那么最终剩下的是有重量一定最小。

#### 用滚动数组

```c++
/*
1
最后dp[target]里是容量为target的背包所能背的最大重量。
那么分成两堆石头，一堆石头的总重量是dp[target]，另一堆就是sum - dp[target]。
在计算target的时候，target = sum / 2 因为是向下取整，所以sum - dp[target] 一定是大于等于dp[target]的。
那么相撞之后剩下的最小石头重量就是 (sum - dp[target]) - dp[target]。
*/
class Solution {
public:
    int lastStoneWeightII(vector<int>& stones) {
        // 用滚动数组的方法，dp[j]表示：背包容量为j时，所装的石头的最大重量
        int total = accumulate(stones.begin(), stones.end(), 0);
        int target = total / 2;
        vector<int> dp(target + 1, 0);
        for(int i = 0; i < stones.size(); ++i){
            for(int j = target; j >= stones[i]; --j){ // 当j < stones[i]时，背包不可能放下石头，所以一定还是不用stone[i]时，背包所能背的最大重量
                dp[j] = max(dp[j], dp[j-stones[i]] + stones[i]); 
            }
        }
        return total - 2 * dp[target]; // 1
    }
};
```



#### [494. 目标和](https://leetcode.cn/problems/target-sum/)（中等-动态规划-背包问题）



#### [474. 一和零](https://leetcode.cn/problems/ones-and-zeroes/)（中等-动态规划-背包问题）？

本题本质上还是0-1背包，因为strs中每个str都是不一样的，不存在重复。

只是背包的容量有两个维度。因此for loop还是需要倒序遍历，防止重复包含str。

**for(auto str: strs){** 就是之前的题目中遍历物品的语句。

```c++
class Solution {
public:
    int findMaxForm(vector<string>& strs, int m, int n) {
        // dp[i][j] 表示 容量为i个0 和 j个1时，最大子集的长度
        vector<vector<int>> dp(m+1, vector<int>(n+1, 0));
        for(auto str: strs){
            int zeroNum = 0, oneNum = 0;
            for(auto c: str){
                if(c == '1')
                    ++oneNum;
                else 
                    ++zeroNum;
            }
            for(int i = m; i >= zeroNum; --i){
                for(int j = n; j >= oneNum; --j){
                    dp[i][j] = max(dp[i][j], dp[i-zeroNum][j-oneNum] + 1);
                }
            }
        }
        return dp[m][n];
    }
};
```

### 完全背包问题

**完全背包：** 如果每件物品最多可以选取无限次，则问题称为完全背包问题。

对于完全背包问题，由于每个物品的数量是无限的，所以在遍历背包容量时需要正向遍历，其他没什么变化。

#### [518. 零钱兑换 II](https://leetcode.cn/problems/coin-change-ii/)（中等-背包问题）√

```c++
class Solution {
public:
    int change(int amount, vector<int>& coins) {
        vector<int> dp(amount + 1);
        dp[0] = 1;
        for(int i = 0; i < coins.size(); ++i){
            for(int j = coins[i]; j <= amount; ++j){
                dp[j] += dp[j - coins[i]];
            }
        }
        return dp[amount];
    }
};
```





#### [279. 完全平方数](https://leetcode.cn/problems/perfect-squares/)

与[322. 零钱兑换](https://leetcode.cn/problems/coin-change/) ✔ 几乎一样。

```C++
class Solution {
public:
    int numSquares(int n) {
        vector<int> dp(n + 1, INT_MAX / 2);
        dp[0] = 0;
        int limit = sqrt(n);
        vector<int> vec(limit);
        for(int i = 1; i<= limit; ++i)
            vec[i-1] = i * i;
        for(auto& c: vec){
            for(int j = c; j <= n; ++j){
                dp[j] = min(dp[j], dp[j-c] + 1);
            }
        }
        return dp[n] == INT_MAX / 2 ? -1 : dp[n];

    }
};
// 有点多此一举
class Solution {
public:
    int numSquares(int n) {
        vector<int> dp(n + 1, INT_MAX);
        dp[0] = 0;
        for (int i = 0; i <= n; i++) { // 遍历背包
            for (int j = 1; j * j <= i; j++) { // 遍历物品 写成这样就行了
                dp[i] = min(dp[i - j * j] + 1, dp[i]);
            }
        }
        return dp[n];
    }
};
```

#### [518. 零钱兑换 II](https://leetcode.cn/problems/coin-change-2/)  √

```c++
class Solution {
public:
    int change(int amount, vector<int>& coins) {
        vector<int> dp(amount + 1, 0); //dp[j]表示凑成金额j有几种方法
        dp[0] = 1; // 凑0元有一种方法，一个硬币都不选
        for(auto& c: coins){
            for(int j = c; j <= amount; ++j){
                dp[j] += dp[j - c];
            }
        }
        return dp[amount];
    }
};
```

#### [377. 组合总和 Ⅳ](https://leetcode.cn/problems/combination-sum-iv/) !!!

如果是组合，不涉及排列问题，那么就先遍历物品，再遍历背包容量；

如果涉及排列问题，那么就要先遍历背包容量，在每个固定的容量中，每个物品在不同的位置都算一次不同的组合。

```c++
class Solution {
public:
    int combinationSum4(vector<int>& nums, int target) {
        vector<int> dp(target + 1, 0);
        dp[0] = 1;
        for(int j = 1; j <= target; ++j){
            for(auto& c: nums){
                if(c <= j && (dp[j]  < INT_MAX - dp[j-c])) //dp[j] += dp[j - c]可能会溢出
                    dp[j] += dp[j - c];
            }                
        }        
    return dp[target];
    }
};
```

**进阶：**如果给定的数组中含有负数会发生什么？问题会产生何种变化？如果允许负数出现，需要向题目中添加哪些限制条件？

https://leetcode.cn/problems/combination-sum-iv/solution/zu-he-zong-he-iv-by-leetcode-solution-q8zv/

## 子序列问题

115.不同的子序列
583.两个字符串的删除操作
72.编辑距离
为了绝杀编辑距离，我做了三步铺垫，你都知道么？
647.回文子串
516.最长回文子序列
动态规划总结篇

### [300. 最长递增子序列](https://leetcode.cn/problems/longest-increasing-subsequence/)（中等-动态规划-子序列问题）

### [674. 最长连续递增序列](https://leetcode.cn/problems/longest-continuous-increasing-subsequence/)（简单-动态规划-子序列问题）

### [718. 最长重复子数组](https://leetcode.cn/problems/maximum-length-of-repeated-subarray/)（中等-动态规划-二维子序列问题）

### [1143. 最长公共子序列](https://leetcode.cn/problems/longest-common-subsequence/)(中等-动态规划-二维子序列问题）√

### [1035. 不相交的线](https://leetcode.cn/problems/uncrossed-lines/)（中等-动态规划-）

### [53. 最大子数组和](https://leetcode.cn/problems/maximum-subarray/)（中等-动态规划/分治）

### [392. 判断子序列](https://leetcode.cn/problems/is-subsequence/)（简单-双指针）

### [115. 不同的子序列](https://leetcode.cn/problems/distinct-subsequences/)（困难）



## 其他

### [213. 打家劫舍 II](https://leetcode.cn/problems/house-robber-ii/)（中等-动态规划）×



### [1746. 经过一次操作后的最大子数组和](https://leetcode.cn/problems/maximum-subarray-sum-after-one-operation/) ×

```cpp
/*
这道题我想偏了，问的明明是子序和，偏偏想成了子序积
1 截至dp0[i]，没有交换过的最大子序和
2 截至dp1[i]，交换过一次的最大子序和
3 看是就交换当前的，还是用曾经交换过的
*/
class Solution {
public:
    int maxSumAfterOperation(vector<int>& nums) {
        int n = nums.size();
        vector<int> dp0(n); // 1
        vector<int> dp1(n); // 2
        dp0[0] = nums[0];
        dp1[0] = nums[0] * nums[0];
        int res = dp1[0];
        for(int i = 1; i < n; ++i){
            dp1[i] = max(dp1[i - 1] + nums[i], max(dp0[i - 1], 0) + nums[i] * nums[i]); // 3
            dp0[i] = max(nums[i], nums[i] + dp0[i - 1]);
            res = max(res, dp1[i]);
        }
        return res;
    }
};
```



# 栈与队列

## [剑指 Offer 09. 用两个栈实现队列](https://leetcode.cn/problems/yong-liang-ge-zhan-shi-xian-dui-lie-lcof/)（简单-栈-队列）√

## [394. 字符串解码](https://leetcode.cn/problems/decode-string/)（中等-栈）××

# 回溯

https://leetcode.cn/problems/subsets/solution/c-zong-jie-liao-hui-su-wen-ti-lei-xing-dai-ni-gao-/

## [78. 子集](https://leetcode.cn/problems/subsets/)（中等-回溯）×



## [77. 组合](https://leetcode.cn/problems/combinations/)（中等）√

```c++
class Solution {
public:
    vector<int> path;
    vector<vector<int>> res;
    vector<vector<int>> combine(int n, int k) {
        backTracking(n, k, 1);
        return res;
    }
    void backTracking(int n, int k, int startIndex){
        for(int i = startIndex; i <= n; ++i){
            path.push_back(i); // 先进来
            if(path.size() != k) // 有没有必要继续递归
                backTracking(n, k, i + 1);
            else res.push_back(path); // 符合条件，加入结果集
            path.pop_back(); // 不管是递归结束，还是符合条件加入结果集，都要弹出尾元素
        }
    }
    return;
};
```

## [17. 电话号码的字母组合](https://leetcode.cn/problems/letter-combinations-of-a-phone-number/)（中等）×√

这道题的关键是，不是从一个序列里面找k个字符的组合，而是从不同的字符串中寻找k个字符的排列。

```c++
class Solution {
public:
    string path;
    vector<string> res;
    vector<string> tel{"", "", "abc", "def", "ghi", "jkl", 
                            "mno", "pqrs", "tuv", "wxyz"};
    vector<string> letterCombinations(string digits) {
        if(!digits.size()) return res;
        DFS(digits, 0);
        return res;
    }
	// 关键在于 startIndex是digits的下标
    void DFS(string &digits, int startIndex){
        // 得到数字对应的字符串在tel中的下标
        int index = digits[startIndex] - '0';
        // 核心在这，常规回溯
        for(int i = 0; i < tel[index].size(); ++i){
            path.push_back(tel[index][i]);
            if(path.size() == digits.size())
                res.push_back(path);
            else if(startIndex + 1 < digits.size())
                DFS(digits, startIndex + 1);
            path.pop_back(); 
        }
    }
};
 
```

## [46. 全排列](https://leetcode.cn/problems/permutations/)(中等-回溯) ××√

```c++
class Solution {
public:
    vector<vector<int>> res;
    vector<int> path;
    vector<vector<int>> permute(vector<int>& nums) {
        vector<bool> visited(nums.size());
        backTrack(nums, visited);
        return res;
    }
    void backTrack(vector<int>& nums, vector<bool>& visited){
        if(path.size() == nums.size()){
            res.push_back(path);
            return;
        }
        for(int i = 0; i < nums.size(); ++i){
            if(visited[i]) continue;
            else{
                path.push_back(nums[i]);
                visited[i] = true;
                backTrack(nums, visited);
                visited[i] = false;
                path.pop_back();
            }
        }
        return;
    }
};
```



## [93. 复原 IP 地址](https://leetcode.cn/problems/restore-ip-addresses/)×！

```c++
/*
1 剪枝
2 用逗号判断递归出口
3 在每次递归中，只要懂了path，就要回溯
4 千万注意！！！！这里要传i！！！！
5 这里必须要判断,否则
输出:["1.2.3.4","1.2.34.","1.23.4.","12.3.4."]
1.2.34. 再递归，应该是直接返回的，因为没有更多的数字了
预期结果:["1.2.3.4"]
*/

class Solution {
public:
    vector<string> res;
    string path;
    vector<string> restoreIpAddresses(string s) {
        if(s.size() < 4 || s.size() > 12) return res; // 1
        backTrack(s, 0, 0);
        return res;
    }
    void backTrack(string& s, int startIndex, int pointNum){ // 2
        if(startIndex >= s.size()) return; // 5
        if(pointNum == 3){
            if(isValid(s, startIndex, s.size() - 1)){
                path += s.substr(startIndex, s.size() - startIndex); // 3
                res.push_back(path);
                path.erase(path.end() - s.size() + startIndex, path.end());
            } 
            return;
        }
        for(int i = startIndex; i < s.size(); ++i){
            if(isValid(s, startIndex, i)){
                path += s.substr(startIndex, i - startIndex + 1);
                path += '.';
                backTrack(s, i + 1, pointNum + 1); // 4
                path.erase(path.end() - i + startIndex - 2, path.end());
            }
            else break; 
        }
    }
    bool isValid(string& s, int l, int r){
        if(s[l] == '0' && l < r) return false; // 判断前导0
        int num = 0;
        for(int i = l; i <= r; ++i){
            if(!isdigit(s[i])) return false; // 非数字不合法
            num = num * 10 + (s[i] - '0');
            if(num > 255) return false;
        }
        return true;
    }

};
```









## [22. 括号生成](https://leetcode.cn/problems/generate-parentheses/)（中等-DFS/回溯）×！×

**好像这种问有多少种组合的题，都应该往DFS/回溯上想！**

通过观察我们可以发现，生成的任何括号组合中都有两个规律：

+ 括号组合中左括号的数量等于右括号的数量；
+ 括号组合中任何位置左括号的数量都是大于等于右括号的数量。

第一条很容易理解，我们来看第二条，比如有效括号"(())()"，**在任何一个位置右括号的数量都不大于左括号的数量**，所以他是有效的。
如果像这样"())()"第3个位置的是右括号，那么他前面只有一个左括号，而他和他前面的右括号有2个，所以无论如何都不能组合成有效的括号。

![](https://pic.leetcode-cn.com/1607265041-lcwBdE-image.png)

```c++
/*
1 path表示已加入结果集中的括号组合，l和r分别代表还需出现的括号的次数
2 递归出口，都不需要出现了，自然满足要求了，将path加入res
3 如果右括号比左括号需要的多，那么可以加右括号
*/
class Solution {
public:
    vector<string> res;
    vector<string> generateParenthesis(int n) {
        dfs("", n, n);
        return res;
    }
    void dfs(string path, int l, int r){ // 1
      if(l == 0 && r == 0){ // 2
          res.push_back(path);
          return;
      }
      if(l > 0) dfs(path + '(', l - 1, r);
      if(r > l) dfs(path + ')', l, r - 1); // 3
    }
};
```



# 

# 哈希表



## [1. 两数之和](https://leetcode.cn/problems/two-sum/)  √

```c++
class Solution {
public:
    vector<int> twoSum(vector<int>& nums, int target) {
        unordered_map<int, int> umap;
        for(int i = 0; i < nums.size(); ++i){
            if(umap.find(target - nums[i]) != umap.end())
                return {i, umap[target - nums[i]]};
            else umap[nums[i]] = i;
        }
        return {};
    }
};
```

## [242. 有效的字母异位词](https://leetcode.cn/problems/valid-anagram/) √ 

异位词的意思就是，某个单词里面的某些字母调换了位置，但是不会有某个字母的数量变多或减少。

解法就是用哈希表，遍历s的时候record[c]++；

遍历t的时候，record[c]--;

如果record中每个value都是0 ->说明是异位词。

```c++
class Solution {
public:
    bool isAnagram(string s, string t) {
        unordered_map<char, int> record;
        for(auto& c: s){
            record[c]++;
        }
        for(auto& c: t){
            record[c]--;
        }
        for(auto it = record.begin(); it != record.end(); ++it){
            if((*it).second != 0)
                return false;
        }
        return true;
    }
};
```

## [349. 两个数组的交集](https://leetcode.cn/problems/intersection-of-two-arrays/)(简单-哈希表) √ √√

```c++
class Solution {
public:
    vector<int> intersection(vector<int>& nums1, vector<int>& nums2) {
        if(nums1.size() > nums2.size()) return intersection(nums2, nums1);
        unordered_set<int> uset1(nums1.begin(), nums1.end());
        unordered_set<int> uset2(nums2.begin(), nums2.end());
        vector<int> res;
        for(auto c: uset1){
            if(uset2.find(c) != uset2.end())
                res.push_back(c);
        }
        return res;
    }
};
```





# 排序

## [215. 数组中的第K个最大元素](https://leetcode.cn/problems/kth-largest-element-in-an-array/)（中等-快排-优先队列）×√！

### [179. 最大数](https://leetcode.cn/problems/largest-number/) （中等）×

```c++
class Solution {
public:
    string largestNumber(vector<int>& nums) {
        string s;
        sort(nums.begin(), nums.end(), [](int &lhs, int &rhs){
            string sl = to_string(lhs);
            string sr = to_string(rhs);
            return sl + sr > sr + sl;
        });
        if(nums[0] == 0) return "0";
        for(auto e: nums){
            cout<< e<<" ";s += to_string(e);
        } 
        return s;
    }
};
```

## [148. 排序链表](https://leetcode.cn/problems/sort-list/)××



## Top K、堆（优先队列）

```c++
int main() {
    vector<int> vec{4, 6, 2, 1, 5};
    priority_queue<int> pque(vec.begin(), vec.end());

    while(!pque.empty()){
        // cout<<pque.top()<<" "; // 6 5 4 2 1 默认是大顶堆
        pque.pop();
    }

    auto comp = [](int i1, int i2){return i1 > i2;}; // >小顶堆，<大顶堆
    priority_queue<int, vector<int>, decltype(comp)> pque2(vec.begin(), vec.end(), comp);

    while(!pque2.empty()){
        cout<<pque2.top()<<" "; // 小顶堆 1 2 4 5 6 
        pque2.pop();
    }

    return 0;
}
```



**一个核心：动态求极值。**

刚开始用，不太熟练，不能用pque.size()去遍历堆，因为pop()之后pque.size()会减一。所以遍历不完，要用empty。

```c++
#include<iostream>
#include <vector>
#include <queue>
using namespace std;
class KthLargest {
public:
    priority_queue<int, std::deque<int>, std::greater<int> > pque;
    int k;
    KthLargest(int k, vector<int>& nums):k(k), pque(nums.begin(), nums.end()) {
        cout<<"size:"<<pque.size()<<endl;
        for(int i = 0; i < pque.size(); i++){
            //pque.size() 出队一个元素它就会减一。
            std::cout << i << " "<< pque.top()<<" "<<endl;
            pque.pop();//移除队头元素的同时，将剩余元素中优先级最大的移至队头
        }
}
};
int main()
{  
    vector<int> nums{4,2,5,8};
    KthLargest k(3, nums);
}
```

### [703. 数据流中的第 K 大元素](https://leetcode.cn/problems/kth-largest-element-in-a-stream/) ×

```c++
template<typename T>
    class myGreater{
    public:
        bool operator()(const T& lhs, const T& rhs){
            return lhs > rhs;
        }
    };
class KthLargest {
public:

    priority_queue<int, vector<int>, myGreater<int>> pque;
    int k;
    KthLargest(int _k, vector<int>& nums): k(_k) {
        for(auto e: nums){
            add(e);
        }

    }
    int add(int val) {
        pque.push(val);
        if(pque.size() > k){
            pque.pop();
        }
        return pque.top();
    }
};
```



### [506. 相对名次](https://leetcode.cn/problems/relative-ranks/) ×

使用大顶堆，类型为pair<int, int>： first： 分数  second：下标。

然后pop的时候就是按名次排的，再根据下标把名次写进res就好。

priority_queue的第三个参数是要给一个可调用对象的类型。

lambda表达式是个可调用**对象**，不是类型。所以要将decltype(cmp)作为模板参数。然后在调用priority_queue的接受一个可调用对象的构造函数。

或者已知compare的的参数和返回类型。可以用std::function包装：

`    priority_queue<pair<int, int>, vector<pair<int, int>>, function<bool(const pair<int, int>& lhs, const pair<int, int>& rhs)>> pque(cmp);`

```c++
class Solution {
public:
    vector<string> findRelativeRanks(vector<int>& score) {
        string desc[3] = {"Gold Medal", "Silver Medal", "Bronze Medal"};
        // pair<int, int>： first： 分数  second：下标
        vector<string> res(score.size());
        auto cmp = [](const pair<int, int>& lhs, const pair<int, int>& rhs){return lhs.first < rhs.first;};
        priority_queue<pair<int, int>, vector<pair<int, int>>, decltype(cmp)> pque(cmp);
        for(int i = 0; i < score.size(); ++i){
            pque.push({score[i], i});
        }
        int num = 1;
        while(!pque.empty()){
            auto athlete = pque.top();
            if(num > 3)
                res[athlete.second] = to_string(num);
            else
                res[athlete.second] = desc[num - 1];
            pque.pop();
            num++;
        }
        
        return res;
    }
};
```

### [1046. 最后一块石头的重量](https://leetcode.cn/problems/last-stone-weight/) √

```C++
class Solution {
public:
    int lastStoneWeight(vector<int>& stones) {
        priority_queue<int> pque(stones.begin(), stones.end());
        int x, y, diff;
        while(pque.size() > 1){
            y = pque.top();
            pque.pop();
            x = pque.top();
            pque.pop();
            diff = y - x;
            if(diff) pque.push(diff);
        }
        if(pque.empty()) return 0;
        else return pque.top();
    }
};
```





### [1464. 数组中两元素的最大乘积](https://leetcode.cn/problems/maximum-product-of-two-elements-in-an-array/) √

```c++
class Solution {
public:
    int maxProduct(vector<int>& nums) {
        priority_queue<int> pque(nums.begin(), nums.end());
        int x, y;
            y = pque.top();
            pque.pop();
            x = pque.top();
            return (x - 1) * (y - 1);
    }
};
```







# 字符串



# 链表

## [2. 两数相加](https://leetcode.cn/problems/add-two-numbers/)（中等） √

```c++
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode() : val(0), next(nullptr) {}
 *     ListNode(int x) : val(x), next(nullptr) {}
 *     ListNode(int x, ListNode *next) : val(x), next(next) {}
 * };
 */
class Solution {
public:
    ListNode* addTwoNumbers(ListNode* l1, ListNode* l2) {
        ListNode* head = new ListNode; // 注意这一步，要new一个节点，不能只定义一个指针
        ListNode *r = head;
        int temp = 0;
        int s1 = 0, s2 = 0, s = 0;
        while(l1 || l2){
            if(l1){
                s1 = l1->val;
                l1 = l1->next;
            }else s1 = 0; // 如果发现s1已经到头了，那后面一直置0就可以，指针不需在移动
            if(l2){
                s2 = l2->val;
                l2 = l2->next;
            }else s2 = 0;
            s = s1 + s2 + temp;
            temp = s / 10;
            int val = s % 10;
            r->next = new ListNode(val);
            r = r->next;
        }
        if(temp) r->next = new ListNode(temp);
        return head->next;
    }
};
```





## [25. K 个一组翻转链表](https://leetcode.cn/problems/reverse-nodes-in-k-group/)（困难-链表-双指针）√

```c++
/*
使用快慢指针，当fast - slow = k时，反转slow和fast之间的节点
1：如果k = 1，分组为1的话就不用翻转
2：指向上一个分组的最后一个节点
3：保存每个分组的下一个节点
4：翻转一组节点时，尾节点的next指针要为空
5：上一组的尾节点指向这一组的头节点
6：更新end，因为反转过后slow指向的是这一组的尾节点
7：slow指向下一组的头节点
8：fast指向下一组的头节点
9：因为fast已经指向下一组的第一个节点，因此i要回1
10：如果next不为空，说明不够一组，接到end后面；如果为空，说明链表正好分成了n / k 组
*/
class Solution {
public:
    ListNode* reverseKGroup(ListNode* head, int k) {
        if(k == 1) return head; // 1
        ListNode* preHead = new ListNode(-1, head);
        ListNode* end = preHead; // 2
        ListNode* slow = head, *fast = head;
        ListNode *next;
        for(int i = 1; fast; ++i){
            if(i == k){
                next = fast->next; // 3
                fast->next = nullptr; // 4
                ListNode* newHead = reverse(slow);
                end->next = newHead; // 5
                end = slow; // 6
                slow = next; // 7
                fast = slow; // 8
                i = 1; // 9
            }
            fast = fast ? fast->next : nullptr;
        }
        end->next = next; // 10
        return preHead->next;
       

    }
    ListNode* reverse(ListNode* head){
        ListNode* pre = nullptr;
        while(head){
            ListNode* next = head->next;
            head->next = pre;
            pre = head;
            head = next;
        }
        return pre;
    }
};
```



## [92. 反转链表 II](https://leetcode.cn/problems/reverse-linked-list-ii/)（中等-链表）×！×√

```c++
/*
输入：head = [1,2,3,4,5], left = 2, right = 4
输出：[1,4,3,2,5]
1 pre指向1
2 cur永远指向2 tail从3遍历到4
3 将3插到1 2 之间 -> 1 3 2 4 5; 将4插到1 3之间-> 1 4 3 2 5 
4 将2指向5
*/
class Solution {
public:
    ListNode* reverseBetween(ListNode* head, int left, int right) {
        ListNode* preHead = new ListNode(-1, head), *pre = preHead;
        for(int i = 0; i < left - 1; ++i)
            pre = pre->next; // 1
        ListNode* cur = pre->next, *tail = cur->next; // 2
        for(int i = 0; i < right - left; ++i){ // 3
            ListNode* next = tail->next;
            tail->next = pre->next;
            pre->next = tail;
            tail = next;
        }
        cur->next = tail; // 4
        return preHead->next;
    }
};
```



## [142. 环形链表 II](https://leetcode.cn/problems/linked-list-cycle-ii/)（中等-链表）

**再看看公式。**

![环形链表](E:\拯救者\study\leetcode\img\环形链表.jpg)

**证明链表有环：**

假设链表有环，链表头节点到环入口节点口有m个节点，环的入口标号为0，环上一共有k + 1个节点，那么环的最后一个节点（指入口节点的上一个节点）标号为k，假设现在慢指针第一次来到了环的0号位置，那快指针必然已经停在环的某个节点上，那么则有：

x % （k + 1） = [2 * x + m]  % （k  + 1）

x % （k + 1） = [m + x  + x] % （k  + 1）

解释一下公式，背景是慢指针在环的0位置处，**再走x步快慢指针相遇**，等号左面好理解，等号右面，因为快指针是慢指针速度的两倍，因此当慢指针走到环的0位置处，快指针已经走了2 * m，因此快指针此时是停在m % (k + 1)位置处的。

然后看结论，只要存在m + x 是（k + 1）的整数倍，那么两个指针就可以相遇。

**再证明如何找到环的入口节点：**

由上面的结论可以得知，**相遇的条件是m + x 是（k + 1）的整数倍**，因此快慢指针相遇的时候，慢指针是在环的x位置处的，再走m步会到达 (x + m) % （k + 1） == 0的位置，也就是还的入口处。但是我们不知道m是几，因此我们就再定义一个慢指针2从链表的头节点跟环中的慢指针同时出发，当相遇的时候，慢指针2已经走了m步到达了入口节点，慢指针1也会停在在环的入口节点。

```C++
/*
注意，判断链表有环时必须让slow和fast出于同一起点出发，不能然fast先走一步，也就是不能这样写：
ListNode* slow = head;
        ListNode* fast = head->next;
        while(fast && fast->next){
            if(slow == fast) return true;
            slow = slow->next;
            fast = fast->next->next;
        }
        
 这样可以判断有无环，但是不满足fast每次走两步的公式。
*/
class Solution {
public:
    ListNode *detectCycle(ListNode *head) {
        if(!head) return nullptr;
        ListNode* slow = head, *fast = head;
        while(fast && fast->next){
            slow = slow->next;
            fast = fast->next->next;
            if(slow == fast) break;
            
        }
        if(!fast || !fast->next) return nullptr;
        while(head != slow){
            head = head->next;
            slow = slow->next;
        }
        return head;
    }
};

```

### [143. 重排链表](https://leetcode-cn.com/problems/reorder-list/)（中等）√  ×注意穿针引线的写法

```C++
/*
1 注意，中点必须这么找，不管是奇数还是偶数，都要确保中点为n/2(n为最后一个节点的索引)
2 条件这么写没问题
1 2 3
4 3 
这样是不行的，因为1-4-2 然后2-3-3 然后r2->next是空的，导致循环结束，最终的结果是 1 4 2 3 3（互相指向）
1 2 
4 3 2 就可以，1-4-2    =》 2-3-nullptr

1 2 3 
5 4 3  尽管最后也会成环，但是由于最后一次r1 和r2都不是空，r2->next = next1; 这一行会把3->next 置为nulptr，所以不影响
*/
class Solution {
public:
    void reorderList(ListNode* head) {
        ListNode* mid = findMid(head);
        ListNode* newHead = reverse(mid);
        ListNode* r1 = head, *r2 = newHead;
        while(r1 && r2){ // 2
            ListNode* next1 = r1->next;
            ListNode* next2 = r2->next;
            r1->next = r2;
            r2->next = next1;
            r1 = next1;
            r2 = next2;
        }
    }
    ListNode* findMid(ListNode* head){ // 1
        ListNode* slow = head;
        ListNode* fast = head;
        while (fast->next != nullptr && fast->next->next != nullptr) {
            slow = slow->next;
            fast= fast->next->next;
        }
        return slow;
    }
    ListNode* reverse(ListNode* head){
        ListNode* pre = nullptr;
        while(head){
            ListNode* next = head->next;
            head->next = pre;
            pre = head;
            head = next;
        }
        return pre;
    }
};
```













## [剑指 Offer 36. 二叉搜索树与双向链表](https://leetcode.cn/problems/er-cha-sou-suo-shu-yu-shuang-xiang-lian-biao-lcof/)（中等-二叉树-链表）×

## [61. 旋转链表](https://leetcode.cn/problems/rotate-list/)（中等-链表）×

## [328. 奇偶链表](https://leetcode.cn/problems/odd-even-linked-list/)（中等-链表）×

## [445. 两数相加 II](https://leetcode.cn/problems/add-two-numbers-ii/)（中等-链表-栈）×

# 二分查找

https://leetcode.cn/problems/find-minimum-in-rotated-sorted-array/solution/yi-wen-dai-ni-gao-ding-er-fen-cha-zhao-j-00kj/

## [33. 搜索旋转排序数组](https://leetcode.cn/problems/search-in-rotated-sorted-array/)（中等-二分）√

其实仔细想想跟普通二分是一样的，都是算出mid然后调整l和r。



## [153. 寻找旋转排序数组中的最小值](https://leetcode.cn/problems/find-minimum-in-rotated-sorted-array/)（中等）√

```c++
class Solution {
public:
    int findMin(vector<int>& nums) {
        int l = 0, r = nums.size() - 1;
        while(l <= r){
            int mid = l + (r - l) / 2;
            if(nums[mid] < nums[r]) r = mid;
            else if (nums[mid] > nums[r]) l = mid + 1;
            else --r;
        }
        return nums[l];
    }
};
```

## [删除排序链表中的重复元素 II](https://leetcode.cn/problems/remove-duplicates-from-sorted-list-ii/)







## 最大最小

### [2517. 礼盒的最大甜蜜度](https://leetcode.cn/problems/maximum-tastiness-of-candy-basket/)！！！！！！！！

最大化最小，最小化最大

https://leetcode.cn/problems/maximum-tastiness-of-candy-basket/solutions/2031994/er-fen-da-an-by-endlesscheng-r418/



```c++
/*
思路：二分 + 贪心。 注意这里的二分不是下标，而是值。
最小甜度l：0   
最大甜度r：排序后的最大值-最小值，因为如果k取2，毫无疑问就是最大值-最小值

然后就是用二分，得到mid后，判断数组中有没有k个数能满足甜蜜度为mid，
满足的话，那么先记录下当前mid，然后有可能存在更大的甜蜜度，也就是寻找符合条件的最右边的值

1 在lambda中，首先price是被排序了的，然后这里就利用了贪心的思想：
假设1 2 5 8 13 21， mid = 8。 初始pre = 1，所以cnt初始也为1。当遍历到13时，13 - 1 >= 8, cnt = 2;
将pre设为13，然后x = 21 - 13 >= 8 ,cnt = 3 >= k。因为13与1的差已经大于8，那么21比13大，21与1的差更大与8。

*/

class Solution {
public:
    int maximumTastiness(vector<int>& price, int k) {
        int n = price.size(), res = 0;
        sort(price.begin(), price.end());  
        auto check = [&](int target){ // 1
            int pre = price[0], cnt = 1;
            for(auto p: price){
                if(p - pre >= target){
                    ++cnt;
                    pre = p;
                }
            }
            return cnt >= k;
        };
        int l = 0, r = price[n - 1] - price[0], mid = 0;
        while(l <= r){
            mid = l + (r - l) / 2;
            if(check(mid)){
                res = mid;
                l = mid + 1;
            }
            else r = mid - 1;
        }
        return res;
    }
};
```



### [2439. 最小化数组中的最大值](https://leetcode.cn/problems/minimize-maximum-of-array/)！！！

```c++

/*
思路：注意题意：我们希望的是 经过操作，得到的数组中的最大值越小越好。
      所以我们使用二分法，来寻找这个最小的 数组中的最大值。
1 核心就在这，当我们得到一个mid后，我们就是要判断，经过操作后，数组的最大值能否是mid
比如：[3,10,1,6] 假设mid = 8，那么没什么好说的，3 + 5， 10 - 5，一下就能得到最大值8

所以在 2 处，我们寻找的是符合条件的最小值；

然后当mid == 5时， hold是nums[i-1]可以帮nums[i]承载的数量，看上面的用例，
3可以帮10承载2，但是10需要的承载量为5，所以return false, 后面再判断么意义了，因为10 - 2 = 8
8 再后面遍历的过程中也只能 +1 不能 - 1了。
*/

class Solution {
public:
    int minimizeArrayValue(vector<int> &nums) {
        int l = 0, r = *max_element(nums.begin(), nums.end());
        auto check = [&](int target){ // 1
            long hold = 0;
            for(auto e: nums){
                if(e <= target)
                    hold += target - e;
                else{
                    if(e - target  > hold) return false;
                    hold -= e - target;
                }
            }
            return true;
        };
        int res = 0;
        while(l <= r){
            int mid = l + (r - l) / 2;
            if(check(mid)){ // 2
                res = mid;
                r = mid - 1;
            }
            else l = mid + 1;
        }
        return res;
    }
};

```







# 树

## [98. 验证二叉搜索树](https://leetcode.cn/problems/validate-binary-search-tree/)×

要用中序遍历。

递归如果要是有返回值，那么就要判断。

```c++
class Solution {
public:
    long pre = LONG_MIN;
    bool isValidBST(TreeNode* root) {
        if(!root) return true;
        if(!isValidBST(root->left)) return false;
        if(root->val <= pre) return false;
        pre = root->val;
        if(!isValidBST(root->right)) return false;
        return true;
    }
};
```



## [剑指 Offer 68 - II. 二叉树的最近公共祖先](https://leetcode.cn/problems/er-cha-shu-de-zui-jin-gong-gong-zu-xian-lcof/)（简单） ×

思路就是DFS，如果找到了p或q或遍历到空节点，那就返回；

如果left返回为空，那就返回right，有可能right也为空，也有可能right找到了p或q；

只有当某个节点的left和right都不为空时，说明它是最近公共祖先。一直向上返回。

如果说在根节点的左子树已经找到了答案，那么根节点的右子树一定是返回null。

```c++
class Solution {
public:
    TreeNode* lowestCommonAncestor(TreeNode* root, TreeNode* p, TreeNode* q) {
        if(root == nullptr || root == p || root == q) return root;
        TreeNode *left = lowestCommonAncestor(root->left, p, q);
        TreeNode *right = lowestCommonAncestor(root->right, p, q);
        if(left == nullptr) return right;
        if(right == nullptr) return left;
        return root;
    }
};
```



## [662. 二叉树最大宽度](https://leetcode.cn/problems/maximum-width-of-binary-tree/)(中等) ×！×

有很多点需要注意：

适配器一律不支持初始化列表；

max 比较的时候，两个类型一定要相等；

push_back只接受对象，或者能隐式转换的单参；

emplace_pack可以接受任意参数，因为会调用完美转发执行构造函数

```c++
class Solution {
public:
    int widthOfBinaryTree(TreeNode* root) {
        unsigned long long res = 1;
        vector<pair<TreeNode*, unsigned long long>> vec;
        vec.push_back({root, 1});
        while(vec.size()){
            vector<pair<TreeNode*, unsigned long long>> tmp;
            for(auto &[node, index]: vec){
                if(node->left) tmp.push_back({node->left, index * 2});
                if(node->right) tmp.push_back({node->right, index * 2 + 1});
            }
            res = max(res, vec.back().second - vec.front().second + 1);
            vec = move(tmp);
        }
        
        return res;
    }
    
};
```

## [958. 二叉树的完全性检验](https://leetcode.cn/problems/check-completeness-of-a-binary-tree/)（中等-二叉树-层次遍历）×



## 二叉树路径的问题

https://leetcode.cn/problems/binary-tree-maximum-path-sum/solution/yi-pian-wen-zhang-jie-jue-suo-you-er-cha-kcb0/

**二叉树路径的问题**大致可以分为两类：

### 1 自顶向下

顾名思义，就是从某一个节点(**不一定是根节点**)，从上向下寻找路径，到某一个节点(**不一定是叶节点**)结束。
具体题目如下：

面试题 04.12. 求和路径

路径总和

路径总和 II

路径总和 III

从叶结点开始的最小字符串

而继续细分的话还可以分成一般路径与给定和的路径。
### 模板一（一般路径）


```cpp
vector<vector<int>>res;
vector<int> path;
void DFS(TreeNode*root)
{
    if(!root) return;  
    path.push_back(root->val);  
    if(!root->left && !root->right) //如果到叶节点  
    {
        res.push_back(path);
        //return; // 如果要回溯的话，就不能提前return
    }
    DFS(root->left); 
    DFS(root->right);
    path.pop_back(); // 这里要回溯
}

```
### [257. 二叉树的所有路径](https://leetcode.cn/problems/binary-tree-paths/)

回溯比较麻烦就不回溯了。
使用模板一:

```c++
class Solution {
public:
    vector<string> res;
    vector<string> binaryTreePaths(TreeNode* root) {
        dfs(root, "");
        return res;
    }
    void dfs(TreeNode* root, string path){
        if(!root) return;
        path += to_string(root->val);
        if(!root->left && !root->right){
            res.push_back(path);
            return;
        }
        dfs(root->left, path + "->");
        dfs(root->right, path + "->");

    }
};
```

### 模板二（给定和路径）

强调一下，自顶向下的模板，path其实是有必要回溯的，否则一直使用值传递，会产生大量的容器拷贝，相当耗内存。
```cpp
    vector<vector<int>> res;
    vector<int> path;
    void DFS(TreeNode* root, int targetSum){
        if(!root) return;

        path.push_back(root->val);
        targetSum -= root->val;
       
        if (!root->left && !root->right && targetSum == 0){
            res.push_back(path);
            // return;  不能在这里返回，因为path还没有回溯
            // 把回溯语句写到return 前面也不行，因为到了叶节点targetSum != 0的情况也是要回溯的，
            // 必须写到最后面
        }
        DFS(root->left, targetSum);
        DFS(root->right, targetSum);

        // targetSum += root->val; // 强调一下，这里不需要回溯，因为targetSum是值传递不是引用传递
        path.pop_back(); // path 是全局变量需要回溯
    }
};
```

### [112. 路径总和](https://leetcode.cn/problems/path-sum/)√

### [113. 路径总和 II](https://leetcode.cn/problems/path-sum-ii/)（中等）√

使用模板二。
```c++
class Solution{
public:
    vector<vector<int>> res;
    vector<int> path;
    vector<vector<int>> pathSum(TreeNode* root, int targetSum){
        DFS(root, targetSum);
        return res;
    } 
    void DFS(TreeNode* root, int targetSum){
        if(!root) return;

        path.push_back(root->val);
        targetSum -= root->val;
        

        if (!root->left && !root->right && targetSum == 0){
            res.push_back(path);
            // return;  不能在这里返回，因为path还没有回溯
        }
        DFS(root->left, targetSum);
        DFS(root->right, targetSum);

        // targetSum += root->val; // 强调一下，这里不需要回溯，因为targetSum是值传递不是引用传递
        path.pop_back(); // path 是全局变量需要回溯
    }
};
```



[437. 路径总和 III](https://leetcode.cn/problems/path-sum-iii/submissions/) 
使用模板二。

pathSum本身也是一个递归，目的是递归的以每个节点为根节点去DFS。
```c++
class Solution{
public:
    int res = 0;
    int pathSum(TreeNode* root, int targetSum){
        if(!root) return 0;
        // 以root为根节点查找路径
        DFS(root, targetSum);
        // 递归到子节点，以子节点为根节点去查找路径
        pathSum(root->left, targetSum);
        pathSum(root->right, targetSum);
        return res;
    } 
    void DFS(TreeNode* root, long targetSum){
        if(!root) return;
        targetSum -= root->val;
        //注意不要return,因为节点val可能为负，所以加加减减可能又得到一条路径  这里试一下
        if(targetSum == 0) ++res;
        DFS(root->left, targetSum);
        DFS(root->right, targetSum);
    }
};
```

[988. 从叶结点开始的最小字符串](https://leetcode.cn/problems/smallest-string-starting-from-leaf/)


https://leetcode.cn/problems/diameter-of-binary-tree/solution/yi-pian-wen-zhang-jie-jue-suo-you-er-cha-6g00/



### 2 非自顶向下：

就是从任意节点到任意节点的路径，不需要自顶向下：

**模板：后序遍历**

```c++
int res=0;
int maxPath(TreeNode *root) //以root为路径起始点的最长路径
{
    if (!root) return 0;
    int left=maxPath(root->left);
    int right=maxPath(root->right);
    res = max(res, left + right + root->val); //更新全局变量  
    return max(left, right) + 1;   //返回左右路径较长者
}
```

### [124. 二叉树中的最大路径和](https://leetcode.cn/problems/binary-tree-maximum-path-sum/)（困难）×！！！！

```c++
class Solution {
public:
    int maxPath = INT_MIN;
    int maxPathSum(TreeNode* root) {
        getMax(root);
        return maxPath;
    }
    int getMax(TreeNode* root){
        if(!root) return 0;
        int left = max(0, getMax(root->left));
        int right = max(0, getMax(root->right));
        maxPath = max(left + right + root->val, maxPath); // 1 
        return max(left, right) + root->val; // 2
    }
};
```

### [543. 二叉树的直径](https://leetcode.cn/problems/diameter-of-binary-tree/)（简单）×

跟124几乎一样。

```c++
/*
left、right 是任意节点的左右子树的深度；
左右子树的深度相加，就是经过该节点的路径长度；
算出left、right后,先更新res，比如left、right都是0，那么res就是0，因为没有边，路径长度是0。
 */
class Solution {
public:
    int maxPath = INT_MIN;
    int diameterOfBinaryTree(TreeNode* root) {
        getMax(root);
        return maxPath;
    }
    int getMax(TreeNode* root){
        if(!root) return 0;
        int left = getMax(root->left);
        int right = getMax(root->right);
        maxPath = max(maxPath, left + right);
        return max(left, right) + 1;
    }
};
```

## [450. 删除二叉搜索树中的节点](https://leetcode.cn/problems/delete-node-in-a-bst/)（中等-二叉搜索树）×

## [剑指 Offer 26. 树的子结构](https://leetcode.cn/problems/shu-de-zi-jie-gou-lcof/)（中等-二叉树）×

## [114. 二叉树展开为链表](https://leetcode.cn/problems/flatten-binary-tree-to-linked-list/)（中等-二叉树）×

## 



# 深度优先搜索

## 岛屿问题

## [200. 岛屿数量](https://leetcode.cn/problems/number-of-islands/)(中等-DFS) √×√√

遍历一块陆地，就将它沉底。

```c++
class Solution {
public:
    int x[4] = {0, 1, 0, -1};
    int y[4] = {1, 0, -1, 0};
    int numIslands(vector<vector<char>>& grid) {
        int res = 0;
        for(int i = 0; i < grid.size(); ++i){
            for(int j = 0; j < grid[0].size(); ++j){
                if(grid[i][j] == '1'){
                    dfs(grid, i, j);
                    ++res;
                }
            }
        }
        return res;
    }
    void dfs(vector<vector<char>>& grid, int i, int j){
        if(!inArea(grid, i, j)) return;
        if(grid[i][j] == '0') return;
        grid[i][j] = '0';
        for(int t = 0; t < 4; ++t)
            dfs(grid, i + x[t], j + y[t]);
        
    }
    bool inArea(vector<vector<char>>& grid, int i, int j){
        return i >= 0 && i < grid.size() && j >= 0 && j < grid[0].size();
    }
};
```

#### [695. 岛屿的最大面积](https://leetcode.cn/problems/max-area-of-island/)（中等-DFS）√√√

```c++
class Solution {
public:
    int maxAreaOfIsland(vector<vector<int>>& grid) {
        int res = 0, area = 0;
        int row = grid.size();
        int col = grid[0].size();
        for(int i = 0; i < row; ++i){
            for(int j = 0; j < col; ++j){
                if(grid[i][j] == 1){
                    area = dfs(grid, i, j);
                    res = max(res, area);
                }
            }
        }
        return res;
    }
    int dfs(vector<vector<int>>& grid, int i, int j){
        if(!inArea(grid, i, j)) return 0;
        if(grid[i][j] != 1) return 0;
        grid[i][j] = 2;
        return 1 + dfs(grid, i - 1, j)+
               dfs(grid, i + 1, j)+
               dfs(grid, i, j - 1)+
               dfs(grid, i, j + 1);
    }

    bool inArea(vector<vector<int>>& grid, int i, int j){
        return i >= 0 && i < grid.size() && j >= 0 && j < grid[0].size();
    }
};
```

#### [463. 岛屿的周长](https://leetcode.cn/problems/island-perimeter/)(简单) √

```c++
class Solution {
public:
    int islandPerimeter(vector<vector<int>>& grid) {
        for(int r = 0; r < grid.size(); ++r){
            for(int c = 0; c < grid[0].size(); ++c){
               if(grid[r][c] == 1)
                    return dfs(grid, r, c); 
            }
        }
        return 0;
    }

    int dfs(vector<vector<int>>& grid, int r, int c){
        if(!inArea(grid, r, c)) return 1;
        if(grid[r][c] == 0) return 1;

        if (grid[r][c] == 2) {
            return 0;
        }
        grid[r][c] = 2; // 必须要标记，因为向左一个节点，它还会向右遍历回来。会造成循环递归
        return dfs(grid, r - 1, c)+
               dfs(grid, r + 1, c)+
               dfs(grid, r, c - 1)+
               dfs(grid, r, c + 1);
    }

    bool inArea(vector<vector<int>>& grid, int r, int c){
        return r >= 0 && r < grid.size() && c >= 0 && c < grid[0].size();
    }

};
```

## 拓扑排序

#### [207. 课程表](https://leetcode.cn/problems/course-schedule/)（中等）×

才弄懂，这道题是拓扑排序，起始可以理解为判断一棵树是否存在环。

因为一道题可能有两棵树，因为必须从每个节点开始都遍历一次。

那么在遍历的时候，首先如果flags[i]是0，说明这个节点没来过，标记为1；如果没有任何后继节点，说明从这个节点开始的DFS无环，所以就标记为-1，返回true。

```c++
class Solution {
public:
    bool canFinish(int numCourses, vector<vector<int>>& prerequisites) {
        vector<vector<int>> adjacent(numCourses); // 邻接表
        vector<int> flags(numCourses); // 用于判断节点是否被访问
        for(int i = 0; i < prerequisites.size(); ++i){
            // 邻接表下标为某个课程，对应的数组为其后置课程
            adjacent[prerequisites[i][1]].push_back(prerequisites[i][0]);
        }
        for(int i = 0; i < numCourses; ++i){
            if(!DFS(i, adjacent, flags)) return false;
        }
        return true;
    }

    bool DFS(int i, vector<vector<int>> &adjacent, vector<int> &flags){
        if(flags[i] == -1) return true; // 如果是-1 说明这个节点后面不存在环
        if(flags[i] == 1) return false;  // 如果是1 说明这个节点已经遍历过，存在环
        flags[i] = 1; // 本来是0，说明没遍历过，标记为1（已遍历）
        for(auto j: adjacent[i])
            // 如果发现从哪个节点开始的DFS返回的是false，说明是有环的
            if(!DFS(j, adjacent, flags)) return false; 
        // 如果某个节点的邻接数组为空，说明已经没有后置课程，无环
        flags[i] = -1; // 无环
        return true;
    }
};
```

## 图 

### [797. 所有可能的路径](https://leetcode.cn/problems/all-paths-from-source-to-target/)（中等）√

```c++
class Solution {
public:
    vector<vector<int>> res;    
    vector<vector<int>> allPathsSourceTarget(vector<vector<int>>& graph) {
        // 邻接表已经给了
        DFS(0, graph, vector<int>());
        return res;
    }
    void DFS(int i, vector<vector<int>>& graph, vector<int> path){
        path.push_back(i);
        if(i == graph.size() - 1){
            res.push_back(path);
            return;
        }
        for(auto j: graph[i]){
            DFS(j, graph, path);
        }
        return;
    }
};
```

# 广度优先搜索



https://leetcode.cn/problems/binary-tree-level-order-traversal/solution/bfs-de-shi-yong-chang-jing-zong-jie-ceng-xu-bian-l/



## 层序遍历

### [662. 二叉树最大宽度](https://leetcode.cn/problems/maximum-width-of-binary-tree/)（中等）×

```c++

```







## 最短路径

### [1162. 地图分析](https://leetcode.cn/problems/as-far-from-land-as-possible/)（中等）×

就是说，所有陆地先都入队，同时进行BFS。那么最后被遍历到的海洋，一定是离所有陆地最远的。

也就是符合题目中说的，这个海洋单元格离它最近的陆地单元距离最大~

```c++
class Solution {
public:
    int maxDistance(vector<vector<int>>& grid) {
        int n = grid.size();
        queue<pair<int, int>> que;
        for(int r = 0; r < n; ++r){
            for(int c = 0; c < n; ++c){
                // 多源BFS 把所有为陆地的节点都先入队
                if(grid[r][c] == 1){
                    que.push({r, c});
                }
            }
        }
        // 如果只有陆地或海洋，return -1
        if(que.empty() || que.size() == n * n) return -1;

        int moves[4][2] = {{-1, 0}, {1, 0}, {0, -1}, {0, 1}};
        int distance = -1;
        while(!que.empty()){
            ++distance; // 记录陆地和海洋的距离
            int n = que.size();
            for(int i = 0; i < n; ++i){
                auto coord = que.front();
                que.pop();
                int r = coord.first;
                int c = coord.second;
                // 向当前陆地的四面八方搜索(只搜索没遍历过的)
                for(auto &a: moves){
                    int r2 = r + a[0];
                    int c2 = c + a[1];
                    if(inArea(grid, r2, c2) && grid[r2][c2] == 0){
                        grid[r2][c2] = 2;
                        que.push({r2, c2});
                    }
                }
            }
        }
        return distance;
    }
    bool inArea(vector<vector<int>>& grid, int r, int c){
        return r >= 0 && r < grid.size() && c >= 0 && c < grid[0].size();
    }
};
```





### [994. 腐烂的橘子](https://leetcode.cn/problems/rotting-oranges/)（中等）×



### [310. 最小高度树](https://leetcode.cn/problems/minimum-height-trees/)（中等）×



## 拓扑排序

[207. 课程表](https://leetcode.cn/problems/course-schedule/)（中等）×

1 先统计邻接数组和入度数组

2 将入度为0的课程入队

3 得到队头，出队（选课，count++），得到队头的后置课程，将他们的入度-1， 如果入度变为0，说明可选，入队

4 如果最终选课数量等于numCourses，说明该选的都选了。

```c++
class Solution {
public:
    bool canFinish(int numCourses, vector<vector<int>>& prerequisites) {
        // 邻接表,下标为某个课程，对应的数组为其后置课程
        vector<vector<int>> adjacent(numCourses); 
        vector<int> inDegree(numCourses); // 入度数组
        queue<int> que; // 队列用来BFS
        for(int i = 0; i < prerequisites.size(); ++i){
            adjacent[prerequisites[i][1]].push_back(prerequisites[i][0]);
            inDegree[prerequisites[i][0]]++;
        }
        for(int i = 0; i < numCourses; ++i)
            if(inDegree[i] == 0) que.push(i); // 先把入度为0的课程全部入队
        
        int count = 0; // 记录已经选择的课程数量
        while(que.size()){
            int selected = que.front();
            que.pop();
            ++count; // 已选课程数量+1
            vector<int> courses(std::move(adjacent[selected]));
            if(courses.size()){ // 雀实有后置课程
                for(int i = 0; i < courses.size(); ++i){
                    --inDegree[courses[i]]; // 是后置课程的入度减一！！！！！！
                    if(inDegree[courses[i]] == 0) que.push(courses[i]);
                }
            }
        }
        if(count == numCourses) return true;
        return false;       
    }    
};
```



# 滑动窗口

> 滑动窗口题目:
>
> 3. 无重复字符的最长子串
>
> 4. 串联所有单词的子串
>
> 5. 最小覆盖子串
>
> 6. 至多包含两个不同字符的最长子串
>
> 7. 长度最小的子数组√
>
> 8. 滑动窗口最大值
>
> 9. 字符串的排列
>
> 10. 最小区间
>
> 11. 最小窗口子序列







## [剑指 Offer 57 - II. 和为s的连续正数序列](https://leetcode.cn/problems/he-wei-sde-lian-xu-zheng-shu-xu-lie-lcof/)（简单）×

https://leetcode.cn/problems/he-wei-sde-lian-xu-zheng-shu-xu-lie-lcof/solution/shi-yao-shi-hua-dong-chuang-kou-yi-ji-ru-he-yong-h/

```c++
class Solution {
public:
    vector<vector<int>> findContinuousSequence(int target) {
        int sum = 0; // 滑动窗口中数字的和
        vector<vector<int>> res;
        for(int left(1), right(1); left <= target / 2;){
            if(sum < target){
                sum += right;
                ++right;
            }else if(sum > target){
                sum -= left;
                ++left;
            }else{
                vector<int> temp;
                for(int i = left; i < right; ++i){
                    temp.push_back(i);
                }
                res.push_back(temp);
                sum -= left;
                ++left;
            }
        }
        return res;
    }
};
```

## [209. 长度最小的子数组](https://leetcode.cn/problems/minimum-size-subarray-sum/)

## [594. 最长和谐子序列](https://leetcode.cn/problems/longest-harmonious-subsequence/)（简单）×  √

首先要排序，因为这个答案只能是22223333或者33334444 ,排序只是为了用滑动窗口，最终把多余的摘出去就可以了。

```c++
class Solution {
public:
    int findLHS(vector<int>& nums) {
        sort(nums.begin(), nums.end());
        int res = 0;
        for(int slow(0), fast(0); fast < nums.size(); ++fast){
            while(nums[fast] - nums[slow] > 1) ++slow;
            if(nums[fast] - nums[slow] == 1) res = max(res, fast - slow + 1);
        }
        return res;
    }
};

```

## [3. 无重复字符的最长子串](https://leetcode.cn/problems/longest-substring-without-repeating-characters/) （中等-滑动窗口-哈希表）  × √×√√！

## [219. 存在重复元素 II](https://leetcode.cn/problems/contains-duplicate-ii/)（简单）√

+ 1 首先让窗口大小不要超过限制；
+ 2 根据新元素判断/更新结果；
+ 3 将新元素加入窗口（根据题意，各种）；

1 2 3 步顺序不是固定的，要具体情况具体分析，如果是本题，新元素不能出现在窗口内，因为是判断新元素在不在窗口里！所以要更新结果之后再把新元素加进窗口。

但是对于239.滑动窗口最大值，它的窗口大小是固定的，因此必须把新元素加进窗口满足窗口大小要求，再更新结果。

```c++
class Solution {
public:
    bool containsNearbyDuplicate(vector<int>& nums, int k) {
        int n = nums.size();
        unordered_set<int> uset;
        for(int l(0), r(0); r < n; ++r){
            if(r - l > k){
                uset.erase(nums[l]);
                ++l;            
            }
            if(uset.find(nums[r]) != uset.end()) return true;
            uset.insert(nums[r]);
        }
        return false;
    }
};
```

## [239. 滑动窗口最大值](https://leetcode.cn/problems/sliding-window-maximum/)（困难）×××



## [76. 最小覆盖子串](https://leetcode.cn/problems/minimum-window-substring/)（困难-滑动窗口）×！×



## [567. 字符串的排列](https://leetcode.cn/problems/permutation-in-string/)（高频）×！

https://leetcode.cn/problems/permutation-in-string/solution/zhu-shi-chao-xiang-xi-de-hua-dong-chuang-rc7d/

下次看一下哈希表中val等于0，可key压根不存在的区别。

```c++
/*
这道题跟76题其实是很像的，但是这题是要判断s2中有没有s1的排列，不要求顺序一样，但是要连续，这里我没想到要怎么解。
看题解是先在s2中定义一个s1.size()大小的窗口(因为要连续，那么窗口必须等于s1的大小)
1：如果两个哈希表相等，说明窗口等于s1的大小，每个元素出现的次数也相同。
2：如果要判断两个哈希表是否相等，那么当val为0时，必须要用erase移除该key！！
*/
```

```C++
class Solution {
public:
    bool checkInclusion(string s1, string s2) {
        int n1 = s1.size(), n2 = s2.size();
        unordered_map<char, int> umap_s1, umap_s2;
        for(auto c: s1)
            umap_s1[c]++;  
        if(n1 > n2) return false; // 如果len_s1 > len_s2肯定就是不行了
        for(int i = 0; i < n1; i++)
            umap_s2[s2[i]]++; // 记录s2的初始map
        for(int l(0), r(n1 - 1); r < n2; ){
            if(umap_s1 == umap_s2) return true; // 1
            if(--umap_s2[s2[l]] == 0) umap_s2.erase(s2[l]); // 2
            l++;
            r++;
            umap_s2[s2[r]]++;
        }
        return false;
    }
};
```

```c++
/*
一个哈希表：umap记录的是s2中需要出现的字符的次数。
1：val都等于0说明窗口内不多不少都是t中需要出现字符。
*/
class Solution {
public:
    bool checkInclusion(string s1, string s2) {
        auto cmp = [](unordered_map<char, int>& umap){
            for(auto p: umap){
                if(p.second != 0) return false;
            }
            return true;
        };
        int n1 = s1.size(), n2 = s2.size();
        unordered_map<char, int> umap;
        if(n1 > n2) return false; // 如果len_s1 > len_s2肯定就是不行了
        for(int i = 0; i < n1; i++){
            ++umap[s1[i]];  
            --umap[s2[i]]; 
        }    
        for(int l(0), r(n1 - 1); r < n2; ){
            if(cmp(umap)) return true; // 1
            ++umap[s2[l]];
            l++;
            r++;
            --umap[s2[r]];
        }
        return false;
    }
};
```

## [438. 找到字符串中所有字母异位词](https://leetcode.cn/problems/find-all-anagrams-in-a-string/)（中等-滑动窗口-哈希表）×√

跟567题差不多，只是这个多了一个条件：记录每个排列的起始坐标。

## [1004. 最大连续1的个数 III](https://leetcode.cn/problems/max-consecutive-ones-iii/)（中等-滑动窗口）×

## [395. 至少有 K 个重复字符的最长子串](https://leetcode.cn/problems/longest-substring-with-at-least-k-repeating-characters/)（中等-分治-滑动窗口）×！×

# 前缀和+哈希表

## [303. 区域和检索 - 数组不可变](https://leetcode.cn/problems/range-sum-query-immutable/)（简单）√

前缀和：preSum[i]表示nums前i个元素的和（nums[0:i-1]）。

```c++
class NumArray {
public:
    vector<int> preSum;
    NumArray(vector<int>& nums):preSum(nums.size()+1) {
        preSum[0] = 0;
        for(int i = 0; i < nums.size(); ++i){
            preSum[i + 1] = preSum[i] + nums[i];
        }
    }
    
    int sumRange(int left, int right) {
        return preSum[right + 1] - preSum[left]; 
    }
};
```

## [560. 和为 K 的子数组](https://leetcode.cn/problems/subarray-sum-equals-k/) （中等-前缀和+哈希表）×！！c√

前缀和和哈希表结合，是为了动态的，一次遍历统计子数组的数量。**哈希表记录的是某个前缀和的数量。**

## [525. 连续数组](https://leetcode.cn/problems/contiguous-array/)×！！c√

跟560很像，但是525的哈希表的value记录的是下标，目的是得到最长子数组。

但是核心都是动态的统计前缀和，然后获得其他信息。

## [209. 长度最小的子数组](https://leetcode.cn/problems/minimum-size-subarray-sum/)（前缀和 + 二分）？

## [523. 连续的子数组和](https://leetcode.cn/problems/continuous-subarray-sum/)（前缀和-哈希-同余定理）×







# 区间问题

## [252. 会议室](https://leetcode.cn/problems/meeting-rooms/) √

```C++
class Solution {
public:
    bool canAttendMeetings(vector<vector<int>>& intervals) {
        sort(intervals.begin(), intervals.end(), 
            [](vector<int> &lhs, vector<int> &rhs){ return lhs[0] < rhs[0]; });
        for(int i = 1; i < intervals.size(); ++i){
            if(intervals[i][0] < intervals[i-1][1]) return false;
        }
        return true;

    }
};
```



## [435. 无重叠区间](https://leetcode.cn/problems/non-overlapping-intervals/)（中等-区间问题）×

这种二维数组的题首先都要按照一个维度排序，我选择按照起始端点排序，然后记录重叠区间（要删除区间的个数）。

当发生重叠时，想象一下，要将end更新为较小的右端点。因为题目说了， **需要移除区间的最小数量**，所以删除较长的区间才能减少冲突的可能，使要移除的区间数量最少。

比如：[[1,3],[2,5],[3,4],[1,3]]，0和1冲突了，但end肯定要更新为3，把1删掉。2才不会和0冲突。如果删掉0，那么1和2又冲了，又要删掉2，就不对了。

```c++
class Solution {
public:
    int eraseOverlapIntervals(vector<vector<int>>& intervals) {
        if(intervals.size() == 0) return 0;
        sort(intervals.begin(), intervals.end(), [](vector<int>& l, vector<int>& r){
            return l[0] < r[0];
        });
        int cnt = 0;
        int end = intervals[0][1];
        for(int i = 1; i < intervals.size(); ++i){
            if(intervals[i][0] >= end)
                end = intervals[i][1];
            else{
                end = min(end, intervals[i][1]);
                ++cnt;
            }
        }
        return cnt;
    }
};
```

## [452. 用最少数量的箭引爆气球](https://leetcode.cn/problems/minimum-number-of-arrows-to-burst-balloons/)（中等-区间问题）√

跟435一样。

```c++
class Solution {
public:
    int findMinArrowShots(vector<vector<int>>& points) {
        sort(points.begin(), points.end(), [](vector<int>& l, vector<int>& r){
            return l[0] < r[0];
        });
        int cnt = 1;
        int end = points[0][1];
        for(int i = 1; i < points.size(); ++i){
            if(points[i][0] > end){
                ++cnt;
                end = points[i][1];
            }else
                end = min(end, points[i][1]);
        }
        return cnt;
    }
};
```







## [1288. 删除被覆盖区间](https://leetcode.cn/problems/remove-covered-intervals/) ×

```c++

```









# 双指针



## 缩减搜索空间

### [167. 两数之和 II - 输入有序数组](https://leetcode.cn/problems/two-sum-ii-input-array-is-sorted/)（中等）√

### [11. 盛最多水的容器](https://leetcode.cn/problems/container-with-most-water/)（中等-双指针）√√√

## [986. 区间列表的交集](https://leetcode.cn/problems/interval-list-intersections/)（中等-双指针）√√ 

看图跟着做就行了，首先找左边界，再找右边界。
然后右边界小的更新到下一段。
注意如果fast < slow 的情况，没有交集。

```c++
class Solution {
public:
    vector<vector<int>> intervalIntersection(vector<vector<int>>& firstList, vector<vector<int>>& secondList) {
        vector<vector<int>> res;
        int slow = 0, fast = 0;
        for(int i(0), j(0); i < firstList.size() && j < secondList.size(); ){
            slow = max(firstList[i][0], secondList[j][0]);
            fast = min(firstList[i][1], secondList[j][1]);
            if(slow <= fast)
                res.push_back({slow, fast});
            if(firstList[i][1] < secondList[j][1]) ++i;
            else ++j;
        }
        return res;
    }
};
```

## [面试题 01.06. 字符串压缩](https://leetcode.cn/problems/compress-string-lcci/)（简单-双指针）√

```c++
class Solution {
public:
    string compressString(string S) {
        if(S.size() == 0) return "";
        string res;
        int slow(0), fast(0);
        for(; fast < S.size(); ++fast){
            if(S[fast] != S[slow]){
                res.push_back(S[slow]);
                res += to_string(fast - slow);
                slow = fast;
            }
        }
        res.push_back(S[slow]);
        res += to_string(fast - slow);
        return S.size() <= res.size() ? S : res;
    }
};
```

## [16. 最接近的三数之和](https://leetcode.cn/problems/3sum-closest/)（中等-双指针）×



# 单调栈

## [739. 每日温度](https://leetcode.cn/problems/daily-temperatures/)（中等-单调栈）√×

## [402. 移掉 K 位数字](https://leetcode.cn/problems/remove-k-digits/)（单调栈）××







## 

# 并查集

并查集就是描述图中连通分量的；也可以查找两个节点是否连通。

## [547. 省份数量](https://leetcode.cn/problems/number-of-provinces/)（中等）×√

```c++
class UnionFind{
public:
    // 初始化 初始状态每个节点的父节点是自己
    UnionFind(int n): size(n), conn(n){
        for(int i = 0; i < size; ++i)
            father[i] = i;
    }
    // 查找节点的父节点
    int find(int x){
        int root = x;
        while(father[root] != root) root = father[root];
        // 到这里root已指向x的根节点 接下来做路径压缩
        // 其实就是把x到root这颗子树上的节点全不挂到根节点下，
        // 下次再查找就是O(1)复杂度
        while(x != root){
            int ori_father = father[x];
            father[x] = root;
            x = ori_father;
        }
        return root;
    }

    bool isConnected(int x, int y){ return find(x) == find(y); };

    void join(int x, int y){
        // 先分别查找x和y的根节点
        int root_x = find(x);
        int root_y =find(y);
        // 如果他们的根结点不同，说明它们不连通，就将它们连通起来
        if(root_x != root_y){
            father[root_x] = root_y;
            --conn;
        }
            
        // 否则就是连通的，什么都不做
    }
    void insert(int x){
        if(!father.count(x)){
            father[x] = x;
            ++size;
            ++conn;
        }
    }
    int getConn(){ return conn; }

private:
    unordered_map<int, int> father;
    int size;
    int conn; // 记录连通分量的个数
};


class Solution {
public:
    int findCircleNum(vector<vector<int>>& isConnected) {
        int n = isConnected.size();
        UnionFind uf(n);
        for (int i = 0; i < n; i++) {
            for (int j = 0; j < n; j++) {
                if (isConnected[i][j]) uf.join(i, j);
            }
        }
        return uf.getConn();
    }
};
```



## [128. 最长连续序列](https://leetcode.cn/problems/longest-consecutive-sequence/)！！！！！高频手撕

https://leetcode.cn/problems/longest-consecutive-sequence/solution/xiao-bai-lang-ha-xi-ji-he-ha-xi-biao-don-j5a2/





# 模拟题





## 矩形

### [836. 矩形重叠](https://leetcode.cn/problems/rectangle-overlap/)！！

```c++
/*
x1: 0 y1: 1  x2: 2  y2 :3
判断重叠：最大的左端点必须 必须小于最小的右端点
*/

class Solution {
public:
    bool isRectangleOverlap(vector<int>& rec1, vector<int>& rec2) {
        return (max(rec1[0], rec2[0]) < min(rec1[2], rec2[2]) &&
                max(rec1[1], rec2[1]) < min(rec1[3], rec2[3]));
    }
};

```



### [223. 矩形面积](https://leetcode.cn/problems/rectangle-area/)√

总面积 - IOU

![](https://assets.leetcode.com/uploads/2021/05/08/rectangle-plane.png)

```c++
class Solution {
public:
    int computeArea(int ax1, int ay1, int ax2, int ay2, int bx1, int by1, int bx2, int by2) {
        int total_s = (ax2 - ax1) * (ay2 - ay1) + (bx2 - bx1) * (by2 - by1); // 总面积
        int lx = max(ax1, bx1), rx = min(ax2, bx2);
        int ly = max(ay1, by1), ry = min(ay2, by2);
        if(lx >= rx || ly >= ry) return total_s; // 判断是否不重叠
        else{
            int iou = (rx - lx) * (ry - ly);
            return total_s - iou;
        }
    }
};
```











## [59. 螺旋矩阵 II](https://leetcode.cn/problems/spiral-matrix-ii/)（中等）√×

```c++
/*
注意两个易错点：
1 i是从1 到 n * n
2 这里的越界条件，小于0要拐弯，大于n要拐弯，已访问过要拐弯
*/

class Solution {
public:
    int x[4] = {0, 1, 0, -1};
    int y[4] = {1, 0, -1, 0};
    vector<vector<int>> generateMatrix(int n) {
        vector<vector<int>> res(n, vector<int>(n));
        int r = 0, c = 0, t = 0;
        for(int i = 1; i <= n * n; ++i){ // 1
            res[r][c] = i;
            int new_r = r + x[t];
            int new_c = c + y[t];
            if(new_r < 0 || new_c < 0 || new_r >= n || new_c >= n || res[new_r][new_c] != 0){ // 2
                t = (t + 1) % 4;
                new_r = r + x[t];
                new_c = c + y[t];
            }
            r = new_r;
            c = new_c;
        }
        return res;

    }
};
```



## [498. 对角线遍历](https://leetcode.cn/problems/diagonal-traverse/)（中等-矩阵模拟）××

```c++
/*
1 一共 m + n - 1条对角线；
2 如果对角线的索引是偶数，说明是向上的；
3 那么 起始 横坐标x：当i < m 时， x就等于i； 否则就等于m - 1。纵坐标：对纵坐标来说，因为x + y ==！！！！！！ i，所以y直接算就好了！！
4 如果对角线的索引是奇数，说明是向下的；
5 那么 起始 横坐标x：当i < 列数n，那么i一定是0，当i >= 列数n，那么纵坐标一定是n-1，那么根据i =  x + y, x又能被算出来了

*/
class Solution {
public:
    vector<int> findDiagonalOrder(vector<vector<int>>& mat) {
        int m = mat.size(), n = mat[0].size();
        vector<int> res;
        for (int i = 0; i < m + n - 1; i++) { // 1
            if (i % 2 == 0) { // 2
                int x = i < m ? i : m - 1; // 3
                int y = i < m ? 0 : i - m + 1;
                while (x >= 0 && y < n) 
                    res.emplace_back(mat[x--][y++]);
            }else{ // 4
                int x = i < n ? 0 : i - n + 1; // 5
                int y = i < n ? i : n - 1;
                while (x < m && y >= 0) 
                    res.emplace_back(mat[x++][y--]);
            }
        }
        return res;
    }
};

```



## [670. 最大交换](https://leetcode.cn/problems/maximum-swap/)（超高频）×！

https://leetcode.cn/problems/maximum-swap/solution/by-muse-77-hwnt/

会了就不难，难就不会，c。

先把num转成str，而后定义一个maxIndexs数组，maxIndexs[i]的含义是，倒叙遍历str，str[i]之后(包括str[i])的最大值的下标。

比如             2 7 3 6

maxIndexs   1 1 3 3

然后再正序遍历数组，当第一个str[i] != str[maxIndexs[i]]出现时，说明可以交换成最大值

```c++
class Solution {
public:
    int maximumSwap(int num) {
        // num : 2 7 3 6
        string str(to_string(num));
        vector<int> maxIndexs(str.size());
        int index = str.size() - 1;
        for(int i = str.size() - 1; i >= 0; --i){
            if(str[i] > str[index]) index = i; // 这里不能是>=，因为当用例为1993时 要保证1和最右边的9交换，调试一下就懂了
            maxIndexs[i] = index;
        }
        for(int i = 0; i < str.size(); ++i){
            // if(maxIndexs[i] != i){ !!!!!!!!!!!! 不能这样判断！！因为 假设 8 6 3 6 明明 3 和 6 应该交换
            // 但是maxIndexs[2] 是等于3 的，所以等于没换
            if(str[i] != str[maxIndexs[i]]){
                swap(str[i], str[maxIndexs[i]]);
                break;
            }
        }
        
        return atoi(str.c_str());
    }
};

```



## [384. 打乱数组](https://leetcode.cn/problems/shuffle-an-array/)（中等-模拟-洗牌算法）×

## [剑指 Offer 62. 圆圈中最后剩下的数字](https://leetcode.cn/problems/yuan-quan-zhong-zui-hou-sheng-xia-de-shu-zi-lcof/)（简单-约瑟夫环）×

## [9. 回文数](https://leetcode.cn/problems/palindrome-number/)（简单）×



## [151. 反转字符串中的单词](https://leetcode.cn/problems/reverse-words-in-a-string/)

```c++
/*
1 这里查了一下 当超出字符串s的长度时，就会返回false
2 这里要注意，如果上来就遇到空字符，那么vec就会把空字符插进数组，所以要判断一下
3 直接倒序遍历
*/
class Solution {
public:
    string reverseWords(string& s) {
        stringstream ss(s);
        vector<string> vec;
        string tmp;
        while(getline(ss, tmp, ' ')) // 1
            if(tmp.size()) vec.push_back(std::move(tmp)); // 2
        string res;
        for(int i = vec.size() - 1; i >=0; --i){ // 3
            res.append(vec[i]);
            res += ' ';
        }
        res.pop_back();
        return res;
    }
};

```











# 排序算法

[](https://blog.csdn.net/m0_60935443/article/details/123651582?ops_request_misc=%257B%2522request%255Fid%2522%253A%2522166815239216800192213559%2522%252C%2522scm%2522%253A%252220140713.130102334..%2522%257D&request_id=166815239216800192213559&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~all~baidu_landing_v2~default-1-123651582-null-null.142^v63^pc_rank_34_queryrelevant25,201^v3^control_1,213^v2^t3_esquery_v3&utm_term=c%2B%2B%E5%8D%81%E5%A4%A7%E6%8E%92%E5%BA%8F%E7%AE%97%E6%B3%95&spm=1018.2226.3001.4187)

[](https://blog.csdn.net/weixin_45511189/article/details/115828362?ops_request_misc=&request_id=&biz_id=102&utm_term=c++%E5%8D%81%E5%A4%A7%E6%8E%92%E5%BA%8F%E7%AE%97%E6%B3%95&utm_medium=distribute.pc_search_result.none-task-blog-2~all~sobaiduweb~default-5-115828362.142^v63^pc_rank_34_queryrelevant25,201^v3^control_1,213^v2^t3_esquery_v3&spm=1018.2226.3001.4187)

## 冒泡排序

冒泡排序（Bubble Sort），顾名思义，就是指越小的元素会经由交换慢慢“浮”到数列的顶端。

1.1 算法描述

- 从左到右，依次比较相邻的元素大小，更大的元素交换到右边；
- 从第一组相邻元素比较到最后一组相邻元素，这一步结束最后一个元素必然是参与比较的元素中最大的元素；
- 重复从左到后比较，而前一轮中得到的最后一个元素不参与比较，得出新一轮的最大元素；
- 按照上述规则，每一轮结束会减少一个元素参与比较，直到没有任何一组元素需要比较。

![](https://img-blog.csdnimg.cn/img_convert/7f2592d55f58691a342aaa13b8fbef53.gif)

```c++
void bubbleSort(int arr[], int n){
    bool bExchange = false; // 某一趟是否发生过交换
    for(int i = 0, i < n - 1; ++i){ // 最多需要n - 1 趟排序
        bExchange = false;
        for(int j = 0; j < n - 1 - i; ++j){
            if(arr[j] > arr[j + 1]){ //如果相邻元素相等，不交换，是稳定排序算法
                swap(arr[j], arr[j + 1]); 
                bExchange = true; // 交换过
            }
        }
        if(!bExchage) return; // 如果某一趟未发生交换，说明已经有序
    }
        
    	
}
```



## 快速排序

快速排序（Quick Sort），是冒泡排序的改进版，之所以“快速”，是因为使用了**分治法**。它也属于**交换排序**，通过元素之间的位置交换来达到排序的目的。

基本思想：通过一趟排序将待排记录分隔成独立的两部分，其中一部分记录的关键字均比另一部分的关键字小，则可分别对这两部分记录继续进行排序，以达到整个序列有序。

**算法描述**

- 从数列中挑出一个元素，称为 “基准”（pivot）；
- 重新排序数列，所有元素比基准值小的摆放在基准前面，所有元素比基准值大的摆在基准的后面（相同的数可以到任一边）。在这个分区退出之后，该基准就处于数列的中间位置。这个称为分区（partition）操作；
- 递归地（recursive）把小于基准值元素的子数列和大于基准值元素的子数列排序。

6.2 动图演示

![](https://pic.leetcode-cn.com/1612615167-lWmBlA-Picture7.png)

```c++
/*
1 初始化哨兵位置，防止出现最差时间复杂度，随机选择哨兵位置
2 从右向左 查找首个小于基准数的元素
3 从左向右 查找首个大于基准数的元素  
！！！这两个顺序不能交换！！！！为啥？假设用例是[1，2，3], 本来数组是有序的，但如果先从左向右搜索，因为条件是nums[i] <= nums[l]，那么当i==0时，就是1跟1比，i肯定会++的，然后最终i与j停到1，这样是不对的。但是如果先从右向左搜索，最终i与j都停到0，是对的。

	为什么结束条件必须是i < j？因为当i==j，说明左面都是小于基准数的，右面都是大于基准数的，所以用基准数跟这个位置交换正合适，另外如果条件是 i <= j,那么如上图，i就到4的位置i了，显然是错的。
4 子数组长度为1时终止划分

*/
int partition(vector<int> nums, int l, int r){
    int randomIndex = l + rand() % (r - l + 1); // 1
    swap(nums[l], nums[randomIndex]);
    int i = l, j = r; 
    while(i < j){
        while(i < j && nums[j] >= nums[l]) --j; // 2
        while(i < j && nums[i] <= nums[l]) ++i; // 3
        swap(nums[i], nums[j]);
    }
    swap(nums[j], nums[l]);
    return i;
}

void quickSort(vector<int> nums, int l, int r){
    if(l >= r) return; // 4
    int mid = partition(nums, l, r);
    quickSort(nums, l, mid - 1);
    quickSort(nums, mid + 1, r);
}

// 调用
vector<int> nums = { 4, 1, 3, 2, 5, 1 };
quickSort(nums, 0, nums.size() - 1);
```

[时间复杂度计算](https://blog.csdn.net/qq_39124199/article/details/107924471)



## 归并排序

![](https://img-blog.csdnimg.cn/fd1e5ee39edc41ff812daad1fbe18e2d.png)

有个问题就是我记得合并两个有序数组是不需要额外空间的，但是那个是因为nums1后面预留了nums2的空间，这个却没有，假设以下这种情况：

30和20交换之后，nums1就不符合链表有序的情况了，所以必须需要辅助空间。

![](https://img-blog.csdnimg.cn/1ef6333cf1be4e0fbac90d24855ef0ee.png)

```c++
class Solution {
public:
    vector<int> sortArray(vector<int>& nums) {
        mergeSort(nums, 0, nums.size() - 1);
        return nums;
    }
    void mergeSort(vector<int>& nums, int l, int r){
        if(l >= r) return; // 递归出口
        // 切割
        int mid = l + (r - l) / 2;
        mergeSort(nums, l, mid);
        mergeSort(nums, mid + 1, r);
        // 合并
        merge(nums, l, mid, r);
    }
    void merge(vector<int>& nums, int l, int mid, int r){
        vector<int> tmp(r - l + 1);
        // i为左数组的下标， j为右数组的下标，k为tmp的下标
        int i = l, j = mid + 1, k = 0;
        while(i <= mid && j <= r){
            if(nums[i] <= nums[j]) tmp[k++] = nums[i++];
            else tmp[k++] = nums[j++];
        }
        while(i <= mid) tmp[k++] = nums[i++];
        while(j <= r) tmp[k++] = nums[j++];
        --k;
        while(k >= 0) nums[r--] = tmp[k--];

    }
};
```



## 堆排序

https://www.cnblogs.com/chengxiao/p/6129630.html

```c++
/*
1 从最后一个非叶子节点开始调整，一直调整到根节点
2 一直调整到最后一个非叶子节点
*/
class Solution {
    void buildMaxHeap(vector<int>& nums) {
        int n = nums.size();
        for (int i = n / 2 - 1; i >= 0; --i) { // 1
            maxHeapify(nums, i, n);
        }
    }
    void maxHeapify(vector<int>& nums, int i, int n) {
        while ((i + 1) * 2 <= n) { // 2
            int lSon = 2 * i + 1; // 左节点下标
            int rSon = 2 * i + 2; // 右节点下标
            int large = i; // 记录当前三个节点的最大值的下标
            // 确保两个子节点的下标不越界，并更新最大值的下标
            if (lSon < n && nums[lSon] > nums[i]) large = lSon;
            if (rSon < n && nums[rSon] > nums[large]) large = rSon;
            // 如果当前父节点就是最大值，那么就不用再往下调整了
            if (large == i) break;
            else{ // 否则就要继续调整
                swap(nums[i], nums[large]);
                i = large;
            }
        }
    }
public:
    // heapSort 堆排序
    vector<int> sortArray(vector<int>& nums) {
        int n = nums.size();
        // 将数组整理成大根堆
        buildMaxHeap(nums);
        // 依次交换堆顶和队尾，然后只调整0~n-2内的元素
        for (int i = n - 1; i >= 1; --i) {
            swap(nums[0], nums[i]);
            maxHeapify(nums, 0, --n);
        }
        return nums;
    }
};
```

堆排序比快速排序慢的原因：可能是因为堆排序是跳着访问内存的，这对CPU缓存机制不太友好。

https://blog.csdn.net/qq_39530821/article/details/125351519

[**HJ7** **取近似值**](https://www.nowcoder.com/practice/3ab09737afb645cc82c35d56a5ce802a?tpId=37&tqId=21230&rp=1&ru=/exam/oj/ta&qru=/exam/oj/ta&sourceUrl=%2Fexam%2Foj%2Fta%3FtpId%3D37&difficulty=undefined&judgeStatus=undefined&tags=&title=)

太笨了，这都能不会

```c++
#include <iostream>
using namespace std;

int main() {
    double n;
    while(cin >> n){
        cout << static_cast<int>(n + 0.5) << endl;
    }
}
// 64 位输出请用 printf("%lld")
```



# 数学

## [质数因子](https://www.nowcoder.com/practice/196534628ca6490ebce2e336b47b3607?tpId=37&tqId=21229&rp=1&ru=/exam/oj/ta&qru=/exam/oj/ta&sourceUrl=%2Fexam%2Foj%2Fta%3FtpId%3D37&difficulty=undefined&judgeStatus=undefined&tags=&title=)×（分解质因数）！！！

![](https://uploadfiles.nowcoder.com/images/20211210/7618167_1639076808421/1ED65F7C0B78526A4920437E1AD7C954)

**素数就是质数。**

**唯一分解定理**：

- 每个合数都可以写成几个质数相乘的形式，其中每个质数都是这个合数的质因数，这个过程叫做分解质因数。
- 分解质因数只针对合数（7是质数它的因数只有1和7，1不是质数，因此质数不能分解为几个数的乘积）。 并且，每个合数能够且仅仅能够被分解为唯一一组质因数的乘积。 
- 对一个正整数n来说，如果它存在[2,n]范围内的质因子，要么这些质因子全部 <= sqrt(n)，要么只存在 一个 > sqrt(n)的质因子，而其余质因子全部 小于等于 sqrt(n)。也就是说 **一个正整数n，它最小的质因子一定不会大于sqrt(n)。**

```c++
/*
思路就是，从最小的质数开始判断，是不是n的因子，如果是，那么输出这个质数，然后用n去除以这个质数；
1 判断是否有重复的质因数，抵消掉for循环中的++i，下次还用这个i 去除 n
2 如果最后剩的数是个质数，它就不能再被质因分解了，所以只要n大于1，就要输出

不用担心遍历i的时候 i 是 合数， 因为如果4是180中的一个质因数，那么4一定会先被质因分解成2 * 2，所以从小的质因数开始查找就行
*/
#include <bits/stdc++.h>
using namespace std;

int main() {
    long n; cin >> n;
    for(int i = 2; i <= sqrt(n); ++i){
        if(n % i == 0){
            cout << i << " ";
            n = n / i;
            --i; // 1 
        }
    }
    if(n > 1) cout << n; // 2
    return 0;
}

```







# SQL

**176 子查询**

没想到怎么获取第二高的薪水。做法是从小于最高薪水的所有薪水中获取最高的薪水，也就是第二高。

思路一：

```sql
select max(Salary) as SecondHighestSalary 
from Employee 
where Salary != (select max(Salary) from Employee);
```

思路二：

limit n子句表示查询结果返回前n条数据

offset n 表示跳过n条语句

limit y offset x 分句表示查询结果跳过 x 条数据，读取前 y 条数据

```sql
select ifNull(
(select distinct salary
from Employee 
order by Salary Desc
limit 1,1),
null
) as SecondHighestSalary;
```

**max作为函数，肯定有返回值，当没有符合条件的值，那么就返回NULL，但是limit是筛选符合条件的行，当没有符合条件的行，就什么都不返回，因此想要达到和max一样的效果，应该和IFNULL配合使用。**

## [177. 第N高的薪水](https://leetcode.cn/problems/nth-highest-salary/)×

limit..offset不能用表达式

```sql
CREATE FUNCTION getNthHighestSalary(N INT) RETURNS INT
BEGIN
set N = N - 1;
  RETURN (
      # Write your MySQL query statement below.
      select ifnull(
      (select distinct salary
      from Employee 
      order by salary desc
      limit 1 offset N), null
      ) as getNthHighestSalary
  );
END
```



## [178. 分数排名](https://leetcode.cn/problems/rank-scores/)×！

[窗口函数](https://mp.weixin.qq.com/s?__biz=MzAxMTMwNTMxMQ==&mid=2649247566&idx=1&sn=f9c7018c299498673b38221db2ecd5cd&chksm=835fc77eb4284e68b7528fd7f75eedb8868a6740704af8559f8a5cbdd2867a49ffa21bf4e531&token=426730634&lang=zh_CN#rd)

```sql
select score, 
       dense_rank() over(order by Score desc) as 'rank'
from Scores;
```





## [180. 连续出现的数字](https://leetcode.cn/problems/consecutive-numbers/)？？？

https://leetcode.cn/problems/consecutive-numbers/solution/chuang-kou-han-shu-lagleadrow_number-by-9bu18/



## [181. 超过经理收入的员工](https://leetcode.cn/problems/employees-earning-more-than-their-managers/)？

方法不对，好像要用连接，但是忘了，看笔记再来

```sql
select name as Employee
from Employee e
where salary > (select salary from Employee where id = e.managerId);
```



# [力扣高频 SQL 50 题（基础版）](https://leetcode.cn/studyplan/sql-free-50/)

## 查询

#### [1757. 可回收且低脂的产品](https://leetcode.cn/problems/recyclable-and-low-fat-products/)√

## 连接

#### [1378. 使用唯一标识码替换员工ID](https://leetcode.cn/problems/replace-employee-id-with-the-unique-identifier/)√

## 聚合函数

#### [620. 有趣的电影](https://leetcode.cn/problems/not-boring-movies/)√



## 排序和分组

#### [2356. 每位教师所教授的科目种类的数量](https://leetcode.cn/problems/number-of-unique-subjects-taught-by-each-teacher/)√



## 高级查询和连接

#### [1731. 每位经理的下属员工数量](https://leetcode.cn/problems/the-number-of-employees-which-report-to-each-employee/)

```sql
-- ROUND(AVG(e.age), 0) 四舍五入，0是保留到整数 1就是保留1位小数
SELECT r.employee_id , r.name, count(*) as reports_count, ROUND(AVG(e.age), 0) average_age
FROM Employees e
INNER JOIN Employees r on e.reports_to = r.employee_id
GROUP BY r.employee_id
ORDER BY r.employee_id
```

