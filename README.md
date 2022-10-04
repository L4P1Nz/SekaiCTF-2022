# **SekaiCTF 2022**
## **Sekai Game Start** 
___
### **Description:**
Hey it's our Sekai Game – try to make it start!!

Author: bwjy

```http
http://sekai-game-start.ctf.sekai.team
```
___


![1.png](https://github.com/L4P1Nz/SekaiCTF-2022/blob/main/Media/1.png)

Bài này có 2 vấn đề mà ta phải giải quyết

1. Exploit unserialize in php

Search google "bypass __wakeup in php" ta sẽ có được một vài resources để exploit

Dựa vào response từ server, ta biết được server chạy php version 7.4.5

![1.png](https://github.com/L4P1Nz/SekaiCTF-2022/blob/main/Media/2.png)

Sau một hồi focus vào [issues](https://github.com/php/php-src/issues/9618) này không thành công thì mình bắt đầu thử một [bug](https://bugs.php.net/bug.php?id=81151) từ năm 2021

Và có vẻ như mình đã đi đúng hướng :)

![3.png](https://github.com/L4P1Nz/SekaiCTF-2022/blob/main/Media/3.png)

2. Replace dot

![4.png](https://github.com/L4P1Nz/SekaiCTF-2022/blob/main/Media/4.png)

Để ý rằng ta không để truyền được giá trị cho $_GET[sekai_game.run] bởi [*tính năng*](https://www.php.net/manual/en/language.variables.external.php#language.variables.external.dot-in-names) của PHP.

Nom na thì khi ta gửi request ```/?sekai_game.run=test``` thì PHP sẽ hiểu thành ```$_GET[sekai_game_run]=test```. 

![5.png](https://github.com/L4P1Nz/SekaiCTF-2022/blob/main/Media/5.png)

Tương tự ```sekai.game run``` đều sẽ thành ```sekai_game_run```

Sau một hồi mình test trên máy local các trường hợp thì mình nhận ra rằng: sau khi PHP replace "```[```" thì nó sẽ ignore các ký tự phía sau

Kết lại:

```
http://sekai-game-start.ctf.sekai.team/?sekai[game.run=C:10:%22Sekai_Game%22:0:{}
```



![6.png](https://github.com/L4P1Nz/SekaiCTF-2022/blob/main/Media/6.png)
