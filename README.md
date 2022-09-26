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
<input type="file" id="image" />
...
<canvas id="ourCanvas" />
```
![readImage1](https://github.com/riszkyhermawan/Computer-Graphic-Course/blob/34c0ea2431d586c09ca2922b97410612c61ea717/img/WhatsApp%20Image%202022-09-25%20at%2020.13.27.jpeg)

2. Masukkan elemen Canvas dan file input ke variable di Javascript
```js
const canvas = document.getElementById("ourCanvas");
      context = canvas.getContext('2d');
      image = document.getElementById('image');
```
3. Tambahkan Event Listener 
```js
window.addEventListener('DOMContentLoaded', imageLoader);
```
4. Buat fungsi imageLoader 
```js
const imageLoader = () => {
  image.addEventListener('change', handleImage);
  
  const handleImage = (ev) => {
    const file = ev.target.files[0];
    handleFile(file);
  }
}
```
5. Buat fungsi HandleFile
```js
const handleFile = (file) => {
  const imageType = /image.*/;
  
  //cek apakah file merupakan gambar
  if(file.type.match(imageType)) {
    const reader = new FileReader();
    
    //fungsi ini akan diesekusi setelah file selesai di upload
    reader.onLoadend = function(ev) {
      const tempImageStore = new Image();
      
      tempImageStorage.onLoad = function(ev){
      
        //menyesuaikan ukuran canvas untuk gambar
        canvas.height = ev.target.height;
        canvas.width = ev.target.width;
        
        //memasukan gambar ke canvas
        context.drawImage(ev.target, 0,0);
      }
     tempImageStorage.src = event.target.result;
    }
    reader.readDataAsDataURL(file);
  }
}
```
6. Setelah gambar di-upload maka tampilannya akan menjadi seperti berikut:
<br></br>
![readImage1](https://github.com/riszkyhermawan/Computer-Graphic-Course/blob/7180b06ec3e982d2a3d391cd699d6110151cc77d/img/WhatsApp%20Image%202022-09-25%20at%2021.51.34.jpeg)


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
