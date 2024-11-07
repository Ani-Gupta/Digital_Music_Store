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
