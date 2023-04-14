# Задание 1
Есть скрипт:

![sript](https://github.com/EVolgina/devops-netology16/blob/main/skropt1.PNG)

Ответ:\
Какое значение будет присвоено переменной c? - TypeError: unsupported operand type(s) for +: 'int' and 'str'\
Как получить для переменной c значение 12? - необходимо изменить тип a на str

![dz1](https://github.com/EVolgina/devops-netology16/blob/main/zd1-1.PNG)

Как получить для переменной c значение 3? - необходимо поменять тип b на int

![dz1-2](https://github.com/EVolgina/devops-netology16/blob/main/zd-2.PNG)

# Задание 2
Мы устроились на работу в компанию, где раньше уже был DevOps-инженер. Он написал скрипт, позволяющий узнать, какие файлы модифицированы в репозитории относительно локальных изменений. Этим скриптом недовольно начальство, потому что в его выводе есть не все изменённые файлы, а также непонятен полный путь к директории, где они находятся.
Как можно доработать скрипт ниже, чтобы он исполнял требования вашего руководителя?

![zd2](https://github.com/EVolgina/devops-netology16/blob/main/zd2.PNG)

Ваш скрипт:
![skr](https://github.com/EVolgina/devops-netology16/blob/main/zadane%202.PNG)

Вывод скрипта при запуске во время тестирования:
![vivod]()


# Задание 3
Доработать скрипт выше так, чтобы он не только мог проверять локальный репозиторий в текущей директории, но и умел воспринимать путь к репозиторию, который мы передаём, как входной параметр. Мы точно знаем, что начальство будет проверять работу этого скрипта в директориях, которые не являются локальными репозиториями.

Ваш скрипт:

![]()

Вывод скрипта при запуске во время тестирования:
![zd3vivod](https://github.com/EVolgina/devops-netology16/blob/main/zd3vivod.PNG)

#Задание 4
Наша команда разрабатывает несколько веб-сервисов, доступных по HTTPS. Мы точно знаем, что на их стенде нет никакой балансировки, кластеризации, за DNS прячется конкретный IP сервера, где установлен сервис.
Проблема в том, что отдел, занимающийся нашей инфраструктурой, очень часто меняет нам сервера, поэтому IP меняются примерно раз в неделю, при этом сервисы сохраняют за собой DNS-имена. Это бы совсем никого не беспокоило, если бы несколько раз сервера не уезжали в такой сегмент сети нашей компании, который недоступен для разработчиков.\
Мы хотим написать скрипт, который:\
опрашивает веб-сервисы;\
получает их IP;\
выводит информацию в стандартный вывод в виде: <URL сервиса> - <его IP>.\
Также должна быть реализована возможность проверки текущего IP сервиса c его IP из предыдущей проверки. Если проверка будет провалена — оповестить об этом в стандартный вывод сообщением: [ERROR] <URL сервиса> IP mismatch: <старый IP> <Новый IP>. Будем считать, что наша разработка реализовала сервисы: drive.google.com, mail.google.com, google.com.\

Ваш скрипт:

![]()

Вывод скрипта при запуске во время тестирования:
![]()
