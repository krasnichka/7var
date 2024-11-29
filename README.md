
```
@startuml
actor "Руководитель" as Manager
actor "Исполнитель" as Worker
actor "Система" as System

Manager --> (Создание задания)
Manager --> (Просмотр статуса задания)
Worker --> (Просмотр задания)
Worker --> (Регистрация времени)
System --> (Обработка времени)

(Создание задания) .> (Просмотр задания) : include
(Просмотр статуса задания) .> (Регистрация времени) : extend

@enduml
```

![[Pasted image 20241129150859.png]]


```
@startuml

package models {
  class Task {
    - id: int
    - description: String
    - status: String
    - assigned_to: User
    + createTask(): void
    + updateStatus(): void
    + assignToUser(): void
  }

  class User {
    - id: int
    - name: String
    - role: String
    + login(): void
    + logout(): void
    + viewTasks(): void
  }

  class TimeEntry {
    - id: int
    - task_id: int
    - user_id: int
    - start_time: DateTime
    - end_time: DateTime
    - duration: int
    + createEntry(): void
    + editEntry(): void
    + viewEntries(): void
  }
}

package reports {
  class Report {
    - id: int
    - user_id: int
    - task_id: int
    - total_time: int
    + generateReport(): void
    + viewReport(): void
  }
}

package services {
  class TaskService {
    + createTask(): void
    + updateStatus(): void
  }
  class TimeEntryService {
    + createEntry(): void
    + generateReport(): void
  }
}

' Relationships
Task --> User : assigns
TimeEntry --> Task : aggregates
TimeEntry --> User : aggregates
Report --> Task : depends
Report --> User : depends

@enduml
```
- **Task** (Задание): хранит информацию о задании, статусе и пользователе, которому оно назначено.
- **User** (Пользователь): хранит информацию о пользователях (менеджерах, исполнителях), их ролях и задачах.
- **TimeEntry** (Запись времени): хранит данные о времени, затраченном на выполнение задания.
- **Report** (Отчет): генерирует отчет о времени, затраченном на задания, для конкретного пользователя.
- **TaskService** и **TimeEntryService**: сервисы для работы с задачами и временем.

##### Связи между классами:
- **Task** зависит от **User**: задание связано с пользователем (оно назначается конкретному пользователю).
- **TimeEntry** агрегирует **Task** и **User**: каждая запись времени связана с конкретной задачей и пользователем.
- **Report** зависит от **Task** и **User**: отчет формируется для задания и пользователя.



![[Pasted image 20241129164920.png]]


Диаграмма последовательности для **Создания задания** (Manager)
**Менеджер** (Руководитель) входит в систему, создает задание, которое система сохраняет и подтверждает создание.
```
@startuml
actor Manager
entity System
entity Task

Manager -> System : Войти в систему
System -> Manager : Успешный вход
Manager -> Task : Создать новое задание
Task -> System : Сохранить задание
System -> Manager : Подтвердить создание задания
@enduml

```

![[Pasted image 20241129164225.png]]


Диаграмма последовательности для **Просмотра статуса задания** (Manager)
**Менеджер** просматривает статус задания, система обрабатывает запрос и возвращает актуальный статус.
```
@startuml
actor Manager
entity System
entity Task

Manager -> System : Войти в систему
System -> Manager : Успешный вход
Manager -> Task : Просмотреть статус задания
Task -> System : Запросить статус
System -> Task : Возвращение статуса задания
System -> Manager : Отобразить статус задания
@enduml
```

![[Pasted image 20241129164452.png]]


Диаграмма последовательности для **Просмотра задания** (Worker)
**Исполнитель** входит в систему, просматривает задание, которое возвращает система.
```
@startuml
actor Worker
entity System
entity Task

Worker -> System : Войти в систему
System -> Worker : Успешный вход
Worker -> Task : Просмотреть задание
Task -> System : Запросить задание
System -> Task : Возвращение задания
System -> Worker : Отобразить задание
@enduml
```

![[Pasted image 20241129164604.png]]


Диаграмма последовательности для **Регистрации рабочего времени** (Worker)
**Исполнитель** выбирает задание, вводит время, система сохраняет запись времени.
```
@startuml
actor Worker
entity System
entity TimeEntry
entity Task

Worker -> System : Войти в систему
System -> Worker : Успешный вход
Worker -> Task : Выбрать задание
Task -> Worker : Отобразить задание
Worker -> TimeEntry : Ввести время начала и конца
TimeEntry -> System : Сохранить запись времени
System -> Worker : Подтвердить сохранение записи
@enduml
```

![[Pasted image 20241129164659.png]]


Диаграмма последовательности для **Обработки времени** (System)
**Система** обрабатывает запись времени, обновляет информацию о задаче.
```
@startuml
actor System
entity TimeEntry
entity Task

System -> TimeEntry : Запросить данные записи времени
TimeEntry -> System : Отправить данные записи
System -> Task : Обработать данные по времени
System -> System : Обновить статус задания
@enduml
```

![[Pasted image 20241129164803.png]]

