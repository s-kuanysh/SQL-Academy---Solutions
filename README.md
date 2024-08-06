[SQL Academy](https://sql-academy.org/) is an online training platform to improve skills in SQL by solving problems. 

Here are the solutions only for publicly open 88 problems. 
Problems: 1-80, 93, 94, 97, 99, 101, 103, 109, 111.

MySQL syntax. 


1. Print the names of all the people who are in the airline database 
[(link)](https://sql-academy.org/ru/trainer/tasks/1)

Difficulty: Easy

Solution: 

```mysql
SELECT name
FROM Passenger;
```


2. Display the names of all airlines [(link)](https://sql-academy.org/en/trainer/tasks/2)

Difficulty: Easy

Solution:

```mysql
SELECT name
FROM Company;
```


3. Print all trips made from Moscow [(link)](https://sql-academy.org/en/trainer/tasks/3)

Difficulty: Easy

Solution:

```mysql
SELECT *
FROM Trip
WHERE town_from LIKE 'Moscow';
```


4. Print the names of people that end in "man" [(link)](https://sql-academy.org/en/trainer/tasks/4)

Difficulty: Easy

Solution:

```mysql
SELECT name
FROM Passenger
WHERE name LIKE '%man';
```


5. Print the number of trips completed on TU-134 [(link)](https://sql-academy.org/en/trainer/tasks/5)

Difficulty: Easy

Solution:

```mysql
SELECT COUNT(*) as count
FROM Trip
WHERE plane LIKE 'TU-134';
```


6. Which companies have flown on Boeing [(link)](https://sql-academy.org/en/trainer/tasks/6)

Difficulty: Easy

Solution:

```mysql
SELECT DISTINCT c.name
FROM Company c
	JOIN Trip t ON c.id = t.company
WHERE t.plane LIKE 'Boeing';
```


7. Display all the names of aircraft that you can fly to Moscow [(link)](https://sql-academy.org/en/trainer/tasks/7)

Difficulty: Easy

Solution:

```mysql
SELECT DISTINCT t.plane
FROM Trip t
WHERE town_from LIKE 'Moscow';
```


8. What cities can I fly to from Paris and how long will it take? [(link)](https://sql-academy.org/en/trainer/tasks/8)

Difficulty: Medium

Solution:

```mysql
SELECT town_to,
	TIMEDIFF(time_in, time_out) as flight_time
FROM Trip
WHERE town_from = 'Paris';
```


9. What companies organize flights from Vladivostok? [(link)](https://sql-academy.org/en/trainer/tasks/9)

Difficulty: Easy

Solution:

```mysql
SELECT DISTINCT c.name
FROM Company c
	JOIN Trip t ON c.id = t.company
WHERE t.town_from = 'Vladivostok';
```


10. Print trips made from 10 a.m. to 2 p.m. on January 1, 1900. [(link)](https://sql-academy.org/en/trainer/tasks/10)

Difficulty: Medium

Solution:

```mysql
SELECT *
FROM Trip
WHERE time_out >= '1900-01-01T10:00:00.000Z'
	AND time_out <= '1900-01-01T14:00:00.000Z';
```


11. Print the passengers with the longest full name. Spaces, hyphens, and dots are considered part of the name.
    [(link)](https://sql-academy.org/en/trainer/tasks/11)

Difficulty: Medium

Solution:

```mysql
SELECT name
FROM Passenger
WHERE LENGTH(name) = (
		SELECT MAX(LENGTH(name))
		FROM Passenger
	);
```


12. Print the id and number of passengers for all past trips [(link)](https://sql-academy.org/en/trainer/tasks/12)

Difficulty: Easy

Solution:

```mysql
SELECT trip,
	COUNT(*) as count
FROM Pass_in_trip
GROUP BY trip;
```


13. Display the names of people who have a full namesake among passengers
[(link)](https://sql-academy.org/en/trainer/tasks/13)

Difficulty: Medium

Solution:

```mysql
SELECT name
FROM Passenger
GROUP BY name
HAVING COUNT(name) > 1;
```


14. Which cities did Bruce Willis visit [(link)](https://sql-academy.org/en/trainer/tasks/14)

Difficulty: Easy

Solution:

```mysql
SELECT DISTINCT t.town_to
FROM Trip t
	JOIN Pass_in_trip pt ON pt.trip = t.id
	JOIN Passenger p ON p.id = pt.passenger
WHERE p.name LIKE 'Bruce Willis';
```


15. Print the date and time of arrival of the passenger Steve Martin to London (London)
    [(link)](https://sql-academy.org/en/trainer/tasks/15)

Difficulty: Easy

Solution:

```mysql
SELECT t.time_in
FROM Trip t
	JOIN Pass_in_trip pt ON pt.trip = t.id
	JOIN Passenger p ON p.id = pt.passenger
WHERE p.name LIKE 'Steve Martin'
	AND t.town_to LIKE 'London';

```


16. Display the list of passengers sorted by the number of flights (in descending order) and name (in ascending order) who have made at least 1 flight. [(link)](https://sql-academy.org/en/trainer/tasks/16)

Difficulty: Medium

Solution:


```mysql
SELECT p.name,
	COUNT(pt.trip) AS count
FROM Passenger p
	JOIN Pass_in_trip pt ON p.id = pt.passenger
GROUP BY p.name
HAVING COUNT(pt.trip) >= 1
ORDER BY count DESC,
	p.name ASC;
```


17. Determine how much each family member spent in 2005. In the resulting sample, do not output those family members who
have not spent anything. [(link)](https://sql-academy.org/en/trainer/tasks/17)

Difficulty: Medium

Solution:


```mysql
SELECT fm.member_name,
	fm.status,
	SUM(p.amount * p.unit_price) AS costs
FROM FamilyMembers fm
	JOIN Payments p ON p.family_member = fm.member_id
WHERE p.date LIKE '2005%'
GROUP BY fm.member_name,
	fm.status
HAVING costs > 0;
```


18. Print the name of the oldest person. If there are several of them, then output them all. 
[(link)](https://sql-academy.org/en/trainer/tasks/18)

Difficulty: Medium

Solution:

```mysql
SELECT member_name
FROM FamilyMembers
WHERE DATEDIFF(CURRENT_DATE, birthday) = (
		SELECT MAX(DATEDIFF(CURRENT_DATE, birthday))
		FROM FamilyMembers
	);
```



19. Determine which family member bought potatoes (potato) [(link)](https://sql-academy.org/en/trainer/tasks/19)

Difficulty: Easy

Solution:

```mysql
SELECT DISTINCT fm.status
FROM FamilyMembers fm
	JOIN Payments p ON fm.member_id = p.family_member
	JOIN Goods g ON g.good_id = p.good
WHERE g.good_name = 'potato';
```



20. How much and who from the family spent on entertainment. Print family status, name, amount
[(link)](https://sql-academy.org/en/trainer/tasks/20)

Difficulty: Medium

Solution:

```mysql
SELECT fm.status,
	fm.member_name,
	SUM(p.amount * p.unit_price) AS costs
FROM FamilyMembers fm
	JOIN Payments p ON p.family_member = fm.member_id
	JOIN Goods g ON g.good_id = p.good
	JOIN GoodTypes gt ON gt.good_type_id = g.type
WHERE gt.good_type_name = 'entertainment'
GROUP BY fm.status,
	fm.member_name;
```



21. Identify products that have been purchased more than 1 time [(link)](https://sql-academy.org/en/trainer/tasks/21)

Difficulty: Medium

Solution:
  
```mysql
SELECT DISTINCT g.good_name
FROM Goods g
	JOIN Payments p ON g.good_id = p.good
WHERE p.good IN (
		SELECT good
		FROM Payments
		GROUP BY good
		HAVING COUNT(*) > 1
	);
```



22. Print the names of all mothers [(link)](https://sql-academy.org/en/trainer/tasks/22)

Difficulty: Easy

Solution:
  
```mysql
SELECT DISTINCT member_name
FROM FamilyMembers
WHERE status LIKE 'mother';
```



23. Find the most expensive delicacy (delicacies) and print its price 
[(link)](https://sql-academy.org/en/trainer/tasks/23)

Difficulty: Medium

Solution:
  
```mysql
SELECT g.good_name,
	p.unit_price
FROM Goods g
	JOIN Payments p ON g.good_id = p.good
	JOIN GoodTypes gt ON gt.good_type_id = g.type
WHERE gt.good_type_name LIKE 'delicacies'
	AND p.unit_price IN (
		SELECT MAX(p.unit_price)
		FROM Payments p
			JOIN Goods g ON g.good_id = p.good
			JOIN GoodTypes gt ON gt.good_type_id = g.type
		WHERE gt.good_type_name LIKE 'delicacies'
	);
```



24. Determine who spent how much in June 2005 [(link)](https://sql-academy.org/en/trainer/tasks/24)

Difficulty: Medium

Solution:

```mysql
SELECT fm.member_name,
	SUM(p.amount * p.unit_price) AS costs
FROM FamilyMembers fm
	JOIN Payments p ON p.family_member = fm.member_id
WHERE p.date LIKE '2005-06%'
GROUP BY fm.member_name;
```



25. Determine which products were not purchased in 2005 [(link)](https://sql-academy.org/en/trainer/tasks/25)

Difficulty: Medium

Solution:

```mysql
SELECT g.good_name
FROM Goods g
	LEFT JOIN Payments p ON p.good = g.good_id
	AND YEAR(p.date) = '2005'
WHERE p.good IS NULL;
```



26. Identify groups of products that were not purchased in 2005 [(link)](https://sql-academy.org/en/trainer/tasks/26)

Difficulty: Medium

Solution:

```mysql
SELECT good_type_name
FROM GoodTypes gt
WHERE good_type_id NOT IN (
		SELECT type
		FROM Goods
			JOIN Payments ON good_id = good
		WHERE YEAR(date) = 2005
	);
```



27. Find out how much was spent on each of the product groups in 2005. Print the group name and amount
    [(link)](https://sql-academy.org/en/trainer/tasks/27)

Difficulty: Medium

Solution:

```mysql
SELECT gt.good_type_name,
	SUM(p.amount * p.unit_price) AS costs
FROM GoodTypes gt
	JOIN Goods g ON g.type = gt.good_type_id
	JOIN Payments p ON p.good = g.good_id
WHERE YEAR(p.date) = '2005'
GROUP BY gt.good_type_name
HAVING costs > 0;
```



28. How many trips were made by airlines from Rostov to Moscow ? [(link)](https://sql-academy.org/en/trainer/tasks/28)

Difficulty: Easy

Solution:
  
```mysql
SELECT COUNT(*) AS count
FROM Trip
WHERE town_from = 'Rostov'
	AND town_to = 'Moscow';
```



29. Print the names of passengers who flew to Moscow on TU-134 [(link)](https://sql-academy.org/en/trainer/tasks/29)

Difficulty: Medium

Solution:
  
```mysql
SELECT DISTINCT p.name
FROM Passenger p
	JOIN Pass_in_trip pt ON pt.passenger = p.id
	JOIN Trip t ON t.id = pt.trip
WHERE t.town_to = 'Moscow'
	AND t.plane = 'TU-134';
```



30. Output the load (number of passengers) of each trip . Output the result in sorted form in descending order of load. 
[(link)](https://sql-academy.org/en/trainer/tasks/30)

Difficulty: Medium

Solution
  
```mysql
SELECT trip,
	COUNT(passenger) AS count
FROM Pass_in_trip
GROUP BY trip
ORDER BY count DESC;
```



31. Print all family members with the last name Quincey. [(link)](https://sql-academy.org/en/trainer/tasks/31)

Difficulty: Medium

Solution:
  
```mysql
SELECT *
FROM FamilyMembers
WHERE member_name LIKE '%Quincey';
```



32. Display the average age of people (in years) stored in the database. The result is rounded to the nearest whole in 
the smaller side. [(link)](https://sql-academy.org/en/trainer/tasks/32)

Difficulty: Medium

Solution:
  

```mysql
SELECT FLOOR(AVG(DATEDIFF(CURDATE(), birthday) / 365.25)) AS age
FROM FamilyMembers;
```



33. Find the average cost of caviar. The database stores data on purchases of red caviar and black caviar.
[(link)](https://sql-academy.org/en/trainer/tasks/33)

Difficulty: Medium

Solution: 
  

```mysql
SELECT AVG(p.unit_price) AS cost
FROM Payments p
	JOIN Goods g ON g.good_id = p.good
WHERE g.good_name = 'red caviar'
	OR g.good_name = 'black caviar';
```



34. How many 10th grades are there in total [(link)](https://sql-academy.org/en/trainer/tasks/34)

Difficulty: Easy

Solution:
  

```mysql
SELECT COUNT(*) AS count
FROM Class
WHERE name LIKE '10%';
```



35. How many different classrooms of the school were used on September 2, 2019 for classes?
    [(link)](https://sql-academy.org/en/trainer/tasks/35)

Difficulty: Medium

Solution:
  

```mysql
SELECT COUNT(DISTINCT classroom) AS count
FROM Schedule
WHERE date LIKE '2019-09-02%';
```



36. Print information about students living on Pushkin street (ul. Pushkina)?
    [(link)](https://sql-academy.org/en/trainer/tasks/36)

Difficulty: Easy

Solution:
  

```mysql
SELECT *
FROM Student
WHERE address LIKE '%ul. Pushkina%';
```



37. How old is the youngest student ? [(link)](https://sql-academy.org/en/trainer/tasks/37)

Difficulty: Medium

Solution:
  

```mysql
SELECT FLOOR(MIN(DATEDIFF(CURDATE(), birthday) / 365.25)) AS year
FROM Student;
```



38. How many Ann (Anna) goes to school ? [(link)](https://sql-academy.org/en/trainer/tasks/38)

Difficulty: Easy

Solution: 
  

```mysql
SELECT COUNT(*) AS count
FROM Student
WHERE first_name = 'Anna';
```



39. How many students are in grade 10 B ? [(link)](https://sql-academy.org/en/trainer/tasks/39)

Difficulty: Easy

Solution:
  

```mysql
SELECT COUNT(sc.student) AS count
FROM Student_in_class sc
	JOIN Class c ON c.id = sc.class
WHERE c.name = '10 B';
```



40. Print the name of the subjects that Romashkin P. P. teaches ? [(link)](https://sql-academy.org/en/trainer/tasks/40)

Difficulty: Medium

Solution:
  

```mysql
SELECT s.name AS subjects
FROM Subject s
	JOIN Schedule sh ON sh.subject = s.id
	JOIN Teacher t ON t.id = sh.teacher
WHERE t.first_name LIKE 'P%'
	AND t.middle_name LIKE 'P%'
	AND t.last_name LIKE 'Romashkin';
```



41. Find out what time the fourth lesson starts according to the schedule ? [(link)](https://sql-academy.org/en/trainer/tasks/41)

Difficulty: Easy

Solution:
  

```mysql
SELECT start_pair
FROM Timepair
WHERE id = 4;
```



42. How long will the student be in school, studying from the 2nd to the 4th academic subject?
    [(link)](https://sql-academy.org/en/trainer/tasks/42)

Difficulty: Medium

Solution:
  

```mysql
SELECT TIMEDIFF(
		(
			SELECT end_pair
			FROM Timepair
			WHERE id = 4
		),
		(
			SELECT start_pair
			FROM Timepair
			WHERE id = 2
		)
	) AS time;
```



43. Print the last names of the teachers who teach physical education (Physical Culture). Sort the teachers by last name
in alphabetical order. [(link)](https://sql-academy.org/en/trainer/tasks/43)

Difficulty: Medium

Solution:
  

```mysql
SELECT t.last_name
FROM Teacher t
	JOIN Schedule sh ON sh.teacher = t.id
	JOIN Subject s ON s.id = sh.subject
WHERE s.name = 'Physical Culture'
ORDER BY t.last_name;
```



44. Find the maximum age (number years) among students of 10 classes ?
[(link)](https://sql-academy.org/en/trainer/tasks/44)

Difficulty: Hard

Solution:
  

```mysql
SELECT MAX(TIMESTAMPDIFF(YEAR, st.birthday, NOW())) AS max_year
FROM Student st
	JOIN Student_in_class sc ON sc.student = st.id
	JOIN Class c ON c.id = sc.class
WHERE c.name LIKE '10%';
```



45. Which classrooms were most often used for classes? Output the ones that have been used the maximum number of times. 
[(link)](https://sql-academy.org/en/trainer/tasks/45)

Difficulty: Hard

Solution:
  

```mysql
SELECT classroom
FROM Schedule s
GROUP BY classroom
HAVING COUNT(s.classroom) = (
		SELECT COUNT(classroom) AS lala
		FROM Schedule
		GROUP BY classroom
		ORDER BY lala DESC
		LIMIT 1
	);
```



46. In which classes will the Krauze teacher introduce classes ? [(link)](https://sql-academy.org/en/trainer/tasks/46)

Difficulty: Medium

Solution:
  

```mysql
SELECT DISTINCT c.name
FROM Class c
	JOIN Schedule sh ON sh.class = c.id
	JOIN Teacher t ON t.id = sh.teacher
WHERE t.last_name = 'Krauze';
```



47. How many classes did Krauze hold on August 30, 2019? [(link)](https://sql-academy.org/en/trainer/tasks/47)

Difficulty: Medium

Solution:
  

```mysql
SELECT COUNT(sh.subject) AS count
FROM Schedule sh
	JOIN Teacher t ON t.id = sh.teacher
WHERE sh.date LIKE '2019-08-30%'
	AND t.last_name LIKE 'Krauze';
```



48. Print the number of students in classes in descending order [(link)](https://sql-academy.org/en/trainer/tasks/48)

Difficulty: Medium

Solution:
  

```mysql
SELECT c.name,
	COUNT(sc.student) AS count
FROM Class c
	JOIN Student_in_class sc ON sc.class = c.id
GROUP BY c.name
ORDER BY count DESC;
```



49. What percentage of students are styding in the "10 A" class? Print the answer in the range from 0 to 100 without 
rounding, for example, 96.0201. [(link)](https://sql-academy.org/en/trainer/tasks/49)

Difficulty: Medium

Solution:
  

```mysql
SELECT ROUND(
		(
			(
				SELECT COUNT(*)
				FROM Student_in_class sc
					JOIN Class c ON sc.class = c.id
				WHERE c.name = '10 A'
			) / (
				SELECT COUNT(*)
				FROM Student_in_class
			)
		) * 100,
		4
	) AS percent;
```



50. What percentage of students were born in 2000? The result of rounding to the nearest whole in the smaller side.
    [(link)](https://sql-academy.org/en/trainer/tasks/50)

Difficulty: Medium

Solution:
  

```mysql
SELECT (
		FLOOR(
			(
				(
					SELECT COUNT(*)
					FROM Student
					WHERE YEAR(birthday) = '2000'
				) / (
					SELECT COUNT(*)
					FROM Student
				)
			) * 100
		)
	) AS percent;
```



51. Add a product with the name "Cheese" and the type "food" to the list of goods (Goods).
    [(link)](https://sql-academy.org/en/trainer/tasks/51)

Difficulty: Medium

Solution:
  

```mysql
INSERT INTO Goods (good_id, good_name, type)
SELECT COUNT(good_id) + 1,
	'Cheese',
	2
FROM Goods;
```



52. Add to the list of product types (GoodTypes) a new type of "auto". 
[(link)](https://sql-academy.org/en/trainer/tasks/52)

Difficulty: Medium

Solution:
  

```mysql
INSERT INTO GoodTypes(good_type_id, good_type_name)
SELECT COUNT(good_type_id) + 1,
	'auto'
FROM GoodTypes;
```



53. Change the name "Andie Quincey" to the new "Andie Anthony". [(link)](https://sql-academy.org/en/trainer/tasks/53)

Difficulty: Easy

Solution:
  

```mysql
UPDATE FamilyMembers
SET member_name = 'Andie Anthony'
WHERE member_name = 'Andie Quincey';
```



54. Remove all family members whose last name is "Quincey". [(link)](https://sql-academy.org/en/trainer/tasks/54)

Difficulty: Medium

Solution: 
  

```mysql
DELETE FROM FamilyMembers
WHERE member_name LIKE '%Quincey';
```



55. Delete the companies that made the least number of flights. [(link)](https://sql-academy.org/en/trainer/tasks/55)

Difficulty: Hard

Solution:
  

```mysql
DELETE FROM Company
WHERE id IN (
		SELECT company
		FROM Trip
		GROUP BY company
		HAVING COUNT(id) = (
				SELECT COUNT(id) as cnt
				FROM Trip
				GROUP BY company
				ORDER BY cnt
				LIMIT 1
			)
	);
```



56. Delete all flights made from Moscow (Moscow). [(link)](https://sql-academy.org/en/trainer/tasks/56)

Difficulty: Easy

Solution:
  

```mysql
DELETE FROM Trip
WHERE town_from = 'Moscow';
```



57. Reschedule the schedule of all classes for 30 minutes. forward [(link)](https://sql-academy.org/en/trainer/tasks/57)

Difficulty: Medium

Solution:
  

```mysql
UPDATE Timepair
SET start_pair = TIMESTAMPADD(MINUTE, 30, start_pair),
	end_pair = TIMESTAMPADD(MINUTE, 30, end_pair);
```



58. Add a 5-star comment on housing located at 11218 Friel Place, New York on behalf of George Clooney 
[(link)](https://sql-academy.org/en/trainer/tasks/58)

Difficulty: Hard

Solution:
  

```mysql
INSERT INTO Reviews
SET rating = 5,
	id = (
		SELECT COUNT(id) + 1
		FROM Reviews AS a
	),
	reservation_id = (
		SELECT r.id
		FROM Reservations r
			JOIN Users u ON r.user_id = u.id
			JOIN Rooms rm ON r.room_id = rm.id
		WHERE address = '11218, Friel Place, New York'
			AND name = 'George Clooney'
	);
```



59. Print users who have a Belarusian phone number? The telephone code of Belarus is +375.
    [(link)](https://sql-academy.org/en/trainer/tasks/59)

Difficulty: Medium 

Solution:
  

```mysql
SELECT *
FROM Users
WHERE phone_number LIKE '+375%';
```



60. Print the identifiers of the teachers who taught at least once for the entire time in each of the eleventh grades. 
[(link)](https://sql-academy.org/en/trainer/tasks/60)

Difficulty: Hard

Solution:
  

```mysql
SELECT teacher
FROM Schedule
	JOIN Class ON Class.id = Schedule.class
WHERE name LIKE '11%'
GROUP BY teacher
HAVING COUNT(DISTINCT name) = (
		SELECT COUNT(name)
		FROM Class
		WHERE name LIKE '11%'
	);
```



61. Output a list of rooms that have been reserved for at least one day in the 12th week of 2020. In this task, take a 
period of seven days as one week, the first of which begins on January 1, 2020. For example, the first week of the year 
is January 1-7, and the third is January 15-21. [(link)](https://sql-academy.org/en/trainer/tasks/61)

Difficulty: Medium

Solution:
  

```mysql
SELECT Rooms.*
FROM Rooms
	JOIN Reservations ON Rooms.id = Reservations.room_id
WHERE (
		WEEK(start_date, 1) = 12
		AND YEAR(start_date) = 2020
	)
	OR (
		WEEK(end_date, 1) = 12
		AND YEAR(end_date) = 2020
	);
```



62. List the 2nd-level domain names used by users for email in descending order of popularity. The resulting result must
be additionally sorted in ascending order of domain names. [(link)](https://sql-academy.org/en/trainer/tasks/62)

Difficulty: Medium

Solution:
  

```mysql
SELECT SUBSTRING_INDEX(email, '@', -1) as domain,
	COUNT(SUBSTRING_INDEX(email, '@', -1)) as count
FROM Users
GROUP BY domain
ORDER BY count DESC,
	domain;
```



63. Print the sorted list (in ascending order) of students' lastnames and firstnames in the form Surname.F.
    [(link)](https://sql-academy.org/en/trainer/tasks/63)

Difficulty: Medium

Solution: 
  

```mysql
SELECT CONCAT(last_name, '.', LEFT(first_name, 1), '.') AS name
FROM Student
ORDER BY name;
```



64. Display the number of reservations for each month of each year that had at least 1 reservation. Sort the result in 
ascending order of reservation date. [(link)](https://sql-academy.org/en/trainer/tasks/64)

Difficulty: Medium

Solution:
  

```mysql
SELECT YEAR(start_date) AS year,
	MONTH(start_date) AS month,
	COUNT(id) AS amount
FROM Reservations
GROUP BY year,
	month
ORDER BY year,
	month;
```



65. It is necessary to display the rating for rooms that have been rented at least once, as the average of the rating of
reviews, rounded down to the integer. [(link)](https://sql-academy.org/en/trainer/tasks/65)

Difficulty: Medium

Solution:
  

```mysql
SELECT room_id,
	FLOOR(AVG(rating)) AS rating
FROM Reviews
	JOIN Reservations ON Reviews.reservation_id = Reservations.id
GROUP BY room_id;
```



66. Display a list of rooms with all amenities (TV, Internet, kitchen, and air conditioning), as well as the total 
number of days and the amount for all days of renting each of these rooms. 
[(link)](https://sql-academy.org/en/trainer/tasks/66)

Difficulty: Medium

Solution:
  

```mysql
SELECT home_type,
	address,
	COALESCE(
		SUM(TIMESTAMPDIFF(DAY, start_date, end_date)),
		0
	) AS days,
	COALESCE(SUM(total), 0) AS total_fee
FROM Rooms r
	LEFT JOIN Reservations rv ON r.id = rv.room_id
WHERE (has_tv, has_internet, has_kitchen, has_air_con) = (1, 1, 1, 1)
GROUP BY home_type,
	address;
```



67. Output the departure time and arrival time for each flight in "HH:MM, DD.MM - HH:MM, DD.MM" format, where the hours 
and minutes are with a leading zero, and the day and month are without. 
[(link)](https://sql-academy.org/en/trainer/tasks/67)

Difficulty: Medium

Solution:
  

```mysql
SELECT CONCAT(
		DATE_FORMAT(time_out, '%H:%i, %e.%c'),
		' - ',
		DATE_FORMAT(time_in, '%H:%i, %e.%c')
	) AS flight_time
FROM Trip;
```



68. For each room selected at least 1 time, find the name of the person who last rented it and used it when they checked
out [(link)](https://sql-academy.org/en/trainer/tasks/68)

Difficulty: Hard

Solution:
  

```mysql
with get_data as (
	select room_id,
		max(end_date) as end_date
	from Reservations
	group by room_id
)
select rs.room_id,
	u.name,
	rs.end_date
from Reservations as rs
	join get_data as gd on gd.room_id = rs.room_id
	and gd.end_date = rs.end_date
	join users as u on u.id = rs.user_id;
```



69. Display the identifiers of all the owners of the rooms that are placed on our service for choosing accommodation and
the value they have earned. [(link)](https://sql-academy.org/en/trainer/tasks/69)

Difficulty: Hard

Solution:
  

```mysql
SELECT owner_id,
	IFNULL(SUM(total), 0) AS total_earn
FROM Rooms
	LEFT JOIN Reservations ON Reservations.room_id = Rooms.id
GROUP BY owner_id;
```



70. It is necessary to categorize rooms into economy, comfort, premium at a price <= 100, 100 < price < 200, >= 200, 
respectively. As a result, display a table with the name of the category and the number of housing falling into this 
category [(link)](https://sql-academy.org/en/trainer/tasks/70)

Difficulty: Medium 

Solution:
  

```mysql
WITH temt_t AS (
	SELECT id,
		CASE
			WHEN price <= 100 THEN 'economy'
			WHEN price > 100
			AND price < 200 THEN 'comfort'
			WHEN price >= 200 THEN 'premium'
			ELSE '4to-to neponyatnoe'
		END AS category
	FROM Rooms
)
SELECT category,
	COUNT(id) AS count
FROM temt_t
GROUP BY category;
```



71. Find what percentage of users registered on the booking service have rented or rented out a property at least once. 
Round the result to the nearest hundredth. [(link)](https://sql-academy.org/en/trainer/tasks/71)

Difficulty: Hard

Solution:
  

```mysql
SELECT ROUND(
		(
			SELECT COUNT(*)
			FROM (
					SELECT DISTINCT owner_id
					FROM Rooms rm
						JOIN Reservations rs ON rm.id = rs.room_id
					UNION
					SELECT user_id
					FROM Reservations
				) active_users
		) * 100 / (
			SELECT COUNT(*)
			FROM Users
		),
		2
	) AS percent;
```



72. ВPrint the average booking price per day for each of the rooms that have been booked at least once. The average cost
must be rounded up to an integer value. [(link)](https://sql-academy.org/en/trainer/tasks/72)

Difficulty: Medium

Solution:
  

```mysql
SELECT rs.room_id,
	CEIL(AVG(rs.price)) AS avg_price
FROM Reservations rs
GROUP BY rs.room_id;
```



73. Print the id of those rooms that have been rented an odd number of times
    [(link)](https://sql-academy.org/en/trainer/tasks/73)

Difficulty: Medium

Solution:
  

```mysql
SELECT room_id,
	COUNT(*) AS count
FROM Reservations
GROUP BY room_id
HAVING count % 2 != 0;
```



74. Display housing identifiers and the presence of the Internet in the housing. If there is Internet in the rented 
housing, then print "YES", otherwise "NO". [(link)](https://sql-academy.org/en/trainer/tasks/74)

Difficulty: Easy

Solution:
  

```mysql
SELECT id,
	IF(has_internet, 'YES', 'NO') AS has_internet
FROM Rooms;
```



75. Display the last name, first name and date of birth of students who were born in May.
    [(link)](https://sql-academy.org/en/trainer/tasks/75)

Difficulty: Easy

Solution:
  

```mysql
SELECT last_name,
	first_name,
	birthday
FROM Student
WHERE birthday LIKE '%-05-%';
```



76. Display the names of all users of the booking service, as well as the status of whether the user is the owner of the
housing and whether he has rented the housing at least once. If the user has the specified status, display 1 in the 
appropriate field, otherwise 0. [(link)](https://sql-academy.org/en/trainer/tasks/76)

Difficulty: Medium

Solution:
  

```mysql
SELECT us.name,
	MAX(
		CASE
			WHEN rm.owner_id IS NOT NULL THEN 1
			ELSE 0
		END
	) AS is_owner,
	MAX(
		CASE
			WHEN rs.user_id IS NOT NULL THEN 1
			ELSE 0
		END
	) AS is_tenant
FROM Users us
	LEFT JOIN Rooms rm ON rm.owner_id = us.id
	LEFT JOIN Reservations rs ON rs.user_id = us.id
GROUP BY us.id,
	us.name;
```



77. Create a view called "People" that will contain a list of first names and last names of all students and teachers 
[(link)](https://sql-academy.org/en/trainer/tasks/77)

Difficulty: Medium

Solution:
  

```mysql
CREATE VIEW People AS
SELECT first_name,
	last_name
FROM Student
UNION
SELECT first_name,
	last_name
FROM Teacher;
```



78. Display all users with email in "hotmail.com" [(link)](https://sql-academy.org/en/trainer/tasks/78)

Difficulty: Medium

Solution:
  

```mysql
SELECT *
FROM Users
WHERE email LIKE '%@hotmail.com';
```



79. Print the id, home_type, price fields for all rooms. If the room has a TV and Internet at the same time, then 
display the price in the price field, applying a 10% discount [(link)](https://sql-academy.org/en/trainer/tasks/79)

Difficulty: Medium

Solution:
  

```mysql
SELECT id,
	home_type,
	(price * 0.9) AS price
FROM Rooms
WHERE has_tv = 1
	AND has_internet = 1;
SELECT id,
	home_type,
	CASE
		WHEN has_tv = 1
		AND has_internet = 1 THEN price * 0.9
		ELSE price
	END AS price
FROM Rooms;
```



80. Create a "Verified_Users" view with id, name and email fields that will only show users who have a verified email 
address. [(link)](https://sql-academy.org/en/trainer/tasks/80)

Difficulty: Medium

Solution:
  

```mysql
CREATE VIEW Verified_Users AS
SELECT id,
	name,
	email
FROM Users
WHERE email_verified_at IS NOT NULL;

```


93. What is the average age of customers who bought a Smartwatch (use product.name) in 2024? [(link)](https://sql-academy.org/en/trainer/tasks/93)

Difficulty: Medium

Solution:
  

```mysql
SELECT AVG(age) AS average_age
FROM (
		SELECT DISTINCT c.age
		FROM Customer c
			JOIN Purchase pr ON c.customer_key = pr.customer_key
			JOIN Product pd ON pd.product_key = pr.product_key
		WHERE pr.date LIKE '2024%'
			AND pd.name = 'Smartwatch'
	) AS distinct_ages;
```

94. Display the names of customers who each purchased a "Laptop" and "Monitor" (use product.name) in March 2024? [(link)](https://sql-academy.org/en/trainer/tasks/94)

Difficulty: Medium

Solution:
  

```mysql
SELECT c.name
FROM Customer c
	JOIN Purchase pr ON c.customer_key = pr.customer_key
	JOIN Product pd ON pd.product_key = pr.product_key
WHERE pr.date LIKE '2024-03%'
	AND pd.name IN ('Laptop', 'Monitor')
GROUP BY c.name
HAVING COUNT(DISTINCT pd.name) = 2;
```

97. Calculate the number of operating warehouses as of the current date for each city. Display only those cities that have more than 80 warehouses. The output data is the city, the number of warehouses. [(link)](https://sql-academy.org/en/trainer/tasks/97)

Difficulty: Medium

Solution:
  

```mysql
CREATE VIEW Verified_Users AS
SELECT id,
	name,
	email
FROM Users
WHERE email_verified_at IS NOT NULL;

```

99. Calculate the income from the female audience (income = sum price * items). Please note that in the table the female audience has the user_gender field “female” or “f”. [(link)](https://sql-academy.org/en/trainer/tasks/99)

Difficulty: Easy

Solution:
  

```mysql
SELECT SUM(price * items) AS income_from_female
FROM Purchases
WHERE user_gender IN ('female', 'f');
```

101. Display for each user the first item that he ordered (the first in transaction time). [(link)](https://sql-academy.org/en/trainer/tasks/101)

Difficulty: Medium

Solution:
  

```mysql
SELECT t.user_id,
	t.item
FROM Transactions t
	JOIN (
		SELECT user_id,
			MIN(transaction_ts) AS first_transaction
		FROM Transactions
		GROUP BY user_id
	) ft ON t.user_id = ft.user_id
	AND t.transaction_ts = ft.first_transaction;
```

103. Display a list of names of employees receiving a salary greater than that of their immediate chief. [(link)](https://sql-academy.org/en/trainer/tasks/103)

Difficulty: Easy

Solution:
  

```mysql
SELECT e1.name
FROM Employee e1
	JOIN Employee e2 ON e1.chief_id = e2.id
WHERE e1.salary > e2.salary;
```

109. Find out the name of the country that is located in the city of Salzburg. [(link)](https://sql-academy.org/en/trainer/tasks/109)

Difficulty: Easy

Solution:
  

```mysql
SELECT c.name AS country_name
FROM Countries c
	JOIN Regions r ON r.countryid = c.id
	JOIN Cities ct ON ct.regionid = r.id
WHERE ct.name LIKE 'Salzburg';
```


111. Count the population of each region. As a result, print the name of the region and its population. [(link)](https://sql-academy.org/en/trainer/tasks/111)

Difficulty: Medium

Solution:
  

```mysql
SELECT r.name AS region_name,
	SUM(ct.population) AS total_population
FROM Regions r
	JOIN Cities ct ON ct.regionid = r.id
GROUP BY region_name;
```
