### 题目：

给定一个整数数组 nums 和一个整数目标值 target，请你在该数组中找出 和为目标值 target  的那 两个 整数，并返回它们的数组下标。

你可以假设每种输入只会对应一个答案。但是，数组中同一个元素在答案里不能重复出现。可以按任意顺序返回答案。

注: 仅有一个正确答案。

### 难度：

简单。

### 示例：

```
输入：nums = [2,7,11,15], target = 9
输出：[0,1]
解释：因为 nums[0] + nums[1] == 9 ，返回 [0, 1] 。
```

### 解题思路：

1.暴力破解：
空间复杂度为 O(1)
时间复杂度为 O(n²) 双层循环
2.用空间换时间
遍历的同时进行记录，利用哈希表记录已经遍历过的数组元素。

```
/*伪代码
1.获取数组长度，遍历数组
2.判断 value = target - current 是否存储在哈希表内
3.如果存在返回他们的数组下标，函数结束。如果不存在，存储数组下标和数组值，进行下一次判断。
*/
```

### 代码实现：

```c++
class Solution {
public:
    vector<int> twoSum(vector<int>& nums, int target) {
        //cout << nums.size();
        vector<int> result(2);
        //采用C++的Map结构来存储哈希表,需要记录的有数组元素的下标和元素值
        unordered_map<int, int> m;
        int index1, index2 = 0;
        int value = 0;
        //遍历数组
        for(int i=0; i<nums.size(); i++){
            //判断是否Value是否存在
            value = target - nums[i];
            if(m.find(value) != m.end()){
                index2 = i;
                index1 = m.at(value);
            }
            else{
                //不存在就插入到哈希表中去。
                //pair<T1, T2> p(v1, v2);
                m.insert(pair<int, int>(nums[i], i));
            }
        }
        result[0] = index1;
        result[1] = index2;
        return result;
    }
}
```

### 知识点：

#### 1.C++ 向量 vector

​	vector 是同一种类型的对象的集合，每个对象都有一个对应的整数索引值。我们把 vector 称为容器，是因为它可以包含其他对象。一个容器中的所有对象都必须是同一种类型的。

```
//头文件
#include <vector>
```

​	vector 是一个类模板(class template)。必须说明保存对象的类型。

```
vector<int> test1;
vector<string> test2;
```

​	此外还有：

```
//向量
vector<int> k;
//int指针的向量
vector<int*>kk;
//vector向量指针
vector<int>*kkk;
//int指针的向量指针
vector<int*>*kkkk;//对比int*p理解，指针变量前面的“*”表示该变量的类型为指针变量，p是指针变量名，而不是*p
//vector 不是一种数据类型，而只是一个类模板，可用来定义任意多种数据类型。
//vector 类型的每一种都指定了其保存元素的类型。因此，vector<int> 和 vector<string> 都是数据类型。
```

1).vector<int> k;

示例：

```
#include<iostream>  
#include<vector>  
using namespace std;  
int main(){  
    vector<int> k;  
    for (int i=0; i<10; i++)  
    {  
        k.push_back(i);//向k中追加值  
    }  
    for (int i=0; i<10; i++)  
    {  
        cout <<k[i] << end1;  
    }  
    system("pause");  
    return 0;  
}  
//k中的每一位存储了一个int类型的对象。
```

2).vector<int*> k;

示例：

```
#include<iostream>  
#include<vector>  
using namespace std;  
int main(){  
    vector<int*> k;  
    int *p = new int[10];  //int 数组
    for (int i=0; i<10; i++)  
    {  
        p[i] = i;  
        k.push_back(&p[i]);  
    }  
    for (int i=0; i<10; i++)  
    {  
        cout << *k[i]<< " ";
        //因为向量容器里面都是int型的指针变量,所以值都是指针，所以需要间接访问运算符*  
    }                           
    delete[]p;  
    system("pause");  
    return 0;  
}  
//kk中的每一位存储了一个int指针类型的对象。
```

3).vector<int> *k;

示例：

