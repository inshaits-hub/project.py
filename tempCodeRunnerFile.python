import os
import csv
import datetime
 
   # ----------------Linked List Implementation----------------------

class LLNode:
    def __init__(self, task):
        self.task = task            # dictionary representing the task
        self.next = None

class LinkedList:

    #Singly linked list for sequential insertion-order display.

    def __init__(self):
        self.head = None

    def append(self, task):     #Insert task at end of linked list
        node = LLNode(task)
        if not self.head:
            self.head = node
            return
        cur = self.head
        while cur.next:
            cur = cur.next
        cur.next = node

    def remove_by_id(self, task_id): #Remove task node based on ID
        prev = None
        cur = self.head
        while cur:
            if cur.task['id'] == task_id:
                if prev:
                    prev.next = cur.next
                else:
                    self.head = cur.next
                return True
            prev = cur
            cur = cur.next
        return False

    def to_list(self):   #Convert linked list into Python list
        arr = []
        cur = self.head
        while cur:
            arr.append(cur.task)
            cur = cur.next
        return arr

    def display(self):   #Print all tasks in linked list (in insertion order)
        cur = self.head
        if not cur:
            print("No tasks in linked list.")
            return
        print("\n" + "="*85)
        print(f"{'ID':<5} | {'Task Name':<25} | {'Priority':<10} | {'Deadline':<12} | {'Status':<10}")
        print("="*85)
        while cur:
            t = cur.task
            print(f"{t['id']:<5} | {t['name']:<25} | {t['priority']:<10} | {t['deadline']:<12} | {t['status']:<10}")
            cur = cur.next
        print("="*85 + "\n")

# -----------------Binary Search Tree (BST)---------------------

# -----------------Binary Search Tree (BST)---------------------

class BSTNode:
    def __init__(self, task):
        self.task = task
        self.left = None
        self.right = None

class BST:
    def __init__(self):
        self.root = None

    def insert(self, task):
        def _insert(node, task):
            if not node:
                return BSTNode(task)
            if task['id'] < node.task['id']:
                node.left = _insert(node.left, task)
            else:
                node.right = _insert(node.right, task)
            return node
        self.root = _insert(self.root, task)

    def inorder(self):
        res = []
        def _in(n):
            if n:
                _in(n.left)
                res.append(n.task)
                _in(n.right)
        _in(self.root)
        return res

    def preorder(self):
        res = []
        def _pre(n):
            if n:
                res.append(n.task)
                _pre(n.left)
                _pre(n.right)
        _pre(self.root)
        return res

    def postorder(self):
        res = []
        def _post(n):
            if n:
                _post(n.left)
                _post(n.right)
                res.append(n.task)
        _post(self.root)
        return res

    # Display helper
    def display_tasks(self, arr, title):
        if not arr:
            print("BST empty.")
            return
        print(f"\n--- {title} ---")
        print("="*85)
        print(f"{'ID':<5} | {'Task Name':<25} | {'Priority':<10} | {'Deadline':<12} | {'Status':<10}")
        print("="*85)
        for t in arr:
            print(f"{t['id']:<5} | {t['name']:<25} | {t['priority']:<10} | {t['deadline']:<12} | {t['status']:<10}")
        print("="*85 + "\n")

    def display_inorder(self):
        self.display_tasks(self.inorder(), "BST Inorder Traversal (Left → Root → Right)")

    def display_preorder(self):
        self.display_tasks(self.preorder(), "BST Preorder Traversal (Root → Left → Right)")

    def display_postorder(self):
        self.display_tasks(self.postorder(), "BST Postorder Traversal (Left → Right → Root)")

#--------------------- Hash Table Implementation---------------------

class HashTable:
    #simple hash table with chaining for collision resolution
    def __init__(self, size=101):
        self.size = size
        self.table = [[] for _ in range(size)]

    def _hash(self, key):  #Simple hash function for integer keys
        return key % self.size

    def insert(self, task): #Insert task into hash table
        h = self._hash(task['id'])

        # replace if id exists

        for i, (k, v) in enumerate(self.table[h]):
            if k == task['id']:
                self.table[h][i] = (task['id'], task)
                return
        self.table[h].append((task['id'], task))

    def search(self, task_id):  #Search task by ID
        h = self._hash(task_id)
        for k, v in self.table[h]:
            if k == task_id:
                return v
        return None

    def delete(self, task_id): #Delete task from hash table
        h = self._hash(task_id)
        bucket = self.table[h]
        for i, (k, v) in enumerate(bucket):
            if k == task_id:
                bucket.pop(i)
                return True
        return False

    def all_items(self): #Return all task objects from hash table
        items = []
        for bucket in self.table:
            for k, v in bucket:
                items.append(v)
        return items

    def display_all(self): #Print all items in hash table
        arr = self.all_items()
        if not arr:
            print("Hash Table empty.")
            return
        print("\n" + "="*85)
        print(f"{'ID':<5} | {'Task Name':<25} | {'Priority':<10} | {'Deadline':<12} | {'Status':<10}")
        print("="*85)
        for t in arr:
            print(f"{t['id']:<5} | {t['name']:<25} | {t['priority']:<10} | {t['deadline']:<12} | {t['status']:<10}")
        print("="*85 + "\n")

