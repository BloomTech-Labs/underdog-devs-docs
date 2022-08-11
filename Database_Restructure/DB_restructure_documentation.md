**Last updated: 07/27/2022**

# Database Restructuring

Included files:
- restructuring_research.xls
- mongodb_schema_design.xls

## Purpose

Restructure the Postgres and MongoDB databases to be independent, clean, and comprehensible. To maintain data security, the responsibilities of the two databases are:

- The Postgres database will store only authentication/authorization information in conjunction with the project's use of Okta/Auth0.
- The MongoDB database will store all other data, i.e. all data collected from users, and backend logic will ensure that users sending or receiving that data on the frontend have the proper authorization.

## Background

In June 2022, Underdog project developers became aware of data duplications in the Postgres and MongoDB databases. When data was added or updated, the changes were made in one of the databases, but often not the other, introducing discrepancies. Even though the project was only using mock data at the time, this data integrity issue needed to be addressed.

The DS team proposed a solution to separate the responsibilities of the two databases so as to remove the duplications while maintaining data security. This solution is stated in the "Purpose" section above.

The first step taken by the DS team was to mirror all of the non-authentication tables from the Postgres database into the MongoDB database (as collections). Only the `schema.py` file in the DS codebase was altered in this process; no data generators were created for the new fields, and no changes were made to the API to reflect the additions. It was expected that as the restructure design was created and implemented in the codebase, the related collections mirrored from Postgres would be deleted.

The DS team then created a Google spreadsheet as a brainstorming platform for consolidating non-authentication-related fields from the relationship-based Postgres tables into the document-based MongoDB collections (restructuring_research.xls).

At the time, there had been a sizeable influx of DS students into the project with few Web students to complement. The current Web students on the project were occupied with priority web-related features, resulting in little Frontend/Backend input in restructuring the database. As a result, the DS team resorted to their own research, deciphering the frontend/backend codebases themselves, and making their best educated guesses as to how the Web team would access the data, and consequently, how to consolidate the Postgres tables/fields into MongoDB collections.

The first round of redesign was completed in the Google spreadsheet. From there, the DS team needed an approach to deconstructing this design into manageable sections of work consistent with AGILE development, while maintaining enough independence in the work to avoid disrupting the effectiveness of the Web team. Their solution was to approach changes to the codebase 1-2 collections at a time:

1. Alter or add the collection(s) to the `schema.py` file in the DS codebase (including the respective update collection(s))
2. Make the respective changes to the data generators for the altered collection(s) in the `generators.py` file in the DS codebase
3. Ensure the new data generators will operate with the `seeds.py` file in the DS codebase
4. Ensure that the changes made in the previous 3 steps will operate with the DS API calls in the `api.py` file in the DS codebase

With these steps completed, the DS team would notify the Web team of the changes, and the Web team would make the necessary alterations to the forms in the frontend and to the data pipeline in the backend.

**NOTE!** This current process is insufficient and will need to be changed to ease the development process for both DS and Web - to reiterate, it was developed by the DS team in near-total absence of participation by the Web team. **It is highly recommended that moving forward, DS and Web both have specific members dedicated to the restructuring.**

The DS team would repeat this process for the other collections as indicated by the design, and once Web had connected their forms and data pipelines to the MongoDB database (and disconnected their forms and pipelines from the duplicate tables in Postgres), the Web team would be responsible for deleting all non-authentication tables from the Postgres database.

## Current State

There is a current design for what collections will be held in the `schema.py` file in the DS codebase (mongodb_schema_design.xls). It currently contains 5 collections:
- Mentor
- Mentee
- Meeting
- Resource
- Ticket

Mentor, Mentee, and Meeting have:
Create class added to `schema.py`, update class added to `schema.py`, data generators created in `generators.py`, changes operate with `seeds.py` and API calls (to the best knowledge of the DS team).

Resource has a class in `schema.py` but it has not been updated with changes. **NOTE:** There is also a 'Resources' class, which is residual from the Postgres table mirroring.

Ticket does not have a class. **NOTE:** There is a TicketsTable class which appears to have the fields for the Ticket collection in the new design, but it does not have an update class.

There are other collections in the `schema.py` file not discussed thus far:

- The Feedback class, as well as its Update and Options class, are currently used for development purposes to build DS models and visualizations (NLP, vader sentiment score, etc.).
- There are still a number of collections which are residuals from the Postgres mirroring: MentorIntake, Role, Comments, Notes, MenteeProgression, Assignment, Reviews



## Future Needs and Considerations (Roadmap)

### Current Collections

