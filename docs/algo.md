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

