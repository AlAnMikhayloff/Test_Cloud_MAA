# Test_Cloud_MAA
Тестовое задание

  # 1. Вопросы для разогрева
     
Ответы:
1. В процессе настраивания узла доступа к Интернет в 1994 году занимался задачами, связанными с обеспечением безопасности при доступе к серверу и работе сети. Они включали аудит безопасности, анализ угроз, защиту от атак и обеспечение безопасности приложений.
2. Не приходилось.
3. Такого опыта не было.
4. Хочу участвовать в стажировке, чтобы получить навыки в области безопасной разработки, изучить новые технологии и методики защиты, а также получить ценный опыт работы в профессиональной команде. Готов к обучению, развитию и применению полученных знаний на практике.

   # Часть 1. Security code review: GO

Ответы:
1. Уязвимости присутствующие в коде:

 а) Использование `http.Error` для обработки ошибок, которое не скрывает информацию о системе и может быть использовано злоумышленниками для проведения атак информационной безопасности (например, при атаках на CSRF).
 б) Использование сырых пользовательских данных в SQL-запросе без предварительной обработки, что создает риск SQL-инъекций.
 в) Необработка потенциальных ошибок при сканировании результата запроса из базы данных и конвертации его в строку.

2. Строки, в которых присутствуют уязвимости:
 а) Строка 27: использование `http.Error(w, "Query failed", http.StatusInternalServerError)` для обработки ошибок.
 б) Строка 32: формирование SQL-запроса без предварительной защиты от SQL-инъекций.
 в) Строка 40: отсутствие обработки ошибок при сканировании результата запроса.

3. Последствия эксплуатации уязвимостей:
 а) Атаки на CSRF и другие виды межсайтовых атак могут привести к утечке конфиденциальной информации или выполнению несанкционированных действий от имени пользователя.
 б) SQL-инъекции могут позволить злоумышленнику извлекать, изменять или удалять данные из базы данных.
 в) Необработанные ошибки при сканировании результата запроса могут привести к выходу приложения из строя или утечке информации.

4. Способы исправления уязвимостей:
 а) Заменить использование `http.Error` на более общий и менее информативный способ обработки ошибок.
 б) Использовать параметризованные запросы или функции предварительной очистки пользовательского ввода для SQL-запросов.
 в) Добавить обработку ошибок при сканировании результатов запроса из базы данных и принимать соответствующие меры, если возникнет ошибка.

5. Чтобы исправить уязвимости SQL-инъекций необходимо использование параметризованных запросов или функций очистки пользовательского ввода. Параметризованные запросы позволяют избежать внедрения вредоносного кода и обеспечивают безопасность при работе с базой данных. Применение этого метода позволит устранить риск SQL-инъекций и обеспечить безопасность при выполнении запросов в базу данных.

   # Часть 2: Security code review: Python

   Примет № 2.1.

    Ответы:

Код на Python содержит уязвимость типа **Injection** (конкретно **Server-Side Template Injection (SSTI)**).

1. Строки, в которых присутствуют уязвимости:
   - Строка 11: `output = Template('Hello ' + name + '! Your age is ' + age + '.').render()`

2. Последствиями эксплуатации уязвимости SSTI могут быть:
   - Злоумышленник может внедрить вредоносный код в шаблон и запустить его на сервере.
   - Утечка конфиденциальных данных, выполнение произвольного кода и даже полный контроль над сервером.

3. Способы исправления уязвимости SSTI:
   - Использование безопасных методов форматирования строк, таких как `format()` или f-строки в Python.
   - Если необходимо использовать шаблоны, следует использовать библиотеку шаблонов, которая предотвращает выполнение произвольного кода, например, Jinja2 с автоэкранированием переменных.
   
4. Лучший способ исправления уязвимости SSTI - использование безопасных способов форматирования строк или использование библиотек шаблонов, таких как Jinja2 с автоэкранированием переменных. Это позволит предотвратить выполнение произвольного кода в шаблонах и обеспечит безопасность при построении и рендеринге HTML-страниц.

