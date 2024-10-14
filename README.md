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
```
{
  _id: ObjectId(),
  name: "NiyasTheen",
  email: "niyastheen@example.com",
  batch: "FSD-WD-T-B4",
  codekata_problems_solved: 120,
  attendance: [
    {

      date: ISODate("2020-10-15"),
      status: "present"
    }
  ],
  tasks: [
    {
      topic_id: ObjectId(),
      task_name: "Task 1",
      submitted: true
    }
  ],
  mentor_id: ObjectId()
}
```
#### `company_drives` Collection
```
{
  _id: ObjectId(),
  company_name: "zeal creations",
  date: ISODate("2020-10-20"),
  students_appeared": [
    ObjectId(),
    ObjectId()
  ]
}
```

### Queries

#### 1. Find all the topics and tasks taught in the month of October
```
db.topics.aggregate([
  { 
    $match: { 
      "tasks.date": { 
        $gte: ISODate("2020-10-01"), 
        $lte: ISODate("2020-10-31") 
      } 
    }
  },
  {
    $project: {
      topic_name: 1,
      tasks: {
        $filter: {
          input: "$tasks",
          as: "task",
          cond: { 
            $and: [
              { $gte: ["$$task.date", ISODate("2020-10-01")] },
              { $lte: ["$$task.date", ISODate("2020-10-31")] }
            ]
          }
        }
      }
    }
  }
])
```

#### 2. Find all the company drives which appeared between 15-Oct-2020 and 31-Oct-2020
```
db.company_drives.find({
  date: {
    $gte: ISODate("2020-10-15"),
    $lte: ISODate("2020-10-31")
  }
})
```

#### 3. Find all the company drives and students who appeared for the placement
```
db.company_drives.aggregate([
  {
    $lookup: {
      from: "users",
      localField: "students_appeared",
      foreignField: "_id",
      as: "appeared_students"
    }
  },
  {
    $project: {
      company_name: 1,
      date: 1,
      "appeared_students.name": 1,
      "appeared_students.email": 1
    }
  }
])
```

#### 4. Find the number of problems solved by the user in CodeKata
```
db.users.find(
  { name: "John Doe" },
  { codekata_problems_solved: 1 }
)
```

#### 5. Find all the mentors who have mentees count more than 15
```
db.mentors.find({
  mentees_count: { $gt: 15 }
})
```

#### 6. Find the number of users who are absent and task is not submitted between 15-Oct-2020 and 31-Oct-2020
```
db.users.aggregate([
  {
    $match: {
      "attendance.date": { $gte: ISODate("2020-10-15"), $lte: ISODate("2020-10-31") },
      "attendance.status": "absent",
      "tasks.submitted": false
    }
  },
  {
    $count: "absent_and_not_submitted"
  }
])
```

### Project Setup
- Install MongoDB on your system.
- Set up the collections mentioned above using the sample data provided.
- Execute the queries in a MongoDB shell or MongoDB Compass to retrieve the desired results.
