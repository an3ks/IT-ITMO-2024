
# Информатика ИТМО. Авдиенко Данила

## **Задание 1: Автоматизация проверки файлов при коммите**

1. Я перешел в папку **`.git/hooks`** и создал файл **`pre-commit`**.
2. Добавил в файл следующий **Bash-скрипт**:

   ```bash
   #!/bin/bash

   # Pre-commit hook: проверка .txt файлов на наличие "Author:" строки

   error_found=0

   # Получаем список всех .txt файлов, добавленных в коммит
   files=$(git diff --cached --name-only --diff-filter=ACM | grep '\.txt$')

   # Если .txt файлов нет — завершаем скрипт
   if [[ -z "$files" ]]; then
       echo "No .txt files to check."
       exit 0
   fi

   # Проверка содержимого каждого .txt файла
   for file in $files; do
       if [[ -f "$file" ]]; then
           if ! grep -q "Author:" "$file"; then
               echo "Error: File '$file' does not contain 'Author:' line."
               error_found=1
           fi
       fi
   done

   # Завершаем коммит с ошибкой при проблемах
   if [[ $error_found -eq 1 ]]; then
       echo "Pre-commit check failed. Add 'Author:' to the .txt files and try again."
       exit 1
   else
       echo "All .txt files passed the pre-commit check."
   fi
   ```

3. Скрипт проверяет все **`.txt` файлы**, добавленные в коммит:
   - Если в файле отсутствует строка **`Author:`**, коммит прерывается.
   - Если проверки пройдены, коммит успешно выполняется.

4. **Результат работы**:
   - На скриншоте видно, что коммит файла **example.txt** без строки `Author:` был отклонен с сообщением об ошибке.
   - После добавления строки `Author:`, коммит прошёл успешно.

---

## **Задание 2: Использование Git Flow в проекте**

1. **Инициализация Git Flow**:  
   Я выполнил команду:
   ```bash
   git flow init
   ```
   - В качестве ветки для релизов выбрал **`main`**.
   - В качестве ветки для разработки выбрал **`develop`**.

2. **Создание новой фичи**:
   Я создал ветку для новой функциональности **`task-management`**:
   ```bash
   git flow feature start task-management
   ```

3. **Внесение изменений и завершение фичи**:
   - Создал файл и добавил изменения.  
   - Сделал коммит изменений:
     ```bash
     git add task_manager.py
     git commit -m "Добавлен функционал управления задачами"
     ```
   - Завершил работу над фичей и слил её с веткой **`develop`**:
     ```bash
     git flow feature finish task-management
     ```

4. **Создание релиза**:
   - Перешел на ветку **`develop`** и начал создание релиза **`v1.0.0`**:
     ```bash
     git flow release start v1.0.0
     ```
   - Обновил файл версии и сделал коммит:
     ```bash
     echo "v1.0.0" > version.txt
     git add version.txt
     git commit -m "Обновлена версия для релиза v1.0.0"
     ```
   - Завершил релиз и слил изменения в ветки **`develop`** и **`main`**:
     ```bash
     git flow release finish v1.0.0
     ```

5. **Hotfix**:
   - Создал ветку для исправления ошибки:
     ```bash
     git flow hotfix start hotfix-1.0.1
     ```
   - Создал файл и закоммитил его:
     ```bash
     git add file_with_error.py
     git commit -m "Исправлена критическая ошибка"
     ```
   - Завершил hotfix:
     ```bash
     git flow hotfix finish hotfix-1.0.1
     ```

6. **Отправка изменений на удалённый репозиторий**:
   Отправил ветки **`develop`** и **`main`** на удалённый репозиторий:
   ```bash
   git push origin develop
   git push origin main
   ```
