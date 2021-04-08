# Algorithm


**已完成**

- 遇到大整数乘法取模，优先开long long，不然乘法容易爆int。int 4个字节，long 8个字节， long long 16个字节，就算两个int的最大值相乘也不过8个字节，所以开long或者long long足够解决溢出的问题。==再不行记得开unsigned long long==
- 多行输出，一定要注意数组，变量的重新初始化！！！
- 字符串处理时**最差时间复杂度**注意：**子串**(最差复杂度O(N^2)) 与**子序列**（最差复杂度O(2^N)）
- $\sum_{i=1}^{5}$
- c++特定字符串分隔字符stringstream （while(getline(ss,tmp,'/'))）
-  位运算: mask&(-mask) 可以取到mask右边第一个非0的二进制的位置; 负数的补码：反码（符号位不变）+1
- KMP 算法求最短循环节时，注意把最长公共前后缀字符数组D先求出来，然后观察D与要求的内容（比如最小循环节的关系, len-D(len))
- 回文字符串的一个特征：假设回文串为P，那么字符串P+'#'+reverse(P) 与KMP 的关系为: D[len-1]=P.length()
- 滑动窗口模板
题目：[leetcode 713. 乘积小于K的子数组](https://leetcode-cn.com/problems/subarray-product-less-than-k/)
```cpp
class Solution {
  public:
    //滑动窗口模板题
    int numSubarrayProductLessThanK(vector<int>& nums, int k) {
        int  n=nums.size();
        if(!n) return 0;

​        int l=0,r=0;
​        long long mul=1;
​        int ans=0;
​        while(r<n){
​            mul*=nums[r];
​            while(mul>=k && l<n){//[1,2,3] 0
​                mul/=nums[l]; 
​                l++;
​            }

​            if(mul<k)  //这里尤其注意k为0 的情况
​                ans+=r-l+1; //固定 右端点之后，左端点往左走的结果
​            r++;
​        }

​        return ans;
​    }
};
```



- 优先队列 priority_queue 头文件 ```#inlcude<queue>``` 自定义结构体排序方式，注意与sort函数的自定义排序方式的区别

```cpp
struct Node{
	int val;//值 
	int loc;//位置
	
	Node (int v,int l){
		val=v;
		loc=l;
	}
//	bool operator < (const Node b){//按值的从小到大的顺序的结果输出 
//		if(val==b.val) return loc<b.loc;
//		
//		return val<b.val;
//	} 
};

//bool cmp
struct cmp{
	bool operator () (Node a,Node b){ //重新定义()的排序方式
		return a.val > b.val;
	}
}; 
```

- c++二分查找的两个板子：
1. l<r
2. l<=r
leetcode 81. 搜索旋转排序数组 II https://leetcode-cn.com/problems/search-in-rotated-sorted-array-ii/


```cpp
class Solution {
public:
    //二分查找 单边有序
    //注意到始终有一边是有序的，有序的这部分可以采用二分查找
    //剩下的未有序的部分可以重复上述过程
    //写着写着 感觉又写复杂了

    //注意题目描述 输入的数据有重复的地方 二分查找需要注意
    bool search(vector<int>& nums, int target) {
        if(!nums.size()) return false;
        int n=nums.size();
        int l=0,r=n-1;

        while(l<=r){
            //消除重复的数据，与三数求和问题类似
            // while(l<=r && nums[l]==nums[l+1]) l++;
            // while(l<=r && nums[r]==nums[r-1]) r--;
            while(l<r && nums[l]==nums[l+1]) l++;
            while(l<r && nums[r]==nums[r-1]) r--;

            //开始二分查找，注意的是，只有一边可能有序
            int mid=l+(r-l)/2;
            if(nums[mid]==target) return true;

            if(nums[mid]>nums[l]){//左边有序，这里的符号也需要注意, case [3,1] 1 
                //再二分查找？
                //判断target 是否在这个区间内部
                if(target>=nums[l] && target<nums[mid]) r=mid-1;
                else l=mid+1;
            }
            else{
                if(target>nums[mid] && target<=nums[r]) l=mid+1;
                else r=mid-1;
            }

            // cout<<l<<" "<<r<<"\n";
        }

        return false;
    }
    // bool check(int l, int r, vector<int> nums,int target){
    //     while(l<r){
    //         int midd=(l+r)>>1;
    //         if(nums[midd]==target) return true;
    //         else if(nums[midd]<target) l=midd+1;
    //         else r=midd;
    //     }
    //     if(nums[l]==target) return true;
    //     return false;
    // }

    // bool sovle(int l,int r, vector<int> nums,int target){
    //     if(l>=r){
    //         if(nums[l]==target) return true;
    //         return false;
    //     } 
    //     int mid=(l+r)>>1;
    //     if(nums[mid]==target) return true;

    //     if(nums[mid]>nums[l]) {
    //         if(target<nums[mid]) return check(l,mid,nums,target);
    //         else return sovle(mid+1,r,nums,target);
    //     }
    //     else{
    //         if(target>nums[mid]) return check(mid+1,r,nums,target);
    //         else return sovle(l,mid,nums,target);
    //     }
    //     return false;
    // }

    // bool search(vector<int>& nums, int target) {
    //     if(!nums.size()) return false;
    //     int n=nums.size();
    //     return sovle(0,n,nums,target);
    // }
};
```



- c++二分查找实现lower_bound和upper_bound的方法，针对左闭右开区间
[剑指 Offer 53 - I. 在排序数组中查找数字 I](https://leetcode-cn.com/problems/zai-pai-xu-shu-zu-zhong-cha-zhao-shu-zi-lcof/)

```cpp
class Solution {
    public:
    //upper_bound
    //lower_bound
    int search(vector<int>& nums, int target) {
        if(! nums.size()||target<nums[0] || target >nums[nums.size()-1])  return 0;
        if(nums.size()==1){
            if(nums[0]==target) return 1;
            return 0;
        }
        //lower_bound
        int l=0,r=nums.size();//[l,r)
        while(l<r){//左闭右开
            int mid=(l+r)>>1;
            if(nums[mid]<target) l=mid+1;
            else r=mid;
        }

        //upper_bound
        int indl=l;
        l=0,r=nums.size();//左闭右开
        while(l<r){//r比l至少大1
            int mid=(l+r)>>1;
            if(nums[mid]<=target) l=mid+1;
            else r=mid;
        }

        
        int indr=l;//
        // cout<<indr<<" "<<indl<<"\n";
        return indr-indl;
    }

}; 
```
- string 格式化多次输入，空格跳过，输入输出流==istringstream==, ==ostringstream==
库函数
```
#include<sstream>
```
- string 寻找出现的所有的相同字符
```cpp
int p=-1;
while((p=str.find('.',p+1))!=string::npos)//string::find(string,pos)从pos开始寻找
```
- string 类以行读入，并以特定分隔符进行分隔

```cpp
#include<iostream>
#include<cstdio>
#include<cstring>
#include<cstdlib>
#include<algorithm>
#include<vector>
#include<string>
#include <sstream>
using namespace std;
int main(int argc, char** argv)
{
    freopen("in.txt", "r", stdin);
    string str,tmp;
    getline(cin,str);//读入一行字符串 
    
    cout<<str<<"\n";
    
    int cnt=count(str.begin(),str.end(),'.');//字符串统计出现次数 
    cout<<cnt<<"\n";
    
    vector<string> ans;
    istringstream ss(str);//输入流 
//	std::stringstream ss;
    
    while(getline(ss,tmp,'.')){//字符串分隔 
    	ans.push_back(tmp);
	}
	
	for(string tmp: ans){
		cout<<tmp<<"\n";
	}
    return 0;
}

```

- KMP算法简单易懂理解

[https://kb.cnblogs.com/page/176818/](https://kb.cnblogs.com/page/176818/)
[KMP算法主要思路以及理解过程bilibili](https://www.bilibili.com/video/BV1iJ411a7Kb?p=1)


```cpp
#include<iostream>
#include<cstdio>
#include<cstring>
#include<cstdlib>
#include<algorithm>
#include<vector>
#include<string>
using namespace std;
const int N=1e6+10;

int D[N]= {0}; //表示[0,i]这个前面i+1个子字符串  包含的共同前后缀的长度 初始化所有值都是0

string T,P;//len(T) >= len(P)

//void kmp(string T, int Nt, string P, int Np) { //长串，模式串
//	//假设我们这里已经计算好D数组
//
//	int i=0,j=0;
//	while(i<Nt) {
//		if(j==Np) {
//			printf("%d\n",i-Np+1);
//			j=0;
////			return i-Np;//找到了匹配开始的位置
//		}
//		if(T[i]==P[j]) {
//			i++;
//			j++;
//		} else {
//			if(j>0) j=D[j-1];
//			else i++;
//		}
//
//	}
//
////	return -1;
//}
void kmp(string T, int Nt, string P, int Np) { //长串，模式串
	//假设我们这里已经计算好D数组

	int i=0,j=0;
	while(i<Nt) {
		if(T[i]==P[j]) {
			i++;
			j++;
			
			if(j==Np){
				printf("%d\n",i-Np+1);
				j=D[j-1];//从上一次没有匹配好的地方开始匹配 
			}
		} else {
			if(j>0) j=D[j-1];
			else i++;
			
		}
	}
	
//	return -1;
}
void calc_D(string P,int Np) { //模式串
	//利用与KMP算法类似的思想计算 D数组
	//这里假设有另一个串已经向右顺移一位
	int i=1;//后缀
	int j=0;//前缀

	while(i<Np) {
		if(P[i]==P[j]) {
			D[i++]=++j;
		} else {
			if(j>0)	j=D[j-1];
			else i++;
		}
	}
}

void print_D(int Np) {
	for(int i=0; i<Np; i++) {
		printf("%d ",D[i]);
	}
	printf("\n");
}
int main(int argc, char** argv) {
	freopen("in.txt", "r", stdin);
	cin >> T>>P;

	calc_D(P,P.size());//计算D数组

	kmp(T,T.size(),P,P.size());

	print_D(P.size());
//	printf("%d\n",ans);//第一次在T中匹配到的位置

	return 0;
}


```
- 快速排序模板
快速排序算法理解：[白话经典算法系列之六 快速排序 快速搞定](https://blog.csdn.net/morewindows/article/details/6684558)
模板测试：[洛谷P1177 【模板】快速排序]()

```cpp

#include<iostream>
#include<cstdio>
#include<cstring>
#include<cstdlib>
#include<algorithm>
#include<vector>
using namespace std;
//快排实现
const int N=1e5+9;
int a[N];
void solve(int,int);
 
//方法1，选择基准数为首位 
int main(int argc, char** argv)
{
    freopen("in.txt", "r", stdin);
    int n;
	scanf("%d",&n); 
	for(int i=0;i<n;i++){
		scanf("%d",&a[i]);
	}
	
	solve(0,n-1);
	
	for(int i=0;i<n;i++){
		if(!i) printf("%d",a[i]);
		else printf(" %d",a[i]);
	}
	printf("\n");
    return 0;
}

//1. 为什么从右边开始
//2. 有哪些优化方法
//3. 快排为什么不稳定

void solve(int l,int r){
	if(l>=r) return ;
	
	int tmp=a[l]; //选择基准数，挖坑 
	
	int i=l,j=r;
	while(i<j){
		while(i<j && a[j]>=tmp) j--;//为什么要先从右边开始 
		if(i<j){
			a[i]=a[j];
			i++;
		}

		while(i<j && a[i]<=tmp) i++;
		if(i<j){
			a[j]=a[i];
			j--;
		}
		
	}
	a[i]=tmp;
	
	solve(l,i-1);
	solve(i+1,r);
}
```
- 快排单链表（超时）
[参考思路https://leetcode-cn.com/problems/sort-list/solution/gui-bing-pai-xu-kuai-su-pai-xu-by-datacruiser/](https://leetcode-cn.com/problems/sort-list/solution/gui-bing-pai-xu-kuai-su-pai-xu-by-datacruiser/)

```cpp
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
    //快排单链表
    // void swap_(int *a,int *b){
    //     int *t=a;
    //     a=b;
    //     b=t;
    // }
    // void swap_(int *a ,int *b){
    //     int *t=a;
    //     *a=*b;
    //     *b=*t;
    // }
    void swap_(int *a,int *b){
        int t=*a;
        *a=*b;
        *b=t;
    }
    ListNode *split(ListNode *left,ListNode *right){//找到中间划分的点
        //默认left=begin(),right=NULL
        if(left==right || !left) return left;
        int val=left->val;//基准值

        ListNode *p1=left,*p2=left->next;//找到mid对应的点
        while(p2){
            if(p2->val < val){
                p1=p1->next;//将p1往前走，逐步靠近mid
                swap_(&p1->val,&p2->val);
            }
            p2=p2->next;
        }
        swap_(&p1->val,&left->val);

        return p1;//返回mid对应的点
    }

    void solve(ListNode *left,ListNode *right){
        if(left == right || !left) return ;

        ListNode *mid=split(left,right);
        solve(left,mid);
        solve(mid->next,right);
    }

    ListNode* sortList(ListNode* head) {
        if(!head || !head->next) return head;
        // ListNode *mid=split(head,NULL);
        solve(head,NULL);

        return head;
    }
};
```
- 快速排序应用：O(N)求解Top k问题
题目：[leetcode 215. Kth Largest Element in an Array](https://leetcode.com/problems/kth-largest-element-in-an-array/)
注意基准数的选取影响快排的效率，采用rand()函数随机指定一个基准数，然后将其与首部节点（默认）交换

```cpp
class Solution {
public:
    //小顶堆
    // int findKthLargest(vector<int>& nums, int k) {
    //     priority_queue<int,vector<int>,greater<int> > Q;
    //     for(auto num: nums){
    //         Q.push(num);
    //         if(Q.size()>k) Q.pop();
    //     }
    //     return Q.top();
    // }
    
    //快排算法求解top k
    int findKthLargest(vector<int>& nums, int k){
        if(!nums.size()) return 0;
        
        return solve(0,nums.size()-1,k,nums);
    }
    
    int solve(int l,int r, int k,vector<int> &nums){
        // cout<<l<<" "<<r<<"\n";
        if(l>r) return -1;
        //随机选择基准值
        int rand_=rand()%(r-l+1)+l;
        swap(nums[rand_],nums[l]);
            
        int tmp=nums[l];
        
        int i=l,j=r;
        while(i<j){
            while(i<j && nums[j]>=tmp) j--;
            
            if(i<j) {
                nums[i]=nums[j];
                i++;
            }
            
            while(i<j && nums[i]<=tmp) i++;
            if(i<j){
                nums[j]=nums[i];
                j--;
            }
        }
        
        nums[i]=tmp;//i的位置已经固定
        // cout<<i<<"\n";
        
        int right=nums.size()-1;
        
        if(right-i+1==k) return nums[i];//注意这个地方，最右边应该固定为right
        else if(right-i+1>k) return solve(i+1,r,k,nums);
        else return solve(l,i-1,k,nums);
    }
    
};
```
- 归并算法模板
模板测试：[洛谷P1177 【模板】快速排序](https://www.luogu.com.cn/problem/P1177)


```cpp
#include<iostream>
#include<cstdio>
#include<cstring>
#include<cstdlib>
#include<algorithm>
#include<vector>
using namespace std;
const int N=1e5+9;
int a[N];
void solve(int,int);
int tmp[N]; 
int main(int argc, char** argv)
{
    freopen("in.txt", "r", stdin);
    int n;
	scanf("%d",&n); 
	for(int i=0;i<n;i++){
		scanf("%d",&a[i]);
	}
	
	solve(0,n-1);
	
	for(int i=0;i<n;i++){
		if(!i) printf("%d",a[i]);
		else printf(" %d",a[i]);
	}
	printf("\n");
    return 0;
}

//先划分，再合并 
void solve(int l,int r){
	if(l>=r) return ;
	//先划分
	int mid=(l+r)>>1;
	
	solve(l,mid);
	solve(mid+1,r);
	//再合并
	 
	int i=l,j=mid+1;
	int cnt=i;
	while(i<=mid && j<=r) {
		if(a[i]<a[j]) tmp[cnt++]=a[i++];
		else tmp[cnt++]=a[j++];
	}
	while(i<=mid) tmp[cnt++]=a[i++];
	while(j<=r) tmp[cnt++]=a[j++];
	
	for(int k=l;k<=r;k++) a[k]=tmp[k];
}
```

- 归并排序应用：单链表排序(空间O(1)（不考虑堆栈占用的空间）,时间O(NlogN))


```cpp
class Solution {
public:
    //归并排序单链表
    //时间复杂度O(NlogN) 空间复杂度O(logN)
    ListNode* sortList(ListNode* head) {
        if(head==NULL || head->next==NULL) return head;
        
        ListNode *l=head,*r=head->next;
        while(r && r->next){
            l=l->next;
            r=r->next->next;
        }
        r=l->next;
        
        l->next=NULL;
        
        ListNode *left=sortList(head);
        ListNode *right=sortList(r);
        return merge(left,right);
    }
    
    ListNode * merge(ListNode *l, ListNode *r){
        ListNode tmp(0);//空节点
        ListNode *t=&tmp;
        
        while(l && r){
            if(l->val <r->val){
                t->next=l;
                l=l->next;
                t=t->next;
            }
            else{
                t->next=r;
                r=r->next;
                t=t->next;
            }
        }
        if(l){
            t->next=l;
            // tmp->next=l;
            // l=l->next;
            // tmp=tmp->next;
        }
        
        if(r){
            t->next=r;
            // tmp->next=r;
            // r=r->next;
            // tmp=tmp->next;
        }
        
        return tmp.next;
    }
};

```


- 堆排序
1. 建堆，时间复杂度o(N)，从非叶子节点往上排
```cpp
	//从非叶子节点从下往上调整
	for(int i=n/2-1; i>=0; i--) {
		max_heap(i,n);
	}
```
2. 每次把剩下序列里面的最大值放在堆顶，然后与堆的最后一个值交换；
3. 重新调整整个堆，使其满足大顶堆的性质，时间复杂度O(NlogN)


```cpp
#include<iostream>
#include<cstdio>
#include<cstring>
#include<cstdlib>
#include<algorithm>
#include<vector>
#include<string>
using namespace std;
const int N=1e5+10;
//堆排序
int num[N];
int n;
void max_heap(int i,int n) {
	int cur=num[i];
	//比较一下左右两个叶子，谁比较大
	for(int k=i*2+1;k<n;k=k*2+1){
		if(k+1<n && num[k+1]>num[k]) k++;
		
		if(num[k]>cur){//因为需要维护之前已经排好的顺序 
//		if(num[k]>num[i]){//为什么？？ 
			num[i]=num[k];
			i=k;
		} 
		
		else{
			break;//当前这一小部分满足大顶堆的要求 
		}
	}
	
	num[i]=cur;//换回来

}


int main(int argc, char** argv) {
//	freopen("in.txt", "r", stdin);
	scanf("%d",&n);
	for(int i=0; i<n; i++) scanf("%d",&num[i]);
	if(n<=1) {
		for(int i=0; i<n; i++) {
			printf("%d\n",num[i]);
		}
		return 0;
	}
	//从非叶子节点从下往上调整
	for(int i=n/2-1; i>=0; i--) {
		max_heap(i,n);
	}
	
	for(int i=n-1;i>=0;i--){
		swap(num[0],num[i]);//每一轮把最大的值放在堆的最后面，感觉有点像冒泡排序的思想 
		max_heap(0,i);
	} 
	
	for(int i=0;i<n;i++){
		if(!i) printf("%d",num[i]);
		else printf(" %d",num[i]);
	}
	printf("\n");
	
	
	return 0;
}


```

- 堆优化的Dijstra算法
堆优化版本:[https://www.luogu.com.cn/blog/action/dijkstra-suan-fa-dui-you-hua-post](https://www.luogu.com.cn/blog/action/dijkstra-suan-fa-dui-you-hua-post)
