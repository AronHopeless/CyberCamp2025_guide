# CyberCamp2025_guide
The way the CyberCamp's tasks were sold by mine

## Основной раздел. Задание 2.
### Условие инцедента
![Инцидент](/imgs/Incident.png)
![Задача](/imgs/Pasted%20image%2020250719092336.png)
### Флаги для поиска 
![Флаги](/imgs/Pasted%20image%2020250719093700.png)
### Решение
#### Флаг 1
В первую очередь перейдём по предложенной ссылке:
```
https://willy-the-cat.github.io/nft_cats/
```
![Флаги](/imgs/Pasted%20image%2020250719092406.png)

Видим, что страницы уже не существует -> необходимо просмотреть, что было на ней раньше  
Для этого перейдём на https://archive.org  
В строке поиска выбираем 'искать сайты', вставляем наш адрес
![Архив_поиск](/imgs/Pasted%20image%2020250719092528.png)

Существует одна версия сайта от 26го июня:
![Архив_поиск](/imgs/Pasted%20image%2020250719092642.png)

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
```
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

Просмотрим полную информацию о транзацкии
```
e28852904fe51ef17502f083e70b9b6ce08d173acc3a51a40135798f6418adf7
```
![Архив_поиск](/imgs/Pasted%20image%2020250719101232.png)




![Архив_поиск](/imgs/Pasted%20image%2020250719092528.png)
![Архив_поиск](/imgs/Pasted%20image%2020250719092528.png)
![Архив_поиск](/imgs/Pasted%20image%2020250719092528.png)
![Архив_поиск](/imgs/Pasted%20image%2020250719092528.png)
![Архив_поиск](/imgs/Pasted%20image%2020250719092528.png)
![Архив_поиск](/imgs/Pasted%20image%2020250719092528.png)
![Архив_поиск](/imgs/Pasted%20image%2020250719092528.png)
