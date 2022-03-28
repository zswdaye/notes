## 图片处理

### file转base64

```js
let base64Img = ''
function transform(file) {
  let reader = new FileReader();
  reader.readAsDataURL(file[0]);
  reader.onload = function(){
  	base64Img = reader.result
    console.log(reader.result); //获取到base64格式图片
  };
}
```

### base64转file

```js
/**
 * 将base64转换为文件
 * @param {String} dataurl base64字符串
 * @param {String} filename 文件名称
 *  */ 
function dataURLtoFile(dataurl, filename) {
  let arr = dataurl.split(','),
      mime = arr[0].match(/:(.*?);/)[1],
      bstr = atob(arr[1]),
      n = bstr.length,
      u8arr = new Uint8Array(n);
  while(n--){
      u8arr[n] = bstr.charCodeAt(n);
  }
  return new File([u8arr], filename, {type:mime});
}
```

### base64转blob流

```js
function dataURItoBlob(base64Data) {
  //console.log(base64Data);//data:image/png;base64,
  var byteString;
  if(base64Data.split(',')[0].indexOf('base64') >= 0)
    byteString = atob(base64Data.split(',')[1]);//base64 解码
  else{
    byteString = unescape(base64Data.split(',')[1]);
  }
  var mimeString = base64Data.split(',')[0].split(':')[1].split(';')[0];//mime类型 -- image/png

  // var arrayBuffer = new ArrayBuffer(byteString.length); //创建缓冲数组
  // var ia = new Uint8Array(arrayBuffer);//创建视图
  var ia = new Uint8Array(byteString.length);//创建视图
  for(var i = 0; i < byteString.length; i++) {
    ia[i] = byteString.charCodeAt(i);
  }
  var blob = new Blob([ia], {
    type: mimeString
  });
  return blob;
}
```

### blob流转base64

```js
function blobToDataURI(blob, callback) {
   var reader = new FileReader();
   reader.readAsDataURL(blob);
   reader.onload = function (e) {
     callback(e.target.result);
     console.log(e.target.result); //获取到base64格式图片
   }
}
```

