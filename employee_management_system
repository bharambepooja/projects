import streamlit as st
import mysql.connector
from datetime import date
from datetime import datetime
import pandas as pd
st.set_page_config(page_title="Employee Management System.App")
import base64
def add_bg_from_local(image_file):
    with open(image_file, "rb") as image_file:
        encoded_string = base64.b64encode(image_file.read())
    st.markdown(
    f"""
    <style>
    .stApp {{
        background-image: url(data:image/{"jpg"};base64,{encoded_string.decode()});
        background-size: cover
    }}
    </style>
    """,
    unsafe_allow_html=True
    )
add_bg_from_local('image2.jpg')  
st.title("EMPLOYEE MANAGEMENT SYSTEM")
choice=st.sidebar.selectbox("My Menu",("Home","Employee","Admin"))
if(choice=="Home"):
    st.image("https://www.analyticsinsight.net/wp-content/uploads/2018/12/Employee-management.jpg")
elif(choice=="Employee"):
    if'login' not in st.session_state:
        st.session_state['login']=False
    id=st.text_input("Enter Employee ID")
    password=st.text_input("Enter Password")
    btn=st.button("Login")
    if btn:
        mydb=mysql.connector.connect(host="localhost",user="root",password="123456",database="employee")
        c=mydb.cursor()
        c.execute("select * from users")
        for r in c:
            if (r[0]==id and r[1]==password):
                st.session_state['login']=True
                break
        if(st.session_state['login']==False):
            st.subheader("Incorrect ID or Password")
    if(st.session_state['login']==True):
        st.subheader("Login Successful")
        st.success("Welcome Employee!")
        choice2=st.selectbox("Features",("None","View All Employees","Department","Add Leave Request","Attendance","Employee Shift Scheduled"))
        if(choice2=="View All Employees"):
            mydb=mysql.connector.connect(host="localhost",user="root",password="123456",database="employee")
            c=mydb.cursor()
            c.execute("select * from employees")
            l=[]
            for r in c:
                l.append(r)
            df=pd.DataFrame(data=l,columns=['Emp No','Employee Name','Age','Gender','Mob No'])
            st.dataframe(df)
        elif(choice2=="Department"):
            mydb=mysql.connector.connect(host="localhost",user="root",password="123456",database="employee")
            c=mydb.cursor()
            c.execute("select * from department")
            l=[]
            for r in c:
                l.append(r)
            df=pd.DataFrame(data=l,columns=['Dept No','Department Name','EMP No'])
            st.dataframe(df)
        elif(choice2=="Add Leave Request"):
            st.subheader("Add Leave Request")
            emp_name= st.text_input("Employee Name")
            leave_type = st.selectbox("Leave Type", ["Sick Leave", "Vacation Leave", "Other"])
            start_date = st.date_input("Start Date", min_value=date.today())
            end_date = st.date_input("End Date", min_value=date.today())
            btn2=st.button("Submit Leave Request")
            if (btn2):
                mydb=mysql.connector.connect(host="localhost",user="root",password="123456",database="employee")
                c=mydb.cursor()
                c.execute("insert into leave_request (emp_name, leave_type, start_date, end_date,leave_status) VALUES (%s, %s, %s, %s, %s)", (emp_name, leave_type, start_date, end_date, "Pending"))
                mydb.commit()
                st.success("Leave request submitted successfully!")
        elif(choice2=="Attendance"):
            st.subheader ("Attendance")
            emp_name= st.text_input("Employee Name")
            if st.button("Clock In"):
                attendance_date = datetime.today().date()
                clock_in_time = datetime.today().time()
                mydb=mysql.connector.connect(host="localhost",user="root",password="123456",database="employee")
                c=mydb.cursor()
                c.execute("insert into attendance (emp_name, attendance_date, clock_in_time) VALUES (%s, %s, %s)", (emp_name, attendance_date, clock_in_time))
                mydb.commit()
                st.success(f"Clock In recorded at {clock_in_time}")

            if st.button("Clock Out"):
                attendance_date = datetime.today().date()
                clock_out_time = datetime.today().time()
                mydb=mysql.connector.connect(host="localhost",user="root",password="123456",database="employee")
                c=mydb.cursor()
                c.execute("update attendance set clock_out_time = %s WHERE emp_name = %s AND attendance_date = %s", (clock_out_time, emp_name, attendance_date))
                mydb.commit()
                st.success(f"Clock Out recorded at {clock_out_time}")
        elif(choice2=="Employee Shift Scheduled"):
            st.subheader("Shift schedule")
            shift_id=st.selectbox("shift",[1,2,3])
            mydb=mysql.connector.connect(host="localhost",user="root",password="123456",database="employee")
            c=mydb.cursor()
            c.execute("select * from shifts where shift_id= %s", [shift_id])
            l=[]
            for r in c:
                l.append(r)
            df=pd.DataFrame(data=l,columns=['Emp No','Emp Name','Shift Id','Start Time','End Time','Scheduled Date'])
            st.dataframe(df)
            
            

            
