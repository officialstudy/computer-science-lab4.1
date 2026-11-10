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

1. Какие студенты ни разу не записывались на курсы? Сколько их и к каким группам они относятся?(`students`, `groups`, `enrollments`)
2. Сколько студентов в каждой группе?(`students`, `groups`)
3. Сколько студентов записалось на самое большое количество курсов?( `enrollments`)
4. На какой курс больше всего записей?(`courses`, `enrollments`)
5. На каком курсе больше всего результатов "100"?(`courses`, `enrollments`, `results`)
6. Какие студенты учаться в конкретной группе?(`students`)
7. В каких группах учится более 15 студентов?(`students`)
## Шаг 3. Фильтрация и сортировка

### 2 Вопрос

```sql
SELECT group_id, COUNT(id) FROM students GROUP BY group_id
```
10

Связь students и groups. Смотрим сколько студентов в каждой группе.

### 6 Вопрос

```sql
SELECT full_name , group_id FROM students WHERE group_id=1
```
Связь students и group. Через group_id считаем студентов с одинаковым значением.

## Шаг 4. Агрегирование и группировка

### 2 Вопрос

```sql
SELECT group_id, COUNT(id) FROM students GROUP BY group_id
```
10

Связь students и groups. Смотрим сколько студентов в каждой группе.

### 3 Вопрос

```sql
SELECT student_id, COUNT(course_id) AS course_count FROM enrollments GROUP BY student_id ORDER BY course_count DESC;
```

### 4 Вопрос

```sql
SELECT course_id, COUNT(student_id) AS student_count FROM enrollments GROUP BY course_id ORDER BY student_count DESC;
```
Связь students и courses через enrollments. Через поступления смотрим сколько студентов имеет одниковое id курсы.

### 5 Вопрос

```sql
SELECT course_id, COUNT(*) FROM enrollments JOIN results ON enrollments.id = results.enrollment_id WHERE results.score = 100 GROUP BY course_id ORDER BY COUNT(*) DESC
```

Связь students и courses через enrollments. Смотрим у каких студентов score равен 100.

### 7 Вопрос

```sql
SELECT group_id, COUNT(id) AS student_count FROM students GROUP BY group_id HAVING COUNT(id) > 15;
```

Связь students с group_id. Смотрим чтобы у группы было более 15 студентов.

Разница where и having. Having фильтрует группы, а Where отдельные строки.

## Шаг 5. Объединение связанных таблиц

### 5 Вопрос

```sql
SELECT course_id, COUNT(*) FROM enrollments JOIN results ON enrollments.id = results.enrollment_id WHERE results.score = 100 GROUP BY course_id ORDER BY COUNT(*) DESC
```

Связь students и courses через enrollments. Через результаты смотрим id поступления, где выводим курс. JOIN склеивает id , чтобы  по ним можно было искать студента к которому относится score.

### Студенты и названия их групп

```sql
SELECT students.full_name, groups.group_name AS group_name FROM students INNER JOIN groups ON students.group_id = groups.id LIMIT 10;
```


###
```sql
SELECT teachers.full_name, courses.course_name FROM teachers INNER JOIN courses ON teachers.id = courses.teacher_id ;
```
