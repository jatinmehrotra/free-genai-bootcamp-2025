# Backend Server Technical Specs

## Business Goals

A language learning school wants to build a prototype of learning portal which will act as three things:

- Inventory of possible vocabulary that can be learned
- Act as a Learning record store (LRS), providing correct and wrong score on practice vocabulary
- A unified launchpad to launch different learning apps

## Technical Requirements

- Use SQLite3 as the database
- Use Flask as the web framework
- Use Python as the programming language
- Api will always return json format
- There will be no authentication or authorization. Everything will be treated as single user.

## Database Schema

![Database Schema](./Screenshot%202025-02-11%20at%2012.16.03.png)

- `words` table will store all the vocabulary
  - `id` integer
  - `japanese` string
  - `romaji` string
  - `english` string
  - `parts` json
- `words_groups` join table for many-to-many relationship between words and groups
  - `id` integer
  - `word_id` integer
  - `group_id` integer
- `groups` thematic group of words
  - id integer
  - name string
- `study_sessions` record of study sessions grouping word review items
  - id integer
  - group_id integer
  - created_at datetime
  - study_activity_id integer
- `study_activities` a specific study activity linking study session to a group
  - id integer
  - study_session_id integer
  - group_id integer
  - created_at datetime
- `word_review_items` record of word practice, determining if the word was Practice Correct or Wrong
  - word_id integer
  - study_session_id integer
  - correct boolean
  - created_at datetime


  ## API Endpoints

- Dashboard
  - `GET /api/dashboard/last-study-sessions/`
  - `GET /api/dashboard/stats`
  - `GET /api/dashboard/study-progress`
- Study Activities
  - `GET /api/study-activities/:id`
  - `GET /api/study-activities/:id/study_sessions`
  - `POST /api/study-activities/`
    - required params: group_id, study_activity_id
- Words
  - `GET /api/words`
    - Pagination with 100 items per page
  - `GET /api/words/:id`
- Groups
  - `GET /api/groups`
    - Pagination with 100 items per page
  - `GET /api/groups/:id`
  - `GET /api/groups/:id/words`
  - `GET /api/groups/:id/study-sessions`
- Sessions
  - `GET /api/study-sessions/:id`
  - `GET /api/study-sessions/:id/words`
- Settings
  - `POST /api/reset_history`
  - `POST /api/full_reset`

- POST /api/study_sessions/:id/words/:word_id/review
  - required params: correct