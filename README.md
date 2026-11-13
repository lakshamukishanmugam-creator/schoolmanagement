# schoolmanagement
created for my project
import mysql.connector as mys

# Connecting with school database
conn = mys.connect(
    host="localhost",
    user="root",
    passwd="YOUR_MYSQL_PASSWORD",
    database="school"
)

cur = conn.cursor()


# Add Student
def add_student():
    admnno = int(input("Enter Admission Number: "))
    sname = input("Enter Student Name: ")
    grade = int(input("Enter Grade: "))
    address = input("Enter Address: ")
    contact = input("Enter Contact Number: ")

    sql = """INSERT INTO students
    (admnno, sname, grade, address, contact)
    VALUES (%s, %s, %s, %s, %s)"""

    cur.execute(sql, (admnno, sname, grade, address, contact))
    conn.commit()

    print("Student added successfully.")


# View All Students
def view_students():
    cur.execute("SELECT * FROM students")
    students = cur.fetchall()

    if students:
        print("\n--- Students List ---")

        for s in students:
            print(
                f"Admission No: {s[0]} | "
                f"Name: {s[1]} | "
                f"Grade: {s[2]} | "
                f"Address: {s[3]} | "
                f"Contact: {s[4]}"
            )
    else:
        print("No student records found.")


# Display Student
def display_student():
    admnno = int(input("Enter Admission Number: "))

    sql = "SELECT * FROM students WHERE admnno = %s"
    cur.execute(sql, (admnno,))

    s = cur.fetchone()

    if s:
        print("\nAdmission No:", s[0])
        print("Name:", s[1])
        print("Grade:", s[2])
        print("Address:", s[3])
        print("Contact:", s[4])
    else:
        print("No student found with that admission number.")


# Update Student
def update_student():
    admnno = int(input("Enter Admission Number of the student to update: "))

    print("\n1. Update Name")
    print("2. Update Grade")
    print("3. Update Address")
    print("4. Update Contact")

    choice = input("Enter your choice: ")

    if choice == '1':
        value = input("Enter new name: ")
        sql = "UPDATE students SET sname = %s WHERE admnno = %s"

    elif choice == '2':
        value = int(input("Enter new grade: "))
        sql = "UPDATE students SET grade = %s WHERE admnno = %s"

    elif choice == '3':
        value = input("Enter new address: ")
        sql = "UPDATE students SET address = %s WHERE admnno = %s"

    elif choice == '4':
        value = input("Enter new contact number: ")
        sql = "UPDATE students SET contact = %s WHERE admnno = %s"

    else:
        print("Invalid choice.")
        return

    cur.execute(sql, (value, admnno))
    conn.commit()

    if cur.rowcount > 0:
        print("Student details updated successfully.")
    else:
        print("Student not found or no changes made.")


# Search Student by Name
def search_student():
    name = input("Enter Student Name to search: ")

    sql = "SELECT * FROM students WHERE sname LIKE %s"
    cur.execute(sql, ('%' + name + '%',))

    students = cur.fetchall()

    if students:
        print("\n--- Search Results ---")

        for s in students:
            print(
                f"Admission No: {s[0]} | "
                f"Name: {s[1]} | "
                f"Grade: {s[2]} | "
                f"Address: {s[3]} | "
                f"Contact: {s[4]}"
            )
    else:
        print("No matching student found.")


# Delete Student
def delete_student():
    admnno = int(input("Enter Admission Number to delete: "))

    sql = "DELETE FROM students WHERE admnno = %s"
    cur.execute(sql, (admnno,))
    conn.commit()

    if cur.rowcount > 0:
        print("Student deleted successfully.")
    else:
        print("Student not found.")


# Main Menu
def menu():

    while True:
        print("\n========== SCHOOL MANAGEMENT SYSTEM ==========")

        print("1. Add Student")
        print("2. View All Students")
        print("3. Display Student")
        print("4. Update Student")
        print("5. Search Student by Name")
        print("6. Delete Student")
        print("7. Exit")

        choice = input("\nEnter your choice (1-7): ")

        if choice == '1':
            add_student()

        elif choice == '2':
            view_students()

        elif choice == '3':
            display_student()

        elif choice == '4':
            update_student()

        elif choice == '5':
            search_student()

        elif choice == '6':
            delete_student()

        elif choice == '7':
            print("Goodbye!")
            break

        else:
            print("Invalid choice. Please try again.")


# Run the Program
if __name__ == "__main__":
    try:
        menu()
    finally:
        cur.close()
        conn.close()
