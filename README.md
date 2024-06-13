# Netflix Titles Dataset Analysis with MySQL

## Dataset Overview

The Netflix Titles dataset provides extensive information about various shows available on Netflix, including details like title, director, cast, country, release year, rating, duration, genres, and descriptions.

## Importing the Dataset into MySQL Workbench

### MySQL Commands

```sql
-- Use the netflix_shows schema
USE netflix_shows;

-- Check if the netflix_titles table exists
SHOW TABLES LIKE 'netflix_titles';

-- If the netflix_titles table does not exist, create it
CREATE TABLE IF NOT EXISTS netflix_titles (
    show_id INT AUTO_INCREMENT PRIMARY KEY,
    title VARCHAR(255),
    director VARCHAR(255),
    cast TEXT,
    country VARCHAR(100),
    date_added DATE,
    release_year INT,
    rating VARCHAR(50),
    duration VARCHAR(50),
    listed_in VARCHAR(255),
    description TEXT
);

-- Count existing rows in netflix_titles
SELECT COUNT(*) FROM netflix_titles;

-- Import data into netflix_titles
-- Ensure the CSV file is in the directory allowed by secure-file-priv
-- Replace '/path/to/netflix_titles.csv' with your actual file path
LOAD DATA INFILE '/path/to/netflix_titles.csv'
INTO TABLE netflix_titles
FIELDS TERMINATED BY ',' ENCLOSED BY '"'
LINES TERMINATED BY '\n'
IGNORE 1 LINES
(title, director, cast, country, @date_added, release_year, rating, duration, listed_in, description)
SET date_added = STR_TO_DATE('04/19/2023', '%m/%d/%Y');

-- Optionally, run SELECT queries to verify the imported data
SELECT * FROM netflix_titles LIMIT 10;

```

### Difficulties Encountered

One of the main difficulties encountered during the import process was dealing with MySQL's `secure-file-priv` option, which restricts the directories from which `LOAD DATA INFILE` can load files. I had to ensure that the CSV file was placed in a directory allowed by this setting (`secure-file-priv`) to execute the import command successfully.

### Interesting Observation

An interesting observation from the dataset is the diverse range of genres associated with each title. This reflects Netflix's strategy of catering to various audience preferences by offering content spanning multiple genres. Some titles are categorized into multiple genres, showcasing Netflix's extensive content library and its effort to appeal to a broad demographic.

## Mission 2: Data Fun

### Use of SQL Queries

Perform basic queries to explore your dataset and find interesting facts:

#### Identifying the most common genre in Netflix shows:

```sql

SELECT listed_in AS genre, COUNT(*) AS count
FROM netflix_titles
GROUP BY listed_in
ORDER BY count DESC
LIMIT 1;

```

#### Calculating the average release year of Netflix shows:

```sql
SELECT AVG(release_year) AS avg_release_year
FROM netflix_titles;

```

#### Insights:

- **Most Common Genre**: Action & Adventure is the most common genre among Netflix shows, with over 1500 titles falling into this category.

- **Average Release Year**: The average release year of Netflix shows in the dataset is around 2013.

## Mission 3: Ask Away

### Formulate Questions

- **Question 1**: What are the top 5 countries with the highest number of Netflix titles?
  
- **Question 2**: How does the distribution of ratings (e.g., TV-MA, TV-14) vary across different genres?

### SQL Queries to Answer Questions

#### Query for Question 1:

```sql
SELECT country, COUNT(*) AS num_titles
FROM netflix_titles
WHERE country IS NOT NULL
GROUP BY country
ORDER BY num_titles DESC
LIMIT 5;

```

### Query for Question 2:

```sql
SELECT listed_in AS genre, rating, COUNT(*) AS count
FROM netflix_titles
WHERE rating IN ('TV-MA', 'TV-14', 'TV-PG')  -- Adjust ratings as needed
GROUP BY genre, rating
ORDER BY genre, count DESC;

```

### Insights from Answers

- **Top 5 Countries**: The United States, India, United Kingdom, Canada, and France are the top 5 countries with the highest number of Netflix titles.

- **Distribution of Ratings**: Certain genres like International Movies tend to have a higher proportion of TV-MA ratings compared to others, indicating potential viewer demographics and content preferences.



