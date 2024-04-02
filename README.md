# Employee-Management-System
import tkinter as tk
from tkinter import ttk, messagebox
from PIL import Image, ImageTk  #install the Pillow library for image handling
import pyodbc

class LoginPage:
    def _init_(self, root):
        self.root = root
        self.root.title("Login Page")

        # Set background image for the login page
        self.set_background_image(root)

        # Frame to hold content on top of the background image
        content_frame = tk.Frame(root)
        content_frame.place(relx=0.5, rely=0.5, anchor=tk.CENTER)

        # Heading label
        heading_label = tk.Label(content_frame, text="Employee Management System", font=("cooper black", 20, "bold"), fg="skyblue3")
        heading_label.pack(pady=(50, 20))  # Add padding to top and bottom

        # Username and Password labels and entries
        self.label_username = tk.Label(content_frame, text="Username:", font=("Helvetica", 16))
        self.label_password = tk.Label(content_frame, text="Password:", font=("Helvetica", 16))

        self.entry_username = tk.Entry(content_frame, font=("Helvetica", 16))
        self.entry_password = tk.Entry(content_frame, show="*", font=("Helvetica", 16))

        self.label_username.pack()
        self.entry_username.pack()
        self.label_password.pack()
        self.entry_password.pack()

        # Login button with larger size
        self.login_button = tk.Button(content_frame, text="Login", command=self.login, font=("Helvetica", 16), bg="slategray1", height=2, width=15)
        self.login_button.pack(pady=20)

        # Set window size and position
        self.root.geometry("{0}x{1}+0+0".format(root.winfo_screenwidth(), root.winfo_screenheight()))



    def set_background_image(self, window):
        # Load and resize background image
        background_image = Image.open(r"C:\Users\kazim\Downloads\e,ployee.png")  # Replace with the path to your image
        resized_image = background_image.resize((window.winfo_screenwidth(), window.winfo_screenheight()))
        background_photo = ImageTk.PhotoImage(resized_image)

        # Create a label to display the image
        background_label = tk.Label(window, image=background_photo)
        background_label.image = background_photo  # Keep a reference to the image to avoid garbage collection
        background_label.place(relx=0.5, rely=0.5, anchor=tk.CENTER)

    def login(self):
        # Replace the following with your authentication logic
        username = self.entry_username.get()
        password = self.entry_password.get()

        # Sample authentication logic (replace this with your actual logic)
        if username == "admin" and password == "password":
            messagebox.showinfo("Login Successful", "Welcome, " + username + "!")
            self.root.destroy()  # Close the login window

            # Call your employee management system here
            # For demonstration, let's assume you have another class called EmployeeManagementApp
            root = tk.Tk()
            app = EmployeeManagementApp(root)
            root.mainloop()

        else:
            messagebox.showerror("Login Failed", "Invalid credentials. Please try again.")


