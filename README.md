# Report

## The original table in first normal form (1NF)

The following table is the original table, representing students' grades in courses at a university, which is already in first normal form (1NF) - all records have the same number of fields, and there is only one value per field.

| assignment_id | student_id | due_date | professor | assignment_topic                | classroom | grade | relevant_reading    | professor_email   | 
| :------------ | :--------- | :------- | :-------- | :------------------------------ | :-------- | :---- | :------------------ | :---------------- |
| 1             | 1          | 23.02.21 | Melvin    | Data normalization              | WWH 101   | 80    | Deumlich Chapter 3  | l.melvin@foo.edu  |
| 2             | 7          | 18.11.21 | Logston   | Single table queries            | 60FA 314  | 25    | Dümmlers Chapter 11 | e.logston@foo.edu |
| 1             | 4          | 23.02.21 | Melvin    | Data normalization              | WWH 101   | 75    | Deumlich Chapter 3  | l.melvin@foo.edu  |
| 5             | 2          | 05.05.21 | Logston   | Python and pandas               | 60FA 314  | 92    | Dümmlers Chapter 14 | e.logston@foo.edu |
| 4             | 2          | 04.07.21 | Nevarez   | Spreadsheet aggregate functions | WWH 201   | 65    | Zehnder Page 87     | i.nevarez@foo.edu |
| ...           | ...        | ...      | ...       | ...                             | ...       | ...   | ...                 | ...               |
### Adding obvious missing fields as additional fields
In this case, I decided to add 6 additional fields:
1. **student_name**: Name of student.
2. **student_email**: Email of student.
1. **course_name**: Name of the course.
2. **section_id**: Identifier for the specific section of the course.
3. **course_code**: Unique code assigned to the course, useful for identification purposes.
4. **classroom_location**: Location of classroom

Here is the tables with three more fields:

|assignment_id | student_id |student_name|student_email| due_date | professor | assignment_topic | classroom | grade | relevant_reading | professor_email | course_name | section_id | course_code|classroom_location|
|--------|----|-----|-------------|-----|---------|----|-----|----------|---------|------------|---------------|--------------|------------------|------------------|
|1              | 1            |Peter Wang|PW3445@uni.edu| 23.02.21  | Melvin       | Data normalization | WWH 101 | 80      | Deumlich Chapter 3 | l.melvin@foo.edu | Introduction to Database Systems | 10101 | CS101| 251 Mercer St Room 109
|2              | 7            |Leo Song|LS3458@uni.edu| 18.11.21  | Logston     | Single table queries  | 60FA 314 | 25     | Dümmlers Chapter 11 | e.logston@foo.edu | Database Management | 31401 | CS201|194 Mercer St Room 203
|1              | 4            |Saveliy Arsen Reese|SR2485@uni.edu| 23.02.21  | Melvin       | Data normalization | WWH 101 | 75      | Deumlich Chapter 3 | l.melvin@foo.edu | Introduction to Database Systems | 10102 | CS101| 12 Waverly PI Room G08
5              | 2            |Sally Carelli|SC3458@uni.edu| 05.05.21  | Logston     | Python and pandas      | 60FA 314 | 92      | Dümmlers Chapter 14 | e.logston@foo.edu | Data Analysis with Python | 31402 | CS203|194 Mercer St Room 304
4              | 2          |Sally Carelli |SC3458@uni.edu| 04.07.21  | Nevarez    | Spreadsheet aggregate functions | WWH 201 | 65 | Zehnder Page 87 | i.nevarez@foo.edu | Spreadsheet Applications | 20101 | BUS102|7 East 12th St Room LL25
...|...|...|...|...|...|...|...|...|...|...|...|...|...|...


### What makes the original data set not compliant with 4NF
1. This dataset is not compliant with Fourth Normal Form (4NF) primarily due to the presence of **more than one independent multi-valued fact about an entity**. To be more specific, a single student might have multiple assignments and multiple professors. For example, the student with id 2 has two different assignments and two different professors.

2. There is **redundant information** in the table, such as the professor_email and the due_date, which are repeated for each student-assignment combination. This redundancy could be eliminated by normalizing the data.

3. Then, this dataset is not compliant with 4NF due to that **a non-key field is a fact about another non-key field**. To be more specific, professor_email is a fact about professor which is a non-key field.

