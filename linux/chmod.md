# chmod Command Explained

## 1. File Permissions Overview
Each file and directory in Linux has associated **permissions** that determine which users can read, write, or execute them. These permissions are divided into three categories:

| Category | Symbol | Description |
|----------|--------|-------------|
| **Owner** | `u` | The user who owns the file |
| **Group** | `g` | Users in the file's group |
| **Others** | `o` | All other users |

Each category has three permission types:

| Permission | Symbol | Numeric Value | Description |
|------------|--------|---------------|-------------|
| **Read** | `r` | `4` | View the contents of a file/directory |
| **Write** | `w` | `2` | Modify or delete a file/directory |
| **Execute** | `x` | `1` | Run a file (if it's a script or program) or enter a directory |

## 2. Syntax
```bash
chmod [options] mode file
```
- **`mode`** – Specifies the new permissions in symbolic (`rwx`) or numeric (`777`) format.
- **`file`** – The file or directory whose permissions will be changed.

## 3. Using chmod (Symbolic Mode)
You can modify permissions using letters (`u`, `g`, `o`, `a`) and symbols (`+`, `-`, `=`):

| Symbol | Meaning |
|--------|---------|
| `+` | Adds a permission |
| `-` | Removes a permission |
| `=` | Sets an exact permission |

### Examples:
```bash
chmod u+x file.sh  # Give execute permission to the owner
chmod g-w file.txt  # Remove write permission from the group
chmod o+rx file.sh  # Give read and execute permissions to others
chmod u=rwx,g=,o= file.txt  # Owner full, no permission for others
chmod a+rwx file.txt  # Full permissions to everyone
```

## 4. Using chmod (Numeric Mode)
Each permission type has a numeric value:
- `r` (Read) = `4`
- `w` (Write) = `2`
- `x` (Execute) = `1`

Combine values to set permissions:
```bash
chmod 755 file.sh  # Owner rwx, group r-x, others r-x
chmod 777 file.txt  # Full permissions for everyone
chmod 664 file.txt  # Owner & group rw-, others r--
chmod 111 file.sh  # Execute-only for everyone
```

## 5. Special Permissions (setuid, setgid, sticky bit)
| Special Permission | Symbol | Numeric Value | Description |
|--------------------|--------|---------------|-------------|
| **SetUID** | `s` (on `u`) | `4` | Run file as the owner |
| **SetGID** | `s` (on `g`) | `2` | Files run as group, directories inherit group |
| **Sticky Bit** | `t` (on `o`) | `1` | Prevents deletion by non-owners |

### Examples:
```bash
chmod 4755 script.sh  # SetUID
chmod 2755 /shared_folder  # SetGID
chmod 1777 /tmp  # Sticky bit
```

## 6. Changing Permissions Recursively (-R)
```bash
chmod -R 755 /my_directory  # Change all contents
```

## 7. Checking File Permissions
```bash
ls -l file.txt
```
Example output:
```
-rw-r--r--  1 user group  1234 Mar 20 12:34 file.txt
```

## 8. Combining chmod with find
```bash
find /path -type f -name "*.sh" -exec chmod 755 {} \;
find /path -type d -exec chmod 755 {} \;
```

## 9. Important Notes
- **Only the file owner or root user can change permissions** using `chmod`.
- **Use `777` with caution**, as it gives full access to everyone.
- **Use `-R` carefully**, as it modifies all subdirectories and files.
