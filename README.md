
# Design Create Query

This is part of my course to design, create and querying a database. It involved a problem statement and the definition/constraints/features of the database and the problems.

<details>

  <summary>
    Click to expand the given problem and the project constraints.
  </summary>


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

</details>


## Table Creation

You can find the sql to run [here](create.sql) and a separate markdown for table creation read [here](create.md)

Or you can directly expand the section below to read.

<details>
  <summary>
    Details for creating different tables and populating data
  </summary>

  ### Create database

  Uncomment and use below DROP statement if you need start from the start
  ```sql
  DROP DATABASE SocialEvents;
  ```

  ```sql
  CREATE DATABASE SocialEvents;
  Use SocialEvents;
  ```

  ### Students Table: CREATE AND INSERT

  ```sql
  CREATE TABLE Students (
      stud_id VARCHAR(20) UNIQUE PRIMARY KEY,
      stud_email VARCHAR(50) UNIQUE NOT NULL,
      stud_name VARCHAR(50) NOT NULL,
      stud_phone VARCHAR(12) UNIQUE NOT NULL
  );
  ```
  - Add some sample data
  ```sql
  INSERT INTO Students (stud_id, stud_email, stud_name, stud_phone) VALUES
      ("STUD000023457", "STUD000023457@swansea.ac.uk", "Mayank Purwaar", "9876543211"),
      ("STUD000023456", "STUD000023456@swansea.ac.uk", "Praveen Singh", "9876543210"),
      ("STUD000023458", "STUD000023458@swansea.ac.uk", "Suman", "9876543219"),
      ("STUD000023459", "STUD000023459@swansea.ac.uk", "Joshua Isebor", "9876543212"),
      ("STUD000023460", "STUD000023460@swansea.ac.uk", "Swathi", "9876543213"),
      ("STUD000023462", "STUD000023462@swansea.ac.uk", "Nizwa", "9876543215"),
      ("STUD000023463", "STUD000023463@swansea.ac.uk", "Pamela", "9876543216"),
      ("STUD000023464", "STUD000023464@swansea.ac.uk", "Rahul", "9876543217"),
      ("STUD000023465", "STUD000023465@swansea.ac.uk", "Vedanta", "9876543218");
  ```

  ### Organiser Table: CREATE AND INSERT
  ```sql
  CREATE TABLE Organiser (
      organiser_id VARCHAR(20) UNIQUE PRIMARY KEY,
      organiser VARCHAR(50) UNIQUE NOT NULL,
      organiser_phone VARCHAR(12) UNIQUE NOT NULL,
      organiser_email VARCHAR(50) UNIQUE NOT NULL
  );
  ```
  - ALTER TABLE Organiser ADD PRIMARY KEY (organiser_id);
  ```sql
  INSERT INTO Organiser (organiser_id, organiser, organiser_email, organiser_phone) VALUES
      ("ORG000023456", "Sports Club", "ORG000023456@swansea.ac.uk", "09876543210"),
      ("ORG000023457", "Swansea Cricket Club", "ORG000023457@swansea.ac.uk", "09876543211"),
      ("ORG000023458", "Swansea Badminton Club", "ORG000023458@swansea.ac.uk", "09876543212"),
      ("ORG000023459", "Swansea Union", "ORG000023459@swansea.ac.uk", "09876543213"),
      ("ORG000023460", "Swansea Paint Club", "ORG000023460@swansea.ac.uk", "09876543214");
  ```

  ### Location Table: CREATE AND INSERT
  ```sql
  CREATE TABLE Location (
      loc_id SERIAL PRIMARY KEY,
      loc_address VARCHAR(200) UNIQUE NOT NULL,
      loc_zip VARCHAR(7) NOT NULL,
      loc_gmap_link VARCHAR(5000)
  );
  ```
  ```sql
  INSERT INTO Location (loc_address, loc_zip, loc_gmap_link)
  VALUES
      ("Cricket Club, Bay Sports Park", "SA1 8EN", "https://www.google.com/maps/place/Bay+Sports+Centre/@51.6187179,-3.8821509,19z/data=!4m6!3m5!1s0x486e6077db135999:0x6f4fed9b36232128!8m2!3d51.6188405!4d-3.8817693!16s%2Fg%2F11cmdgvqys?entry=ttu"),
      ("Badminton Club, Bay Sports Park", "SA1 8EN", "gmap location for badminton club"),
      ("JCs, Singleton Campus", "SA1 7EN", "gmap location for JCs"),
      ("Room 213, The College, Bay Campus", "SA1 8EN", "gmap location for The College"),
      ("Room GH212, Greate Hall, Bay Campus", "SA1 8EN", "gmap location for Great Hall");
  ```



  ### Event Table: CREATE AND INSERT
  - This tables is important as any event created will have an organiser and a location. And these attributes need to already exist any event cannot have imaginary location and organiser. So we add these attributes as "Foreign Key"
  ```sql
  CREATE TABLE Event (
      event_id SERIAL PRIMARY KEY,
      event_name VARCHAR(50) NOT NULL,
      event_date DATETIME NOT NULL,
      event_type VARCHAR(20),
      event_organiser VARCHAR(20) NOT NULL,
      event_loc BIGINT(20) UNSIGNED,
      FOREIGN KEY (event_organiser) REFERENCES Organiser(organiser_id),
      FOREIGN KEY (event_loc) REFERENCES Location(loc_id)
  );
  ```
  - Constraint to not let the location be double booked
  ```sql
  ALTER TABLE Event ADD UNIQUE KEY loc_available(event_date, event_loc);
  ```

  - Let organisers CREATE new event
  ```sql
  INSERT INTO Event(event_name, event_date, event_type, event_organiser, event_loc)
  VALUES
      ("Cricket Trials", "2023-11-30 11:00:00", "Sports", "ORG000023457", 1),
      ("Badminton Trials", "2023-11-30 12:00:00", "Sports", "ORG000023458", 1),
      ("Freshers Party", "2023-11-30 20:00:00", "Social", "ORG000023459", 3),
      ("Painting Competition", "2023-11-30 11:00:00", "Arts", "ORG000023460", 4),
      ("Hiking", "2023-12-01 09:00:00", "Sports", "ORG000023457", 3),
      ("Meet and Mingle", "2023-12-02 18:00:00", "Social", "ORG000023459", 3),
      ("Badminton Club Meet", "2023-12-02 11:00:00", "Social", "ORG000023458", 2),
      ("Cricket Club Meet", "2023-12-04 11:00:00", "Social", "ORG000023457", 1),
      ("Movie Night", "2023-11-30 18:00:00", "Social", "ORG000023459", 5),
      ("Party Night", "2023-12-11 21:00:00", "Social", "ORG000023459", 1),
      ("Quiz Night", "2023-12-10 19:00:00", "Social", "ORG000023459", 5),
      ("Bowling Night", "2023-12-09 20:00:00", "Social", "ORG000023460", 3),
      ("Party Night", "2023-12-11 21:00:00", "Social", "ORG000023459", 5);
  ```

  - INSERT query to demonstrate that 2 events cannot take place at the same time and at the same place
  ```sql
  INSERT INTO Event (event_name, event_date, event_type, event_organiser, event_loc)
  VALUES  ("Event same time & place", "2023-11-30 11:00:00", "Some Event", "ORG000023459", 1);
  ```

  ### Registration Table: CREATE

  ```sql
  CREATE TABLE Registration (
      student_reg_date DATETIME,
      student_rating DECIMAL DEFAULT 0 NOT NULL,
      stud_id VARCHAR(20) NOT NULL,
      event_id BIGINT(20) UNSIGNED,
      FOREIGN KEY (stud_id) REFERENCES Students(stud_id),
      FOREIGN KEY (event_id) REFERENCES Event(event_id)
  );
  ```
  - Primary key as combination of stud_id and event_id. A student should only registers once for any event
  ```sql
  ALTER TABLE Registration ADD PRIMARY KEY (stud_id, event_id);
  ```
  - Rating should be within some range. Else impractical values may be submitted for rating any event.
  ```sql
  ALTER TABLE Registration ADD CHECK (student_rating <= 5.0 AND student_rating >= 0.0);
  ```


  - Let Students register for the events
    - The student can only register to an event if the event is not over.

  <!-- INSERT INTO Registration (student_reg_date, stud_id, event_id, student_rating)
  VALUES (NOW(), "STUD000023456", "01", (SELECT 0 FROM Event WHERE event_id = "01" AND event_date > NOW())); -->
  <!-- -- Since I do not know when will this will be checked and marked the followig query use exact datetime instead of NOW() as in above. -->
  ```sql
  INSERT INTO Registration (student_reg_date, stud_id, event_id, student_rating)
  VALUES (NOW(), "STUD000023457", "01", (SELECT 0 FROM Event WHERE event_id = "01" AND event_date > NOW())),
        (NOW(), "STUD000023457", "02", (SELECT 0 FROM Event WHERE event_id = "02" AND event_date > NOW())),
        (NOW(), "STUD000023458", "02", (SELECT 0 FROM Event WHERE event_id = "02" AND event_date > NOW())),
        (NOW(), "STUD000023459", "03", (SELECT 0 FROM Event WHERE event_id = "03" AND event_date > NOW())),
        (NOW(), "STUD000023458", "03", (SELECT 0 FROM Event WHERE event_id = "03" AND event_date > NOW())),
        (NOW(), "STUD000023459", "05", (SELECT 0 FROM Event WHERE event_id = "05" AND event_date > NOW())),
        (NOW(), "STUD000023460", "05", (SELECT 0 FROM Event WHERE event_id = "05" AND event_date > NOW())),
        (NOW(), "STUD000023462", "06", (SELECT 0 FROM Event WHERE event_id = "06" AND event_date > NOW())),
        (NOW(), "STUD000023463", "06", (SELECT 0 FROM Event WHERE event_id = "06" AND event_date > NOW())),
        (NOW(), "STUD000023457", "03", (SELECT 0 FROM Event WHERE event_id = "03" AND event_date > NOW())),
        (NOW(), "STUD000023457", "04", (SELECT 0 FROM Event WHERE event_id = "04" AND event_date > NOW())),
        (NOW(), "STUD000023457", "05", (SELECT 0 FROM Event WHERE event_id = "05" AND event_date > NOW())),
        (NOW(), "STUD000023457", "06", (SELECT 0 FROM Event WHERE event_id = "06" AND event_date > NOW())),
        (NOW(), "STUD000023457", "07", (SELECT 0 FROM Event WHERE event_id = "07" AND event_date > NOW())),
        (NOW(), "STUD000023457", "08", (SELECT 0 FROM Event WHERE event_id = "08" AND event_date > NOW())),
        (NOW(), "STUD000023457", "09", (SELECT 0 FROM Event WHERE event_id = "09" AND event_date > NOW())),
        (NOW(), "STUD000023457", "10", (SELECT 0 FROM Event WHERE event_id = "10" AND event_date > NOW())),
        (NOW(), "STUD000023457", "11", (SELECT 0 FROM Event WHERE event_id = "11" AND event_date > NOW())),
        (NOW(), "STUD000023457", "12", (SELECT 0 FROM Event WHERE event_id = "12" AND event_date > NOW()));
  ```
  <!-- INSERT INTO Registration (student_reg_date, stud_id, event_id, student_rating)
  VALUES ("2023-11-21 22:12:34", "STUD000023462", "07", (SELECT 0 FROM Event WHERE event_id = "07" AND event_date > "2023-11-21 22:12:34")); -->

  - A student can try to register after the event is over, for this following query will give error, kindly uncomment query to run.
  ```sql
  -- INSERT INTO Registration (student_reg_date, stud_id, event_id, student_rating)
  -- VALUES ("2023-12-30 22:12:34", "STUD000023460", "09", (SELECT 0 FROM Event WHERE event_id = "09" AND event_date > "2023-12-30 22:12:34"));
  ```

  ### Rate the EVENTS

  - Student can only rate events for which they have registered before the event date

  ```sql
  UPDATE Registration, Event
  SET student_rating=4
  WHERE
      Registration.stud_id="STUD000023458" AND Registration.event_id=3 AND Event.event_id=3 AND Event.event_date<=NOW();
  ```

  ### Create VIEWS to use in further queries

  - These views are used later in query section of this task.
  - These views are going to simplify my queries a lot in later query section.
  - Plus creating these views also restrict access to data as needed.

  ```sql
  CREATE VIEW AvgEventRating
  AS
      SELECT event_id, AVG(student_rating) AS event_rating FROM Registration GROUP BY event_id;
  ```
  ```sql
  CREATE VIEW EventLoc
  AS
      SELECT event_name, event_date, event_type, event_organiser, L.loc_address AS event_location, event_id
      FROM
          Event AS E
          LEFT JOIN Location AS L -- this join is to get location address from location ID
          ON L.loc_id = E.event_loc;
  ```
