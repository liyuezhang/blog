---
title: JS当中的数组去重、冒泡、快速排序
date: 2016-11-12 17:28:36
tags:
---

	## 数组去重：
	

			var arr = [1,1,1,2,2,2,3,3,3];
			function Array(arr){
				var res= [];
				for(var i=0;i<arr.length;i++){
					if(res.indexOf(arr[i])==-1){
						res.push(arr[i]);
					}
				}
				console.log(res);
			}
			Array(arr);

	
	

	  ## 快速排序 ：
	　　（1）在数据集之中，找一个基准点  
	　　（2）建立两个数组，分别存储左边和右边的数组  
	　　（3）利用递归进行下次比较 

	 
		      
		function quickSort(arr){
			if(arr.length<=1){return arr;}如果数组只有一个数，就直接返回；
			var num = Math.floor(arr.length/2);找到中间数的索引值，如果是浮点数，则向下取整
			var newValue = arr.splice(num,1);找到中间数的值 
			var left=[],right=[];
			for(var i=0;i<arr.length;i++){
				if(arr[i]<newValue){
					left.push(arr[i]);	基准点的左边的数传到左边数组
				}else{
					right.push(arr[i]);基准点的右边的数传到右边数组
				}
			}
			return quickSort(left).concat(newValue,quickSort(right));递归不断重复比较
		
		}
		console.log(quickSort([1,3,4,5,6,2]));

	
	
	## 冒泡排序：
	
	　　随便从数组中拿一位数和后一位比较，如果是想从小到大排序，那么就把小的那一位放到前面，大的放在后面，简单来说就是交换它们的位置，如此反复的交换位置就可以得到排序的效果。
	 
	 
	var arr = [3,1,4,2,5,21,6,15,63];
	
	function sortA(arr){
	    for(var i=0;i<arr.length-1;i++){
	    	因为一次循环只能交换一个最大的值，所以需要再套一层for循环。
	        for(var j=i+1;j<arr.length;j++){
	                      获取第一个值和后一个值比较
	            var cur = arr[i];
	            if(cur>arr[j]){
	                       因为需要交换值，所以会把后一个值替换，我们要先保存下来
	                var index = arr[j];
	                        交换值
	                arr[j] = cur;
	                arr[i] = index;
	            }
	        }
	    }
	    return arr;
	}
	console.log(sortA(arr));
	
