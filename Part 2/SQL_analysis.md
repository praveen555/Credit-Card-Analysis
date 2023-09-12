## Let us look at how the data looks like 

```
select * from credit_data
limit 10;
```
![image](https://github.com/praveen555/Credit-Card-Analysis/assets/23379996/e81bb43a-6328-49de-bd6f-a256bde09ee8)

## Let's ask ourselves some questions as business owners. 

1. What is the amount spent per year so far ?

```
select year,round(sum(credit),2) as total_payment from credit_data
group by year 
```
![image](https://github.com/praveen555/Credit-Card-Analysis/assets/23379996/21b83636-cc9a-4a37-9653-936367133e2d)

Thoughts : It went quite up due to my India Trip in 2023. One point to note is that the date for 2023 is till 28th Aug. We need to take that into account when we are doing analysis

2. What is the percentage change of spend in year 2022 and 2023.

```
with cte1 as 
(
select year,round(sum(credit),2) as total_payment from credit_data
group by year
)
SELECT
 round((MAX(CASE WHEN year = 2023 THEN total_payment END) - MAX(CASE WHEN year = 2022 THEN total_payment END)) / MAX(CASE WHEN year = 2022 THEN total_payment END)* 100,2) AS percentage_change
FROM
  cte1;
```
![image](https://github.com/praveen555/Credit-Card-Analysis/assets/23379996/0c51e91a-72a2-4150-b4de-03a1f670b4cb)

3. What is monthly wise spend report for each month and on which month was it the highest ?
```
select year,month,round(sum(credit),2) as payment from credit_data
group by year,month
```
 ![image](https://github.com/praveen555/Credit-Card-Analysis/assets/23379996/92cec9ce-fc0f-4800-a554-e373274a327d)

```
select year,month,round(sum(credit),2) as payment from credit_data
group by year,month
)
select * from cte1
where payment= (SELECT MAX(payment) FROM cte1)
```
![image](https://github.com/praveen555/Credit-Card-Analysis/assets/23379996/8183db57-1eaf-4a9c-b51f-209510e0bb1e)

Holiday season :) 

4. When was it lowest ?

```
select year,month,round(sum(credit),2) as payment from credit_data
group by year,month
)
select * from cte1
where payment= (SELECT min(payment) FROM cte1)
```
![image](https://github.com/praveen555/Credit-Card-Analysis/assets/23379996/654c429f-7222-4f7f-866d-8175653f603b)
 
 that is my second month in canada
5. Count the distinct number of business_name, store number, province and location
```
select count(distinct(business_name)) as name_count,
count(distinct(store_number)) as store_count,
count(distinct(location)) as location_count,
count(distinct(province)) as province_count
from credit_data
```
![image](https://github.com/praveen555/Credit-Card-Analysis/assets/23379996/8dc470f1-e621-440b-b428-a0593331009e)

## Analysis of spend behaviours 

1. What is the highest number of times I have done a purchase ?
```
select business_name,count(business_name) as count,round(sum(credit),2) as money_spent
from credit_data
group by business_name
order by count desc
```
![image](https://github.com/praveen555/Credit-Card-Analysis/assets/23379996/b7c72ec7-3b87-481b-942d-51e0276accc8)

Thoughts : Rexall pharmacy is where I work as part time cashier so I used buy snacks during my break which accounts for the highest number of visits. 
Tim hortons is second with 150 visits and $573. Freshco is a grocery store which I have visited only 77 times but made purchases worth $ 1659.37. 
By slighly changing the question to highest purchases made we will get freshco as the first. That is intersting to see. Another interesting is see my presto charges $527. 

2. Let's see how many times I ate pizza.

```
SELECT *
FROM credit_data
WHERE business_name LIKE '%pizza%';
```
![image](https://github.com/praveen555/Credit-Card-Analysis/assets/23379996/74789fc0-9266-487f-9d2f-882d3a5d4470)

```
SELECT *
FROM credit_data
WHERE business_name LIKE '%pizza%';
```
![image](https://github.com/praveen555/Credit-Card-Analysis/assets/23379996/b3311459-d989-4427-b802-2c503fe600d2)

You can further filter it based on names like Pizza pizza, dominoes etc. Overall 16 times in the 1.8 years. Quite healthy ? One can argue though. 


## Probably a good idea have cateogories to tag each business name into one. Maybe will work on it a later stage. Could be naive method of maintaining and list of use a classifying model. However for further analysis let's move ahead 

3. Any particular store number I like ?

```
SELECT business_name,store_number, COUNT(store_number) AS count
FROM credit_data
WHERE store_number <> ''
GROUP BY store_number,business_name
ORDER BY count DESC;
```
![image](https://github.com/praveen555/Credit-Card-Analysis/assets/23379996/31946b00-598e-4e38-8370-d7ca54445122)

Thoughts- The rogers should technically be a account number which is >4 digits long however due to nature of my earlier cleaning this got captured as store number. 

Since tim hortons is the second most purchase let's analyze more on the store numebr of that by modifying the above query 

![image](https://github.com/praveen555/Credit-Card-Analysis/assets/23379996/248058db-0d78-4c3c-add8-64edd5de3f22)

Another example for not so frequent visits is dollarama.

![image](https://github.com/praveen555/Credit-Card-Analysis/assets/23379996/8b8fd906-a54e-4e88-b72c-5e9f615fa06d)

Thoughts- Good to have area/pincode mapped for each store number to analytics based on locations.


Final thoughts :- Mostly my credit card expenses are one sided. Grocieries from frescho, snacks during my break from Rexall and coffee addict from Tim hortons. This would be entirely different from someone who shops frequently, goes to movies etc. 

Let me know what other specific queries you want to see from my spend. :) 