```
#include<iostream>  
#include<vector>  
using namespace std;  
int main()  {  
    //vector向量指针  
    vector<int> *k;
    k = new vector<int>[5];  
    //相当于int *p = new int[5];
    //即vector<int> *k=new vector<int>[5];  
    for (int i=0; i<5; i++)  
    {  
        for (int j=0; j<10; j++)  
        {  
            k[i].push_back(j);//像向量指针中追加值  
        }  
    }  
    for (int i=0; i<5; i++)  
    {  
        for (int j=0; j < k[i].size(); j++)  
            cout <<  k[i][j] << " ";  
        cout << endl;  
    }  
    delete[] kkk;  
    system("pause");  
}  
//kkk中的每一位存储了一个vector类型的对象,vector类型的对象的每一位存储了一个int 类型的对象。
```

4).vector<int*> *kkkk;

示例：

```
#include<iostream>  
#include<vector>  
using namespace std;  
int main()  {  
	//int指针的向量指针  
    vector<int*> *k;
    k = new vector<int*>[5];  
    int *p = new int[10];  
    for (int i=0; i<5; i++)  
    {  
        for (int j=0; j<10; j++)  
        {  
            p[j] = j;  
            k[i].push_back(&p[j]);  
        }  
    }  
    for (int i=0; i<5; i++)  
    {  
        for (int j=0; j<10; j++)  
        {  
            cout<<*k[i][j]<<"  ";  
        }  
        cout << endl;  
    }  
    delete[]p;  
    delete[]k;  
    system("pause");  
}  
//kkk中的每一位存储了一个vector类型的对象,vector类型的对象的每一位存储了一个int指针类型的对象。
```

#### 2.C++  Map类

​	可以利用Map实现哈希表。map以模板(泛型)方式实现，可以存储任意类型的数据，包括使用者自定义的数据类型。Map主要用于资料一对一映射(one-to-one)的情況，map內部的实现自建一颗红黑树，这颗树具有对数据自动排序的功能，在map内部所有的数据都是有序的。

**功能：**

​	自动建立key － value的对应。key 和 value可以是任意你需要的类型，包括自定义类型。

**声明：**

```
#include <map>

map<type, type> mapName;
```

**部分功能**

```
#插入
#include <iostream>
#include <map>

int main()
{
    std::map<char, int> mymap;

    // 插入单个值
    mymap.insert(std::pair<char, int>('a', 100));
    mymap.insert(std::pair<char, int>('z', 200));

    //返回插入位置以及是否插入成功
    std::pair<std::map<char, int>::iterator, bool> ret;
    ret = mymap.insert(std::pair<char, int>('z', 500));
    if (ret.second == false) {
        std::cout << "element 'z' already existed";
        std::cout << " with a value of " << ret.first->second << '\n';
    }

    //指定位置插入
    std::map<char, int>::iterator it = mymap.begin();
    mymap.insert(it, std::pair<char, int>('b', 300));  //效率更高
    mymap.insert(it, std::pair<char, int>('c', 400));  //效率非最高

    //范围多值插入
    std::map<char, int> anothermap;
    anothermap.insert(mymap.begin(), mymap.find('c'));

    // 列表形式插入
    anothermap.insert({ { 'd', 100 }, {'e', 200} });

    return 0;
}
```

```
#取值
map<int, string> ID_Name;

//ID_Name中没有关键字2016，使用[]取值会导致插入
//因此，下面语句不会报错，但打印结果为空
cout<<ID_Name[2016].c_str()<<endl;

//使用at会进行关键字检查，因此下面语句会报错
ID_Name.at(2016) = "Bob";
```

```
#查找
// 关键字查询，找到则返回指向该关键字的迭代器，否则返回指向end的迭代器
// 根据map的类型，返回的迭代器为 iterator 或者 const_iterator
iterator find (const key_type& k);
const_iterator find (const key_type& k) const;
```

### 官方代码：

```
class Solution {
public:
    vector<int> twoSum(vector<int>& nums, int target) {
        unordered_map<int, int> hashtable;
        for (int i = 0; i < nums.size(); ++i) {
            auto it = hashtable.find(target - nums[i]);
            if (it != hashtable.end()) {
                return {it->second, i};
            }
            hashtable[nums[i]] = i;
        }
        return {};
    }
};
```

### 热评：

```
有人相爱，有人夜里开车看海，有人leetcode第一题都做不出来。
```

```
女朋友平安夜和别人吃苹果我没哭，她和我说在跑步我也不在乎，但是你告诉我这第一题是简单的开篇，我哭了。
```

