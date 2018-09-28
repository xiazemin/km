结合一个带权最大匹配问题的实例，给出这个算法在实际应用中的一个实现。问题是这样的，某公司有5名技术工人，他们都可以完成公司流程中的5种工作，但是每个工人的技术侧重点不同，熟练程度也不同，因此他们完成同样的工作所产生的经济效益也不相同。如果用0～5范围内的值对每个工人完成每种工作所产生的经济效益进行评价，可得到如表7-1所示的经济效益矩阵。假如你是这家公司的负责人，你需要找到一种工人和工作之间的匹配关系，使得这种匹配关系能产生的经济效益最大。根据之前对Kuhn-Munkres算法的分析，我们针对这个问题设计了KM\_MATCH匹配数据结构，如下所示：

typedef struct tagKmMatch

{

int edge\[UNIT\_COUNT\]\[UNIT\_COUNT\]; //Xi与Yj对应的边的权重

bool sub\_map\[UNIT\_COUNT\]\[UNIT\_COUNT\];// 二分图的相等子图, sub\_map\[i\]\[j\] = 1 代表Xi与Yj有边

bool x\_on\_path\[UNIT\_COUNT\]; // 标记在一次寻找增广路径的过程中，Xi是否在增广路径上

bool y\_on\_path\[UNIT\_COUNT\]; // 标记在一次寻找增广路径的过程中，Yi是否在增广路径上

int path\[UNIT\_COUNT\]; // 匹配信息，其中i为Y中的顶点标号，path\[i\]为X中顶点标号

}KM\_MATCH;

相对于匈牙利算法中的GRAPH\_MATCH定义，KM\_MATCH的主要变化是增加了sub\_map作为相等子图定义和标识yi是否在增广路径上的y\_on\_path标识。相对于我们前面对Kuhn-Munkres算法的分析，edge对应于边的权重表weight，sub\_map对应于算法执行过程中的相等子图，x\_on\_path和y\_on\_path分别用于标识X集合和Y集合中的顶点是否属于增广路径上的S集合和T集合，path就是最后匹配的结果

```
            工作1 工作2 工作3 工作4 工作5
```

工人1        3       5          5         4          1

工人2         2       2          0         2          2

工人3        2        4          4          1          0

工人4        0        1           1          0         0

工人5         1         2          1         3          3

下面给出Kuhn-Munkres算法的具体实现代码，Kuhn\_Munkres\_Match\(\)函数虽然很长，但是并不难理解，因为这个代码是严格按照之前给出的Kuhn-Munkres算法的流程实现的。包括顶标的初始化、使用匈牙利算法求完美匹配和顶标调整在内的三个主要算法步骤在Kuhn\_Munkres\_Match\(\)函数中都得到体现，并且界定非常清晰。其中寻找增广路径的FindAugmentPath\(\)函数与之前介绍匈牙利算法时给出的FindAugmentPath\(\)函数实现非常类似，区别就是使用sub\_map而不是直接使用edge，并且额外记录了x\_on\_path标识。ResetMatchPath\(\)函数负责每次开始寻找相等子图之前清除上一次搜寻产生的临时增广路径，ClearOnPathSign\(\)函数负责在每次搜寻增广路径之前清除顶点是否属于S集合或T集合的标识。

