# Bank Loan Analysis

Dashboard Link : https://github.com/user-attachments/assets/d46b69e7-ce73-41a1-8865-94716278771e

## Project Overview: 

This project undertakes a comprehensive analysis of bank loan data, leveraging SQL for robust data querying and manipulation. The analysis is complemented by advanced visualization and further exploration using Power BI. The primary objective is to extract actionable insights concerning loan applications, funding, repayments, and borrower demographics. The analysis spans various dimensions, including temporal (monthly trends, loan terms), geographical (state-level analysis), and categorical aspects (loan purposes, home ownership). 

## KPI Requirements: 

    1) Total Loan Applications: Calculate the total number of loan applications received during a specific period, monitor MTD Loan applications and track changes MoM. 

    2) Total Funded Amount: Understand the total amount of funds disbursed as loans, and keep an eye on the MTD Total Funded Amount and analyse MoM changes in this metric. 

    3) Total Amount Received: Track the amount received from borrowers(assesing banks cash flow and loan repayment), and analyse the MTD amount received and MoM changes.

    4) Average Interest Rate: Calculate the average interest rate across all loans, MTD and monitor the MoM variations.
    
    5) Average Debt-to-Income Ratio: Evaluate the average DTI for the borrowers, for all the loans, MTD and track MoM fluctuations. 

    6) Good Loan vs Bad Loan KPIs w.r.t.:  

    a) Number of Applications
    b) Application Percentage
    c) Funded Amount
    d) Total Received Amount

### Tools Used: SQL to obtain the results by retrieving relevant results from the respective tables with the help of SQL queries and visualizing the relevant KPIs in the form of interactive elements for better understanding. 

### Steps Followed: Part 1 - Data Preparation, Query & Manipulation Through SQL  

- Step 1 : Review the dataset which is available as csv files in excel and analyse the number of column heads and the number of rows for the data(24 columns and 38756 rows in this case). Get familiar with the data and spend some time analysing the column heads along with data types.  

- Step 2: Load data into MS SQL server by creating a new database and importing the aforementioned flat file as table. For each of the table inserted within the database, change the datatypes of the column heads as required.   For e.g. - 

      a) change the datatype of "emp_title" column head from nvarchar(50) to varchar(100) datatype. The data is then cleaned by modifying the columns types appropriately for each of the column heads. 

      b) do remember to add primary keys to the table("id" column in this case) while modifying the columns. 

- Step 3: The next step includes firing SQL queries depending upon the problem statements and storing the results. We will also prepare a WORD file to document the SQL queries for future reference. 

#### SQL QUERIES: 

a) To find out the total number of Loan Applications -  

    SELECT COUNT(id) 
    FROM bank_loan_data

![BL 1](https://github.com/user-attachments/assets/1d53e6bb-79de-46db-98d0-bda4ed06cf00)

b) To find MTD Loan Applications(for December): 

    SELECT COUNT(id) AS MTD_Loan_Applications
    FROM bank_loan_data
    WHERE MONTH(issue_date) = 12

 Below is the snapshot of the query fired and the subsequent result:

![BA 2](https://github.com/user-attachments/assets/50c49a1b-1076-4724-8a2d-4779bfe2240c)

c) To find the PMTD Loan Applications - 

    SELECT COUNT(id) AS PMTD_Loan_Applications
    FROM bank_loan_data
    WHERE MONTH(issue_date) = 11


![BA 3](https://github.com/user-attachments/assets/c1d0adac-a8ef-4827-930d-35eff20d1e56)

 d) To find Month-on-Month Change in Loan Applications(in%)  - 

    SELECT 
    CAST(
        ROUND(
            (
                (SELECT COUNT(id) FROM bank_loan_data WHERE MONTH(issue_date) = 12 AND YEAR(issue_date) = 2021) -  
                (SELECT COUNT(id) FROM bank_loan_data WHERE MONTH(issue_date) = 11 AND YEAR(issue_date) = 2021)
            ) * 100.0 / 
            NULLIF(
                (SELECT COUNT(id) FROM bank_loan_data WHERE MONTH(issue_date) = 11 AND YEAR(issue_date) = 2021), 0
            ), 2
        ) AS DECIMAL(10, 2)
    ) AS MoM_Change_Percentage;

