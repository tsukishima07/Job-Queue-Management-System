# Job-Queue-Management-System
Job Queue management System


import tkinter as tk
from tkinter import messagebox

# Structure to represent a job
class Job:
    def __init__(self, id, priority, burst_time):
        self.id = id
        self.priority = priority
        self.burst_time = burst_time

# Priority queue class
class PriorityQueue:
    def __init__(self):
        self.jobs = []
    
    def enqueue(self, job):
        self.jobs.append(job)
    
    def dequeue(self):
        if not self.jobs:
            return None

        highest_priority_index = 0
        for i in range(1, len(self.jobs)):
            if (self.jobs[i].priority > self.jobs[highest_priority_index].priority or
               (self.jobs[i].priority == self.jobs[highest_priority_index].priority and
                self.jobs[i].burst_time < self.jobs[highest_priority_index].burst_time)):
                highest_priority_index = i

        return self.jobs.pop(highest_priority_index)

    def display(self):
        return self.jobs

    def is_empty(self):
        return len(self.jobs) == 0

# Functions for the GUI
def add_job():
    try:
        id = int(entry_id.get())
        priority = int(entry_priority.get())
        burst_time = int(entry_burst_time.get())
        
        job = Job(id, priority, burst_time)
        pq.enqueue(job)
        
        messagebox.showinfo("Success", "Job added successfully!")
        entry_id.delete(0, tk.END)
        entry_priority.delete(0, tk.END)
        entry_burst_time.delete(0, tk.END)
    except ValueError:
        messagebox.showerror("Error", "Please enter valid integer values for Job ID, Priority, and Burst Time.")

def display_queue():
    if pq.is_empty():
        messagebox.showinfo("Queue", "The queue is empty!")
    else:
        jobs = pq.display()
        display_text = "\n".join([f"Job ID: {job.id}, Priority: {job.priority}, Burst Time: {job.burst_time}" for job in jobs])
        messagebox.showinfo("Queue", display_text)

def process_jobs():
    if pq.is_empty():
        messagebox.showinfo("Processing", "No jobs to process!")
    else:
        processing_text = ""
        while not pq.is_empty():
            job = pq.dequeue()
            processing_text += f"Processing Job ID: {job.id} with Priority: {job.priority} and Burst Time: {job.burst_time}\n"
        messagebox.showinfo("Processing", processing_text)

# Initialize the priority queue
pq = PriorityQueue()

# GUI setup
root = tk.Tk()
root.title("Job Queue Management System")

# Job ID
label_id = tk.Label(root, text="Job ID:")
label_id.grid(row=0, column=0)
entry_id = tk.Entry(root)
entry_id.grid(row=0, column=1)

# Job Priority
label_priority = tk.Label(root, text="Job Priority:")
label_priority.grid(row=1, column=0)
entry_priority = tk.Entry(root)
entry_priority.grid(row=1, column=1)

# Burst Time
label_burst_time = tk.Label(root, text="Burst Time:")
label_burst_time.grid(row=2, column=0)
entry_burst_time = tk.Entry(root)
entry_burst_time.grid(row=2, column=1)

# Buttons
button_add = tk.Button(root, text="Add Job", command=add_job)
button_add.grid(row=3, column=0, pady=10)

button_display = tk.Button(root, text="Display Queue", command=display_queue)
button_display.grid(row=3, column=1)

button_process = tk.Button(root, text="Process Jobs", command=process_jobs)
button_process.grid(row=4, column=0, columnspan=2, pady=10)

# Start the GUI loop
root.mainloop()
