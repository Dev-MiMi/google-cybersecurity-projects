# File Permissions in Linux

*Cybersecurity Portfolio Activity*

---

## Project Description

As a security professional at a large organization, I was tasked with examining and managing file permissions on the Linux file system for the research team. The goal was to ensure that users had appropriate access to files and directories — no more, no less — in alignment with the organization's authorization policies.

Using Linux Bash shell commands, I reviewed existing permissions on files and subdirectories within the `/home/researcher2/projects` directory, identified any misconfigurations, and applied corrective changes using the `chmod` command. This work helps protect sensitive data and maintains the principle of least privilege across the system.

---

## Task 1: Check File and Directory Details

### Command Used

To navigate to the `projects` directory and list all file permissions (including hidden files):

```bash
cd projects
ls -la
```

> **Note:** The `-la` flag combines `-l` (long format) and `-a` (all files, including hidden). Hidden files begin with a period (`.`), such as `.project_x.txt`.

### Output / Current Permissions

```
drwx--x---  2 researcher2 research_team 4096 Mar 14 18:40 drafts
-rw-rw-rw-  1 researcher2 research_team   46 Mar 14 18:40 project_k.txt
-rw-r-----  1 researcher2 research_team   46 Mar 14 18:40 project_m.txt
-rw-rw-r--  1 researcher2 research_team   46 Mar 14 18:40 project_r.txt
-rw-rw-r--  1 researcher2 research_team   46 Mar 14 18:40 project_t.txt
-rw--w----  1 researcher2 research_team   46 Mar 14 18:40 .project_x.txt
```

---

## Task 2: Describe the Permissions String

**Example:** The permissions string for `project_k.txt` is `-rw-rw-rw-`

| Position | Character | Meaning |
|----------|-----------|---------|
| 1 | `-` | Regular file (a `d` would indicate a directory) |
| 2–4 | `rw-` | **User (owner):** read and write, no execute |
| 5–7 | `rw-` | **Group:** read and write, no execute |
| 8–10 | `rw-` | **Other:** read and write — *this violates organizational policy* |

---

## Task 3: Change File Permissions

The organization does not allow `other` users to have write access to any files.  
`project_k.txt` was found to grant write permission to others (`-rw-rw-rw-`).  
Additionally, `project_m.txt` should not be readable or writable by the group.

### Commands Used

```bash
# Remove write permission for 'other'
chmod o-w project_k.txt

# Remove read permission for group
chmod g-r project_m.txt
```

---

## Task 4: Change File Permissions on a Hidden File

The hidden file `.project_x.txt` (identifiable by the leading period in its name) is archived and should not be writable by anyone, while remaining readable by both the user and group.  
The `ls -la` output revealed it had write permissions for both user and group (`-rw--w----`).

### Command Used

```bash
chmod u-w,g-w,g+r .project_x.txt
```

This removed write access for both user and group, while ensuring the group retains read access.

---

## Task 5: Change Directory Permissions

Only the `researcher2` user should be allowed to access the `drafts` subdirectory.  
The `ls -l` output showed the group had execute permissions (`drwx--x---`), meaning group members could access its contents.

### Command Used

```bash
chmod g-x drafts
```

This removed the execute permission for the group from the `drafts` directory, ensuring only `researcher2` can access it and its contents.

---

## Summary

In this activity, I demonstrated the use of Linux Bash shell commands to audit and manage file permissions within the `/home/researcher2/projects` directory.

Using `ls -la`, I examined all file and directory permissions, including hidden files (those starting with a period). I identified and corrected several permission misconfigurations using the `chmod` command:

- Removed write access for `other` on `project_k.txt`
- Removed group read access on the restricted `project_m.txt`
- Corrected write permissions on the hidden archived file `.project_x.txt`
- Removed group execute access on the `drafts` directory

These changes enforce the **principle of least privilege** and reduce the organization's security risk by ensuring users only have the access they are authorized to have.
