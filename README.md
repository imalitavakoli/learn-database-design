# Database Design Tips & Tricks
So are you a newbie in SQL and databases? Are you working on a server side project? Or maybe starting to build an awesome mobile application and need to design the local databases?  

Well, it doesn't matter! Wherever and whenever you need databases, you should know the basics of database design! And that's what we're going to cover in this short tutorial.

We're going to learn how to think when it comes to database design and what steps should we take. So are you ready? Cool :thumbsup:




# Steps in order to design a database
Of course you have to know what your client wants to say, understand the project completely, know the audiences, what do they need to achieve, analyze data, use the best project processing, be agile and etc...

Yes, yes! There are lots of things that you need to take into consideration! But apart from any methodology and project development process, you need to consider the following general three simple steps for any kind of project to design a database. From the very simple projects to the more huge and complex ones.

1. First you have to ask our client and see what are the project's use cases that she has in mind, what is her main goal, and what data is the most important one at the end.

2. We also need to checkout the use cases from different actors. I mean different people who are going to use our software, such as Manager and Secretary and etc... As different roles in a company, have different use cases.

3. Now that we know the goal and use cases, we can design our initial database model.




# What should we concern in database design?
Considering the following points in designing a database can help you a lot to keep your data clean and develop a more scalable project.

- **Database design is like OOP**(Object Oriented Programming), more classes with less line is better! Here, more tables with less columns and rows is better.

- Consider the following fact in OOP:  
A *Class* has some *Attributes*. Now an *Object* is an instance of the class and it has its own values for those attributes.

  Now in database design, we can actually say this:  
  Class = table; Attribute = column; Object = row.

  Example:  
  Class = a table that holds plants' data; Attributes = the table have different columns about each parameter of a plant; Objects = we have 5 different types of plants totally(rows).

- **Remember to have repeated data as less as possible**. This would prevent inconsistent information. For example, an 'assignment' table which has a `project_name` column, is not sufficient... Because all of the assignments are for only one single project, so every row needs to store a repeated data! In this way, we're repeating ourselves and putting the same project name again and again in each row!

  Instead we can create a new table called 'project' and link 'assignment' table with a *Foreign key* to the 'project' table.

- **Have as less connections as possible** between tables, it prevents extra complexity.

- **Most of the tables need a *Primary key***. Although other information must distinguish each row from another.

  For example in the 'project' table, each project must have an unique data, otherwise what's the point of having two rows if all of the information of them is the same!  
  So as I have mentioned before, although each row has unique data so we can easily distinguish them from one another, but a if we like to access them in the future, a *Primary key* is still needed, because **any information may change but ID(the *Primary key*) never changes**.

- If most of the columns of two different tables are the same, then make a parent table(super class) and make those two children(sub classes) of the parent, so that they can inherit from the parent table.

  **In SQL we don't have inheritance in real meaning, but by making a *Foreign key* for children and *Primary key* for the parent table, we can accomplish this**.

  For example we have two children('student', 'lecturer') and a parent('person'). All of the people will be saved in the 'Person' table(`id`, `last_name`, `first_name`) but other specific info about a student or lecturer will be saved in the child tables. 'student' table(`person_id`, `degree`) and 'lecturer' table(`person_id`, `salary`).

- **Normalize our design**. It's critical that all the attributes are in the correct table so that we won't face a problem in the future. In normalize we mainly check three facts:
  - Modification issues: It's when inserting or modifying a column causes issues. For example we may insert something misspelled accidentally. But by removing repeated info, we simply prevent that.

  - Insertion issues: It's when we cannot insert new values for our columns as we don't have access to them because of a bad database design. In a good database design, with only knowing one and only one *Primary key* value, we should be able to insert new values for all of the other related columns.

  - Deletion issues: It's when we delete extra info accidentally because of a bad database design.

  **Normalize also helps us have functional dependency in our tables** (it means that if I know the value of an attribute, I can uniquely tell the value of the other related attributes. For example if I know the `person_id`, then I can tell that person's first name, last name, and etc).

- **Don't insert multi data for a column**. For example when we have 'plant' table and each plant has different uses, don't make a `uses` column in the 'plant' table and insert a multi data value for it, like this: `'firewoord, soil stability, shelter'`. Instead make a new table called 'plant_use'(`plant_id`, `use`) and simply repeat a plant ID per use. For instance *plant1* has 3 uses, so its ID will be in 3 rows in the 'plant_use' table, each time with one use.




# Useful SQL statements that help us design more efficiently
Now let's take a look at some simple but useful SQL statements that help us in our database design.

## `CHECK` keyword
It helps us to make sure that we won't misspell when inserting new data into a row. Because our `type` column in this example, only let us to insert the chosen three values.

**NOTE:** Though, unfortunately the `CHECK` keyword is not supported by MySQL. But still there are some [alternative ways](http://stackoverflow.com/a/7522055/247670) that you can try.
```sql
CREATE TABLE member (
id INT AUTO_INCREMENT NOT NULL,
last_name VARCHAR(20),
first_name VARCHAR(20),
type VARCHAR(20),
PRIMARY KEY (id),
CHECK type IN ('Senior', 'Junior', 'Social')
);
```

## `UNIQUE` keyword
It makes sure we won't insert a new repeated record value for all of the selected columns for it. Because in our example here, `id` and the combination of `date`, `farm` and `paddock` altogether, were our candidates for choosing the *Primary key*. But because it's better to have only one column to be chosen as the Primary key in most of the times then we didn't set them as the *Primary key*... But well, we still like the value of their combination be uniquely inserted for each row, and that's why we use `UNIQUE` keyword.
```sql
CREATE TABLE visit (
id INT AUTO_INCREMENT NOT NULL,
date DATE,
farm INT,
paddock INT,
PRIMARY KEY (id),
FOREIGN KEY (farm, paddock) REFERENCES paddock(id),
UNIQUE (date, farm, paddock)
);
```




# What's next?
In this short tutorial I just explained the very important facts that you should consider when designing a database. Of course there's a lot more to cover. So like to learn more? Well, [Beginning Database Design](http://www.amazon.com/Beginning-Database-Design-Novice-Professional/dp/1430242094/) book, teaches you the database design from basics to details.
