# Algorithm for File Updates in Python

*Automate Cybersecurity Tasks with Python*

---

## Project Description

This project demonstrates a Python algorithm developed to automate the management of an IP address allow list for a healthcare organisation's restricted subnetwork. As a security analyst, maintaining accurate access control records is a core responsibility: employees whose roles no longer require access to patient data must be removed from the allow list promptly to uphold the principle of least privilege.

The algorithm reads a text file named `allow_list.txt` that stores permitted IP addresses, cross-references those addresses against a `remove_list` of revoked entries, deletes any matches, and writes the updated list back to the same file. By encapsulating the entire process inside a reusable function, the solution can be called at any time with a different file or a different remove list, making it adaptable to evolving access-control requirements without duplicating code.

The algorithm makes use of Python's built-in file I/O mechanisms (`open()`, `.read()`, `.write()`), list methods (`.split()`, `.remove()`, `.join()`), and control-flow structures (`for` loop, `if` statement) to complete the task efficiently and with minimal external dependencies.

---

## Step 1: Open the File that Contains the Allow List

The first step is to assign the filename to a variable and open the file safely using a `with` statement.

```python
# Assign 'import_file' to the name of the file
import_file = "allow_list.txt"

# Assign 'remove_list' to a list of IP addresses to be removed
remove_list = ["192.168.97.225", "192.168.158.170", "192.168.201.40", "192.168.58.57"]

# Open the file in read mode
with open(import_file, "r") as file:
```

### Syntax and Keywords Explained

- **`import_file`** — a string variable storing the filename `"allow_list.txt"`. Storing the filename in a variable means it only needs to be updated in one place if the file path ever changes.
- **`remove_list`** — a Python list literal containing the four IP addresses that must be revoked. Lists are mutable sequences, which is important later when the `.remove()` method is called.
- **`with open(import_file, "r") as file:`** — the `with` keyword creates a context manager that automatically closes the file object when the indented block exits, even if an exception is raised. `open()` is Python's built-in function for file access; the second argument `"r"` specifies read-only mode. The `as file` clause binds the file object to the name `file` for use within the block.

---

## Step 2: Read the File Contents

Inside the `with` block, the `.read()` method is used to load the entire file into memory as a single string.

```python
with open(import_file, "r") as file:
    # Read the file contents and store as a string
    ip_addresses = file.read()
```

### Syntax and Keywords Explained

- **`file.read()`** — reads the complete contents of the file and returns them as a string, assigned to `ip_addresses`. At this stage, the variable holds a long string where each IP address is separated by whitespace (spaces or newlines). The string cannot be iterated over address by address until it is converted into a list, which happens in the next step.

---

## Step 3: Convert the String into a List

Before individual IP addresses can be checked or removed, the raw string must be split into discrete elements.

```python
# Convert string to a list using .split()
ip_addresses = ip_addresses.split()
```

### Syntax and Keywords Explained

- **`ip_addresses.split()`** — called with no arguments, splits the string on any whitespace character (spaces, tabs, or newlines) and returns a Python list of the resulting substrings. It also collapses consecutive whitespace, producing a clean list with no empty-string elements. The return value is reassigned back to `ip_addresses`, changing its type from `str` to `list`. This list structure is required because the subsequent `for` loop must iterate over individual addresses, and `.remove()` only works on list objects.

---

## Step 4: Iterate Through the Remove List

A `for` loop is used to visit each IP address in the allow list so it can be evaluated against the remove list.

```python
# Iterate through ip_addresses
for element in ip_addresses:
    print(element)
```

### Syntax and Keywords Explained

- **`for element in ip_addresses:`** — a definite iteration statement. The `for` keyword signals the start of the loop. `element` is the loop variable — on each iteration it is automatically bound to the next item in the `ip_addresses` list. The `in` keyword connects the loop variable to the iterable. The colon marks the end of the loop header, and all indented code below it forms the loop body, which executes once per element.

On each pass, `element` holds one IP address string (e.g. `"192.168.97.225"`), giving the algorithm access to each address individually for comparison against the remove list.