Но, в данном конкретном случае можно просто изменить порядок условия в коде, чтобы исправить проблему.
1. Переместить строку 12 (`return output`) внутрь блока функции `page()`.
2. Проверить имя пользователя перед использованием в шаблоне для предотвращения уязвимости SSTI.


    Пример № 2.2.

Ответ:
1. Строки, в которых присутствуют уязвимости:
   - Строка 10: `cmd = 'nslookup ' + hostname'
   - Строка 11: `output = subprocess.check_output(cmd, shell=True, text=True)`

2. Последствиями эксплуатации уязвимости **Command Injection** могут быть:
   - Злоумышленник может выполнить произвольные команды на сервере, что может привести к компрометации сервера, утечке данных и другим критическим последствиям.

3. Способы исправления уязвимости **Command Injection**:
   - Избегать конкатенации команд с входными данными и использовать безопасные методы выполнения внешних команд.
   - Заменить `subprocess.check_output()` на `subprocess.run()` с передачей команды и аргументов как список.

4. В данном случае рекомендуется исправить уязвимость, заменив `subprocess.check_output()` на `subprocess.run()`. Это позволит передавать аргументы как список, что предотвращает возможность внедрения произвольных команд. Также важно проводить валидацию и фильтрацию входных данных для предотвращения уязвимости Command Injection.


   # 3. Моделирование угроз.

Ответы:
- Потенциальные проблемы безопасности

1. Недостаточная авторизация и аутентификация. Пользователи могут получить доступ к данным, к которым у них нет доступа, если они знают URL-адрес конечной точки API.
2. Уязвимости внедрения SQL-кода. Входные данные не проверяются должным образом, что может привести к внедрению SQL-кода и компрометации базы данных.
3. Уязвимости межсайтового скриптинга (XSS). Входные данные не экранируются должным образом, что может привести к внедрению вредоносного кода в приложение.
4. Уязвимости подделки межсайтовых запросов (CSRF). Пользователи могут быть Уловки для выполнения вредоносных действий, таких как смена паролей или раскрытие конфиденциальной информации.
5. Небезопасная конфигурация сервера. Сервер может быть настроен неправильно, что может привести к его компрометации.

- Последствия эксплуатации проблем

Эксплуатация этих проблем может привести к следующим последствиям:

1. Несанкционированный доступ к данным. Злоумышленники могут получить доступ к конфиденциальным данным, таким как номера кредитных карт, адреса и пароли.
2. Уничтожение или изменение данных. Злоумышленники могут удалить или изменить данные, что может привести к потере данных или нарушению работы приложения.
3. Отказ в обслуживании (DoS). Злоумышленники могут перегрузить сервер, что сделает его недоступным для пользователей.
4. Выполнение произвольного кода. Злоумышленники могут выполнить произвольный код на сервере, что может привести к компрометации всего приложения.

- Способы исправления уязвимостей и смягчения рисков

Для исправления уязвимостей и смягчения рисков можно предложить следующие меры:

1. Внедрить строгую авторизацию и аутентификацию. Убедитесь, что пользователи могут получить доступ только к тем данным, к которым у них есть доступ.
2. Проверять входные данные на наличие вредоносного кода. Перед сохранением или обработкой данных убедитесь, что они не содержат вредоносного кода.
3. Использовать межсетевой экран (WAF). WAF может помочь заблокировать вредоносный трафик, направляемый на ваше приложение.
4. Регулярно обновлять программное обеспечение. Убедитесь, что ваше программное обеспечение обновлено до последней версии, чтобы исправить любые известные уязвимости.
5. Проводить аудит безопасности. Регулярно проводите аудит безопасности вашего приложения, чтобы выявлять и устранять любые уязвимости.

- Уточняющие вопросы к разработчикам сервиса

1. Какие меры безопасности были приняты для защиты приложения от несанкционированного доступа?
2. Как данные защищены от утечки и несанкционированного доступа?
3. Как приложение защищено от атак типа "отказ в обслуживании"?
4. Как приложение защищено от внедрения SQL-кода и других уязвимостей?
5. Какие процедуры используются для обеспечения безопасности данных?

