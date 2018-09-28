下面给出Kuhn-Munkres算法的具体实现代码，Kuhn\_Munkres\_Match\(\)函数虽然很长，但是并不难理解，因为这个代码是严格按照之前给出的Kuhn-Munkres算法的流程实现的。包括顶标的初始化、使用匈牙利算法求完美匹配和顶标调整在内的三个主要算法步骤在Kuhn\_Munkres\_Match\(\)函数中都得到体现，并且界定非常清晰。其中寻找增广路径的FindAugmentPath\(\)函数与之前介绍匈牙利算法时给出的FindAugmentPath\(\)函数实现非常类似，区别就是使用sub\_map而不是直接使用edge，并且额外记录了x\_on\_path标识。ResetMatchPath\(\)函数负责每次开始寻找相等子图之前清除上一次搜寻产生的临时增广路径，ClearOnPathSign\(\)函数负责在每次搜寻增广路径之前清除顶点是否属于S集合或T集合的标识，大家可以从本书的配套代码中找到此函数的代码。

```
bool Kuhn_Munkres_Match(KM_MATCH *km)   
{  
    int i, j;  
    int A[UNIT_COUNT], B[UNIT_COUNT];  
    // 初始化Xi与Yi的顶标  
    for(i = 0; i < UNIT_COUNT; i++)   
    {  
        B[i] = 0;  
        A[i] = -INFINITE;  
        for(j = 0; j < UNIT_COUNT; j++)   
        {  
            A[i] = std::max(A[i], km->edge[i][j]);  
        }  
    }  
    while(true)   
    {  
        // 初始化带权二分图的相等子图  
        for(i = 0; i < UNIT_COUNT; i++)  
        {  
            for(j = 0; j < UNIT_COUNT; j++)   
            {  
                km->sub_map[i][j] = ((A[i]+B[j]) == km->edge[i][j]);  
            }  
        }  
        //使用匈牙利算法寻找相等子图的完备匹配  
        int match = 0;  
        ResetMatchPath(km);  
        for(int xi = 0; xi < UNIT_COUNT; xi++)   
        {  
            ClearOnPathSign(km);  
            if(FindAugmentPath(km, xi))   
                match++;  
            else   
            {  
                km->x_on_path[xi] = true;  
                break;  
            }  
        }  
        //如果找到完备匹配就返回结果  
        if(match == UNIT_COUNT)  
        {  
            return true;  
        }  
        //调整顶标，继续算法  
        int dx = INFINITE;  
        for(i = 0; i < UNIT_COUNT; i++)   
        {  
            if(km->x_on_path[i])  
            {  
                for(j = 0; j < UNIT_COUNT; j++)   
                {  
                    if(!km->y_on_path[j])   
                        dx = std::min(dx, A[i] + B[j] - km->edge[i][j]);  
                }  
            }  
        }  
        for(i = 0; i < UNIT_COUNT; i++)   
        {  
            if(km->x_on_path[i])   
                A[i] -= dx;  
            if(km->y_on_path[i])   
                B[i] += dx;  
        }  
    }  
 
    return false;  
} 
```


