# 5.1 分段地址转换

图 5-2 展示了更多关于处理器怎样将逻辑地址转换为线性地址的细节。

为了处理好分段地址转换，处理器使用了如下的数据结构：

* 描述符(Descriptors)
* 描述符表
* 选择器
* 段寄存器(Segment Registers)

## 5.1.1 描述符

段描述符向处理器提供了必需的数据将逻辑地址转换为线性地址。描述符是被编译器、链接器、加载器或操作系统创建的，而不是应用程序。图 5-3 说明了两种常用的描述符格式。所有段描述符都是这两种格式中的一种。段描述符的字段如下：

基址(BASE)：决定了段在 4GB 线性地址空间中的位置。处理器将三部分基址连接起来形成单独的 32 位的值(基址)

界限(LIMIT)：决定了段的大小。处理器用两部分的界限字段形成一个 20 位的界限值。处理器用两种方式解析界限字段，解析方式取决于粒度位(granularity bit)的设置：

1. 当单元的大小为一个字节时，定义一个最大为 1M 字节的段。
2. 如果单元的大小为 4K 字节，定义一个最大为 4G 字节的段。当(被)加载时 LIMIT 值将会被左移 12 位，低位会被 0 填充。

粒度位(Granularity bit)：决定了 LIMIT 字段被处理器解析的方式。当此位被复位时，LIMIT 将以一个字节为单元；当它被置位时， LIMIT 将以 4K 为一个单元。

类型(TYPE)：用于区别不同类型的描述符。

描述符特权级(DPL, Descriptor Privilege Level)：用于保护机制(参见第 6 章)

段存在位(Segment-Present bit)：如果此位为 0 ，则此描述符无效，不能用于地址转换；