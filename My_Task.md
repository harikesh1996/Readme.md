 # What work did you get
### Task 
 1. Creating a spreadsheet of self evaluation & other evaluation  
 2. Put on the formula of average in the average column. 
 3. If average column Value is equal to or less than then row column should be appear red.

# How to write formulae
              =SUM(COUNTIF(C4:J4,"Excellent")*5,COUNTIF(C4:J4,"Very Good")*4,COUNTIF(C4:J4,"Good")*3,
              COUNTIF(C4:J4,"Satisfatory")*3,COUNTIF(C4:J4,"Fair")*2,
              COUNTIF(C4:J4,"Poor")*0,)/COUNTA(C4:J4)

  # How does the formulae work? 
  
           1.Get the average of a rating of numbers. 
           2.Syntax = SUM(COUNTIF(range,"string1")*number1,COUNTIF(range,"string2")*number2)/COUNTA(range)
           3. Average, which is the arithmetic mean, and is calculated & countif means range selected by adding a group of numbers and then dividing by the count   of range numbers.
            
            
# Objective of Exercises      
:black_circle: improve of the average formula and also Improved to how to set average range of formula.
 

# How it was implemented
:black_circle: Is in Average range of stating that some formula of process is implemented and completely done with. While the average formula It has been implemented is the sum and countif in the average column. 

# Explanation of Implementation
    1. This article describes the formula syntax and usage of the AVERAGE function in evaluation sheet. 
    2. The average (arithmetic mean) of the arguments. For example, if the range A1:A20 contains numbers, the formula =AVERAGE(A1:A20) returns the average of    those numbers.

    AVERAGE(number1, [number2], ...)

    The AVERAGE function syntax has the following arguments:
    Number1,    Required. The first number, cell reference, or range for which you want the average.
    Number2, ...    Optional. Additional numbers, string or ranges for which you want the average, up to a maximum of 255.
# Test Results
 

# Conclusion

I have complete in my task.


