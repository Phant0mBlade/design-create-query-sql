
## Though Process in designing

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
Can multiple events be held on same location?