#--------------Main Task Scheduler System -----------------

class TaskScheduler:
    DATA_FILE = 'tasks_data.csv'

    def __init__(self):

        # Initialize all DS and load data

        self.tasks = []      
        self.linked_list = LinkedList()
        self.bst = BST()
        self.ht = HashTable(size=211)       

        # load data from file (if available)
        self.load_data()

   
    # ---------------------Utility methods---------------------
  
    def clear_screen(self): #Generate new unique numeric ID
        os.system('cls' if os.name == 'nt' else 'clear')

    def get_next_id(self):
        # compute next id based on max id present
        all_ids = [t['id'] for t in self.tasks] if self.tasks else []
        if not all_ids:
            return 1
        return max(all_ids) + 1

   
    # ---------------------File Handling (CSV)---------------------

    def save_file(self): #Save tasks to CSV file
        filename = self.DATA_FILE
        try:
            with open(filename, 'w', newline='', encoding='utf-8') as f:
                writer = csv.writer(f)
                writer.writerow(['id', 'name', 'priority', 'deadline', 'status'])
                for t in self.tasks:
                    writer.writerow([t['id'], t['name'], t['priority'], t['deadline'], t['status']])
            print(f"Saved to {filename}")
        except Exception as e:
            print("Error saving file:", e)

    def load_data(self):
        filename = self.DATA_FILE

        # clear structures

        self.tasks = []
        self.linked_list = LinkedList()
        self.bst = BST()
        self.ht = HashTable(size=211)

        if not os.path.exists(filename):

            # Load default testing data if file not exists

            default = [
           
                 {"id": 1, "name": "Finish Python Project", "priority": "High", "deadline": "2025-12-15", "status": "Pending"},
                 {"id": 2, "name": "Study for Calculus", "priority": "High", "deadline": "2025-12-10", "status": "Pending"},
                 {"id": 3, "name": "Buy Groceries", "priority": "Low", "deadline": "2025-12-05", "status": "Completed"},
                 {"id": 4, "name": "Call Parents", "priority": "Medium", "deadline": "2025-12-06", "status": "Pending"},
                 {"id": 5, "name": "Gym Workout", "priority": "Medium", "deadline": "2025-12-04", "status": "Pending"},
                 {"id": 6, "name": "Read History Chapter", "priority": "Low", "deadline": "2025-12-12", "status": "Pending"},
                 {"id": 7, "name": "Clean Dorm Room", "priority": "Low", "deadline": "2025-12-07", "status": "Pending"},
                 {"id": 8, "name": "Register for Next Sem", "priority": "High", "deadline": "2025-12-20", "status": "Pending"} 
                  
     ]
            
            for t in default:
                task = dict(t)
                self.tasks.append(task)
                self.linked_list.append(task)
                self.bst.insert(task)
                self.ht.insert(task)
            return
        
            

        try:
            with open(filename, 'r', newline='', encoding='utf-8') as f:
                reader = csv.DictReader(f)
                for row in reader:

                    # CSV fields are strings; cast id to int

                    try:
                        task = {
                            'id': int(row['id']),
                            'name': row['name'],
                            'priority': row['priority'],
                            'deadline': row['deadline'],
                            'status': row['status']
                        }
                    except Exception:
                        continue
                    self.tasks.append(task)
                    self.linked_list.append(task)
                    self.bst.insert(task)
                    self.ht.insert(task)
            print(f"Loaded {len(self.tasks)} tasks from {filename}")
        except Exception as e:
            print("Error loading file:", e)

    
    # ----------------Core operations-----------------

    def view_tasks_linkedlist(self):
        print("\n--- Tasks (Linked List - insertion order) ---")
        self.linked_list.display()

    def view_tasks_bst_ordered(self):
        print("\n--- Tasks (BST inorder - ordered by ID) ---")
        self.bst.display_inorder() 

    def view_tasks_bst_preorder(self):
        print("\n--- Tasks (BST Preorder Traversal) ---")
        self.bst.display_preorder()

    def view_tasks_bst_postorder(self):
        print("\n--- Tasks (BST Postorder Traversal) ---")
        self.bst.display_postorder()
    
    
    def view_tasks_hashtable(self):
        print("\n--- Tasks (Hash Table - all items in table) ---")
        self.ht.display_all()

    def add_task(self):
        print("\n--- Add New Task ---")
        name = input("Enter Task Name: ").strip()
        if not name:
            print("Task name cannot be empty.")
            return
        
        # Priority validation

        while True:
            priority = input("Priority (High/Medium/Low): ").capitalize().strip()
            if priority in ["High", "Medium", "Low"]:
                break
            print("Invalid input. Please type High, Medium, or Low.")

        # Date validation

        while True:
            date_str = input("Deadline (YYYY-MM-DD): ").strip()
            try:
                datetime.datetime.strptime(date_str, '%Y-%m-%d')
                break
            except ValueError:
                print("Incorrect format. Please use YYYY-MM-DD.")

        new_task = {
            "id": self.get_next_id(),
            "name": name,
            "priority": priority,
            "deadline": date_str,
            "status": "Pending"
        }

        # insert into all DS

        self.tasks.append(new_task)
        self.linked_list.append(new_task)
        self.bst.insert(new_task)
        self.ht.insert(new_task)
        print("Task added successfully! ID =", new_task['id'])

    def search_task(self):
        try:
            task_id = int(input("Enter ID to search: "))
        except ValueError:
            print("Invalid input.")
            return
        task = self.ht.search(task_id)
        if task:
            print("Found task:")
            print(f"ID: {task['id']}")
            print(f"Name: {task['name']}")
            print(f"Priority: {task['priority']}")
            print(f"Deadline: {task['deadline']}")
            print(f"Status: {task['status']}")
        else:
            print("Task ID not found.")

    def mark_completed(self):
        try:
            task_id = int(input("Enter ID to mark as Completed: "))
        except ValueError:
            print("Invalid input.")
            return
        task = self.ht.search(task_id)
        if task:
            task['status'] = 'Completed'
            print('Status updated to Completed.')
        else:
            print('ID not found.')

    def delete_task(self):
        try:
            task_id = int(input("Enter ID to Delete: "))
        except ValueError:
            print("Invalid input.")
            return
        
        # first delete from hash table (check existence)

        if self.ht.delete(task_id):

            # delete from BST and linked list and tasks list

            self.bst.delete(task_id)
            removed = self.linked_list.remove_by_id(task_id)
            self.tasks = [t for t in self.tasks if t['id'] != task_id]
            print("Deleted.")
        else:
            print("ID not found.")

    
    #------------- Menu and interaction------------------
    
    def main_menu(self):
        while True:
            print("\nTASK MANAGER MENU")
            print("1. View Tasks (Linked List - sequential)")
            print("2. View Tasks (BST - ordered by ID)")
            print("3. View Tasks (BST Preorder)")
            print("4. View Tasks (BST Postorder)")
            print("5. View Tasks (Hash Table - all)")
            print("6. Add Task")
            print("7. Search Task by ID")
            print("8. Mark Task Completed")
            print("9. Delete Task")
            print("10. Save to File")
            print("11. Reload from File (discard unsaved changes)")
            print("12. Exit")

            choice = input("Select (1-12): ").strip()

            if choice == '1':
                self.view_tasks_linkedlist()
            elif choice == '2':
                self.view_tasks_bst_ordered()
            elif choice == '3':
                self.view_tasks_bst_preorder()
            elif choice == '4':
                self.view_tasks_bst_postorder()   
            elif choice == '5':
                self.view_tasks_hashtable()
            elif choice == '6':
                self.add_task()
            elif choice == '7':
                self.search_task()
            elif choice == '8':
                self.mark_completed()
            elif choice == '9':
                self.delete_task()
            elif choice == '10':
                self.save_file()
            elif choice == '11':
                confirm = input("This will discard unsaved changes. Continue? (y/n): ").lower()
                if confirm == 'y':
                    self.load_data()
                    print("Reloaded from file (or defaults if no file).")
                else:
                    print("Reload canceled.")
            elif choice == '12':
                print("Exiting...")
                break
            else:
                print("Try again.")

#---------------- Run the program---------------------

if __name__ == "__main__":
    app = TaskScheduler()
    app.main_menu()
