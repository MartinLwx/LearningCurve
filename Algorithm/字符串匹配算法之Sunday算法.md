## 什么是Sunday算法

Sunday算法是字符串匹配算法的一种,  比KMP的代码简单.  平均情况下执行效率一样.

- 讨论前提: 假设文本串为`str`,  长度为`n`,  用`i`指针扫描;  匹配串为`pat`,  长度为`n`,  用`j`指针来扫描;  字符串下标从`0`开始;  

- 算法流程

  - ~~~c++
    str: a c b c d ? f d k f 
    pat: a c b c e
    
    // ? == c
    pat:     a c b c e
        
    // ? 不在pat中
    pat:   a b c d e
    pat:     a b c d e
    ~~~

  - pat向右边移动多少位取决于`?`这个位置上的字符**是否有在`pat`中出现过**

    - **出现过**:  我们将`?`和`pat`中对应的字符(*最靠右边的, 比如上面c在1和3的位置上都出现过, 我们选3这个位置的c*) 对齐,  值得关注的是我们这里**对齐的步数**是怎么算的.
      - 比如上面`pat`中第3个位置上的`c`, 首先它要走到字符串末尾-->再往前跨一步-->它就到了`?`这个位置了.  我们也就对齐成功了. 用公式来描述就是`m - last_occ[c]`. 其中`last_occ[c]`表示`c`这个字符在`pat`中最后出现的位置
      - :question:为什么选择靠右的位置,  道理很简单,  如果选了靠左的位置,  那么夹在中间的**可能匹配**就被我们跳过了
    - **没出现过**:  这个很好理解,  如果`?`这个位置的字符在`pat`中从来没有出现过,  那么可以直接跨过它.  可以想象一下如果不跨过的话,  每一次肯定有`?`和`pat`中某个字符的无意义比较,  *可以看看上面我列出来的浪费的情况*.  **这是很大的浪费**.  那么直接跨过的步数就很简单了,  肯定是`m + 1`步.  为了和**出现过**的情况的表达式统一.  可以表示为`m - (-1)`

  - 指针的更新

    - 每次`pat`移动后,  `j = 0`从头开始,  `i`则指向移动后的`pat`的起始位置在`str`中的索引. 
    - 在上面的讨论中我们已经得出了`pat`移动的规律,  那么我们的指针`i`更新也是一个道理.  就是`i + m - last_occ[i + m]`

- 举个:chestnut:

  - | index     |  0   | 1     | 2    | 3    | 4    | 5    | 6    | 7    | 8    | 9    | 10   |
    | --------- | :--: | ----- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- | ---- |
    | str       |  a   | b     | c    | d    | c    | a    | o    | b    | x    | c    | d    |
    | pat       |  a   | **x** | c    | d    |      |      |      |      |      |      |      |
    | move:one: |      |       | a    | x    | c    | d    |      |      |      |      |      |
    | move:two: |      |       |      |      |      |      |      | a    | x    | c    | d    |

  - :one:在`index[1]`的位置`b`和`x`不匹配, 此时我们检查`index[4]`位置上的`c`有没有在`pat`里面出现过,  显然`pat[1] = c`,  根据上面提到的规则,  此时`pat`向右边移动`4 - 2 = 2`格.  `i`的更新为`i = 0 + 4 - 2 = 2`

  - :two:在`index[2]`的`a`位置和`c`不匹配, 此时我们检查`index[6]`位置上的`o`有没有在`pat`里面出现过,  显然,  我们在`pat`中找不到`o`,  根据上面提到的规则,  此时`pat`向右边移动`4-(-1) = 5`格.  `i`的更新为`i = 2 + 4 - (-1) = 7`

- 复杂度分析

  - **最好的时间复杂度**就是$T-O(m + n/m)≈O(n)$,  $m$是每次0位置上的比较.  即每一次比较,  如果每次将`pat[0]`和`str[0]`比较都是不匹配,  而且`?`的字符没有出现在`pat`中,  那么一次就能
  - **最差时间复杂度**就是$T-O(nm)$,  可以想象一下每次都是`pat[m-1]`才匹配失败,  而且每次只能动一格的情况
  - **平均时间复杂度**就是$T-O(n+m)$

- 特点

  - 并不规定`pat`中字符匹配的顺序,  像Boyer-Moore算法就规定了是从右向左比,  所以Sunday算法会更加灵活.  你完全可以从一个最可能不匹配的位置开始比较.  *以下面为例,  如果从index[1]`开始比较,  我们第一次就发现不匹配了.  此时就可以马上看`t`,  发现不在`pat`中,  那么我们可以直接跨过去*

  - | index |  0   | 1     | 2    | 3    | 4    | 5    | 6    | 7    | 8    |
    | ----- | :--: | ----- | ---- | ---- | ---- | ---- | ---- | ---- | ---- |
    | str   |  a   | b     | c    | d    | t    | w    | x    | y    | b    |
    | pat   |  a   | **x** | c    | d    |      |      |      |      |      |
    | move  |      |       |      |      |      | a    | x    | c    | d    |

## 写出代码

- 以[Leetcode 28. Implement strStr()](https://leetcode-cn.com/problems/implement-strstr/)为例,  可以在上面提交代码测试.  此处`haystack`即主串,  `needle`为子串.  经过我的实际测试,  **Sunday算法的执行效率和KMP持平,  代码还比KMP简单​**:thumbsup:

- 要点
  - `last_occ`的初始化,  默认为`-1`,  对应:two:.  其他情况更新为在`str`中的索引就好了. 

~~~c++
class Solution {
public:
    vector<int> get_last_occ(string pat)
    {
        vector<int> occ;
        for(int i=0;i<256;i++)      //ASCII码表的上限为256
            occ.push_back(-1);
        for(int i=0;i<pat.size();i++)   //从左向右遍历, 所以相同字符总是能更新为最右边的字符
            occ[pat[i]] = i;
        return occ;
    }
    int strStr(string haystack, string needle) {
        int n = haystack.size();
        int m = needle.size();
        auto last_occ = get_last_occ(needle);
        int i = 0;
        while(i <= n-m)
        {
            int cur;
            for(cur=i;cur<i+m;cur++)    // 主串从i开始, 模式串都从0开始比较
                if(haystack[cur] != needle[cur - i])
                    break;
            if(cur == i+m)      // cur == i + m说明已经找到了符合的子串
                return i;
            else{
                i += m;		// 定位到?的位置
                if(i < n)   
                    i -= last_occ[haystack[i]];		// 查看?字符的最后出现位置
                else
                    break;
            }
        }
        return -1;
    }
};
~~~



##### 