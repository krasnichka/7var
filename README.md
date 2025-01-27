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


**Task** (Задание): хранит информацию о задании, статусе и пользователе, которому оно назначено.
- **User** (Пользователь): хранит информацию о пользователях (менеджерах, исполнителях), их ролях и задачах.
- **TimeEntry** (Запись времени): хранит данные о времени, затраченном на выполнение задания.
- **Report** (Отчет): генерирует отчет о времени, затраченном на задания, для конкретного пользователя.
- **TaskService** и **TimeEntryService**: сервисы для работы с задачами и временем.



##### Связи между классами:
- **Task** зависит от **User**: задание связано с пользователем (оно назначается конкретному пользователю).
- **TimeEntry** агрегирует **Task** и **User**: каждая запись времени связана с конкретной задачей и пользователем.
- **Report** зависит от **Task** и **User**: отчет формируется для задания и пользователя.

![Pasted image 20241129164920](https://github.com/user-attachments/assets/65423f35-af31-42ea-8d12-de6a633145f0)