The results of above query will be displayed as follows - 

![BA 4](https://github.com/user-attachments/assets/48595d6a-92b9-4752-a46a-d37e9c5d5f2e) 

e) 5)	To find out the Total Funded Amount -
    
    SELECT SUM(loan_amount) AS Total_Funded_Amount
    FROM bank_loan_data

![BA 5](https://github.com/user-attachments/assets/994290bc-1208-4ddd-a48e-2bca017f81bd)

f) To find out the MTD Funded Amount(for December) - 

    SELECT SUM(loan_amount) AS MTD_Funded_Amount
    FROM bank_loan_data
    WHERE MONTH(issue_date) = 12

![BA 6](https://github.com/user-attachments/assets/1a14ea66-9972-48fc-8f11-8a7d08cd8319)

g) To find out the PMTD Funded Amount -

    SELECT SUM(loan_amount) AS PMTD_Funded_Amount
    FROM bank_loan_data
    WHERE MONTH(issue_date) = 11

![BA 7](https://github.com/user-attachments/assets/a36eae45-526f-4c41-bad4-52203be417ba)


h) To find Month-on-Month Change in Funded Amount(in%) - 

      SELECT 
    CAST(
        ROUND(
            (
                (SELECT SUM(loan_amount) FROM bank_loan_data WHERE MONTH(issue_date) = 12 AND YEAR(issue_date) = 2021) -  
                (SELECT SUM(loan_amount) FROM bank_loan_data WHERE MONTH(issue_date) = 11 AND YEAR(issue_date) = 2021)
            ) * 100.0 / 
            NULLIF(
                (SELECT SUM(loan_amount) FROM bank_loan_data WHERE MONTH(issue_date) = 11 AND YEAR(issue_date) = 2021), 0
            ), 2
        ) AS DECIMAL(10, 2)
    ) AS MoM_Change_Percentage;


![BA 8](https://github.com/user-attachments/assets/1da67487-f600-43cb-8a44-5e08c94029e1)

i) To find out Total Amount Received - 

    SELECT SUM(total_payment) AS Total_Amount_Received
    FROM bank_loan_data

![BA 9](https://github.com/user-attachments/assets/d27fe45a-e0c5-4188-9d37-59623d02f8fd)

j) To find out MTD Amount Received(for December) – 

    SELECT SUM(total_payment) AS MTD_Funded_Amount
    FROM bank_loan_data
    WHERE MONTH(issue_date) = 12

![BA 10](https://github.com/user-attachments/assets/7c2b0411-1f0c-4095-a859-ea51e2fbc384)

k) To find out PMTD Amount Received –

    SELECT SUM(total_payment) AS PMTD_Funded_Amount
    FROM bank_loan_data
    WHERE MONTH(issue_date) = 11

![BA 11](https://github.com/user-attachments/assets/e8e64f46-da2f-4cd5-b92c-68b61a203616)

l) To find Month-on-Month Change in Funded Amount(in%) - 
    
    SELECT 
    CAST(
        ROUND(
            (
                (SELECT SUM(total_payment) FROM bank_loan_data WHERE MONTH(issue_date) = 12 AND YEAR(issue_date) = 2021) -  
                (SELECT SUM(total_payment) FROM bank_loan_data WHERE MONTH(issue_date) = 11 AND YEAR(issue_date) = 2021)
            ) * 100.0 / 
            NULLIF(
                (SELECT SUM(total_payment) FROM bank_loan_data WHERE MONTH(issue_date) = 11 AND YEAR(issue_date) = 2021), 0
            ), 2
        ) AS DECIMAL(10, 2)
    ) AS MoM_Change_Percentage; 

![BA 12](https://github.com/user-attachments/assets/5f81d1d7-3b78-48ab-af2f-c6c3a2c640a1)


m) To find out Average Interest Rate across all the loans - 

    SELECT (ROUND(AVG(int_rate), 4) * 100) AS Average_Interest_Rate
	FROM bank_loan_data


![BA 13](https://github.com/user-attachments/assets/082d6453-76d2-4945-961f-a01fb390a2f7)


n) To find out MTD Average Interest Rate(for December) -

    SELECT (ROUND(AVG(int_rate), 4) * 100) AS MTD_Average_Interest_Rate
	FROM bank_loan_data
	WHERE MONTH(issue_date) = 12;

    
