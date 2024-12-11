####  🎫 Subscription Table

    CREATE TABLE subscription(
        SubscriptionID INT AUTO_INCREMENT PRIMARY KEY,
        SubscriptionType VARCHAR(50) NOT NULL,
        MonthlyFee DECIMAL(10,2) NOT NULL,
        CHECK (SubscriptionType = 'Basic' OR SubscriptionType = 'Premium')
    );

####  👤 Users Table

    CREATE TABLE users(
        UserID INT AUTO_INCREMENT PRIMARY KEY,
        FirstName VARCHAR(100) NOT NULL,
        LastName VARCHAR(100) NOT NULL,
        Email VARCHAR(100) NOT NULL UNIQUE,
        RegistrationDate DATE NOT NULL,
        SubscriptionID INT,
        FOREIGN KEY (SubscriptionID) REFERENCES subscription(SubscriptionID)
    );

####  🎬 Movie Table
    CREATE TABLE movie(
        MovieID INT AUTO_INCREMENT PRIMARY KEY,
        Title VARCHAR(255) NOT NULL,
        Genre VARCHAR(100),
        ReleaseYear INT,
        Duration INT,
        Rating VARCHAR(10)
    );

####  🌟 Review Table
    CREATE TABLE review(
        ReviewID INT AUTO_INCREMENT PRIMARY KEY,
        UserID INT NOT NULL,
        MovieID INT NOT NULL,
        Rating INT CHECK (Rating BETWEEN 1 AND 10),
        ReviewText TEXT NOT NULL,
        ReviewDate DATE NOT NULL,
        FOREIGN KEY (UserID) REFERENCES users(UserID),
        FOREIGN KEY (MovieID) REFERENCES movie(MovieID)
    );

####  📜 Watch History Table
    CREATE TABLE watchhistory(
        WatchHistoryID INT AUTO_INCREMENT PRIMARY KEY,
        UserID INT NOT NULL,
        MovieID INT NOT NULL,
        WatchDate DATE NOT NULL,
        CompletionPercentage INT DEFAULT 0 CHECK (CompletionPercentage BETWEEN 0 AND 100),
        FOREIGN KEY (UserID) REFERENCES users(UserID),
        FOREIGN KEY (MovieID) REFERENCES movie(MovieID)
    );


#### Question 1

    INSERT INTO movie(Title,Genre,ReleaseYear,Duration,Rating)
    VALUES ("Data Science Adventures","Documentary", 1999, 136, 'R');


#### Question 2

    SELECT * 
    FROM movie
    WHERE Genre = 'Comedy' AND ReleaseYEAR >= 2021;


#### Question 3


    UPDATE users 
    SET SubscriptionID=(SELECT SubscriptionID FROM subscription WHERE SubscriptionType='Premium' LIMIT 1)
    WHERE SubscriptionID=(SELECT SubscriptionID FROM subscription WHERE SubscriptionType='Basic' LIMIT 1);


#### Question 4

    SELECT users.FirstName, users.LastName, subscription.SubscriptionType
    FROM users
    LEFT JOIN subscription ON users.SubscriptionID = subscription.SubscriptionID;


#### Question 5

    SELECT DISTINCT users.FirstName, users.LastName, watchhistory.CompletionPercentage, movie.Title
    FROM users
	JOIN movie ON users.UserID = movie.MovieID
	JOIN watchhistory ON users.UserID = watchhistory.UserID
    WHERE watchhistory.CompletionPercentage = 100;

#### Question 6

    select * from movie
    order by  duration DESC
    limit 3;

#### Question 7

    SELECT avg(watchhistory.CompletionPercentage), movie.title
    FROM watchhistory
	right JOIN movie ON movie.MovieID = watchhistory.MovieID
    GROUP BY movie.title;

#### Question 8

    SELECT subscription.SubscriptionType, COUNT(users.UserID) 
    FROM users
    JOIN subscription ON users.SubscriptionID = subscription.SubscriptionID
    GROUP BY subscription.SubscriptionType;

#### Question 9

    SELECT movie.Title, AVG(review.Rating)
    FROM movie
    right JOIN review ON movie.MovieID = review.MovieID
    GROUP BY movie.title
    HAVING AVG(review.Rating) > 8;

#### Question 10

    SELECT m1.Title AS Movie1, m2.Title AS Movie2, m1.Genre, m1.Regit addleaseYear
    FROM movie m1
    JOIN movie m2 ON m1.Genre = m2.Genre 
    AND m1.ReleaseYear = m2.ReleaseYear
    AND m1.MovieID < m2.MovieID;

#### Question 11

    WITH MovieRatings AS (
    SELECT movie.MovieID, movie.Title, AVG(review.Rating) as AverageRating
    FROM movie
    JOIN review ON movie.MovieID = review.MovieID
    GROUP BY movie.MovieID, movie.Title
    )

    SELECT Title, AverageRating
    FROM MovieRatings
    ORDER BY AverageRating DESC
    LIMIT 3;

#### Question 12
