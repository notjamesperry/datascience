# James Perry - Data Science Portfolio
Welcome to all my data science junk! This is my portfolio in progress.



## Project 2 - Tableau - "True Income" by US Area

### PROBLEM
I was interested in answering this question: Where would the average person have the highest AVERAGE SPENDING POWER? (because it doesn't matter that your income is twice as high if your costs are twice as high)

### SOLUTION
  
**COLLECT THE DATA**

I searched all over the US government websites until I found enough data to get interesting insights. I would have rather had data on the city level, but that was not available. I finally found metro area data for both income and cost of living (at least RPP) to start coming to conclusions.
  
**CLEAN THE DATA**

I brought the tables into a database and used SQL queries to get a processed version of the data, knowing that I would need both the metro areas and the states so I could group the areas by state in Tableau
```
/*
CREATE TABLE vast-mapper-337320.income_v_cost.states (
    name STRING(32),
    abbr STRING(2)
);
INSERT INTO vast-mapper-337320.income_v_cost.states (name, abbr) 
    values ('Alabama', 'AL'),
       ('Alaska', 'AK'),
       ('Arizona', 'AZ'),
       ('Arkansas', 'AR'),
       ('California', 'CA'),
       ('Colorado', 'CO'),
       ('Connecticut', 'CT'),
       ('Delaware', 'DE'),
       ('District of Columbia', 'DC'),
       ('Florida', 'FL'),
       ('Georgia', 'GA'),
       ('Hawaii', 'HI'),
       ('Idaho', 'ID'),
       ('Illinois', 'IL'),
       ('Indiana', 'IN'),
       ('Iowa', 'IA'),
       ('Kansas', 'KS'),
       ('Kentucky', 'KY'),
       ('Louisiana', 'LA'),
       ('Maine', 'ME'),
       ('Maryland', 'MD'),
       ('Massachusetts', 'MA'),
       ('Michigan', 'MI'),
       ('Minnesota', 'MN'),
       ('Mississippi', 'MS'),
       ('Missouri', 'MO'),
       ('Montana', 'MT'),
       ('Nebraska', 'NE'),
       ('Nevada', 'NV'),
       ('New Hampshire', 'NH'),
       ('New Jersey', 'NJ'),
       ('New Mexico', 'NM'),
       ('New York', 'NY'),
       ('North Carolina', 'NC'),
       ('North Dakota', 'ND'),
       ('Ohio', 'OH'),
       ('Oklahoma', 'OK'),
       ('Oregon', 'OR'),
       ('Pennsylvania', 'PA'),
       ('Rhode Island', 'RI'),
       ('South Carolina', 'SC'),
       ('South Dakota', 'SD'),
       ('Tennessee', 'TN'),
       ('Texas', 'TX'),
       ('Utah', 'UT'),
       ('Vermont', 'VT'),
       ('Virginia', 'VA'),
       ('Washington', 'WA'),
       ('West Virginia', 'WV'),
       ('Wisconsin', 'WI'),
       ('Wyoming', 'WY');
*/   
SELECT *,
    (select states.name
    from `vast-mapper-337320.income_v_cost.states` as states 
    where abbr LIKE state) as state_name
FROM 
    (SELECT *, SUBSTR(metro_area, LENGTH(metro_area ) - 1, LENGTH(metro_area ) ) as state
    FROM 
        (SELECT SUBSTR(string_field_1, 0, LENGTH(string_field_1) - LENGTH(' (Metropolitan Statistical Area)')) as metro_area, 
            int64_field_2 as income
        FROM `vast-mapper-337320.income_v_cost.income_by_msa` as inc_msa
        WHERE safe_cast(inc_msa.string_field_0 AS INT) is not null
            and string_field_1 != 'United States'
        )
    ) as inc_msa_clean
SELECT *,
    (select states.name
    from `vast-mapper-337320.income_v_cost.states` as states 
    where abbr LIKE state) as state_name
FROM 
    (SELECT *, SUBSTR(metro_area, LENGTH(metro_area ) - 1, LENGTH(metro_area ) ) as state
    FROM 
        (SELECT SUBSTR(string_field_1, 0, LENGTH(string_field_1) - LENGTH(' (Metropolitan Statistical Area)')) as metro_area, 
            double_field_2 as rpp
        FROM `vast-mapper-337320.income_v_cost.rpps_by_msa` as rpp_msa
        WHERE safe_cast(rpp_msa.string_field_0 AS INT) is not null
            and string_field_1 != 'United States'
        )
    ) as rpp_msa_clean
```

**VISUALISE THE DATA**

I created a map of all data, with a state layer and a metro layer, allowing you to drill down through the state layer in the metro area. I wanted to cleary see the highest and lowest value per state and per metro area within each state. The only interesting finding for seeing the metro areas across states, in my opinion, was to see the minimum and maximum, so I went ahead and displayed those for both state and metro area.
Unfortuantely, Hawaii is one of the minimums and is off the map for the default view.

[BEST/WORST TRUE INCOME areas of the US](https://public.tableau.com/views/BESTWORSTTRUEINCOMEareasoftheUS/Dashboard1?:language=en-US&:display_count=n&:origin=viz_share_link).

Here is a preview. Please follow the above link to explore the data yourself.
![Picture of Tableau maps](/images/Dashboard_IncomeCost.png)

**HOW I WOULD IMPROVE**

- I would want data for all areas of the state, preferably cities if I could find the data.
- Beautiful design is not my strength, and I would like it to look better.
- I would figure out a way to display Hawaii and Alaska next to the US map.
- I would see if there was a better way to get state information rather than just getting the mean average of the metros, possibly by accounting for the population of those metro areas compared to the states.



## Project 1 - Google Sheets - Compensation Plan
- PROBLEM: JK Studios has 10 members, each contributing different amounts of work in different roles on over 100 projects. How do we pay everyone fairly according to their work, as well as pay a portion to members as an equal distribution simply for their founding contribution to the brand?
- SOLUTION:
  - Held a meeting to convince the other 9 members of my plan.
  - Collaborated to determine the worth of every role that might be done by a member, and create a template grid. 
  - Created a "Video Template" with a grid of predetermined points where contributors points are added by the project manager.
  - Created a "Payroll Template". Every Pay period, data for the income from each project is downloaded and portioned out according to the points distribution for that project. Use the "Payroll template" which contains the spreadsheet relationships we established, and enter copy the video income into it.
  - Entered the totals calculated for each individual into our payroll system and got everyone paid.
- DUE TO SENSITIVE INFORMATION: I cannot display the spreadsheet workbook publicly. Please contact me for a preview where real numbers have been changed. Thanks!

### Payroll Template
![Payroll Template](/images/Payroll_Template_Screenshot.png)

### Video Template
![Video Template](/images/Video_Template_Screenshot.png)
