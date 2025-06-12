# DAY-44---Lesson-5-In-a-Snap-Assignment

# CS50 Databases — Lesson 5: In a Snap Assignment

## Overview

This assignment explores SQL queries on a messaging app database, focusing on speed and index usage to support instant picture messaging (similar to Snapchat).

The database contains three tables:

- **users**: User information
- **friends**: Friend relationships (one-way)
- **messages**: Picture messages with expiration timestamps

## Tasks Completed

1. **Find active users** who logged in since 2024-01-01 using an index on `last_login_date`.
EXPLAIN QUERY PLAN
SELECT *
FROM users
WHERE last_login_date >= '2024-01-01';

   
2. **Find message expiration** timestamp for message with ID 151 using primary key indexing.
EXPLAIN QUERY PLAN
SELECT *
FROM messages
WHERE ID = 151;

   
3. **Identify top 3 best friends** a user sends messages to by message count using index on `from_user_id`.
EXPLAIN QUERY PLAN
SELECT to_user_id, COUNT(*) AS message_count
FROM messages
WHERE from_user_id = (
    SELECT id
    FROM users
    WHERE username = 'creativewisdom377'
)
GROUP BY to_user_id
ORDER BY message_count DESC
LIMIT 3;



4. **Find the most popular user** with most messages received using index on `to_user_id`.
EXPLAIN QUERY PLAN
SELECT id, username,
       (
           SELECT COUNT(*)
           FROM messages
           WHERE to_user_id = users.id
       ) AS total_messages_received
FROM users
WHERE id = (
    SELECT to_user_id
    FROM messages
    GROUP BY to_user_id
    ORDER BY COUNT(*) DESC
    LIMIT 1
);



5. **Find mutual friends** (user IDs and usernames) between two users using the auto-generated primary key index on `friends`.
EXPLAIN QUERY PLAN
SELECT username
FROM users
WHERE id IN (
    SELECT friend_id
    FROM friends
    WHERE user_id = 284

    INTERSECT

    SELECT friend_id
    FROM friends
    WHERE user_id = 440
);



## Sample Query — Mutual Friends with Usernames


Tools Used
SQLite3

SQL indexing and query optimization

EXPLAIN QUERY PLAN for performance verification

