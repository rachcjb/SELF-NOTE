import tkinter as tk
from tkinter import messagebox
from tkinter.scrolledtext import ScrolledText


def save_note():
    note = note_entry.get("1.0", "end-1c")
    with open("saved_notes.txt", "a") as f:
        f.write(note + "\n")
    status_bar.config(text="Note saved successfully!")
    note_entry.delete("1.0", "end-1c")
def pin_note():
    note = note_entry.get("1.0", "end-1c")
    with open("pinned_notes.txt", "a") as f:
        f.write(note + "\n")
    status_bar.config(text="Note pinned successfully!")
    note_entry.delete("1.0", "end-1c")
def delete():
    note = note_entry.get("1.0", "end-1c")
    with open("deleted_notes.txt", "a") as f:
        f.write(note + "\n")
    status_bar.config(text="Note deleted successfully!")
    note_entry.delete("1.0", "end-1c")
    view_recently_deleted_notes()  
def view_saved_notes():
    try:
        with open("saved_notes.txt", "r") as f:
            notes = f.read()
            messagebox.showinfo("Saved Notes", notes)
    except FileNotFoundError:
        messagebox.showwarning("View Saved Notes", "No saved notes found!")
def view_pinned_notes():
    try:
        with open("pinned_notes.txt", "r") as f:
            notes = f.read()
            messagebox.showinfo("Pinned Notes", notes)
    except FileNotFoundError:
        messagebox.showwarning("View pinned Notes", "No pinned notes found!")
def view_recently_deleted_notes():
    try:
        with open("deleted_notes.txt", "r") as f:
            notes = f.read()
            messagebox.showinfo("Recently Deleted Notes", notes)
    except FileNotFoundError:
        messagebox.showwarning("View Recently Deleted Notes", "No deleted notes found!")
def on_closing():
    if messagebox.askokcancel("Quit", "Do you want to exit the application?"):
        root.destroy()

root = tk.Tk()
root.title("Self Note")
root.geometry("400x200")
root.config(bg="black")

menu = tk.Menu(root)
root.config(menu=menu)

file_menu = tk.Menu(menu, tearoff=0)
menu.add_cascade(label="On My Note", menu=file_menu)
file_menu.add_command(label="Save Note", command=save_note)
file_menu.add_command(label="Pin Note", command=pin_note)
file_menu.add_command(label="Delete", command=delete)
file_menu.add_separator()
file_menu.add_command(label="View Saved Notes", command=view_saved_notes)
file_menu.add_command(label="View Pinned Notes", command=view_pinned_notes)
file_menu.add_command(label="View Recently Deleted Notes", command=view_recently_deleted_notes)
file_menu.add_separator()
file_menu.add_command(label="Exit", command=on_closing)


label = tk.Label(root, text="Enter note:", font='Helvetica', fg="black")
note_entry = ScrolledText(root, height=3, width=20, bg="white")
save_button = tk.Button(root, text="Save Note", command=save_note, bg="yellow")
pin_button = tk.Button(root, text="Pin Note", command=pin_note, bg="lightgreen")
delete_button=tk.Button(root, text="Delete", command=delete, bg="red")

label.grid(row=0, column=0, padx=20, pady=20)
note_entry.grid(row=0, column=1, columnspan=2, padx=10, pady=25, sticky="nsew")
save_button.grid(row=2, column=1, padx=10, pady=10, sticky="we")
pin_button.grid(row=2, column=2, padx=10, pady=10, sticky="we")
delete_button.grid(row=2, column=3, padx=10, pady=10, sticky="we")

status_bar = tk.Label(root, text="Ready", bd=1, relief="sunken", anchor="w")
status_bar.pack

root.mainloop()
# SELF-NOTE
