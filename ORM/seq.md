# Storing Data Step by Step

Sequelize + MySQL

# What is Sequelize?

Sequelize is an ORM (Object Relational Mapper) for Node.js. It allows us to work with a MySQL database using JavaScript instead of writing SQL queries every time.

For example, instead of directly writing:

INSERT INTO Users (name, email, password)
VALUES ('Rahul', 'rahul@email.com', '123');

We can use Sequelize:
```
await User.create({
  name: "Rahul",
  email: "rahul@email.com",
  password: "123"
});
```
Both methods add a new user to the database, but Sequelize makes the process easier to handle through JavaScript.

---

# 1. Connecting Node.js with MySQL

"configurations/database.js"

First, we create a Sequelize object and provide the details required to connect with MySQL.
```
const { Sequelize } = require("sequelize");

const sequelize = new Sequelize(
  "resume_db",
  "root",
  "your_password",
  {
    host: "localhost",
    dialect: "mysql",
    logging: false,
  }
);

module.exports = sequelize;
```
Here:

- "resume_db" is the database name.
- "root" is the MySQL username.
- "your_password" is the MySQL password.
- "localhost" means MySQL is running on the local computer.
- "dialect: "mysql"" tells Sequelize which database system we are using.
- "logging: false" prevents Sequelize from printing SQL queries in the console.

---

# 2. Creating the User Model

"models/user.js"

A model represents a table in the database. Here, the "User" model represents the users table.
```
const { DataTypes } = require("sequelize");
const bcrypt = require("bcryptjs");
const sequelize = require("../config/database");

const User = sequelize.define("User", {
  name: {
    type: DataTypes.STRING,
    allowNull: false
  },

  email: {
    type: DataTypes.STRING,
    allowNull: false,
    unique: true,
    validate: {
      isEmail: true
    }
  },

  password: {
    type: DataTypes.STRING,
    allowNull: false
  }
});
```
The model contains three main fields:

- "name" stores the user's name.
- "email" stores the email address.
- "password" stores the password in hashed form.

"allowNull: false" means the field cannot be left empty.

"unique: true" makes sure that two users cannot register using the same email.

"isEmail: true" checks whether the entered value follows a valid email format.

---

# Password Protection

Passwords should never be stored directly as plain text.

Before a new user is saved, we can hash the password using bcrypt.
```
User.beforeCreate(async (user) => {
  const salt = await bcrypt.genSalt(10);

  user.password = await bcrypt.hash(
    user.password,
    salt
  );
});
```
The "beforeCreate" hook runs before the user is inserted into the database.

So, if the user enters:

mypassword123

the database stores a hashed version instead of the original password.

---

Checking a Password During Login

We can also add a method to compare the entered password with the stored hash.
```
User.prototype.checkPassword = function (plainText) {
  return bcrypt.compare(plainText, this.password);
};
```
If the password matches, the result will be "true"; otherwise, it will be "false".

---

# 3. Connecting Users and Resumes

One user can have multiple resumes, so we create a one-to-many relationship.
```
User.associate = (models) => {
  User.hasMany(models.Resume, {
    foreignKey: "userId",
    onDelete: "CASCADE"
  });
};
```
This means:

One User → Many Resumes

The "userId" field will identify which user owns each resume.

"onDelete: "CASCADE"" means that if a user is removed, the resumes connected to that user will also be removed.

Finally:

module.exports = User;

---

# 4. Creating the Resume Model

"models/resume.js"

The resume table contains information about each resume.
```
const { DataTypes } = require("sequelize");
const sequelize = require("../config/database");

const Resume = sequelize.define("Resume", {
  title: {
    type: DataTypes.STRING,
    allowNull: false
  },

  summary: {
    type: DataTypes.TEXT
  }
});
```
Here:

- "title" stores the resume title.
- "summary" contains a longer description.
- "DataTypes.TEXT" is useful when we need to store more text.

A resume belongs to a particular user:
```
Resume.associate = (models) => {
  Resume.belongsTo(models.User, {
    foreignKey: "userId"
  });
};

module.exports = Resume;
```
So the relationship can be understood as:
```
User → hasMany → Resumes

Resume → belongsTo → User
```
---

# 5. Loading the Models Together

"models/index.js"

This file loads both models and connects their relationships.
```
const sequelize = require("../config/database");

const User = require("./user");
const Resume = require("./resume");

const models = {
  User,
  Resume
};

Object.values(models).forEach((model) => {
  if (model.associate) {
    model.associate(models);
  }
});

module.exports = {
  sequelize,
  ...models
};
```
The important thing here is that both models are loaded first. After that, their associations are connected.

