# 冒泡排序

## 思路

遍历每一个子集，都相邻比较最大并交换

## 实现

```javascript
function bubbleSort(arr){
	for(let i = 1; i < arr.length; i++){
  	for(let j = 0; j <= i; i++){
    	if(arr[j] > arr[j+1]){
      	const temp = arr[j]
        arr[j] = arr[j+1]
        arr[j+1] = temp
      }
    }
  }
  return arr
}
```

在比较交换的时候，就是计算机中最经典的交换策略，用临时变量`temp`保存值，但是面试官问过我，**ES6有没有简单的方法实现？** 有的，如下：

```javascript
arr2 = [1,2,3,4];
[arr2[0],arr2[1]] = [arr2[1],arr2[0]]  //ES6解构赋值
console.log(arr2)  // [2, 1, 3, 4]
```

所以，刚才的冒牌排序可以优化如下：

```javascript
function bubbleSort(arr){
	for(let i = 1; i < arr.length; i++){
  	for(let j = 0; j <= i; i++){
    	if(arr[j] > arr[j+1]){
        [arr[j],arr[j+1]] = [arr[j+1],arr[j]]
      }
    }
  }
  return arr
}
```

