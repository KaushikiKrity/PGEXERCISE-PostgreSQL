### Format the names of members

**_Output the names of all members, formatted as 'Surname, Firstname'._**

```
SELECT Format ('%s, %s', surname, firstname) NAME
FROM   cd.members;
```

### 2. Find facilities by a name prefix

**_Find all facilities whose name begins with 'Tennis'. Retrieve all columns._**

```
   SELECT *
FROM   cd.facilities
WHERE  NAME LIKE 'Tennis%';
```

### 3. Perform a case-insensitive search

**_Perform a case-insensitive search to find all facilities whose name begins with 'tennis'. Retrieve all columns._**

```
     SELECT *
FROM   cd.facilities
WHERE  NAME ILIKE 'Tennis%';
```

### 4. Find telephone numbers with parentheses

**_You've noticed that the club's member table has telephone numbers with very inconsistent formatting. You'd like to find all the telephone numbers that contain parentheses, returning the member ID and telephone number sorted by member ID._**

      SELECT memid,
           telephone
    FROM   cd.members
    WHERE  telephone LIKE '(___)%'
    ORDER  BY memid;

### 5. Pad zip codes with leading zeroes

**_The zip codes in our example dataset have had leading zeroes removed from them by virtue of being stored as a numeric type. Retrieve all zip codes from the members table, padding any zip codes less than 5 characters long with leading zeroes. Order by the new zip code.._**

        SELECT Lpad(zipcode :: text, '5', '0') AS zip
    FROM   cd.members
    ORDER  BY zip;

### 6.Count the number of members whose surname starts with each letter of the alphabet

**_You'd like to produce a count of how many members you have whose surname starts with each letter of the alphabet. Sort by the letter, and don't worry about printing out a letter if the count is 0._**

    SELECT
      substring(surname, 1, 1) AS letter,
      count(*)
    FROM
      cd.members
    GROUP BY
      letter
    ORDER BY
      letter ;

### 7. Clean up telephone numbers

**_The telephone numbers in the database are very inconsistently formatted. You'd like to print a list of member ids and numbers that have had '-','(',')', and ' ' characters removed. Order by member id._**

    SELECT memid,
           Translate(telephone, ' -()', '') AS telephone
    FROM   cd.members
    ORDER  BY memid;
