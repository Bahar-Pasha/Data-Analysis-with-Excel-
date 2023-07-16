# Data-Analysis-with-Excel-
One of the widely used tools for data analysis is Excel. I have been asked by a school to help automate their accounts processing, and I am going to describe the analysis process step by step. The Excel file has three sheets, two of which include raw data that will be used for the third sheet. The names of the sheets are: "BPAY Bank File," "SYS DATA," and "Upload" (the third sheet). This is where all of our calculations will be done.


<img width="761" alt="042FD8ED-026F-4883-9E6B-D0E60112A7D4" src="https://user-images.githubusercontent.com/127425854/230914052-f374a878-bdf3-4bc4-846a-8b539ce5a0c6.png">


<img width="467" alt="5AC0246D-3B7A-48D9-9CDC-EC20C8E50079" src="https://user-images.githubusercontent.com/127425854/230914450-f3ea80d5-2e89-4543-9148-ea07a0eaf8bc.png">



## Join Texts 

The first phase of this project is joining texts from the "BPAY Bank" sheet (columns: MERCHANT, SETTLEMENT DATE, MERCHANT REFERENCE) to create a Transaction Reference with no space between them. We can do this using four different methods or formulas: 1) CONCATENATE, 2) CONCAT, 3) TEXTJOIN, and 4) &.

I used the following formula:

**=CONCAT(MERCHANT,SETTLEMENT DATE,MERCHANT REFERENCE)**

<img width="620" alt="844D4BDF-E94C-45D4-AE5B-D46C5DC121AB" src="https://user-images.githubusercontent.com/127425854/230916450-3e8c587c-420e-4b70-baef-808ecb196d24.png">


## Customer Ref
For the Customer Ref, which is column C in the "Upload" sheet, we should create a calculation to extract the last 5 characters from the BPAY Reference (from the "BPAY Bank" sheet) and convert it to a numeric value. There was a space at the end of the code in the BPAY Reference, so I used the TRIM function to get rid of any space before and after the code. Then, I extracted the last 5 characters using the RIGHT function and converted them into a number since the format was text.

**=VALUE(RIGHT(TRIM(BPAY Reference),5))**

Alternatively, we could use:

**=VALUE(RIGHT(SUBSTITUTE(PAY Reference," ",""),5))**



<img width="474" alt="21949222-64FD-4FB8-9348-2B23B495A6B0" src="https://user-images.githubusercontent.com/127425854/230916695-7157639a-52d7-4bae-99f8-455ee1fd5c1a.png">



## Paid Month
IIn the "BPAY" sheet, the dates are in the format YYYYMMDD, which makes them difficult to perform calculations with. In column D, a calculation was created to extract the two-digit month from the PAYMENT DATE in the "BPAY" sheet. This calculation was done using the MID function.

**=MID(PAYMENT DATE,5,2)**


<img width="377" alt="B0B33A3E-ED38-4F5D-831C-DEB0A1614B8B" src="https://user-images.githubusercontent.com/127425854/230916940-b4d2b7c0-de98-4ff9-8fe6-b42a86cb12ed.png">

## Valid Format Date 
In column E, we need to use a calculation to convert the paid date in the "BPAY" sheet to a valid Excel date format. We need to separate and rejoin the separated parts of the date using an appropriate date function. Using the **LEFT, MID**, and**RIGHT functions**, I separately calculated the Year, Month, and Day, and then put them all in the DATE function to create a formatted date. 

**=DATE(LEFT(PAYMENT DATE,4),MID(PAYMENT DATE,5,2),RIGHT(PAYMENT DATE!G18,2))**


<img width="596" alt="30575F77-0151-4101-BA68-FE04BC5B4F63" src="https://user-images.githubusercontent.com/127425854/230941123-0531ca29-bada-47a0-9f4c-61e8300fc06f.png">


## Payment Amount 

In column F, we need to extract the payment amount from the "BPAY" sheet. However, I noticed that the format is text because of the "AU" at the beginning. Therefore, we should remove "AU" using the **SUBSTITUTE** function and then convert it to a numeric value.

**=VALUE(SUBSTITUTE(PAYMENT AMOUNT,"AU$",""))**


<img width="582" alt="AEEC0F8F-7E40-42B1-BADB-81604B6FD5F8" src="https://user-images.githubusercontent.com/127425854/230955850-a4bc787a-156f-432d-88f9-e5ef5e618d1c.png">

## Balance 

To calculate the balance for each student, we need to go back to the "SYS DATA" sheet and sum the amount based on the Customer Ref column. To do this, we earlier used the RIGHT function to extract the 5 digits of the BPAY Reference column from the "BPAY Bank" Sheet, which is equal to the Customer Ref in the "SYS DATA" sheet. Then, using the SUMIFS function, we sum the Amount based on the Customer Ref.

**=SUMIFS(Amount,Cust_Ref,C2)**

-"Amount" and "Cust_Ref" were already named using **named ranges**.


<img width="660" alt="2DE5334A-32F7-4535-A249-91C6238C0388" src="https://user-images.githubusercontent.com/127425854/230956177-17b1f92f-4161-4d12-80ea-79fcd01e670f.png">


<img width="582" alt="9D644744-4D5C-44C7-948E-2CB0052DEEC1" src="https://user-images.githubusercontent.com/127425854/230956378-b5429907-5bc2-427f-9c2b-b31dd6b8f1b1.png">

