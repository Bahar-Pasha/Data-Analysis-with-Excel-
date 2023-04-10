# Data-Analysis-with-Excel-
One of the widley-used tools for data analysis is excel.I have been asked by a school to help automate their accounts processing and am going to describe the analysis process step by step. The excel file has 3 sheets which two of them include data which are going to be used for the thired sheet, this is a sheet where all of our calculations will be done. Names of sheets are :BPAY Bank File, SYS DATA, Upload(third sheet): 

## Join Texts 
 1- The first phase of this porjoect is joing texts from BPAY Bank sheet (column:MERCHANT,SETTLEMENT DATE,MERCHANT REFERENCE)to create Transaction Reference with no space between them. We can do this by 4 different ways or different formula: 1- CONCATENATE 2- CONCAT 3- TEXTJOIN 4-&
Idid:
=CONCAT(MERCHANT,SETTLEMENT DATE,MERCHANT REFERENCE)


## Customer Ref
2- For Customer Ref which is column C in Upload sheet we should create a calculation to extract the last 5 characters from the BPAY Reference (from BPAY Bank sheet) and convert it to a numeric value. There was a space at the end of the code in BPAY Reference so I used TRIM to get ride of any space before and after the code, then I got the 5 last characters by RIGHT and turned it into number since the format was text. 

=VALUE(RIGHT(TRIM(BPAY Reference),5))
Instead of TRIM we could use: 
=VALUE(RIGHT(SUBSTITUTE(PAY Reference," ",""),5))
