# 11ty Simple - MVP Gen v2.0

Базовый простой проект на основе 11ty. Предназначен для сборки на основе готовых собранных тем и готовых ресурсов (html, css, js, img / video, font, favicon) и для генерации html на основе .md-файлов

## Технологии

[11ty / Eleventy](https://www.11ty.dev/)

## **Использование**

Установите все npm-пакеты с помощью команды:

```bash
$ npm i 
```

## Разработка

### Требования

Для установки и запуска проекта, необходим [NodeJS](https://nodejs.org/) v8+.

- index.html - без минификации для удобства
- Section с классом .main-content для seo-контента
    
    ```html
    <section class="main-content">
        <div class="container">
            ... 
        </div>
    </section>
    ```
    
- Обертка с классом .table-wrap - для скролла таблицы на маленьких экранах
    
    ```html
    <div class="table-wrap">
	<table>
	...
	</table>
    </div>
    ```
    
    ```css
    .main-content .table-wrap {
      max-width: 100%;
      overflow-x: scroll;
    }
    ```
    
    *seo.css - стили в папке css для описания .main-content и внутренних селекторов, можете использовать его
    

### Предварительная настройка

1. Предполагается, что у вас есть готовая сборка “темы”  под нужный бренд, но без разметки *seo-контента* со следующей структурой:
    
    ```bash
    └── theme/                
	    	├── css/                # Стили в расширении .css
	    	├── js/                 # Скрипт
	    	    └── index.js      	# Для удобства имя index, но можно изменить в подключении в разметке и добавить дополнительные скрипты      
	        ├── img/                # Изображения
	        ├── font/               # Шрифты
	        ├── index.html          # Разметка страницы 		
	        └── favicon.ico
    ```
    
    *Папку ***theme*** нужно переместить в папку **src**
    
2. Код из ***index.html*** переместить в файл index.njk.
    - [ ]  Поменять статичный контент на переменные, которые будут извлекаться из index.md
	    - [ ]  lang → {{ lang }}
	    - [ ]  title → {{ title }}
	    - [ ]  content → {{ description }}
	    - [ ]  main-content → {{ content | wrapTables | safe }}
	        
	        *wrapTables - самописный фильтр для обертки таблиц, добавляющей скролл
	        
	        *safe - фильтр, сообщающий 11ty, что содержимое безопасно для рендеринга как HTML
        
    - [ ]  При необходимости заменить пути (добавить /theme/ для корректной загрузки css, js, изображений и шрифтов)
    
    **Пример**
    
    ```html
    <!DOCTYPE html>
    <html lang="{{ lang }}">
    
    <head>
        <meta charset="UTF-8">
        <meta name="viewport" content="width=device-width,initial-scale=1">
        <meta name="robots" content="noindex, nofollow">
        <title>{{ title }}</title>
        <link rel="icon" href="./favicon.ico">
        <link rel="stylesheet" href="./theme/css/main.min.css">
        <meta name="description" content="{{ description }}">
    </head>
    
    <body>
	<div class="page">
	   <section class="main-content">
		<div class="container">
		  {{ content | wrapTables | safe }}  
		</div>
	   </section>
	</div>
    </body>
    ```
    

1. Файл фавиконки favicon.ico или favicon.* переместить в корень проекта
2. Подготовка seo-контента
    - [ ]  Экспорт в формат .md из google docs
    - [ ]  Копирование содержимого в файл index.md
    - [ ]  Настройка мета-информации в заглавной части файла index.md
        - [ ]  layout → index.njk - * На его основе строится html-разметка страницы
        - [ ]  title - Название страницы, содержимое тега title
        - [ ]  description - Мета-описание, содержимое атрибута content мета-тега описания
        - [ ]  lang - код языка страницы, содержимое атрибута lang тега html
    
    **Пример**
    
    ```markdown
    ---
    layout: index.njk
    title: 11ty Simple Template
    description: 11ty Simple Template - description
    lang: fr
    ---
    ```
    

### Дополнительные настройки

Если у вас в проекте есть специфичные директории и файлы, например video, то в .eleventy.js в корне проекта можно добавить строчку для отслеживания и копирования в сборку данной папки, здесь же можно поменять пути и названия папок, в зависимости от вашей структуры

```jsx
export default function(eleventyConfig) {
    // Копирование ресурсов - папки `src/*` в папку `public`
    eleventyConfig.addPassthroughCopy('favicon.ico');
    eleventyConfig.addPassthroughCopy('src/theme/img');
    eleventyConfig.addPassthroughCopy('src/theme/font');
    eleventyConfig.addPassthroughCopy('src/theme/css');
    eleventyConfig.addPassthroughCopy('src/theme/js');
		
		// Копирование папки video в public
		eleventyConfig.addPassthroughCopy('src/theme/video');

    return {
        dir: {
          input: 'src',
          output: 'public',
          includes: 'components',
          layouts: 'layouts'
        }
    };
};
```

## Обновление v2.0

### Глобальный конфиг для сайта

_data/site.json_
```json
{
    "title": "11ty Simple",
    "description": "11ty Simple - desc",
    "lang": "en",
    "alternate": "en-US",
    "favicon": "svg",
    "domain": "11ty-simple-example.com"
}
```

**Поля:**

	1. _title_ - название сайта, применится если у страницы .md во frontmatter не передан title
 
	2. _description_ - мета-описание сайта, применится если у страницы .md во frontmatter не передан description
 
	3. _lang_ - язык сайта, применяется если не передан во frontmatter
 
	4. _alternate_ - используется для назначения языка при генерации link alternate
 
	5. _faviсon_ - указывается расширение фавиконки (чтобы не изменять постоянно в шаблоне index.njk)
 
	6. _domain_ - адрес домена для продакшен сборки (без https://) - используется для генерации sitemap.xml и robots.txt

### Шаблоны для robots.txt, sitemap.xml

Добавлены 2 шаблона robots.njk, sitemap.njk - для генерации robots.txt и sitemap.xml

#### Как использовать sitemap?

Используйте коллекцию **sitemap** в  **поле tags** для страницы .md, чтобы добавить ее при генерации в итоговую карту сайта:

*index.md*
```md
---
tags: sitemap
sitemap:
    priority: "0.8"
---
```
Также можно указать **поле sitemap.priority** для доп. настройки - приоритета страницы, 
	(рек.: указать в кавычках для избежания лишнего форматирования числа)

## **Запуск Development сервера**

Для запуска live-сервера можно использовать следующую команду

```powershell
npm start
```

## Запуск сборки проекта

```powershell
npm run build
```
