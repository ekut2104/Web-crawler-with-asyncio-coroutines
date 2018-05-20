# Веб бот, использующий сопрограммы asyncio

[Часть 1](https://github.com/igorzakhar/Web-crawler-with-asyncio-coroutines/blob/master/Part_1.md) / 
[Часть 2](https://github.com/igorzakhar/Web-crawler-with-asyncio-coroutines/blob/master/Part_2.md) / 
[Часть 3](https://github.com/igorzakhar/Web-crawler-with-asyncio-coroutines/blob/master/Part_3.md) 
---

Предварительная публикация главы из сборника "500 строк или меньше", новой книги серии [«The Architecture of Open Source Applications»](http://rus-linux.net/MyLDP/BOOKS/Architecture-Open-Source-Applications/index.html)  

Оригинал: ["A Web Crawler With asyncio Coroutines"](http://aosabook.org/en/500L/a-web-crawler-with-asyncio-coroutines.html)  
Автор: A. Jesse Jiryu Davis and Guido van Rossum  
Дата публикации: 15 September 2015  
Перевод: Н.Ромоданов  
Дата перевода: январь 2016 г.  

Глава 3 из предварительной версии книги "500 Lines or Less", которая входит в серию ["Архитектура приложений с открытым исходным кодом"](http://rus-linux.net/MyLDP/BOOKS/Architecture-Open-Source-Applications/index.html), том 4.  

#### Creative Commons  

Перевод был сделан в соответствие с лицензией [Creative Commons](http://creativecommons.org/licenses/by/3.0/legalcode). С русским вариантом лицензии можно ознакомиться [здесь](http://wiki.creativecommons.org/images/0/03/Attribution_3.0_%D0%A1%D0%A1_BY_rus.pdf).  

Это предварительная публикация главы из сборника [«500 строк или меньше»](https://github.com/aosabook/500lines/blob/master/README.md), четвертой книги из серии книг [Архитектура приложений с открытым исходным кодом](http://aosabook.org/). Пожалуйста, сообщайте о любых проблемах, которые вы обнаружите при чтении этой главы, в нашем треккере GitHub. Следите за объявлениями о предварительных публикациях новых глав и окончательной публикацией в блоге [AOSA](http://aosabook.org/blog/) или в [Твиттере](https://twitter.com/aosabook).  

Сегодня мы представляем третью главу предварительной публикации нашего нового сборника «500 строк или меньше». Глава была написана А. Джесси Джури Дэвис ([A. Jesse Jiryu Davis](https://twitter.com/jessejiryudavis)) и Гвидо ван Россумом ([Guido van Rossum](https://twitter.com/gvanrossum)).  

Мы надеемся, что эта глава поможет программистам, использующим язык Python, подробно разобраться с тем, как работают сопрограммы, а также почему и когда нам следует ими пользоваться. С того момента, когда был выпущен фреймворк asyncio, сопрограммы стали горячей темой в языке Python; сейчас, в версии Python 3.5, они уже встроены непосредственно в сам язык.  

Если не ограничиваться языком Python, то в области компьютерных наук есть давнее противостояние между системами, использующими потоки , и системами, использующими события. Практикующему программисту полезно понимать нюансы этой дискуссии, а те, кто пользуется фреймворками, в основе которых лежат эти подходы, несомненно должны в них разбираться.  

В сборнике «500 строк или меньше» у нас будут главы, рассказывающие о создании систем, ориентированных на использовании потоков, и систем, ориентированных на использовании событий, но мы надеемся, что в окончательном варианте сборника данная глава будет хорошо выглядеть на фоне этих глав.  

Если вы обнаружите ошибки, о которых, как вы считаете, стоит сообщить, пожалуйста, сделайте запись на нашем [треккере GitHub](https://github.com/aosabook/500lines/issues).  

Приятного чтения!  

_А. Джесси Джури Дэвис работает инженером в MongoDB в Нью-Йорке. Он написал Motor, асинхронный драйвер для языка Python в MongoDB, и он является ведущим разработчиком драйвера для C в MongoDB C и входит в состав команды, работающей над PyMongo. К его разработкам относятся asyncio и Торнадо. Он пишет на сайте [http://emptysqua.re](http://emptysqua.re/)_.

_Гвидо ван Россум является создателем языка Python, одного из основных языков программирования, используемого как в сети, так и вне сети. Сообщество Python называет его великодушным диктатором (BDFL - Benevolent Dictator For Life), титулом, взятом непосредственно из пародии Монти Пайтона. Домашекй страничкой Гвидо в сети является [http://www.python.org/~guido/](http://www.python.org/~guido/)._

---
[Часть 1](https://github.com/igorzakhar/Web-crawler-with-asyncio-coroutines/blob/master/Part_1.md) / 
[Часть 2](https://github.com/igorzakhar/Web-crawler-with-asyncio-coroutines/blob/master/Part_2.md) / 
[Часть 3](https://github.com/igorzakhar/Web-crawler-with-asyncio-coroutines/blob/master/Part_3.md) 
---
[Source code:](https://github.com/aosabook/500lines/tree/master/crawler)