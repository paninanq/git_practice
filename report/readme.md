### Отчет по лабораторной работе №5

## Шаг 1
Для начала я создала новый репозиторий inf_lab5 на Github и склонировала его. После я создала файл example.txt:
```
Мат анализ
Глава 1: Введение
Глава 2: Мат индукция
Глава 3: Последовательности
Глава 4: Пределы 
Глава 5: Производная и дифференциял
```

Далее запушила его в репозиторий 

```
git add example.txt
git commit -m "File added example.txt"
git push origin main
```

После я создала новую ветку и переключилась на неё

```
git branch feature-branch
git checkout feature-branch
```

Я изменила текст в файл example.txt:

```
Мат анализ
Глава 1: Введение
Глава 2: Мат индукция
Глава 3: Последовательности
Глава 4: Пределы 
Глава 5: Производная и дифференциял
Глава 6: Ряды Тейлора
```
И снова запушила его 
```
git add example.txt
git commit -m "File edited example.txt"
git push origin feature-branch
```

Потом я переключилась на основную ветку и слила изменения из ветки feature-branch в основную ветку:
```
git checkout main
git merge feature-branch
git push origin main
```
![exdoc](/report/exdoc.png)

Все получилось) на основной ветке файл слияния веток!

## Шаг 2
Создадим файл book.txt 
```
Название книги

Глава 1: Введение
Здесь будет введение в тему книги

Глава 2: Основы гит
Основные понятия и команды гит
```

Создадим ветку feature-login и переключимся на нее

```
git checkout -b feature-login
```
Изменим файл book.txt
```
Название книги

Глава 1: Введение
Здесь будет введение в тему книги

Глава 2: Основы гит
Основные понятия и команды гит

Глава 3: Вход в систему
Раздел по новой функциональности входа в систему.
```
Запушим его в feature-login
```
git add book.txt
git commit -m "Добавлена глава 3: Вход в систему"
git push origin feature-login
```
Снова переключимся на main создадим такой же файл book.txt, только добавим название
```
Название книги: Приключения в мире ГИТ

Глава 1: Введение
Здесь будет введение в тему книги

Глава 2: Основы гит
Основные понятия и команды гит

Глава 3: Вход в систему
Раздел по новой функциональности входа в систему.
```

Закоммитим и запушим изменения в main

Теперь смоделируем конфликт.

Переключимся на ветку feature-login и изменим в файле book.txt 2 главу:
```
Название книги: Приключения в мире ГИТ

Глава 1: Введение 
Здесь будет введение в тему книги

Глава 2: Основы гит и магия конфликтов
Основные  понятия и команды гит, а также волшебство разрешения конфликтов

Глава 3: Вход в систему
Раздел по функциональности входа в систему
```
Закоммитим и запушим изменения.

Теперь снова переключаемся на main и пробуем слить изменения:
```
git checkout main
git merge feature-login
```
Однако мы получаем конфликт:

![image](/report/conflict.png)

Изменим файл book.txt

![image](/report/p7.png)
![image](/report/p8.png)

Следующая попутка слить ветки успешна!! конфликт решен

## Шаг 3
Этот шаг будем выполнять в новом репозитории git-practice
Склонируем репозиторий и перейдем в него.

Пусть txt файл будет подходить под формат, если в нем содержиться больше или равно одного слова Chapter

Напишем bash-script check_format.sh
```
#!/bin/bash
name=$1
if [[ ${name##*.} == "txt" ]]; then
    cnt=$( grep -e "Chapter" -c $1 )
    if [[ $cnt>0 ]]; then
        echo 0
    else
        echo 1
    fi
else
    echo 0
fi
```
Копируем его в .git/hooks/pre-commit и назначем права доступа:
```
cp check_format.sh .git/hooks/pre-commit
chmod +x .git/hooks/pre-commit
```
Создадим файл text.txt
```
blablablablablabla
```
Попробуем закомитить его и отправить на Github

![image](/report/conf_scr.png)

Изменим файл text.txt
```
Chapter 1: blablablablablabla
```
Снова попробуем закомитить его и отправить на Github
![image](/report/without_conf_scr.png)

Конфликт разрешен!!!
## Шаг 4
Использовать Git Flow будем в репозитории git_flow_lab

Склонируем репозиторий и перейдем в него.
Инициилизируем Git Flow в корне репозитория
```
git flow init
```
Создадим ветку для новой функциональности task-management:
```
git flow feature start task-management
```
Создадим файл task_manager.py
```
def create_task(title, description):
    print(f"Создана новая задача: {title}")
```
Выполним коммит по мере разработки
```
git add task_manager.py
git commit -m "New functional added"
```
Завершим фичу:
```
git flow feature finish task-management
```
![image](/report/git_fl.png)

Git flow автоматически слил эту ветку с веткой develop, так как конфликтов не возникло.
Переключимся на ветку develop и начнем создание релиза
```
git checkout develop
git flow release start v1.0.0
```
Обновим версию в файле version.txt
```
echo "v1.0.0" > version.txt
git add version.txt
git commit -m "Version release updated v1.0.0"
```
Завершим релиз и объединим его с ветками "develop" и "main"
```
git flow release finish v1.0.0
```
![image](/report/flow_cont.png)

В заключение отправим изменения на удаленный репозиторий
```
git push origin develop
git push origin main
```
![image](/report/end.png)

Таким образом, все поставленные задачи выполнены.
