title: "LeetCode-Two Sum"
date: 2015-04-07 00:20:36
tags: 
- LeetCode
- 算法
- 哈希
categories: LeetCode
---
> Given an array of integers, find two numbers such that they add up to
> a specific target number.
> 
> The function twoSum should return indices of the two numbers such that
> they add up to the target, where index1 must be less than index2.
> Please note that your returned answers (both index1 and index2) are
> not zero-based.
> 
> You may assume that each input would have exactly one solution.
> 
> **Input**: numbers={2, 7, 11, 15}, target=9 
> **Output**: index1=1, index2=2

-----
### Accepted Code：

    public class Solution {      
	    public int[] twoSum(int[] numbers, int target) {      
		    int[] a = new int[2];      
		    for (int i = 0; i < numbers.length; i++) {      
			    for (int j = i + 1; j < numbers.length; j++) {      
				    if (numbers[i] + numbers[j] == target) {      
					    a[0] = i + 1;      
					    a[1] = j + 1;
					    return a;      
				    }      
			    }      
		    }      
		    return a;      
	    }
    }
     

 
Problems:
1. Time Limit Exceeded(超时)。

Then:

    if (numbers[i] + numbers[j] == target) {
	    a[0] = i + 1;
	    a[1] = j + 1;
	    return a;//this line is the Key Code.
    }

 
**省略计算不必要的结果，得到正确结果立刻返回。**

---
Other Solution:

> 代码是其他语言。

题目大意：给一个数组，找出其中是否有两个数之和等于给定的值。类似的还有3 sum ，4 sum ..等 K sum 问题。其实原理是差不多的，这样想：先取出一个数，那么我只要在剩下的数字里面找到两个数字使得他们的和等于(target – 那个取出的数)。

**解法1：先排序，然后从开头和结尾同时向中间查找，原理也比较简单。复杂度O(nlogn)**

    typedef struct Node{    
	    int id,val;
    }Node;
    
    bool compare(const Node &amp; a,const Node &amp; b){    
	    return a.val < b.val;
    }
    
    class Solution {
	    public:
	    vector<int> twoSum(vector<int> &amp;numbers, int target){
	    
		    Node nodes[numbers.size()];
		    
		    for(unsigned int i=0; i<numbers.size(); i++){
		    
		    nodes[i].id = i+1;
		    
		    nodes[i].val = numbers[i];
	    
	    }
	    sort(nodes, nodes+numbers.size(), compare);
	    int start=0,end=numbers.size()-1;
	    vector<int> ans;
	    while(start < end){
	    
		    if(nodes[start].val + nodes[end].val == target){
		    
		    if(nodes[start].id > nodes[end].id)
		    
		    swap(nodes[start].id , nodes[end].id);
		    
		    ans.push_back(nodes[start].id);
		    
		    ans.push_back(nodes[end].id);
		    
		    return ans;
	    
		    }else if( nodes[start].val + nodes[end].val <target ){	    
			    start++;	    
		    } else	    
			    end--;
		    }
	    }
    };

**解法2：使用HashMap。把每个数都存入map中，任何再逐个遍历，查找是否有 target – nubmers[i]。 时间复杂度 O(n)**

    vector<int> twoSum(vector<int> &numbers, int target) {
            // Start typing your C/C++ solution below
            // DO NOT write int main() function
            vector<int> res;
        	int length = numbers.size();
    		map<int,int> mp;
    		for(int i = 0; i < length; ++i)
    			mp[numbers[i]] = i;
    		map<int,int>::iterator it = mp.end();
    		for(int i = 0; i < length; ++i) {
    			it = mp.find(target - numbers[i]);
    			if(it != mp.end()) {
    				res.push_back(min(i+1,it->second +1));
    				res.push_back(max(i+1,it->second +1));
    				break;
    			}
    		}
    		return res;
    }
    
其实可以优化一下，因为题目只要求要到一个解，找到后即可返回。

    vector<int> twoSum(vector<int> &numbers, int target) {
            vector<int> res;
        	int length = numbers.size();
    		map<int,int> mp;
    		int find;
    		for(int i = 0; i < length; ++i){
    			find=mp[target - numbers[i]];
    			if( find ){
    				res.push_back(find);
    				res.push_back(i+1);
    				break;
    			}
    			mp[numbers[i]] = i+1;
    		}
    		return res;
    }
    
 
 
---

Java 版本解法：

Two Sum

Given an array of integers, find two numbers such that they add up to a specific target number.

The function twoSum should return indices of the two numbers such that they add up to the target, where index1 must be less than index2. Please note that your returned answers (both index1 and index2) are not zero-based.

You may assume that each input would have exactly one solution.

思路：

先把原数组复制一遍，然后进行排序。在排序后的数组中找这两个数。最后再在原数组中找这两个数字的index即可。

时间复杂度O(nlogn)+O(n)+O(n) = O(nlogn)

注意的是结果有可能是两个数是相同的，比如 0 3 4 0, 0要返回1和4，不要返回成1和1或者4和4.

代码：

	public int[] twoSum(int[] numbers, int target) {
		// Start typing your Java solution below
		// DO NOT write main() function
		// Copy the original array and sort it
		int N = numbers.length;
		int[] sorted = new int[N];
		System.arraycopy(numbers, 0, sorted, 0, N);
		Arrays.sort(sorted);
		// find the two numbers using the sorted arrays
		int first = 0;
		int second = sorted.length - 1;
		while (first < second) {

			if (sorted[first] + sorted[second] < target) {

				first++;

				continue;

			}

			else if (sorted[first] + sorted[second] > target) {

				second--;

				continue;

			}

			else
				break;
		}
		int number1 = sorted[first];
		int number2 = sorted[second];
		// Find the two indexes in the original array
		int index1 = -1, index2 = -1;
		for (int i = 0; i < N; i++) {

			if ((numbers[i] == number1) || (numbers[i] == number2)) {

				if (index1 == -1)

					index1 = i + 1;

				else

					index2 = i + 1;

			}

		}
		int[] result = new int[] { index1, index2 };
		Arrays.sort(result);
		return result;
	}
 

还有个无耻地利用hashmap的O(n)的算法，原理和暴力搜索没有本质区别，只不过hashmap的搜索速度是O(1)。

	public int[] twoSum(int[] numbers, int target) {
		// Start typing your Java solution below
		// DO NOT write main() function

		HashMap<Integer, Integer> map = new HashMap<Integer, Integer>();

		int n = numbers.length;

		int[] result = new int[2];

		for (int i = 0; i < numbers.length; i++) {
			if (map.containsKey(target - numbers[i])) {
				result[0] = map.get(target - numbers[i]) + 1;
				result[1] = i + 1;
				break;
			} else {
				map.put(numbers[i], i);
			}
		}

		return result;
	}
 
 
后记：你的想法很独特嘛。
