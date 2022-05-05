# SauresHA
[![hacs_badge](https://img.shields.io/badge/HACS-Custom-orange.svg)](https://github.com/custom-components/hacs)
[![GitHub release (latest by date)](https://img.shields.io/github/v/release/volshebniks/sauresha)](https://github.com/volshebniks/sauresha/releases)
![GitHub Release Date](https://img.shields.io/github/release-date/volshebniks/sauresha)
[![GitHub](https://img.shields.io/github/license/volshebniks/sauresha)](LICENSE)

[![Maintenance](https://img.shields.io/badge/Maintained%3F-Yes-brightgreen.svg)](https://github.com/volshebniks/sauresha/graphs/commit-activity)
[![GitHub issues](https://img.shields.io/github/issues/volshebniks/sauresha)](https://github.com/volshebniks/sauresha/issues)

Пожертвование на развитие проекта  [Яндекс.Деньги](https://money.yandex.ru/to/41001566881198)

## Update 6: версия 1.0.0:
 * !!! при возникновении проблем создаейте GitHub issues и сделайте откат на предыдущую версию!
 * сделан глобальный рефакторинг кода
 * все настройки теперь в одном месте, без разделения на платформы
 * для начала работы требуется минимум настроек
 * количество обращений к серверу Saures, сведено к минимуму
 * добавлено управление кранами в виде switch. Не тестировалось! 
 * обязательное задание серийных номеров не требуется
 * имена устанавливаются в личном кабинете Saures

## Update 5: Начиная с версии 0.6: 
 * значительно изменен механизм настройки
 * можно задавать свои мена для всего
 * можно в настройках делать ссылки на !secret
 * добавил в manifest, version

## Update 4: Начиная с версии 0.5:
 * Существенно сокращено кол-во обращений к серверу Saures, для предотвращения блокировки.
   <br />Рекомендую в настройках указать:
```yaml
  scan_interval:
    minutes: 30 
```
   Иначе могут быть блокировки в будущем.

## Update 3: версия 0.3.8:
 * вернул синхронные вызовы

## Update 2: версия 0.3.5:
 * уменьшено количество вызово API
 * ускорена первоначальная инициализация модуля
 * сделан переход на асинхронные методы
 * исправлена ошибка с заднием своего scan_interval
 * исправлена ошибка связанная с наличием русских букв в серийных номерах
 * на стороне Saures явно починили кеширование для нового API

## Update 1: Начиная с версии 0.3: 
 * сделан переход на новое клиентское API
 * добавлена необязательная настройка для sensor - scan_interval. Время обновления в минутах. По умолчанию = 10 минут.
 * из-за перехода появились новые атрибуты у сенсоров.

## Содержание

* [Установка](#устнановка)
  * [Ручная установка](#ручная-установка)
  * [Установка через HACS](#hacs_установка)
* [Настройка](#как-использовать)
  * [Параметры](#Параметры)



Для связи: <master@g-s-a.me>

Интеграция котроллеров [Saures](https://www.saures.ru) c [Home Assistant](https://www.home-assistant.io/)
# Описание

В настоящее время поддерживаются следующие типы устройств от Saurus
1. Счетчик холодной воды (м³) = sensor в Home Assistant
2. Счетчик горячей воды (м³) = sensor в Home Assistant
3. Счетчик газа (м³) = sensor в Home Assistant
4. Датчик протечки (0 – нет протечки, 1 - протечка) = binary_sensor в Home Assistant
5. Датчик температуры (градусы) = sensor в Home Assistant
6. Электро-шаровой кран управление (0 – открыться, 1 - закрыться) -switch в Home Assistant, не тестировалось
7. Счетчик тепла (кВт*ч) = sensor в Home Assistant
8. Счетчик электричества (кВт*ч) (в том числе многотарифные) = sensor в Home Assistant
9. Сухой контакт (0 – деактивирован, 1 – активирован) = binary_sensor в Home Assistant
10. Электро-шаровой кран состояние (0 – не подключен модуль, 1 – неизвестное состояние, 2 – открыт, 3 - закрыт) = sensor в Home Assistant
11. Непосредственно сами контроллеры = sensor в Home Assistant

## Установка

### Ручная установка

1. Добавляем компонент в Home Assistant
Распаковываем архив. Папку sauresha берем целиком и копируем в custom_components.
2. Осуществляем конфигурацию компонента в Home Assistant.
- email и password - Ваши учетные данные личного кабинета на saures.ru.
- flats (как заполнить указано ниже)
3. Перезагружаем HA

### HACS установка

1. Убедитесь, что [HACS](https://custom-components.github.io/hacs/) уже устновлен.
2. Перейдите на закладку SETTINGS
3. Введите https://github.com/volshebniks/sauresha   и выберите категорию Integration, нажмите Сохранить
4. Новый репозиторий Integration Saures controllers with HA будет добавлен на закладке Integration
5. Устновите SauresHA из него 
3. Настройте интеграцию в configuration.yaml
4. Перезапустите HA.

## Как использовать

### Параметры

Это добавляем в configuration.yaml
```yaml
sauresha:
  email: !secret saures_login
  password: !secret saures_pass
  flats:  6716   # необязательное,  можно несколько, через запятую
  scan_interval: # необязательное
    minutes: 30  
```

* Важно: Если не знаете как в личном кабине увидеть flat_id - высвечивается при наведении на адресс в адресной сроке, то вводите flat_id : 0. При наличии у вас только одного созданного адреса, работать будет и так. Но все же рекомендую - его узнать и заполнить - будет меньше запросов к API saures.

Узнать flat_id (усли указан flat_id: 0) можно найдя в логе Home Assistant, строки вида:
![Лог](https://github.com/volshebniks/images/raw/master/log_saures.jpg)
<br />
Цифра в конце, после двоеточия и есть необходимый flat_id

# План развития проекта
- [X] Добавить проект в HACS
- [ ] Сделать сенсоры для счетчиков с показаниями за день/месяц/год
- [X] Добавить управление кранами
- [ ] Сделать pallete для Node-Red
- [ ] Сделать полноценную интеграцию с Home Assistant (добавляется в раздел интеграции)

# Credits

Большое спасибо следующим организациям и проектам, работа которых имеет важное значение для развития проекта:

Нет их пока :)

----------------------------------------------------------------------------------------------------------------------------------
Пожертвование на развитие проекта  [Яндекс.Деньги](https://money.yandex.ru/to/41001566881198)
