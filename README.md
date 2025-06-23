# to-do-list-python
A simple To-Do List desktop app using Python
import tkinter as tk
from tkinter import messagebox
from tkinter import PhotoImage

tasks = []

def add_task(event=None):
    task = entry.get().strip()
    if task:
        tasks.append(task)
        update_listbox()
        entry.delete(0, tk.END)
    else:
        messagebox.showwarning("Warning", "Please enter a task first!")

def delete_task():
    selected = listbox.curselection()
    if selected:
        index = selected[0]
        tasks.pop(index)
        update_listbox()
    else:
        messagebox.showinfo("Info", "Please select a task to delete")

def clear_all():
    if messagebox.askyesno("Confirmation", "Are you sure you want to delete all tasks?"):
        tasks.clear()
        update_listbox()

def update_listbox():
    listbox.delete(0, tk.END)
    for i, task in enumerate(tasks, 1):
        listbox.insert(tk.END, f"{i}. {task}")
    task_count.set(f"Total tasks: {len(tasks)}")

# Create the main window
root = tk.Tk()
root.title("📝 To-Do List")
root.geometry("600x500")
root.configure(bg="#add8e6")

# Allow the window to be resizable
root.rowconfigure(2, weight=1)  # row where the listbox lives
root.columnconfigure(0, weight=1)

# Task count variable
task_count = tk.StringVar()
task_count.set("Total tasks: 0")

# Top frame (entry and add button)
frame_top = tk.Frame(root, bg="#add8e6")
frame_top.grid(row=0, column=0, pady=10)

entry = tk.Entry(frame_top, width=35, font=("Arial", 14))
entry.pack(side=tk.LEFT, padx=5)
entry.bind("<Return>", add_task)

add_button = tk.Button(frame_top, text="➕ Add", command=add_task, bg="#90ee90", font=("Arial", 10, "bold"))
add_button.pack(side=tk.LEFT)

# Task list (centered and resizable)
frame_middle = tk.Frame(root, bg="#add8e6")
frame_middle.grid(row=2, column=0, sticky="nsew", padx=10, pady=5)
frame_middle.rowconfigure(0, weight=1)
frame_middle.columnconfigure(0, weight=1)

listbox = tk.Listbox(frame_middle, font=("Arial", 13))
listbox.grid(row=0, column=0, sticky="nsew")

# Scrollbar for listbox
scrollbar = tk.Scrollbar(frame_middle, orient="vertical", command=listbox.yview)
scrollbar.grid(row=0, column=1, sticky="ns")
listbox.config(yscrollcommand=scrollbar.set)

# Bottom frame (buttons)
frame_bottom = tk.Frame(root, bg="#add8e6")
frame_bottom.grid(row=3, column=0, pady=10)

delete_button = tk.Button(frame_bottom, text="🗑️ Delete Selected", command=delete_task, bg="#ffcccb", font=("Arial", 10))
delete_button.pack(side=tk.LEFT, padx=5)

clear_button = tk.Button(frame_bottom, text="🧹 Clear All", command=clear_all, bg="#ffa07a", font=("Arial", 10))
clear_button.pack(side=tk.LEFT, padx=5)

# Image (moved below buttons, resized)
try:
    logo_img = PhotoImage(file="pincil.png").subsample(6, 6)
    logo_label = tk.Label(root, image=logo_img, bg="#add8e6")
    logo_label.grid(row=4, column=0, pady=10)
except:
    logo_label = tk.Label(root, text="(Image not found)", bg="#add8e6", fg="red")
    logo_label.grid(row=4, column=0, pady=10)

# Label for task count
label_count = tk.Label(root, textvariable=task_count, bg="#add8e6", font=("Arial", 11, "italic"))
label_count.grid(row=5, column=0, pady=5)

# Start the app
root.mainloop()
