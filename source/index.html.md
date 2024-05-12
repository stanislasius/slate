---
title: Caffesta API

language_tabs: # must be one of https://github.com/rouge-ruby/rouge/wiki/List-of-supported-languages-and-lexers
  - python : Python

<!--toc_footers:
  - <a href='#'>Sign Up for a Developer Key</a>
  - <a href='https://github.com/slatedocs/slate'>Documentation Powered by Slate</a>-->

includes:
  - errors

search: false

code_clipboard: true

meta:
  - name: Caffesta API
    content: Протокол интеграции с Caffesta посредством API
---

# Описание протокола взаимодействия с Caffesta

```text
Выше выберите язык, для которого Вам нужны примеры.
```

Данный протокол взаимодействия по API с Caffesta предназначен для получения или передачи данных по разным разделам
программы.

Весь обмен данными происходит с использованием **JSON-объектов**.

Кодировка передаваемого текста - **UTF-8**.

<aside class="warning">
Все объекты, у которых есть <b>ref_code</b> создаются на основе данных из интегрирующейся программы. 
</aside>

Caffesta будет сначала искать объект с переданным значением в `ref_code`, и если не найдёт совпадений, создаст новый
объект.

Примеры таких объектов:

* Внешний код категории - `category_ref_code`;
* Внешний код цеха/отдела - `workspace_ref_code`;
* Внешний код товара - `product_code`.

Также при реализации некоторых возможностей через API могут встречаться объекты (формата `caf_*_id`), которые должны уже
быть созданы. Например:

* `caf_storage_id` - ID склада точки продаж или
  заведения ([создаётся в административной части программы]("https://caffesta.com/ru/help/41"))
* `caf_shop_id` - ID точки продаж(нужно связаться с отделом продаж Caffesta).






