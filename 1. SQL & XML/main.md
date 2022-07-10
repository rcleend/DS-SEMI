## Assignment 1: SQL/XML
____
**Goal**: Transform relational data to XML with SQL/XML, and brush up prerequisite knowledge on SQL.

a) Warming up: Give the old school SQL query that produces three columns: ‘name’, ‘year’, and ‘rating’.

> SELECT name, year, rating FROM movie

b) Give the SQL/XML query that produces three columns, each with an element: name, year, rating

> SELECT xmlelement(name name, name), xmlelement(name year, year), xmlelement(name rating, rating) FROM movie

c) Give the SQL/XML query that produces a single column, with all three elements name, year,
rating.

> SELECT xmlforest(name, year, rating) from movie

d) Give the SQL/XML query that produces a single column, with all three elements name, year,
rating nested inside an element movie. (Bonus: add an attribute ’id’ with the movie identifier
‘mid’.)

> SELECT xmlelement(name movie, xmlattributes(mid as id), xmlforest(name, year, rating)) FROM movie 

e) Write an ordinary SQL query that produces four columns, ‘name’, ‘year’, ‘rating’, and ’nr of roles’,
where the last column contains the number of actor roles for each movie. Then change the SELECT
part of your query, such that it produces lines like the following, such that the result enumerates all
roles instead of counting them (and commit this too):
`<movie><name>Jaws</name><year>1975</year><rating>8.2</rating><role>Chief Martin
Brody</role><role>Quint</role> ... </movie>`

> SELECT xmlelement(name movie, xmlelement(name name, name), xmlelement(name year, year), xmlelement(name rating, rating), xmlagg(xmlelement(name role, role))) from movie LEFT JOIN acts a ON movie.mid = a.mid GROUP BY name, year, rating

f) Write an ordinary SQL query that produces four columns, ‘name’, ‘year’, ‘rating’, ’nr of roles’,
’nr or directors’ where the last column contains the number of directors for each movie. Tip: Solution
2e can be done with a GROUP BY clause, but also without. Then change the SELECT part of your
query, such that it produces lines like the following (and commit this too):
`<movie><name>Jaws</name><year>1975</year><rating>8.2</rating><role>Chief Martin
Brody</role><role>Quint</role> ... <director>Steven Spielberg</director> </movie>`

> SELECT xmlelement(name movie, xmlelement(name name, m.name), xmlelement(name year, m.year), xmlelement(name rating, m.rating), xmlagg(xmlelement(name role, a.role)), xmlagg(xmlelement(name director, p.name)))
> from movie m
> LEFT JOIN acts a ON m.mid = a.mid
> LEFT JOIN directs d on m.mid = d.mid
> LEFT JOIN person p on d.pid = p.pid
> GROUP BY m.name, m.year, m.rating 