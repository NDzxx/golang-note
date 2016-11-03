\#js golang双向加密解密解决方案

```js
<!doctype html>

<html lang="en">

<head>

 <meta charset="UTF-8">

 <title>需要秘钥（key）及秘钥偏移量（iv）的aes加解密</title>

</head>

<body>

 <script src="aes_1.js"></script>

 <script>

 var key = CryptoJS.enc.Utf8.parse("1234567812345678");

 var iv = CryptoJS.enc.Utf8.parse('1234567890123456');

 function Encrypt(word){

 var srcs = CryptoJS.enc.Utf8.parse(word);

 var encrypted = CryptoJS.AES.encrypt(srcs, key, { iv: iv,mode:CryptoJS.mode.CBC,padding: CryptoJS.pad.Pkcs7});

 return encrypted.toString();

 }

 function Decrypt(word){

 var decrypt = CryptoJS.AES.decrypt(word, key, { iv: iv,mode:CryptoJS.mode.CBC,padding: CryptoJS.pad.Pkcs7});

 var decryptedStr = decrypt.toString(CryptoJS.enc.Utf8);

 return decryptedStr.toString();

 }

 var mm = Encrypt('polaris@studygolang')

 console.log(mm);

 var jm = Decrypt(mm);

 console.log(jm)

 </script>

</body>

</html>

```

```go
package main



import (

"bytes"

"crypto/aes"

"crypto/cipher"

"encoding/base64"

"fmt"

)



func main() {

// AES-128。key长度：16, 24, 32 bytes 对应 AES-128, AES-192, AES-256

key := []byte("1234567812345678")

iv := []byte("1234567890123456")

str := "polaris@studygolang"

enByte, Enerr := Encrypt([]byte(str), key, iv)

if Enerr == nil {

//pjcn8w3lAHaVCT1sEaTWMrOsjTmyVt/V5Q399mL4gt4= 是base64编码的结果

fmt.Println(base64.StdEncoding.EncodeToString(enByte))

}



deByte, err := Decrypt(enByte, key, iv)

if err == nil {

fmt.Println(string(deByte))

}



}


func Encrypt(plantText []byte, key []byte, iv []byte) ([]byte, error) {

 block, err := aes.NewCipher(key)

 //选择加密算法

 if err != nil {

 return nil, err

 }

 plantText = PKCS7Padding(plantText, block.BlockSize())

 if len(plantText)%aes.BlockSize != 0 { //块大小在aes.BlockSize中定义

 panic("plantText is not a multiple of the block size")

 }

 blockModel := cipher.NewCBCEncrypter(block, iv)

 ciphertext := make([]byte, len(plantText))

 blockModel.CryptBlocks(ciphertext, plantText)

 return ciphertext, nil

}

func PKCS7Padding(ciphertext []byte, blockSize int) []byte {

 padding := blockSize - len(ciphertext)%blockSize

 padtext := bytes.Repeat([]byte{byte(padding)}, padding)

 return append(ciphertext, padtext...)

}

func Decrypt(ciphertext []byte, key []byte, iv []byte) ([]byte, error) {

 block, err := aes.NewCipher(key)

 //选择加密算法

 if err != nil {

 return nil, err

 }

 blockModel := cipher.NewCBCDecrypter(block, iv)

 plantText := make([]byte, len(ciphertext))

 blockModel.CryptBlocks(plantText, ciphertext)

 plantText = PKCS7UnPadding(plantText, block.BlockSize())

 return plantText, nil

}

func PKCS7UnPadding(plantText []byte, blockSize int) []byte {

 length := len(plantText)

 unpadding := int(plantText[length-1])

 return plantText[:(length - unpadding)]

}




```

