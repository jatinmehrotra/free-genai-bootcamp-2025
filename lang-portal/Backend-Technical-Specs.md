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

### Dashboard Endpoints

#### `GET /api/dashboard/last-study-sessions/`
Returns the most recent study sessions.
```json
{
  "study_sessions": [
    {
      "id": 1,
      "group_id": 1,
      "group_name": "Basic Greetings",
      "created_at": "2025-02-14T10:30:00Z",
      "study_activity_id": 1,
      "total_words": 10,
      "correct_count": 8
    }
  ]
}
```

#### `GET /api/dashboard/stats`
Returns overall study statistics.
```json
{
  "total_words_studied": 150,
  "total_correct": 120,
  "total_sessions": 15,
  "accuracy_rate": 80.0,
  "groups_studied": 5
}
```

#### `GET /api/dashboard/study-progress`
Returns study progress over time.
```json
{
  "daily_progress": [
    {
      "date": "2025-02-14",
      "words_studied": 20,
      "correct_count": 15
    }
  ]
}
```

### Study Activities Endpoints

#### `GET /api/study-activities/:id`
Returns details of a specific study activity.
```json
{
  "id": 1,
  "study_session_id": 1,
  "group_id": 1,
  "created_at": "2025-02-14T10:30:00Z",
  "total_words": 10,
  "completed_words": 8
}
```

#### `GET /api/study-activities/:id/study_sessions`
Returns all study sessions for a specific activity.
```json
{
  "study_sessions": [
    {
      "id": 1,
      "created_at": "2025-02-14T10:30:00Z",
      "total_words": 10,
      "correct_count": 8
    }
  ]
}
```

#### `POST /api/study-activities/`
Creates a new study activity.
Required params: group_id, study_activity_id
```json
{
  "id": 1,
  "group_id": 1,
  "study_activity_id": 1,
  "created_at": "2025-02-14T10:30:00Z"
}
```

### Words Endpoints

#### `GET /api/words`
Returns paginated list of words.
```json
{
  "words": [
    {
      "id": 1,
      "japanese": "こんにちは",
      "romaji": "konnichiwa",
      "english": "hello",
      "parts": {
        "type": "greeting",
        "formality": "neutral"
      }
    }
  ],
  "page": 1,
  "total_pages": 10,
  "total_items": 1000
}
```

#### `GET /api/words/:id`
Returns details of a specific word.
```json
{
  "id": 1,
  "japanese": "こんにちは",
  "romaji": "konnichiwa",
  "english": "hello",
  "parts": {
    "type": "greeting",
    "formality": "neutral"
  },
  "groups": [
    {
      "id": 1,
      "name": "Basic Greetings"
    }
  ]
}
```

### Groups Endpoints

#### `GET /api/groups`
Returns paginated list of groups.
```json
{
  "groups": [
    {
      "id": 1,
      "name": "Basic Greetings",
      "word_count": 10
    }
  ],
  "page": 1,
  "total_pages": 5,
  "total_items": 500
}
```

#### `GET /api/groups/:id`
Returns details of a specific group.
```json
{
  "id": 1,
  "name": "Basic Greetings",
  "word_count": 10,
  "last_studied": "2025-02-14T10:30:00Z",
  "study_sessions_count": 5
}
```

#### `GET /api/groups/:id/words`
Returns all words in a specific group.
```json
{
  "group_id": 1,
  "group_name": "Basic Greetings",
  "words": [
    {
      "id": 1,
      "japanese": "こんにちは",
      "romaji": "konnichiwa",
      "english": "hello",
      "parts": {
        "type": "greeting",
        "formality": "neutral"
      }
    }
  ]
}
```

#### `GET /api/groups/:id/study-sessions`
Returns study sessions for a specific group.
```json
{
  "group_id": 1,
  "study_sessions": [
    {
      "id": 1,
      "created_at": "2025-02-14T10:30:00Z",
      "total_words": 10,
      "correct_count": 8
    }
  ]
}
```

### Sessions Endpoints

#### `GET /api/study-sessions/:id`
Returns details of a specific study session.
```json
{
  "id": 1,
  "group_id": 1,
  "created_at": "2025-02-14T10:30:00Z",
  "total_words": 10,
  "correct_count": 8,
  "accuracy_rate": 80.0
}
```

#### `GET /api/study-sessions/:id/words`
Returns words reviewed in a specific study session.
```json
{
  "session_id": 1,
  "words": [
    {
      "word_id": 1,
      "japanese": "こんにちは",
      "romaji": "konnichiwa",
      "english": "hello",
      "correct": true,
      "reviewed_at": "2025-02-14T10:30:00Z"
    }
  ]
}
```

### Review Endpoints

#### `POST /api/study_sessions/:id/words/:word_id/review`
Records a word review result.
Required params: correct
```json
{
  "session_id": 1,
  "word_id": 1,
  "correct": true,
  "created_at": "2025-02-14T10:30:00Z"
}
```

### Settings Endpoints

#### `POST /api/reset_history`
Resets all study history.
```json
{
  "success": true,
  "message": "Study history has been reset",
  "reset_at": "2025-02-14T10:30:00Z"
}
```

#### `POST /api/full_reset`
Resets all data including words and groups.
```json
{
  "success": true,
  "message": "All data has been reset",
  "reset_at": "2025-02-14T10:30:00Z"
}