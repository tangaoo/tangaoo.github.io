---
layout: post
title: ttlib(1) 编码风格
---

1. 为什么函数、类型前有 tt_ 前缀？
> C 没有空间命名，而ttlib是以库的形式提供给使用者，为避免命名冲突，所以加 tt_ 前缀。tt_ 取自我的名字缩写，而且重复字母也便于打字。

2. 命名规则
> ttlib 采用小写加下划线形式，这个主要是个人习惯。
  
3. 头文件引用
> 采用完整路径引用。有点是提高编译速度，也不需要而外工程配置。为了减少头文件包含数量，每个模块都会有一个以该模块名命令的.h文件，用来提供该模块的所有接口，如utils模块中utils.h文件。

4. 常用类型
> 所有常用类型都重新 typedef 了，主要还是为避免冲突。

~~~ c
typedef signed int                                 tt_int_t;
typedef unsigned int                               tt_uint_t;
typedef signed short                               tt_short_t;
typedef unsigned short                             tt_ushort_t;
typedef signed char                                tt_int8_t;
typedef unsigned char                              tt_uint8_t;
typedef signed short                               tt_int16_t;
typedef unsigned short                             tt_uint16_t;
typedef signed int                                 tt_int32_t;
typedef unsigned int                               tt_uint32_t;
typedef signed long long                           tt_int64_t;
typedef unsigned long long                         tt_uint64_t;
typedef float                                      tt_f32_t;
typedef double                                     tt_f64_t;
typedef char                                       tt_char_t;
typedef tt_uint8_t                                 tt_byte_t;
typedef tt_int8_t                                  tt_bool_t;
typedef void                                       tt_void_t;
typedef tt_void_t *                                tt_pointer_t;
typedef tt_void_t const *                          tt_cpointer_t;
typedef tt_pointer_t                               tt_handle_t;
~~~

5. 结构体定义
>  结构体命名格式为 tt_xxx_t， tt_xxx_ref_t， ref表结构体指针，具体看下面例子就明白了。

~~~ c
typedef union __tt_ipv4_t
{
    // u32, little endian
    tt_uint32_t     u32;

    // u16
    tt_uint16_t     u16[2];

    // u8
    tt_uint8_t      u8[4];
}tt_ipv4_t, *tt_ipv4_ref_t;
~~~

6. 注释风格
> 注释风格主要就是个人习惯，目标就是整齐清晰。另外我是用 vscode + vim 码代码，可以设置 User Snippets，这样就可以把常用注释格式保存，方便使用。具体风格见代码。
  
7. 局部变量
> 局部变量没有在函数头一起定义，而是在使用处定义。

具体风格看代码哈～