# Итоговое задание - Игра в города.

Вам необходимо написать полноценную игру в города, разработать простой
искусственный интеллект для хода компьютера и реализовать алгоритм определения
победителя.


### Основные правила игры:

- Города должны лежать в файле cities.txt
- Ответы должны записываться в файл answers.txt
- Игра начинается с того, что компьютер выбирает случайный город из 
  общего списка городов и выводит его на экран
- Игрок имеет 5 попыток на всю игру, каждый раз, когда игрок называет
  неверный город, у него снимается 1 попытка. Когда количество попыток
  будет 0 - игрок проиграл.
- Если компьютер(ИИ) не смог найти город, в ответ на город игрока, то
  считаем, что компьютер проиграл.
- Если текущий город оканчивается на какую-то из букв "ь", "ъ", "ы", то игрок/компьютер 
  должен подобрать город, который начинается на предпоследнюю букву текущего города.
- При каждом правильном ходе игрока и ходе компьютера, записывать город в answers.txt
- В конце игры файл answers.txt необходимо очистить


### Рекомендации к выполнению задания:

1. Список городов можно получить так:

    ```python3
    f = open('cities.txt')
    cities_list = [line.strip().lower() for line in f]
    f.close()
    ```

    В итоге в cities_list получим список примерно такого вида:

    `['астрахань', 'борисоглебск', 'киров', 'воронеж', ...]`


2. Разбейте код программы на функции, например создайте функцию для определения последней буквы города,
   а затем вызывайте её в программе.
    ```python3
   def get_last_char(city):
       last_char = city[-1]
       if last_char in "ьъы":
           last_char = city[-2]
       return last_char
    ```
   Пример:
   - Компьютер вывел город: Владимир
   - Вызываем функцию get_last_char, и получаем букву, на которую должны ввести свой город
     ```python3
     last_char = get_last_char("Владимир")
     ```
   
   Теперь можете реализовать остальные функции:
   - validate_user_city, функция отвечает за проверку того, правильный город назвал игрок или нет
   - get_computer_city, функция пытается найти подходящий город в списке городов для хода компьютера.
     внутри этой функции вы ищите город по заданным критериям и возвращаете его, либо возвращаете None,
     если не нашли ни одного подходящего города
   - write_answer, функция, которая записывает переданный в неё город в файл answers.txt
   - load_cities, функция принимает на вход имя файла с городами(у нас это cities.txt или answers.txt),
     и затем возвращает список городов из файла, пример кода для работы с файлом выше в п.1
   - game, основная функция игры

### Общий алгоритм игры:

- Из файла cities.txt считываются города и сохраняются в список, например `cities_list`
- Компьютер выбирает случайный город из списка городов `cities_list`,
  этот город записывается в переменную `current_city`, далее город записывается в файл `answers.txt `
  и выводится на экран, например: 

    `Ход компьютера: Омск`
- Затем игрок должен ввести свой город, город игрока нужно сохранить в переменную например `user_city`,
  после чего нужно проверить, правильный ли город ввёл игрок(УЧТИТЕ регистры городов!):
  - Город верный если:
    - Начинается на ту же букву, на которую оканчивается город компьютера, если город компьютера оканчивается
      на букву `"ь", "ъ", "ы"`, тогда город игрока должен начинаться на предпоследнюю букву.
      Пример:
      `Компьютер: Казань`  # последняя буква `ь`, значит берём предпоследнюю, это буква `н`

      `Игрок: Новгород`
    - Города нет в списке использованных городов(в файле answers.txt)
    - Город есть в списке городов `cities_list`(файл `cities.txt`)
  - Если город верный, то уже компьютер должен найти город на букву города игрока. 
    Для определения того, на какую букву искать город, можно использовать функцию `get_last_char`
    - Если компьютер смог найти город - печатаем его в консоль, записываем в `answers.txt` и продолжаем игру
    - Если компьютер не смог найти город - печатаем в консоль сообщение о том, что игрок победил и завершаем игру
  - Если игрок ввёл неправильный город, отнимаем `1 попытку`, затем проверяем, сколько попыток есть у игрока:
    - У игрока 0 попыток, завершаем игру
    - У игрока есть хотя бы `1 попытка` - продолжаем игру до тех пор, пока игрок либо не введёт верный город, либо 
      у него не закончатся попытки
- В конце игры очищаем файл `answers.txt`

