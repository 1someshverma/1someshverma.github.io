---
layout: post
title: Utilising community bonding period Effectively!
---

I started to contribute to RADIS from January'23 and whenever i needed help in some issue , mentors were quite helpful in providing revelant information to resolve that issue . And this thing helped me , to develop good bonding and understanding with the mentors .

So,I thought, i should use community bonding period more productively thus, i decided to work on project from 15 May . 

In community bonding period i have done following things -

- Read the RADIS and HITRAN Paper (selected parts)
- Worked on Loading the dataframe in Vaex format (task of first week)

##### Loading Dataframe in Vaex format
There are two main functions which are used load databank in RADIS
These are :  
- fetch_databank()
- load_databank()

fetch_databank() : It is used to load databank from standard databank like HITRAN,HITEMP,EXOMOL,GEISA by fetching it from their respective APIs, then parsing and processing them.

load_databank() : It is used to load databank from the local file or to load databank from user defined databank . 

Upto now, i have worked on fetch_databank() function to load dataframe in Vaex format , some of the things were already implemented while at other points i needed to write code specifically for vaex dataframe format.

Similarly, for load_databank() function , the hurdle was to parse it in vaex dataframe format as virual columns which are used in vaex dataframe to reduce memory use , it was throwing error . After trying many i finally found a fix for it .


Now, I have made necessary changes to these two functions to load dataframe in Vaex format. Next week i will be working on writing test cases for these.