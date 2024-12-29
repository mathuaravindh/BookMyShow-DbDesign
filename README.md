# BookMyShow Database Design

This repository provides the database schema, sample data, and queries for implementing the **BookMyShow** use case, covering entities like theatres, screens, movies, shows, and bookings.

## Problem Statement

The system manages:
- Theatres and their screens.
- Movies and their associated details.
- Shows scheduled for specific screens on given dates and times.
- Bookings made by users for specific shows.

---

## Database Schema

### Entities and Attributes

1. **Theatre**
   - `theatre_id` (Primary Key)
   - `theatre_name`
   - `address`

2. **Screen**
   - `screen_id` (Primary Key)
   - `screen_name`
   - `capacity`
   - `theatre_id` (Foreign Key – `Theatre.theatre_id`)

3. **Movie**
   - `movie_id` (Primary Key)
   - `movie_name`
   - `genre`
   - `description`
   - `language`

4. **Show**
   - `show_id` (Primary Key)
   - `movie_id` (Foreign Key – `Movie.movie_id`)
   - `screen_id` (Foreign Key – `Screen.screen_id`)
   - `date`
   - `time`

5. **Booking**
   - `booking_id` (Primary Key)
   - `show_id` (Foreign Key – `Show.show_id`)
   - `user_id` (Assumed Foreign Key – `User.user_id`)
   - `no_of_tickets`

---

## Queries

```sql
-- Theatre Table
CREATE TABLE Theatre (
    theatre_id INT AUTO_INCREMENT PRIMARY KEY,
    theatre_name VARCHAR(100) NOT NULL,
    address VARCHAR(255) NOT NULL
);

-- Screen Table
CREATE TABLE Screen (
    screen_id INT AUTO_INCREMENT PRIMARY KEY,
    screen_name VARCHAR(50) NOT NULL,
    capacity INT NOT NULL,
    theatre_id INT NOT NULL,
    FOREIGN KEY (theatre_id) REFERENCES Theatre(theatre_id)
);

-- Movie Table
CREATE TABLE Movie (
    movie_id INT AUTO_INCREMENT PRIMARY KEY,
    movie_name VARCHAR(100) NOT NULL,
    genre VARCHAR(50),
    description TEXT,
    language VARCHAR(50) NOT NULL
);

-- Show Table
CREATE TABLE Show (
    show_id INT AUTO_INCREMENT PRIMARY KEY,
    movie_id INT NOT NULL,
    screen_id INT NOT NULL,
    date DATE NOT NULL,
    time TIME NOT NULL,
    FOREIGN KEY (movie_id) REFERENCES Movie(movie_id),
    FOREIGN KEY (screen_id) REFERENCES Screen(screen_id)
);

-- Booking Table
CREATE TABLE Booking (
    booking_id INT AUTO_INCREMENT PRIMARY KEY,
    show_id INT NOT NULL,
    user_id INT NOT NULL,
    no_of_tickets INT NOT NULL,
    FOREIGN KEY (show_id) REFERENCES Show(show_id)
);

---


-- Sample data
INSERT INTO Theatre (theatre_name, address) VALUES 
('PVR Nexus', 'Forum Mall, Bangalore'),
('INOX Lido', 'MG Road, Bangalore');


INSERT INTO Screen (screen_name, capacity, theatre_id) VALUES 
('Screen 1', 200, 1),
('Screen 2', 150, 1),
('Screen 1', 180, 2);


INSERT INTO Movie (movie_name, genre, description, language) VALUES 
('Dasara', 'Action', 'A gripping story set in rural India.', 'Telugu'),
('Kisi Ka Bhai Kisi Ki Jaan', 'Drama', 'An emotional family saga.', 'Hindi'),
('Avatar: The Way of Water', 'Sci-Fi', 'The epic sequel to Avatar.', 'English');


INSERT INTO Show (movie_id, screen_id, date, time) VALUES 
(1, 1, '2024-04-25', '12:10:00'),
(2, 2, '2024-04-25', '13:00:00'),
(3, 3, '2024-04-25', '19:20:00');


INSERT INTO Booking (show_id, user_id, no_of_tickets) VALUES 
(1, 101, 3),
(2, 102, 2),
(3, 103, 5);


## P2 - Query to Fetch All Shows for a Given Date and Theatre
SELECT 
    T.theatre_name,
    S.screen_name,
    M.movie_name,
    M.language,
    M.genre,
    Sh.date,
    Sh.time
FROM 
    Show Sh
JOIN 
    Screen S ON Sh.screen_id = S.screen_id
JOIN 
    Theatre T ON S.theatre_id = T.theatre_id
JOIN 
    Movie M ON Sh.movie_id = M.movie_id
WHERE 
    Sh.date = '2024-04-25'
    AND T.theatre_name = 'PVR Nexus';




