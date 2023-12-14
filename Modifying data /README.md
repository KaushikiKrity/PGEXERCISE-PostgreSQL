### 1. Insert some data into a table

**_The club is adding a new facility - a spa. We need to add it into the facilities table. Use the following values:_**

- **_facid: 9, Name: 'Spa', membercost: 20, guestcost: 30, initialoutlay: 100000, monthlymaintenance: 800._**

```
INSERT INTO cd.facilities
           (facid,
            NAME,
            membercost,
            guestcost,
            initialoutlay,
            monthlymaintenance)
VALUES      (9,
            'Spa',
            20,
            30,
            100000,
            800)
```

### 2. Insert multiple rows of data into a table

**_In the previous exercise, you learned how to add a facility. Now you're going to add multiple facilities in one command. Use the following values:_**

- **_facid: 9, Name: 'Spa', membercost: 20, guestcost: 30, initialoutlay: 100000, monthlymaintenance: 800._**
- **_facid: 10, Name: 'Squash Court 2', membercost: 3.5, guestcost: 17.5, initialoutlay: 5000, monthlymaintenance: 80._**

```
    INSERT INTO cd.facilities
    VALUES      (9,
                 'Spa',
                 20,
                 30,
                 100000,
                 800),
                (10,
                 'Squash Court 2',
                 3.5,
                 17.5,
                 5000,
                 80)
```

### 3. Insert calculated data into a table

**_Let's try adding the spa to the facilities table again. This time, though, we want to automatically generate the value for the next facid, rather than specifying it as a constant. Use the following values for everything else:_**

- **_Name: 'Spa', membercost: 20, guestcost: 30, initialoutlay: 100000, monthlymaintenance: 800._**

```
     INSERT INTO cd.facilities
SELECT(SELECT Max(facid)
       FROM   cd.facilities)
      + 1,
      'Spa',
      20,
      30,
      100000,
      800
```

### 4. Update some existing data

**_We made a mistake when entering the data for the second tennis court. The initial outlay was 10000 rather than 8000: you need to alter the data to fix the error._**

         UPDATE cd.facilities
    SET    initialoutlay = 10000
    WHERE  NAME = 'Tennis Court 2';

### 5. Update multiple rows and columns at the same time

**_We want to increase the price of the tennis courts for both members and guests. Update the costs to be 6 for members, and 30 for guests._**

       UPDATE cd.facilities
    SET    membercost = 6,
           guestcost = 30
    WHERE  facid IN ( 0, 1 );

### 6.Update a row based on the contents of another row

**_We want to alter the price of the second tennis court so that it costs 10% more than the first one. Try to do this without using constant values for the prices, so that we can reuse the statement if we want to.._**

    UPDATE cd.facilities
    SET    membercost = (SELECT membercost
                         FROM   cd.facilities
                         WHERE  facid = 0) * 1.1,
           guestcost = (SELECT guestcost
                        FROM   cd.facilities
                        WHERE  facid = 0) * 1.1
    WHERE  facid = 1;

### 7. Delete all bookings

**_As part of a clearout of our database, we want to delete all bookings from the cd.bookings table. How can we accomplish this?_**

    delete from cd.bookings;

### 8. Delete a member from the cd.members table

**_We want to remove member 37, who has never made a booking, from our database. How can we achieve that?_**

    delete from cd.members where memid=37

### 9. Delete based on a subquery

**_In our previous exercises, we deleted a specific member who had never made a booking. How can we make that more general, to delete all members who have never made a booking?_**

    DELETE FROM cd.members
    WHERE  memid NOT IN (SELECT memid
                         FROM   cd.bookings);
