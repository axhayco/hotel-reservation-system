# Presentation and viva guide

## 5-minute demonstration

1. **Problem and goal (30 sec):** “This system gives a hotel operator a desktop interface to manage rooms, guests and reservations, with MySQL persistence.”
2. **Architecture (60 sec):** show `HotelApp` (Swing screens and event listeners), `Db` (JDBC connections and startup seed), `Models` (typed records), and `schema.sql` (relational structure and constraints).
3. **CRUD (60 sec):** add/edit a room and guest; explain that prepared statements parameterize values and the foreign keys preserve reservation references.
4. **Validation (60 sec):** make a reservation, attempt overlapping dates for the same room, then try back-to-back dates. Explain the half-open interval overlap test and cancelled status behavior.
5. **Files and collections (45 sec):** export CSV; save/load serialized reservations. Point out the map, array list and linked list used in loaded application data.
6. **Wrap-up (30 sec):** mention the single-operator scope and possible next steps: transactions for concurrent bookings, authentication and automated tests.

## Likely viva questions

**Why JDBC?** It is Java's standard API for connecting to relational databases and executing parameterized SQL.

**Why prepared statements?** They separate SQL structure from values, handle quoting and reduce SQL injection risk.

**How do you detect overlapping stays?** A requested `[start,end)` conflicts with a booked `[oldStart,oldEnd)` when `oldStart < end && oldEnd > start`. A checkout at the exact next check-in is therefore permitted.

**Why keep cancelled reservations?** They preserve a useful audit/history row while being excluded from availability checks.

**Where is CRUD implemented?** Room and guest screens provide insert, select/list, update and delete; reservation creation/listing/cancellation are the relevant operations for bookings.

**What does serialization do here?** It writes the serializable reservation model list into a `.ser` file. Loading is intentionally for review and does not alter persistent database rows.

**What constraints are enforced by MySQL?** Unique room number/email, positive capacity, non-negative rate, valid date order, foreign keys and indexed room/date lookup.

**What is a limitation?** The overlap query and insert are not one transaction. Two simultaneous users could race; a production design should use a transaction with appropriate locking/isolation and stronger concurrency controls.

## Important source files

- `src/main/java/hotel/HotelApp.java`: Swing UI, actions, validations, CRUD and export/backup.
- `src/main/java/hotel/Db.java`: connection configuration and sample room initialization.
- `src/main/java/hotel/Models.java`: room, guest and serializable reservation data types.
- `src/main/resources/schema.sql`: schema and integrity constraints.
