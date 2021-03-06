---
layout: post
title: Обработка тяжелого json-файла, получаемого с удаленного сервера
---


В каких случаях может понадобиться обработать тяжелый json? Как правило, это получение кучи данных с удаленного API, как и получилось у меня. Несмотря на то, что апишный джейсон время от времени обновляется  и меняет свое содержимое, каждый раз при необходимости обработать его содержимое перекачивать 15-30 мегабайт попросту неразумно. Поэтому возникла идея каким-либо образом "закешировать" полученный джейсон и обновлять его только тогда, когда это реально необходимо. У меня необходимость определяется просто: сменилась временная метка файла, значит надо обновлять.

Задача простая, но было несколько вариантов её решения. Я остановился на самом простом и незамысловатом.

Во-первых, мы тупо скачиваем json-файл. 

~~~ruby
def self.download_json(url_to_json)
  url.slice! "http://remote.host.com"
  Net::HTTP.start("remote.host.com") do |http|
    resp = http.get(url)
    open("public/temp/remote_api.json", "wb") do |file|
      file.write(resp.body)
    end
  end
end
~~~

Во-вторых, мы его обрабатываем по своему усмотрению.

В третьих не будет, потому что больше для решения этой задачи ничего не нужно, знай себе обновляй джейсон и обрабатывай его как тебе нужно :)