## The new tables in Fourth Normal Form (4NF)
To convert the table to the Fourth Normal Form (4NF), we need to ensure that it eliminates any multi-valued dependencies and maintains the integrity of the data. Based on the provided data and assumptions, we can derive the following entities:
1. **Assignment Table**
- assignment_id (Primary Key)
- due_date (due date of assignment)
- assignment_topic (topic of assignment)
- relevant_reading (relevant reading provided according to each assignment)
- section_id (Foreign Key) (the section assignment belongs to)

| assignment_id | section_id | due_date | assignment_topic           | relevant_reading      |
|---------------|------------|----------|----------------------------|-----------------------|
| 1             | 10101      | 23.02.21 | Data normalization         | Deumlich Chapter 3    |
| 1             | 10102      | 23.02.21 | Data normalization         | Deumlich Chapter 3    |
| 2             | 31401      | 18.11.21 | Single table queries       | Dümmlers Chapter 11   |
| 4             | 20101      | 04.07.21 | Spreadsheet aggregate functions | Zehnder Page 87   |
| 5             | 31402      | 05.05.21 | Python and pandas          | Dümmlers Chapter 14   |
|...|...|...|...|...|

2. **Student Table**
- student_id (Primary Key)
- student_name (Added to store the student’s name)
- student_email(Added to store the student’s email)

| student_id | student_name         | student_email   |
|------------|----------------------|-----------------|
| 1          | Peter Wang           | PW3445@uni.edu  |
| 2          | Sally Carelli        | SC3458@uni.edu  |
| 4          | Saveliy Arsen Reese  | SR2485@uni.edu  |
| 7          | Leo Song             | LS3458@uni.edu  |
|...|...|...|


3. **Professors Table**
- professor_email (Primary Key)
- professor

| professor_email   | professor  |
|-------------------|------------|
| l.melvin@foo.edu | Melvin     |
| e.logston@foo.edu| Logston    |
| i.nevarez@foo.edu| Nevarez    |
|...|...|

4. **Courses Table**
- course_code (Primary Key)
- course_name (Added to store the name of the course)

| course_code | course_name |
|-------------|-------------|
| CS101       | Introduction to Database Systems |
| BUS102      | Spreadsheet Applications |
| CS201       | Database Management |
| CS203       | Data Analysis with Python |
|...|...|



5. **Sections Table**:
- section_id (Primary Key)
- course_id (Foreign Key)
- professor_email (Foreign Key)
- classroom_location ((Added to store the name of the course))

| section_id | course_id  | professor_email  | classroom_location |
|---------------------------|--------------------------|-------------------------------|--------------------|
| 10101                     | CS101                    | l.melvin@foo.edu             | 251 Mercer St Room 109 |
| 10102                     | CS101                    | l.melvin@foo.edu             | 12 Waverly PI Room G08 |
| 20101                     | BUS102                   | i.nevarez@foo.edu            | 7 East 12th St Room LL25 |
| 31401                     | CS201                    | e.logston@foo.edu            | 194 Mercer St Room 203 |
| 31402                     | CS203                    | e.logston@foo.edu            | 194 Mercer St Room 304 |
...|...|...|...|...|


6. **Grades Table**:
- grade_id (Primary Key)
- assignment_id (Foreign Key)
- student_id (Foreign Key)
- grade

| grade_id| assignment_id | student_id  | grade |
|-------------------------|-----------------------------|--------------------------|-------|
| 1                       | 1                           | 1                        | 80    |
| 2                       | 2                           | 7                        | 25    |
| 3                       | 1                           | 4                        | 75    |
| 4                       | 5                           | 2                        | 92    |
| 5                       | 4                           | 2                        | 65    |
...|...|...|...|

7. **Enrollment Table**:
- enrollment_id (Primary Key)
- student_id (Foreign Key)
- section_id (Foreign Key)

|enrollment_id|student_id|section_id|
|-------------|----------|----------|
|1|1|10101|
|2|7|CS201|
|3|4|CS101|
|4|2|CS203|
|5|2|BUS102|
|...|...|...|

## ER diagram of 4NF-compliant version of the data set
Here is the ER diagram:

![ER diagram](ER%20diagram.png)

