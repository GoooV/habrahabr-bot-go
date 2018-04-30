# Habrahabr-бот на Go #

Неофициальный бот для рассылки статей с сайтов [habrahabr.ru](https://habrahabr.ru/) и [geektimes.ru](https://geektimes.ru/) в Telegram. Бота можно найти [здесь](https://t.me/unofficial_habr_bot). Статью, описывающую процесс создания бота – [здесь](https://habrahabr.ru/post/350858/)

## Требования ##

* Язык - go1.10
* Сторонние библиотеки:
	* Telegram Bot API – [telegram-bot-api.v4](http://gopkg.in/telegram-bot-api.v4)
	* RSS парсер – [gofeed](https://github.com/mmcdole/gofeed)
	* Web scraper – [soup](https://github.com/anaskhan96/soup)
	* Парсер дат и времени – [jodaTime](https://github.com/vjeantet/jodaTime)
	* Job Scheduling Package – [gocron](https://github.com/jasonlvhit/gocron)
	* Продвинутое логгирование – [advanced-log](https://github.com/ShoshinNikita/advanced-log) и библиотека для Go – [advanced-log-go](https://github.com/ShoshinNikita/advanced-log-go)

## Информация о работе ##

Бот использует [RSS-ленту](https://habrahabr.ru/rss/all) сайта [habrahabr.ru](https://habrahabr.ru/) ([аналогично](https://geektimes.ru/rss/all/) для Geektimes) для получения списка статей. Данные пользователей (id, теги) хранятся в SQLite базе данных.

## Файлы и их описание ##

### Структура папок исходного кода ###

* src
	* main.go – главный файл
	* bot
		* bot.go – модуль, отвечающий за бота
		* commands.go – функции, которые обрабатывают команды бота
		* mailout.go – функции, осуществляющие рассылку
		* functions.go – полезные функции
		* reminders.go – функции, отвечающие за создание и отправку напоминаний
		* structures.go – структуры, которые используются в боте
		* constants.go – константы
	* articlesdb
		* articlesdb – отвечает за хранение статей
	* config
		* config.go – хранит конфигурационную информацию
	* userdb
		* userdb.go – отвечает за взаимодействие с базой данных
		* functions.go
	* logging
		* logging.go – отвечает за логгирование всего, что происходит в программе
	* website
		* website.go – модуль, отвечающая за сайт
* data
	* database.db
	* lastArticleTime.json
* templates
	* index.html - страница отправки сообщений
* stuff – содержит разные материалы

### Конфигурационная информация ###

Конфигурационная информация передаётся при запуске программы с помощью аргументов

```
  -aToken string
    	token of an app
  -bToken string
    	token of a bot
  -debug
    	debug mode (default – false)
  -delay int
    	delay of getting articles (default 600000000000)
  -logUrl string
    	url of advanced-log
  -pass string
    	password for the site
  -port
		port for website, without ':'
  -prefix string
    	prefix for paths to files (db, *.json)
```

### Содержание файлов ###

* Файл users.db – boltDB база данных, хранящая данные пользователей
	Структура:

	* users
		* id
			* HabrTags
			* HabrMailout
			* GeekTags
			* GeekMailout

* Файл articles.db – boltDB база данных, хранящая статьи за последние 7 дней
	Структура:

	* articles
		* id – text

* Файл lastArticleTime.json хранит время последних отправленных статей в UNIX формате

```json
{
	"habr": 0,
	"geek": 0
}
```

* Файл reminders.json используется для хранения напоминаний. Напоминания автоматически загружаются при запуске программы

## Лицензия ##

[MIT License](LICENSE)