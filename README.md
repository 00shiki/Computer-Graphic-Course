# Assigment 1
## Technology Director : Riszky Hermawan
## Other team member:
- Solver : Aditya Firmansyah Soedira
- Programmer : Ahmad Luhur Pakerti


### Programming Language : Javascript
Pada tugas kali ini kami akan menggunakan bahasa pemrograman Javascript dengan HTML dan CSS. Lebih spesifiknya kita akan menggunakan fitur Canvas pada Javascript.


## Documentation
### - Read Image with Js
1. Buat elemen input file dan elemen Canvas pada HTML dengan menggunakan Id yang akan digunakan pada script (Javascript)
contoh :
```HTML
<input type="file" id="uploaded-file" />
...
<canvas id="ourCanvas" width="500" height="300" style="border:1px solid #000000;"></canvas>
```
![readImage1](https://github.com/riszkyhermawan/Computer-Graphic-Course/blob/d0405eacae8ef8be0645d70e134bf1360e255f8a/img/Screenshot%20(576).png)

2. Masukkan elemen Canvas dan file input ke variable di Javascript
```js
var canvas = document.getElementById("ourCanvas"),
  context = canvas.getContext('2d'),
  uploadedFile = document.getElementById('uploaded-file');

```
3. Tambahkan Event Listener 
```js
window.addEventListener('DOMContentLoaded', initImageLoader);
```
4. Buat fungsi imageLoader 
```js
function initImageLoader() {
  uploadedFile.addEventListener('change', handleManualUploadedFiles);
  function handleManualUploadedFiles(ev) {
    var file = ev.target.files[0];
    handleFile(file);
  }

}
```
5. Buat fungsi HandleFile untuk menampilkan gambar pada canvas
```js
async function handleFile(file) {
  const bmp = await createImageBitmap(file); 
  canvas.width = bmp.width; 
  canvas.height = bmp.height; 
  context.drawImage(bmp, 0, 0);
}

```
6. Setelah gambar di-upload maka tampilannya akan menjadi seperti berikut:
<br></br>
![readImage1](https://github.com/riszkyhermawan/Computer-Graphic-Course/blob/d0405eacae8ef8be0645d70e134bf1360e255f8a/img/Screenshot%20(577).png)


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

