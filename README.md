Создание онлайн-интерпретатора Python с использованием WebAssembly (Wasm) — это интересный проект, который позволяет запускать Python-код в браузере. Для этого можно использовать проекты, такие как `Pyodide` или `Brython`, которые уже предоставляют готовые решения для запуска Python в браузере с использованием WebAssembly.

В этом руководстве мы рассмотрим, как создать простой онлайн-интерпретатор Python с использованием `Pyodide`, который компилирует CPython в WebAssembly.

### Шаги для создания онлайн-интерпретатора Python с использованием Pyodide:

#### Шаг 1: Подготовка проекта

1. **Создайте папку для проекта**:
   ```bash
   mkdir python-interpreter
   cd python-interpreter
   ```

2. **Создайте структуру проекта**:
   ```bash
   mkdir public
   ```

#### Шаг 2: Загрузка Pyodide

1. **Скачайте Pyodide**:
   Перейдите на [официальный сайт Pyodide](https://pyodide.org/en/stable/usage/downloading-and-deploying.html) и скачайте последнюю версию Pyodide. Распакуйте скачанный архив в папку `public`.

2. **Создайте HTML-файл**:
   Создайте файл `index.html` в папке `public` со следующим содержимым:

   ```html
   <!DOCTYPE html>
   <html lang="en">
   <head>
       <meta charset="UTF-8">
       <title>Online Python Interpreter</title>
       <script src="pyodide/pyodide.js"></script>
   </head>
   <body>
       <h1>Online Python Interpreter</h1>
       <textarea id="code" rows="10" cols="50">print("Hello, World!")</textarea>
       <br>
       <button id="run">Run</button>
       <pre id="output"></pre>

       <script>
           async function main() {
               // Загрузка Pyodide
               let pyodide = await loadPyodide();

               // Обработчик нажатия кнопки "Run"
               document.getElementById('run').addEventListener('click', () => {
                   const code = document.getElementById('code').value;
                   try {
                       // Выполнение Python-кода
                       pyodide.runPython(code);
                       // Вывод результата
                       document.getElementById('output').innerText = pyodide.runPython(`
                           import sys
                           sys.stdout.getvalue()
                       `);
                   } catch (err) {
                       // Вывод ошибки
                       document.getElementById('output').innerText = err;
                   }
               });
           }
           main();
       </script>
   </body>
   </html>
   ```

#### Шаг 3: Запуск проекта

1. **Запустите простой HTTP-сервер**:
   Для тестирования проекта можно использовать простой HTTP-сервер. Например, с помощью Python:

   ```bash
   cd public
   python -m http.server
   ```

2. **Откройте браузер**:
   Перейдите по адресу `http://localhost:8000` в браузере. Вы увидите онлайн-интерпретатор Python. Введите Python-код в текстовое поле и нажмите кнопку "Run", чтобы выполнить код.

### Дополнительные улучшения

- **Автодополнение и подсветка синтаксиса**: Используйте библиотеки, такие как `CodeMirror` или `Monaco Editor`, для добавления автодополнения и подсветки синтаксиса в текстовое поле.
- **Управление зависимостями**: Pyodide поддерживает установку пакетов Python через `micropip`. Вы можете добавить возможность установки пакетов прямо в интерфейсе.
- **Обработка ввода/вывода**: Улучшите обработку ввода/вывода, чтобы пользователи могли взаимодействовать с интерпретатором через консоль.

### Пример с использованием CodeMirror:

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Online Python Interpreter</title>
    <script src="pyodide/pyodide.js"></script>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/codemirror/5.62.0/codemirror.min.css">
    <script src="https://cdnjs.cloudflare.com/ajax/libs/codemirror/5.62.0/codemirror.min.js"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/codemirror/5.62.0/mode/python/python.min.js"></script>
</head>
<body>
    <h1>Online Python Interpreter</h1>
    <textarea id="code" rows="10" cols="50">print("Hello, World!")</textarea>
    <br>
    <button id="run">Run</button>
    <pre id="output"></pre>

    <script>
        async function main() {
            // Загрузка Pyodide
            let pyodide = await loadPyodide();

            // Инициализация CodeMirror
            let editor = CodeMirror.fromTextArea(document.getElementById('code'), {
                mode: 'python',
                lineNumbers: true
            });

            // Обработчик нажатия кнопки "Run"
            document.getElementById('run').addEventListener('click', () => {
                const code = editor.getValue();
                try {
                    // Выполнение Python-кода
                    pyodide.runPython(code);
                    // Вывод результата
                    document.getElementById('output').innerText = pyodide.runPython(`
                        import sys
                        sys.stdout.getvalue()
                    `);
                } catch (err) {
                    // Вывод ошибки
                    document.getElementById('output').innerText = err;
                }
            });
        }
        main();
    </script>
</body>
</html>
```

Теперь у вас есть базовый онлайн-интерпретатор Python, работающий в браузере с использованием WebAssembly. Вы можете расширять и улучшать этот проект, добавляя новые функции и интеграции.
