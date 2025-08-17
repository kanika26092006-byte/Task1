import json
import os

# File to save tasks
TASKS_FILE = "tasks.json"

# Load existing tasks if file exists
def load_tasks():
    if os.path.exists(TASKS_FILE):
        with open(TASKS_FILE, "r") as f:
            return json.load(f)
    return []

# Save tasks to file
def save_tasks(tasks):
    with open(TASKS_FILE, "w") as f:
        json.dump(tasks, f, indent=4)

# Show all tasks
def view_tasks(tasks):
    if not tasks:
        print("\n✅ No tasks available!\n")
    else:
        print("\n📋 Your To-Do List:")
        for i, task in enumerate(tasks, start=1):
            status = "✔️ Done" if task["done"] else "❌ Pending"
            print(f"{i}. {task['title']} [{status}]")
    print()

# Add new task
def add_task(tasks):
    title = input("Enter task: ").strip()
    if title:
        tasks.append({"title": title, "done": False})
        save_tasks(tasks)
        print("✅ Task added successfully!\n")
    else:
        print("⚠️ Task cannot be empty!\n")

# Mark task as done
def mark_done(tasks):
    view_tasks(tasks)
    if tasks:
        try:
            num = int(input("Enter task number to mark as done: "))
            if 1 <= num <= len(tasks):
                tasks[num-1]["done"] = True
                save_tasks(tasks)
                print("✅ Task marked as done!\n")
            else:
                print("⚠️ Invalid task number!\n")
        except ValueError:
            print("⚠️ Please enter a valid number!\n")

# Edit task
def edit_task(tasks):
    view_tasks(tasks)
    if tasks:
        try:
            num = int(input("Enter task number to edit: "))
            if 1 <= num <= len(tasks):
                new_title = input("Enter new task name: ").strip()
                if new_title:
                    tasks[num-1]["title"] = new_title
                    save_tasks(tasks)
                    print("✅ Task updated!\n")
                else:
                    print("⚠️ Task cannot be empty!\n")
            else:
                print("⚠️ Invalid task number!\n")
        except ValueError:
            print("⚠️ Please enter a valid number!\n")

# Delete task
def delete_task(tasks):
    view_tasks(tasks)
    if tasks:
        try:
            num = int(input("Enter task number to delete: "))
            if 1 <= num <= len(tasks):
                deleted = tasks.pop(num-1)
                save_tasks(tasks)
                print(f"🗑️ Deleted task: {deleted['title']}\n")
            else:
                print("⚠️ Invalid task number!\n")
        except ValueError:
            print("⚠️ Please enter a valid number!\n")

# Main Menu
def main():
    tasks = load_tasks()
    
    while True:
        print("=== 📝 TO-DO LIST MENU ===")
        print("1. View Tasks")
        print("2. Add Task")
        print("3. Mark Task as Done")
        print("4. Edit Task")
        print("5. Delete Task")
        print("6. Exit")
        
        choice = input("Enter choice (1-6): ")
        print()
        
        if choice == "1":
            view_tasks(tasks)
        elif choice == "2":
            add_task(tasks)
        elif choice == "3":
            mark_done(tasks)
        elif choice == "4":
            edit_task(tasks)
        elif choice == "5":
            delete_task(tasks)
        elif choice == "6":
            print("👋 Exiting... Your tasks are saved!")
            break
        else:
            print("⚠️ Invalid choice! Please try again.\n")

if __name__ == "__main__":
    main()