![BA 14](https://github.com/user-attachments/assets/1895859d-b78c-4528-a917-c108ace2f828)

o) To find out MoM Variations in Average Interest Rates - 
  

    SELECT 
    CAST(
        ROUND(
            (
                (SELECT AVG(int_rate) FROM bank_loan_data WHERE MONTH(issue_date) = 12 AND YEAR(issue_date) = 2021) -  
                (SELECT AVG(int_rate) FROM bank_loan_data WHERE MONTH(issue_date) = 11 AND YEAR(issue_date) = 2021)
            ) * 100.0 / 
            NULLIF(
                (SELECT AVG(int_rate) FROM bank_loan_data WHERE MONTH(issue_date) = 11 AND YEAR(issue_date) = 2021), 0
            ), 2
        ) AS DECIMAL(10, 2)
    ) AS MoM_Change_Percentage;

![BA 15](https://github.com/user-attachments/assets/dca42276-e6dd-4d99-a75d-d6c9c25d20a6)      

p) To find out Average Debt-to-Income(DTI) Ratio across all loans -  

    SELECT ROUND(AVG(dti), 4) * 100 AS Average_DTI
    FROM bank_loan_data

![BA 16](https://github.com/user-attachments/assets/86653ec4-cc51-40ba-a3d2-251baae5a1e4)

q) To find out MTD Average Debt-to-Income(DTI) Ratio(for December) - 

    SELECT ROUND(AVG(dti), 4) * 100 AS MTD_Average_DTI
    FROM bank_loan_data
    WHERE MONTH(issue_date) = 12;

![BA 17](https://github.com/user-attachments/assets/2ccd1fe4-c693-49d8-8f05-18bb663f74a2)

r) To find out PMTD Average Debt-to-Income(DTI) Ratio - 

    SELECT ROUND(AVG(dti), 4) * 100 AS PMTD_Average_DTI
    FROM bank_loan_data
    WHERE MONTH(issue_date) = 11;

![BA 18](https://github.com/user-attachments/assets/becc19f3-19d7-431a-a92a-458ba347f12a)  

s) To find out MoM Variations in Average Debt-to-Income(DTI) Ratio - 

    SELECT 
    CAST(
        ROUND(
            (
                (SELECT AVG(dti) FROM bank_loan_data WHERE MONTH(issue_date) = 12 AND YEAR(issue_date) = 2021) -  
                (SELECT AVG(dti) FROM bank_loan_data WHERE MONTH(issue_date) = 11 AND YEAR(issue_date) = 2021)
            ) * 100.0 / 
            NULLIF(
                (SELECT AVG(dti) FROM bank_loan_data WHERE MONTH(issue_date) = 11 AND YEAR(issue_date) = 2021), 0
            ), 2
        ) AS DECIMAL(10, 2)
    ) AS MoM_Change_Percentage;

![BA 19](https://github.com/user-attachments/assets/f598560b-e2cd-46ce-aa10-c7d463b83cda)

- Step 4: Now, we will analyse the data in terms of Good Loans vs Bad Loans which will be based on the number of application percentage for each type, funded amount and amount received. 
NOTE - Good Loans are the ones whose Loan status is either "Fully Paid" or "Current" while bad loans have the loan status as "Charged Off". 

Following are the queries used - 

a) To find out Good Loan Percentage across all loans -  

    SELECT
    (COUNT(CASE WHEN loan_status = 'Fully Paid' OR loan_status = 'Current' THEN id END) * 100)/ COUNT(id) AS Good_Loan_Percentage
    FROM bank_loan_data

![BA 20](https://github.com/user-attachments/assets/18d9d4ed-6bd7-437c-be31-3682c8182ff5)

b) To find out the Good Loan Applications across all loans - 

    SELECT COUNT(loan_status) AS Good_Loan_Applications
    FROM bank_loan_data
    WHERE loan_status = 'Fully Paid' OR loan_status = 'Current';

![BA 21](https://github.com/user-attachments/assets/b6fa7293-4c49-4384-b644-8477b0ffac29)

c) To find out Good Loan Funded Amount - 

    SELECT SUM(loan_amount) AS Good_Loan_Funded_Amount
    FROM bank_loan_data
    WHERE loan_status = 'Fully Paid' OR loan_status = 'Current'; 