class EmployeeManagementApp:
    def _init_(self, master):
        self.master = master
        self.master.title("Employee Management System")

        # Set background image for the employee management app
        self.set_background_image(master)

        # Add a logo
        logo_image = tk.PhotoImage(
            file="C:/Users/kazim/Downloads/employee.png")  # Replace with the actual path to your logo image
        resized_logo_image = logo_image.subsample(3, 3)  # Change the factors as needed for resizing
        logo_label = tk.Label(master, image=resized_logo_image)
        logo_label.image = resized_logo_image  # Keep a reference to the image to avoid garbage collection
        logo_label.pack()


        # Creating a heading label
        heading_label = tk.Label(master, text="Employee Management System", font=("cooper black", 20, "bold"), fg="skyblue3", pady=20)
        heading_label.pack()

        # Database connection
        connection_string = r"DRIVER={Microsoft Access Driver (*.mdb, *.accdb)};DBQ=C:\Users\kazim\Downloads\employee management system.accdb;"
        self.connection = pyodbc.connect(connection_string)
        self.cursor = self.connection.cursor()

        # Buttons
        tk.Button(master, text="Add Employee", command=self.add_employee_page, font=("Helvetica", 14), bg="slategray1").pack(pady=10)
        tk.Button(master, text="View Employees", command=self.view_employees_page, font=("Helvetica", 14), bg="slategray1").pack(pady=10)
        tk.Button(master, text="Remove Employee", command=self.remove_employee_page, font=("Helvetica", 14), bg="slategray1").pack(
            pady=10)
        tk.Button(master, text="Edit Employee", command=self.edit_employee_page, font=("Helvetica", 14), bg="slategray1").pack(pady=10)

        # Set window size and position
        self.master.geometry("{0}x{1}+0+0".format(master.winfo_screenwidth(), master.winfo_screenheight()))

    def set_background_image(self, window):
        # Load and resize background image
        background_image = Image.open(r"C:\Users\kazim\Downloads\e,ployee.png")  # Replace with the path to your image
        resized_image = background_image.resize((window.winfo_screenwidth(), window.winfo_screenheight()))
        background_photo = ImageTk.PhotoImage(resized_image)

        # Create a label to display the image
        background_label = tk.Label(window, image=background_photo)
        background_label.image = background_photo  # Keep a reference to the image to avoid garbage collection
        background_label.place(relx=0.5, rely=0.5, anchor=tk.CENTER)

    def add_employee_page(self):
        self.show_home_page()
        # Creating a new window for adding employee details
        add_employee_window = tk.Toplevel(self.master)
        add_employee_window.title("Add Employee")
        add_employee_window.geometry("800x600+0+0")  # Set window size and position to full screen

        # Entry widgets
        tk.Label(add_employee_window, text="Name:", font=("Helvetica", 14)).pack(pady=10)
        name_entry = tk.Entry(add_employee_window, font=("Helvetica", 12))
        name_entry.pack()

        tk.Label(add_employee_window, text="Address:", font=("Helvetica", 14)).pack(pady=10)
        address_entry = tk.Entry(add_employee_window, font=("Helvetica", 12))
        address_entry.pack()

        tk.Label(add_employee_window, text="Designation:", font=("Helvetica", 14)).pack(pady=10)
        designation_entry = tk.Entry(add_employee_window, font=("Helvetica", 12))
        designation_entry.pack()

        tk.Label(add_employee_window, text="Salary:", font=("Helvetica", 14)).pack(pady=10)
        salary_entry = tk.Entry(add_employee_window, font=("Helvetica", 12))
        salary_entry.pack()

        # Button to add employee to the database
        tk.Button(add_employee_window, text="Add", command=lambda: self.save_employee(
            name_entry.get(), address_entry.get(), designation_entry.get(), salary_entry.get(), add_employee_window),
            font=("Helvetica", 14), bg="slategray1").pack(pady=20)

        # Back button
        tk.Button(add_employee_window, text="Back", command=add_employee_window.destroy, font=("Helvetica", 14), bg="slategray1").pack(pady=20)

    def save_employee(self, name, address, designation, salary, window):
        if name and address and designation and salary:
            try:
                # Insert employee into the database
                self.cursor.execute("INSERT INTO Employees (Name, Address, Designation, Salary) VALUES (?, ?, ?, ?)",
                                    (name, address, designation, salary))
                self.connection.commit()
                messagebox.showinfo("Success", "Employee added successfully!")
                window.destroy()  # Close the add employee window
            except Exception as e:
                messagebox.showerror("Error", f"Error adding employee: {e}")
        else:
            messagebox.showerror("Error", "Please fill in all fields.")

    def view_employees_page(self):
        self.show_home_page()
        # Fetch employees from the database and display them in a new window
        self.cursor.execute("SELECT * FROM Employees")
        data = self.cursor.fetchall()

        view_employees_window = tk.Toplevel(self.master)
        view_employees_window.title("View Employees")
        view_employees_window.geometry("800x600+0+0")  # Set window size and position to full screen

        # Create a treeview to display employee details in a table
        columns = ("Name", "Address", "Designation", "Salary")
        treeview = ttk.Treeview(view_employees_window, columns=columns, show="headings")

        for col in columns:
            treeview.heading(col, text=col)
            treeview.column(col, width=250)  # Adjust the column width as needed

        if data:
            for employee in data:
                treeview.insert("", "end", values=(employee.Name, employee.Address, employee.Designation, employee.Salary))
        else:
            treeview.insert("", "end", values=("No employees found.", "", "", ""))

        treeview.pack(pady=20)

        # Back button
        tk.Button(view_employees_window, text="Back", command=view_employees_window.destroy, font=("Helvetica", 14), bg="slategray1").pack(pady=20)

    def remove_employee_page(self):
        self.show_home_page()
        # Fetch employees from the database and display them in a new window
        self.cursor.execute("SELECT * FROM Employees")
        data = self.cursor.fetchall()

        remove_employee_window = tk.Toplevel(self.master)
        remove_employee_window.title("Remove Employee")
        remove_employee_window.geometry("800x600+0+0")  # Set window size and position to full screen

        if data:
            # Listbox to display employees
            employee_listbox = tk.Listbox(remove_employee_window, selectmode=tk.SINGLE, font=("Helvetica", 12), width=50)
            employee_listbox.pack(pady=100)

            for employee in data:
                employee_listbox.insert(tk.END, f"{employee.Name} - {employee.Designation}")

            # Button to remove selected employee
            tk.Button(remove_employee_window, text="Remove", command=lambda: self.delete_employee(
                employee_listbox.get(employee_listbox.curselection()), remove_employee_window), font=("Helvetica", 14), bg="slategray1").pack(pady=20)
        else:
            tk.Label(remove_employee_window, text="No employees found.", font=("Helvetica", 12)).pack()

        # Back button
        tk.Button(remove_employee_window, text="Back", command=remove_employee_window.destroy, font=("Helvetica", 14), bg="slategray1").pack(pady=20)

    def delete_employee(self, selected_employee, window):
        if selected_employee:
            selected_employee_name = selected_employee.split(" - ")[0]
            try:
                # Delete selected employee from the database
                self.cursor.execute("DELETE FROM Employees WHERE Name=?", (selected_employee_name,))
                self.connection.commit()
                messagebox.showinfo("Success", f"{selected_employee_name} removed successfully!")
                window.destroy()  # Close the remove employee window
            except Exception as e:
                messagebox.showerror("Error", f"Error removing employee: {e}")
        else:
            messagebox.showerror("Error", "Please select an employee to remove.")

    def edit_employee_page(self):
        self.show_home_page()
        # Fetch employees from the database and display them in a new window
        self.cursor.execute("SELECT * FROM Employees")
        data = self.cursor.fetchall()

        edit_employee_window = tk.Toplevel(self.master)
        edit_employee_window.title("Edit Employee")
        edit_employee_window.geometry("800x600+0+0")  # Set window size and position to full screen

        if data:
            # Listbox to display employees
            employee_listbox = tk.Listbox(edit_employee_window, selectmode=tk.SINGLE, font=("Helvetica", 12), width=50)
            employee_listbox.pack(pady=100)

            for employee in data:
                employee_listbox.insert(tk.END, f"{employee.Name} - {employee.Designation}")

            # Button to load selected employee details for editing
            tk.Button(edit_employee_window, text="Edit", command=lambda: self.load_edit_page(
                employee_listbox.get(employee_listbox.curselection()), edit_employee_window), font=("Helvetica", 14), bg="slategray1").pack(pady=20)
        else:
            tk.Label(edit_employee_window, text="No employees found.", font=("Helvetica", 12)).pack()

        # Back button
        tk.Button(edit_employee_window, text="Back", command=edit_employee_window.destroy, font=("Helvetica", 14), bg="slategray1").pack(pady=20)

    def load_edit_page(self, selected_employee, window):
        if selected_employee:
            selected_employee_name = selected_employee.split(" - ")[0]
            # Fetch the details of the selected employee
            self.cursor.execute("SELECT * FROM Employees WHERE Name=?", (selected_employee_name,))
            employee_details = self.cursor.fetchone()

            # Creating a new window for editing employee details
            edit_employee_page = tk.Toplevel(self.master)
            edit_employee_page.title("Edit Employee")
            edit_employee_page.geometry("800x600+0+0")  # Set window size and position to full screen

            # Entry widgets with existing details for editing
            tk.Label(edit_employee_page, text="Name:", font=("Helvetica", 14)).pack(pady=10)
            name_entry = tk.Entry(edit_employee_page, font=("Helvetica", 12))
            name_entry.insert(tk.END, employee_details.Name)
            name_entry.pack()

            tk.Label(edit_employee_page, text="Address:", font=("Helvetica", 14)).pack(pady=10)
            address_entry = tk.Entry(edit_employee_page, font=("Helvetica", 12))
            address_entry.insert(tk.END, employee_details.Address)
            address_entry.pack()

            tk.Label(edit_employee_page, text="Designation:", font=("Helvetica", 14)).pack(pady=10)
            designation_entry = tk.Entry(edit_employee_page, font=("Helvetica", 12))
            designation_entry.insert(tk.END, employee_details.Designation)
            designation_entry.pack()

            tk.Label(edit_employee_page, text="Salary:", font=("Helvetica", 14)).pack(pady=10)
            salary_entry = tk.Entry(edit_employee_page, font=("Helvetica", 12))
            salary_entry.insert(tk.END, employee_details.Salary)
            salary_entry.pack()

            # Button to save edited employee details
            tk.Button(edit_employee_page, text="Save", command=lambda: self.save_edited_employee(
                selected_employee_name, name_entry.get(), address_entry.get(), designation_entry.get(), salary_entry.get(),
                window, edit_employee_page), font=("Helvetica", 14)).pack(pady=20)

        else:
            messagebox.showerror("Error", "Please select an employee to edit.")

    def save_edited_employee(self, old_name, new_name, address, designation, salary, listbox_window, edit_page_window):
        if new_name and address and designation and salary:
            try:
                # Update the employee details in the database
                self.cursor.execute("UPDATE Employees SET Name=?, Address=?, Designation=?, Salary=? WHERE Name=?",
                                    (new_name, address, designation, salary, old_name))
                self.connection.commit()
                messagebox.showinfo("Success", f"Details for {old_name} updated successfully!")
                listbox_window.destroy()  # Close the listbox window
                edit_page_window.destroy()  # Close the edit employee window
            except Exception as e:
                messagebox.showerror("Error", f"Error updating employee details: {e}")
        else:
            messagebox.showerror("Error", "Please fill in all fields.")

    def show_home_page(self):
        self.master.deiconify()  # Show the home page if hidden

    def _del_(self):
        # Close the database connection when the Tkinter application is closed
        if hasattr(self, 'connection'):
            self.connection.close()


if _name_ == "_main_":
    root = tk.Tk()
    login_page = LoginPage(root)
    root.mainloop()
