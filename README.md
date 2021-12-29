# Модуль общедоменного перевода русский-английский, английский-русский

Модуль общедоменного перевода представляет собой две модели, предобученные на выборке параллельных текстов датасета [OPUS](https://opus.nlpl.eu/). Модели тестировались на тестовой выборке русского и английского языков соревнования [The Tatoeba Translation Challenge (v2021-08-07)](https://github.com/Helsinki-NLP/Tatoeba-Challenge/blob/master/README-v2021-08-07.md).

## Установка

- Необходимо установить виртуальное окружение Python не ниже 3.7
- Для установка зависимостей необходимо запустить `pip install -r requirements.txt`
- Для запуска моделей необходимо использовать GPU, таким образом префиксом команд будет `CUDA_VISIBLE_DEVICES=i`, где `i` — индекс свободной GPU.

## Обучение модели с нуля

- Запуск скрипта `tatoeba_train_enru.sh` или `tatoeba_train_ruen.sh` вызовет режим обучения в направлении английский-русский или русский-английский соответсвенно со следующими параметрами:
  - Количество эпох: 10
  - Максимальная длина входной последовательности: 192
  - Максимальная длина выходной последовательности: 192
- Перед началом обучения инициализируется со случайными весами и сохраняется в папку `base_enru` или `base_ruen` базовая модель для соответствующего направления
- Для ускорения обучения возможно использование нескольких GPU
- Обученная модель и информация об обучении по умолчанию сохраняются в папку `output_enru` или `output_ruen` в зависимости от направления перевода

## Оценка качества

- Для того, чтобы получить оценки качества модели в направлении английский-русский необходимо запустить `run_evaluate_enru.sh`. В результате работы скрипта в папке `output_enru` будет представлена статистика качества перевода на тестовой выборке датасета Tatoeba для соревнования The Tatoeba Translation Challenge (v2021-08-07).
- Аналогично для пары русский-английский `run_evaluate_ruen.sh`.

## Генерация

Для генерации переводов необходимо использовать скрипт `run_predict.py`. Скрипт генерирует последовательности на целевом языке на основе `txt` файла с текстовыми последовательностями на исходном языке. Скрипт использует следующие параметры:

- `direction` — направление перевода, либо `enru`, либо `ruen`.
- `sentences_path` — путь к файлу с последовательностями на исходном языке (в формате один пример для перевода на каждой строке).
- `output_file` — путь к файлу, в котором будут сохранены переводы модели.
- `model` — путь к папке с моделью или название модели на HuggingFace Hub.
- `num_return_sequences` — количество генерируемых гипотез для каждого примера, при значении > 1 варианты перевода разделяются знаком табуляции.
- `num_beams` — количество лучей при генерации лучевым поиском, при значени `1` используется жадный алгоритм.
- `batch_size` — размер батча, используемого при генерации гипотез.
