\#js golang双向加密解密解决方案



&lt;!doctype html&gt;

&lt;html lang="en"&gt;

&lt;head&gt;

 &lt;meta charset="UTF-8"&gt;

 &lt;title&gt;需要秘钥（key）及秘钥偏移量（iv）的aes加解密&lt;\/title&gt;

&lt;\/head&gt;

&lt;body&gt;

 &lt;script src="aes\_1.js"&gt;&lt;\/script&gt;

 &lt;script&gt;

 var key = CryptoJS.enc.Utf8.parse\("1234567812345678"\);

 var iv = CryptoJS.enc.Utf8.parse\('1234567890123456'\);

 function Encrypt\(word\){

 var srcs = CryptoJS.enc.Utf8.parse\(word\);

 var encrypted = CryptoJS.AES.encrypt\(srcs, key, { iv: iv,mode:CryptoJS.mode.CBC,padding: CryptoJS.pad.Pkcs7}\);

 return encrypted.toString\(\);

 }

 function Decrypt\(word\){

 var decrypt = CryptoJS.AES.decrypt\(word, key, { iv: iv,mode:CryptoJS.mode.CBC,padding: CryptoJS.pad.Pkcs7}\);

 var decryptedStr = decrypt.toString\(CryptoJS.enc.Utf8\);

 return decryptedStr.toString\(\);

 }

 var mm = Encrypt\('polaris@studygolang'\)

 console.log\(mm\);

 var jm = Decrypt\(mm\);

 console.log\(jm\)

 &lt;\/script&gt;

&lt;\/body&gt;

&lt;\/html&gt;

