# SQL-Data-cleaning

### Introduction
This is a learning project about cleaning the data from a CSV file on Github. Hope you find this helpful hehe!

#### Progess
##### Step 1: Taking a look
First of all, let's have a look at the CSV file I've received (its name is club_member_info). In Dbeaver clients, I would take the first 10 rows to see how bad the data is (hehe):
```sql
SELECT * FROM club_member_info LIMIT 10;
```
|full_name|age|martial_status|email|phone|full_address|job_title|membership_date|
|---------|---|--------------|-----|-----|------------|---------|---------------|
|addie lush|40|married|alush0@shutterfly.com|254-389-8708|3226 Eastlawn Pass,Temple,Texas|Assistant Professor|7/31/2013|
|      ROCK CRADICK|46|married|rcradick1@newsvine.com|910-566-2007|4 Harbort Avenue,Fayetteville,North Carolina|Programmer III|5/27/2018|
|Sydel Sharvell|46|divorced|ssharvell2@amazon.co.jp|702-187-8715|4 School Place,Las Vegas,Nevada|Budget/Accounting Analyst I|10/6/2017|
|Constantin de la cruz|35||co3@bloglines.com|402-688-7162|6 Monument Crossing,Omaha,Nebraska|Desktop Support Technician|10/20/2015|
|  Gaylor Redhole|38|married|gredhole4@japanpost.jp|917-394-6001|88 Cherokee Pass,New York City,New York|Legal Assistant|5/29/2019|
|Wanda del mar       |44|single|wkunzel5@slideshare.net|937-467-6942|10864 Buhler Plaza,Hamilton,Ohio|Human Resources Assistant IV|3/24/2015|
|Joann Kenealy|41|married|jkenealy6@bloomberg.com|513-726-9885|733 Hagan Parkway,Cincinnati,Ohio|Accountant IV|4/17/2013|
|   Joete Cudiff|51|divorced|jcudiff7@ycombinator.com|616-617-0965|975 Dwight Plaza,Grand Rapids,Michigan|Research Nurse|11/16/2014|
|mendie alexandrescu|46|single|malexandrescu8@state.gov|504-918-4753|34 Delladonna Terrace,New Orleans,Louisiana|Systems Administrator III|3/12/1921|
| fey kloss|52|married|fkloss9@godaddy.com|808-177-0318|8976 Jackson Park,Honolulu,Hawaii|Chemical Engineer|11/5/2014|

##### Step 2: Creating a new table with the name club_member_info_cleaned for cleaning purpose while keeping the original table
I coppied the DDL of the former table the renamed the naming part of the new table, then I would have a new table with the same columns type. The code will be look somewhere like this:
```sql
CREATE TABLE club_member_info_cleaned (
	full_name VARCHAR(50),
	age INTEGER,
	martial_status VARCHAR(50),
	email VARCHAR(50),
	phone VARCHAR(50),
	full_address VARCHAR(50),
	job_title VARCHAR(50),
	membership_date VARCHAR(50)
);
```
Then I can insert all the data from the former table to the new table using this syntax:
```sql
INSERT INTO club_member_info_cleaned
SELECT * FROM club_member_info;
```
##### Step 3: Cleaningggggg!!!!
The data from the CSV file that I received and imported to DBeaver client seems to be very messy. So, firstly I started to trim and capitalize all the name in the full_name column, kick all the abnormal people with extreme high in age (I take the range of age from 1 to 150 instead of 100 as there are some records showing that some people can live up to 140 years old) and then I take the the first 10 rows with no missing data (just simply I hate the missing data so bad hehe) for examples as below:
|full_name|age|martial_status|email|phone|full_address|job_title|membership_date|
|---------|---|--------------|-----|-----|------------|---------|---------------|
|ADDIE LUSH|40|married|alush0@shutterfly.com|254-389-8708|3226 Eastlawn Pass,Temple,Texas|Assistant Professor|7/31/2013|
|ROCK CRADICK|46|married|rcradick1@newsvine.com|910-566-2007|4 Harbort Avenue,Fayetteville,North Carolina|Programmer III|5/27/2018|
|SYDEL SHARVELL|46|divorced|ssharvell2@amazon.co.jp|702-187-8715|4 School Place,Las Vegas,Nevada|Budget/Accounting Analyst I|10/6/2017|
|GAYLOR REDHOLE|38|married|gredhole4@japanpost.jp|917-394-6001|88 Cherokee Pass,New York City,New York|Legal Assistant|5/29/2019|
|WANDA DEL MAR|44|single|wkunzel5@slideshare.net|937-467-6942|10864 Buhler Plaza,Hamilton,Ohio|Human Resources Assistant IV|3/24/2015|
|JOANN KENEALY|41|married|jkenealy6@bloomberg.com|513-726-9885|733 Hagan Parkway,Cincinnati,Ohio|Accountant IV|4/17/2013|
|JOETE CUDIFF|51|divorced|jcudiff7@ycombinator.com|616-617-0965|975 Dwight Plaza,Grand Rapids,Michigan|Research Nurse|11/16/2014|
|MENDIE ALEXANDRESCU|46|single|malexandrescu8@state.gov|504-918-4753|34 Delladonna Terrace,New Orleans,Louisiana|Systems Administrator III|3/12/1921|
|FEY KLOSS|52|married|fkloss9@godaddy.com|808-177-0318|8976 Jackson Park,Honolulu,Hawaii|Chemical Engineer|11/5/2014|
|DARWIN VENTAM|42|married|dventama@uol.com.br|203-993-0118|2254 Express Hill,New Haven,Connecticut|Chemical Engineer|3/12/2017|

##### Syntax for the above table
Trimming the column full_name:
```sql
UPDATE club_member_info_cleaned
SET full_name = TRIM(full_name);
```
Capitalize all the names in the column:
```sql
UPDATE club_member_info_cleaned
SET full_name = UPPER(full_name);
```
Select the 10 sample rows:
```sql
SELECT * FROM club_member_info_cleaned
WHERE full_name <> ""
AND age <> "" AND age BETWEEN 1 AND 150
AND maritial_status <> ""
AND email <> ""
AND phone <> ""
AND full_address <> ""
AND job_title <> ""
AND membership_date <> ""
LIMIT 10;
```
Well, at this step, I may already had a table with the wanted data. However, I didn't mean to delete the all the unused rows, and if you are insisted on deleting them, here is the syntax:
To trim all the column (except full_name as we already did it before) to make sure the will be no spaces in the selected columns:
```sql
UPDATE club_member_info_cleaned
SET 
    age = TRIM(age),
    maritial_status = TRIM(maritial_status),
    email = TRIM(email),
    phone = TRIM(phone),
    full_address = TRIM(full_address),
    job_title = TRIM(job_title),
    membership_date = TRIM(membership_date);
```
To delete all the missing data:
```sql
DELETE FROM club_member_info_cleaned
WHERE full_name = ""
OR age = "" OR age > 150
OR maritial_status = ""
OR email = ""
OR phone = ""
OR full_address = ""
OR job_title = ""
OR membership_date = "";
```
Plan B: To replace all the missing data with 'null' instead, here is the syntax: 
```sql
UPDATE club_member_info_cleaned
SET 
    age = 'null' WHERE age = '',
    maritial_status = 'null' WHERE maritial_status = '',
    email = 'null' WHERE email = '',
    phone = 'null' WHERE phone = '',
    full_address = 'null' WHERE full_address = '',
    job_title = 'null' WHERE job_title = '',
    membership_date = 'null' WHERE membership_date = '';
```



