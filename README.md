# Callback
(Call me back) 
- callback = high other function
- callback = di eksekusi di function lain melalaui parameter


## 1. Callback on Synchronous
### Callback sebagai injeksi sebuah function
```
ffunction uangSakuInRupiah(fromDad, fromMom){
  return fromDad+fromMom+" rupiah"
} 
function calculate(param1, param2, callbackAsParam) {
  var result = param1 + param2 ;

  // check callback is function
  if(typeof callbackAsParam == 'function'){
    return callbackAsParam(param1, param2);
  }

  return result
}

// execute
var a = calculate(100, 400, uangSakuInRupiah);
var b = calculate(300, 200, function(fromDad, fromMom){ return "$"+ (fromDad+fromMom);});
var c = calculate(200, 300, 0);

console.log(a)
console.log(b)
console.log(c)

```

### callback sebagai event listener (on web API)
```
$('#my_button').on('click', function(e) {
  console.log('click id my_button');
})
```


## 2. Callback on Asynchronous
#### Case
```
function a(){
  console.log('a done');
}

function b(){
  setTimeout(function(){
    console.log('b done');
  }, 5000);
}

function c() {
  console.log('c done');
}
a();
b();
c();

```
#### solution
```
function a(){
  console.log('a done');
}

function b(callback) {
  setTimeout(function(){
    console.log('b done')
    callback();
  }, 5000)
}

function c(){
  console.log('c done')
}
a()
b(c)
```
### ajax proses
```
function requestAjax(callback){

  // inisialisasi library XML Http Request
  var xhr = new XMLHttpRequest();

  // set target request
  xhr.open('GET','https://jsonplaceholder.typicode.com/users/1')

  // terapkan callback
  xhr.onload = function(){
    if(xhr.status === 200){
      callback(xhr.responseText)
    }else{
      callback('Failed')
    }
  }

  // mulai request
  xhr.send()
  
}

function showResult(data){
  if (data != 'Failed'){
    //tampilkan Data
    data=JSON.parse(data)
    console.log(data)
  }
}

requestAjax(showResult)
```
### Operasi File
Operasi file pada nodejs support synchronous dan asynchronous

#### sample read file synchronous
```
const fs = require('fs');
var data = fs.readFileSync('hello.md')
console.log('Read File Done :' + data.toString());
```
---
**NOTE**
- Hasil eksekusi code diatas akan membloking keseluruhan prosess hingga proses baca file selesai. 
- Kelemahannya tentu aplikasi akan berhenti sampai proses tersebut selesai 
- ditambah lagi tidak ada manajemen error.
---

#### sample read file asynchronous
```
function readFileCallback(err,data){
  if (err){
    console.log('Error Read File :' + err);
  }else{
    console.log(data.toString())
  }
}
var data = fs.readFile('hello.md',readFileCallback)
```


## 3. Callback Hell
- ketika membuat beberapa callback bercabang
- callback di dalam callback

```
var a = readFileContent("a.md");
var b = readFileContent("b.md");
var c = readFileContent("c.md");
writeFileContent("result.md", a + b + c);
console.log("we are done");
```

```
readFileContent("a.md", function (a){
  readFileContent("b.md", function (b){
    readFileContent("b.md", function (b){
        writeFileContent("result.md", a + b + c, function(){
            console.log("we are done");
        })
      })   
  })
})
```