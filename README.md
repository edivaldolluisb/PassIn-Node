# passIn


PassIn is an application for **managing participants in in-person events**. The app allows organizers to create events, register participants and facilitate check-in on the event day

The tool allows the organizer to register an event and open a public registration page.

Participants registered can issue a credential for check-in on the day of the event.

The system will scan the participant's credential to allow entry to the event.


## Requirements

install dependencies:   
    ```npm install```

!! using sqlite as database, no need to install any database(for test purpose) !!

run: ```npx prisma db seed``` (to seed the database)

run: ```npm run dev```

### Functional requirements

- [x] The organizer must be able to register a new event;
- [x] The organizer must be able to view event data;
- [x] The organizer must be able to view the list of participants;
- [x] The participant must be able to register for an event;
- [x] The participant must be able to view their registration badge;
- [x] The participant must be able to check-in at the event;

### Business rules

- [x] The participant can only register for an event once;
- [x] The participant can only register for events with available vacancies;
- [x] The participant can only check-in at an event once;

### Non-functional requirements

- [x] The check-in at the event will be done through a QRCode;

## API Documentation (Swagger)

To access the API documentation, go to the link: https://nlw-unite-nodejs.onrender.com/docs

## Database

In this application we will use a relational database (SQL). For development environment we will continue with SQLite for the ease of the environment.

### Diagram ERD

<img src=".github/erd.svg" width="600" alt="Diagrama ERD do banco de dados" />

### Database structure (SQL)

```sql
-- CreateTable
CREATE TABLE "events" (
    "id" TEXT NOT NULL PRIMARY KEY,
    "title" TEXT NOT NULL,
    "details" TEXT,
    "slug" TEXT NOT NULL,
    "maximum_attendees" INTEGER
);

-- CreateTable
CREATE TABLE "attendees" (
    "id" INTEGER NOT NULL PRIMARY KEY AUTOINCREMENT,
    "name" TEXT NOT NULL,
    "email" TEXT NOT NULL,
    "event_id" TEXT NOT NULL,
    "created_at" DATETIME NOT NULL DEFAULT CURRENT_TIMESTAMP,
    CONSTRAINT "attendees_event_id_fkey" FOREIGN KEY ("event_id") REFERENCES "events" ("id") ON DELETE RESTRICT ON UPDATE CASCADE
);

-- CreateTable
CREATE TABLE "check_ins" (
    "id" INTEGER NOT NULL PRIMARY KEY AUTOINCREMENT,
    "created_at" DATETIME NOT NULL DEFAULT CURRENT_TIMESTAMP,
    "attendeeId" INTEGER NOT NULL,
    CONSTRAINT "check_ins_attendeeId_fkey" FOREIGN KEY ("attendeeId") REFERENCES "attendees" ("id") ON DELETE RESTRICT ON UPDATE CASCADE
);

-- CreateIndex
CREATE UNIQUE INDEX "events_slug_key" ON "events"("slug");

-- CreateIndex
CREATE UNIQUE INDEX "attendees_event_id_email_key" ON "attendees"("event_id", "email");

-- CreateIndex
CREATE UNIQUE INDEX "check_ins_attendeeId_key" ON "check_ins"("attendeeId");
```