## Invoice Date 

In column H I used the Customer Reference to look up the invoice date from the data in the SYS DATA sheet.

**=VLOOKUP(Cust_Ref,SysData,2,0)**


<img width="582" alt="0D94101E-5CAC-41AF-9868-7EE52EAA5550" src="https://user-images.githubusercontent.com/127425854/230957062-e5846a10-7acf-4684-8af1-5afb8fde93e0.png">


## Due Date 

The due date for invoices is**21 working days** after the invoices are issued. I used the WORKDAY function to calculate this date. 

**=WORKDAY(Invoice Date ,21)**

Then, in the "Days to Pay" column, I calculated the difference between the paid date and the invoice date to estimate how many days it takes for an invoice to be paid. I used the DAYS function to calculate the difference.

**=DAYS(Paid Date,Invoice Date)**

<img width="641" alt="E52072E8-AA36-409C-A36D-B15B7664B44C" src="https://user-images.githubusercontent.com/127425854/230962424-afed8441-093b-4c72-9270-d3a9935a6e3d.png">


## Paied before Due Date 

The due date for invoices is 21 working days after the invoices are issued. In this stage, we would like to know who paid earlier than the due date. We need this data to offer discounts to people who paid early. Therefore, using the IF function, we returned the required data by showing "Y" if paid early and "" for those who paid late. 

**=IF(Paied Date >Due Date,"Y","")**

Next, I am going to calculate how many days people pay early except working days. I performed calculation with **NETWORKDAYS** and **IF** functions, also people who pay late 0 day should be returned:

**=IF([@[Paid before Due Date]]="Y",NETWORKDAYS([@[Paid Date]],[@[Due Date]]),0)**


<img width="756" alt="432E4907-C223-45F4-94DC-4D3AD89B8C08" src="https://user-images.githubusercontent.com/127425854/231073123-4c32768d-6d79-47a3-9b18-5a4018cffab4.png">

## Discount Awarded
Customers who pay 5 working days before the due date are eligible for a discount. In column M, we calculate 5 working days before the due date (excluding Saturdays and Sundays).

**=WORKDAY([@[Due Date]],-5)**

Discounts are calculated as a percentage of the balance owed, the rates vary depending on how large the balance is, based on the following table:

<img width="211" alt="2748123E-44A1-4CD4-9ED4-6A2A34427F1A" src="https://user-images.githubusercontent.com/127425854/231082033-8d32ab8a-22f8-4388-801c-fe6d055ae805.png">


According to this table, we calculate how much discount will be offered to each person based on their balance owed. Using the VLOOKUP function, I looked for a range that was closest to the balance owed, then multiplied the discount percentage by the balance. 

**=VLOOKUP([@Balance],$S$2:$T$5,2,1)*[@Balance]**

<img width="721" alt="83280DAB-0DB1-485F-9D8F-E8C5415ADDBA" src="https://user-images.githubusercontent.com/127425854/231082150-55bf852f-3dcd-492d-a81a-54c67c2c8576.png">


## Discount OFFERED 
To be eligible for a discount, customers must have paid before the Discount Due Date and must have paid at least the full balance owing less the discount amount. In other words, if the balance was $300, and the discount was $15, if they paid early and paid $285 or more, they get the discount.

**=IF(AND([@[Paid before Due Date]]="Y",OR([@[Payment Amount]]=[@Balance],[@[Payment Amount]]>=[@Balance]-[@[Discount Offered]])),[@[Discount Offered]],0)**

<img width="985" alt="35731FC8-FD74-4A3F-A6EC-174960276994" src="https://user-images.githubusercontent.com/127425854/231100384-9e3d490d-b527-44c2-8542-df9db7060c4f.png">

## Discount Awarded 
Where there is **more than one student** enrolled, customers are given a **5% sibling discount**. We calculate this by looking up the enrollments for the customer in the "SYS DATA" sheet and only calculating the Sibling Discount if the number of enrollments is two or more. I used the XLOOKUP function to look up the enrollments based on the Customer Ref. Then, using the IF function, I calculated the rest.


**=IF(OR(XLOOKUP([@[Customer Ref]],Cust_Ref,Enrolments)>=2),Payment Amount/(1-5%)-Payment Amount,0)**

<img width="922" alt="F6F71DBC-4B1E-4D56-A583-06E01B825536" src="https://user-images.githubusercontent.com/127425854/231100499-0544df50-11b4-4619-994f-b07eb1042d42.png">

## NEW Balance 
In the final stage, I calculated the New Balance after all of the above calculations.

**=ROUNDDOWN(Bakance-Discount Awarded-Payment Amount,2)**


## Reslut 

Using a **Pivot Table**, the following results were produced:

People have paid most of their debts in October compared to September. 53% of the debts have been paid in December, which has the largest amount. 60% of discount offers have been offered to people who paid in December, but 94% of awarded discounts have been dedicated to January. This means that in December, 60% of people had early payments but did not pay at least the full balance owing less the discount amount.

<img width="811" alt="E91D698E-2E0F-49D9-B7B6-D2B5A19778BB" src="https://user-images.githubusercontent.com/127425854/231208660-f1c72054-49bf-4629-93ba-508f38d48891.png">
