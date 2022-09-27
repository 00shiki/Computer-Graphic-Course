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
Untuk mengubah file image menjadi binary menggunakan openCV, kita menggunakan fungsi yang telah disediakan :<br>
dengan source code yang sama, pada file HTML kita menambahkan canvas untuk menampilkan binary image.

1. Tambahkan canvas untuk binary image
```HTML
<canvas id="canvasBinary"></canvas>
```

2. pada file script.js, kita tambahkan code berikut:
```js
let src = cv.imread('canvasOutput');
let dst = new cv.Mat();
cv.cvtColor(src, src, cv.COLOR_RGBA2GRAY, 0);
cv.adaptiveThreshold(src, dst, 200, cv.ADAPTIVE_THRESH_GAUSSIAN_C, cv.THRESH_BINARY, 3, 2);
cv.imshow('canvasBinary', dst);
src.delete();
dst.delete();
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
