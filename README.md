Q1: Who is the senior most employee based on job title?
select employee_id, last_name, first_name, title, Levels from employee
order by Levels desc
limit 1;

Q2: Which countries have the most Invoices?
select billing_country, count(invoice_id) as No_of_invoices from invoice
group by billing_country;

Q3: What are top 3 values of total invoice?
select * from invoice
order by total desc
limit 3;

Q4: Which city has the best customers? We would like to throw a promotional Music Festival in the city we made the most money. Write a query that returns one city that has 
the highest sum of invoice totals. Return both the city name & sum of all invoice totals
select billing_city, sum(total) as total_sum from invoice
group by billing_city
order by total_sum desc;

Q5: Who is the best customer? The customer who has spent the most money will be declared the best customer. Write a query that returns the person who has spent the most money.
select c.customer_id, c.first_name, c.last_name, sum(i.total) as sum_total from customer c join invoice i
on c.customer_id=i.customer_id
group by c.customer_id, c.first_name, c.last_name
order by sum_total desc
Limit 1;

Q1: Write query to return the email, first name, last name, & Genre of all Rock Music listeners. Return your list ordered alphabetically by email starting with A
select c.first_name, c.last_name, c.email from customer c
join invoice i on c.customer_id = i.customer_id
join invoice_line il on i.invoice_id = il.invoice_id
join track t on t.track_id = il.track_id
where genre_id = (select genre_id from genre where genre_id = 1)
group by c.first_name, c.last_name, c.email
order by email;

Q2: Let's invite the artists who have written the most rock music in our dataset. Write a query that returns the Artist name and total track count of the top 10 rock bands
select a.name, count(*) from artist a
Join album b on a.artist_id = b.artist_id
Join track t on b.album_id = t.album_id
where genre_id = (select genre_id from genre where genre_id=1)
group by a.name
order by count(*) desc
limit 10;	

Q3: Return all the track names that have a song length longer than the average song length. 
Return the Name and Milliseconds for each track. Order by the song length with the longest songs listed first.
select name, milliseconds from track
where milliseconds>(select avg(milliseconds) from track)
order by milliseconds desc;

Q1: Find how much amount spent by each customer on artists? Write a query to return customer name, artist name and total spent
WITH best_selling_artist AS (
SELECT artist.artist_id AS artist_id, artist.name AS artist_name,
SUM(invoice_line.unit_price*invoice_line.quantity) AS total_sales
FROM invoice_line
JOIN track ON track.track_id = invoice_line.track_id
JOIN album ON album.album_id = track. album_id
JOIN artist ON artist.artist_id = album.artist_id
GROUP BY artist_id, artist_name
ORDER BY total_sales DESC
LIMIT 1
)

SELECT c.customer_id, c.first_name, c.last_name, bsa.artist_name,
SUM(il.unit_price*il.quantity) AS amount_spent
FROM invoice i
JOIN customer c ON c.customer_id = i.customer_id
JOIN invoice_line il ON il. invoice_id = i.invoice_id
JOIN track t ON t. track_id = il.track_id
JOIN album alb ON alb. album_id = t. album_id
JOIN best_selling_artist bsa ON bsa.artist_id = alb.artist_id
GROUP BY c.customer_id, c.first_name, c.last_name, bsa.artist_name
ORDER BY amount_spent DESC;

Q2: We want to find out the most popular music Genre for each country. We determine the most popular genre as the genre with the highest amount of purchases. Write a query that returns each country along with the top Genre. For countries where the maximum number of purchases is shared return all Genres.

