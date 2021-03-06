# Custom WordPress theme creation

![title-picture](/img/title-picture.jpg)

* [Without bycicles. Download boilerplate theme](#without-bycicles-download-boilerplate-theme)
* [Установка](#Установка)
* [Кастомизируем](#Кастомизируем)
* [Инициализация скриптов и стилей](#Инициализация-скриптов-и-стилей)
	* [Подробный пример подключения скрипта](#Подробный-пример-подключения-скрипта)
* [Переносим управление контентом в настройки страницы/поста](#Переносим-управление-контентом-в-настройки-страницыпоста)
* [Кастомные типы страниц/постов](#Кастомные-типы-страницпостов)
* [Custom taxonomy wordpress](https://github.com/yuchiko/guide-wp-custom-theme/blob/master/chapter/custom-taxonomy.md)
* [Создание дополнительных шаблонов страниц](#Создание-дополнительных-шаблонов-страниц)
* [Функции, которые могут пригодится](https://github.com/yuchiko/guide-wp-custom-theme/blob/master/chapter/functions.md)
* [Полезности](#Полезности)
* [Возможности плагина ACF][https://github.com/yuchiko/guide-wp-custom-theme/blob/master/chapter/acf.md]
* [Плагины](https://github.com/yuchiko/guide-wp-custom-theme/blob/master/chapter/plugins.md)

## Краткий TODO <sup>1</sup>

![check-img](/img/check.png) Создать базу данных<br>
![check-img](/img/check.png) Скачать и установить WordPress<br>
![check-img](/img/check.png) Скачать и установить Underscores тему<br>
![check-img](/img/check.png) Закрыть сайт от индексации<br>
![check-img](/img/check.png) Установить плагины (стартовый набор у всех свой)<br>
![check-img](/img/check.png) Нарисовать структуру сайта<br>
![check-img](/img/check.png) Регистрация скриптов/стилей в functions.php<br>
![check-img](/img/check.png) Регистрация панели виджетов <sup>2</sup><br>
![check-img](/img/check.png) Регистрация кастомных типов страниц / постов <sup>2</sup><br>
![check-img](/img/check.png) Перенос верстки повторяющихся элементов (header, footer, sidebar)<br>
![check-img](/img/check.png) Перенос верстки Главной страницы (front-page.php)<br>
![check-img](/img/check.png) Перенос верстки вспомогательной страницы (page.php)<br>
![check-img](/img/check.png) Перенос верстки кастомных страниц/постов<br>
![check-img](/img/check.png) Автоматизация контента на страницах с помощью ACF<br>
![check-img](/img/check.png) Локализация страниц/постов и статических строк в шаблоне <sup>2</sup><br>
![check-img](/img/check.png) Заполнение контентом<br>
![check-img](/img/check.png) Открыть сайт для индексации<br>

<sup>1</sup> - с готовой версткой<br>
<sup>2</sup> - по необходимости

## Without bycicles. Download boilerplate theme
Не нужно изобретать велосипед и начинать с нуля, для старта хорошо использовать [Underscores тему](https://underscores.me/)

Сделать это можно через сайт [https://underscores.me/](https://underscores.me/), плюсом будет, что тут есть сразу поля ввода для имени вашей будущей темы, описания темы, имя автора, ссылка на автора. Удобно:) Ввел -> подтвердил -> получил архив с заполненным конфигом и уже переименованную папку под имя твоего проекта.

Второй способ: скачать архив с их гит репозитория [https://github.com/automattic/_s](https://github.com/automattic/_s), но тогда Вам нужно будет выполнить ряд измененний, которые указаны у них в README.md репозитория

**Важно! Именование темы должно быть уникальным, в противном случае можете столкнуться с тем, что тема с таким именем уже создана и вы даже не заметите как нажмете кнопку Обновить в админ панеле и получите совсем не известные для Вас файлы**

## Установка
Загружаем самую свежую версию WordPress с официального сайта. Распаковываем и скидываем нашу тему по адресу `/корень_сайта/wp-content/themes/`

## Кастомизируем
Замените дефолтное изображение проекта `screenshot.png` на скрин страницы или изображение логотипа. Размер должен быть 1200x900

### Убираем комментарии
Можно выпилять это полностью со всех файлов по очереди, а можно выключить в `Настройки -> Обсуждение`. Советую убрать галочки напротив этих пунктов

* Пытаться оповестить блоги, упоминаемые в статье 

* Разрешить оповещения с других блогов (уведомления и обратные ссылки) на новые статьи 

* Разрешить оставлять комментарии на новые статьи 

**Если уже были созданы какие-то страницы, то для них нужно вручную убрать эти настройки уже в настройках страницы `Настройки экрана -> Обсуждение`**

<!-- ### Добавление поддержки загрузки SVG в media

-

### Добавление кастомных размеров для обрезки фотографий -->

## Структура сайта
Визуализация сайта в виде схемы связей страниц важно для понимая, что из себя представляет представленный контент - пост, страница, кастомная страница. Простой пример:
![template-structure](/img/site-structure.jpg)

<!-- ## Разбитие темы на отдельные файлы -->

## Инициализация скриптов и стилей
Вся инициализация делается в нашем файле `functions.php`
Для инициализации стиля используем функцию [wp_enqueue_style](https://developer.wordpress.org/reference/functions/wp_enqueue_style/)
Для скриптов [wp_enqueue_script](https://developer.wordpress.org/reference/functions/wp_enqueue_script/)

Конкретно для темы `_s` все обьявления лучше делать группировать внутри функции `имя_темы_scripts`

Код для обьявления jQuery в конец тега `</body>`
```
wp_deregister_script( 'jquery' );
wp_register_script( 'jquery', '//ajax.googleapis.com/ajax/libs/jquery/1.11.2/jquery.min.js', array(), '20151215', true );
wp_enqueue_script( 'jquery' );
```

*Для взаимодействия php и javascript*, например передать путь к папке нашей темы, можно воспользоваться функцией, которая создаст глобальную переменную с нужной информацией
```
$wnm_custom = array( 'stylesheet_directory_uri' => get_stylesheet_directory_uri() );
wp_localize_script( 'main', 'directory_uri', $wnm_custom );
```

Выбор куда размещать скрипт в `<head>` или `</body>` управляется с помощью передачи параметра `$in_footer => true` в функцию `wp_enqueue_script`
Например:
```
wp_enqueue_script( 'jquery.maskedinput', get_template_directory_uri() . '/js/vendor/jquery.maskedinput.min.js', array(), '20151215', true );
```
### Подробный пример подключения скрипта
Для примера обьясню как правильно подключить ваш файл `custom.js` в вашей теме, которую, допустим, вы назвали `custom-theme`

Хорошим тоном будет группировать все скрипты в отдельной папке `js` или `scripts`. Еще лучше, если все подключаемые элементы (images,javascript,styles) будут лежать в папке `assets`, которую нужно создать в корне папке вашей темы.

Итого мы создали папку `assets` в ней `js` и поместили туда наш `custom.js`. Путь к нашему файлу выходит такой `http://your_site.com/wp-content/themes/custom-theme/assets/js/custom.js`

Переходим в файл `functions.php` находим функцию `custom-theme_scripts` и внутри пишем наше подключение, для этого случая конечный код будет таким
```
function custom_theme_scripts() {
	wp_enqueue_script( 'custom-name', get_template_directory_uri() . '/assets/js/custom.js', array('jquery'), '20171230', true);
}
add_action( 'wp_enqueue_scripts', 'custom_theme_scripts' );
```

* Разберем подробно: *

- `custom-name` (`$handle`) - рабочее название нашего скрипта, в дальнейшем, если мы хотим внести какой-то другой скрипт в зависимость (`$deps`) от этого скрипта, то используем именно его рабочее имя.

- `get_template_directory_uri() . '/assets/js/custom.js'` (`$src`) -  никто не пишет весь от корня к вашей папке с темой, для этого есть функция `get_template_directory_uri()`, которая вернет строку с URL текущей темы. Конкатенируем это с нашим `/assets/js/custom.js` и получаем тот полный путь, о котором мы говорили в начале. Важно не забыть `/` сразу после знака конкатенации `.`, потому как функция `get_template_directory_uri()` вернет нам путь без слеша в конце.

- `array('jquery')` (`$deps`) - массив названий скриптов от которых зависит наш скрипт. В данном случае наш скрипт подгрузится только после скрипта с рабочим названием `jquery`

- `'20171230'` (`$ver`) - версия скрипта, указывается в том случае, если потом скрипт будет обновляться и ваш сайт кешируется, тогда сменной даты мы покажем браузеру, что скрипт обновился и с кеша его брать не нужно, а надо повторно скачать с сайта.

- `true` (`$in_footer`)- передаем true, если скрипт нужно обьявить перед закрывающим тегом `body`

## Инициализация скриптов и стилей для админки и редактора

Для админки
```
function load_custom_wp_admin_style() {
        wp_register_style( 'custom_wp_admin_css', get_template_directory_uri() . '/assets/css/admin-style.css', false, '1.0.0' );
        wp_enqueue_style( 'custom_wp_admin_css' );
}
add_action( 'admin_enqueue_scripts', 'load_custom_wp_admin_style' );
```

Для редактора
```
function wpdocs_theme_add_editor_styles() {
    add_editor_style( get_template_directory_uri() . '/assets/css/admin-style.css' );
}
add_action( 'admin_init', 'wpdocs_theme_add_editor_styles' );
```

## Переносим управление контентом в настройки страницы/поста
Смену всего динамического контента нужно реализовать максимально легко для менеджера/админа сайта. Для этого используем [AdvancedCustomFields](https://www.advancedcustomfields.com/)
Хорошо приобрести его платное дополнение для создания повторяющихся полей(например изображений в слайдере) - [Repeater Field](https://www.advancedcustomfields.com/add-ons/repeater-field/)
Одна покупка на все сайты и пожизненное обновление, на этом фоне цена в 25$ звучит смешно.
### Кратко как использовать
После установки плагина у нас появится вкладка "Произвольные поля" в сайдбаре админки. Переходим -> Добавляем новую запись -> Выбираем правило в каком обьекте WP появятся наши доп. поля.

При создании стоит понимать, что `Имя поля` это его будущее название переменной. Я использую БЭМ именование и поля называю, если например применять наши доп. поля применимы по правилу к типу постов `Projects`, тогда все имена полей я начинаю с `projects__` (`projects__name`, `projects__addition-image`)

Созданное поле появится в выбранном по правилам обьекте. А вывести его можно по разному, в зависимости от типа, который ему выбрали. 

* Если нужно запомнить значение в переменной тогда подойдет запись `<?php $value = get_field("имя_поля"); ?>`
* Другой случай - вывод в конкретном месте, тогда в выбранном месте пишем `<?php the_field("имя_поля");?>` или `<?php echo get_field("имя_поля");?>`

Перебор кастомных полей в поле типа `Repeater`<br>
* Проверка на наличие полей<br>
`<?php if( have_rows('имя_основного_поля_того_что_repeater') ):?>
`
* Перебор полей, что содержатся в repeater'e<br>
`<?php while ( have_rows('имя_основного_поля_того_что_repeater') ) : the_row(); ?>`
* Внутри этого перебора обращения к полям пишутся с добавление `_sub_`, а имеено `get_sub_field`,`the_sub_field`
* Если нужно прекратить вывод на какой-то итерации, тогда выход из цикла командой `break` и после цикла, строчки `endwhile` в этом случае, обязательно пишем сброс нашего запроса с глобального массива `reset_rows();`

Пример кода:
```
<?php if( have_rows('site__row') ): $i = 0; ?>
  <div class="row__div">
    <h2 class="row__title"><?php the_field('row__title');?></h2>
    <ul>
	  <?php while ( have_rows('landing__reviews') ) : the_row(); $i++ ?>
		<li>
		  <img src="<?php the_sub_field('row-child__field');?>" alt="">
		</li>
	  <?php if ($i == 3) { break; } ?>
	  <?php endwhile; ?>
	  <?php reset_rows(); ?>
    </ul>
  </div>
<?php endif; ?>
```


Подробнее на сайте плагина в [Документации](https://www.advancedcustomfields.com/resources/)


## Кастомные типы страниц/постов
Просто пары полей может быть мало для полной свободы действий и универсальности решения. Например по ТЗ у нас есть три типа постов полностью отличных друг от друга по внешнему виду и типу контента - ПРОЕКТЫ, БИБЛИОТЕКА, НОВОСТИ. Для начала нам нужно добавить эти сущности в WP. Для этого идем в наш файлик `functions.php`, который лежит в папке нашей темы и в конце делаем запись `require get_template_directory() . '/inc/post-types.php';`

> Это можно было бы и не писать и все обьявления делать тут же в файлике, но так будет удобнее и файлик `functions.php` будет чище.

Создаем файл `post-types.php` в папке `inc`, которая лежит там же где и наш `functions.php`

В файлике описываем новую сущность с помощью данного кода:
```
<?php
function project_post_type() {
	$labels = array(
		'name'                => _x( 'Проекты', 'Post Type General Name', 'имя_темы' ),
		'singular_name'       => _x( 'Проект', 'Post Type Singular Name', 'имя_темы' ),
		'menu_name'           => __( 'Проекты', 'имя_темы' ),
		'name_admin_bar'      => __( 'Проекты', 'имя_темы' ),
		'parent_item_colon'   => __( 'Родительский элемент:', 'имя_темы' ),
		'all_items'           => __( 'Все Проекты', 'имя_темы' ),
		'add_new_item'        => __( 'Добавить новый проект', 'имя_темы' ),
		'add_new'             => __( 'Добавить новый', 'имя_темы' ),
		'new_item'            => __( 'Новый', 'имя_темы' ),
		'edit_item'           => __( 'Редактировать', 'имя_темы' ),
		'update_item'         => __( 'Обновить', 'имя_темы' ),
		'view_item'           => __( 'Посмотреть', 'имя_темы' ),
		'search_items'        => __( 'Искать проект', 'имя_темы' ),
		'not_found'           => __( 'Не найдено', 'имя_темы' ),
		'not_found_in_trash'  => __( 'Не найдено в корзине', 'имя_темы' ),
	);
	$args = array(
		'menu_icon' 		  => 'dashicons-cart',
		'label'               => __( 'projects', 'имя_темы' ),
		'description'         => __( 'Все Проекты на сайте', 'имя_темы' ),
		'labels'              => $labels,
		'supports'            => array( 'title', 'thumbnail', 'revisions', 'page-attributes', ),
		'taxonomies'          => array(),
		'hierarchical'        => false,
		'public'              => true,
		'show_ui'             => true,
		'show_in_menu'        => true,
		'menu_position'       => 5,
		'show_in_admin_bar'   => true,
		'show_in_nav_menus'   => true,
		'can_export'          => true,
		'has_archive'         => false,
		'exclude_from_search' => false,
		'publicly_queryable'  => true,
		'capability_type'     => 'page',
	);
	register_post_type( 'project', $args );
}
add_action( 'init', 'project_post_type', 0 );
```
Подробнее можно почитать [тут](https://codex.wordpress.org/Post_Types)

### Вывод кастомных типов c помощью WP_Query

## Создание дополнительных шаблонов страниц

Если нужно несколько вариантов дизайна страницы, можно добавить дополнительные шаблон. Для этого сделайте дубль файла `page.php` в вашей теме и назовите его, допустим `page-projects.php`, то какое имя он будет иметь в админ панеле при создании страницы можно задать внутри созданного Вами файла `page-projects.php` строчкой `Template name: Имя шаблона` вначале файла
```
<?php
/**
 * Template name: Страница проекта
 *
 * @link https://codex.wordpress.org/Template_Hierarchy
 *
 * @package Your_template_name
 */
?>
```
Есть и другие варианты, дефолтную страницу можно перебить создав шаблон с конкретной привязкой к id или slug страницы.
- page-[id].php
- page-[slug].php

Вот информация о иерархии шаблонов WordPress.
![template hierarchy](/img/template-hierarchy.png)

## Мультиязычность

[Polylang](https://polylang.pro/)

## Полезности

### Другое

[Wordpress generated css cheat sheet](http://www.wpbeginner.com/wp-themes/default-wordpress-generated-css-cheat-sheet-for-beginners/)

[Wordpress Best Practice](https://codex.wordpress.org/%D0%A1%D1%82%D0%B0%D0%BD%D0%B4%D0%B0%D1%80%D1%82%D1%8B_%D0%BA%D0%BE%D0%B4%D0%B8%D1%80%D0%BE%D0%B2%D0%B0%D0%BD%D0%B8%D1%8F_WordPress)

[CSS testing in the WordPress environment](http://www.wpfill.me/)

[Font Converter](https://onlinefontconverter.com/)

### READ
[Migrate existing website to wordpress](https://www.smashingmagazine.com/2013/05/migrate-existing-website-to-wordpress/)
[Complete guide custom post types](https://www.smashingmagazine.com/2012/11/complete-guide-custom-post-types/)
