# Design Create Query
This is part of my course to design, create and querying a database. It involved a problem statement and the definition/constraints/features of the database and the problems.

## Task

Swansea University is located on the beautiful Gower peninsula, so the university has decided that students should enjoy a healthy work/study/life balance. To this end, they have asked you to help design a new fun activity database for students. This database will help students find things to do and allow event organizers to provide options to the large student population in Swansea. This database could be used for any type of event at any location that an organizer wanted to offer: clubs, pubs, restaurants, bands, hiking clubs, meetups, etc.
The primary users of this database will be students trying to find interesting things to do with their time, and organizers who want to provide new fun activities that students can attend. There will be no central administrators for this database, so we will need to put in information and features that will help students and organizers connect and to decide whether to trust the information that is entered. The details of all these needed features will be described below.

## Project Definition

Your database will need to track and connect students and organizers of fun activities in Swansea. These users
will interact with the database using SQL code that you provide.

1. Implement. Your database will comprise at most 5 relations, among them will be Student, Organizer, Location and Event. You will likely need one other relation complete this database correctly. Each of these relations must contain at least 4 realistic attributes for the stated problem. You will also need to populate the database with enough sample data to effectively show that your code works (5 entry rows per table should be enough). Your choice of tables, keys and attributes should reflect the best approaches to database design as learned. Do NOT submit an ER diagram with your course work, but you may find it useful to create one when planning the relations for this project.

2. Once you’ve created your new database, you will add functionality for the system admin to interact with the data. Some of these tasks with require you to query the existing tables, some of the tasks will require you to modify or add data to the existing tables. The purpose of this database is for students to find and rate events, and for organizers to create events and learn about how well they did.

## Functionality

To demonstrate the **functionality** of your database, write SQL code for the following:

- Create New Event: Organizers will want the ability to create a new, unique event in the database. This new event should be stored permanently in the database and be immediately available for students to register their intent to attend.
- Rate a previous event: Any student should be able to enter a numerical rating for any event that they registered to attend. There will be no way for this database to track whether a student actually attended the event, so the best you can do is make sure that they registered prior to the event. You may assume that a student will only be allowed to rate a given previous event once.
- Student report: Show the information on a student including last 10 events attended and the average of how they rated all previous events (pretend there are no privacy issues here and do not show contact info for the student).
- What’s coming: List all coming events of a certain type in a certain area. Include the average rating of all events that this organizer has created in the past.
- Favourites summary: List the top X rated events (including locations, attendance and event types) that has occurred within the last Y months.