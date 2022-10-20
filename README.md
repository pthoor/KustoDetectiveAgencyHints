# 🕵️ Kusto Detective Agency Hints
This repo contains some additional hints to the existing ones in the game of Kusto Detective Agency 🕵️ 🔐

![](https://detective.kusto.io/img/logo-inv.png)

Kusto Detective Agency is an interactive big data contest with five different challenges. Here's the first instruction:

> Welcome to the Kusto Detective Agency, rookie!

> I’m glad to have you on the team. This place sure needs some new blood.

> As the newest member of the department, you’re going to have to start small and work your way up the ladder. Click on the button below to get your first case assignment. Hurry up and do your best – rewards await the best new recruits. Be careful, though. Things aren’t always what they seem around here in Digitown.

> Good luck, rookie. You’re going to need it.

> The Lieutenant

## Getting started and how to use Azure Data Explorer
https://detective.kusto.io/


## Challenge 1
The rarest book is missing!
![](https://detective.kusto.io/img/questions/01-jy6th.png)

```kusto

```

## Challenge 2
Election fraud?
![](https://detective.kusto.io/img/questions/02-syh7t.png)

```kusto
summarize by bin()
```

https://learn.microsoft.com/en-us/azure/data-explorer/kusto/query/binfunction?WT.mc_id=AZ-MVP-5004683

## Challenge 3
Bank robbery
![](https://detective.kusto.io/img/questions/03-gb96s.png)

```kusto
summarize Timestamp
arg_max()
join
```

https://learn.microsoft.com/en-us/azure/data-explorer/kusto/query/arg-max-aggfunction?WT.mc_id=AZ-MVP-5004683

https://learn.microsoft.com/en-us/azure/data-explorer/kusto/query/joinoperator?pivots=azuredataexplorer#join-flavors?WT.mc_id=AZ-MVP-5004683

## Challenge 4

## Challenge 5
