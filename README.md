# <img src = "https://user-images.githubusercontent.com/94797745/146946052-cd0b562f-fcb3-4a7a-8eb7-4cda515a75a4.png" width = "75" height = "75"> SQL Murder Mystery

# 📕 Table Of Contents
* ### 📝 Background
* ### 📂 Dataset
* ### 🚀 Solutions

## 📝 Background
A crime has taken place and the detective needs your help. The detective gave you the crime scene report, but you somehow lost it. You vaguely remember that the crime was a murder that occurred sometime on Jan.15, 2018 and that it took place in SQL City. Start by retrieving the corresponding crime scene report from the police department’s database. 

### 📂 Dataset
#### UNDERSTANDING THE DATA
To understand the available dataset, I ran queries to find the names of the tables in the database and also understand the table structures

#### To find the names of tables:
      SELECT name
      FROM sqlite_master
      WHERE type = 'table'
#### Result:
| name                   | 
|  -------------         |
| crime_scene_report     |
| drivers_license|
| person|
| facebook_event_checkin |
| interview |
| get_fit_now_member |
| get_fit_now_check_in |
| income |
| solution |

To understand the structure of each table:
#### Crime_scene_report
	PRAGMA table_info(crime_scene_report);
cid|	name|	type|	notnull|	dflt_value	|pk|
---|--------|-------|----------|------------------------|---|
0|	date|	integer|	0|	null	0|
1|	type|	text|	0|	null|	0|
2|	description|	text|	0|	null|	0|
3|	city	|text|	0|	null|	0|

#### Drivers_license
	PRAGMA table_info (Drivers_license);

cid |	name|	type|	notnull	|dflt_value|	pk|
----|--------| -----|-----------|----------|------|
0|	id|	integer|	0|	null|	1|
1|	age|	integer	|0|	null|	0|
2|	height|	integer	|0|	null|	0|
3|	eye_color|	text|	0|	null|	0|
4|	hair_color|	text|	0|	null|	0|
5|	gender|	text|	0|	null|	0|
6|	plate_number|	text|	0|	null|	0|
7|	car_make|	text|	0|	null|	0|
8|	car_model|	text|	0|	null|	0|

#### Person
	PRAGMA table_info(person);
cid|	name|	type|	notnull|	dflt_value|	pk|
---|--------| -----|-----------|----------|------|
0|	id |	integer|	0|	null|	1|
1|	name |	text|	0|	null|	0|
2|	license_id|	integer|	0|	null|	0|
3|	address_number|	integer|	0|	null|	0|
4|	address_street_name|	text|	0|	null|	0|
5|	ssn|	integer|	0|	null|	0|

#### Facebook_event_checkin
	PRAGMA table_info(facebook_event_checkin);
cid|	name|	type|	notnull|	dflt_value|	pk|
---|--------|-------|-----------|-----------------|-------|
0|	person_id|	integer|	0|	null|	0|
1|	event_id|	integer| 0|	null|	0|
2|	event_name|	text|	0|	null|	0|
3|	date|	integer|	0|	null|	0|

#### Interview
	PRAGMA table_info(interview);
cid|	name|	type|	notnull	|dflt_value	|pk|
---|--------|-------|-----------|-----------------|-------|
0|	person_id|	integer|	0|	null|	0|
1|	transcript|	text|	0|	null|	0|

#### Get_fit_now_member
	PRAGMA table_info(Get_fit_now_member);
cid|	name|	type|	notnull	|dflt_value	|pk|
---|--------|-------|-----------|-----------------|-------|
0|	id|	text|	0|	null|	1|
1|	person_id|	integer|	0|	null|	0|
2|	name|	text|	0|	null|	0|
3|	membership_start_date|	integer|	0|	null|	0|
4|	membership_status|	text|	0|	null|	0|

#### Get_fit_now_check_in
	PRAGMA table_info(Get_fit_now_check_in);
cid|	name|	type|	notnull	|dflt_value|	pk|
---|--------|-------|-----------|-----------------|-------|
0|	membership_id|	text	|0|	null|	0|
1|	check_in_date|	integer	|0|	null|	0|
2|	check_in_time|	integer	|0|	null|	0|
3|	check_out_time|	integer	|0|	null|	0|

#### Income
	PRAGMA table_info(income);
cid|	name|	type|	notnull	| dflt_value	|pk|
---|--------|-------|-----------|-----------------|-------|
0|	ssn|	integer|	0|	null|	1|
1|	annual_income|	integer|	0|	null|	0|

