# Alterno

Visual document comparison using the blink comparator technique.

## What is Alterno?

Alterno rapidly alternates between two or more documents, displaying them in the same screen position. Any differences become immediately visible through visual flickering — added, removed, or changed content "blinks" while identical content remains stable.

This technique is inspired by **blink comparators** used by astronomers since 1904. Clyde Tombaugh discovered Pluto in 1930 using exactly this method — rapidly switching between photographic plates to spot moving objects.

## Features

- **Text Blink Mode**: Compare text documents (`.txt`, `.md`, `.java`, `.json`, `.xml`, `.pdf`)
- **Image Blink Mode**: Compare images pixel-by-pixel
- **Press Mode**: Export comparison as video file (`.avi`, `.mkv`) for lag-free playback in any video player
- **Multi-document Support**: Compare more than two documents (planned)
- **Adjustable Speed**: 1-10 FPS with customizable pause duration
- **Cross-platform**: Windows, Linux, macOS

## Why Blink Comparison?

Traditional diff tools show changes side-by-side or with highlighting. But human vision is exceptionally good at detecting motion and flickering. Blink comparison leverages this:
- **Instant detection**: Even single-pixel changes become obvious
- **No reading required**: You don't need to scan line by line
- **Pattern recognition**: Your brain spots anomalies automatically

## Use Cases

1. **Code review**: Spot changes between versions instantly
2. **Proofreading**: Find typos, missing commas, formatting issues
3. **UI comparison**: Detect pixel-level interface changes
4. **Document verification**: Ensure two documents are identical

## Etymology

**Alterno** (Latin): "I alternate, I change by turns"
- From *alternare* (to alternate)
- From *alter* (other, one of two)

The name reflects the core mechanic: documents alternate to reveal their differences.

## Technical Stack

- **Language**: Java 21+
- **Build**: Maven
- **UI**: JavaFX or Swing
- **Video Export**: FFmpeg or JavaCV (planned)
- **License**: The Unlicense

## Roadmap

- [x] Core concept and specification
- [ ] MVP: Text blink mode with two documents
- [ ] Image blink mode
- [ ] Press mode (video export)
- [ ] Multi-document comparison (3+ documents)
- [ ] PDF support
- [ ] Diff overlay mode

## Getting Started

*Coming soon — development in progress.*

## Contributing

This is a personal project. Ideas and feedback welcome.

## License

The Unlicense — public domain. See [LICENSE](LICENSE) file.



___
___
___



# Alterno

Визуальное сравнение документов методом мигающего компаратора.

## Что такое Alterno?

Alterno быстро чередует два или более документов, отображая их в одной позиции экрана. Любые различия становятся мгновенно видимыми благодаря визуальному мерцанию — добавленный, удалённый или изменённый контент "мигает", а идентичный остаётся стабильным.

Эта техника вдохновлена **мигающими компараторами**, которые астрономы используют с 1904 года. Клайд Томбо открыл Плутон в 1930 году именно этим методом — быстро переключая фотопластинки, чтобы заметить движущиеся объекты.

## Возможности

- **Режим Text Blink**: Сравнение текстовых документов (`.txt`, `.md`, `.java`, `.json`, `.xml`, `.pdf`)
- **Режим Image Blink**: Сравнение изображений попиксельно
- **Режим Press**: Экспорт сравнения как видеофайл (`.avi`, `.mkv`) для плавного воспроизведения без лагов в любом видеоплеере
- **Поддержка нескольких документов**: Сравнение более двух документов (планируется)
- **Настраиваемая скорость**: 1-10 FPS с настраиваемой паузой
- **Кроссплатформенность**: Windows, Linux, macOS

## Почему мигающее сравнение?

Традиционные diff-инструменты показывают изменения рядом или с подсветкой. Но человеческое зрение исключительно хорошо обнаруживает движение и мерцание. Мигающее сравнение использует это:
- **Мгновенное обнаружение**: Даже изменения в один пиксель становятся очевидными
- **Не нужно читать**: Не нужно сканировать строку за строкой
- **Распознавание паттернов**: Ваш мозг автоматически замечает аномалии

## Сценарии использования

1. **Code review**: Мгновенно заметить изменения между версиями
2. **Корректура**: Найти опечатки, пропущенные запятые, проблемы форматирования
3. **Сравнение UI**: Обнаружить изменения интерфейса на уровне пикселей
4. **Верификация документов**: Убедиться что два документа идентичны

## Этимология

**Alterno** (латинский): "я чередую, я сменяю поочерёдно"
- От *alternare* (чередовать)
- От *alter* (другой, один из двух)

Название отражает основную механику: документы чередуются, чтобы выявить их различия.

## Технический стек

- **Язык**: Java 21+
- **Сборка**: Maven
- **Интерфейс**: JavaFX или Swing
- **Экспорт видео**: FFmpeg или JavaCV (планируется)
- **Лицензия**: The Unlicense

## Дорожная карта

- [x] Основная концепция и спецификация
- [ ] MVP: Режим text blink с двумя документами
- [ ] Режим image blink
- [ ] Режим press (экспорт видео)
- [ ] Сравнение нескольких документов (3+ документов)
- [ ] Поддержка PDF
- [ ] Режим diff overlay

## Начало работы

*Скоро — разработка в процессе.*

## Участие

Это личный проект. Идеи и обратная связь приветствуются.

## Лицензия

The Unlicense — общественное достояние. См. файл [LICENSE](LICENSE).