</details>


## Reports

You can find separate reports SQL's [here](query.sql) and a separate query document [here](query.md)

OR directly expand the section below

<details>
  <summary>
    SQL to show different reports based on data available
  </summary>

  - Select the correct Database
  Use SocialEvents;

  ### Student Report

  - Show the information on a student including last 10 events attended and the average of how they rated all previous events
  ```
  SELECT
      E.event_name AS "Event Name",
      E.event_date AS "Event Date",
      E.event_type AS "Type of Event",
      E.stud_name AS "Student Name"
  FROM
      (
          SELECT
              E.event_name AS event_name,
              E.event_date AS event_date,
              E.event_type AS event_type,
              S.stud_name AS stud_name
          FROM
              Event AS E
          RIGHT JOIN
              (
                  SELECT S.stud_name, S.stud_id, R.event_id
                  FROM
                      Students AS S
                  RIGHT JOIN
                      (
                          SELECT event_id, stud_id FROM Registration WHERE stud_id="STUD000023457"
                      ) AS R
                      ON S.stud_id=R.stud_id
              ) AS S
          ON E.event_id=S.event_id
          WHERE
              E.event_date < "2023-12-25" -- Why fix date instead of NOW()? Again, because enough data and supported data may not be present
          ORDER BY E.event_date
      ) AS E
  JOIN
      (
          SELECT AVG(student_rating) AS event_rating FROM Registration WHERE stud_id="STUD000023457"
      ) AS R
  LIMIT 10;
  ```

  ### What’s coming
  <!-- - IMP QU, does the rating needs to be average of event rating for the organiser, or the average of rating of the organiser by student -->
  - List all coming events of a certain type in a certain area. Include the average rating of all events that this organizer has created in the past.
  ```
  SELECT
      E.event_name AS "Event Name",
      E.event_date AS "Event Date",
      E.event_type AS "Event Type",
      E.event_organiser AS "Event Organiser",
      E.event_location AS "Event Location",
      AVG(R.event_rating) AS "Organiser Rating"
  FROM
      (
          SELECT * FROM EventLoc
          WHERE
              event_date > NOW() AND
              event_type in ("Sports", "Arts", "Social") AND
              event_location in ("Cricket Club, Bay Sports Park", "Badminton Club, Bay Sports Park", "JCs, Singleton Campus", "Room 213, The College, Bay Campus", "Room GH212, Greate Hall, Bay Campus")
      ) AS E
      RIGHT JOIN (
          SELECT E.event_organiser AS event_organiser, AVG(event_rating) AS event_rating
          FROM
              Event AS E
          LEFT JOIN 
              (
                  SELECT * FROM AvgEventRating
              ) AS R
          ON E.event_id=R.event_id GROUP BY E.event_organiser
      ) AS R
      ON R.event_organiser=E.event_organiser
  WHERE
      E.event_name IS NOT NULL
  GROUP BY
      E.event_id
  ORDER BY
      E.event_date;
  ```

  ### Favourites summary
  - List top rated events that occured in last few months

  ```
  SELECT
      E.event_name AS "Event Name",
      E.event_type AS "Event Type",
      E.event_location AS "Event Location",
      E.attendance AS "Number of Student Attended",
      R.event_rating AS "Event Rating"
  FROM
      (
          SELECT
              E.event_name AS event_name,
              E.event_type AS event_type,
              E.event_location AS event_location,
              R.attendance AS attendance,
              E.event_date AS event_date,
              E.event_id AS event_id
          FROM
              (
                  SELECT * FROM EventLoc
                  WHERE
                      event_date > NOW() - INTERVAL 5 MONTH AND
                      event_date < "2023-12-25" -- instead of "NOW()" I am using an exact date since it may happen that the event still have not arrived at the time of this code being run
              ) AS E
          LEFT JOIN
              (
                  SELECT COUNT(*) AS attendance, event_id FROM Registration GROUP BY event_id
              ) AS R
          ON E.event_id = R.event_id
      ) AS E
  LEFT JOIN (SELECT * FROM AvgEventRating) AS R
  ON E.event_id=R.event_id
  ORDER BY R.event_rating DESC;
  ```
</details>