elif(choice=="Admin"):
    if'alogin' not in st.session_state:
        st.session_state['alogin']=False
    id=st.text_input("Enter Admin ID")
    password=st.text_input("Enter Password")
    btn=st.button("Login")
    if btn:
        mydb=mysql.connector.connect(host="localhost",user="root",password="123456",database="employee")
        c=mydb.cursor()
        c.execute("select * from admins")
        for r in c:
            if (r[0]==id and r[1]==password):
                st.session_state['alogin']=True
                break
        if(st.session_state['alogin']==False):
            st.subheader("Incorrect ID or Password")
    if(st.session_state['alogin']==True):
        st.subheader("Login Successful")
        st.success("Welcome Admin")
        choice2=st.selectbox("Features",("None","Employees Salary and Job Type","Add Employees","Delete Employees","Display Leave Request","Attendance Record","Schedule Shift for Employees"))
        if(choice2=="Employees Salary and Job Type"):
            mydb=mysql.connector.connect(host="localhost",user="root",password="123456",database="employee")
            c=mydb.cursor()
            c.execute("select * from employee_jobinfo")
            l=[]
            for r in c:
                l.append(r)
            df=pd.DataFrame(data=l,columns=['Emp No','Salary','Job Type'])
            st.dataframe(df)
        elif(choice2=="Add Employees"):
            emp_no=st.text_input("Enter Employee No")
            emp_name=st.text_input("Enter Employee Name")
            emp_age=st.text_input("Enter Employee Age")
            emp_gender=st.text_input("Enter Employee Gender")
            emp_Mob_no=st.text_input("Enter Employee Mob_No")
            btn2=st.button("Add Employee")
            if(btn2):
                mydb=mysql.connector.connect(host="localhost",user="root",password="123456",database="employee")
                c=mydb.cursor()
                c.execute("insert into employees values(%s,%s,%s,%s,%s)",(emp_no,emp_name,emp_age,emp_gender,emp_Mob_no))
                mydb.commit()
                st.success("Employee data Added Successfully")
        elif(choice2=="Delete Employees"):
            emp_name = st.text_input("Employee Name to Delete")
            btn3=st.button("Delete Employee")
            if(btn3):
                mydb=mysql.connector.connect(host="localhost",user="root",password="123456",database="employee")
                c=mydb.cursor()
                c.execute("select * from employees where emp_name = %s", [emp_name])
                employee = c.fetchone()
                if employee:
                    c.execute("delete from employees where emp_name = %s", [emp_name])
                    mydb.commit()
                    st.success("Employee data deleted successfully!")
                else:
                    st.error("Employee not found. No data deleted.")
        elif(choice2=="Display Leave Request"):
            st.subheader("Leave Request")
            mydb=mysql.connector.connect(host="localhost",user="root",password="123456",database="employee")
            c=mydb.cursor()
            c.execute("select * from leave_request")
            l=[]
            for r in c:
                l.append(r)
            df=pd.DataFrame(data=l,columns=['Emp Name','Leave Type','Start Date','End Date','Leave Status'])
            st.dataframe(df)
        elif(choice2=="Attendance Record"):
            st.subheader("Attendance Records")
            mydb=mysql.connector.connect(host="localhost",user="root",password="123456",database="employee")
            c=mydb.cursor()
            c.execute("select * from attendance")
            l=[]
            for r in c:
                l.append(r)
            df=pd.DataFrame(data=l,columns=['Emp Name','Attendance Date','Clock In Time','Clock Out Time'])
            st.dataframe(df)
        elif(choice2=="Schedule Shift for Employees"):
            st.subheader("Schedule Shift for Employees")
            emp_no=st.text_input("Enter Employee No")
            emp_name=st.text_input("Enter Employee Name")
            st.write("Select Shift")
            shift_id = st.selectbox("Shift", [1, 2, 3])
            start_time = st.time_input("Start Time")
            end_time = st.time_input("End Time")
            scheduled_date = st.date_input("Scheduled Date")
            btn4=st.button("Schedule Shift")
            if(btn4):
                mydb=mysql.connector.connect(host="localhost",user="root",password="123456",database="employee")
                c=mydb.cursor()
                c.execute("insert into shifts VALUES (%s, %s, %s, %s,%s,%s)", (emp_no,emp_name,shift_id, start_time,end_time,scheduled_date))
                mydb.commit()
                st.success("Shift scheduled successfully!")


