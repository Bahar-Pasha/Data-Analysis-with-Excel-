# Data-Analysis-with-Excel-
One of the widley-used tools for data analysis is excel.I have been asked by a school to help automate their accounts processing and am going to describe the analysis process step by step. The excel file has 3 sheets which two of them include data which are going to be used for the thired sheet, this is a sheet where all of our calculations will be done. Names of sheets are :BPAY Bank File, SYS DATA, Upload(third sheet): 


<img width="761" alt="042FD8ED-026F-4883-9E6B-D0E60112A7D4" src="https://user-images.githubusercontent.com/127425854/230914052-f374a878-bdf3-4bc4-846a-8b539ce5a0c6.png">


<img width="467" alt="5AC0246D-3B7A-48D9-9CDC-EC20C8E50079" src="https://user-images.githubusercontent.com/127425854/230914450-f3ea80d5-2e89-4543-9148-ea07a0eaf8bc.png">



## Join Texts 
 1- The first phase of this porjoect is joing texts from BPAY Bank sheet (column:MERCHANT,SETTLEMENT DATE,MERCHANT REFERENCE)to create Transaction Reference with no space between them. We can do this by 4 different ways or different formula: 1- CONCATENATE 2- CONCAT 3- TEXTJOIN 4-&
Idid:
=CONCAT(MERCHANT,SETTLEMENT DATE,MERCHANT REFERENCE)

<img width="620" alt="844D4BDF-E94C-45D4-AE5B-D46C5DC121AB" src="https://user-images.githubusercontent.com/127425854/230916450-3e8c587c-420e-4b70-baef-808ecb196d24.png">


## Customer Ref
2- For Customer Ref which is column C in Upload sheet we should create a calculation to extract the last 5 characters from the BPAY Reference (from BPAY Bank sheet) and convert it to a numeric value. There was a space at the end of the code in BPAY Reference so I used TRIM to get ride of any space before and after the code, then I got the 5 last characters by RIGHT and turned it into number since the format was text. 

=VALUE(RIGHT(TRIM(BPAY Reference),5))
Instead of TRIM we could use: 
=VALUE(RIGHT(SUBSTITUTE(PAY Reference," ",""),5))



<img width="474" alt="21949222-64FD-4FB8-9348-2B23B495A6B0" src="https://user-images.githubusercontent.com/127425854/230916695-7157639a-52d7-4bae-99f8-455ee1fd5c1a.png">



## Paid Month
In the BPAY sheet dates come through in the format YYYYMMDD, which makes them difficult to perform calculations with.In column D a calculation was created to extract the two digit month from PAYMENT DATE in the BPAY sheet. This calculation was done through MID. 
=MID(PAYMENT DATE,5,2)


<img width="377" alt="B0B33A3E-ED38-4F5D-831C-DEB0A1614B8B" src="https://user-images.githubusercontent.com/127425854/230916940-b4d2b7c0-de98-4ff9-8fe6-b42a86cb12ed.png">




