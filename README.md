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

## Challenge 1
The rarest book is missing!

<img src="https://detective.kusto.io/img/questions/01-jy6th.png" width=35% height=35%>

In this challenge you must find the book that's missing. The book will be at one of the shelves.

### Hint
Look at the weights of the sheleves and books. 

Take a look at the sum() function to learn more.

```kusto
summarize sum() by
```

[sum() function](https://learn.microsoft.com/en-us/azure/data-explorer/kusto/query/sum-aggfunction?WT.mc_id=AZ-MVP-5004683)

## Challenge 2
Election fraud?

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

## Challenge 3
Bank robbery

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

## Challenge 4
To be released - on October 30, 2022 (Sunday night) 00:00 UTC

## Challenge 5
To be released - on November 13, 2022 (Sunday night) 00:00 UTC
