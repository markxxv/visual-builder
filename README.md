# Visual Builder

Минимальный WYSIWYG-конструктор страниц на Alpine.js + Tailwind CSS.

Идея простая: дизайн секций задаётся разработчиком, клиент меняет текст, добавляет готовые блоки, переставляет их и управляет содержимым grid-секций без свободного редактирования вёрстки.

## Запуск

Сборка не нужна. Открой `index.html` в браузере или запусти любой static server.

Основные файлы:

```
index.html
img/
  01_text.png
  02_stats.png
  ...
```

Все шаблоны сейчас находятся в массиве `templates` внутри `editorApp()`.

## Обычный блок

Минимальный шаблон:

```js
{
    name: 'Text Block',
    image: 'img/01_text.png',
    hasColumnSelector: false,
    html: `
        <section class="text_block">
            <h2 contenteditable="true">Section Heading</h2>
            <p contenteditable="true">Editable text</p>
        </section>
    `
}
```

Текст, который должен редактироваться пользователем, помечается:

```html
contenteditable="true"
```

Стили блока можно добавить в `<style type="text/tailwindcss">`:

```css
.text_block {
    @apply space-y-6 mt-12;
}

.text_block h2 {
    @apply text-2xl font-semibold;
}
```

## Grid-блок

Для секций с повторяемыми карточками включается `hasColumnSelector`.

```js
{
    name: 'Stats',
    image: 'img/02_stats.png',
    hasColumnSelector: true,
    defaultColumns: 2,

    itemTemplate: `
        <div class="card">
            <strong contenteditable="true">100%</strong>
            <p contenteditable="true">Description</p>
        </div>
    `,

    html: `
        <section
            class="grid grid-cols-1 gap-4"
            data-grid-section
        >
            <div class="card">
                <strong contenteditable="true">50%</strong>
                <p contenteditable="true">First item</p>
            </div>

            <div class="card">
                <strong contenteditable="true">80%</strong>
                <p contenteditable="true">Second item</p>
            </div>
        </section>
    `
}
```

`data-grid-section` указывает редактору контейнер карточек.

`itemTemplate` используется кнопкой **Add Block** для создания нового элемента.

Для таких секций редактор автоматически даёт выбор колонок: `1 / 2 / 3 / 4 / 6`.

## Как добавить новый блок

1. Сделай HTML секции.
2. Добавь её стили.
3. Добавь объект в массив `templates`.
4. Положи preview в `img/`.
5. Для изменяемого текста используй `contenteditable="true"`.
6. Для повторяемых элементов добавь `hasColumnSelector`, `itemTemplate` и `data-grid-section`.

Пример:

```js
{
    name: 'Quote',
    image: 'img/07_quote.png',
    hasColumnSelector: false,
    html: `
        <section class="quote_block">
            <blockquote contenteditable="true">
                Your quote
            </blockquote>

            <p contenteditable="true">
                Author
            </p>
        </section>
    `
}
```

## Состояние

Редактор хранит страницу как массив секций:

```js
[
    {
        id: 1,
        name: 'Text Block',
        html: '<section>...</section>',
        hasColumnSelector: false,
        columns: 2,
        itemTemplate: ''
    }
]
```

Текущее состояние можно получить через:

```js
saveToConsole()
```

И восстановить:

```js
loadState(data)
```

Это состояние в дальнейшем можно сохранять в Laravel как JSON.

## Что уже есть

- добавление готовых секций;
- inline-редактирование текста;
- H2 / H3 / H4, bold, italic, списки;
- перемещение и удаление секций;
- добавление и удаление элементов grid;
- смена количества колонок;
- редактирование SVG;
- Undo / Redo;
- сериализация состояния в JSON.
