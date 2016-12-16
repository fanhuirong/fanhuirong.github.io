
---

title: codewars 记录
---
#codewars

###字符串翻转 
对一句话，大于等于五个字符的单词进行翻转
```
spinWords( "Hey fellow warriors" ) => returns "Hey wollef sroirraw" 
spinWords( "This is a test") => returns "This is a test" 
spinWords( "This is another test" )=> returns "This is rehtona test"
```
使用数组与字符串进行实现。
String.prototype.split()
Array.prototype.reverse()
Array.prototype.join()
```js
function spinWords(str){
  var strArr , tempArr
  strArr = str.split(" ");
  strArr.forEach(function(item,index){  
        tempArr = item.split('')
        if(tempArr.length>4){
          strArr[index] = tempArr.reverse().join("");
        }
  })
  return strArr.join(" ");
}
```
其他人优雅的解法 使用[Array.prototype.map](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array/map)
```js
function spinWords(words){
  return words.split(' ').map(function (word) {
    return (word.length > 4) ? word.split('').reverse().join('') : word;
  }).join(' ');
}
```


###Tribonacci Sequence

```js
tribonacci([3,2,1],10) =>[3,2,1,6,9,16,31,56,103,190]
tribonacci([1,1,1],1) =>[1]
tribonacci([300,200,100],0) =>[]

function tribonacci(signature,n){
  var tempArr=[]
  if(n==0){return tempArr}
  if(n<4&&n>0){
    for(var j =0;j<n;j++){
      tempArr.push(signature[j])
    }
     return tempArr
  }
  
  for(var i = 3;i<n;i++){
      signature[i]  =  signature[i-3] + signature[i-2] + signature[i-1]  
  }
  return signature
}

//进化
function tribonacci(signature,n){
 
  if(n<4){
	return signature.slice(0,n)
  }
  
  for(var i = 3;i<n;i++){
      signature[i]  =  signature[i-3] + signature[i-2] + signature[i-1]  
  }
  return signature
}

```
优雅的解法 使用Array.prototype.slice()将数组切割
```js
function tribonacci(signature,n){  
  for (var i = 0; i < n-3; i++) { // iterate n times
    signature.push(signature[i] + signature[i+1] + signature[i+2]); // add last 3 array items and push to trib
  }
  return signature.slice(0, n); //return trib - length of n
}
```