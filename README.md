# 🕵️ Kusto Detective Agency Hints
This repo contains some additional hints to the existing ones in the game of Kusto Detective Agency 🕵️ 🔐

<img src="https://detective.kusto.io/img/logo-inv.png" width=35% height=35%>

Kusto Detective Agency is an interactive big data contest with five different challenges.

> Welcome to the Kusto Detective Agency, rookie!

> I’m glad to have you on the team. This place sure needs some new blood.

> As the newest member of the department, you’re going to have to start small and work your way up the ladder. Click on the button below to get your first case assignment. Hurry up and do your best – rewards await the best new recruits. Be careful, though. Things aren’t always what they seem around here in Digitown.

> Good luck, rookie. You’re going to need it.

> The Lieutenant

[![Kusto](https://img.youtube.com/vi/BaW0qsxxYRc/hqdefault.jpg)](https://youtu.be/BaW0qsxxYRc)

## Getting started and how to use Azure Data Explorer
Visit to get all the instructions: https://detective.kusto.io/

To create a free Kusto cluster, you need either a Microsoft account (MSA) or an Azure Active Directory (AAD) identity. 

Create your cluster here: https://aka.ms/kustofree

Copy the Cluster URI you need this as a part of the answer.

After that we need to paste the below KQL script to ingest the data into a new table called Onboarding. Wait for 10 seconds for the script to complete. 

```kusto
.execute database script <|
// Create table for the data
.create-merge table Onboarding(Score:long)
// Import data
.ingest into table Onboarding ('https://kustodetectiveagency.blob.core.windows.net/onboarding/onboarding.csv.gz') with (ignoreFirstRecord=true)
```

Then calculate the sum of the "Score" column.

## Challenge 1 - The rarest book is missing!

<img src="https://detective.kusto.io/img/questions/01-jy6th.png" width=35% height=35%>

In this challenge you must find the book that's missing. The book will be at one of the shelves.

### Hint
Look at the weights of the sheleves and books. 

Take a look at the sum() function to learn more.

```kusto
summarize sum() by
```

[sum() function](https://learn.microsoft.com/en-us/azure/data-explorer/kusto/query/sum-aggfunction?WT.mc_id=AZ-MVP-5004683)

## Challenge 2 - Election fraud?

<img src="https://detective.kusto.io/img/questions/02-syh7t.png" width=35% height=35%>

Poppy the Goldfish are not so nice as you can think. In this challenge you need to prove that it has been a voting fraud and correct the numbers.

```kusto
// Query that counts the votes (with fraud):
Votes
| summarize Count=count() by vote
| as hint.materialized=true T
| extend Total = toscalar(T | summarize sum(Count))
| project vote, Percentage = round(Count*100.0 / Total, 1), Count
| order by Count
```

### Hint
Look at only Poppy's votes to get a feeling how the voting was and how he could get 2,601,570 votes. Sounds fishy! 🐠
Are the timing strange for each vote per IP address?

```kusto
summarize count() by bin()
```

[bin() function](https://learn.microsoft.com/en-us/azure/data-explorer/kusto/query/binfunction?WT.mc_id=AZ-MVP-5004683)

## Challenge 3 - Bank robbery

<img src="https://detective.kusto.io/img/questions/03-gb96s.png" width=35% height=35%>

Now you need to think like a robber, yeah I know, not so easy perhaps. 

### Hint
Look at the time when the robbers left the bank and expand your search to nearby Avenue and Streets, they did not left in the same car, nor at the same time. They for sure gathered at one place in the end, but arrivied at different times.

Look at the between() and arg_max() function to learn more.

```kusto
where Timestamp between()
arg_max()
join
```

[arg_max() function](https://learn.microsoft.com/en-us/azure/data-explorer/kusto/query/arg-max-aggfunction?WT.mc_id=AZ-MVP-5004683)

[join](https://learn.microsoft.com/en-us/azure/data-explorer/kusto/query/joinoperator?pivots=azuredataexplorer#join-flavors?WT.mc_id=AZ-MVP-5004683)

## Challenge 4 - Ready to play?

<img src="https://detective.kusto.io/img/questions/04-pq5sd.png" width=35% height=35%>

This challenge or case are divided into several parts. We need to first find a Special Prime Number, then go to a aka.ms page to get more data and hints to find the El Puente with some H3 cells (geospatial), get a Google Maps link, look around to find a key of some sort, correct the decrypt function so the message fit and then we are done!

Start by grabbing Prime Numbers from
https://kustodetectiveagency.blob.core.windows.net/prime-numbers/prime-numbers.csv.gz and educate yourself on Special Prime numbers (https://www.geeksforgeeks.org/special-prime-numbers), this should get you to
https://aka.ms/{Largest_special_prime_under_100M}

### Hint part 1 - Prime Numbers

```kusto
sort by <column>
serialize
prev() function
```

When you have the answer go then to aka.ms/ThatNumber

In the hint at the aka.ms page you will then be instructed to find El Puente.

> It's time to meet. Let's go for a virtual sTREEt tour... Across the Big Apple city, there is a special place with Turkish Hazelnut and four Schubert Chokecherries within 66-meters radius area. Go 'out' and look for me there, near the smallest American Linden tree (within the same area). Find me and the bottom line: my key message to you.

### Hint part 2
After you have imported the data as described in the aka.ms page, you will find two new functions. Decrypt will have the answer for you that you will insert into the detective.kusto.io site for this case. And the VirtualTourLink funktion will get you the Google Maps link that you need to find the hidden key.

![image](https://user-images.githubusercontent.com/34333810/199028928-c793c6a5-75ab-4408-81a0-4a10ea06b12a.png)

For me the KQL was very similiar to the Challenge (Case) 3.
Look for the places as descibed in the text "there is a special place with Turkish Hazelnut and four Schubert Chokecherries within 66-meters radius area" - you will find that in the spc_common column.

```kusto
summarize count() by h3cell = geo_point_to_h3cell(longitude, latitude, 10)
join kind=inner
```

At the end you will get the answer with:

```kusto
print h3cell = geo_h3cell_to_central_point("The_H3_Cell")
```

The answer from geo_h3cell_to_central_point will give you the Longitude and the Latitude. 
Take the values and give it to the function VirtualTourLink(lat,lon) - this will give you a Google Maps Link.
Now, look for the trees as stated in the Hints section at detective.kusto.io and find the key that you need to decode the encrypted message. It's hidden in the streets, not so low that's it's printed on the street and not so high as to the sky.

Could be that the key are from a catchy song...

Remember that the key are sensitive to spaces 🕵️

### Hint part 3
Now that you have the key, you will see that Data Explorer doesn't like the message.

![image](https://user-images.githubusercontent.com/34333810/199035681-a41fff8c-8b09-4f72-bf71-10d1a2af2fae.png)

First add "print" before Decrypt function.
The message must be formatted correctly, see what you can do with the @-sign.

```kusto
print Decrypt(_message=..., _key=...)
```

When you have the message and the key right - congrats! Read the text from the output, grab the hint (be aware of trailing commas) and then you will have your badge 🌟

## Challenge 5

<img src="https://detective.kusto.io/img/questions/05-bhr5e.png" width=35% height=35%>

Like challenge 4, this challenge has multiple steps to do to be able to get the answer. 
Now we are looking for a heist date, as well as Longitude and Latitude for the heist. 

After the import we have the table 'Chatlogs', start by taking 10 rows. 
Chatlogs
| take 10

Now we see Timestamp and Message. We need to break down Message into several parts - with parse. 
You need to find a specific channel that the gang are using, and take the bin() function with you - because the gang will not join the channel at the same time. 

When you have found the channel and the usernames, take a look at their IP addresses, you should collect 4 IP addresses. 
Use the hack tool provided in the Kusto Detective site and use the 4 IP addreses. You will see more clues, look at the pictures, the movie, the email and so on. 
Look at the pictures metadata, are there any useful information? 

Have you find the elephant and the geo location?
