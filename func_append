在使用append函数时，发生了一些奇怪的问题：

实例代码：
nums1:=[]int{0,1,2,3,4,5,6,7}
appendedNums :=[]int{}
appendedNums=nums1[:]
fmt.Println(appendedNums)
appendedNums=append(appendedNums[:3],appendedNums[4:]...)
fmt.Println(appendedNums)
fmt.Println(nums1)

第一次打印和第二次打印的结果均符合预期，但第三次打印"nums1"时，结果为
