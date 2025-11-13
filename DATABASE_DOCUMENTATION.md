# Movie Database Documentation

## Overview
This database contains information about movies, their associated metadata, cast, and crew from the TMDB 5000 dataset. It was created by converting the `tmdb_5000_movies.csv` and `tmdb_5000_credits.csv` files into a normalized SQLite database schema.

## Database Statistics
- 4,803 movies
- 20 distinct genres
- 9,504 distinct keywords
- 4,887 production companies
- 60,362 people (actors and crew)
- 37,770 cast relationships
- 83,364 crew relationships

## Schema Description

### 1. `movies` table
Primary table containing core movie information.

| Column | Type | Description |
|--------|------|-------------|
| id | INTEGER | Primary key, unique movie identifier |
| budget | INTEGER | Production budget in USD |
| homepage | TEXT | Official movie homepage URL |
| original_language | TEXT | ISO code for the original language |
| original_title | TEXT | Original movie title |
| overview | TEXT | Plot summary or description |
| popularity | REAL | Popularity score |
| release_date | TEXT | Release date in YYYY-MM-DD format |
| revenue | INTEGER | Total revenue in USD |
| runtime | REAL | Movie duration in minutes |
| status | TEXT | Production status (e.g., Released, Post Production) |
| tagline | TEXT | Movie tagline |
| title | TEXT | Movie title |
| vote_average | REAL | Average rating (0-10 scale) |
| vote_count | INTEGER | Number of votes received |

### 2. `genres` table
Contains distinct movie genre information.

| Column | Type | Description |
|--------|------|-------------|
| id | INTEGER | Primary key, unique genre identifier |
| name | TEXT | Name of the genre (e.g., Action, Drama) |

### 3. `keywords` table
Contains distinct movie keywords/tags.

| Column | Type | Description |
|--------|------|-------------|
| id | INTEGER | Primary key, unique keyword identifier |
| name | TEXT | Name of the keyword (e.g., time travel, based on novel) |

### 4. `production_companies` table
Contains information about production companies.

| Column | Type | Description |
|--------|------|-------------|
| id | INTEGER | Primary key, unique company identifier |
| name | TEXT | Name of the production company |

### 5. `production_countries` table
Contains information about production countries.

| Column | Type | Description |
|--------|------|-------------|
| iso_3166_1 | TEXT | Primary key, ISO 3166-1 alpha-2 country code |
| name | TEXT | Full country name |

### 6. `spoken_languages` table
Contains information about spoken languages in movies.

| Column | Type | Description |
|--------|------|-------------|
| iso_639_1 | TEXT | Primary key, ISO 639-1 language code |
| name | TEXT | Full language name |

### 7. `people` table
Contains information about actors and crew members.

| Column | Type | Description |
|--------|------|-------------|
| id | INTEGER | Primary key, unique person identifier |
| name | TEXT | Person's name |
| gender | INTEGER | Gender identifier (0: Unspecified, 1: Female, 2: Male) |

### 8. `movies_genres` table (junction)
Associates movies with their genres.

| Column | Type | Description |
|--------|------|-------------|
| movie_id | INTEGER | Foreign key referencing movies.id |
| genre_id | INTEGER | Foreign key referencing genres.id |

### 9. `movies_keywords` table (junction)
Associates movies with their keywords.

| Column | Type | Description |
|--------|------|-------------|
| movie_id | INTEGER | Foreign key referencing movies.id |
| keyword_id | INTEGER | Foreign key referencing keywords.id |

### 10. `movies_production_companies` table (junction)
Associates movies with their production companies.

| Column | Type | Description |
|--------|------|-------------|
| movie_id | INTEGER | Foreign key referencing movies.id |
| company_id | INTEGER | Foreign key referencing production_companies.id |

### 11. `movies_production_countries` table (junction)
Associates movies with their production countries.

| Column | Type | Description |
|--------|------|-------------|
| movie_id | INTEGER | Foreign key referencing movies.id |
| country_iso | TEXT | Foreign key referencing production_countries.iso_3166_1 |

### 12. `movies_spoken_languages` table (junction)
Associates movies with their spoken languages.

| Column | Type | Description |
|--------|------|-------------|
| movie_id | INTEGER | Foreign key referencing movies.id |
| language_iso | TEXT | Foreign key referencing spoken_languages.iso_639_1 |

### 13. `movies_cast` table (junction)
Associates movies with cast members and their roles.

| Column | Type | Description |
|--------|------|-------------|
| movie_id | INTEGER | Foreign key referencing movies.id |
| person_id | INTEGER | Foreign key referencing people.id |
| cast_id | INTEGER | Unique cast identifier |
| character | TEXT | Name of the character played |
| credit_id | TEXT | Unique credit identifier |
| gender | INTEGER | Gender of the character (0: Unspecified, 1: Female, 2: Male) |
| order_num | INTEGER | Billing order of the actor |

### 14. `movies_crew` table (junction)
Associates movies with crew members and their roles.

| Column | Type | Description |
|--------|------|-------------|
| movie_id | INTEGER | Foreign key referencing movies.id |
| person_id | INTEGER | Foreign key referencing people.id |
| credit_id | TEXT | Unique credit identifier |
| department | TEXT | Department the crew member worked in (e.g., Directing, Sound) |
| gender | INTEGER | Gender of the crew member (0: Unspecified, 1: Female, 2: Male) |
| job | TEXT | Specific job title (e.g., Director, Editor) |

## Indexes
The following indexes have been created to improve query performance:
- `idx_movies_title` on movies(title)
- `idx_movies_release_date` on movies(release_date)
- `idx_movies_vote_average` on movies(vote_average)
- `idx_people_name` on people(name)

## Sample Queries

### Find all movies with a specific genre:
```sql
SELECT m.title, m.release_date, m.vote_average
FROM movies m
JOIN movies_genres mg ON m.id = mg.movie_id
JOIN genres g ON mg.genre_id = g.id
WHERE g.name = 'Action';
```

### Get all cast members of a movie:
```sql
SELECT p.name, mc.character
FROM people p
JOIN movies_cast mc ON p.id = mc.person_id
JOIN movies m ON mc.movie_id = m.id
WHERE m.title = 'Avatar';
```

### Find top-rated movies with a specific keyword:
```sql
SELECT m.title, m.vote_average, m.release_date
FROM movies m
JOIN movies_keywords mk ON m.id = mk.movie_id
JOIN keywords k ON mk.keyword_id = k.id
WHERE k.name = 'time travel' AND m.vote_average > 7.0
ORDER BY m.vote_average DESC;
```

### Find a person's filmography as actor:
```sql
SELECT m.title, m.release_date, mc.character
FROM movies m
JOIN movies_cast mc ON m.id = mc.movie_id
JOIN people p ON mc.person_id = p.id
WHERE p.name = 'Leonardo DiCaprio'
ORDER BY m.release_date;
```

## Data Sources
This database was created from two CSV files:
- `tmdb_5000_movies.csv` - Contains movie metadata including budgets, revenues, ratings, etc.
- `tmdb_5000_credits.csv` - Contains cast and crew information

## Notes
- The original CSV files contained JSON-formatted data in text fields which were parsed and normalized into separate tables
- Foreign key constraints are enforced between related tables
- The database schema is normalized to reduce redundancy while maintaining relational integrity
- Character names and job titles are preserved as they appeared in the original dataset