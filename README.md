import tkinter as tk
class Node:
    def __init__(self, key):
        self.left = None
        self.right = None
        self.val = key

class BSTVisualizer:
    def __init__(self, root):
        self.root = root
        self.window = tk.Tk()
        self.window.title("Binary Search Tree Visualization")
        self.canvas = tk.Canvas(self.window, width=800, height=600, bg="white")
        self.canvas.pack()
        self.node_radius = 20
        self.horizontal_gap = 50
        self.update_tree()

        # Input fields and buttons
        self.entry = tk.Entry(self.window)
        self.entry.pack()
        self.insert_button = tk.Button(self.window, text="Insert", command=self.insert_node)
        self.insert_button.pack()
        self.delete_button = tk.Button(self.window, text="Delete", command=self.delete_node)
        self.delete_button.pack()

        self.window.mainloop()

    def insert(self, root, key):
        if root is None:
            return Node(key)
        elif key < root.val:
            root.left = self.insert(root.left, key)
        else:
            root.right = self.insert(root.right, key)
        return root

    def delete(self, root, key):
        if root is None:
            return root
        if key < root.val:
            root.left = self.delete(root.left, key)
        elif key > root.val:
            root.right = self.delete(root.right, key)
        else:
            if root.left is None:
                return root.right
            elif root.right is None:
                return root.left
            temp_val = self.min_value_node(root.right)
            root.val = temp_val.val
            root.right = self.delete(root.right, temp_val.val)
        return root

    def min_value_node(self, node):
        current = node
        while current.left is not None:
            current = current.left
        return current

    def update_tree(self):
        self.canvas.delete("all")
        self.draw_tree(self.root, 400, 50, 200)

    def draw_tree(self, node, x, y, x_gap):
        if node is not None:
            if node.left:
                self.canvas.create_line(x, y, x - x_gap, y + 80)
                self.draw_tree(node.left, x - x_gap, y + 80, x_gap // 2)
            if node.right:
                self.canvas.create_line(x, y, x + x_gap, y + 80)
                self.draw_tree(node.right, x + x_gap, y + 80, x_gap // 2)
            self.draw_node(x, y, node.val)

    def draw_node(self, x, y, value):
        self.canvas.create_oval(x - self.node_radius, y - self.node_radius, 
                                x + self.node_radius, y + self.node_radius, fill="lightblue")
        self.canvas.create_text(x, y, text=str(value), font=("Arial", 12))

    def insert_node(self):
        try:
            key = int(self.entry.get())
            self.root = self.insert(self.root, key)
            self.update_tree()
        except ValueError:
            pass

    def delete_node(self):
        try:
            key = int(self.entry.get())
            self.root = self.delete(self.root, key)
            self.update_tree()
        except ValueError:
            pass

if __name__ == "__main__":
    root = None
    BSTVisualizer(root)
