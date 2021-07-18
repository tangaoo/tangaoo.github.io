---
layout: post
title:  "第2篇 ttlib element模块介绍"
categories: ttlib
tags:  element
author: tangoo
mathjax: true
---


* content
{:toc}

讲讲 element 模块，element 模块是对 `uint8 uint32 long size_t mem str ptr`等类型的封装，然后给所有自维护数据的容器使用。






{% raw %}

## 0. C++ 的泛型

这里先扯一句 C++ 的泛型，看一下示例代码：

```cpp
template<typename T>
T const & max_element(T const *array, unsigned int n) {
        T const *max_value = array;
        for (unsigned int i = 1; i < n; i++) {
                if (array[i] > *max_value) {
                        max_value = &(array[i]);
                }
        }
        return *max_value;
}
```
看似泛型 T 可以适应“所有”类型数组的问题，找出最大元素。但有一个最大的问题无法解决，不是所有数据都支持 > 操作符哦。也就是说，想完全的泛型，要把数据以及数据支持的操作都包含进来。element 就是解决这个问题的。

同样，我们看看 c 库是怎么解决这个问题的呢？

```cpp
void qsort(void *base, size_t nmemb, size_t size,
                  int (*compar)(const void *, const void *));
```

## 1. 为啥抽象 element 模块

有了前面的铺垫，这个问题答案是显而易见的了。

ttlib 中有很多自维护数据的容器，比如 vector list 等（补充一句 list_entry 这种是外部维护数据的）。那么这些容器会装哪些数据呢？有 `uint8、int、size_t、long、mem、str、ptr` 类型，所以我们需要抽象成 element 类型（详见源码），把数据与数据支持的操作封装起来，泛型化。然后在容器初始化时传入，实现在容器操作中根据不同数据类型回调对应的数据操作方法。下面看一下 vector 容器初始化代码：

~~~cpp
/*! init vector
 *
 * @param grow          the item grow
 * @param e             the element
 *
 * @return              the vector
 */
tt_vector_ref_t         tt_vector_init(tt_size_t grow, tt_element_t e);
~~~

## 2. 不同 data 类型差异
### 2.1 `uint8 int size_t long`等类型

这种纯数据比较简单，数据本身就在容器内部维护了，所以 insert 插入的时候直接就是 copy 拷贝，而 free 的时候直接置0就OK了。看看代码。

```cpp
static tt_void_t tt_element_uint8_free(tt_element_ref_t e, tt_pointer_t buff)
{
    /// check
    tt_assert_and_check_return(buff);

    *((tt_uint8_t *)buff) = 0;
}

static tt_void_t tt_element_uint8_copy(tt_element_ref_t e, tt_pointer_t buff, tt_cpointer_t data)
{
    /// check
    tt_assert_and_check_return(buff);

    *((tt_uint8_t *)buff) = tt_p2u8(data);
}
```

### 2.2 `str` 类型
`str` 比较特殊，`str` 是字符串指针，外面传进来的 `str` 即可能是动态分配也可能是静态分配（如字符串常量），所以在往容器里传入时，都是重新 duplicate 一次，也就是数据都在 element 内部维护，那么这样在释放时候，统一调用 free即可。看看代码。

```cpp
static tt_void_t tt_element_str_free(tt_element_ref_t e, tt_pointer_t buff)
{
    /// check
    tt_assert_and_check_return(e && buff);

    tt_pointer_t cstr =  *((tt_pointer_t*)buff);
    if(cstr)
    {
        tt_free(cstr);

        // clear it
        *((tt_pointer_t*)buff) = tt_null;
    }
}

static tt_void_t tt_element_str_dupl(tt_element_ref_t e, tt_pointer_t buff, tt_cpointer_t data)
{
    // check
    tt_assert_and_check_return(e && buff);

    // duplicate it
    if(data) *((tt_pointer_t**)buff) = strdup((tt_char_t const*)data);
    else *((tt_pointer_t**)buff) = tt_null;
}
```

### 2.3 `mem ptr` 类型

对于`mem` 和 `ptr` 类型，insert 插入时 duplicate 其实就是 copy，但是 free 需要用户自己传入 free 回调函数。element 内部仅仅只是置0，特别是 ptr 要注意，element 内部默认 free 不会释放 ptr 所指向的动态内存。具体见源码，这里就不贴了。

## 3. element 接口说明

* data 有很多种，所以统一抽象成 void* 类似，然后在各自函数内部强转。

* 如上面 tt_element_str_dupl 接口所示，element 接口大多都有 buff、data 形参，buff 是存储 element 的 buff，永远都是地址，而 data 就是 data，例如 `uint8、int、size_t、long、mem、str、ptr`。但是实际上都会抽象成 void* 类型。




{% endraw %}