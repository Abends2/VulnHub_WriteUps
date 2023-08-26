# VulnHub: Noob

Сканируем машину на наличие открытых портов при помощи Nmap:
```sh
nmap -sC -sV 192.168.0.22
```
![ScreenShot](screenshots/1.png)

Мы нашли:
- 21 port - FTP (vsftpd 3.0.3)
- 80 port - HTTP (Apache httpd 2.4.29)

При переходе на веб-ресурс попадаем на страницу логина:

![ScreenShot](screenshots/2.png)

Возвращаемся к FTP-серверу, куда входим под аккаунтом **anonymous**. Сразу копируем все файлы:
 
![ScreenShot](screenshots/3.png)

Файл **welcome**:

![ScreenShot](screenshots/4.png)

Файл **cred.txt**:

![ScreenShot](screenshots/5.png)

Декодируем содержимое файла из base64:

![ScreenShot](screenshots/6.png)

Получаем данные (**champ:password**) для входа на веб-ресурс:

![ScreenShot](screenshots/7.png)

При нажатии на **About Us** происходит скачивание архива **downloads.rar**:

![ScreenShot](screenshots/8.png)

![ScreenShot](screenshots/9.png)

![ScreenShot](screenshots/10.png)

В файле **sudo** намек на его же название. Скорее всего, информация скрыта внутри картинок, которые находятся внутри архива. Через **steghide** проверим это:

![ScreenShot](screenshots/11.png)

![ScreenShot](screenshots/12.png)

Примечательно, что данный пароль (**sudo**) применяется только к одному из файлов (с расширением **bmp**). Извлекаем данные:

![ScreenShot](screenshots/13.png)

В файле **hint.py** нам говорят, что мы ходим вокруг да около. Декодируем содержимое **user.txt** с помощью ROT-13

![ScreenShot](screenshots/14.png)

![ScreenShot](screenshots/15.png)

![ScreenShot](screenshots/16.png)

На всякий случай маломальски проверяем файл **hint.py**:

![ScreenShot](screenshots/17.png)

Декодированная фраза похожа на данные для входа (логин: wtf, пароль: this one is a simple one). Тут вспоминаем, что в самом начале мы не обнаружили порт для SSH. Исправляемся:

![ScreenShot](screenshots/18.png)

![ScreenShot](screenshots/19.png)

Порт нашли, пробуем подключиться:

![ScreenShot](screenshots/20.png)

У нас это успешно получается. Забираем первый флаг:

![ScreenShot](screenshots/21.png)

![ScreenShot](screenshots/22.png)

Проверяем, что мы можем исполнять от лица sudo:

![ScreenShot](screenshots/23.png)

В итоге мы имеем полный доступ к sudo, поэтому через, например, `sudo -i` получаем привилегированный доступ и забираем второй флаг.
