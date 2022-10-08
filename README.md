# Assigment 1
## Technology Director : Riszky Hermawan (1313620031)
## Other team member:
- Solver : Aditya Firmansyah Soedira (1313620017)
- Programmer : Ahmad Luhur Pakerti (1313620028)


### Programming Language : Javascript
Pada tugas kali ini kami akan menggunakan bahasa pemrograman Javascript dengan HTML dan CSS. Lebih spesifiknya kita akan menggunakan fitur Canvas dan openCV-js. OpenCV-js dipilih karena instalasi (set up) nya yang mudah. Berikut langkah-langkah set up openCV-js pada web :

## Set Up OpenCV-js
1. Buat elemen ```input file``` dan ```canvas``` pada HTML yang sudah dibuat tadi dengan cara menambahkan code berikut pada elemen ```body```
```HTML
<!DOCTYPE html>
<html>
<head>
<meta charset="utf-8">
<title>Komgraf</title>
</head>
<body>

</body>
</html>
```
2. Buat file ```openCv.js``` kemudian buka link script openCV-js di [link-openCV-js](https://docs.opencv.org/3.4.0/opencv.js.) <br>
Copy semua code dan paste kedalam file ```openCV.js```
3. Masukan code dibawah pada bagian akhir elemen ```body``` di HTML
```HTML
<script async src="opencv.js" type="text/javascript"></script>
```

# Documentation
## - Read Image with Js
1. Buat file HTML yang berisi input dan canvas.
```HTML
<h2>KOMGRAF</h2>
<p id="status">OpenCV.js is loading...</p>
<div>
  <div class="inputoutput">
    <img id="imageSrc" alt="No Image"/>
    <div class="caption">imageSrc <input type="file" id="fileInput" name="file" /></div>
  </div>        
  <div class="inputoutput">
    <div class="caption">Canvas Gambar= </div>
    <canvas id="canvasOutput" ></canvas>
  </div>
</div>
```
2. Buat file ```script.js``` dan tambahkan code dibawah ini pada bagian akhir elemen ```body``` HTML untuk membuat koneksi HTML -> Js.
```HTML
<script src="script.js"></script>
```
 2. Pada ```script.js``` tambahkan code ini pada javasript untuk mengecek apakah openCV sudah berfungsi.
 ```js
 var Module = {
  // https://emscripten.org/docs/api_reference/module.html#Module.onRuntimeInitialized
  onRuntimeInitialized() {
    document.getElementById('status').innerHTML = 'OpenCV.js is ready.';
  }
};
```
 3. Masukkan canvas dan input ke variabel di Javascript.
 ```js
 let imgElement = document.getElementById('imageSrc');
let inputElement = document.getElementById('fileInput');
inputElement.addEventListener('change', (e) => {
  imgElement.src = URL.createObjectURL(e.target.files[0]);
}, false);
 ```
 
 4. Buat onload function untuk membaca file dan menampilkannya dalam canvas
 ```js
 imgElement.onload = () => {
  let mat = cv.imread(imgElement);
  cv.imshow('canvasOutput', mat);
  mat.delete();
 };
 ```

## - Convert Image to Binary
Untuk melakukan konversi gambar biasa ke binary kita akan menggunakan ```function cv.adaptiveThreshold()``` dari openCV. Sebelumnya kita harus menambahkan canvas pada HTML .

1. Tambahkan canvas untuk binary image dengan pada HTML dengan menambahkan code berikut setelah bagian ```canvas output``` di HTML,
```HTML
<div class="inputoutput">
    <div class="caption">Canvas Binary =</div>
    <canvas id="canvasBinary"></canvas>
</div>
```

2. Masukan code ini pada onload function di ```script.js``` untuk mengubah file img -> binary img
```js
let src = cv.imread('canvasOutput');
let dst = new cv.Mat();
cv.adaptiveThreshold(src, dst, 200, cv.ADAPTIVE_THRESH_GAUSSIAN_C, cv.THRESH_BINARY, 3, 2);
cv.imshow('canvasBinary', dst);
src.delete();
dst.delete();
```
sehingga onload function akan jadi seperti ini:
```js
imgElement.onload = () => {
  //read image
  let mat = cv.imread(imgElement);
  cv.imshow('canvasOutput', mat);
  mat.delete();


  //read image
  let src = cv.imread('canvasOutput');
  let dst = new cv.Mat();
  cv.adaptiveThreshold(src, dst, 200, cv.ADAPTIVE_THRESH_GAUSSIAN_C, cv.THRESH_BINARY, 3, 2);
  cv.imshow('canvasBinary', dst);
  src.delete();
  dst.delete();
};

```
3. Agar hanya terlihat 2 gambar, kita bisa menghilangkan gambar pertama dengan menggunakan style inline css pada elemen ```img``` di HTML, sehingga elemen tersebut manjadi:
```HTML
<img id="imageSrc" alt="No Image" style="display:none"/>
```



## - Query Pixel
Kali ini kita akan menampilkan hasilnya pada console di browser. Untuk itu klik kanan pada browser dan pilih ```inspect``` dan pilih ```console``` .<br>
Untuk mengubah gambar menjadi matrix kita menggunakan method yang disediakan oleh openCV js. Dalam openCV js pixel diambil menggunakan RGBA, untuk mengkonversi gambar binary ke matriks kita hanya akan menggunakan nilai alpha saja.
1. Pada ```onload()``` di ```script.js``` function yang telah kita buat, tambahkan code dibawah ini untuk mengambil setiap pixel pada gambar binary
```js
let r = [];
  for (let i = 0; i < dst.rows; i++) {
    let c = [];
    for (let j = 0; j < dst.cols; j++) {
      let A = dst.data[i * dst.cols * dst.channels() + j * dst.channels() + 3];
      if(A===0){
        c.push(1);
      }else {
        c.push(0);
      }
    }
    r.push(c);
    c = [];
  }
```
2. Masukkan code di bawah inipada bagian akhir code di ```onload``` function untuk menampilkan hasil pada console
```js
console.table(r)
```

3. Pindahkan code dibawah ini dari atas ke bagian paling <b>akhir</b> di ```onload``` function, code ini terletak di bawah method ```cv.tresshold```
```js
src.delete();
dst.delete();
```


## - Draw Text on Canvas
Cara untuk memasukan tulisan pada Canvas adalah dengan menggunakan salah satu fitur dari Canvas Javascript, lebih jelasnya sebagai berikut: <br>
1. Masukkan code ini pada ```script.js``` dan text akan masuk pada ```canvas binary```
```js
const canvas = document.getElementById("canvasBinary");
const ctx = canvas.getContext('2d');

//define font
ctx.font = "20px Georgia";
//Masukkan text ke Canvas
ctx.fillText("Ini adalah text", 10, 50);
```
Pada method ``` fillText() ``` diisi dengan text yang akan dimasukkan dan posisi nya di Canvas.


## - Animate 
Kita akan membuat animasi pada ```canvas binary``` , akan ada garis yang seolah-olah melakukan scan pada canvas.
1. Atur panjang dan lebar canvas di HTML, dengan cara ubah elemen ```canvas binary``` menjadi seperti berikut
```HTML
<canvas id="canvasBinary" width="500" height="300" style="border: 10px solid black"></canvas>
```
1. Buat variabel x dan y pada ```script.js``` bagian akhir, untuk set point start animasi.
```js
let x= 0;
let y =0;
```

2. Masukkan code dibawah ini pada ```script.js``` dibawah deklarasi variabel x dan y tdai, untuk memulai animasi
```js
window.requestAnimationFrame(function loop() {
  if(y<=canvas.height){
    x+=1
    ctx.fillStyle = 'green'
    ctx.fillRect(x,y,10,10)
    window.requestAnimationFrame(loop)
    if(x==canvas.width){
      x = 0
      y+=10
    }
  }
})
```


## - Button
Untuk membuat button , menggunakan elemen HTML yaitu ```<button>``` dengan properti ```onclick()```. 
1. Buat elemen ```button``` pada HTML, letakkan dibawah elemen ```canvas binary```
```HTML
<button onclick="convert()">Mulai Konversi</button>
<button onclick="anim()">Mulai Animasi</button>
```
2. Buat fungsi ```convert()``` pada ```script.js``` , dan ambil code pada ```onload()``` function yang bagian ini seperti dibawah, sehingga code pada ```convert()``` dan ```onload()``` menjadi seperti ini: <br>
 - img.onload
 ```js
 imgElement.onload = () => {
  let mat = cv.imread(imgElement);
  cv.imshow('canvasOutput', mat);
  mat.delete();

};

- convert()
```js
const convert = () => {
  //convert img -> binary
  let src = cv.imread('canvasOutput');
  let dst = new cv.Mat.eye(src.rows, src.cols, src.type());
  cv.cvtColor(src, src, cv.COLOR_RGBA2GRAY, 0);
  cv.adaptiveThreshold(src, dst, 200, cv.ADAPTIVE_THRESH_GAUSSIAN_C, cv.THRESH_BINARY, 3, 2);
  cv.imshow('canvasBinary', dst);

  //Buat matriks pixel
  let r = [];
  for (let i = 0; i < dst.rows; i++) {
    let c = [];
    for (let j = 0; j < dst.cols; j++) {
      if (dst.isContinuous()) {
        let A = dst.data[i * dst.cols * dst.channels() + j * dst.channels() + 3];
        if (A === 0) {
          c.push(1);
        } else {
          c.push(0);
        }
      }
    }
    r.push(c);
    c = [];
  }
}

```

3. Buat fungsi ```anim()```, dan masukkan code pada langkah bagian Animate tadi ke fungsi ini, sehingga fungsi ```animate``` akan tampak menjadi seperti berikut:
```js
const anim = () => {
  let x = 0;
  let y = 0;
  
  window.requestAnimationFrame(function loop() {
    if (y <= canvas.height) {
      x += 1
      ctx.fillStyle = 'green'
      ctx.fillRect(x, y, 1, 1)
      window.requestAnimationFrame(loop)
      if (x == canvas.width) {
        x = 0
        y += 10
      }
    }
  })
}

```


## - Display The Result
Kali ini kita akan menggunakan elemen table HTML untuk menampilkan hasil konversi gamber ke matriks.
1. Tambahkan elemen parent yang akan kita gunakan sebagai tabel pada bagian akhir body sebelum ```<script></script>```
```js
<div id="table"></div>
```
2. Masukkan elemen tersebut kedalam variabel, kemudian buat elemen baru dengan javascript untuk membuat tabel. Lalu untuk isi tabel itu sendiri adalah matriks yang suda kita buat pada bagian convert image to binary
```js
let body = document.getElementById('table');
  let tbl = document.createElement('table');
  let tblBody = document.createElement('tbody');
  for(let i=0;i<dst.rows;i++){
    let tblRow = document.createElement('tr');
    for(let j=0;j<dst.cols;j++){
      let tblCol = document.createElement('td');
      let content = document.createTextNode(`${r[i][j]}`);
      tblCol.appendChild(content);
      tblRow.appendChild(tblCol);
    }
    tblBody.appendChild(tblRow);
  }
  tbl.appendChild(tblBody);
  body.appendChild(tbl);

```
3. Masukkan code di atas pada  fungsi ```convert()``` , sehingga fungsi ```convert()``` akan jadi seperti ini:
```js
const convert = () => {
  //convert img -> binary
  let src = cv.imread('canvasOutput');
  let dst = new cv.Mat.eye(src.rows, src.cols, src.type());
  cv.cvtColor(src, src, cv.COLOR_RGBA2GRAY, 0);
  cv.adaptiveThreshold(src, dst, 200, cv.ADAPTIVE_THRESH_GAUSSIAN_C, cv.THRESH_BINARY, 3, 2);
  cv.imshow('canvasBinary', dst);

  //Buat matriks pixel
  let r = [];
  for (let i = 0; i < dst.rows; i++) {
    let c = [];
    for (let j = 0; j < dst.cols; j++) {
      if (dst.isContinuous()) {
        let A = dst.data[i * dst.cols * dst.channels() + j * dst.channels() + 3];
        if (A === 0) {
          c.push(1);
        } else {
          c.push(0);
        }
      }
    }
    r.push(c);
    c = [];
  }


  //buat tabel
  let body = document.getElementById('table');
  let tbl = document.createElement('table');
  let tblBody = document.createElement('tbody');
  for(let i=0;i<dst.rows;i++){
    let tblRow = document.createElement('tr');
    for(let j=0;j<dst.cols;j++){
      let tblCol = document.createElement('td');
      let content = document.createTextNode(`${r[i][j]}`);
      tblCol.appendChild(content);
      tblRow.appendChild(tblCol);
    }
    tblBody.appendChild(tblRow);
  }
  tbl.appendChild(tblBody);
  body.appendChild(tbl);


  console.table(r);
  src.delete();
  dst.delete();
}

```

# Image 
![circle](img/circle.png)
![circlewithnoise](img/circle%20with%20noises.png)
![similarcircle](img/similar%20circles.png)
![variouscircle](img/various%20circle.png)
  
