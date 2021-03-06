\(1\) 初始化各个顶点的顶标值；

\(2\) 找出符合“A\[i\]+B\[j\] = weight\[i,j\]”条件的边构成相等子图，使用匈牙利算法寻找相等子图的完美匹配；

\(3\) 如果找到相等子图的完美匹配，则算法结束，否则调整相关顶点的顶标值；

\(4\) 重复步骤\(2\)\(3\)，直到找到完美匹配为止。

第\(1\)步初始化顶点顶标值可采用式\(7-1\)计算：

![](/assets/km.png)

因为A\[i\]总是取与之相邻的边中最大的权值作为初始值，因此初始阶段能保证满足“A\[i\]+B\[j\] ≥ weight\[i,j\]”条件。如果在第\(2\)步的相等子图中没有找到完美匹配，说明相等子图中某个顶点出发的增广路径不能覆盖所有顶点。此时需要调整各个顶点的顶标值，然后重新在相等子图中寻找完美匹配。调整顶标的目的是为了扩大相等子图，使得更多的边进入相等子图，并最终能够找到一个完美匹配。设当前增广路径上所有属于X集合的顶点构成一个子集S，所有属于Y集合的顶点构成一个子集T，dx为顶标调整的变化量，则dx可采用式\(7-2\)给出的方法计算：

![](/assets/km2.png)

由dx的计算公式可知，如果把S集合中所有顶点的顶标都减少dx，一定会有一条一端在S中，另一端不在T中的边因满足“A\[i\]+B\[j\] = weight\[i,j\]”的条件而进入相等子图，这就扩大了相等子图。对S集合中所有顶点的顶标都减少dx之后，为了使原来已经在相等子图中的边继续留在相等子图中，需要将T集合中所有顶点的顶标值增加dx，使A\[i\]+B\[j\]之和不变。

现在总结一下顶标调整的方法，首先采用式\(7-2\)计算出调整变化量dx，然后将S集合中所有顶点的顶标值减少dx，同时将T集合中所有顶点的顶标值增加dx，这样的调整，对整个图上的所有顶点会产生如下四种结果。

对于两端点都在当前相等子图的增广路径上的边（xi, yj），其顶标值A\[i\]+B\[j\]的和没有变化。也就是说，原来属于相等子图的边，调整后仍然属于相等子图。

对于两端点都不在当前相等子图的增广路径上的边（xi, yj），其顶标值A\[i\]和B\[j\]的值没有变化。也就是说，此边与相等子图的隶属关系没有变化，原来属于相等子图的边现在仍然属于相等子图，原来不属于相等子图的边现在仍然不属于相等子图。

对于xi在当前相等子图的增广路径上，yj不在当前相等子图的增广路径上的边（xi, yj），其顶标值A\[i\]+B\[j\]的和略有减小，原来不属于相等子图，现在有可能属于相等子图，使得相等子图有机会得到扩大。

如果xi不在当前相等子图的增广路径上，yj在当前相等子图的增广路径上的边（xi, yj），其顶标值A\[i\]+B\[j\]的和略有增加，原来不属于相等子图，现在仍不属于相等子图。

由此可见，每次调整顶标，都能在图的基本状态保持不变的情况下扩大相等子图，使得相等子图有机会找到一个完美匹配，这就是顶标值调整在算法中的意义所在。

