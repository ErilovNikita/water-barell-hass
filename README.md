# Проект для автоматизации работы резервуара с водой
Проект создан для автоматизаци контроля уровня воды в бочке, а так же для автоматического наполнения ее без участия человека.

## Оглавление
- [Аппаратная часть](#Аппаратная-часть)
    - [Материалы]
    - [Сборка]
- [Программая часть](#Программая-часть)
    - [Функциионал](#Функциионал)
    - [Прошивка](#Прошивка)
    - [Настройка Home Assistant](#Настройка-Home-Assistant)
- [Дополнительные скриншоты](#Дополнительные-скриншоты)

## Аппаратная часть
### Материалы
- Микроконтроллер Esp-01 x 1
- Ультазвуковой дальномер HC-SR04 x 1
- Резистор 10kOm x 1
- Конденцатор 10v 10uF x 2
- 4-х пиновые разьемы для удобства x 2
- Россыпь проводов

### Сборка 
![Схема подключения](/scheme/scheme2.png "Схема подключения")
Для удобства я нарисовал всю схему по поключению и разработал печатную плату на известном сайте

Для более удобной сборки можно воспользоваться grabber файлом из данного проекта 👉 [Gerber_water-barrel-hass.zip](/scheme/Gerber_water-barrel-hass.zip)

Если вы будете заказывать плату из репозитория вы получите что-то наподобии этого
![Готовая плата](/scheme/scheme1.png "Готовая плата")


## Программная часть

### Функциионал
- Есть управление минимального уровня воды
- Автоматическое управление насосами
- Контроль за уровнем воды
- Карточка для lovelace

### Прошивка
Программаная часть устройства строиться на базе прошивки [ESPHome](https://esphome.io)

Исходный код прошивки можно найти тут 👉 [water-barrel.yaml](/esphome/water-barrel.yaml)

Добавляем новый проект в ESPHome и прошиваем новое устройство

### Настройка Home Assistant
Нам необходимо добавить package с автоматизациями и дополнительными обьектами 
1. Скачиваем пакет [water-barrel-package.yaml](/homeassistant/water-barrel-package.yaml)
1. Добавляем его в папку */config/packages/*
1. Обязательно необходимо заменить переменные в коде на свои, для этого смотрим таблицу ниже 

    | Переменная в коде        | Описание            |
    |--------------------------|---------------------|
    | switch.pump              | Выключатель насосов |
    | sensor.ultrasonic_sensor | Ультразвуковой дальномер, имя обьекта из прошивки esp |


Для визуализиции данных необходимо вывести обьекты на lovelace, пример карточки можно увидеть чуть ниже

![](/screenshots/screen1.png)
``` yaml
type: vertical-stack
cards:
  - type: gauge
    entity: sensor.template_barrel_water_level
    name: Уровень воды
    needle: true
    severity:
      green: 80
      yellow: 20
      red: 0
  - type: entities
    entities:
      - entity: sensor.template_barrel
      - entity: switch.barrel_filling
      - entity: input_boolean.barrel_auto_level
      - entity: input_number.barrel_min_level

```

## Дополнительные скриншоты
![](/screenshots/screen2.png)