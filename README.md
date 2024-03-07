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
4. **section_location**: Location of classroom

Here is the tables with three more fields:

|assignment_id | student_id |student_name|student_email| due_date | professor | assignment_topic | classroom | grade | relevant_reading | professor_email | course_name | section_id | course_code|section_location|
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
1. assignment
2. student
3. professor
4. course
5. section
6. grade


Then I abstract some attributes from the orginal table to each of them. And then split the original table into seven tables in 4NF.

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

For this table, I found that there are still multi-valued dependencies, like one assignment can be assigned to multiple sections and thus have multiple due dates. So I decided to split the table again to satisfy the 4NF. I want to assume that a specific assignment_id has the specific topic and relevant_reading. 

One is `assignment`:
| assignment_id |assignment_topic           | relevant_reading      |
|---------------|----------------------------------|-----------------------|
| 1             | Data normalization         | Deumlich Chapter 3    |
| 2             |  Single table queries       | Dümmlers Chapter 11   |
| 4             |  Spreadsheet aggregate functions | Zehnder Page 87   |
| 5             |  Python and pandas          | Dümmlers Chapter 14   |
|...|...|...|

Another for `assignment_section`


assignment_section_id| assignment_id | section_id | 
|---------|---------------|------------|
|1| 1             | 10101      |  
|2| 1             | 10102      | 
|3| 2             | 31401      |  
|4| 4             | 20101      | 
|5| 5             | 31402      | 
|...|...|...|

Another for `assignment_section_and_due_date`

| assignment_section_id | due_date |
|---------------|------------|
| 1             | 23.02.21 |
| 2             | 18.11.21 | 
| 4             |  04.07.21 | 
| 5             |  05.05.21 |
|...|...|...|...|...|

2. **Student Table**
- student_id (Primary Key)
- student_name (Added to store the student’s name)(name of student)
- student_email(Added to store the student’s email)(email of student)

| student_id | student_name         | student_email   |
|------------|----------------------|-----------------|
| 1          | Peter Wang           | PW3445@uni.edu  |
| 2          | Sally Carelli        | SC3458@uni.edu  |
| 4          | Saveliy Arsen Reese  | SR2485@uni.edu  |
| 7          | Leo Song             | LS3458@uni.edu  |
|...|...|...|


3. **Professors Table**
- professor_email (Primary Key)(email of professor)
- professor (name of professor)

We assume professor have different emails from each other. 

| professor_email   | professor  |
|-------------------|------------|
| l.melvin@foo.edu | Melvin     |
| e.logston@foo.edu| Logston    |
| i.nevarez@foo.edu| Nevarez    |
|...|...|

4. **Courses Table**
- course_code (Primary Key)
- course_name (Added to store the name of the course) (name of course)

| course_code | course_name |
|-------------|-------------|
| CS101       | Introduction to Database Systems |
| BUS102      | Spreadsheet Applications |
| CS201       | Database Management |
| CS203       | Data Analysis with Python |
|...|...|



5. **Sections Table**:
- section_id (Primary Key)
- course_id (Foreign Key) (the course the section belongs to)
- professor_email (Foreign Key) (the professor who teach the section)
- location of the section
- student_id (which student enroll this section)

We assume that a section is belong to a specific course and in specific location. However, one section can be taught by more than one professors. So the table is still not in 4NF.

| section_id | course_code | professor_email  | section_location|student_id|
|---------------------------|--------------------------|-------------------------------|---------|-----|
| 10101                     | CS101                    | l.melvin@foo.edu             | Mercer St Room 109|1
| 10102                     | CS101                    | l.melvin@foo.edu             | 12 Waverly PI Room G08|3
| 20101                     | BUS102                   | i.nevarez@foo.edu            | 7 East 12th St Room LL25|4
| 31401                     | CS201                    | e.logston@foo.edu            | 194 Mercer St Room 203|2
| 31402                     | CS203                    | e.logston@foo.edu            | 194 Mercer St Room 304|2
...|...|...|...|...|

However, one section can be taught by more than one professors. And one section can be enrolled by multiple students. So the table is still not in 4NF. So I decided to split the table into more 4NF tables.

