# Assigment 1
## Technology Director : Riszky Hermawan
## Other team member:
- Solver : Aditya Firmansyah Soedira
- Programmer : Ahmad Luhur Pakerti


### Programming Language : Javascript
Pada tugas kali ini kami akan menggunakan bahasa pemrograman Javascript dengan HTML dan CSS. Lebih spesifiknya kita akan menggunakan fitur Canvas dan openCV js.


# Documentation
## - Read Image with Js
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



## - Convert Image to Binary
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

## - Query Pixel

## - Draw Text on Canvas
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


## - Animate 
Sebelum memulai animasi, kita harus mengosongkan canvas terlebih dahulu dengan method :
1. KIta gunakan bola sebagai contoh untuk animasi, pertama buat bola di canvas menggunakan code berikut:
```js
const canvas = document.getElementById('canvas');
const ctx = canvas.getContext('2d');

const ball = {
  x: 100,
  y: 100,
  radius: 25,
  color: 'blue',
  draw() {
    ctx.beginPath();
    ctx.arc(this.x, this.y, this.radius, 0, Math.PI * 2, true);
    ctx.closePath();
    ctx.fillStyle = this.color;
    ctx.fill();
  }
};

ball.draw();
```

2. Buat function draw untuk memulai animasi
```js
function draw() {
  ctx.clearRect(0,0, canvas.width, canvas.height);
  ball.draw();
  ball.x += ball.vx;
  ball.y += ball.vy;
  raf = window.requestAnimationFrame(draw);
}
```

3. Kali ini kita akan menggunakan pointer mouse untuk meng-triger animasi untuk mulai. Yaitu ketika kursos diarahkan ke canvas maka animasi dimulai, sebaliknya jika pointer diarahkan keluar maka animasi berhenti. Untuk melakukan itu, kita akan membuat event listener dengan parameter 'onchange' , lebih jelasnya sebagai berikut:
```js
canvas.addEventListener('mouseover', (e) => {
  raf = window.requestAnimationFrame(draw);
});

canvas.addEventListener('mouseout', (e) => {
  window.cancelAnimationFrame(raf);
});

//panggil fungsi draw() untuk menjalankan animasi
ball.draw()
```

4. Untuk mencegah bola bergeser keluar dari canvas, maka kita pasang boundaries atau batas yang jika bola tersebut melewatinya, bola tersebut akan memantul. Untuk itu tambahkan code berikut pada script:
```js
if (ball.y + ball.vy > canvas.height ||
      ball.y + ball.vy < 0) {
    ball.vy = -ball.vy;
  }
  if (ball.x + ball.vx > canvas.width ||
      ball.x + ball.vx < 0) {
    ball.vx = -ball.vx;
  }

```
## - Button
Untuk membuat button , menggunakan elemen HTML yaitu ```<button>``` dan pada javascript kita menggunakan eventlistener untuk merender fungsinya. Sebagai contoh kita akan menggunakan source code dari dokumentasi animasi. Kita akan membuat button untuk memulai dan menghentikan animasi.
1. Buat elemen button pada HTML
```HTML
<button id="btnStart">Start</button>
<button id="btnStop">Stop</button>
```
2. Buat event listener pada script
```js
const btnstart = document.getElementById('btnStart');
btnstart.addEventListener('click', (e) => {
  raf = window.requestAnimationFrame(draw);
})


const btnstop = document.getElementById('btnStop');
btnstop.addEventListener('click', (e) => {
  raf = window.cancelAnimationFrame(raf);
})
```




# Image 
![circle](img/circle.png)
![circlewithnoise](https://github.com/riszkyhermawan/Computer-Graphic-Course/blob/0f3f270ca0a57834590509c401a202735ad83f16/img/circle%20with%20noises.png)
![similarcircle](https://github.com/riszkyhermawan/Computer-Graphic-Course/blob/0f3f270ca0a57834590509c401a202735ad83f16/img/similar%20circles.png)
![variouscircle](https://github.com/riszkyhermawan/Computer-Graphic-Course/blob/0f3f270ca0a57834590509c401a202735ad83f16/img/various%20circle.png)
  
