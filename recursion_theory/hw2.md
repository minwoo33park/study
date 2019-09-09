###problem1

A valid program should start with `1` and end with `#`.  
Also, any consecutive `#`'s should not be longer than 5.

The label `valid` will clear everything and output 1 for R1 then halt.  
The label `invalid` will clear everything and output # for R1 then halt.  
The label `top` reads the first letter. Empty program is valid and if it starts with # it is invalid.  
The label `pop_one` assumes we have removed the leading 1 and keeps removing 1 until R1 is empty or we get #. If R1 is empty the program is invalid because it is ending with 1. If we get # we go to the label `consecutive_check`.
The label `consecutive_check` checks if the consecutive `#`'s are longer than 5. So we use 5 `case 1` instructions to check the length. If 1 comes up before 6 consecutive #'s, we go to the label `pop_one` again. Also, if we find out R1 is empty, we conclude the program is valid because it is ending with #. At the last case instruction of `consecutive_check`, if # comes up, we conclude the program is invalid because we found `######` in the program.

```text
label top
case 1
goto valid
goto pop_one
goto invalid

label pop_one
case 1
goto invalid
goto pop_one
goto consecutive_check

label valid
label valid_clear
case 1
jump 3
goto valid_clear
goto valid_clear
push1 1
goto end

label invalid
label invalid_clear
case 1
jump 3
goto invalid_clear
goto invalid_clear
push# 1
goto end

label consecutive_check
case 1
goto valid
goto pop_one
case 1
goto valid
goto pop_one
case 1
goto valid
goto pop_one
case 1
goto valid
goto pop_one
case 1
goto valid
goto pop_one
goto invalid

label done
goto end
label end
```

This 1# sanity code will generate the following 1# program:

```text
1##### ;Line 1 - Case on R1 (Label top)
1111111### ;Line 2 - Jump forward 7 (Goto valid)
11### ;Line 3 - Jump forward 2 (Goto pop_one)
11111111111### ;Line 4 - Jump forward 11 (Goto invalid)
1##### ;Line 5 - Case on R1 (Label pop_one)
111111111### ;Line 6 - Jump forward 9 (Goto invalid)
11#### ;Line 7 - Jump backwards 2 (Goto pop_one)
1111111111111### ;Line 8 - Jump forward 13 (Goto consecutive_check)
1##### ;Line 9 - Case on R1 (Label valid) (Label valid_clear)
111### ;Line 10 - Jump forward 3 lines
11#### ;Line 11 - Jump backwards 2 (Goto valid_clear)
111#### ;Line 12 - Jump backwards 3 (Goto valid_clear)
1# ;Line 13 - Append 1 to R1
111111111111111111111111### ;Line 14 - Jump forward 24 (Goto end)
1##### ;Line 15 - Case on R1 (Label invalid) (Label invalid_clear)
111### ;Line 16 - Jump forward 3 lines
11#### ;Line 17 - Jump backwards 2 (Goto invalid_clear)
111#### ;Line 18 - Jump backwards 3 (Goto invalid_clear)
1## ;Line 19 - Append # to R1
111111111111111111### ;Line 20 - Jump forward 18 (Goto end)
1##### ;Line 21 - Case on R1 (Label consecutive_check)
1111111111111#### ;Line 22 - Jump backwards 13 (Goto valid)
111111111111111111#### ;Line 23 - Jump backwards 18 (Goto pop_one)
1##### ;Line 24 - Case on R1
1111111111111111#### ;Line 25 - Jump backwards 16 (Goto valid)
111111111111111111111#### ;Line 26 - Jump backwards 21 (Goto pop_one)
1##### ;Line 27 - Case on R1
1111111111111111111#### ;Line 28 - Jump backwards 19 (Goto valid)
111111111111111111111111#### ;Line 29 - Jump backwards 24 (Goto pop_one)
1##### ;Line 30 - Case on R1
1111111111111111111111#### ;Line 31 - Jump backwards 22 (Goto valid)
111111111111111111111111111#### ;Line 32 - Jump backwards 27 (Goto pop_one)
1##### ;Line 33 - Case on R1
1111111111111111111111111#### ;Line 34 - Jump backwards 25 (Goto valid)
111111111111111111111111111111#### ;Line 35 - Jump backwards 30 (Goto pop_one)
111111111111111111111#### ;Line 36 - Jump backwards 21 (Goto invalid)
1### ;Line 37 - Jump forward 1 (Goto end) (Label done)
```  

###problem2

