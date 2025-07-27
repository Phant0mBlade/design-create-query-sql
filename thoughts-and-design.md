
## Design choices

we need a database where "students", can attend "events", organiser will organise "events", "events" will be organised at some "place". "students" will be registering to these events so we will require the registration details and confirmation to students.

### Tables

Based on the details in the [problem statement](problem-statement.md) and the simplification above, the tables we will need will be,

1. Students - to store details of the students, so that they can register for events and get confirmation and location details of these events
  - student name, id, email and phone. To have students register and update about events
2. Organiser - the details of organiser, which will create events
  - organiser name, id, phone and email, similar to students
3. Events - actual events created
  - event id, name, date, type, ForeignKey(organiser), ForeignKey(location)
  - Since an organiser will be organising an event and every event will have some location.
4. Location - location of these events
  - location id, address, zip, map link
5. Registration - which students have registered to which events
  - Since, each of this registration is unique corresponding to an event and the student who registered we will need a relation between them too
  - student registration date, student rating, ForeignKey(student id), ForeignKey(event id)
  - PrimaryKey(stud id, event id) - an event is registered for by a student

## DB Contraints

So far we have the tables, there attributes but we also need to discuss there constraints and dependancy(though we showed some already above).

Students table have no contraint. We can have email id and phone number regex on these attributes. So any student that gets added to this database have a valid email and phone. Similary of Organiser table.

Location also do not have any constraint. As we find any venue where these events can be held, we continue to add them in our databse. location id will be primary key with SERIAL

Events have contraints. It can only exist if an organiser and a location exist. So we add organiser id and the location id as the foreign key contraint to this table. event id will be primary key with SERIAL. I also decided that any event venue/location cannot be double booked. For that we add a unique key contraint for event location and event date.

Registration too only exist if a student and an event exist. Without those registration has no meaning. So we also have student id and event id as the foreign key in this table. Event rating within some range

## Some SQL constraints

While updating we need to make sure we follow some contraints.
For example
  1. we cannot let student register for events when the event date has already passed.
  2. we should only let those students rate events who have registered for the events.

These contraints will be reflected in our update statements.

## What else?

I created couple of views to help me with generating reports and metrics for these events. Primary reason to create these views was to simplify my report queries.


<!-- ## Rough notes for thought process in designing

below are some rough notes while designing the above database

Student
find things to do

Organiser

Location
location of event

Event
actual event



STUDENT - Student_id, Student_name, Student_phone
ORGANISER - Event_organiser(PK), Organiser_rating - why is organiser required? - because of D)
LOCATION - Location_id, Location_address(mandatory), Location_available(bool, default=True)

EVENT - Event_id(PK), Event_name(mandatory), Event_date(mandatory), Event_organiser(FK)(mandatory), Event_type, Location_id(mandatory and should be available and should exist in LOCATION)

A) STUDENT_EVENT_REGISTRATION - [Student_id, Event_id(mandatory and only if it exist in Event)], Student_registration_date(less than Event_date else Null), Student_rating(if Student_registration_date exist and currenttime > Event_date)

A) EVENT - Event_id, Event_name(mandatory), Event_date(mandatory), Event_organiser(mandatory), Event_type, Location_id(mandatory and should be available and should exist in LOCATION), primary - Event_name+Event_date

B) ALTER table student where Student_id=x and Student_registration_date is not null and Event_id=x

C) where Student_id=x and Student_registration_date is not null order by Student_registration_date -|- average of all previous events attended - these are two different things

D) where Event_type=x and Event_location=x -|- also show average rating of the organiser for these events


Event_rating(avg of Student_rating) - no actual data, calculate through query

Lets say location is unique for each event, why? At the same location two or more events cannot happen.

Ask, whether event name will be Unique or not?
Can multiple events be held on same location? -->