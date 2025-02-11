## Frontend Technical Specs

## Pages

### Dashboard `/dashboard`

#### Purpose

The purpose of this page is to provide the summary of learning and act as a default page when user visits the web app.

#### Components

This page contains the following elements:

- Last study session
  - shows last activity used
  - shows when last activity was used
  - summarizes wrong vs correct of last activity
  - has a link to group
- Study Progress
  - Total words studied eg 3/124
    - across all study session show the total words studied out of all possible words in our database
    - Display a mastery progress
    - 
- Quick Stats
  - Success rate eg. 80%
  - total study sessions eg. 4
  - total study groups eg. 3
  - study streak eg. 4 days
  
- Start Studying Button
  - go to study activities page

## Needed API Endpoints

GET /api/dashboard/last-study-sessions/
GET /api/dashboard/stats
GET /api/dashboard/study-progress

### Study Activities Index `/study-activities`

#### Purpose

The purpose of this page is to show a collection of study activities with thumbnail and its name, to either launch or view the study activity

#### Components

- Study Activity Card
  - Show a thumbnail of the study activity
  - The name of the study activity
  - "Launch" button to launch the study activity
  - "View" button to view more information about past study sessions for this study activity

### Needed API Endpoints

- GET /api/study-activities

----

### Study Activity `/study-activities/:id`

#### Purpose

- The purpose of this page is to show details about a study activity and its past study sessions

#### Components

- Name of the study activity
- Thumbnail of the study activity
- Launch button to launch the study activity
- Study Activities Paginated list
  - id
  - Activity Name
  - Group Name
  - start time 
  - end time(inferred by the last word_review_item submitted)
  - number of review items

#### Needed API Endpoints

- GET /api/study-activities/:id
- GET /api/study-activities/:id/study_sessions


### Study Activities Launch `/study-activities/:id/launch`

#### Purpose

The purpose of this page is to launch a study activity

#### Components

- name of study activity
- Launch form
  - select field for group
  - launch button

#### Behaviour

After the form is submitted, a new tab open with the study activity based on its URL provided in the database

Also, after form is submitted the page will redirect to the study session show page.

#### Needed API Endpoints

- POST /api/study-activities/


#### Words Index `/words`

#### Purpose

The purpose of this page is to show all the words in the database.

#### Components 

- Paginated Word List
  - Fields
    - Japanese
    - Romaji
    - English
    - Correct Count
    - Wrong Count
  - Pagination with 100 items per page
  - Clicking the japanese word will take us to word show page

#### Needed API Endpoints

- GET /api/words


### Word show Index `/words/:id`

#### Purpose

The purpose of this page is to show information about specific word.

#### Components 

- Word
  - Japanese
  - Romaji
  - English
  - Study Stats
    - Correct Count
    - Wrong Count
  - Word Groups
    - shown as series of pills eg. tags
    - when group name is clicked

#### Needed API Endpoints

- GET /api/words/:id


### Word Groups Index `/groups`


#### Purpose

- The purpose of this page is to show a list of word groups in the database

#### Components 

- Paginated list of word groups
  - Coloumns
    - Group Name
    - Word Count
  - Clicking the group name will take us to word group show page

#### Needed API Endpoints

 - `GET /api/groups`

### Word Group show `/groups/:id`

#### Purpose

- The purpose of this page is to show information about a specific word group

#### Components 

- Group Name
- Group Statistics
  - Total Word Count
- Words in Group (Paginated list of words)
  - should use the same component as the Words Index page
- Study Sessions (paginated list of study sessions)
  - Should use the same component as the Study session Index page
  
#### Needed API Endpoints

- `GET /api/groups/:id` (the name and group stats)
- `GET /api/groups/:id/words`
- `GET /api/groups/:id/study-sessions`

### Study Sessions Index `/study-sessions`

#### Purpose

- The purpose of this page is to show a list of study sessions in the database

#### Components 

- Paginated list of study sessions
  - Columns
    - Id
    - Activity Name
    - Group Name
    - Start Time
    - End Time
    - Number of Review Items
  - Clicking the study session name will take us to study session show page

#### Needed API Endpoints

- `GET /api/study-sessions`


#### Study Session Show `/study-sessions/:id`

#### Purpose

- The purpose of this page is to show information about a specific study session

#### Components

- Study Session Details
  - Activity Name
  - Group Name
  - Start Time
  - End Time
  - Number of Review Items
- Words Reviewed
  - should use the same component as the Words Index page

#### Needed API Endpoints

- `GET /api/study-sessions/:id`
- `GET /api/study-sessions/:id/words`


### Settings `/settings`

#### Purpose

The purpose of this page is to make configurations the study portal.

#### Components

- Theme Selection eg. Light, Dark, System
- Reset History
  - This will delete all the study sessions and word review items.
- Full Reset Button
  - This will drop all tables and recreate with seed data.

#### Needed API Endpoints

- POST /api/reset_history
- POST /api/full_reset