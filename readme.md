# Критерии хорошей верстки

Обязательные и необязательные критерии оценки вёрстки.


## 1. Разметка


### 1.1. Соблюден код-гайд

TODO: Добавить ссылку на код-гайд.


### 1.2. Разметка проходит валидацию на [validator.w3.org/nu](https://validator.w3.org/nu/)

Проверяются все сверстанные страницы.

Проверке подвергается и логическая структура документа (флаг `Show Outline` при проверке). Допустим «безымянный» блок для `<nav>`.


### 1.3. Отсутствуют грубые ошибки

Примеры грубых ошибок: ссылки не являются ссылками, `<br><br>` вместо параграфов.

Примеры негрубых (условно-допустимых) ошибок: `<div>` вместо `<section>`, `<header>` или `<article>`, отсутствие списка в одноуровневой навигации.


### 1.4. Разметка работоспособна без стилей и скриптов

Страница без стилей и скриптов, будучи открыта в браузере, выглядит логично и работает:

- видна чёткая иерархия блоков (заголовки),
- не видны элементы, ненужные или не работающие без стилей и скриптов (пример: бургер главного меню),
- интерактивные элементы (ссылки, в т.ч. якорные, формы) видны и работоспособны.

**Исключение:** страницы, работоспособность которых невозможна без стилей/скриптов.


### 1.5. Использованы доступные семантические возможности

- Текстовые описания элементов форм являются `<label>`-ами и привязаны к соответствующим `<input>`.
- Использованы правильные типы для `<input>` (`email`, `tel`, `date` ect.), даже если у них недостаточная кроссбраузерная поддержка.
- Табличные данные представлены в `<table>`. **Исключение:** некоторые адаптивные таблицы.


### 1.6. Использована методология БЭМ, отсутствуют типовые ошибки

