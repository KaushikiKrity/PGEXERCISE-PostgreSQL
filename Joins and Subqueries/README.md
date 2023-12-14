### 1. Retrieve the start times of members' bookings

**_How can you produce a list of the start times for bookings by members named 'David Farrell'?_**

       SELECT b.starttime
    FROM   cd.bookings b
           INNER JOIN cd.members m
                   ON m.memid = b.memid
    WHERE  m.firstname = 'David'
           AND m.surname = 'Farrell';

### 2. Work out the start times of bookings for tennis courts

**_How can you produce a list of the start times for bookings for tennis courts, for the date '2012-09-21'? Return a list of start time and facility name pairings, ordered by the time._**

      SELECT
      b.starttime as start, f.name
    FROM
      cd.bookings AS b
    INNER JOIN
      cd.facilities AS f
    ON
      b.facid = f.facid
    WHERE
      f.name LIKE 'Tennis Court%'
      AND date(b.starttime) = '2012-09-21'
    ORDER BY
      b.starttime;

### 3. Produce a list of all members who have recommended another member

**_How can you output a list of all members who have recommended another member? Ensure that there are no duplicates in the list, and that results are ordered by (surname, firstname)._**

       select
      distinct m.firstname,
      m.surname
    from
      cd.members m
      inner join cd.members r on m.memid = r.recommendedby
    order by
      m.surname,
      m.firstname;

### 4. Produce a list of all members, along with their recommender

**_How can you output a list of all members, including the individual who recommended them (if any)? Ensure that results are ordered by (surname, firstname)._**

        SELECT
      m.firstname AS memfname, m.surname AS memsname,
      r.firstname AS recfname, r.surname AS recsname
    FROM
      cd.members AS m
    LEFT JOIN
      cd.members AS r
    ON
      m.recommendedby = r.memid
    ORDER BY
      memsname, memfname;

### 5. Produce a list of all members who have used a tennis court

**_How can you produce a list of all members who have used a tennis court? Include in your output the name of the court, and the name of the member formatted as a single column. Ensure no duplicate data, and order by the member name followed by the facility name._**

    SELECT DISTINCT CONCAT (m.firstname , ' ', m.surname) AS "member",
                    f.NAME       AS facility
    FROM   cd.members m
           INNER JOIN cd.bookings b
                   ON m.memid = b.memid
           INNER JOIN cd.facilities f
                   ON b.facid = f.facid
    WHERE  f.NAME LIKE 'Tennis Court%'
    ORDER  BY "member",
              facility

### 6.Produce a list of costly bookings

**_How can you produce a list of bookings on the day of 2012-09-14 which will cost the member (or guest) more than $30? Remember that guests have different costs to members (the listed costs are per half-hour 'slot'), and the guest user is always ID 0. Include in your output the name of the facility, the name of the member formatted as a single column, and the cost. Order by descending cost, and do not use any subqueries._**

### 7. Produce a list of all members, along with their recommender, using no joins.

**_How can you output a list of all members, including the individual who recommended them (if any), without using any joins? Ensure that there are no duplicates in the list, and that each firstname + surname pairing is formatted as a column and ordered._**

    select
      distinct concat(m.firstname, ' ', m.surname) as member,
      (
        select
          concat (r.firstname, ' ', r.surname) as recommender
        from
          cd.members r
        where
          m.recommendedby = r.memid
      )
    from
      cd.members m
    order by
      member,
      recommender

### 8. Produce a list of costly bookings, using a subquery

**_The [Produce a list of costly bookings]) exercise contained some messy logic: we had to calculate the booking cost in both the WHERE clause and the CASE statement. Try to simplify this calculation using subqueries. For reference, the question was:_**

**_How can you produce a list of bookings on the day of 2012-09-14 which will cost the member (or guest) more than $30? Remember that guests have different costs to members (the listed costs are per half-hour 'slot'), and the guest user is always ID 0. Include in your output the name of the facility, the name of the member formatted as a single column, and the cost. Order by descending cost._**

    SELECT CONCAT(m.firstname, ' ', m.surname) AS member
           , a.NAME                          AS facility
           , a.cost
    FROM   (SELECT b.memid
                   , f.NAME
                   , ( CASE
                         WHEN b.memid = 0 THEN f.guestcost
                         ELSE f.membercost
                       END ) * b.slots AS cost
            FROM   cd.bookings AS b
                   INNER JOIN cd.facilities AS f
                           ON b.facid = f.facid
            WHERE  DATE(b.starttime) = '2012-09-14') AS a
           INNER JOIN cd.members AS m
                   ON a.memid = m.memid
                      AND a.cost > 30
    ORDER  BY cost DESC;