![BA 22](https://github.com/user-attachments/assets/886d39c3-8aa4-4a84-8f33-86ac77448502)

d) To find out Good Loan Amount Received -

    SELECT SUM(total_payment) AS Good_Loan_Amount_Received
    FROM bank_loan_data
    WHERE loan_status = 'Fully Paid' OR loan_status = 'Current';


![BA 23](https://github.com/user-attachments/assets/99c638cc-6b6e-49b3-a2b0-93e6c2275653)

e) To find out Bad Loan Percentage across all loans -

    SELECT
    (COUNT(CASE WHEN loan_status = 'Charged Off' THEN id END) * 100)/ COUNT(id) AS Bad_Loan_Percentage
    FROM bank_loan_data    

![BA 24](https://github.com/user-attachments/assets/12983752-bdc1-4fb4-b8b5-4f3361e14ad2)

f) To find out the Bad Loan Applications across all loans -

    SELECT COUNT(loan_status) AS Bad_Loan_Applications
    FROM bank_loan_data
    WHERE loan_status = 'Charged Off'; 

![BA 25](https://github.com/user-attachments/assets/6d2760cc-2fcd-4ba4-8e95-9ba3b9cded61)

g) To find out Bad Loan Funded Amount - 

    SELECT SUM(loan_amount) AS Bad_Loan_Funded_Amount
    FROM bank_loan_data
    WHERE loan_status = 'Charged Off'; 

![BA 26](https://github.com/user-attachments/assets/3b1116f2-46f6-4be0-a0a8-c5c0983da77f)

h) To find out Bad Loan Amount Received -

    SELECT SUM(total_payment) AS Bad_Loan_Amount_Received
    FROM bank_loan_data
    WHERE loan_status = 'Charged Off'; 

![BA 27](https://github.com/user-attachments/assets/e0da3eac-1cf4-46c5-9c4f-8b09cc03c419)

- Step 5: Analysis w.r.t. Loan Status

a) Now, we will analyse the loan applications w.r.t. the Loan Status - 

    SELECT
    COUNT(id) AS Total_Loan_Applications,
    SUM(total_payment) AS Total_Amount_Received,
    SUM(loan_amount) AS Total_Funded_Amount,
    AVG(int_rate) AS Interest_Rate,
    AVG(dti * 100) AS DTI
    FROM bank_loan_data
    GROUP BY loan_status;

![BA 28](https://github.com/user-attachments/assets/b4e712dc-5be7-43b7-b8de-e1de9487ff1c)

b) To carry out the MTD Loan status analysis(for December) - 

    SELECT 
	loan_status, 
	SUM(total_payment) AS MTD_Total_Amount_Received, 
	SUM(loan_amount) AS MTD_Total_Funded_Amount 
    FROM bank_loan_data
    WHERE MONTH(issue_date) = 12 
    GROUP BY loan_status; 


![BA 29](https://github.com/user-attachments/assets/5d5e2976-62e8-4075-863b-05c87047dce7)

- Step 6: Analyse Total Loan Applications, Funded Amount, Amount Received w.r.t. following parameters - 

a) Analysis w.r.t. Month - 

    SELECT
    MONTH(issue_date) AS Month_Number,
    DATENAME(MONTH, issue_date) AS Month_Name,
    COUNT(id) AS Total_Loan_Applications,
    SUM(loan_amount) AS Total_Funded_Amount,
    SUM(total_payment) AS Total_Amount_Received
    FROM bank_loan_data
    GROUP BY MONTH(issue_date), DATENAME(MONTH, issue_date)
    ORDER BY MONTH(issue_date), DATENAME(MONTH, issue_date);


![BA 30](https://github.com/user-attachments/assets/e1cbe865-cc26-496e-8e75-acb49c9d2e05)

b) Analysis w.r.t. Region(State) - 

    SELECT
    address_state,
    COUNT(id) AS Total_Loan_Applications,
    SUM(loan_amount) AS Total_Funded_Amount,
    SUM(total_payment) AS Total_Amount_Received
    FROM bank_loan_data
    GROUP BY address_state
    ORDER BY address_state;


