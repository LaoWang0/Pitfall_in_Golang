append用的浅拷贝
在使用append函数时，发生了一些奇怪的问题：

示例代码：
    nums1:=[]int{0,1,2,3,4,5,6,7}
    appendedNums :=[]int{}
    appendedNums=nums1[:]
    fmt.Println(appendedNums)
    appendedNums=append(appendedNums[:3],appendedNums[4:]...)
    fmt.Println(appendedNums)
    fmt.Println(nums1)

第一次打印和第二次打印的结果均符合预期，但第三次打印'nums1'时，结果为
[0 1 2 3 4 5 6 7]   //打印appendedNums数组
[0 1 2 4 5 6 7]     //打印去除元素3后，appendedNums数组
[0 1 2 4 5 6 7 7]   //打印nums1数组

可发现，nums1数组中的元素结构也被改变。
原因在于，使用append()将元素3删去时，对appendedNums数组内存进行操作。它将appendedNums数组的后4位置提到前面去，覆盖掉原来的元素，并减短appendedNums数组的长度，来实现删除数组元素。
但由于内存中的元素改变，并且nums1与appendedNums共享同一内存，所以nums1中的前7位元素变得跟appendedNums一样，最后一位元素仍是7（保持不变，因为没有对最后一位所在的内存进行操作）



下面这段代码也是同样的原因(将数组作为元素，append到更高阶的数组中）：
    array1:=[][]int{}
    nums2:=[]int{1,2,3}
    array1=append(array1,nums2)
    fmt.Println(array1)
    nums2[0]=100
    fmt.Println(array1)
    
打印结果为：
[[1 2 3]]
[[100 2 3]]
由于array1[1]与nums2共享同一块内存，因此nums2元素改变后，array1[1]也改变。同样的，array1[1]元素改变后，nums2也改变。
若不想让上述二者关联改变，将上述代码第三行改为：
	  array1=append(array1,append([]int(nil),nums2...))
先将nums2与一个为nil的数组（后面叫它nil数组）拼接，这样，append返回的是nil数组的地址。再将nil数组拼接到array1中。
这样操作，对nums2做更改后，不会影响array1中的元素


