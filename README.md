# Python_database
-----------database-------------
create database test;
use test;
create table records(c_name varchar(30), college_name varchar(50) , round1_marks float , round2_marks float , round3_marks float ,  techmarks float , total float);
create table result(name varchar(30), result varchar(20));

----------Python-----------------------
import mysql.connector
mydb = mysql.connector.connect(host="localhost", user="root", password="password123", database="test")
cur1 = mydb.cursor()
cur2 = mydb.cursor()
cur3 = mydb.cursor()

choice = int(input("Press 1 to enter data , Press 2 to see data\n"))

if choice == 1:
    while True:
        name = input("Enter candidate name\n")
        college_name = input("Enter college name\n")
        r1 = float(input("Enter round1 marks out of 10\n"))
        r2 = float(input("Enter round2 marks out of 10\n"))
        r3 = float(input("Enter round3 marks out of 10\n"))
        tech = float(input("Enter technical marks out of 20\n"))
        total = r1+r2+r3+tech

        cur1.execute("insert into records values('{}','{}',{},{},{},{},{})".format(name, college_name, r1, r2, r3, tech, total))

        if total >= 35:
            result = "selected"
        else:
            result = "rejected"

        cur1.execute("insert into result values('{}','{}')".format(name,result))
        mydb.commit()


        choice2 = int(input("Do you want to enter details of more students Press 1 otherwise Press 0"))

        if choice2 == 0:
            break
elif choice == 2:
    print("Details of all candidates")
    print("NAME,COLLEGE,ROUND1,ROUND2,ROUND3,TECH,TOTAL")
    cur1.execute("select * from records")
    data = cur1.fetchall()
    for i in data:
        print(i)

    print("------------")
    print("\nRESULT STATUS\n")
    cur2.execute("select * from result")
    data2 = cur2.fetchall()
    for i in data2:
        print(i)


else:
    print("Wrong choice")


print("----------------")
print("\nRanking status\n")
print(" Name  Total Rank")
cur3.execute("select c_name , total , dense_rank() over (order by total DESC) as s_rank from records;")
data3 = cur3.fetchall()
for i in data3:
    print(i)
    
    
    
-----------------output-------------------
                                       1.   -------entering data----------
Press 1 to enter data , Press 2 to see data
1
Enter candidate name
e
Enter college name
dro
Enter round1 marks out of 10
5
Enter round2 marks out of 10
7
Enter round3 marks out of 10
8
Enter technical marks out of 20
10
Do you want to enter details of more students Press 1 otherwise Press 0 0
----------------
                                       2. ----------------See data--------------

Press 1 to enter data , Press 2 to see data
2
Details of all candidates
NAME,COLLEGE,ROUND1,ROUND2,ROUND3,TECH,TOTAL
('bhumika', 'dronacharya', 7.0, 8.0, 9.0, 16.0, 40.0)
('bhumika', 'dronacharya', 5.0, 6.0, 7.0, 19.0, 37.0)
('bhumika', 'druu', 7.0, 8.0, 1.0, 16.0, 32.0)
('aditi', 'dd', 1.0, 2.0, 3.0, 5.0, 11.0)
('ram', 'dronacharya', 1.0, 2.0, 3.0, 5.0, 11.0)
('shyam', 'dronacharya', 6.0, 7.0, 8.0, 18.0, 39.0)
('mittu', 'dronacharya', 4.0, 5.0, 7.0, 12.0, 28.0)
('a', 'dronacharya', 5.0, 10.0, 10.0, 15.0, 40.0)
('S', 'dce', 10.0, 10.0, 10.0, 7.0, 37.0)
('e', 'dro', 5.0, 7.0, 8.0, 10.0, 30.0)
------------

RESULT STATUS

('bhumika', 'selected')
('bhumika', 'selected')
('bhumika', 'rejected')
('aditi', 'rejected')
('ram', 'rejected')
('shyam', 'selected')
('mittu', 'rejected')
('a', 'selected')
('S', 'selected')
('e', 'rejected')
----------------

Ranking status

 Name  Total Rank
('bhumika', 40.0, 1)
('a', 40.0, 1)
('shyam', 39.0, 2)
('bhumika', 37.0, 3)
('S', 37.0, 3)
('bhumika', 32.0, 4)
('e', 30.0, 5)
('mittu', 28.0, 6)
('aditi', 11.0, 7)
('ram', 11.0, 7)

Process finished with exit code 0