![BA 31](https://github.com/user-attachments/assets/02372798-efd5-486e-b051-3f168e2fab26)

c) Analysis w.r.t. Loan term - 

    SELECT
    term AS Term,
    COUNT(id) AS Total_Loan_Applications,
    SUM(loan_amount) AS Total_Funded_Amount,
    SUM(total_payment) AS Total_Amount_Received
    FROM bank_loan_data
    GROUP BY Term
    ORDER BY Term;


![BA 32](https://github.com/user-attachments/assets/937baeb9-41e3-49c3-a364-3f815c7633ec)

d) Analysis w.r.t Employment Lenghts - 

    SELECT
    emp_length AS Employment_Length,
    COUNT(id) AS Total_Loan_Applications,
    SUM(loan_amount) AS Total_Funded_Amount,
    SUM(total_payment) AS Total_Amount_Received
    FROM bank_loan_data
    GROUP BY emp_length
    ORDER BY emp_length;


![BA 33](https://github.com/user-attachments/assets/abc68bb0-dc0d-43e8-988a-9bb208ca2196)

e) Analysis w.r.t. Loan purpose - 

    SELECT
    purpose AS Purpose,
    COUNT(id) AS Total_Loan_Applications,
    SUM(loan_amount) AS Total_Funded_Amount,
    SUM(total_payment) AS Total_Amount_Received
    FROM bank_loan_data
    GROUP BY purpose
    ORDER BY purpose;

![BA 34](https://github.com/user-attachments/assets/cdb7e82d-77d5-46bb-a9c9-2f8f81a64889)

f) Analysis w.r.t. Home Ownership - 

    SELECT
    home_ownership AS Home_Ownership,
    COUNT(id) AS Total_Loan_Applications,
    SUM(loan_amount) AS Total_Funded_Amount,
    SUM(total_payment) AS Total_Amount_Received
    FROM bank_loan_data
    GROUP BY home_ownership
    ORDER BY home_ownership;

