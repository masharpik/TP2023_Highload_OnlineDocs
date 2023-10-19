# TikTok

## 1. Тема и целевая аудитория

<B>ТикТок</B> представляет собой сервис, который предназначен для создания и просмотра видеороликов небольшого размера.

### MVP

1. Публикация видео
2. Просмотр ленты
3. Сбор статистики (Пропуски/оценка/комментарии)

### Целевая аудитория

По состоянию на 2023 год <ins>MAU</ins> =  1.1 млрд [^1]. 

Распределение по странам
Страна | Подключены пользователей (млн)
:---  | :---:
США | 140.58
Индонезия | 106.9
Бразилия | 74
Россия | 56.31
Мексика | 51.27
Вьетнам | 49.59
Филиппины | 42.75
Таиланд | 39.5
Турция | 30.83
Саудовская Аравия | 25.2
Пакистан | 25.15
Великобретания | 23.82
Египет | 22
Ирак | 20.68
Франция | 19.74

## 2. Расчет нагрузки

По состоянию на 2023 год <ins>DAU</ins> = 600 млн [^2].

Я провела 10 минут на сайте, чтобы выяснить, какие запросы за этот период были отправлены.

Получилось, что за 10 минут было отправлено **2039** запросов(161 МБ), из них 50 запросов за видео(125 МБ).

Среднее время пользования приложением за день (на 2023 год) = 95 мин [^3].

Будем расчитывать нагрузку с расчетом на 5 лет [^4].

### Публикация видео

#### Продуктовые метрики

Исходя из данных, можно сделать вывод, что средний размер скачиваемого видео: 125 МБ / 50 = 2.5 МБ. За день пользователи загружают 34 млн видео [^5].

Средний месячный размер хранилища пользователя равен:

```
2.5 МБ * (34 млн видео / 600 млн пользователей) * 30 дней = 4,25 МБ.
```

Значит, <ins>средний размер хранилища пользователя</ins>:

```
4.25 МБ * 12 месяцев * 5 лет = 455 МБ
```

<ins>Среднее количество действий пользователя в день</ins>:

```
34 млн видео / 600 млн пользователей = 0.06 видео
```

#### Технические метрики

При загрузке одного моего видео в TikTok пришлось до 350 запросов (6.1 МБ) и около 1 минуты.

Таким образом, <ins>среднее количество запросов в секунду</ins> равно:

```
350 / (1 * 60) * 34000000 / (24 * 60 * 60) = 2295 RPS
```

<ins>Размер хранения</ins> публикаций видео:

```
455 МБ * 1.1 млд человек / 1024 Гб = 488769531 Тб
```

Максимальный размер видео в тикток может быть 287.6 МБайт [^6]. 
<ins>Пиковое потребление в течение суток</ins> равно:

```
(287.6 * 8 / 1024) Гбит * 34 млн/д / (24 * 60 * 60) = 884 Гбит/с
```

<ins>Суммарный суточный</ins> трафик (объем данных, передающихся через сети за сутки):

```
34 млн/д * 2.5 МБ / 1024 = 83008 Гбайт/д
```

### Просмотр ленты

Среднее количество запросов на в секунду равно:

```
2039 / (10 * 60) * 600 000 000 / (24 * 60 * 60) ≈ 23600 RPS
```

Пиковое потребление в течение суток равно: ((((161 МБ / (10 * 60) секунд) * 8 / 1024) Гбит/с * (95 * 60) c/д) Гбит/д / (24 * 60 * 60)) Гбит/с * 600млн  = **82980** Гбит/с 

Суммарный суточный (Гбайт/сутки): (82980 / 8) Гбайт/с * 24 * 60 * 60 = 896184000 Гбайт/день

### Сводная таблица

Нагрузка пользователя в секунду | Размер хранения (Тб) |    RPS   |  Пиковое потребление в течение суток (Гбит/с) |  Суммарный суточный (Гбайт/сутки)
---                             |         ---          | ---      | ---                                           | ---
Публикация видео                |       4458.42        |    2295  |   884                                         |  9547200
Просмотр ленты                  |         -            |    23600 |   82980                                       |  896184000
Лайки/комментарии               |                      |          

## Глобальная балансировка нагрузки

### Распределение по страннам
Страна | Подключены пользователей (млн)
---  | ---
США | 140.58
Индонезия | 106.9
Бразилия | 74
Россия | 56.31
Мексика | 51.27
Вьетнам | 49.59
Филиппины | 42.75
Таиланд | 39.5
Турция | 30.83
Саудовская Аравия | 25.2
Пакистан | 25.15
Великобретания | 23.82
Египет | 22
Ирак | 20.68
Франция | 19.74

