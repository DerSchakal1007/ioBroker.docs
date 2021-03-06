---
title: ioBroker перестал работать
lastChanged: 06.06.2019
translatedFrom: de
translatedWarning: Если вы хотите отредактировать этот документ, удалите поле «translationFrom», в противном случае этот документ будет снова автоматически переведен
editLink: https://github.com/ioBroker/ioBroker.docs/edit/master/docs/ru/trouble/RunsNoMore.md
hash: UsV8EdQdT5BLeSXWa+Fxes2caK4VGm5HBv3MblYRwjI=
---
# IoBroker перестал работать
На форуме часто случается, что ioBroker больше не работает. Но это утверждение, которое несет столько информации, сколько: моя машина не ездит.

Никто не думает, что может быть 1000 причин, по которым автомобиль не ездит: нет топлива, аккумулятор разряжен, шины разряжены и, и, и ...

ioBroker очень модульный, и вы можете легко отремонтировать любую деталь. Файлы конфигурации были удалены из каталога с пакетами Node.js, и пока этот каталог конфигурации еще завершен, с установкой ioBroker ничего серьезного не произошло.

Первое, что вы заметите, это то, что ioBroker не будет работать, если «admin» не запущен. Но есть более-менее понятный алгоритм проверки того, что сломано.
Проверьте, работает ли контроллер js:

** Linux: **

````
Linux: ps -A | grep iobroker
````

** Окна: **

Проверьте, есть ли процесс node.exe в Process Explorer (показать все процессы)

Что-то должно быть видно под Linux:

```
pi@pi:~$ ps -A | grep iobroker
1807 ? 13:59:22 iobroker.js-con
```

Если это не работает, попробуйте ioBroker для начала

** Linux: **

```
cd /opt/iobroker
iobroker start
```

или **Windows:**

```
cd C:\ioBroker
iobroker start
```

Если он все еще не запущен или имеются сообщения об ошибках, попробуйте запустить контроллер js вручную.

```
cd /opt/iobroker
node node_modules/iobroker.js-controller/controller.js --logs
```

Если есть какие-либо сообщения об ошибках, вы можете попробовать обновить «js-controller».

Когда контроллер js работает, порты TCP 9000 и 9001 должны быть заняты. Вы можете проверить это с помощью команды:

```
netstat -n -a -p TCP
```

Следующие строки должны быть видны:

```
TCP 0.0.0.0:9000 0.0.0.0:0 LISTENING
TCP 0.0.0.0:9001 0.0.0.0:0 LISTENING
```

При использовании Redis это должно выглядеть так:

```
TCP 0.0.0.0:6379 0.0.0.0:0 LISTENING
TCP 0.0.0.0:9001 0.0.0.0:0 LISTENING
```

Если ничего не видно (или только один), то, вероятно, порты заняты другими программами. Вы можете изменить порты в */ opt / iobroker / iobroker-data / iobroker.json* Или перенастроить другую программу.

## Переустановите адаптер или контроллер js
Если адаптер или контроллер js работал и не работал после обновления, то, скорее всего, что-то пошло не так с обновлением. Но вы можете очень легко установить адаптер снова. Все, что вам нужно сделать, это написать в консоли:

```
cd /opt/iobroker
iobroker stop adapterName
npm install iobroker.adapterName
iobroker upload adapterName
iobroker start adapterName
```

Или для контроллера JS:

```
cd /opt/iobroker
iobroker stop
npm install iobroker.js-controller
iobroker start
```

## Проверьте или правильно установлены node.js и npm
Если js-controller не запущен, возможно, что node.js вообще не установлен.
Рекомендуется использовать версию 8.x файла node.js.

Версия 10.x Node.js была в значительной степени протестирована (по состоянию на 6 мая 2019 года), и 12.x не дает никаких гарантий, что она будет работать.

Команды

```
node -v
npm -v
```

должен показать тот же номер версии. Если это не так, вы должны удалить и переустановить node.js. Или проверьте путь поиска.

Node.js удаляется и устанавливается так же, как и для ручной установки ioBroker (для Raspberry и других систем Linux).

Необходимые шаги подробно описаны ЗДЕСЬ.

И здесь вы можете найти информацию о других системах ..
Проверьте, работает ли адаптер администратора

Сначала проверьте, активирован ли админ:

```
cd /opt/iobroker
iobroker list instances
```

должна быть строка, подобная этой:

```
system.adapter.admin.0 : admin - enabled, port: 8081, bind: 0.0.0.0, run as: admin
```

Если вместо «включено» есть «отключено», вы можете активировать адаптер следующим образом:

```
iobroker start admin
```

Если IP-адрес не правильный, напишите:

```
iobroker set admin.0 --bind 0.0.0.0
```

разрешить на всех IP-адресах.

Вы также можете изменить порт:

```
iobroker set admin.0 --port 8081
```

или отключите SSL:

```
iobroker set admin.0 --secure false
```

Тогда экземпляр в порту (по умолчанию 8081) должен быть видимым.

С участием

```
netstat -n -a -p TCP
```

Вы можете проверить, найдена ли строка:

```
TCP 0.0.0.0:8081 0.0.0.0:0 LISTENING
```

Если он все еще не запущен, вы можете запустить его вручную и посмотреть, есть ли какие-либо ошибки: cd / opt / iobroker node node_modules / iobroker.admin / admin.js --logs

Там также может быть что-то в журнале. Файл журнала можно найти в папке ***/ opt / iobroker / log / iobroker.JJJJ-MM-DD.log*** .

Вы можете с командой

```
cd /opt/iobroker
cat log/iobroker.JJJJ-MM-TT.log
```

просмотреть файл. Конечно, YYYY-MM-DD необходимо заменить на текущую дату. («Cat» возможен только под Linux)

## Установить еще один экземпляр администратора
Если настройки изменяются консолью администратора, и вы больше не можете получить доступ к странице администратора, есть возможность установить второй экземпляр администратора.

Следовательно:

```
iobroker add admin --port 8089
```

Бежать.

Здесь 8089 - это, безусловно, бесплатный порт. Затем вы можете связаться с администратором по адресу http:// ip: 8089.

После того, как настройки снова в порядке, вы должны удалить новый (второй на порту 8089) экземпляр для экономии ресурсов.

## Npm исчез
>! Нечто подобное происходит в Debian (Raspbian) Buster

Из-за проблемы с npm может случиться так, что после обновления с Linux, в котором nodejs обычно также обновляется в версии скина (6.x; 8.x, 10.x), ничего не работает внезапно.

Например, Адаптер больше не установлен, сообщение об ошибке ***npm не найден***

В таких случаях, пожалуйста, проверьте в консоли:

узел -v npm -v

Обычно (по состоянию на 30 июля 2019 г.) версия узла 8.15.0 и npm не обнаруживаются.

Обычная процедура обновления npm не работает, потому что npm там нет. Поэтому вы должны сначала удалить узел, а затем переустановить его:

```
sudo apt-get --purge remove node
sudo apt-get --purge remove nodejs
sudo apt-get autoremove
curl -sL https://deb.nodesource.com/setup_10.x | sudo -E bash -
sudo apt-get install -y nodejs
node -v
npm -v
```

Теперь обычно нужно установить npm 6.x.

Если ранее была установлена другая основная версия (не 10.x) Node, пакеты должны быть скомпилированы на узле 10

```
cd /opt/iobroker
npm rebuild
```