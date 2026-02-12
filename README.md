# IPL-Auction-Strategy-Analysis-Using-SQL
Developing a Data-Driven Auction Strategy for a New IPL Franchise using SQL Analytics

## IPL Auction Strategy Analysis using SQL  
**Author:** Chukwuemeka Reginald Nkwocha  
**Project Type:** SQL Data Analysis Project  
**Database:** PostgreSQL  
**Dataset:** IPL (Season 1 – Season 13, 2008–2020)

---

## Table of Contents

1. [Title](#title)  
2. [Project Overview / Problem Statement](#project-overview--problem-statement)  
3. [Data Sources](#data-sources)  
4. [Tools Used](#tools-used)  
5. [Database Design & Table Creation](#database-design--table-creation)  
6. [Data Cleaning & Preparation](#data-cleaning--preparation)  
7. [Exploratory Data Analysis (EDA)](#exploratory-data-analysis-eda)  
8. [Data Analysis & SQL Implementation](#data-analysis--sql-implementation)  
   - Aggressive Batters (High Strike Rate)  
   - Anchor Batters (High Average)  
   - Hard Hitters (Boundary %)  
   - Economical Bowlers  
   - Wicket-Taking Bowlers  
   - All-Rounders  
   - Wicketkeeper Selection Criteria  
   - Additional Assessment Questions  
9. [Results & Insights](#results--insights)  
10. [Recommendations](#recommendations)  
11. [Limitations](#limitations)  

---

## 1 Title

### **Developing a Data-Driven Auction Strategy for a New IPL Franchise Using SQL Analytics**

---

## 2 Project Overview / Problem Statement

A new IPL franchise is entering the league and must build a strong, balanced squad during the Mega Auction. The management must:

- Operate within a fixed auction budget.
- Select players based on performance metrics.
- Ensure squad balance (batters, bowlers, all-rounders, wicketkeepers).
- Identify value-for-money players.

As the Data Analyst, the objective of this project is to:

- Analyze historical IPL data (2008–2020).
- Create performance metrics using SQL.
- Identify top 10 candidates in each player category.
- Provide visual and analytical insights to guide auction decisions.

The goal is to combine **performance analytics + financial prudence** to develop a winning auction strategy.

---

## 3 Data Sources

The dataset includes IPL match and ball-by-ball data from Season 1 to Season 13.

### Tables Used:

### 1. `IPL_matches1`
Contains:
- Match ID
- City
- Date
- Venue
- Teams
- Winner
- Toss details
- Player of the match
- Result information

### 2. `IPL_Ball`
Contains ball-by-ball data:
- Batsman
- Bowler
- Runs scored
- Extras
- Dismissal type
- Teams
- Innings, over, ball

This granular data allowed accurate calculation of:
- Strike rate
- Batting average
- Economy rate
- Bowling strike rate
- Boundary percentage

---

## 4 Tools Used

- **PostgreSQL 16**
- SQL (Advanced Queries, Joins, Aggregations, CASE statements)
- Excel (CSV import)
- Data Visualization (Charts & Tables in PPT)
- GitHub (Recommended for upload)

---

## 5 Database Design & Table Creation

Two main tables were created:

```sql
CREATE TABLE IPL_matches1 (
    id INT PRIMARY KEY,
    city VARCHAR,
    date DATE,
    player_of_match VARCHAR,
    venue VARCHAR,
    neutral_venue BOOLEAN,
    team1 VARCHAR,
    team2 VARCHAR,
    toss_winner VARCHAR,
    toss_decision VARCHAR,
    winner VARCHAR,
    result VARCHAR,
    result_margin INT,
    eliminator VARCHAR,
    method VARCHAR,
    umpire1 VARCHAR,
    umpire2 VARCHAR
);
```

```sql
CREATE TABLE IPL_Ball (
    id INT,
    inning INT,
    over INT,
    ball INT,
    batsman VARCHAR,
    non_striker VARCHAR,
    bowler VARCHAR,
    batsman_runs INT,
    extra_runs INT,
    total_runs INT,
    is_wicket BOOLEAN,
    dismissal_kind VARCHAR,
    player_dismissed VARCHAR,
    fielder VARCHAR,
    extras_type VARCHAR,
    batting_team VARCHAR,
    bowling_team VARCHAR
);
```

CSV files were imported using `COPY` command.

---

## 6 Data Cleaning & Preparation

Key Cleaning Steps:

- Ensured correct date format (`SET datestyle = 'ISO, DMY'`)
- Excluded wides when calculating balls faced
- Used `NULLIF()` to prevent division-by-zero errors
- Filtered players with:
  - Minimum 500 balls (batters)
  - Minimum 500 balls bowled (bowlers)
  - Minimum 300 balls bowled (all-rounders)
- Removed dismissal_kind = 'NA'
- Used `CASE WHEN` to classify deliveries

---

## 7 Exploratory Data Analysis (EDA)

EDA included:

- Total runs by player
- Total dismissals
- Balls faced
- Balls bowled
- Seasonal participation
- Boundary frequency
- Extra runs conceded
- Venue-wise run distribution

This helped in understanding:

- Player consistency
- Experience (seasons played)
- Role specialization
- Performance trends

---

## 8 Data Analysis & SQL Implementation

---

### A. Aggressive Batters (High Strike Rate)

**Criteria:**
- Minimum 500 balls faced
- Strike Rate = (Total Runs / Balls Faced) × 100
- Exclude wides from balls faced

Selected top 10 by strike rate.

Insight:
Players had similar high strike rates — allowing flexibility in final auction decisions.

---

### B. Anchor Batters (High Average)

**Criteria:**
- Played more than 2 IPL seasons
- At least 1 dismissal
- Batting Average = Total Runs / Dismissals

Selected top 10 based on average.

Insight:
Anchors provide stability and partnership-building ability.

---

### C. Hard Hitters (Boundary %)

**Criteria:**
- Played > 2 seasons
- Boundary % = Boundary Runs / Total Runs
- Only 4s and 6s counted

Ranked top 10 by boundary percentage.

Insight:
These players are ideal for middle overs and death overs acceleration.

---

### D. Economical Bowlers

**Criteria:**
- Bowled ≥ 500 balls
- Economy Rate = Runs Conceded / Overs Bowled

Top 10 lowest economy bowlers selected.

Insight:
Lower economy = stronger control and defensive strength.

---

### E. Wicket-Taking Bowlers

**Criteria:**
- ≥ 500 legitimate balls
- Strike Rate = Balls Bowled / Wickets

Top 10 lowest bowling strike rates selected.

Insight:
These bowlers consistently break partnerships.

---

### F. All-Rounders

**Criteria:**
- ≥ 500 balls faced
- ≥ 300 balls bowled
- High batting strike rate
- Low bowling strike rate

Ranked by batting SR (desc) & bowling SR (asc).

Insight:
Provides flexibility and squad balance.

---

###  G. Wicketkeeper Selection Criteria

Defined criteria:

- High Batting Strike Rate
- Good Batting Average
- Total Runs
- Total Dismissals (Catches + Stumpings)
- Stumping efficiency

In T20, wicketkeeper must be both:
- Aggressive batter
- Reliable behind the stumps

---

###  Additional Assessment Queries

Included:

1. Count of cities hosting IPL
2. Created `deliveries_v02`
3. Counted boundaries & dot balls
4. Team-wise boundary count
5. Team-wise dot balls bowled
6. Dismissals by type
7. Top 5 bowlers conceding extras
8. Created `deliveries_v03`
9. Venue-wise total runs
10. Year-wise Eden Gardens runs

All required `CREATE TABLE` statements were included.

---

## 9 Results & Insights

- Identified top 10 players in each category.
- Observed minimal performance gaps among top-tier players.
- Strike rate and economy differences were marginal.
- Some players stood out significantly in average.
- Eden Gardens recorded high yearly run totals.
- Certain bowlers consistently conceded high extras.

---

## 10 Recommendations

1. Select 2–3 aggressive batters from top 10 based on purse.
2. Combine:
   - 2 Anchors
   - 2 Hard Hitters
   - 2 Economical Bowlers
   - 2 Strike Bowlers
   - 2 All-rounders
   - 1–2 Wicketkeepers
3. Avoid overspending on marginal performance differences.
4. Prioritize multi-skilled players.
5. Balance economy + strike bowlers.

A balanced squad increases probability of playoff qualification.

---

## 11 Limitations

- Dataset limited to Season 1–13.
- No injury or fitness data.
- No auction price history included.
- Pitch conditions not factored.
- Player form beyond 2020 not included.
- Boundary calculation used only batting data, not match situation.

Future improvements:
- Include machine learning for player valuation.
- Add salary cap modeling.
- Include venue performance splits.

---






- Personal Portfolio Website