Instead of adding # to R4, we add 1 because for 0th power we get 1.  
For step 7, we use `φbump(multiply,3)`.  
For steps 5 and 9, change jump line numbers according to modified program.
(I had to add 120 one's for each steps)
```text
Inputs:
R1: 11
R2: 1#1
Output:
R1: 11##1111
```

The program for function $f(x,y) = x^y$ is as follows.

```text
; R1 has original n
; R2 has original m
; R3 has a counter, starting with 0 and ending with m
; R4 has a running total, so it starts with 1 and ends with what we want

;1-1. add # to R3

111##

;1-2. add 1 to R4 because for 0th power we get 1
1111# 

;2. copy R2 to R5 using R6

11##### 11111111### 1111### 11111## 111111## 11111#### 11111# 111111# 11111111#### 111111##### 111111### 111### 11## 1111#### 11# 111111####

;3. copy R3 to R6 using R7

111##### 11111111### 1111### 111111## 1111111## 11111#### 111111# 1111111# 11111111#### 1111111##### 111111### 111### 111## 1111#### 111# 111111####

;4. compare R5 and 56. This is φ_bump(compare,4).  The program 'bump' may be found in the web text.

11111##### 111111### 111111111### 111111##### 11111111111### 1111111111### 111111#### 111111##### 111111111111111### 111111### 11111### 111111##### 111### 1111111111111#### 1### 11111##### 111### 11#### 111#### 111111##### 1111### 11#### 111#### 11111#

;5 Run a comparison program of R5 and R6.  This is listed in the web text as 'compare'.

;if R5 is empty (indicating that R5 and R6 were not equal), go to step 6

; otherwise, R5 has 1 and we go to step 10 (the cleanup section) - change instuction count

11111##### 111### 1111111111 11111111111 11111111111 11111111111 11111111111 11111111111 11111111111 11111111111 1111111111 1111111111 1111111111 1111111111 1111111111 1111111111 1111111111 1111111111 1111111111 1111111111 1111111111 1111111111 11### 1#

;6. copy R1 to R5 using R6

1##### 11111111### 1111### 11111## 111111## 11111#### 11111# 111111# 11111111#### 111111##### 111111### 111### 1## 1111#### 1# 111111####

;7. multiply R4 to R5, leaving the answer in R4. This is φbump(multiply,3) which can be found in "program for multiplication"

111111##1111111##11111#####11111111###1111###11111111##111111111##11111####11111111#111111111#11111111####111111111#####111111###111###11111##1111####11111#111111####111111#####11111111###1111###111111111##1111111111##11111####111111111#1111111111#11111111####1111111111#####111111###111###111111##1111####111111#111111####11111111#####111111###111111111###111111111#####11111111111###1111111111###111111####111111111#####111111111111111###111111###11111###111111111#####111###1111111111111####1###11111111#####111###11####111####111111111#####1111###11####111####11111111#11111111#####111###11111111111111111111111111111111111111111111111111111111111111111111111111111111111111111###1111#1111#####11111111###1111###11111111##111111111##11111####11111111#111111111#11111111####111111111#####111111###111###1111##1111####1111#111111####1111111#####111###111111###111111111###11111111#####11111111111111111111111111111111111###11111111111111111111111111###111111111111111111111111111###11111111#####11111111111111111111111###1111111111111111111111111111###111111111111111111111###11111111#####111111111111111111111###111111111111111111###1111111111111111111###1111111#####111###111111###111111111###11111111#####11111111111###1111111111111111###111111111###11111111#####1111111111111###1111111111###11111111111###11111111#####111###11111111###1###111111111#111111111111111111111111111111111####111111111##11111111111111111111111111111111111####111111111#111111111111111111111####111111111##11111111111111111111111####1###111111111#####111111###111###1111111##1111####1111111#111111####111111#####1111111111111###1111111111###11111111#111111#####111111###111###11111111##1111####11111111#111111####1111###11111111##1111111111111####11111111#11111111#####111111###111###111111##1111####111111#111111####11111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111111####1111#####111###11####111####11111#####111###11####111####111111#####111###11####111####1111111#####111111###111###1111##1111####1111#111111####

;8. take the successor of R3, using R5. We modify the program succ from above.

111##### 1111111111 111### 1111111111### 11111# 111#####111111###111### 11111##1111#### 11111#111111#### 1111### 11111## 1111111111 111#### 11111# 11111#####111111###111### 111##1111#### 111#111111####

;9. go back to the main loop, step 2 - change instuction count

1111111111 11111111111 11111111111 11111111111 11111111111 11111111111 11111111111 11111111111 11111111111 11111111111 11111111111 11111111111 11111111111 1111111111 1111111111 1111111111 1111111111 1111111111 1111111111 1111111111 1111111111 1111111111 1111111111 1111111111 1111111111 1111####

;10. the "cleanup" stuff: clear R1, R2, and R3; also move R4 to R1

1##### 111### 11#### 111####
11##### 111### 11#### 111####
111##### 111### 11#### 111####
1111##### 111111### 111### 1## 1111#### 1# 111111####

```  

###problem3

Given a program p, if p is tidy, p itself is its tidy equivalent.  
if p is not tidy with n instructions, we add two more instructions at the end:  
```text
1###
1####
```text  
This makes an infinite loop.  
Now, add `11` in front of all the move forward instructions that used to make proper halt so that they will still result in proper halt.  
Next, change all the move forward and backward instructions that makes improper stop to go to the infinite loop at the end.  
This will make the program tidy.  
Since we just changed the program's behavior for improper stop to do an infinite loop, we didn't make any changes to the output of partial function where it is defined, thus getting an equivalent program q.  

  

###problem4

(a)
```text  
1##### ;Line 1 - Case on R1 (Label top)
111### ;Line 2 - Jump forward 3 (Goto c2)
11#### ;Line 3 - Jump backwards 2 (Goto top)
111#### ;Line 4 - Jump backwards 3 (Goto top)
11##### ;Line 5 - Case on R2 (Label c2)
111### ;Line 6 - Jump forward 3 (Goto end)
111111#### ;Line 7 - Jump backwards 6 (Goto top)
1111111#### ;Line 8 - Jump backwards 7 (Goto top)
```
We change the `Jump backwards` instruction at `Line 7` and `Line 8` to goto `top` so everytime we remove one letter from R2, we just check once more if R1 is empty. Thus, it is equivalent to the original program.  
  
(b) 