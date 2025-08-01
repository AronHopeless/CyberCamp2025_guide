# Всегда найдется рыба покрупнее

### Условие инцидента
![Инцидент](/imgs/Incident.png)
![Задача](/imgs/Pasted%20image%2020250719092336.png)

### Флаги для поиска 
![Флаги](/imgs/Pasted%20image%2020250719093700.png)

> [!WARNING]
> *SOLUTION ZONE*  
> *BEWARE OF SPOILERS* 

### Решение
#### Флаг 1
В первую очередь перейдём по предложенной ссылке:
```
https://willy-the-cat.github.io/nft_cats/
```
<p align="center">
  <img src="https://github.com/AronHopeless/CyberCamp2025_guide/blob/main/imgs/Pasted%20image%2020250719092406_2.png">
</p>

Видим, что страницы уже не существует -> необходимо просмотреть, что было на ней раньше  
Для этого перейдём на https://archive.org  
В строке поиска выбираем 'искать сайты', вставляем наш адрес

![Архив_поиск](/imgs/Pasted%20image%2020250719092528.png)

Существует одна версия сайта от 26го июня:

![Архив_поиск](/imgs/Pasted%20image%2020250719092642_2.png)

Перейдём на неё. Имеем страницу "магазина"
```
https://web.archive.org/web/20250626132226/https://willy-the-cat.github.io/nft_cats/
```
![Архив_поиск](/imgs/Pasted%20image%2020250719092741.png)

Поле "Кошелёк" не прогружается; кнопки в снимке страницы, разумеется, не кликабельны.  
Просмотрим содержимое снимка страницы.  

![Архив_поиск](/imgs/Pasted%20image%2020250719092940.png)

В информации мы видим подгружаемые png-картинки, css-стиль и JS-скрипт с названием "payment". (На аналитику гугл не обращаем внимания)  
Рассмотрим платёжный скрипт подробнее.
```js
var _____WB$wombat$assign$function_____ = function(name) {return (self._wb_wombat && self._wb_wombat.local_init && self._wb_wombat.local_init(name)) || self[name]; };
if (!self.__WB_pmw) { self.__WB_pmw = function(obj) { this.__WB_source = obj; return this; } }
{
  let window = _____WB$wombat$assign$function_____("window");
  let self = _____WB$wombat$assign$function_____("self");
  let document = _____WB$wombat$assign$function_____("document");
  let location = _____WB$wombat$assign$function_____("location");
  let top = _____WB$wombat$assign$function_____("top");
  let parent = _____WB$wombat$assign$function_____("parent");
  let frames = _____WB$wombat$assign$function_____("frames");
  let opener = _____WB$wombat$assign$function_____("opener");

(function () {
  const obfuscatedData = "VSBSIGNsb3NlLCBsb29rIGJldHRlciA6KQ==";
  const wallet = atob(obfuscatedData);

  const uselessData = atob("YmMxcWo1dno2c2M3bjkweWxxMzkzaDlraHNhOGVzc3J5eWxqNjVmdmRt");

  document.addEventListener("DOMContentLoaded", () => {
    document.getElementById("wallet-address").innerText = wallet;
  });Add commentMore actions
})();

}
/*
     FILE ARCHIVED ON 13:22:26 Jun 26, 2025 AND RETRIEVED FROM THE
     INTERNET ARCHIVE ON 06:26:51 Jul 19, 2025.
     JAVASCRIPT APPENDED BY WAYBACK MACHINE, COPYRIGHT INTERNET ARCHIVE.

     ALL OTHER CONTENT MAY ALSO BE PROTECTED BY COPYRIGHT (17 U.S.C.
     SECTION 108(a)(3)).
*/
/*
playback timings (ms):
  captures_list: 0.647
  exclusion.robots: 0.026
  exclusion.robots.policy: 0.012
  esindex: 0.012
  cdx.remote: 30.477
  LoadShardBlock: 149.965 (3)
  PetaboxLoader3.datanode: 146.68 (4)
  PetaboxLoader3.resolve: 201.51 (2)
  load_resource: 264.74
*/
```

Из потенциально интересных строк имеем:
```
const obfuscatedData = "VSBSIGNsb3NlLCBsb29rIGJldHRlciA6KQ=="
```
и 
```
const uselessData = atob("YmMxcWo1dno2c2M3bjkweWxxMzkzaDlraHNhOGVzc3J5eWxqNjVmdmRt")
```

Данные строки выглядят абсолютно случайными, однако стоит их проверить в CyberChef

В первом случае строка является неинформативной, просто сообщает нам, что мы близки к ответу:

![Архив_поиск](/imgs/Pasted%20image%2020250719093504.png)

Во втором случае расшифрованная строка выглядит следующим образом:

![Архив_поиск](/imgs/Pasted%20image%2020250719093559.png)

