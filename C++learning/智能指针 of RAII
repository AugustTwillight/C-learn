# 智能指针（RAII Resource Acquisition Is Initialization）：
智能指针是为了解决动态内存分配时带来的内存泄漏以及多次释放同一块内存空间而提出的。C++ 11 中提供了智能指针的定义，所有关于智能指针的定义可以参考 <memory> 头文件。传统的指针在申请完成后，必须要调用 free 或者 delete 来释放指针，否则容易产生内存泄漏的问题；smart pointer 遵循 RAII 原则，当 smart pointer 对象创建时，即为该指针分配了相应的内存，当对象销毁时，析构函数会自动释放内存。需要注意的是，智能指针不能像普通指针那样支持加减运算。
# 参考：
[c++智能指针详解](https://blog.csdn.net/bitcarmanlee/article/details/124847634?ops_request_misc=elastic_search_misc&request_id=c36d5b8f52807910b3e0640be1900d1a&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~all~top_positive~default-1-124847634-null-null.142^v102^pc_search_result_base6&utm_term=C%2B%2B%E6%99%BA%E8%83%BD%E6%8C%87%E9%92%88&spm=1018.2226.3001.4187)
[智能指针（Microsoft）](https://learn.microsoft.com/zh-cn/cpp/cpp/smart-pointers-modern-cpp?view=msvc-170)
从比较简单的层面来看，智能指针是RAII(Resource Acquisition Is Initialization，资源获取即初始化)机制对普通指针进行的一层封装。这样使得智能指针的行为动作像一个指针，本质上却是一个对象，这样可以方便管理一个对象的生命周期。

在c++中，智能指针一共定义了4种：
auto_ptr、unique_ptr、shared_ptr 和 weak_ptr。其中auto_ptr 在 C++11已被摒弃，在C++17中已经移除不可用。
# 原始指针的问题：
原始指针的问题大家都懂，就是如果忘记删除，或者删除的情况没有考虑清楚，容易造成悬挂指针(dangling pointer)或者说野指针(wild pointer)。
- 野指针是指尚未初始化的指针，它指向的地址是未知的、不确定的、随机的。
- 悬挂指针：当指针指向的内存空间已经被释放，但是该指针没有任何的改变，以至于仍然指向已经被回收的内存地址，这种情况下该指针就被称为悬挂指针。

```
objtype *p = new objtype();
p -> func();
delete p;
```
上面的代码里面的问题主要有以下两点：
1.代码的最后，忘记执行delete p的操作。
2.如果func()中有异常，delete p语句执行不到。有可以在func中进行删除操作，理论上是可以这么做，实际操作起来，会非常麻烦也非常复杂。

此时，智能指针就可以方便我们控制指针对象的生命周期。在智能指针中，一个对象什么情况下被析构或被删除，是由指针本身决定的，并不需要用户进行手动管理

RAII 是一种利用对象生命周期来控制程序资源的简单技术。这个原则的基本思想是通过对象的构造函数来获取资源，接着控制对资源的访问使之在对象的生命周期内始终保持有效，最后在对象析构的时候释放资源。“获取资源即初始化”编程惯用法至关重要。 此习惯用法的主要目的是确保资源获取与对象初始化同时发生，从而能够创建该对象的所有资源并在某行代码中准备就绪。 实际上，RAII 的主要原则是为将任何堆分配资源（例如，动态分配内存或系统对象句柄）的所有权提供给其析构函数包含用于删除或释放资源的代码以及任何相关清理代码的堆栈分配对象。

RAll 思想实际上是把管理一份资源的责任托管给了一个对象。这种做法有两大特点：
1，不需要显式地释放资源，对象的生命周期结束时自动释放，避免了内存泄漏的风险。        
2，采用这种方式，对象所需的资源在其生命期内始终保持有效，但对象生命周期一旦结束，资源将自动释放，运用时一定要注意。
3，需要注意的是，智能指针不能像普通指针那样支持加减运算。

```
void UseRawPointer()
{
    // Using a raw pointer -- not recommended.
    Song* pSong = new Song(L"Nothing on You", L"Bruno Mars"); 

    // Use pSong...

    // Don't forget to delete!
    delete pSong;   
}

void UseSmartPointer()
{
    // Declare a smart pointer on stack and pass it the raw pointer.
    unique_ptr<Song> song2(new Song(L"Nothing on You", L"Bruno Mars"));

    // Use song2...
    wstring s = song2->duration_;
    //...

} // song2 is deleted automatically here.
```
如示例所示，智能指针是你在堆栈上声明的类模板，并可通过使用指向某个堆分配的对象的原始指针进行初始化。 在初始化智能指针后，它将拥有原始的指针。 这意味着智能指针负责删除原始指针指定的内存。 智能指针析构函数包括要删除的调用，并且由于在堆栈上声明了智能指针，当智能指针超出范围时将调用其析构函数，尽管堆栈上的某处将进一步引发异常。
通过使用熟悉的指针运算符（-> 和 *）访问封装指针，智能指针类将重载这些运算符以返回封装的原始指针。
**请始终在单独的代码行上创建智能指针，而绝不在参数列表中创建智能指针，这样就不会由于某些参数列表分配规则而发生轻微泄露资源的情况。**
示例演示了如何使用 C++ 标准库中的 unique_ptr 智能指针类型将指针封装到大型对象。

```
class LargeObject
{
public:
    void DoSomething(){}
};

void ProcessLargeObject(const LargeObject& lo){}
void SmartPointerDemo()
{    
    // Create the object and pass it to a smart pointer
    std::unique_ptr<LargeObject> pLarge(new LargeObject());

    //Call a method on the object
    pLarge->DoSomething();

    // Pass a reference to a method.
    ProcessLargeObject(*pLarge);

} //pLarge is deleted automatically when function block goes out of scope.
```
- 将智能指针声明为一个自动（局部）变量。 （不对智能指针本身使用 new 或 malloc 表达式。）
- 在类型参数中，指定封装指针的指向类型。
- 在智能指针构造函数中将原始指针传递至 new 对象。 （某些实用工具函数或智能指针构造函数可为你执行此操作。）
- 使用重载的 -> 和 * 运算符访问对象。
- 允许智能指针删除对象。

性能：
智能指针的设计原则是在内存和性能上尽可能高效。 例如，unique_ptr 中的唯一数据成员是封装的指针。 这意味着，unique_ptr 与该指针的大小完全相同，不是四个字节就是八个字节。 使用重载了 * 和 -> 运算符的智能指针访问封装指针的速度不会明显慢于直接访问原始指针的速度。
智能指针具有通过使用“点”表示法访问的成员函数。 例如，一些 C++ 标准库智能指针具有释放指针所有权的重置成员函数。 如果你想要在智能指针超出范围之前释放其内存将很有用，这会很有用，如以下示例所示：

```
void SmartPointerDemo2()
{
    // Create the object and pass it to a smart pointer
    std::unique_ptr<LargeObject> pLarge(new LargeObject());

    //Call a method on the object
    pLarge->DoSomething();

    // Free the memory before we exit function block.
    pLarge.reset();

    // Do some other work...

}
```
智能指针通常提供直接访问其原始指针的方法。 C++ 标准库智能指针拥有一个用于此目的的 get 成员函数，CComPtr 拥有一个公共的 p 类成员。 通过提供对基础指针的直接访问，你可以使用智能指针管理你自己的代码中的内存，还能将原始指针传递给不支持智能指针的代码。

```
void SmartPointerDemo4()
{
    // Create the object and pass it to a smart pointer
    std::unique_ptr<LargeObject> pLarge(new LargeObject());

    //Call a method on the object
    pLarge->DoSomething();

    // Pass raw pointer to a legacy API
    LegacyLargeObjectFunction(pLarge.get());    
}
```
## 智能指针的类型
### C++ 标准库智能指针
使用这些智能指针作为将指针封装为纯旧 C++ 对象 (POCO) 的首选项。
- unique_ptr
只允许基础指针的一个所有者。 除非你确信需要 shared_ptr，否则请将该指针用作 POCO 的默认选项。 可以移到新所有者，但不会复制或共享。 替换已弃用的 auto_ptr。 与 boost::scoped_ptr 比较。 unique_ptr 小巧高效；大小等同于一个指针且支持 rvalue 引用，从而可实现快速插入和对 C++ 标准库集合的检索。 头文件：<memory>。
- shared_ptr
采用引用计数的智能指针。 如果你想要将一个原始指针分配给多个所有者（例如，从容器返回了指针副本又想保留原始指针时），请使用该指针。 直至所有 shared_ptr 所有者超出了范围或放弃所有权，才会删除原始指针。 大小为两个指针；一个用于对象，另一个用于包含引用计数的共享控制块。 
- weak_ptr
结合 shared_ptr 使用的特例智能指针。 weak_ptr 提供对一个或多个 shared_ptr 实例拥有的对象的访问，但不参与引用计数。 如果你想要观察某个对象但不需要其保持活动状态，请使用该实例。 在某些情况下，需要断开 shared_ptr 实例间的循环引用。 头文件：<memory>。
# COM 对象的智能指针（经典 Windows 编程）
## 组件对象模型 （COM）
COM 是一种独立于平台的分布式面向对象的系统，用于创建可交互的二进制软件组件。 COM 是Microsoft的 OLE（复合文档）和 ActiveX（已启用 Internet 的组件）技术的基础技术。
可以使用各种编程语言创建 COM 对象。 面向对象的语言（如C++）提供简化 COM 对象的实现的编程机制。 这些对象可以位于单个进程中，在其他进程中，甚至在远程计算机上。
COM的核心是一组组件对象间交互的规范，定义了组件对象如何与其用户通过二进制接口标准进行交互，COM的接口是组件的类型契约。
当你使用 COM 对象时，请将接口指针包装到适当的智能指针类型中。 活动模板库 (ATL) 针对各种目的定义了多种智能指针。 你还可以使用 _com_ptr_t 智能指针类型，编译器在从 .tlb 文件创建包装器类时会使用该类型。 无需包含 ATL 标头文件时，它是最好的选择。
# [创建和使用 unique_ptr 实例](https://learn.microsoft.com/zh-cn/cpp/cpp/how-to-create-and-use-unique-ptr-instances?view=msvc-170)
unique_ptr 不共享它的指针。 它无法复制到其他 unique_ptr，无法通过值传递到函数，也无法用于需要副本的任何 C++ 标准库算法。 只能移动 unique_ptr。 这意味着，内存资源所有权将转移到另一 unique_ptr，并且原始 unique_ptr 不再拥有此资源。 我们建议你将对象限制为由一个所有者所有，因为多个所有权会使程序逻辑变得复杂。 因此，当需要智能指针用于纯 C++ 对象时，可使用 unique_ptr，而当构造 unique_ptr 时，可使用 make_unique Helper 函数。
两个 unique_ptr 实例之间的所有权转换:

```
auto ptrA = make_unique<Song>(L"DK",L"LOVE")
ptrA-------------->SongObject

auto ptrB = std::move(ptrA);
ptrA            
ptrB-------------->SongObject
```
unique_ptr 在 C++ 标准库的 <memory> 标头中定义。 它与原始指针一样高效，可在 C++ 标准库容器中使用。 将 unique_ptr 实例添加到 C++ 标准库容器很有效，因为通过 unique_ptr 的移动构造函数，不再需要进行复制操作。
unique_ptr的基本特征：可移动 但不可复制。移动会将所有权转给新的unique_ptr并重置旧的unique_ptr：

```
unique_ptr<Song> SongFactory(const std::wstring& artist, const std::wstring& title)
{
    // Implicit move operation into the variable that stores the result.
    return make_unique<Song>(artist, title);
}

void MakeSongs()
{
    // Create a new unique_ptr with a new object.
    auto song = make_unique<Song>(L"Mr. Children", L"Namonaki Uta");

    // Use the unique_ptr.
    vector<wstring> titles = { song->title };

    // Move raw pointer from one unique_ptr to another.
    unique_ptr<Song> song2 = std::move(song);

    // Obtain unique_ptr from function that returns by value.
    auto song3 = SongFactory(L"Michael Jackson", L"Beat It");
}
```
在 range for 循环中，注意 unique_ptr 通过引用来传递。 如果尝试通过此处的值传递，由于删除了 unique_ptr 复制构造函数，编译器将引发错误：

```
void SongVector()
{
    vector<unique_ptr<Song>> songs;
    
    // Create a few new unique_ptr<Song> instances
    // and add them to vector using implicit move semantics.
    songs.push_back(make_unique<Song>(L"B'z", L"Juice")); 
    songs.push_back(make_unique<Song>(L"Namie Amuro", L"Funky Town")); 
    songs.push_back(make_unique<Song>(L"Kome Kome Club", L"Kimi ga Iru Dake de")); 
    songs.push_back(make_unique<Song>(L"Ayumi Hamasaki", L"Poker Face"));

    // Pass by const reference when possible to avoid copying.
    for (const auto& song : songs)
    {
        wcout << L"Artist: " << song->artist << L"   Title: " << song->title << endl; 
    }    
}
```
如何初始化类成员 unique_ptr：

```
class MyClass
{
private:
    // MyClass owns the unique_ptr.
    unique_ptr<ClassFactory> factory;
public:

    // Initialize by using make_unique with ClassFactory default constructor.
    MyClass() : factory (make_unique<ClassFactory>())
    {
    }

    void MakeClass()
    {
        factory->DoSomething();
    }
};
```
可使用 make_unique 将 unique_ptr 创建到数组，但无法使用 make_unique 初始化数组元素：

```
// Create a unique_ptr to an array of 5 integers.
auto p = make_unique<int[]>(5);

// Initialize the array.
for (int i = 0; i < 5; ++i)
{
    p[i] = i;
    wcout << p[i] << endl;
}
```
# 创建和使用 shared_ptr 实例
shared_ptr 类型是 C++ 标准库中的一种智能指针，专为多个所有者需要管理对象生命周期的方案而设计。 在初始化一个 shared_ptr 之后，可复制它，按值将其传入函数参数，然后将其分配给其他 shared_ptr 实例。 所有实例均指向同一个对象，并共享对一个“控制块”（每当新的 shared_ptr 添加、超出范围或重置时增加和减少引用计数）的访问权限。 当引用计数达到零时，控制块将删除内存资源和自身。
一个shared_ptr有两个指针 一个指向对象，一个指向控制块

```
// shared_ptr-examples.cpp
// The following examples assume these declarations:
#include <algorithm>
#include <iostream>
#include <memory>
#include <string>
#include <vector>

struct MediaAsset
{
    virtual ~MediaAsset() = default; // make it polymorphic
};

struct Song : public MediaAsset
{
    std::wstring artist;
    std::wstring title;
    Song(const std::wstring& artist_, const std::wstring& title_) :
        artist{ artist_ }, title{ title_ } {}
};

struct Photo : public MediaAsset
{
    std::wstring date;
    std::wstring location;
    std::wstring subject;
    Photo(
        const std::wstring& date_,
        const std::wstring& location_,
        const std::wstring& subject_) :
        date{ date_ }, location{ location_ }, subject{ subject_ } {}
};

using namespace std;

int main()
{
    // The examples go here, in order:
    // Example 1
    // Example 2
    // Example 3
    // Example 4
    // Example 6
}
```
尽可能在首次创建内存资源时使用 make_shared 函数来创建 shared_ptr。 make_shared 异常安全。 它使用同一调用为控制块和资源分配内存，这会减少构造开销。 如果不使用 make_shared，则必须先使用显式 new 表达式来创建对象，然后才能将其传递到 shared_ptr 构造函数。 以下示例演示了同时声明和初始化 shared_ptr 和新对象的各种方式。

```
// Use make_shared function when possible.
auto sp1 = make_shared<Song>(L"The Beatles", L"Im Happy Just to Dance With You");

// Ok, but slightly less efficient. 
// Note: Using new expression as constructor argument
// creates no named variable for other code to access.
shared_ptr<Song> sp2(new Song(L"Lady Gaga", L"Just Dance"));

// When initialization must be separate from declaration, e.g. class members, 
// initialize with nullptr to make your programming intent explicit.
shared_ptr<Song> sp5(nullptr);
//Equivalent to: shared_ptr<Song> sp5;
//...
sp5 = make_shared<Song>(L"Elton John", L"I'm Still Standing");
```
声明和初始化对由其他 shared_ptr 分配的对象具有共享所有权的 shared_ptr 实例。 假设 sp2 是已初始化的 shared_ptr：
```
//Initialize with copy constructor. Increments ref count.
auto sp3(sp2);

//Initialize via assignment. Increments ref count.
auto sp4 = sp2;

//Initialize with nullptr. sp7 is empty.
shared_ptr<Song> sp7(nullptr);

// Initialize with another shared_ptr. sp1 and sp2
// swap pointers as well as ref counts.
sp1.swap(sp2);
```
原理说明
交换内容：swap 方法交换两个 shared_ptr 对象的内部状态。这包括：
原始指针：指向的实际内存地址。
控制块：包含引用计数、弱引用计数和删除器等元数据。
引用计数的行为：
交换操作不会改变对象的引用计数值本身，因为引用计数存储在控制块中，而控制块被整体交换。
交换后，两个 shared_ptr 对象的“所有权”互换：原指针的引用计数转移到新指针上。
例如，如果 p1 指向对象A（引用计数为2），p2 指向对象B（引用计数为1），交换后 p1 指向B（引用计数变为1），p2 指向A（引用计数变为2）。
为什么高效：swap 是原子操作，时间复杂O(1)，因为它只交换指针和控制块的地址，不涉及深拷贝或引用计数的修改3。
使用场景：常用于优化资源管理、避免临时拷贝，或在多线程环境中安全地交换指针。

在你使用复制元素的算法时，shared_ptr 在 C++ 标准库容器中很有用。 可以将元素包装在 shared_ptr 中，然后将其复制到其他容器中（请记住，只要需要，基础内存就会一直有效）。 以下示例演示如何在向量中对 remove_copy_if 实例使用 shared_ptr 算法。

```
vector<shared_ptr<Song>> v {
  make_shared<Song>(L"Bob Dylan", L"The Times They Are A Changing"),
  make_shared<Song>(L"Aretha Franklin", L"Bridge Over Troubled Water"),
  make_shared<Song>(L"Thalía", L"Entre El Mar y Una Estrella")
};

vector<shared_ptr<Song>> v2;
remove_copy_if(v.begin(), v.end(), back_inserter(v2), [] (shared_ptr<Song> s) 
{
    return s->artist.compare(L"Bob Dylan") == 0;
});

for (const auto& s : v2)
{
    wcout << s->artist << L":" << s->title << endl;
}
```
可以使用 dynamic_pointer_cast、static_pointer_cast 和 const_pointer_cast 来转换 shared_ptr。 这些函数类似于 dynamic_cast、static_cast 和 const_cast 运算符。 以下示例演示如何测试基类的 shared_ptr 向量中每个元素的派生类型，然后复制元素并显示有关它们的信息。

```
vector<shared_ptr<MediaAsset>> assets {
  make_shared<Song>(L"Himesh Reshammiya", L"Tera Surroor"),
  make_shared<Song>(L"Penaz Masani", L"Tu Dil De De"),
  make_shared<Photo>(L"2011-04-06", L"Redmond, WA", L"Soccer field at Microsoft.")
};

vector<shared_ptr<MediaAsset>> photos;

copy_if(assets.begin(), assets.end(), back_inserter(photos), [] (shared_ptr<MediaAsset> p) -> bool
{
    // Use dynamic_pointer_cast to test whether
    // element is a shared_ptr<Photo>.
    shared_ptr<Photo> temp = dynamic_pointer_cast<Photo>(p);
    return temp.get() != nullptr;
});

for (const auto&  p : photos)
{
    // We know that the photos vector contains only 
    // shared_ptr<Photo> objects, so use static_cast.
    wcout << "Photo location: " << (static_pointer_cast<Photo>(p))->location << endl;
}
```
可以通过下列方式将 shared_ptr 传递给其他函数：

按值传递 shared_ptr。 这将调用复制构造函数，增加引用计数，并使被调用方成为所有者。 此操作的开销很小，但此操作的开销也可能很大，具体取决于要传递多少 shared_ptr 对象。 当调用方和被调用方之间的（隐式或显式）代码协定要求被调用方是所有者时，请使用此选项。

按引用或常量引用传递 shared_ptr。 在这种情况下，引用计数不会增加，并且只要调用方不超出范围，被调用方就可以访问指针。 或者，被调用方可以决定基于引用创建一个 shared_ptr，并成为一个共享所有者。 当调用方并不知道被调用方，或当由于性能原因必须传递一个 shared_ptr 且希望避免复制操作时，请使用此选项。

传递基础指针或对基础对象的引用。 这使被调用方能够使用该对象，但它不会与调用方的 shared_ptr共享对象的所有权。 请注意被调用方从传递的原始指针中创建 shared_ptr 的情况，因为被调用方的 shared_ptr 拥有一个独立于调用方 shared_ptr 的引用计数。 当被调用方中的 shared_ptr 超出范围时，它将删除对象，使调用方的'shared_ptr'中的指针指向释放的内存。 当调用方 shared_ptr 超出范围时，将产生 double-free 结果。 仅当调用方和被调用方之间的协定明确指定调用方保留 shared_ptr 生存期的所有权时，才使用此选项。

在确定如何传递 shared_ptr 时，确定被调用方是否必须共享基础资源的所有权。 “所有者”是只要它需要就可以使基础资源一直有效的对象或函数。 如果调用方必须保证被调用方可以将指针的生命延长到其（函数）生存期以外，则请使用第一个选项。 如果不关心被调用方是否延长生存期，则按引用传递并让被调用方复制或不复制它。

如果必须为帮助程序函数提供对基础指针的访问权限，并且你知道帮助程序函数将使用该指针并且将在调用函数返回前返回，则该函数不必共享基础指针的所有权。 它只需在调用方的 shared_ptr 的生存期内访问指针即可。 在这种情况下，按引用传递 shared_ptr 或传递原始指针或对基础对象的引用是安全的。 通过此方式传递将提供一个小的性能好处，并且还有助于表达编程意图。

有时，例如，在一个 std::vector<shared_ptr<T>> 中，可能必须将每个 shared_ptr 传递给 lambda 表达式主体或命名函数对象。 如果 lambda 或函数没有存储指针，则将按引用传递 shared_ptr 以避免调用每个元素的复制构造函数。

```
void use_shared_ptr_by_value(shared_ptr<int> sp);

void use_shared_ptr_by_reference(shared_ptr<int>& sp);
void use_shared_ptr_by_const_reference(const shared_ptr<int>& sp);

void use_raw_pointer(int* p);
void use_reference(int& r);

void test() {
    auto sp = make_shared<int>(5);

    // Pass the shared_ptr by value.
    // This invokes the copy constructor, increments the reference count, and makes the callee an owner.
    use_shared_ptr_by_value(sp);

    // Pass the shared_ptr by reference or const reference.
    // In this case, the reference count isn't incremented.
    use_shared_ptr_by_reference(sp);
    use_shared_ptr_by_const_reference(sp);

    // Pass the underlying pointer or a reference to the underlying object.
    use_raw_pointer(sp.get());
    use_reference(*sp);

    // Pass the shared_ptr by value.
    // This invokes the move constructor, which doesn't increment the reference count
    // but in fact transfers ownership to the callee.
    use_shared_ptr_by_value(move(sp));
}
```
以下示例演示 shared_ptr 如何重载各种比较运算符以在 shared_ptr 实例所有的内存上实现指针比较。

```
// Initialize two separate raw pointers.
// Note that they contain the same values.
auto song1 = new Song(L"Village People", L"YMCA");
auto song2 = new Song(L"Village People", L"YMCA");

// Create two unrelated shared_ptrs.
shared_ptr<Song> p1(song1);    
shared_ptr<Song> p2(song2);

// Unrelated shared_ptrs are never equal.
wcout << "p1 < p2 = " << std::boolalpha << (p1 < p2) << endl;
wcout << "p1 == p2 = " << std::boolalpha <<(p1 == p2) << endl;

// Related shared_ptr instances are always equal.
shared_ptr<Song> p3(p2);
wcout << "p3 == p2 = " << std::boolalpha << (p3 == p2) << endl;
```
