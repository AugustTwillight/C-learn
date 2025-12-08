# C++语言自定义比较器
```
#include <algorithm>
class Compare1{
public:
    bool operator()(Person v1, Person v2){
        // 按照年龄降序排列
        return v1.age > v2.age; // 返回 true 时, v1 在前面 
    }
};
std：：sort（begin，end，compare1（））
```
# [accumulate函数](https://blog.csdn.net/m0_68250740/article/details/139451760?ops_request_misc=&request_id=&biz_id=102&utm_term=accumulate&utm_medium=distribute.pc_search_result.none-task-blog-2~all~sobaiduweb~default-0-139451760.142^v102^pc_search_result_base6&spm=1018.2226.3001.4187)
头文件

```
#include <numeric>
```
函数共有四个参数，其中前三个为必须，第四个为非必需。

若不指定第四个参数，则默认对范围内的元素进行累加操作。
```
accumulate(起始迭代器, 结束迭代器, 初始值, 自定义操作函数)
```

```
#include <iostream>
#include <vector>
#include <numeric>
 
using namespace std;
 
int fun(int acc, int num) {
	return acc + num * 3;// 计算数组中每个元素乘以3
}
 
int main() {
	vector<int> arr{ 1, 2, 3, 4, 5, 6, 7};
	int sum = accumulate(arr.begin(), arr.end(), 0, fun);
	cout << sum << endl;	//84
	return 0;
}
```
# [开方，平方，绝对值](https://blog.csdn.net/python_new_1/article/details/127170683?ops_request_misc=elastic_search_misc&request_id=87f876c44d7e6d52ffbbefc1c9a56974&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~all~top_click~default-1-127170683-null-null.142^v102^pc_search_result_base6&utm_term=C%2B%2B%E5%B9%B3%E6%96%B9%E5%87%BD%E6%95%B0&spm=1018.2226.3001.4187)
1.平方：pow（底数,指数）;
2.开方：sqrt（被开方数）;
    也可以这么做：pow(被开方数，0.5）;
3.绝对值：整数绝对值：abs（整数）

```
#include <cmath>
using namespace std;
int main()
{
	cout<<pow(4,2)<<" ";
	cout<<pow(4,0.5)<<" ";
	cout<<sqrt(4)<<" ";
	cout<<abs(-4)<<" ";
	cout<<fabs(-4.2)<<" ";
	return 0;
}
```
# [队列](https://blog.csdn.net/qq_45575167/article/details/128710171?ops_request_misc=elastic_search_misc&request_id=a262afb60265c9ce9cc84f5dc73eafae&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~all~top_positive~default-1-128710171-null-null.142^v102^pc_search_result_base6&utm_term=%E9%98%9F%E5%88%97stl&spm=1018.2226.3001.4187)

```
#include <iostream>
#include <queue>
using namespace std;
int main()
{
    std::queue<int> myqueue;
    myqueue.push(1);   //入队
    myqueue.push(2);
    myqueue.push(3);
    cout<<"队列末尾元素："<<myqueue.back(   <<endl;
    cout<<"队列元素出队顺序如下：";
    while(!myqueue.empty()) //判空
    {
        cout<<myqueue.front()<<"  ";    //访问队列头元素
        myqueue.pop();  //队列头元素出对
    }
    return 0;
}

```
## 优先级队列
priority_queue就是优先级队列在STL中的模版，在优先队列中，优先级高的元素先出队列，内部使用堆实现(大顶堆，同Top K问题)。
priority_queue<T,Sequence,Compare>

T TT : 存放容器的元素类型

S e q u e n c e SequenceSequence : 实现优先级队列的底层容器，默认是vector<T>

C o m p a r e CompareCompare : 用于实现优先级的比较函数，默认是functional中的less<T>

```
#include<bits/stdc++.h>
using namespace std;
int main(){
    priority_queue<int,vector<int>,less<int>> maxx; // 大顶堆 数据元素从大往小进行排序
    priority_queue<int,vector<int>,greater<int>> minn; //小顶堆 数据元素从小往大进行排序
}

```
(1)T top()：访问队列的队头元素，并不删除对头元素
(2)void pop()：删除对头元素。
(3)void push(T）：元素入队

```
#include <bits/stdc++.h>
using namespace std;
typedef long long ll;
struct Student
{
    string name;
    int score;
};
class cmp{
public:
    bool operator()(Student &a, Student &b){
        return a.score<b.score; //成绩高者优先
    }    
};
void xianshi(Student a){
    cout<<"成绩最高者为："<<a.name<<' '<<a.score<<endl;
}
int main()
{
    priority_queue<Student,vector<Student>,cmp> sq;  
    // 利用优先队列可以快速的获得自己想要的优先级最高的数据
    Student stu1,stu2,stu3,stu4;
    stu1.name = "小明"; stu1.score = 59;
    sq.push(stu1);
    xianshi(sq.top());
    stu2.name = "小花"; stu2.score = 88;
    sq.push(stu2);
    xianshi(sq.top());
    stu3.name = "小黑"; stu3.score = 99;
    sq.push(stu3);
    xianshi(sq.top());
    stu4.name = "Bob"; stu4.score = 55;
    sq.push(stu4);
    xianshi(sq.top());
    return 0;
}
```
# C++语言自定义比较器
```
#include <algorithm>
class Compare1{
public:
    bool operator()(Person v1, Person v2){
        // 按照年龄降序排列
        return v1.age > v2.age; // 返回 true 时, v1 在前面 
    }
};
std：：sort（begin，end，compare1（））

```
# [set](https://blog.csdn.net/weixin_53235104/article/details/130545112?ops_request_misc=elastic_search_misc&request_id=612570fd58d4f465f330a9758ad7e4eb&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~all~top_positive~default-1-130545112-null-null.142^v102^pc_search_result_base6&utm_term=set&spm=1018.2226.3001.4187)
（set）是一个内部自动有序且不含重复元素的容器，它可以在需要删除重复元素的情况下大放异彩，节省时间，减少思维量。
set 就是关键字的简单集合，当只是想知道一个值是否存在时，set 是最有用的。set 内部采用的是一种非常高效的平衡检索二叉树：红黑树（Red-Black Tree），也称为 RB 树，RB 树的统计性能要好于一般平衡二叉树。在 set 中每个元素的值都唯一，而且系统能根据元素的值自动进行排序，set 中元素的值不能直接被改变。
## set 关联容器分
1. 按关键字有序保存元素：set（关键字即值，即只保存关键字的容器）、multiset（关键字可重复出现的 set）；
2. 无序集合：unordered_set（用哈希函数组织的 set）、unordered_multiset（用哈希函数组织的 set，关键字可以重复出现）。

```
#include <set>
using namespace std;

    // 举例
    set<int> name;
    set<double> name;
    set<char> name;
    set<struct node> name;
    set<set<int>> name;

    //set数组
    set<int> arr[10];
```
set<类型名> 变量名;
## set 容器内的元素访问
set 只能通过迭代器（iterator）访问：

```
    set<int>::iterator it;
    set<char>::iterator it;
```
除了 vector 和 string 以外的 STL 容器都不支持 *(it + i) 的访问方式
将原本无序的元素插入 set 集合，set 内部的元素自动递增排序，且去除了重复元素。
## set 的常用函数
### find()：
find(value) 返回 set 中 value 所对应的迭代器，即 value 的指针（地址），如果没找到则返回 end()

```
#include <iostream>
#include <set>
using namespace std;
int main() 
{
    set<int> st;
    for (int i = 1; i <= 3; i++)
    {
        st.insert(i);
    }

    set<int>::iterator it = st.find(2); // 在set中查找2，返回其迭代器
    cout << *it << endl;

    // 以上可以直接写成
    cout << *(st.find(2)) << endl;
    return 0;
}

// 输出结果
2
2

```
### erase()：
可以删除单个元素或删除一个区间内的所有元素
1. st.erase(it)，其中 it 为所需要删除元素的迭代器。时间复杂度为 O(1)，可以结合 find() 函数来使用。
2. st.erase(value)，其中 value 为所需要删除元素的值。时间复杂度为 O(logN)，N 为 set 内的元素个数。

```   
#include <iostream>
#include <set>
using namespace std;

int main()
{
    set<int> st;
    st.insert(100);
    st.insert(200);
    st.insert(100);
    st.insert(300);
    st.insert(500);

    // 删除单个元素
    st.erase(st.find(100)); // 利用find()函数找到100,然后用erase删除它
    st.erase(200);          // 删除值为200的元素
    for (set<int>::iterator it = st.begin(); it != st.end(); it++)
    {
        cout << *it << " ";
    }
    return 0;
}

// 输出结果
300 500

```
删除一个区间内的所有元素：
st.erase(iteratorBegin, iteratorEnd)，其中 iteratorBegin 为所需要删除区间的起始迭代器， iteratorEnd 为所需要删除区间的结束迭代器的下一个地址，即取 [iteratorBegin, iteratorEnd)

```
#include <iostream>
#include <set>
using namespace std;

int main()
{
    set<int> st;
    st.insert(100);
    st.insert(200);
    st.insert(100);
    st.insert(300);
    st.insert(500);
    set<int>::iterator it = st.find(300);

    // 删除一个区间内的所有元素
    st.erase(it, st.end());
    for (it = st.begin(); it != st.end(); it++)
    {
        cout << *it << " ";
    }
    return 0;
}

// 输出结果
100 200

```
equal_range()：返回一对迭代器，分别表示第一个大于或等于给定关键值的元素和第一个大于给定关键值的元素。这个返回值是一个 pair 类型，第一个是键的 lower_bound，第二个是键的 upper_bound。如果这一对定位器中哪个返回失败，则返回 end() 
