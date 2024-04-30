# SQL Murder Mystery Solution

### 1. Accessing crime scene report

        SELECT * FROM CRIME_SCENE_REPORT
        WHERE DATE = 20180115
        AND TYPE = "murder"
        AND CITY = "SQL City";

### 2. Accessing witness info

--  first witness lives at the last house on "Northwestern Dr" <br>
--  second witness, named Annabel, lives somewhere on "Franklin Ave"

        SELECT * FROM PERSON
        WHERE ADDRESS_STREET_NAME = "Northwestern Dr"
        ORDER BY ADDRESS_NUMBER DESC
        LIMIT 1;

        SELECT * FROM PERSON
        WHERE ADDRESS_STREET_NAME = "Franklin Ave"
        AND NAME LIKE "%annabel%";


### 3. Accessing interview transcripts

        SELECT * FROM INTERVIEW
        WHERE PERSON_ID = 16371 OR PERSON_ID = 14887;


### 3. Accessing person, driver license and gym table

-- murderer attends gym, had a gold membership that starts with 48Z <br>
-- murderer's car plate: H42W <br>
-- murderer attened gym on January the 9th

        SELECT * FROM PERSON
        JOIN DRIVERS_LICENSE ON DRIVERS_LICENSE.ID = PERSON.LICENSE_ID
        JOIN GET_FIT_NOW_MEMBER ON GET_FIT_NOW_MEMBER.PERSON_ID = PERSON.ID
        JOIN GET_FIT_NOW_CHECK_IN ON GET_FIT_NOW_CHECK_IN.MEMBERSHIP_ID = GET_FIT_NOW_MEMBER.ID
        WHERE DRIVERS_LICENSE.PLATE_NUMBER LIKE "%H42W%"
        AND GET_FIT_NOW_MEMBER.MEMBERSHIP_STATUS = "gold"
        AND GET_FIT_NOW_CHECK_IN.CHECK_IN_DATE = 20180109;


### 4. Accessing murderer's transcript

        SELECT * FROM INTERVIEW
        WHERE PERSON_ID = 67318;

67318 I was hired by a woman with a lot of money. I don't know her name but I know she's around 5'5" (65") or 5'7" (67"). She has red hair and she drives a Tesla Model S. I know that she attended the SQL Symphony Concert 3 times in December 2017.

### 5. Looking for the mastermind behind the murder

-- woman, height 65-67'' with red hair <br>
-- drives Tesla Model S <br>
-- attended Symphony Concert 3 times in Dec 2017 <br>

        SELECT * FROM PERSON
        JOIN DRIVERS_LICENSE ON DRIVERS_LICENSE.ID = PERSON.LICENSE_ID
        JOIN FACEBOOK_EVENT_CHECKIN ON FACEBOOK_EVENT_CHECKIN.PERSON_ID = PERSON.ID
        WHERE HAIR_COLOR LIKE "%RED%"
        AND CAR_MAKE LIKE "%TESLA%"
        AND HEIGHT >= 65 AND HEIGHT <= 67;