### Расположение датацентров:
Поскольку самое большое количество пользователей находится в США, то там необходимо расположение ДЦ [^6]. Так же нужно расположить ДЦ  Индонезия, Бразилия и России.Так как наш сервис коротких видео должен быть доступен во всем мире без ограничений по времени пинга для быстрой загрузки нового видео. 
### Принцип балансировки:
#### GeoDNS
В целях выбора региона или страны, в которой располагается ДЦ выбираем балансировку по географии для того чтобы запросы поступали на ближайший сервер.
#### BgpAnycast
Выбор определенного сервера, который может быстрее ответить клиенту дает технология BgpAnycast.


## Список источников:
[^1]: [MAU](https://www.demandsage.com/tiktok-user-statistics/#:~:text=1.1%20billion%20are%20its%20monthly%20active%20users%20as%20of%202023)
[^2]: [DAU](https://www.demandsage.com/tiktok-user-statistics/#:~:text=11.%20How%20Much%20Time%20Do%20TikTok%20Users%20Spend%20On%20The%20Platform%3F)
[^3]: [Среднее время пользования в день](https://www.theverge.com/interface/2020/6/10/21285309/tiktok-2020-user-numbers-revenue-smash-hit-mea-culpa)
[^4]: [Статистика роста регистрации в TikTok с 2017 года](https://www.demandsage.com/tiktok-user-statistics/#:~:text=at%201.677%20billion.-,Here%20is%20a%20table%20showing%20TikTok%E2%80%99s%20Users%20Over%20the%20years%3A,-Year)
[^5]: [Среднее количество загрузки видео в TikTok в день](https://techjury.net/blog/how-many-videos-are-uploaded-to-tiktok-daily/#:~:text=34%20million%20videos%20posted%20on%20Tiktok%20daily)
[^6]: [Максимальный размер видео](https://www.anyrec.io/ru/tiktok-video-size/#:~:text=%D0%94%D0%BB%D1%8F%20%D1%83%D1%81%D1%82%D1%80%D0%BE%D0%B9%D1%81%D1%82%D0%B2%D0%B0%20iOS%20%D0%B2%D1%8B%20%D0%BC%D0%BE%D0%B6%D0%B5%D1%82%D0%B5%20%D0%B7%D0%B0%D0%B3%D1%80%D1%83%D0%B7%D0%B8%D1%82%D1%8C%20%D0%B2%D0%B8%D0%B4%D0%B5%D0%BE%20%D1%81%20%D1%80%D0%B0%D0%B7%D0%BC%D0%B5%D1%80%D0%BE%D0%BC%20%D1%84%D0%B0%D0%B9%D0%BB%D0%B0%20287%2C6%20%D0%9C%D0%91)
[^7]: [Регионы по популярности](https://inclient.ru/tiktok-stats/#auditoria-tiktok-v-2023-godu:~:text=%D0%A1%D0%B0%D0%BC%D1%8B%D0%B9%20%D0%B2%D1%8B%D1%81%D0%BE%D0%BA%D0%B8%D0%B9%20%D0%BE%D1%85%D0%B2%D0%B0%D1%82%20%D0%BF%D0%BE%D0%BB%D1%8C%D0%B7%D0%BE%D0%B2%D0%B0%D1%82%D0%B5%D0%BB%D0%B5%D0%B9%20TikTok%20%D0%BD%D0%B0%D0%B1%D0%BB%D1%8E%D0%B4%D0%B0%D0%B5%D1%82%D1%81%D1%8F%20%D0%B2%20%D0%A1%D0%B0%D1%83%D0%B4%D0%BE%D0%B2%D1%81%D0%BA%D0%BE%D0%B9%20%D0%90%D1%80%D0%B0%D0%B2%D0%B8%D0%B8%20(99.6%25)%2C%20%D0%9E%D0%90%D0%AD%20(97.7%25)%20%D0%B8%20%D0%9A%D1%83%D0%B2%D0%B5%D0%B9%D1%82%D0%B5%20(91.3%25).%20%D0%A1%D0%A8%D0%90%20%D0%B7%D0%B0%D0%BD%D0%B8%D0%BC%D0%B0%D0%B5%D1%82%2020%2D%D0%B5%20%D0%BC%D0%B5%D1%81%D1%82%D0%BE%20%D0%BF%D0%BE%20%D0%BE%D1%85%D0%B2%D0%B0%D1%82%D1%83%20%D0%B2%20TikTok%20(55%25).%20%D0%94%D0%BB%D1%8F%20%D1%81%D1%80%D0%B0%D0%B2%D0%BD%D0%B5%D0%BD%D0%B8%D1%8F%2C%20%D0%BE%D1%85%D0%B2%D0%B0%D1%82%20TikTok%20%D0%B2%20%D0%A0%D0%BE%D1%81%D1%81%D0%B8%D0%B8%20%D0%B2%202023%20%D0%B3%D0%BE%D0%B4%D1%83%20%D1%81%D0%BE%D1%81%D1%82%D0%B0%D0%B2%D0%BB%D1%8F%D0%BB%2049.2%25)