CSS-классы, использованные в разметке, написаны по БЭМ ([Описание методологии](https://ru.bem.info/methodology/quick-start/)). Предпочтительным является использование [диалекта Two Dashes](https://ru.bem.info/methodology/naming-convention/#Стиль-two-dashes) (`__` как разделитель для элемента и `--` как разделитель для модификатора).

Типовые ошибки:

- повторение разделителя более одного раза в одном классе: `block__element__element` или `block--modifier--modifier`,
- класс-модификатор без класса, который он модифицирует: `<div class="block--modifier">` или `<div class="btn  toggler--red">`,
- упоминание внешнего вида блока или элемента в классах, не являющихся модификаторами.


### 1.7. Использован правильный вьюпорт-тег

Для адаптивных сайтов: `<meta name="viewport" content="width=device-width,initial-scale=1">`


## 2. Стилизация


### 2.1. Соблюден код-гайд

TODO: Добавить ссылку на код-гайд.


### 2.2. Отклонения от макета минимальны

На прорисованных ширинах вертикальные отклонения не должны превышать 10px (но не более 5% для малых высот), горизонтальные не должны превышать 5px. Ширины блоков, формирующих раскладку блоков на странице должны точно совпадать с макетом.

Недопустимы отклонения по параметрам текста, цветам, контентным изображениям.

### 2.3. Шрифты заданы правильно

При указании названия шрифта в `font-family` должны быть указаны альтернативные семейства и тип семейства.


### 2.4. Отсутствуют «CSS-заплатки»

В стилях не должно быть `!important`.

На блоках раскладки не должно быть `overflow[-x||-y]: hidden;` (исключение: в дизайне есть горизонтальный скролл).


### 2.5. Фрагменты страницы выдерживают переполнение и недополнение

Простые фрагменты верстки должны выдерживать переполнение текстом (увеличение габаритов и/или сокрытие части длинного текста).

Фрагменты, содержащие потоковые блоки (пример: блок каталога товаров, содержащий поток вложенных блоков с товарами), должны выдерживать переполнение и недополнение потоковыми блоками.

Вставка в разметку контентных изображений с размерами, отличающимися от представленных в макете, не должно ломать страницу.


### 2.6. Использован подход mobile-first

При стилизации компонентов страницы, сначала написана стилизация под мобильные устройства, затем дописаны стили для широких вьюпортов. **Исключение:** фрагменты страницы, имеющие сильные отличия на широких вьюпортах.


## 3. CSS-процессинг


### 3.1. Стилевые файлы разделены поблочно

Один бэм-блок = один препроцессорный файл.

Допустимы несколько глобальных стилевых файлов: диспетчер подключений, переменные.


### 3.2. При вложении селекторов нет вложений глубже 3-го уровня

Псевдоселекторы и условия не считаются увеличивающими уровень вложенности.

```
.promo {
  display: block;

  a {               // первый уровень вложенности

    &:hover {       // не увеличивает уровень вложенности

      span {...}    // второй уровень вложенности
    }

    strong {...}   // второй уровень вложенности
  }
}
```


### 3.3. `@media` написаны в контексте селекторов

```
.promo {
  display: block;

  @media (min-width: $screen-md) {...} // правильно, написано в контексте
}

@media (min-width: $screen-md) {       // неправильно, написано вне контекста
  .promo {...}
}
```


### 3.4. Все переменные описаны в одном файле. В переменные вынесены, как минимум: breakpoints, главные размеры текста, главные шрифтовые группы

```
$font-size:                   1.6rem;
$font-size--h1:               3rem;
$font-size--h2:               2.4rem;

$line-height:                 1.375;

$font-family:                 Roboto, 'Helvetica Neue', Arial, sans-serif;
$font-family--headings:       Georgia, 'Times New Roman', Times, serif;

$screen-sm:                   600px;
$screen-md:                   900px;
$screen-lg:                   1200px;
$screen-xl:                   1800px;
```


### 3.4. Амперсанд использован правильно

Амперсанд допустимо использовать перед:

- разделителем БЭМ-элемента,
- разделителем БЭМ-модификатора,
- псевдоэлементом или псевдоселектором.

```
.promo {

  // Правильно: амперсанд перед псевдоклассом
  &:hover { ... }

  // Правильно: амперсанд перед разделителем элемента
  &__item {

    // НЕПРАВИЛЬНО: амперсанд в месте разделения словосочетания в названии элемента
    &-link { ... }
  }

  // НЕПРАВИЛЬНО: амперсанд в месте разделения словосочетания в названии блока
  &-shover { ... }

  // Правильно: модификаторы нужно писать под элементами
  &--large { ... }

}
```


### 3.5. Использован простпроцессинг

Стили должны обрабатываться, как минимум, Autoprefixer-ом и конкатенатором медиа-выражений.


## 4. Дополнительные файлы

### 4.1. Минимум подключенных файлов

К странице должны быть подключены 1 CSS-файл и не более 3 синхронно подгружаемых JS-файлов.

CSS-файл должен быть подключен в секции `<head>`. JS-файлы должны быть подключены перед `</body>`.


### 4.2. Шрифты подключены в нужных форматах

Необходимые форматы: `woff`, `woff2`.


### 4.3. Растровые изображения оптимизированы

Растровые изображения должны быть оптимизированы, но не иметь видимых отличий от макета.


### 4.4. Векторные изображения не содержат растра

Внутри используемых SVG не должно встречаться растровых изображений.

**Исключение:** использование SVG для каких-либо сложных эффектов на растровых изображениях.


## 5. Оптимизация


### 5.1. Мелкие растровые иконки и декоративные изображения сшиты в компактный спрайт

Отдельные изображения должны быть представлены в папке с исходниками.


## 6. Автоматизация


### 6.1. Все зависимости проекта устанавливаются командой `npm i` или `yarn install` и не хранятся в репозитории проекта

Зависимости прописаны, как минимум, в `package.json`, папки зависимостей (`node_modules`, `bower_components`) добавлены в `.gitignore`.


### 6.2. В репозитории есть `readme.md` с описанием того, как стартовать проект локально и описанием дополнительных команд (если есть)

Выполнение инструкции должно позволять вести разработку проекта вне зависимости от операционной системы и установленных в системе пакетов.

Должен быть представлен список доступных команд. Пример: команда `npm run deploy` — выгрузка содержимого папки `build` на GH-pages.


### 6.3. При старте автоматизации отсутствуют необязательные ресурсоёмкие задачи

Пример: оптимизация растровых изображений должна происходить по отдельной команде, а не при любом старте локального сервера.


### 6.4. Результат сборки проекта не хранится в репозитории

Результат компиляции/транспиляции не должен присутствовать в репозитории проекта. **Исключение:** использование GH-pages, наличие собранного проекта в ветке `gh-pages`.


## 7. Разное

### 7.1. Верстка имеет минимальные отличия в IE11 и последних 2-х версиях Chrome, Firefox, Opera и Safari

Допустимы отличия, связанные с рендером шрифтов и использованием нативных элементов форм (флажки, радиокнопки и т.п.).

Допустимы различия в вертикальных размерах до 10px, но не более 5%.


### 7.2. В именах классов нет транслита, использован английский язык


### 7.3. Использованные фреймворки для верстки (Bootstrap и подобные) установлены как зависимости проекта

NPM- или yarn-зависимости. Ошибкой считается копирование всех файлов фреймворка в папку исходников и хранение их в репозитории.



## Необязательные критерии


### Контроль включенного JS

В разметке страниц использовано что-то вроде:

```
<script>
  // Маркер работающего javascript
  document.documentElement.className = document.documentElement.className.replace('no-js', 'js');
</script>
```


### Спорные текстовые элементы могут быть представлены любым тегом

Если в отношении фрагмента страницы сложно определить является ли он заголовком (или сложно понять уровень заголовка), у этого фрагмента должен быть класс со стилизацией, позволяющей использовать любой подходящий блочный тег (`<div>`, `<h[1-6]>`).


### Текстовые фрагменты, выводимые из CMS не должны иметь классов на тегах

Текстовой фрагмент, предположительно появляющийся из CMS целиком, не должен иметь CSS-классов на тегах, но должен иметь обертку с CSS-классом.

Пример: текст записи в блоге.


### Контроль загрузки шрифтов

При первой загрузке страницы сознательно выбран FOIT/FOUT/FOFT, при второй и далее минимизирована заметность выбранного подхода.


### В автоматизации предусмотрена команда «чистовой» сборки проекта

Выполнение команды приводит к сборке бьютифицированной разметки, минимизированных стилей/скриптов без карт кода.
