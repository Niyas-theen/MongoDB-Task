# Zen Class Programme MongoDB Database Design

This project contains the database design and MongoDB queries for managing the **Zen Class Programme**. It includes collections for users, codekata, attendance, topics, tasks, company drives, and mentors. The database structure enables efficient data storage, retrieval, and querying for tasks such as tracking attendance, task submission, mentor-mentee relationships, and company placement drives.

## Database Structure

### Collections

- **users**: Stores information about users, including their attendance, tasks, and CodeKata problem-solving statistics.
- **codekata**: Stores the number of problems solved by users in CodeKata.
- **attendance**: Tracks attendance records for each user on specific dates.
- **topics**: Stores the topics taught in the Zen class and associated tasks.
- **tasks**: Tracks tasks assigned to users and their submission status.
- **company_drives**: Stores information about company placement drives and students who participated.
- **mentors**: Stores information about mentors and the mentees they are assigned.

### Example Document Structure

#### `users` Collection
```json
{
  "_id": ObjectId(),
  "name": "NiyasTheen",
  "email": "niyastheen@example.com",
  "batch": "FSD-WD-T-B4",
  "codekata_problems_solved": 120,
  "attendance": [
    {
      "date": ISODate("2020-10-15"),
      "status": "present"
    }
  ],
  "tasks": [
    {
      "topic_id": ObjectId(),
      "task_name": "Task 1",
      "submitted": true
    }
  ],
  "mentor_id": ObjectId()
}

company_drives Collection
{
  "_id": ObjectId(),
  "company_name": "ABC Corp",
  "date": ISODate("2020-10-20"),
  "students_appeared": [
    ObjectId(),
    ObjectId()
  ]
}