![BA 35](https://github.com/user-attachments/assets/b6d2bf16-0374-434d-b1f7-543c5b86c79c)

### Part 2 - Data Visualization Through Power-BI
- Step 1: Open Power BI Desktop and select Import Data from SQL Server in GET DATA tab. Select the Load option and the entire data will be loaded in the Power BI. In the Table view option, analyse the number of rows loaded in the Power BI to cross check whether entire data is imported.

- Step 2: Open Power query editor & in view tab under Data preview section, check "column distribution", "column quality" & "column profile" options. check whether entire table is populated correctly and the data types are matched accordingly to the column heads.

- Step 3: Now, to calculate the Time-Intelligence Functions, we need to add a Date Table. Following DAX expression was used for calculating the Date Table:

      Date Table = CALENDAR(MIN(bank_loan_data[issue_date]), MAX(bank_loan_data[issue_date]))

Now, we will extract the month name and month number from the corresponding dates by using the forllowing DAX: 

     Month Name = FORMAT('Date Table'[Date], "mmmm")
     Month Number = MONTH('Date Table'[Date])

- Step 4: Now, in the Model View of the Power BI Desktop, create a relationship between newly built 'Date Table' remember and the 'bank_loan_data' via ONE-TO-MANY Cardinality Relationship on "issue_date" and set the cross-filter direction as SINGLE.

- Step 5: In the Report View of the Power BI Desktop, select the background theme. You can either select an image or in this case use a background color by clicking on the Format Page option in the Visualization Tab. Note: reduce the transparency to 0%. For changing the Canvas size, select Canvas Settings and choose the desired option from the drop down menu.

- Step 6: Now, select the required Text Box for the Title of Dashboard by selecting the Shape option from Insert Tab. Format the background color and shape accordingly.

- Step 7: For calculating the Total Applications, a new measure  is added using the below DAX function:

      Total Loan Applications = COUNT(bank_loan_data[id])

For calculating, MTD Loan Applications, a new measure is added using following: 

    MTD Loan Applications = CALCULATE(TOTALMTD([Total Loan Applications], 'Date Table'[Date])) 

Now, for inserting MoM Change in Loan Applications, we will first calculate PMTD Loan Applications. For this we will use the formulas as below: 

    PMTD Loan Applications = CALCULATE([Total Loan Applications], DATESMTD(DATEADD('Date Table'[Date],-1,MONTH)))

    MoM Loan Applications = ([MTD Loan Applications] - [PMTD Loan Applications])/[PMTD Loan Applications]

- Step 8: Now, we have the values needed to insert into the 1st KPI visual. For this we will add a Card visual, insert shapes and text boxes followed by formatting the visual in accordance with background theme. The KPI visual will look as follows - 

![BA 36](https://github.com/user-attachments/assets/30b6990b-7abd-4ccf-ba8d-f428fc209a60)

- Step 9: For adding another KPIs - namely Total Funded Amount, Total Amount Recived, Average Interest Rate, Average DTI(Debt-to-Income Ratio) with their MTD and Mom change, we will follow the steps 7 & 8 respectively. The end result of this step will visually look like below - 

![BA 37](https://github.com/user-attachments/assets/584f3746-63bc-45dc-84b4-3f25363c0f7e)

- Step 10: Movingforward, the problem statement requires us to analyse the Good Loans vs Bad Loans in terms of number of applications, application percentage, funded amount and amount received. For this, we will use certain DAX functions as below: 

      Good Loan Applications = (CALCULATE([Total Loan Applications], bank_loan_data[Good vs Bad Loan] = "Good Loan"))
      Good Loan Percentage = (CALCULATE([Total Loan Applications], bank_loan_data[Good vs Bad Loan] = "Good Loan"))/ [Total Loan Applications]
      Good Loan Funded Amount = (CALCULATE([Total Funded Amount], bank_loan_data[Good vs Bad Loan] = "Good Loan"))
      Good Loan Amount Received = (CALCULATE([Total Amount Received], bank_loan_data[Good vs Bad Loan] = "Good Loan"))

Similarly, we will use the same DAX calculations for Bad Loan Analysis: 

      Bad Loan Applications = (CALCULATE([Total Loan Applications], bank_loan_data[Good vs Bad Loan] = "Bad Loan"))
      Bad Loan Percentage = (CALCULATE([Total Loan Applications], bank_loan_data[Good vs Bad Loan] = "Bad Loan"))/ [Total Loan Applications]
      Bad Loan Funded Amount = (CALCULATE([Total Funded Amount], bank_loan_data[Good vs Bad Loan] = "Bad Loan"))
      Bad Loan Amount Received = (CALCULATE([Total Amount Received], bank_loan_data[Good vs Bad Loan] = "Bad Loan"))

We will now use Donut chart and a Card(new) visual with proper formatting to display the above calculated values. The chart will look like as follows - 

![BA 38](https://github.com/user-attachments/assets/b545e9a1-efd8-407d-a4fc-05dbbc5793b7)

- Step 11: In the analysis w.r.t. Loan status, we will use a Table chart to visualise the Loan status w.r.t. Loan applications, funded amount and amount received, MTD funded amount, MTD amount received, average interest rate, average DTI. After data and visual formatting, the chart will look like -

![BA 39](https://github.com/user-attachments/assets/c2f82d55-b7f1-44b5-aeae-9b04d228ba79)

- Step 12: The last step in the summary part of the dashboard is to insert slicers namely the State, Grade, Purpose to directly display the values while interacting with the dashboard. 

- Step 13: In the Overview part of the dashboard, we will keep the KPIs from the summary part, along with slicers and design the dashboard w.r.t the following parameters - 

a) Total Applications/Total Funded Amount/Total Amount Received by Month.

b) Total Applications/Total Funded Amount/Total Amount Received by State.

c) Total Applications/Total Funded Amount/Total Amount Received by Term.

d) Total Applications/Total Funded Amount/Total Amount Received by Employee Length.

e) Total Applications/Total Funded Amount/Total Amount Received by Purpose.

f) Total Applications/Total Funded Amount/Total Amount Received by Home Ownership. 

For this we will now create an Area chart for Total Loan Applications with Month Field. Now, in order to incorporate the other 3 charts in the same chart(rather than creating 3 multiple charts for the same field) by inserting field parameter in the Modelling Tab, then selecting all the 3 measures we want to visualise). Now, in the formatting part of the slicer, choose "dropdown" option in slicer setting, then we will select "Single" option in selection setting. The field parameter will be visulaised in the form of a slicer named "Select Measure" from where we change between any of the 3 fields. Format the Area chart by adding markers and data labels.

