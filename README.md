# Лабораторная работа №4

**Тема:** Анализ набора данных с помощью SQL  
**Предметная область примера:** сервис аренды велосипедов  
**СУБД:**  SandboxSQL
**Файл базы:** `dataset.sqlite

## Шаг 1. Загрузка базы и первичная проверка

В SandboxSQL загружена SQLite-база. Выполнена проверка числа строк во всех шести таблицах.

**Количество записей.** `

```sql
SELECT COUNT(*) AS total_students FROM students;SELECT COUNT(*) AS total_courses FROM courses;SELECT COUNT(*) AS total_enrollments FROM enrollments;SELECT COUNT(*) AS total_groups FROM groups;SELECT COUNT(*) AS total_teachers FROM teachers;SELECT COUNT(*) AS total_results FROM results;
```

**Результат:**



## Шаг 2. Изучение структуры и формулирование аналитических вопросов

| Таблица | Назначение | Основные поля | Записей |
|---|---|---|---:|
| `groups` | Группы | `id`, `name`, `stude_year` | 12 |
| `students` | Студенты | `id`, `group_id`, `admission year` | 240 |
| `teachers` | Преподаватели | `id`, `full_name`, `department` | 15 |
| `courses` | Курсы | `id`, `name`, `hours`, `tecaher_id` | 30 |
| `enrollments` | Поступления | `id`, `student_id`, `course_id`, `enrolled_at`, `status` | 1000 |
| `results` | Результаты | `id`, `enrollment_id`, `score`, `completed_at` | 850 |

**Связи:**
- students и groups
- courses и teachers
- enrollments и students, courses
- results и enrollments

  **Просмотр данных.** Просмотр первых пяти аренд.

```sql
SELECT * FROM students LIMIT 5;
```

**Результат:**

```sql
SELECT * FROM enrollments LIMIT 5;
```

**Первичные и внешние ключи. Связи между таблицами**

В базе данных используются первичные ключи (`PRIMARY KEY`) для идентификации записей и внешние ключи (`FOREIGN KEY`) для связи таблиц.

- `groups.id` — первичный ключ группы.
- `students.id` — первичный ключ студентов.
- `teachers.id` — первичный ключ преподователей.
- `courses.id` — первичный ключ курса.
- `enrollments.id` — первичный ключ поступления.
- `results.id` — первичный ключ результатов.

**Связи между таблицами:**

enrollments связана с students и courses. Конкретный студент поступил на конкретный курс.

students связана с groups. Каждый студент находится в определенной группе.

**Сформулированные аналитические вопросы:**

1. Какие студенты ни разу не записывались на курсы? Сколько их и к каким группам они относятся?
2. Сколько студентов в каждой группе?
3. Сколько студентов записалось на самое большое количество курсов?
4. На какой курс больше всего записей?
5. На каком курсе больше всего результатов "100"?

## Шаг 3. Фильтрация и сортировка

### 1 Вопрос

```sql
SELECT * FROM students WHERE id NOT IN (SELECT student_id FROM enrollments);
```
9

### 2 Вопрос

```sql
SELECT group_id, COUNT(id) FROM students GROUP BY group_id
```
10

### 3 Вопрос

```sql
SELECT student_id, COUNT(course_id) AS course_count FROM enrollments GROUP BY student_id ORDER BY course_count DESC;
```

### 4 Вопрос

```sql
SELECT course_id, COUNT(student_id) AS student_count FROM enrollments GROUP BY course_id ORDER BY student_count DESC;
```

### 5 Вопрос