---

Database Operations

# Step 1 — Create the Tables

Before using the models, Sequelize can create the required tables in MySQL.
```
const { sequelize } = require("./models");

await sequelize.sync();
```
"sync()" checks the database and creates missing tables.

## Normally:

```
await sequelize.sync();
```
is safer because it does not delete existing data.

## Avoid using:
```
await sequelize.sync({ force: true });
```
on an important database because it can remove existing tables and recreate them.

---

# Step 2 — Add a New User

We can create a user using "User.create()".
```
const user = await User.create({
  name: "Rahul",
  email: "rahul@example.com",
  password: "mypassword123"
});

console.log("User created with ID:", user.id);
```
Sequelize converts this JavaScript operation into an SQL "INSERT" query.

The database also automatically maintains fields such as:

createdAt
updatedAt

The "id" is generated automatically for the new user.

---

# Step 3 — Add a Resume

After creating a user, we can create a resume and connect it to that user's ID.
```
await Resume.create({
  title: "Backend Developer",
  summary: "Worked with Node.js, Express and MySQL.",
  userId: user.id
});
```
The important part is:

userId: user.id

This creates the connection between the resume and its owner.

---

# Step 4 — Get Resumes of a Particular User

To find all resumes belonging to one user:
```
const resumes = await Resume.findAll({
  where: {
    userId: user.id
  }
});

resumes.forEach((resume) => {
  console.log(resume.title);
});
```
Here, "where" works like the SQL "WHERE" condition.

It filters the records and returns only the resumes associated with that user.

---

# Step 5 — Get a Resume Along With Its User

Sometimes we need information from two related tables at the same time.

For example:
```
const resume = await Resume.findByPk(1, {
  include: User
});

console.log(
  resume.title,
  "belongs to",
  resume.User.name
);
```
The "include: User" option tells Sequelize to fetch the related user information as well.

This is similar to using a SQL "JOIN".

So instead of separately searching for the resume and then searching for its user, Sequelize can retrieve the related information together.

---

# Step 6 — Modify an Existing Resume

Suppose we want to change the resume title:

resume.title = "Full Stack Developer";

await resume.save();

"save()" sends the updated information back to the database.

Sequelize generally updates only the fields that were changed.

The "updatedAt" field is also updated automatically.

---

# Important Sequelize Methods

    Sequelize Method| Purpose
"sequelize.define()"| Defines a model/table
  "sequelize.sync()"| Creates or synchronizes tables
    "Model.create()"| Adds a new record
   "Model.findAll()"| Retrieves multiple records
  "Model.findByPk()"| Finds a record using its primary key
   "Model.findOne()"| Finds one matching record
   "instance.save()"| Saves changes to a record
"instance.destroy()"| Deletes a record
          "include:"| Loads related model data
            "where:"| Filters records
      "beforeCreate"| Runs code before creating a record
           "hasMany"| Defines a one-to-many relationship
         "belongsTo"| Defines a relationship where a record belongs to another model
         

---

Bcrypt Password Hashing

Bcrypt is commonly used to protect passwords before saving them in a database.

const bcrypt = require("bcryptjs");

const salt = await bcrypt.genSalt(10);
```
const hashedPassword = await bcrypt.hash(
  "mypassword123",
  salt
);
```
To verify a password later:
```
const isCorrect = await bcrypt.compare(
  "mypassword123",
  hashedPassword
);

console.log(isCorrect);
```
If the entered password matches the stored hash, the result is "true".

The number "10" represents the salt-round setting. Increasing it makes password hashing more computationally expensive, so it should be chosen according to the application's security and performance requirements.

---

# What I Learned

Today I learned how Sequelize works with MySQL and how JavaScript code can be used to perform database operations.

# I also understood:

- How to connect Node.js with MySQL.
- How Sequelize models represent database tables.
- How to insert, read, update and delete records.
- How foreign keys connect different tables.
- How "hasMany" and "belongsTo" create relationships.
- How "include" can fetch related data.
- Why passwords should be hashed before storing them.
- How bcrypt helps in password protection.
- How Sequelize reduces the need to write raw SQL queries.

# 👨‍💻 Author
Latesh Padaliya

🎓 B.Tech Computer Science Engineering Student

🌱 Aspiring Full Stack Developer

GitHub: https://github.com/LateshDev

LinkedIn: https://www.linkedin.com/in/latesh-padaliya

⭐ Support
If you like this project, consider giving it a ⭐ on GitHub.
- 
