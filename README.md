# RustDesk Server Program

[**Download**](https://github.com/rustdesk/rustdesk-server/releases)

[**Manual**](https://rustdesk.com/docs/en/self-host/) 

[**FAQ**](https://github.com/rustdesk/rustdesk/wiki/FAQ)

Self-host your own RustDesk server, it is free and open source.

```bash
cargo build --release
```

Two executables will be generated in target/release.
  - hbbs - RustDesk ID/Rendezvous server
  - hbbr - RustDesk relay server

If you wanna develop your own server, [rustdesk-server-demo](https://github.com/rustdesk/rustdesk-server-demo) might be a better and simpler start for you than this repo.

## docker-compose

If you have Docker and would like to use it, an included `docker-compose.yml` file is included. Edit line 16 to point to your relay server (the one listening on port 21117). You can also edit the volume lines (L18 and L33) if you need.

(docker-compose credit goes to @lukebarone and @QuiGonLeong)


Представляю вашему вниманию ваш будущий маленький Teamviewer. Полностью открытый, с клиентами на все платформы. Заявлено небольшое потребление серверных ресурсов. Из коробки умеет ходить через наты, как любой уважающий себя AnyDesk. Поскольку ваш сервер, скорее всего ближе к вам географически, то и картинка будет передаваться быстрее, да и зашифрован трафик будет вами же.

На сайте есть документация по развертыванию сервера. Но я хочу docker-compose и поэтому иду за ним на гитхаб и меняю в нём две строки:
command: hbbs -r rustdesk.example.com:21117
command: hbbr
на
command: hbbs -r rustdesk.mysweetdomain.org:21117 -k _
command: hbbr -k _

Я указал свой доменный адрес, причём адреса внутри сети и снаружи у домена разные. Клиенты находящиеся как в моей сети, так и в интернете где-то за натом, несмотря на это, ходят друг другу в гости без проблем. И установил ключ -k _ для того, чтобы шифрование было обязательным. Если вы не будете указывать на клиенте публичный ключ сервера, то соединение не будет зашифровано. Можно и не шифроваться, но почему бы и нет? Да и лишний никто не присосётся.

Далее взмах docker-compose и сервер уже работает и слушает локальные порты. Пробрасываем на роутере 21115,21116,21117,21118,21119 tcp и 21116 udp.

В докере, запущено два контейнера hbbs и hbbr, надо войти в оба из них и посмотреть что у них в файлах ./id_ed25519.pub и ./id_ed25519. Вам нужно сделать так, чтобы в обоих контейнерах эти пары файлов были одинаковыми, иначе вы увидите эту ошибку. Содержимое ./id_ed25519.pub - это и есть ключ. Его надо поместить вот сюда:



В виндовый клиент можно передать эти параметры назвав бинарник клиента, например вот так:
rustdesk-host=rust.mysweetdomain.org,key=wESFQlLasdasdasd5ZGkGZcsas123vasdwRFes=.exe
После этого вы сможете увидеть их в About:


Далее раздаёте всем хитро названый бинарник и пользуетесь!
Работает очень быстро. С телефона удобнее чем RDP! Всем бобра!
