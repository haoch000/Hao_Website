---
date: "2021-10-01"
math: true
title: 3.CEO panel
type: book
weight: 30
diagram: true
---

## Goal:
---
1.  Get CEOs, their spouses and voter history records from LexisNexis data. 
2.  Get panel data of CEOs living addresses and their companies' headquarter addresses from 2000 to 2020
<br/><br/>

## Data source
---
**LexisNexis**: CEOs information

**Compustat Execucomp** and **BoardEx** : CEOs current and previous working company and title
**Company HQ address**: crwaled from SEC 10k reports (from *2.Crawl SEC data*)
<br/><br/>

## Process flowchart
---
```mermaid
graph TD
A(Lexis reports) -->B((Lexis_parsing.py))
B--> |cleaning result and get coordinates|C((err and coor.py))

C --> D1(CEO home info)
C --> D2(CEO Spouse info)
C --> D3(CEO voter info)
A -->|Boats.py| C4(CEOs boat info)

D1-->B2((CEO_panel.py))
D3-->B2
B2-->C2[CEO living address panel]

```

```mermaid
graph TD
A1("Company HQ address (Crawl SEC data)" )-->B1((CEO_panel.py))
A2( Execucomp data)-->B1
A3( BoardEx data)-->B1
B1-->C1[CEOs working companies HQ address panel]
```
<br/><br/>

## Code detail
---
#### Parsing
Since all reports are in the same format, I create some **searching functions** for different **text structures**, e.g., search the content after a keyword; search the content in the following line of a keyword. For addresses, we need to get a list of them. So I search for the beginning and end of this section(like the house address section) and **separate** them based on some **delimiter** like (/n). And then parse each one using a searching function I created

#### Panel (2000-2020)
**Home address**: For each year, I use the house with **max value** and **max number of household members** separately as a proxy for CEOs living addresses. 

**HQ address**: I collect CEO's previous working companies' HQ addresses from **Compustat Execucomp** and **BoardEx**. And then use CIK to match with crawled **Company HQ address**.