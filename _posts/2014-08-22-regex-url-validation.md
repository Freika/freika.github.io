---
layout: post
title: Regex для валидации URL конкретного сайта
---


Ожидаемо возня с регулярными выражениями отняла кучу времени, но результат был достигнут.

~~~ bash
((http?|https?):\/\/)?(www.)?behance.net([\/\w \.-]*)*\/?
~~~

Валидные урлы:

- www.behance.net
- https://www.behance.net
- http://behance.net

а так же все перечисленные выше + латиница вида

http://behance.net/Username

Пара полезных ссылок:

[code.tutsplus.com](http://code.tutsplus.com/tutorials/8-regular-expressions-you-should-know--net-6149)

[regexr.com](http://regexr.com/)