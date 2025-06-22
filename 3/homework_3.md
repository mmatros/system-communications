# домашняя работа #3

добавлю до 23 июня

## схема события

    {
    "$schema": "https://json-schema.org/draft/2020-12/schema",
    "$id": "https://hcb.com/datareplication/manager.schema.json",
    "title": "Manager",
    "description": "A manager of HCB",
    "type": "object",
    "properties": {
        "id": {
            "description": "The unique identifier for a manager",
            "type": "string",
            "pattern": "[0-9a-f]{8}-[0-9a-f]{4}-[0-9a-f]{4}-[0-9a-f]{4}-[0-9a-f]{12}"
        },
        "name": {
            "description": "Name of the manager",
            "type": "string"
        },
    },
    "required": [ "id", "name" ]
    }
    
    {
    "$schema": "https://json-schema.org/draft/2020-12/schema",
    "$id": "https://hcb.com/domain/candidates/task-complete.schema.json",
    "title": "TaskCompletedSuccessfully",
    "description": "Task was completed successfully",
    "type": "object",
    "properties": {
        "id": {
            "description": "The unique identifier for an event",
            "type": "string",
            "pattern": "[0-9a-f]{8}-[0-9a-f]{4}-[0-9a-f]{4}-[0-9a-f]{4}-[0-9a-f]{12}"
        },
        "candidate_id": {
            "description": "Identifier of the candidate",
            "type": "string"
        },
        "task_id": {
            "description": "Identifier of the task",
            "type": "string"
        }, 
    },
    "required": [ "id", "candidate_id", "task_id" ]
    }

## процесс миграции

### COMM-040 sync -> async

1. Добавим новое событие в schema registry
2. Добавим в сервис бонусов (консьюмер) бизнес логику чтения события
3. Добавим логику записи в брокер
4. Удалим синхронный вызов

### COMM-060 async -> sync

1. Добавим новый handler для обработки синхронных запросов
2. Отключаем отправку событий
3. Ждем пока события закончатся
4. Используем новый handler
5. Удаляем старый producer&consumer

## описание способов решения проблем вокруг зачисления и списания средств

1. Для того чтобы не потерять события на стороне сервиса бонусов (консьюмера) из-за ошибок будем иcпользовать DLQ.
2. Для того чтобы избежать дублирования отправки событий нас зачисление бонусов на счет
будем использовать паттерн outbox - для этого добавим отдельную таблицу в БД.
3. Для повышения вероятности доставки событий, добавим копирование во все реплики для подтверждением получения события брокером.
