# SQL Murder Mystery Solution

## 1. Accessing the crime scene report

<p> SELECT \* FROM CRIME_SCENE_REPORT
WHERE DATE = 20180115
AND TYPE = "murder"
AND CITY = "SQL City";

Response: Security footage shows that there were 2 witnesses. The first witness lives at the last house on "Northwestern Dr".
The second witness, named Annabel, lives somewhere on "Franklin Ave". </p>

### 2. Accessing witness info

- 1st witness

SELECT \* FROM PERSON <br>
WHERE ADDRESS_STREET_NAME = "Northwestern Dr" <br>
ORDER BY ADDRESS_NUMBER DESC <br>
LIMIT 1; <br>

Response: <br>
id name license_id address_number address_street_name ssn <br>
14887 Morty Schapiro 118009 4919 Northwestern Dr 111564949 <br>

- 2nd witness

SELECT \* FROM PERSON <br>
WHERE ADDRESS_STREET_NAME = "Franklin Ave" <br>
AND NAME LIKE "%annabel%"; <br>

Response: <br>
id name license_id address_number address_street_name ssn <br>
16371 Annabel Miller 490173 103 Franklin Ave 318771143 <br>

### 3. Accessing interview transcript

SELECT \* FROM INTERVIEW <br>
WHERE PERSON_ID = 16371 OR PERSON_ID = 14887; <br>

Response: <br>

person_id transcript <br>

14887 I heard a gunshot and then saw a man run out. He had a "Get Fit Now Gym" bag. The membership number on the bag started with "48Z". Only gold members have those bags. The man got into a car with a plate that included "H42W".

16371 I saw the murder happen, and I recognized the killer from my gym when I was working out last week on January the 9th.

### 4. Accessing person table

SELECT \* FROM GET_FIT_NOW_MEMBER <br>
WHERE MEMBERSHIP_STATUS = "gold" <br>
AND ID LIKE "%48Z%";

Response: <br>
id person_id name membership_start_date membership_status <br>
48Z7A 28819 Joe Germuska 20160305 gold <br>
48Z55 67318 Jeremy Bowers 20160101 gold <br>

### 5. Accessing drivers table

SELECT \* FROM DRIVERS_LICENSE
WHERE PLATE_NUMBER LIKE "%H42W%";

Response:
id age height eye_color hair_color gender plate_number car_make car_model <br>
183779 21 65 blue blonde female H42W0X Toyota Prius <br>
423327 30 70 brown brown male 0H42W2 Chevrolet Spark LS <br>
664760 21 71 black black male 4H42WR Nissan Altima <br>

## Accessing person table where the plate matches and person has a gold membership and accessed gym on 9th of Jan

SELECT \* FROM PERSON
JOIN DRIVERS_LICENSE ON DRIVERS_LICENSE.ID = PERSON.LICENSE_ID
JOIN GET_FIT_NOW_MEMBER ON GET_FIT_NOW_MEMBER.PERSON_ID = PERSON.ID
JOIN GET_FIT_NOW_CHECK_IN ON GET_FIT_NOW_CHECK_IN.MEMBERSHIP_ID = GET_FIT_NOW_MEMBER.ID
WHERE DRIVERS_LICENSE.PLATE_NUMBER LIKE "%H42W%"
AND GET_FIT_NOW_MEMBER.MEMBERSHIP_STATUS = "gold"
AND GET_FIT_NOW_CHECK_IN.CHECK_IN_DATE = 20180109;

67318 Jeremy Bowers 423327 530 Washington Pl, Apt 3A 871539279 423327 30 70 brown brown male 0H42W2 Chevrolet Spark LS 48Z55 67318 Jeremy Bowers 20160101 gold 48Z55 20180109 1530 1700

### Accessing murderer's transcript

SELECT \* FROM INTERVIEW
WHERE PERSON_ID = 67318;

67318 I was hired by a woman with a lot of money. I don't know her name but I know she's around 5'5" (65") or 5'7" (67"). She has red hair and she drives a Tesla Model S. I know that she attended the SQL Symphony Concert 3 times in December 2017.

SELECT \* FROM PERSON
JOIN DRIVERS_LICENSE ON DRIVERS_LICENSE.ID = PERSON.LICENSE_ID
JOIN FACEBOOK_EVENT_CHECKIN ON FACEBOOK_EVENT_CHECKIN.PERSON_ID = PERSON.ID
WHERE HAIR_COLOR LIKE "%RED%"
AND CAR_MAKE LIKE "%TESLA%"
AND HEIGHT >= 65 AND HEIGHT <= 67
;