#### Solution
	PRAGMA table_info(solution);
cid|	name	type	notnull	dflt_value	pk
---|--------|-------|-----------|-----------------|-------|
0|	user|	integer|	0|	null	|0|
1|	value|	text	|0|	null|	0|

## 🚀 Solutions
After understanding the structure of all the tables, I wrote SQL queries to get leads to put together a comprehensive report that would help identify the culprit.
The result of each query determined the next query.
### Query 1:
	SELECT *
	FROM crime_scene_report
	WHERE City = "SQL City"
		AND Type = "murder"
		AND DATE = "20180115";
|date|	type|	description|	city|
|-----|------|--------------|--------|
|20180115|	murder|	Security footage shows that there were 2 witnesses. The first witness lives at the last house on "Northwestern Dr". The second witness, named Annabel, lives somewhere on "Franklin Ave".|	SQL City|

## Query 2:
	SELECT *
	FROM interview
	WHERE person_id = (
			SELECT id
			FROM person
			WHERE address_number = (
					SELECT max(address_number)
					FROM person
					)
				AND address_street_name = "Northwestern Dr"
			)
		OR person_id = (
			SELECT id
			FROM person
			WHERE address_street_name = "Franklin Ave"
				AND name LIKE 'Annabel%'
			);

|person_id|	transcript|
|---------|----------------|
|14887|	I heard a gunshot and then saw a man run out. He had a "Get Fit Now Gym" bag. The membership number on the bag started with "48Z". Only gold members have those bags. The man got into a car with a plate that included "H42W".|
|16371|	I saw the murder happen, and I recognized the killer from my gym when I was working out last week on January the 9th.|

## Query 3:
	SELECT person.id AS personID
		,person.name
		,gfnm.id AS GetfitID
		,gfnci.check_in_date
		,dl.id AS DriverLicense
		,dl.age
		,dl.height
		,dl.eye_color
		,dl.hair_color
		,dl.gender
		,dl.plate_number
		,dl.car_make
		,dl.car_model
	FROM get_fit_now_member AS gfnm
	JOIN get_fit_now_check_in AS gfnci ON gfnm.id = gfnci.membership_id
	JOIN person ON person.id = gfnm.person_id
	JOIN drivers_license AS dl ON dl.id = person.license_id
	WHERE gfnm.membership_status = "gold"
		AND gfnm.id LIKE "48Z%"
		AND dl.plate_number LIKE "%H42W%"
		AND gfnci.check_in_date = "20180109";

|personID|name|GetfitID|check_in_date|DriverLicense|age|height|eye_color|hair_color|gender|plate_number|car_make|car_model|
|--------|----|---------|-------------|------------|---|-------|-------|---------|--------|------------|--------|----------|
|67318	Jeremy Bowers|	48Z55|	20180109|	423327|	30|	70|	brown|	brown|	male|	0H42W2|	Chevrolet|	Spark LS|

## Query 4:
	SELECT *
	FROM interview
	WHERE person_id = "67318";

|person_id|	transcript|
|---------|---------------|
|67318|	I was hired by a woman with a lot of money. I don't know her name but I know she's around 5'5" (65") or 5'7" (67"). She has red hair and she drives a Tesla Model S. I know that she attended the SQL Symphony Concert 3 times in December 2017.|

## Query 5:
	SELECT person.id AS personID
		,person.name
		,income.ssn
		,income.annual_income
		,dl.id AS DriverLicense
		,dl.age
		,dl.height
		,dl.eye_color
		,dl.hair_color
		,dl.gender
		,dl.plate_number
		,dl.car_make
		,dl.car_model
		,fb.event_name
	FROM person
	JOIN drivers_license AS dl ON person.license_id = dl.id
	JOIN facebook_event_checkin AS fb ON fb.person_id = person.id
	JOIN income ON income.ssn = person.ssn
	WHERE dl.hair_color = "red"
		AND dl.car_make = "Tesla"
		AND dl.car_model = "Model S"
		AND dl.height BETWEEN 65 AND 67
		AND fb.event_name = "SQL Symphony Concert"
		AND fb.DATE LIKE "2017__12"

|personID|name|ssn|annual_income|DriverLicense|age|height|eye_color|hair_color|gender|plate_number|car_make|car_model|event_name|
|--------|----|---|-------------|-------------|---|-------|--------|-----------|------|-----------|---------|--------|-----------|
|99716|	Miranda Priestly|987756388|310000|	202298|	68|	66|	green|	red|	female|	500123	|Tesla|Model S|	SQL Symphony Concert|



