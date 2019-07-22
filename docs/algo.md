* 2-3 搜索树

	[2-3 搜索树](https://github.com/CyC2018/CS-Notes/blob/master/docs/notes/%E7%AE%97%E6%B3%95%20-%20%E7%AC%A6%E5%8F%B7%E8%A1%A8.md#2-3-%E6%9F%A5%E6%89%BE%E6%A0%91)

* 红黑树

	红黑树节点的颜色指的是指向节点的边的颜色

	[红黑树](https://github.com/CyC2018/CS-Notes/blob/master/docs/notes/%E7%AE%97%E6%B3%95%20-%20%E7%AC%A6%E5%8F%B7%E8%A1%A8.md#%E7%BA%A2%E9%BB%91%E6%A0%91)
	
* 霍夫曼编码

	用堆排序来存储出现的频次

	[霍夫曼编码](https://github.com/CyC2018/CS-Notes/blob/master/docs/notes/%E7%AE%97%E6%B3%95%20-%20%E5%85%B6%E5%AE%83.md)	

* 有这样一个数组 A，大小为 n，相邻元素差的绝对值都是1。如：A = {4, 5, 6, 5, 6, 7, 8, 9, 10, 9}。现在，给定 A 和目标整数 t，请找到 t 在 A 中的位置。除了依次遍历，还有更好的方法么?

    这道题目的解决方法非常有趣

    数组的第一个数为 arr[0]，要找的数为 t，设 delta = abs(t - arr[0])，由于每个相邻的数字之差的绝对值为 1，因此只有 arr[delta] 有等于 t 的可能，如果不等于，重复上述操作即可

    ```python
    def find_number_in_arr(arr, target):
        length = len(arr)

        delta = abs(target - arr[0])
        while delta < length:
            if arr[delta] == target:
                return delta
            delta += abs(target - arr[delta])       

        return -1 
    ```

* 最大频率栈

    实现 FreqStack，模拟类似栈的数据结构的操作的一个类。FreqStack 有两个函数： push(int x)，将整数 x 推入栈中。pop()，它移除并返回栈中出现最频繁的元素。如果最频繁的元素不只一个，则移除并返回最接近栈顶的元素。 ◼ 示例： push [5,7,5,7,4,5] pop() -> 返回 5，因为 5 是出现频率最高的。 栈变成 [5,7,5,7,4]。 pop() -> 返回 7，因为 5 和 7 都是频率最高的，但 7 最接近栈 顶。 栈变成 [5,7,5,4]。 pop() -> 返回 5 。 栈变成 [5,7,4]。 pop() -> 返回 4 。 栈变成 [5,7]

    ```python
    from collections import deque, defaultdict    

    class FreqStack(object):
        
        def __init__(self):
            self.freq = defaultdict(int)
            self.group = defaultdict(deque)
            self.max_freq = float('-inf')        
    
        def push(self, x):
            self.freq[x] = self.freq[x] + 1
            if self.freq[x] > self.max_freq:
                self.max_freq = self.freq[x]
            
            self.group[self.max_freq].append(x)

        def pop(self):
            res = self.group[self.max_freq].pop()
            if not self.group[self.max_freq]:
                self.max_freq -= 1
            self.freq[res] -= 1            

            return res
    ```

* 25匹赛马，没有秒表，五条跑道。用最少的比赛场次找出三匹跑得最快的马

    https://blog.csdn.net/qq_32040767/article/details/77677186
