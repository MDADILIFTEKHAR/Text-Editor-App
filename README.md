# Text-Editor-App
🖊️ Built a Powerful Text Editor App Using Python &amp; Tkinter! 🚀 A clean, customizable, and beginner-friendly text editor with light/dark mode, file operations, and font settings — all coded from scratch! Perfect for writing, editing, and learning GUI development in Python."


import tkinter as tk
from tkinter import filedialog, messagebox, font

# Initialize main window
root = tk.Tk()
root.title("Python Text Editor")
root.geometry("800x600")

# ============ Functions ============

def new_file():
    text_area.delete(1.0, tk.END)

def open_file():
    file = filedialog.askopenfilename(filetypes=[("Text Files", "*.txt")])
    if file:
        with open(file, "r") as f:
            text_area.delete(1.0, tk.END)
            text_area.insert(tk.END, f.read())

def save_file():
    file = filedialog.asksaveasfilename(defaultextension=".txt",
                                         filetypes=[("Text Files", "*.txt")])
    if file:
        with open(file, "w") as f:
            f.write(text_area.get(1.0, tk.END))
        messagebox.showinfo("Saved", "File saved successfully!")

def exit_app():
    if messagebox.askyesno("Exit", "Are you sure you want to exit?"):
        root.destroy()

def cut_text():
    text_area.event_generate("<<Cut>>")

def copy_text():
    text_area.event_generate("<<Copy>>")

def paste_text():
    text_area.event_generate("<<Paste>>")

def toggle_theme():
    if theme_var.get() == "Dark":
        text_area.config(bg="black", fg="white", insertbackground="white")
    else:
        text_area.config(bg="white", fg="black", insertbackground="black")

def change_font_size(size):
    text_area.config(font=("Arial", size))

# ============ Menu Bar ============

menu_bar = tk.Menu(root)
root.config(menu=menu_bar)

# File menu
file_menu = tk.Menu(menu_bar, tearoff=0)
file_menu.add_command(label="New", command=new_file)
file_menu.add_command(label="Open", command=open_file)
file_menu.add_command(label="Save", command=save_file)
file_menu.add_separator()
file_menu.add_command(label="Exit", command=exit_app)
menu_bar.add_cascade(label="File", menu=file_menu)

# Edit menu
edit_menu = tk.Menu(menu_bar, tearoff=0)
edit_menu.add_command(label="Cut", command=cut_text)
edit_menu.add_command(label="Copy", command=copy_text)
edit_menu.add_command(label="Paste", command=paste_text)
menu_bar.add_cascade(label="Edit", menu=edit_menu)

# ============ Toolbar ============

toolbar = tk.Frame(root, bg="lightgray", height=30)
toolbar.pack(fill=tk.X)

# Theme switch
theme_var = tk.StringVar(value="Light")
theme_menu = tk.OptionMenu(toolbar, theme_var, "Light", "Dark", command=lambda _: toggle_theme())
theme_menu.pack(side=tk.LEFT, padx=5)

# Font size
font_size = tk.IntVar(value=12)
font_menu = tk.OptionMenu(toolbar, font_size, *[8, 10, 12, 14, 16, 18, 20, 24], command=change_font_size)
font_menu.pack(side=tk.LEFT, padx=5)

# ============ Text Area ============

text_area = tk.Text(root, font=("Arial", 12), undo=True)
text_area.pack(expand=True, fill=tk.BOTH)

# ============ Run App ============

root.mainloop()