One is `section`
| section_id | course_code | section_location
|---------------------------|-------------------------------|---------
| 10101                     | CS101                     | Mercer St Room 109
| 10102                     | CS101                    | 12 Waverly PI Room G08
| 20101                     | BUS102                   | 7 East 12th St Room LL25
| 31401                     | CS201                    | 194 Mercer St Room 203
| 31402                     | CS203                    | 194 Mercer St Room 304
...|...|...|

Another one is `section_and_professor_email`
| section_id |professor_email  | 
|---------------------------|--------------------------|
| 10101                     | l.melvin@foo.edu             |
| 10102                     | l.melvin@foo.edu             | 
| 20101                     |  i.nevarez@foo.edu            | 
| 31401                     |  e.logston@foo.edu            | 
| 31402                      | e.logston@foo.edu            | 
...|...|

Another one is `section_and_student_id`
| section_id | student_id|
|---------------------------|-------|
| 10101                     |1
| 10102                     |3
| 20101                     |4
| 31401                     |2
| 31402                     |2
...|...|...|...|...|

6. **Grades Table**:
- grade_id (Primary Key)
- assignment_id (Foreign Key) (assignment the grade belongs to)
- student_id (Foreign Key) (student who get the grade)
- grade (actual grade)

| grade_id| assignment_id | student_id  | grade |
|-------------------------|-----------------------------|--------------------------|-------|
| 1                       | 1                           | 1                        | 80    |
| 2                       | 2                           | 7                        | 25    |
| 3                       | 1                           | 4                        | 75    |
| 4                       | 5                           | 2                        | 92    |
| 5                       | 4                           | 2                        | 65    |
...|...|...|...|

In the context of the grade table, the primary key is the grade_id. However, we have a transitive dependency between the assignment_id, student_id, and grade. So I split the table into many tables in 4NF.

The first one is `assignment_student` table

| assignment_student_id| assignment_id | student_id  | 
|-------------------------|-----------------------------|--------------------------|
| 1                       | 1                           | 1                        | 
| 2                       | 2                           | 7                        | 
| 3                       | 1                           | 4                        | 
| 4                       | 5                           | 2                        |
| 5                       | 4                           | 2                        | 
...|...|...|

The next one is `grade` table
| grade_id| assignment_student_id | grade |
|-------------------------|---------------------------|-------|
| 1                       | 1                                               | 80    |
| 2                       | 2                           | 25    |
| 3                       | 3                         | 75    |
| 4                       | 4                           |  92    |
| 5                       | 5                           | 65    |
...|...|...|...|



## ER diagram of 4NF-compliant version of the data set
Here is the ER diagram:

![Pushing work in Visual Studio Code](ER%20diagram.png)

Tips: I didn't draw foreign keys in this diagram, but I mentioned them before.

### Explaination of the diagram
Relationship
1. **The relationship between assignment and section**: In a university setting, multiple assignments can be associated with a single section, and each assignment can be assigned to multiple sections. Therefore, the relationship between assignments and sections is usually `many-to-many`.
2. **The relationship between professor and section**: The relationship between professors and sections can vary depending on the structure of the university or educational institution. In my case, a section is typically associated with one professor who teaches it. However, a professor may teach multiple sections, especially in larger courses or when covering multiple sections of the same course. Therefore, the relationship between professor and section is often `one-to-many`, where one professor can be associated with multiple sections, but each section is typically associated with only one professor.
3. **The relationship between student and enrollment**: 
The relationship between students and enrollment is typically `one-to-many`, meaning one student can be enrolled in multiple sections, but each enrollment corresponds to only one student.
4. **The relationship between enrollment and section**: 
The relationship between enrollment and section is `one-to-many`. This means that one enrollment can be associated with only one section, but each section can have multiple enrollments, representing multiple students enrolled in that section.
5. **The relationship between student and grades**:
The relationship between student and grades is `one-to-many`. This means that one student can have multiple grades associated with them, as they can complete multiple assignments in different sections. However, each grade is specific to one student.
6. **The relationship between grades and assignment**:
The relationship between grade and assignment is `one-to-one`. Each grade corresponds to one specific assignment, indicating the score achieved by a student on that assignment. Conversely, each assignment has one grade associated with it, representing the evaluation of the student's performance on that particular assignment.
7. **The relationship between courses and sections**: 
The relationship between course and section is `one-to-many`. A course can have multiple sections, each representing a different offering of the course at a specific time, location, or with a different instructor. Conversely, each section belongs to exactly one course, indicating the specific course it is associated with.