```
bc1qj5vz6sc7n90ylq393h9khsa8essryylj65fvdm
```
Спросив ChatGPT, что это за строка, узнаём, что "она похожа на **адрес Bitcoin-кошелька**, в формате **Bech32** (начинается с `bc1`)."  
Данная строка является [первым флагом](#Флаги-для-поиска) и подходит под его формат.

#### Флаг 3
Попробуем посмотреть информацию о Bitcoin-кошельке. Воспользуемся https://blockchair.com/  

![Архив_поиск](/imgs/Pasted%20image%2020250719095603.png)

Последняя транзакция была произведена 26го июня, тогда же, когда и существовал сайт. Однако датируется 2022 годом, вместо 2025.

![Архив_поиск](/imgs/Pasted%20image%2020250719095706.png)

Обратимся к полной истории транзакций

![Архив_поиск](/imgs/Pasted%20image%2020250719100101.png)

Вторая транзакция соответствует половине третьего флага. Осталось узнать используемую биржу.

Просмотрим полную информацию о транзакции
```
e28852904fe51ef17502f083e70b9b6ce08d173acc3a51a40135798f6418adf7
```
![Архив_поиск](/imgs/Pasted%20image%2020250719101232.png)

У неё имеется один INPUT и 28 OUTPUT, то есть осуществлялась рассылка денежных средств по нескольким адресам.  
Искомый адрес присутствует в списке OUTPUT под 4 номером:

![Архив_поиск](/imgs/Pasted%20image%2020250719101356.png)

На момент перевода сумма составляла лишь 1,108 долларов.

В поисках биржи просмотрим Bitcoin кошелёк INPUT
```
bc1qm34lsc65zpw79lxes69zkqmk6ee3ewf0j77s3h
```

Судя по балансу кошелька, он действительно может принадлежать бирже:

![Архив_поиск](/imgs/Pasted%20image%2020250719101719.png)

Поиск на специализированных сайтах не даёт результата, кошелёк не содержит Label ни одной из известных бирж. 
Продолжим поиск в Google.
Уже на этапе поискового запроса имеем некоторые подсказки:

![Архив_поиск](/imgs/Pasted%20image%2020250719102551.png)
Вероятно, это вызвано популярностью задания в текущий момент времени, поэтому воспользуемся запросом
```
bc1qm34lsc65zpw79lxes69zkqmk6ee3ewf0j77s3h exchange
```
и первым же поисковым результатом:
![Архив_поиск](/imgs/Pasted%20image%2020250719102757.png)
По информации с сайта, кошелёк принадлежит криптобирже Binance:
![Архив_поиск](/imgs/Pasted%20image%2020250719102853.png)

Таким образом, [третий флаг](#Флаги-для-поиска) может выглядеть следующим образом:
`BINANCE_4000`

#### Флаг 2
Для поиска ответа на [второй флаг](#Флаги-для-поиска) осуществим поиск по адресу кошелька на специализированных платформах.  
В данном случае используем https://chainabuse.com/
```
https://chainabuse.com/address/bc1qj5vz6sc7n90ylq393h9khsa8essryylj65fvdm
```
По данным ChainAbuse данный кошелёк замечен в двух Scam report-ах:

![Архив_поиск](/imgs/Pasted%20image%2020250719104301.png)

Один из них, подтверждённый Ransomware, имеет метку `DeadBolt Ransomware`  
Убедимся, чем является DeadBolt:

![Архив_поиск](/imgs/Pasted%20image%2020250719125420.png)

Это действительно ВПО для шифрования данных, и, соответственно, оно является вымогательским.  
Также рабочим поисковым запросом является следующий:
```
"bc1qj5vz6sc7n90ylq393h9khsa8essryylj65fvdm" ransomware
```
Первый поисковый результат по данному запросу ведёт на блог, посвящённый шифровальщику.
На этой же странице можно найти подтверждение ранее найденной информации: сообщение от вероятного автора перевода денежных средств, выполненного для расшифровывания данных по требованию злоумышленников.  

![Архив_поиск](/imgs/Pasted%20image%2020250719110015.png)

[Второй флаг](#Флаги-для-поиска) - `DEADBOLT`

#### Флаг 4
Для поиска четвёртого флага, как и было сказано в задании, воспользуемся [urlscan.io](https://urlscan.io/) и просканируем наш сайт

![Архив_поиск](/imgs/Pasted%20image%2020250719110512.png)

Данное сканирование не является сильно информативным, так как сайт на данный момент не существует.  
Также видим, сайт ранее был просканирован 38 раз, перейдём к списку всех сканирований:

![Архив_поиск](/imgs/Pasted%20image%2020250719111212.png)

Здесь нас интересуют самые ранние сканы (23 дня назад). 
Судя по количеству данных, тогда сайт ещё существовал.

![Архив_поиск](/imgs/Pasted%20image%2020250719111307.png)

Прокликав весь сайт в надежде на какую-то зацепку, перейдём в раздел `HTTP transactions`
```
https://urlscan.io/result/0197ac69-b130-77f8-b991-ba7b71b19c19/#transactions
```

![Архив_поиск](/imgs/Pasted%20image%2020250719132259.png)

Как можно видеть, .png картинки для сайта подгружаются через `raw.githubusercontent.com`  
Потенциально это может быть репозиторием-источником, который используется злоумышленником и до сих пор

Перейдём к поиску по названию картинки.  
Наиболее нестандартное имя у самой первой, хотя с остальными всё работает точно также

![Архив_поиск](/imgs/Pasted%20image%2020250719132812.png)

В поисковых результатах видим сайт, на который данная картинка была загружена совсем недавно, вероятно, сайт ещё активен


![Архив_поиск](/imgs/Pasted%20image%2020250719132922.png)

Перейдя на сайт или просмотрев [информацию](https://urlscan.io/result/019820d9-6d2e-76d0-9c4c-016493d35805/) о нём на urlscan, понимаем, что это сайт схожий с тем, что был использован в инциденте мошенничества.
```
http://shop.nft-kitty.online/
```
![Архив_поиск](/imgs/Pasted%20image%2020250719133128.png)

Данный сайт активен и подходит под маску флага - а значит и является искомым  
[Четвёртый флаг](#Флаги-для-поиска) - `shop.nft-kitty.online`

#### Флаг 5
Для работы с фавиконом попробуем извлечь его с сайта и скачать

*1 способ* - воспользуемся онлайн инструментами.  
По запросу в Google `get site favicon` найдём подходящий ресурс.  
Первым поисковым результатом является https://webutility.io/favicon-extractor, однако он не справился с задачей.  
Поэтому был использован инструмент на втором месте поискового результата - https://folge.me/tools/favicon-downloader

После вставки URL сайта, инструмент успешно нашёл фавикон и предлагает скачать:
<img src="https://github.com/AronHopeless/CyberCamp2025_guide/blob/main/imgs/Pasted%20image%2020250719135944.png" width = "750">

Переход по ссылке `Download` приводит нас на следующую страницу:
```
https://raw.githubusercontent.com/junior03986721/compot-nft/main/bmZ0X2tpdHR5X3VubG9jaw==.png
```

<img src="https://github.com/AronHopeless/CyberCamp2025_guide/blob/main/imgs/Pasted%20image%2020250719140030_2.png" width = "750">

Это одно из изображений, загружаемых с Github-репозитория на сайт, как ранее было обнаружено.  
Можем скачать его прямо из браузера.

*2 способ* - Непосредственно из кода сайта  
В заголовке страницы указывается ссылка на фавикон

![Архив_поиск](/imgs/Pasted%20image%2020250719151308.png)

*3 способ* - С помощью командной строки Linux  
Можно получить ссылку на фавикон из кода страницы с использованием grep:
```
curl -sL http://shop.nft-kitty.online | grep -i 'rel="[^"]*icon'
```
![Архив_поиск](/imgs/Без%20имени-2.jpg)

Скачаем фавикон.  
Обратим внимание на его странное название:
```
bmZ0X2tpdHR5X3VubG9jaw==
```
![Архив_поиск](/imgs/Pasted%20image%2020250719140323.png)

Попробуем применить к нему CyberChef:

![Архив_поиск](/imgs/Pasted%20image%2020250719140431.png)

Результат - `nft_kitty_unlock`

Теперь у нас есть "ключ". Осталось найти, в какой "замок" его вставлять

Не имея иных зацепок, исследуем код сайта.  
В разделе `body` обнаружаем весьма интересный скрипт:

![Архив_поиск](/imgs/Pasted%20image%2020250719152310.png)
```js
<script>
  window.getFinalReward = function(key) {
    fetch("flag.php?key=" + encodeURIComponent(key))
      .then(res => {
        if (!res.ok) throw new Error("Ошибка сервера");
        return res.text();
      })
      .then(flag => alert(flag))
      .catch(err => alert("Ошибка: " + err.message));
  };
</script>
```
Этот скрипт может быть выполнен прямо из браузера и позволяет получить финальный флаг.  
Чтобы им воспользоваться, необходимо открыть страницу `shop.nft-kitty.online`, перейти к инструментам разработчика (`F12`) на вкладку `Console` и выполнить скрипт с обнаруженным ключом:  
```
getFinalReward('nft_kitty_unlock');
```
![Архив_поиск](/imgs/Pasted%20image%2020250719142235.png)

*Finally!* [Финальный флаг](#Флаги-для-поиска) - `{KOMPOT_RESCUED_FUNDS}` - найден.

### Итог
Полный список полученных флагов выглядит следующим образом:
![Архив_поиск](/imgs/Pasted%20image%2020250719142121.png)

> [!NOTE]
> По результатам проверки, задание было выполнено лишь на 80%.
> Третий флаг требовал округлённую сумму транзакции и некорректно принимался системой. По неясным причинам предполагалось округление вниз.
> ![Архив_поиск](/imgs/Pasted%20image%2020250719153821.png)

> [!IMPORTANT]
> UPD: После завершения мероприятия и выявления проблемы c флагом организаторы исправили и пересчитали очки для всех участников.  
> +Respect

