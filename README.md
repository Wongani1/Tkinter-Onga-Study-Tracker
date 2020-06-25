# Tkinter-Onga-Study-Tracker
# A simple study tracker app built using tkinter Python Module
from tkinter import *
import sqlite3

root = Tk()
root.title('Onga Study Tracker')
root.geometry("400x400")

#Databases

#Create a database connect to one
conn = sqlite3.connect('study_box.db')

c = conn. cursor()

#Create table
'''
c.execute("""CREATE TABLE study(
    topic_name text,
    category_name text,
    hours_much integer,
    date date,
    start_time text,
    end_time text
    )""")
'''
#Create Edit Function to Update a Record
def update():
    conn = sqlite3.connect('study_box.db')

    c = conn.cursor()
    equip_id = delete_box.get()
    c.execute(""" update study SET
        topic_name= :topic,
        category_name= :category,
        hours_much= :hours,
        date= :date,
        start_time= :start,
        end_time= :end
        
        WHERE oid= :oid""",
              {
                  'topic' : topic_name_editor.get(),
                  'category': category_name_editor.get(),
                  'hours': hours_much_editor.get(),
                  'date': date_editor.get(),
                  'start': start_time_editor.get(),
                  'end': end_time_editor.get(),

                  'oid' : equip_id
              }
              )
    # Commit Changes
    conn.commit()

    # Close Connection
    conn.close()

    editor.destroy

def edit():
    global editor
    editor = Tk()
    editor.title('Update Study Record')
    editor.geometry("400x300")
    #Create a Database or connect to one
    conn = sqlite3.connect('study_box.db')
    c = conn.cursor()
    equip_id = delete_box.get()
    # Query the database
    c.execute("SELECT * FROM study WHERE oid = " + equip_id)
    equip = c.fetchall()
    #Create Global Variables or text box names
    global topic_name_editor
    global category_name_editor
    global hours_much_editor
    global date_editor
    global start_time_editor
    global end_time_editor
    # create text boxes
    topic_name_editor = Entry(editor, width=30)
    topic_name_editor.grid(row=0, column=1, padx=20, pady=(10, 0))
    category_name_editor = Entry(editor, width=30)
    category_name_editor.grid(row=1, column=1)
    hours_much_editor = Entry(editor, width=30)
    hours_much_editor.grid(row=2, column=1)
    date_editor = Entry(editor, width=30)
    date_editor.grid(row=3, column=1)
    start_time_editor = Entry(editor, width=30)
    start_time_editor.grid(row=4, column=1)
    end_time_editor = Entry(editor, width=30)
    end_time_editor.grid(row=5, column=1)

    # create text box labels
    topic_name_label = Label(editor, text="Topic")
    topic_name_label.grid(row=0, column=0, pady=(10, 0))
    category_name_label = Label(editor, text="Topic Category")
    category_name_label.grid(row=1, column=0)
    hours_much_label = Label(editor, text="Number of Hours")
    hours_much_label.grid(row=2, column=0)
    date_label = Label(editor, text="Date of Study")
    date_label.grid(row=3, column=0)
    start_time_label = Label(editor, text="Start Time")
    start_time_label.grid(row=4, column=0)
    end_time_label = Label(editor, text="End Time")
    end_time_label.grid(row=5, column=0)
    # Loop through results
    for equiping in equip:
        topic_name_editor.insert(0, equiping[0])
        category_name_editor.insert(0, equiping[1])
        hours_much_editor.insert(0, equiping[2])
        date_editor.insert(0, equiping[3])
        start_time_editor.insert(0, equiping[4])
        end_time_editor.insert(0, equiping[5])
    #Create a save Button
    edit_btn = Button(editor, text="Save Record", command=update)
    edit_btn.grid(row=6, column=0, columnspan=2, pady=10, padx=10, ipadx=145)

#Create Function to Delete A Record
def delete():
    conn = sqlite3.connect('study_box.db')
    c = conn.cursor()

    #Delete a Record
    c.execute("DELETE from study WHERE oid = " + delete.get())

    conn.commit()
    # Close Connection
    conn.close()
#create submit function for database
def submit():
    conn = sqlite3.connect('study_box.db')
    c = conn.cursor()
    #Insert Into Table
    c.execute("INSERT INTO study VALUES (:topic_name, :category_name, :hours_much, :date, :start_time, :end_time)",
              {
                  'topic_name': topic_name.get(),
                  'category_name': category_name.get(),
                  'hours_much': hours_much.get(),
                  'date': date.get(),
                  'start_time': start_time.get(),
                  'end_time': end_time.get()
              })
    conn.commit()
    # Close Connection
    conn.close()
    #clear text boxes
    topic_name.delete(0, END)
    category_name.delete(0, END)
    date.delete(0, END)
    hours_much.delete(0, END)
    start_time.delete(0, END)
    end_time.delete(0, END)

#create Query Function
def query():
    conn = sqlite3.connect('study_box.db')
    c = conn.cursor()
    #Query the database
    c.execute("SELECT *, oid FROM study ")
    equip = c.fetchall()
    #print(equip)

    #Loop through study records
    print_equip = ''
    for equiping in equip:
        print_equip += str(equiping[0]) + " " + str(equiping[1]) + " " + str(equiping[2]) + " " + str(equiping[3]) +  "\n"
    query_label = Label(root, text=print_equip)
    query_label.grid(row=12, column=0, columnspan=2)

    conn.commit()
    # Close Connection
    conn.close()
#create text boxes
topic_name= Entry(root, width=30)
topic_name.grid(row=0, column=1, padx=20, pady=(10, 0))
category_name= Entry(root, width=30)
category_name.grid(row=1, column=1)
hours_much= Entry(root, width=30)
hours_much.grid(row=2, column=1)
date= Entry(root, width=30)
date.grid(row=3, column=1)
start_time= Entry(root, width=30)
start_time.grid(row=4, column=1)
end_time= Entry(root, width=30)
end_time.grid(row=5, column=1)
delete_box = Entry(root, width=30)
delete_box.grid(row=9, column=1, pady=5)

#create text box labels
topic_name_label = Label(root, text="Topic")
topic_name_label.grid(row=0, column=0, pady=(10, 0))

category_name_label = Label(root, text="Topic Category")
category_name_label.grid(row=1, column=0)

hours_much_label = Label(root, text="Number of Hours")
hours_much_label.grid(row=2, column=0)

date_label = Label(root, text="Date of Study")
date_label.grid(row=3, column=0)

start_time_label = Label(root, text="Start Time")
start_time_label.grid(row=4, column=0)

end_time_label = Label(root, text="End Time")
end_time_label.grid(row=5, column=0)

delete_box_label = Label(root, text="Study ID")
delete_box_label.grid (row=9, column=0, pady=5)

#Buttons
submit_btn = Button(root, text="Add Study Details to Database", command=submit)
submit_btn.grid(row=6, column=0, columnspan=2, pady=10, padx=10, ipadx=100)

#Create a Query Button
query_btn = Button(root, text="Show Study Details", command=query)
query_btn.grid(row=7, column=0, columnspan=2, pady=10, padx=10, ipadx=137)

#Create a Delete Button
delete_btn = Button(root, text="Delete Study Detail", command=delete)
delete_btn.grid(row=10, column=0, columnspan=2, pady=10, padx=10, ipadx=137)

#Edit a Delete Button
edit_btn = Button(root, text="Update Study Detail", command=edit)
edit_btn.grid(row=11, column=0, columnspan=2, pady=10, padx=10, ipadx=140)

#Commit Changes
conn.commit()

#Close Connection
conn.close()
root.mainloop()
