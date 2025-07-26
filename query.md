
## Reports

- Select the correct Database
Use SocialEvents;

### Student Report

- Show the information on a student including last 10 events attended and the average of how they rated all previous events
```sql
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
```sql
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

```sql
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