# <img src = "https://user-images.githubusercontent.com/94797745/146946052-cd0b562f-fcb3-4a7a-8eb7-4cda515a75a4.png" width = "75" height = "75"> SQL-Murder-Mystery
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


