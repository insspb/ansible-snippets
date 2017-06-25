# Полезные шаблоны для Ansible
## Содержание
- [Введение](#Введение)
  - [О файлах примерах](#О-файлах-примерах)
  - [О версиях Ansible](#О-версиях-ansible)
- [Структура данных](#Структура-данных)
  - [Списки](#Списки)
  - [Словари (Хеш таблицы)](#Словари-Хеш-таблицы)
  - [Списки словарей](#Списки-словарей)
  - [Словари словарей](#Словари-словарей)
- [Циклы](#Циклы)
  - [with_items](#with_items)
    - [С простыми списками](#С-простыми-списками)
  - [with_dict](#with_dict)
- [Полезные ссылки](#Полезные-ссылки)
- [Благодарности](#Благодарности)

## Введение
Отчасти к счастью, отчасти к сожалению, но работа системного администратора фрилансера связана с огромным количеством технологий, меняющихся от проекта к проекту. С одной стороны, это позволяет держать свой мозг в тонусе. С другой стороны, иногда, возвращаясь к той или иной технологии, забываются уже давно изученные вещи. 
В данном документе и репозитории собраны некоторые примеры по работе с системой автоматизации Ansible. Возможно это поможет не только мне.

[⬆ Наверх](#Содержание)

### О файлах примерах
Во многих разделах идёт ссылка на файл пример из директории [examples](examples/). Все файлы примеры написаны с использованием модуля ``debug`` и просто работают с параметрами или синтаксисом, показывая возможное применение и возможные ошибки. Запуск этих файлов-примеров не приводит к каким-либо изменениям на локальной или удалённой системе.

Язык файлов примеров - английский. 

Примеры в самом тексте могут повторятся в нескольких разделах. Это сделано намеренно и позволяет разобрать примеры с разных сторон. Например, примеры в разделе [списки](#Списки) и [with_items](#with_items) одинаковые, но показывают работу разных вещей.

[⬆ Наверх](#Содержание)

### О версиях Ansible
Ansible проект с активной разработкой. Это значит, что некоторый старый синтаксис уже не работает в новых версиях. Данный документ написан и протестирован в Ansible версии 2.3.1.0.

[⬆ Наверх](#Содержание)

## Структура данных
Раздел структура данных подробно описывает различные стуктуры данных, используемые в Ansible. Раздел сильно связан с разделом [Циклы](#Циклы), рекомендую просмотреть оба раздела.

[⬆ Наверх](#Содержание)

### Списки
Списки это простейшая структура данных в Ansible. Задаётся путём присваиванию списку имени и перечислению самого списка с новой строки, через тире и пробел. Как и во всём yaml используются пробелы, размер и порядок отступов (пробелов) имеет значнение! Например, список устанавливаемого софта будет выглядеть следующим образом:
```yaml
soft:
  - htop
  - atop
  - tshark
  - mtr
```
Чаще всего используется с командой ``with_items``. В примере выше сам список будет иметь имя ``"{{ soft }}"``, а само указание на объект списка соответствует служебному ``"{{ item }}"``.
Практическое использование будет выглядеть следующим образом:
```yaml
- name: Utils | Ubuntu | Install basic utilities
  apt:
    pkg: "{{ item }}"
    state: present
  with_items: "{{ soft }}"
```
Мало деталей? Более подробно в [файле примере](examples/lists.yml).

В некоторых случаях вам может потребоваться записать список в виде строки. Это возможно. Список заключается в квадратные скобки, каждый член списка заключается в кавычки и пишется через запятую. В подобном синтаксисе наш список софта будет выглядеть следующим образом:
```yaml
soft: ['htop', 'atop', 'tshark', 'mtr']
```

[⬆ Наверх](#Содержание)

### Словари (Хеш таблицы)

[⬆ Наверх](#Содержание)

### Списки словарей

[⬆ Наверх](#Содержание)

### Словари словарей

[⬆ Наверх](#Содержание)

## Циклы

[⬆ Наверх](#Содержание)

### with_items

[⬆ Наверх](#Содержание)

#### С простыми списками
Команда ``with_items`` делает что-то для всего заданного списка. Список может задаваться непосредственно в команде или предварительно в переменных. 
Если задавать список непосредственно в команде, то код будет выглядеть следующим образом:
```yaml
- name: Utils | Ubuntu | Install basic utilities
  apt:
    pkg: "{{ item }}"
    state: present
  with_items:
    - htop
    - atop
    - tshark
    - mtr
```
Однако, такая конструкция используется достаточно редко, так как не позволяет выносить список в файл конфигурации и делать его уникальным для каждого хоста или группы. Значительно чаще используется конструкция с именнованным списком. В этом случае список задаётся в файле конфигурации хоста\группы\роли и имеет следующий формат:
```yaml
soft:
  - htop
  - atop
  - tshark
  - mtr
```
Тогда исполняемый код будет преобразован в следующий:
```yaml
- name: Utils | Ubuntu | Install basic utilities
  apt:
    pkg: "{{ item }}"
    state: present
  with_items: "{{ soft }}"
```

[⬆ Наверх](#Содержание)

### with_dict

[⬆ Наверх](#Содержание)

### Полезные ссылки

- [Официальная документация по Ansible (английский)](http://docs.ansible.com/ansible/)
- [Проверка yaml синтаксиса онлайн](http://www.yamllint.com/)
- [Проверка regex выражений онлайн](https://regex101.com/)
- [Онлайн редактор для файлов в формате markdown (md)](http://dillinger.io)

[⬆ Наверх](#Содержание)

## Благодарности



[⬆ Наверх](#Содержание)