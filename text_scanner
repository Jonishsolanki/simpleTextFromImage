import cv2
import pytesseract
import numpy as np
import os
import pandas as pd
import tkinter as tk
from tkinter import filedialog
from tkinter import ttk

def read_text_from_image(filepath):
    list_of_text_data = []
    for file in os.listdir(filepath):
        complete_image_path = os.path.join(filepath, file)
        if not complete_image_path.endswith(('.png', '.jpg', '.jpeg', '.gif')):
            continue
        
        image = cv2.imread(complete_image_path)

        if image is None:
            print('Could not open or find the image: ')
            exit(0)

        alpha = 1.4  
        beta = 0     
        new_image = np.clip(alpha * image + beta, 0, 255).astype(np.uint8)

        gray_image = cv2.cvtColor(new_image, cv2.COLOR_BGR2GRAY)

        _, thresh1 = cv2.threshold(gray_image, 127, 255, cv2.THRESH_BINARY)

        config = ('-l eng --oem 1 --psm 3')

        text = pytesseract.image_to_string(thresh1, config=config)
        
        list_of_text_data.append({"filepath": complete_image_path, "text": text})
        
    return list_of_text_data

def select_folder(folder_path_var):
    folder_selected = filedialog.askdirectory(title="Select Folder")
    if folder_selected:
        folder_path_var.set(folder_selected)

def process_folder(folder_path_var):
    folder = folder_path_var.get()
    if folder:
        data = read_text_from_image(folder)
        df = pd.DataFrame(data)
        df.to_csv('image_to_text_data.csv')
        print("Text extraction completed successfully.")

def create_gui():
    root = tk.Tk()
    root.title("Image Text Extraction")
    root.geometry("400x200")
    root.resizable(False, False)

    folder_path_var = tk.StringVar()

    folder_label = tk.Label(root, text="Select Folder:", font=("Arial", 12))
    folder_label.pack(pady=10)

    folder_entry = tk.Entry(root, textvariable=folder_path_var, width=30)
    folder_entry.pack()

    browse_button = tk.Button(root, text="Browse", command=lambda: select_folder(folder_path_var))
    browse_button.pack(pady=5)

    submit_button = tk.Button(root, text="Submit", font=("Arial", 12), command=lambda: process_folder(folder_path_var))
    submit_button.pack(pady=10)

    root.mainloop()

if __name__ == "__main__":
    create_gui()
