# Neovim JSTS Code Editor

Это пошаговая инструкция по сборке полноценного JavaScript/TypeScript Code Editor 
на базе Neovim и конфигурационного фреймворка NvChad.

Проект состоит из пяти основных этапов:

1. Установка и настройка NvChad
2. Установка инструментов разработчика (LSP, линтер, форматтер)
3. Установка дополнительных плагинов
4. Установка и настройка отладчика
5. Настройка pre-commit pipeline (ESLint + Prettier)

А также включает подробку полезных и функциональных плагинов для повседневной работы.

## Содержание

- [Часть 1: Установка и настройка NvChad](#часть-1-установка-и-настройка-nvchad)
    - [Установка NvChad](#установка-nvchad)
    - [Структура файлов NvChad](#структура-файлов-nvchad)
    - [Конфигурируем NvChad](#конфигурируем-nvchad)
- [Часть 2: Установка и настройка инструментов разработчика](#часть-2-установка-и-настройка-инструментов-разработчика)
    - [Treesitter](#treesitter)
    - [LSP TypeScript Language Server (ts_ls)](#lsp-typescript-language-server-ts_ls)
    - [Tailwind CSS Language Server](#tailwind-css-language-server)
    - [ESLint](#eslint)
    - [Emmet](#emmet)
    - [Форматирование с ESLint и Prettier](#форматирование-с-eslint-и-prettier)
- [Часть 3: Установка и настройка дополнительных инструментов](#часть-3-установка-и-настройка-дополнительных-инструментов)
    - [Список дополнительных плагинов](#список-дополнительных-плагинов)
    - [Установка](#установка)
- [Часть 4: Установка и настройка отладчика](#часть-4-установка-и-настройка-отладчика)
- [Часть 5: Настройка pre-commit pipeline](#часть-5-настройка-pre-commit-pipeline)
- [Полезные плагины](#полезные-плагины)
- [Список использованных материалов](#список-использованных-материалов)
- [Приложение 1: Плагины NvChad по умолчанию](#приложение-1-плагины-nvchad-по-умолчанию)
- [Приложение 2: Как работает подсветка синтаксиса](#приложение-2-как-работает-подсветка-синтаксиса)
- [Приложение 3: Как работает LSP](#приложение-3-как-работает-lsp)
- [Приложение 4: Как работает автодополнение](#приложение-4-как-работает-автодополнение)
- [Приложение 5: Как работает отладка](#приложение-5-как-работает-отладка)
- [Tips and tricks](#tips-and-tricks)

## Часть 1: Установка и настройка NvChad

Происходит в соответствии с официальной документацией NvChad.

### Установка NvChad

Перед установкой необходимо удалить предыдущие конфигурации Neovim и убедиться в наличии следующих зависимостей:  

 - `Nerd Font` - требует в качестве шрифтов терминала.  
 - `Tree-sitter-cli` - требуется для установки парсеров nvim-treesitter.  
 - `Ripgrep` - для grep-поиска в Telescope.  
 - `GCC` - для сборки native-расширений.  

Установка выполняется одной командой:

```bash
git clone https://github.com/NvChad/starter ~/.config/nvim && nvim
```

### Структура файлов NvChad


| Директория | Назначение |
|---|---|
| `~/.config/nvim/` | Пользовательские конфигурации — основная рабочая директория |
| `~/.local/share/nvim/` | Установленные плагины (Lazy, Mason) |
| `~/.local/state/nvim/` | Файлы состояния: история, undo, swap, логи |


### Конфигурируем NvChad

Основной файл конфигурации: `~/.config/nvim/lua/chadrc.lua`  

Получить полную справку по его структуре:
```bash
:h nvui
```

В `chadrc.lua` конфигурируются:

- Цветовая схема (раздел base46)
- Элементы пользовательского интерфейса:
    - Стиль окна автодополнения (cmp)
    - Стиль Telescope
    - Стиль statusline
    - Стиль tabufline
- Параметры плавающего терминала (раздел term)
- Приветственное окно (nvdash)
- Стиль cheatsheet
- Настройки `colorify.nvim` — отображение образцов цветов в буфере
- Пакеты Mason для автоматической установки

`hl_override и hl_add`  

Раздел `hl_override` в base46 полностью переопределяет стили для указанных `highlight-групп`.  
Раздел `hl_add` — дополняет существующие стили новыми, не заменяя их.  
Просмотреть доступные для переопределения группы можно командами:  
```bash
## список стандартных групп
:help highlight-groups
## текущие группы и их стили
:hi 
```

После внесения изменений в `chadrc.lua` выполнить для применения конфигурации:
```bash
:Lazy sync
:MasonInstallAll
```

## Часть 2: Установка и настройка инструментов разработчика

### Treesitter 

Установка необходимых парсеров:
```bash
:TSInstall javascript typescript regex lua luap luau html css json jsonc bash markdown markdown_inline yaml toml dockerfile git_config git_rebase gitattributes gitcommit gitignore 
```

### LSP TypeScript Language Server (ts_ls)

### Tailwind CSS Language Server

### ESLint

### Emmet

### Форматирование с ESLint и Prettier

Prettier принимает конфигурацию из файла .prettierrc (или .prettierrc.json).  
  
Пример:  
```json
  "semi": true,
  "trailingComma": "es5",
  "singleQuote": true,
  "printWidth": 80,
  "tabWidth": 2,
  "useTabs": false
```

## Часть 3: Установка и настройка дополнительных инструментов

### Список дополнительных инструментов

`taplo`  

Инструмент для комфортной работы с `*.toml` файлами.  
Работает как lsp-сервер, предоставляет: автодополнение, диагностику ошибок, навигацию, форматирование, валидацию toml по json schema.  

[taplo github page](github.com/tamasfe/taplo)

`yaml language server`

Инструмент для комфортной работы с `*.yaml` файлами.  
Работает как lsp-сервер, предоставляет: автодополнение, диагностику ошибок, hover-информацию, навигацию, форматирование, валидацию схем.  

[yaml-language-server github page](https://github.com/redhat-developer/yaml-language-server)

`docker language server`

Инструмент от Docker для работы с Dockerfile и Bake.  
Работает как lsp-сервер, предоставляет: автодополнение, диагностику ошибок, навигацию, форматирование и hover-информацию.  
Поддерживает линтинг для Dockerfile.  

[docker-language-server github page](https://github.com/docker/docker-language-server)

`dockerfmt`

Форматтер для Dockerfile.  
Он автоматически приводит Dockerfile к единому стилю: форматирует директивы, нормализует пробелы и форматирует shell-команды.  
Умеет преобразовывать устаревший синтаксис ENV.

[dockerfmt github page](https://github.com/reteps/dockerfmt)

### Установка

Добавить в `chadrc.lua`:  
```lua
-- mason
...
M.mason = {
  pkgs = {
      ...
    "yaml-language-server",
    "taplo",
    "docker-language-server",
    "dockerfmt",
  }

```

Добавить в `configs/lspconfig.lua`:
```lua
require("nvchad.configs.lspconfig").defaults()

local servers = {
    ...
  yamlls = {
    filetypes = { "yaml", "yaml.docker-compose" },
  },
  taplo = {},
  docker_language_server = {},
}
...
```

Добавить в `configs/conform.lua`:
```lua
local options = {
    ...
    dockerfile = { "dockerfmt" },
}
```

Выполнить:  
```bash
:MasonInstallAll
```

## Часть 4: Установка и настройка отладчика

Для отладки JavaScript и TypeScript кода в Neovim будем использовать js-debug-adapter.  
Это официальный и самый продвинутый отладчик от Microsoft (тот же самый, что используется по умолчанию в VS Code).  
Он поддерживает отладку как серверного кода (Node.js), так и фронтенда (в связке с браузером), а также отлично работает с TypeScript без предварительной ручной компиляции.

## Часть 5: Настройка pre-commit pipeline

## Полезные плагины

### AI-ассистент

`yetone/avante.nvim`

[avante github page](https://github.com/yetone/avante.nvim)

### Навигация и редактирование

`folke/flash.nvim`

Быстрые прыжки по тексту.  
Улучшенные motions f/t.  

[flash github page](https://github.com/folke/flash.nvim)

`kevinhwang91/nvim-ufo`

Улучшает встроенный folding.  

[neovim ufo github page](https://github.com/kevinhwang91/nvim-ufo)

`mg979/vim-visual-multi`

Полноценное multi-cursor редактирование.  
Одновременная работа с несколькими курсорами и выделениями.  

[vim-visual-multi github page](https://github.com/mg979/vim-visual-multi)

`hinell/duplicate.nvim`

Удобное дублирование строк и выделенных блоков.  

[duplicate github page](https://github.com/hinell/duplicate.nvim)

`fedepujol/move.nvim`

Перемещение строк и блоков с возможностью сохранения индентации.

[move github page](https://github.com/fedepujol/move.nvim)

### Уведомления и интерфейс

`folke/noice.nvim`

Заменяет стандартный UI для сообщений, командной строки и popup-меню:  

- Отображает уведомления с помощью notify.nvim  
- Позволяет гибко маршрутизировать сообщения от neovim и отображать их в разных представлениях  
- Ведёт историю сообщений  
- Добавляет UI для Command line  
- Добавляет UI для поиска  
- Добавляет UI для Hover-документации от LSP  
- Добавляет UI для LSP signature help  
- Подсвечивает синтаксис Vim/Lua прямо в Commad line  
- Отображает длинный вывод команд в обычных буферах/split  
- Подсвечивает синтаксис Vim/Lua прямо в Command line  

[noice github page](https://github.com/folke/noice.nvim)

`lukas-reineke/indent-blankline.nvim`

Подсветка направляющих отступов и вложенности.  
Улучшает читаемость кода.

[indent-blankline github page](https://github.com/lukas-reineke/indent-blankline.nvim)

`HiPhish/rainbow-delimiters.nvim`

Подсвечивает вложенные разделители, теги и ключевые слова разными цветами.  
Улучшает читаемость кода (особенно html и jsx).  

[rainbow-delimiters github page](https://github.com/hiphish/rainbow-delimiters.nvim)

`folke/todo-comments.nvim`

Подсвечивает теги комментариев (FIX, TODO, NOTE, PERF, HACK, WARNING).
Улучшает читаемость кода.

[todo-comments github page](https://github.com/folke/todo-comments.nvim)

`karb94/neoscroll`

Добавляет плавную анимацию прокрутки в Neovim вместо резких скачков при перемещении по файлу.  

[neoscroll github page](https://github.com/karb94/neoscroll.nvim)

`sphamba/smear-cursor.nvim`

Дополнительная анимация для курсора.  
Позволяет лучше замечать перемещения курсора.  

[smear-cursor github page](https://github.com/sphamba/smear-cursor.nvim)

`MeanderingProgrammer/render-markdown.nvim`

Рендеринг Markdown прямо в буфере Neovim.  

[render-markdown github page](https://github.com/MeanderingProgrammer/render-markdown.nvim)

### lsp интеграция

`nvimdev/lspsaga.nvim`

Улучшает встроенный LSP-интерфейс Neovim, предоставляя более удобный UI для навигации, диагностики, code actions и работы с символами:

- Показывает диагностику LSP в удобных float-окнах и позволяет быстро переходить между ошибками  
- Даёт Finder для поиска определений, ссылок и других LSP-символов  
- Добавляет Peek Definition / Peek Type Definition — просмотр определения прямо во всплывающем окне  
- Улучшает Hover — отображение документации и информации о символе  
- Добавляет удобный Rename с предварительным просмотром изменений и поиском/заменой по проекту  
- Показывает Code Actions в отдельном UI с live preview  
- Показывает Lightbulb при наличии доступных Code Actions  
- Добавляет Call Hierarchy — поиск входящих и исходящих вызовов  
- Показывает реализации интерфейсов и позволяет быстро переходить к ним  
- Добавляет Outline — дерево символов текущего файла  
- Добавляет Breadcrumbs — текущий путь по символам в winbar  
- Улучшает переходы Go to Definition / Type Definition визуальным beacon-индикатором  
- Добавляет float terminal  
- Предоставляет единый настраиваемый UI для LSP float-окон, границ, иконок и preview  

[lspsaga github page](https://github.com/nvimdev/lspsaga.nvim)

`folke/trouble.nvim`

Предоставляет единый удобный UI для диагностики и результатов LSP, Quickfix и Location List.  
Удобно для копирования сообщений lsp и линтера.

[trouble github page](https://github.com/folke/trouble.nvim)

### git интеграция

Git-клиент внутри Neovim

`NeogitOrg/neogit`

[neogit github page](https://github.com/NeogitOrg/neogit)

## Список использованных материалов

neovim: [neovim official website](https://neovim.io/)  
neovim: [neovim github page](https://github.com/neovim/neovim)  
  
nvchad: [nvchad official website](https://nvchad.com/)  
nvchad: [nvchad github page](https://github.com/NvChad/NvChad)  

dotfyle - discover neovim plugins: [dotfyle](https://dotfyle.com/)  

lazy: [lazy plugin manager docs](https://lazy.folke.io/)  
lazyvim: [lazyvim setup docs](https://www.lazyvim.org/)  

debugging guide: [guide to debugging in neovim](https://tamerlan.dev/a-guide-to-debugging-applications-in-neovim/)   
lazyvim dap spec: [lazyvim dap core specification](https://www.lazyvim.org/extras/dap/core)

## Приложение 1: Плагины NvChad по умолчанию

### Плагины NvChad

Базовые плагины фреймворка NvChad, обеспечивающие его работу, темы и пользовательские меню.


| Плагин | Описание | Ссылка |
|--------|----------|--------|
| nvchad/base46 | Движок тем NvChad: управление цветовыми схемами, переопределение highlight-групп, интеграции с плагинами | https://github.com/NvChad/base46 |
| nvchad/ui | UI-слой NvChad: statusline, tabufline (вкладки + буферы), cheatsheet, theme switcher, updater | https://github.com/NvChad/ui |
| nvzone/menu | Библиотека для создания всплывающих меню (используется minty и другими nvzone-плагинами) | https://github.com/nvzone/menu |
| nvzone/volt | Фреймворк для создания реактивных UI-компонентов внутри Neovim | https://github.com/nvzone/volt |
| nvzone/minty | Инструменты подбора и манипуляции цветом: команды Huefy и Shades | https://github.com/nvzone/minty |


### Плагины для автодополнения

Система автодополнения (completion) и всё, что с ней связано: движок сниппетов, источники, авто-закрытие скобок.


| Плагин | Описание | Ссылка |
|--------|----------|--------|
| hrsh7th/nvim-cmp | Основной фреймворк автодополнения (completion engine) | https://github.com/hrsh7th/nvim-cmp |
| L3MON4D3/LuaSnip | Движок сниппетов с поддержкой сложных преобразований | https://github.com/L3MON4D3/LuaSnip |
| rafamadriz/friendly-snippets | Коллекция готовых сниппетов для множества языков и фреймворков | https://github.com/rafamadriz/friendly-snippets |
| windwp/nvim-autopairs | Автоматическое закрытие скобок, кавычек и других парных символов | https://github.com/windwp/nvim-autopairs |
| saadparwaiz1/cmp_luasnip | Источник сниппетов LuaSnip для nvim-cmp | https://github.com/saadparwaiz1/cmp_luasnip |
| hrsh7th/cmp-nvim-lua | Источник API Neovim Lua для nvim-cmp | https://github.com/hrsh7th/cmp-nvim-lua |
| hrsh7th/cmp-nvim-lsp | Источник LSP-завершений для nvim-cmp | https://github.com/hrsh7th/cmp-nvim-lsp |
| hrsh7th/cmp-buffer | Источник завершений из содержимого открытых буферов для nvim-cmp | https://github.com/hrsh7th/cmp-buffer |
| FelipeLema/cmp-async-path | Источник путей файловой системы для nvim-cmp | https://codeberg.org/FelipeLema/cmp-async-path |


### User Interface плагины

Плагины, отвечающие за визуальное оформление, навигацию и отображение информации.


| Плагин | Описание | Ссылка |
|--------|----------|--------|
| nvim-tree/nvim-web-devicons | Набор иконок для типов файлов (используется nvim-tree, telescope и др.) | https://github.com/nvim-tree/nvim-web-devicons |
| nvim-tree/nvim-tree.lua | Файловый менеджер в виде дерева директорий | https://github.com/nvim-tree/nvim-tree.lua |
| folke/which-key.nvim | Всплывающая подсказка по доступным хоткеям при нажатии лидера | https://github.com/folke/which-key.nvim |
| lukas-reineke/indent-blankline.nvim | Вертикальные линии отступов (indent guides) и подсветка scope-блоков | https://github.com/lukas-reineke/indent-blankline.nvim |


### Инструменты 

Инструменты для разработки: LSP, форматирование, подсветка синтаксиса.


| Плагин | Описание | Ссылка |
|--------|----------|--------|
| mason-org/mason.nvim | Менеджер внешних инструментов: LSP-серверы, DAP-адаптеры, линтеры, форматтеры | https://github.com/mason-org/mason.nvim |
| neovim/nvim-lspconfig | Стандартная конфигурация LSP-серверов для Neovim | https://github.com/neovim/nvim-lspconfig |
| stevearc/conform.nvim | Форматирование кода с поддержкой множества форматтеров и LSP-fallback | https://github.com/stevearc/conform.nvim |
| nvim-treesitter/nvim-treesitter | Синтаксический парсер: улучшенная подсветка, навигация по AST, инкрементальный выбор | https://github.com/nvim-treesitter/nvim-treesitter |


### Другое 

Плагины общего назначения, не вошедшие в предыдущие категории.


| Плагин | Описание | Ссылка |
|--------|----------|--------|
| nvim-lua/plenary.nvim | Набор Lua-утилит (JSON, Path, async-операции, job control) — зависимость многих плагинов | https://github.com/nvim-lua/plenary.nvim |
| nvim-telescope/telescope.nvim | Универсальный fuzzy-поиск: файлы, grep, diagnostics, git, LSP и сотни расширений | https://github.com/nvim-telescope/telescope.nvim |
| lewis6991/gitsigns.nvim | Git-значки в gutter-колонке: добавлено, изменено, удалено, blame, hunk-навигация | https://github.com/lewis6991/gitsigns.nvim |


## Приложение 2: Как работает подсветка синтаксиса

В современных версиях neovim движок tree-sitter встроен в neovim.  
Плагин `nvim-treesitter.nvim` - это надстройка, которая устанавливает парсеры и поставляет файлы запросов.  
Саму подсветку выполняет встроенный движок tree-sitter.  

Работает примерно так:

1. Определение filetype (*.py, *.html, etc).  
2. Включение подсветки (включается tree-sitter подсветка для буфера).  
3. Поиск парсера (neovim проверяет установлен ли для этого языка парсер).  
4. Построение дерева (если парсер найден, движок tree-sitter строит AST - синтаксическое дерево).  
5. Применение запросов (подбирается файл запроса: такой файл описывает, каким узлам дерева какая синтаксическая роль соответствует. движок tree-sitter помечает узлы специальными именами).  
6. Раскраска (эти имена и есть группы подвестки highlight groups. neovim берёт для них цвета, заданные текущей цветовой схемой).  

## Приложение 3: Как работает LSP

Language Server Protocol (LSP) — это протокол обмена данными между LSP-клиентом и LSP-сервером.  

LSP-клиент встроен в сам Neovim. LSP-серверы, специфичные для каждого языка программирования или разметки, устанавливаются отдельно — в нашем случае через Mason.  

LSP-сервер может поддерживать автодополнение кода, диагностику, переход к определению (go to definition), подсветку семантических элементов и другие возможности. Функциональность у разных LSP-серверов разная: один сервер может уметь всё перечисленное, другой — только часть.  

Встроенному LSP-клиенту для запуска конкретного сервера нужна конфигурация: как его запустить, в какой директории проекта работать, какие настройки ему передать.  
Писать это вручную для каждого сервера неудобно, поэтому используется плагин nvim-lspconfig — он не запускает серверы сам, а просто хранит готовые конфигурации для множества известных LSP-серверов и документацию по ним.  
Neovim использует эти конфигурации, а запускает и поддерживает соединение с сервером уже сам, силами встроенного LSP-клиента.

## Приложение 4: Как работает автодополнение

С автодополнением дело обстоит немного сложнее. У встроенного LSP-клиента есть базовая поддержка автодополнения, но её интерфейс скромный.  
Поэтому обычно используют отдельный плагин — nvim-cmp. Это универсальный движок автодополнения: сам по себе он не знает ни про LSP, ни про буфер, ни про пути к файлам — он умеет только показывать всплывающее меню, сортировать и фильтровать варианты.  
Все конкретные подсказки ему поставляют отдельные плагины-источники (sources), каждый из которых отвечает за свой тип данных:

- cmp-nvim-lsp — источник, который запрашивает варианты у LSP-сервера через встроенный LSP-клиент и передаёт их в nvim-cmp;
- cmp-buffer — предлагает слова, уже встречающиеся в открытых буферах;
- cmp-path — предлагает пути к файлам;
- cmp-nvim-lua — предлагает элементы Lua API самого Neovim (vim.api, vim.lsp.util и т.д.). это отдельный статический источник, никак не связанный с LSP-сервером lua-language-server — он полезен именно при редактировании конфигов Neovim на Lua;
- и другие — под сниппеты, историю буфера обмена и так далее.

Остальные возможности LSP — диагностика, переход к определению, hover-подсказки и подобное — nvim-cmp не касаются.  
Их встроенный LSP-клиент реализует сам, напрямую через Neovim API (vim.lsp.buf.*, знаки на полях, floating-окна, quickfix-список), без участия сторонних плагинов автодополнения.

## Приложение 5: Как работает отладка

### Протокол DAP

DAP (Debug Adapter Protocol) - это спецификация, описывающая формат общения между клиентом-редактором и отладчиком (точнее — debug-адаптером, программой-посредником, которая говорит на DAP и управляет реальным отладчиком под капотом).

В отличие от LSP, поддержка DAP не встроена в Neovim: своего DAP-клиента у редактора нет.  
Эту роль берёт на себя плагин nvim-dap — он реализует протокол DAP и выступает клиентом для Neovim.

### Как это работает на примере Python

Сам процесс отладки Python-кода ведёт программа debugpy — она выступает debug-адаптером, то есть той стороной, которая понимает DAP-команды от nvim-dap и превращает их в реальные действия над отлаживаемым процессом.  
Схема:  
*Neovim (nvim-dap) → по протоколу DAP → debugpy (debug-адаптер) → управляет → отлаживаемый Python-процесс*  

1. Neovim через nvim-dap отправляет debugpy команды: поставить точку останова, начать выполнение, сделать шаг и так далее.  
2. Debugpy запускает отлаживаемый файл нужным интерпретатором Python — это не обязательно системный интерпретатор, часто указывают путь к интерпретатору из virtualenv/venv.  
3. Управляет самим выполнением программы (установка точек останова, пошаговое выполнение, чтение состояния переменных) debugpy не напрямую, а через встроенную в него библиотеку pydevd — именно она вклинивается в выполнение Python-кода на низком уровне.  
4.  Результаты (текущая строка, стек вызовов, значения переменных) идут в обратном направлении: pydevd → debugpy → nvim-dap → Neovim.  

### Дополнительные зависимости

Сам nvim-dap отвечает только за протокол и базовое управление сессией — у него нет ни готового интерфейса, ни настроек под конкретные языки «из коробки».  
Поэтому в связке с ним используют плагины-надстройки:  

- nvim-dap-ui — графический интерфейс для nvim-dap: панели со стеком вызовов, переменными, точками останова, консолью (REPL) и т.д.  
- nvim-dap-virtual-text — показывает значения переменных прямо рядом с кодом, используя тот же механизм virtual text (extmarks), что и подсказки LSP-сервера.  
- nvim-nio — библиотека для асинхронного ввода-вывода, зависимость nvim-dap-ui.  
- nvim-dap-python — берёт на себя настройку nvim-dap специально под debugpy: не нужно вручную прописывать adapter и configuration для Python.  

## Tips and tricks

### Подсветка строки с курсором

Раскомментировать в `options.lua`:  
```lua 
require "nvchad.options"

local o = vim.o

-- enable cursorline
o.cursorlineopt ='both'
```

Цвет подсветки устанавливается в `chadrc.lua`:  
```lua
hl_override = {
    ...
    CursorLine = { bg = "#343b49" },
}
```

### Корректное определение типов файлов на примере docker-compose

Как распознаётся тип текущего файла:
```bash
:set filetype?
```

Создать файл filetypes.lua в `~/.config/nvim/lua/`:  

```lua
vim.filetype.add({
  filename = {
    ["docker-compose.yml"] = "yaml.docker-compose",
    ["docker-compose.yaml"] = "yaml.docker-compose",
    ["compose.yml"] = "yaml.docker-compose",
    ["compose.yaml"] = "yaml.docker-compose",
  },
})
```

Добавить в `init.lua`:  
```lua
...
require "filetypes"
...
```

Перепроверить `filetype?`

