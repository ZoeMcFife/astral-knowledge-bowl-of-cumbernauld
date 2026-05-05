#relational_databases 

![[Exam_RDB_SS26_Reference.pdf]]


# Question 1

## 1

- Foreign Key Contraint
- Yes; Otherwise you’d end up with orders from non existent customers which is not desirable. 
- You insert wrong data into the table. 

## 2

- If only `student_id` is the pk, then one student can only enroll in one course, since you cannot have duplicate pk entries.
- with no changes to the table, `student_id` and `course_id` should be the pk as this allows a student to be enrolled into multiple courses. Though, this wouldn’t able to handle if a student has to repeat a course, so adding something like `course_year` to differentiate different years of students doing the course.
- This is correct, since we want students to enroll in multiple course

## 3

- Dirty Read
- Isolation; Read Committed
- Reading Data that is still being manipulated or is unfinished is obviously bad. In the healthcare case here, this would result in administaring a wrong dose. In banking it would lead to inconsistent money ammounts and transfers, which i dont have to explain why that is really bad.

# Question 2

## 1

- student enrollment relationship is 1:1 and not N:M
- Enrollment is missing Grade
- Proffessor Course Relationship is missing cardinality; should be 1:N; no relationship defined?
- Professor Enrollment relationship not defined in the business rules; should it be here? 
- missing relationship uh diamond shapes; this is chen notation, which requires it. 

## 2

- N:M 

## 3

1:N → relationship ; 

# Question 3

## 1

- Doesn’t violate the first normal form 
- violates 2nd normal form
	- Booktitle doesnt depent on the primary key
	- autothor name doesnt depend on pk
	- member name doesnt depend on pk
	- membder email doesnt depend on pk
- violates 3rd normal form
	- transitive dependencies → booktitle, author on book id
	- member name and member email on member id

## 2

- You cannot add new books or members without adding a loan? idk?

- Inconsistences → you could have the same book have different titles of authors or have users with the same id but different names or emails

- you lose title, author, member and member email fi you delete a loan

## 3

```
load(LoanId - PK, BookId, MemberId, LoanDate)

book(bookId - pk, authorId, booktitle)

author(authorid, authorname)

member(memberid - pk, membername, memberemail)

```

# question 4

## Part A

- Atomicity
- Partially completed transaction → fails, then does a rollback
- Isolation
-  dirty read

## Part B

### a

I’d use a after insert / update trigger that calculated the new grade; maybe have it call a stored function that does the calculation? but that’s debatable; why a trigger? because we must react on an update… and only a trigger does that

### b

stored procedure, since we’re querying for something based off an input


trigger triggers when stuff changes; procedured get called manually with call


