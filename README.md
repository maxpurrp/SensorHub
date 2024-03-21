# Проект Автоматизации с использованием Golang и Arduino

Этот проект состоит из нескольких частей:

## Сервер на Golang
Сервер написан на языке программирования Golang. Он считывает данные с Arduino при помощи интерфейса UART.

## Устройство Arduino
Arduino устройство имеет следующие компоненты:
- Считыватель RFID карт
- Реле
- Датчик воды

## Протокол передачи данных
Разработан и реализован протокол передачи данных между сервером на Golang и устройством Arduino. Данный протокол содержит в себе следующие характеристики:
1. STARTBYTE - стартовый байт, после которого все последующие байты несут в себе полезную информацию
2. COMMAND - Команда, в этот байт записано значение, в зависимости от которого будет активирован один из вышеперечисленных компонентов.
3. LENMSG - количество байт от сообщения
4. MSG - Хранит в себе полезную информацию для компонентов.
5. CRC - Хранит в себе значение, которое используется для проверки целостности данных.
6. ENDBYTE - Последний байт, который позволяет при считывании данных из буфера обозначить конец данных.

## Используемые технологии
- Golang
- Postgres
- Docker

## Установка

Arduino:
Для сборки нужна Arduino uno. Если у вас другая модель - обратите внимание на то, что подключение различных компонентов может различаться.
1. RFID Считыватель :
``` MFRC522 Arduino  Reader/PCD Uno/101

Pin  				Pin
Reader/PCD Uno/101	MFRC522 Arduino
---------------------

RST 				9
SDA(SS) 			10
MOSI 				11
MISO 				12
SCK 				13
VСС                            3.3V
GND 				GND 
```
2. Реле
```
GND - GND
VCC - 5V
IN  - 2
```
3. Датчик Воды
```
IN  - A0
VCC - 5V
GND - GND
```

После подключения всех компонентов к плате - загрузите в нее скетч с помощью Arduino IDE, который лежит в корневой директории.
Сервер:
1. git clone ```https://github.com/maxpurrp/SensorHub```
2. ```docker build -t postgres .```
3. ```docker run -d -p 5432:5432 postgres ```
4. ```go run /.main.go```

При первичном прикладывании карты к считывателю вы должны зайти на сайт и выбрать команду для карты. Пока что доступно 2:
5. Активация реле на выбираемый вами отрезок времени. Можно использовать там, где требуется автоматическое управление устройствами или системами в соответствии с определенным расписанием или условиями работы.
6. Включение датчика обнаружения воды.  Может использоваться для обнаружения протечек, контроля воды и т.д.