---

## Step 5: Remove IP Addresses that are on the Remove List

Inside the loop, a conditional statement checks whether the current element should be removed, and `.remove()` deletes it if so.

```python
for element in ip_addresses:
    # Check whether the element is in the remove list
    if element in remove_list:
        # Remove the element from ip_addresses
        ip_addresses.remove(element)
```

### Syntax and Keywords Explained

- **`if element in remove_list:`** — the `in` membership operator checks whether the value of `element` exists anywhere inside `remove_list`. If the condition evaluates to `True`, the indented body runs; otherwise it is skipped.
- **`ip_addresses.remove(element)`** — locates the first occurrence of its argument in the list and deletes it. Because the allow list contains no duplicate IP addresses, `.remove()` will always target exactly the address that triggered the condition, making this approach safe and predictable.

---

## Step 6: Update the File with the Revised List of IP Addresses

After all disallowed addresses have been removed from the in-memory list, the list is converted back into a string and written to the file, overwriting its previous contents.

```python
# Rejoin the list into a newline-separated string
ip_addresses = "\n".join(ip_addresses)

# Rewrite the file with the updated IP list
with open(import_file, "w") as file:
    file.write(ip_addresses)
```

### Syntax and Keywords Explained

- **`"\n".join(ip_addresses)`** — the `.join()` method concatenates every element of the list into a single string, placing a newline character between consecutive elements. This ensures each IP address occupies its own line in the output file, preserving the original readable format. The result is reassigned to `ip_addresses`, converting it back from `list` to `str`.
- **`with open(import_file, "w") as file:`** — opens the same file in write mode (`"w"`), which truncates the file to zero length before writing begins, ensuring no stale IP addresses remain.
- **`file.write(ip_addresses)`** — writes the string value of `ip_addresses` to the open file. Once the `with` block exits, the context manager automatically flushes the buffer and closes the file.

---

## Complete Algorithm

The six steps above are combined into a single reusable function, `update_file()`, which accepts the filename and the remove list as parameters.

```python
def update_file(import_file, remove_list):

    # Read the current contents of the file
    with open(import_file, "r") as file:
        ip_addresses = file.read()

    # Convert the string into a list
    ip_addresses = ip_addresses.split()

    # Remove disallowed IP addresses
    for element in ip_addresses:
        if element in remove_list:
            ip_addresses.remove(element)

    # Convert the list back into a newline-separated string
    ip_addresses = "\n".join(ip_addresses)

    # Overwrite the file with the updated list
    with open(import_file, "w") as file:
        file.write(ip_addresses)


# Example call
update_file("allow_list.txt", ["192.168.97.225", "192.168.158.170",
                                "192.168.201.40", "192.168.58.57"])
```

Encapsulating the logic inside a function provides two key benefits: it avoids code duplication when the algorithm needs to be run multiple times, and it clearly communicates the algorithm's purpose and interface to any developer reading the code. The function can be called with any filename and any remove list, making it generalisable beyond this specific scenario.

---

## Summary

This algorithm automates a critical access-control maintenance task: keeping a healthcare organisation's IP address allow list accurate and up to date. The solution reads a text file of permitted IP addresses, converts the contents into a Python list for element-wise processing, iterates through every address, and removes any that appear on the revocation list. The cleaned list is then serialised back into a newline-separated string and written to the original file, replacing its outdated contents.

**Key Python techniques demonstrated:**

- `with` statements for safe file handling
- `open()` with `"r"` and `"w"` mode arguments for reading and writing
- `.read()` and `.write()` methods for file I/O
- `.split()` for string-to-list conversion
- `for` loop combined with `if` / `in` conditional for membership-based filtering
- `.remove()` for in-place list mutation
- `.join()` for list-to-string serialisation

Wrapping the entire process in a reusable function makes the algorithm maintainable and scalable: the filename or the remove list can be updated at the call site without modifying the function body. This reflects a core software-engineering principle — writing code that is easy to change — which is especially important in a security context where access-control policies evolve frequently.

---