![BA 40](https://github.com/user-attachments/assets/2436d76e-81bb-404e-875d-d74f315c6679)

- Step 14: We will now create a Shape Map to highlight the Number of Total Applicatins. funded amount, or amount received w.r.t. the State in the United States. For this, insert the Measure field created in Field Parameter in Color Saturation part and address state in the Location field. Format the chart colors, size and text. 


![BA 41](https://github.com/user-attachments/assets/08cf9968-3c7e-4b97-8c64-a19bee782ea3)

- Step 15: For the analysis of the 3 parameters w.r.t. Term, insert a donut chart, and format the chart in accordance with the theme and requirement. 


![BA 42](https://github.com/user-attachments/assets/027d48d9-e581-4518-91ed-f9b6e3e433fd)

- Step 16: For analysis w.r.t. Employee Length and Purpose, we will use two bar charts respectively. 

![BA 43](https://github.com/user-attachments/assets/31f8d01e-ae08-4d5a-96d0-e0ee6a5323a9)

- Step 17: A Tree Map is visualised to analyse the 3 parameters with Home Ownership. Format the chart with background themes. 

![BA 44](https://github.com/user-attachments/assets/4f36cefa-f680-4ec1-b225-2e50dc6d9a7e)

- Step 18: In the 3rd part, we will visulise the "Details" part of the Bank Loan Project. The various fields like id, grade, homeownership details, funded amount, interest rate, installment, and amount recieved are visualised in the form of a Table. The various slicers provided helps to filter the details w.r.t. specific filters.

- Step 19: The last step included inserting buttons for all the 3 pages on the dashboard aiding in smooth hovering from 1 page to another during presentation. 

- Step 20: The report was then published to Power BI Service.

## Report Snapshot(Power BI DESKTOP):
![BA Final 1](https://github.com/user-attachments/assets/8cb4c2e9-2644-496e-b46c-23d763bf9dec)
![BA Final 2](https://github.com/user-attachments/assets/eef46c1e-9779-4c8a-b855-de97f1f9cfc8)
![BA Final 3](https://github.com/user-attachments/assets/993b6c42-e4a6-44e2-8c23-f83c5f3b4f0b)

## Insights: 

### Insights regarding KPIs: 

    a) The Total Loan Applications had a month-on-month increase of 6.9%, reaching to 38.6K applications in the time period, with maximum being in December with 4314 applications.

    b) The month-on-month Average Interest Rate also increased by 3.5%, rising to 12.36% in December. One of the reasons for increase rates of Interest can be due to rising inflation, to curb down spending.  

    c) Overall, the Total Funded Amount was less than Total Amount recived during the time period, which means the bank had extra reserves of money making it profitable. 

    Majority of the loan applications were for Short term of 36 months(~73%) as compared to long term loan applications of 60 months(~27%). This could mean that people prefer financial freedom by paying of their loans as early as possible, although they might had to shell off added amount of EMIs, with lesser tenure. 

### Insights regarding Good Loan vs Bad Loan:

    a) The Good Loan percentage in terms of number of applications was around 86.3%, while bad loan accounted for 13.7% applications.

    b) The overall Bad Loan percentage of 13.7% is still higher as it reflects that the chances of bank losing its money is are higher, whuch in turn can affect the profitability of the banks.
    Thus, banks need to focus in future for lowering this percentage. 

### Insights regarding State:
    The state of California received maximum loan applications(6894) along with maximum Total Funded Amount of $78.5M and Total Amount Received of $83.9M. 

### Insights regarding Employment Purpose:
    With 18214 applications, Debt Consolidation was the major reason for availing a loan among the available reasons.

### Insights regarding Home Ownership:
    Home Ownership status of "RENT" and "MORTGAGE" had maximum number of application for loans across the said time period with ~18K and ~17K applications respectively. 

### Insights regarding Employment:
    a) People with Employment length of 10+ years had maximum number of Loan Applications of ~8.9K.  
    b) People with Employment grade of "A" nad "B" received maximum number of applications(~55%), which means that bank is entertaining people with stable higher income that in turn is safer for bank in future. 
