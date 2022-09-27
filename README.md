# Assigment 1
## Technology Director : Riszky Hermawan
## Other team member:
- Solver : Aditya Firmansyah Soedira
- Programmer : Ahmad Luhur Pakerti


### Programming Language : Javascript
Pada tugas kali ini kami akan menggunakan bahasa pemrograman Javascript dengan HTML dan CSS. Lebih spesifiknya kita akan menggunakan fitur Canvas pada Javascript.


## Documentation
### - Read Image with Js
1. Buat web dan sediakan input untuk file <br>
HTML:
```HTML
<!DOCTYPE html>
<html>
<head>
<meta charset="utf-8">
<title>Hello OpenCV.js</title>
</head>
<body>
<h2>Hello OpenCV.js</h2>
<div>
  <div class="inputoutput">
    <img id="imageSrc" alt="No Image" />
    <div class="caption">imageSrc <input type="file" id="fileInput" name="file" /></div>
  </div>
</div>
<script type="text/javascript" src="script.js">
<script async src="opencv.js" type="text/javascript"></script>
</script>
</body>
</html>
```
Javascript:
```js
let imgElement = document.getElementById("imageSrc")
let inputElement = document.getElementById("fileInput");
inputElement.addEventListener("change", (e) => {
  imgElement.src = URL.createObjectURL(e.target.files[0]);
}, false);
```

2. Masukan file opencv.js, ada dua cara yaitu dengan download dari https://github.com/opencv/opencv/releases  atau copy paste code dari https://docs.opencv.org/3.4.0/opencv.js. Setelah itu sesuaikan directorynya. Dan tambahkan class ini pada Javascript untuk mengecek apakah openCV sudah berhasil diakses
```js
var Module = {
  // https://emscripten.org/docs/api_reference/module.html#Module.onRuntimeInitialized
  onRuntimeInitialized() {
    document.getElementById('status').innerHTML = 'OpenCV.js is ready.';
  }
};
```

3. Buat onload function 
 ```js
imgElement.onload = function () {
  let mat = cv.imread(imgElement);
  cv.imshow('canvasOutput', mat);
  mat.delete();
};
 ```
 4. Buat elemen canvas pada html untuk menampilkan gambar.
 ```HTML
<canvas id="canvasOutput" ></canvas>
 ```



### - Convert Image to Binary
Untuk melakukan convert gambar ke binary ada 3 langkah yaitu : Image -> base64 -> binary <br>
Jika kita menggunakan source code yang sama seperti di atas, maka untuk lebih jelasnya sebagai berikut:

1. Ubah image jadi base64 string
```js
//fungsi reader akan ditambah seperti ini:
reader = onLoad = function(e){
...
      var base64Img = e.target.result;
      var binaryImg = convertDataURIToBinary(base64Img);
      var blob = new Blob([binaryImg], {type: f.type});
      blobURL = window.URL.createObjectURL(blob);
      fileName = f.name;      
...
}

```

2. Ubah base64 String jadi binary (buat fungsi ```convertDataURIToBinary()``` )
```js
function convertDataURIToBinary(dataURI) {
	var BASE64_MARKER = ';base64,';
	var base64Index = dataURI.indexOf(BASE64_MARKER) + BASE64_MARKER.length;
	var base64 = dataURI.substring(base64Index);
	var raw = window.atob(base64);
	var rawLength = raw.length;
	var array = new Uint8Array(new ArrayBuffer(rawLength));

	for(i = 0; i < rawLength; i++) {
		array[i] = raw.charCodeAt(i);
	}
	return array;
}
```

### - Query Pixel

### - Draw Text on Canvas
Cara untuk memasukan tulisan pada Canvas adalah dengan menggunakan salah satu fitur dari Canvas Javascript, lebih jelasnya sebagai berikut: <br>
Dengan source code yang sama maka :
```js
const canvas = document.getElementById("ourCanvas");
const context = canvas.getContext('2d');

//define font
context.font = "20px Georgia";
//Masukkan text ke Canvas
context.fillText = ("Ini adalah text", 10, 50);

```
Pada method ``` fillText() ``` diisi dengan text yang akan dimasukkan dan posisi nya di Canvas.



### - Animate 