The Mentor, Mentee, and Meeting collections are consistent with the needs of the minimum viable product (MVP). Resource is a stretch goal.

The changes to the Mentor and Mentee collections have introduced bugs in the frontend/backend logic. This is currently being addressed, but as of now is yet to be completed.

There is a lot of work to be done by frontend for collecting and displaying meeting information. There is a calendar displayed on the website but there do not appear to be any Meeting forms for users.

### Ticket

Ticket collection is needed for MVP but it is undecided whether this will be a single collection or a set of collections that will all appear as tickets on the website. There are two approaches (so far) and one will need to be chosen before creating the collections:

1. All tickets are consolidated into a single collection and the type of ticket is indicated by a `ticket_type` with options (which may grow if new ticket types are added) `['escalation', 'application', 'resource']`. 
2. Ticket types are separated into unique collections and are consolidated into tickets displayed on the website via backend/frontend logic.

A single collection is easier to use and modify (and reduces the number of generators and API calls required), but different ticket types may have different fields, and any fields which are not shared by all ticket types must, by necessity of using Pydantic in the `schema.py` file, be optional. The current expectation of an optional field in `schema.py` is that the field is truly optional - it can either be filled, or not, with rare exceptions. A single tickets collection with these optional fields would be in violation of this expectation - some of the optional fields would be required based on the ticket type. Should backend pass data without these required fields, the schema would allow the data to be stored because of the optional tag, even though there is required data missing.
For example, a `resource` ticket may have a required field `resource_id`. Since not every ticket has this field, it would be marked as optional in the collection schema. Backend might pass data for a `resource` ticket lacking this `resource_id` field to the database and it would be accepted as valid, even though the field is supposed to be required.

The development team will need to decide which of the two approaches will better suit the needs of the project.

### Miscellaneous Note Collections

There is still a lot of confusion over the miscellaneous note tables in Postgres (Reviews, Notes, Comments) and how they are used on the frontend. There was some mention of a 'Memos' section to be added for admin use, but clarification is needed on how it relates to these note tables. This will need further investigation, and the development team will need to determine if it is prudent to keep these tables as separate collections in MongoDB, if they are kept at all.

### Naming Conventions

Considering the complications of using ambiguous naming conventions in the "Miscellaneous Note Collections" section above, the development team is expected to make names for collections *AND their fields* be clear, comprehensible, and unambiguous.

### Assignments

There has been a significant amount of discussion around how the assignments (pairings of mentors with mentees) will be represented in the database. Some ideas were:

1. A separate collection for assignments
2. Assignments as a field (list of profile_ids) in Mentor or Mentee collection

These were based on the understanding that the assignments would be somewhat concrete, and once a mentor/mentee pairing was made, this pair would have regular meetings. However, due to the natural volatility of having volunteer mentors (also, the stakeholder has been clear on the fact that a dedicated assignment is asking too much of the mentors), it's unlikely that tracking explicit assignments is a pragmatic approach.

The DS team has lightly discussed using meetings with positive feedback from mentors and mentees as an indication that the pair should be matched in the future (and might be implemented in a matching algorithm), but this is unnecessary for the MVP.

### Mentee Progression

Mentee progression was a way to track where a mentee was in the underdog program, which would be marked with one of several text values.

This has been previously discussed, and it was determined that tracking a mentee's specific progression status within the program is unnecessary. A mentee's status in the program will be tracked with the simpler `is_active` boolean in the Mentee collection.

### Feedback

Feedback has been brought up in many meetings but exact details have not been decided. There's generally been two ideas:

1. Feedback is provided by mentee (and optionally for mentors) after a meeting has concluded. This is feedback on how the meeting went, if the individual thought their pairing was a compatible match, etc. This is the only place for a mentor or mentee to provide feedback.
2. Feedback can be provided outside of meetings. The purpose and nature of feedback outside of meetings is undecided.

If feedback can only be provided after meetings, it makes sense to keep feedback in the meeting collection and delete the separate Feedback collection(s).
If feedback can be provided outside of meetings, it should remain a separate collection.

Nothing necessarily needs to be changed in the feedback collections for the MVP, but it should be decided if/how NLP and sentiment analysis will be applied to meeting feedback (the analyses currently only use the Feedback collection).

## Updating this Documentation

Underdog developers are expected to update the "Current State" and "Future Needs and Considerations" sections as progress is made.

restructuring_research.xls is historical and should not be altered; only use this for reference.

mongodb_schema_design.xls should be updated as changes are made to the design and the codebase, especially when a decision is reached regarding adding or altering a collection (Ticket, Feedback).