# Controlling File & Directory Permissions

As I work more with Linux, I quickly realize that permissions are one of the core security layers of the system. They determine who can read, write, or execute files, and understanding them makes troubleshooting and system management much easier. In this section, I break down how permissions work, how to read them, and how to manage them using tools like `chmod` and `chown`.

## Understanding Permission Structure

Every file and directory has three permission groups:
- **Owner** (the user who owns the file)
- **Group** (a group of users)
- **Others** (everyone else)

Each group can have:
- **r** — read  
- **w** — write  
- **x** — execute  

When I run:
ls -l

I see something like:
-rw-r--r--

Here’s how I read it:
- `-` means it’s a file (`d` would mean directory)
- `rw-` → owner can read and write
- `r--` → group can read
- `r--` → others can read

This tells me exactly who can do what.

## Permissions on Directories

Directories behave differently:
- **r** → list contents  
- **w** → create or delete files inside  
- **x** → enter the directory  

A directory without **x** is basically locked — you can see it but not enter it.

## Checking Ownership

To see who owns a file:
ls -l file.txt

The output shows:
- the owner  
- the group  
- the permissions  

To see who I am:
whoami

To see my groups:
groups

This helps me understand why I can or can’t access something.

## Changing Ownership with `chown`

Sometimes a file needs to belong to a different user or group. That’s where `chown` comes in.

Change the owner:
sudo chown newuser file.txt

Change owner and group:
sudo chown newuser:newgroup file.txt

Change ownership of a directory and everything inside it:
sudo chown -R newuser:newgroup directory/

I use `-R` when I’m preparing directories for services, scripts, or shared workspaces.

## Understanding Numeric Permissions (chmod numbers)

Permissions also have numeric values:
- r = 4  
- w = 2  
- x = 1  

Add them together for each group:
- rwx = 7  
- rw- = 6  
- r-- = 4  
- --- = 0  

So:
-rwxr-xr-x  
becomes:
755

This is one of the most common permission sets for scripts and executables.

## Understanding Symbolic Permissions (chmod letters)

Symbolic permissions let me adjust specific parts without touching the rest.

Symbols:
- u = user  
- g = group  
- o = others  
- a = all  

Operations:
- + add  
- - remove  
- = set exactly  

Examples:
u+x  
g-w  
o=r  
a+rw  

Symbolic mode is great when I want to tweak one piece instead of rewriting everything.

## Changing Permissions with `chmod`

To give the owner execute permission:
chmod u+x script.sh

To remove write permission from group:
chmod g-w file.txt

To set permissions exactly using numbers:
chmod 644 file.txt   # owner read/write, everyone else read
chmod 755 script.sh  # owner rwx, others rx

To apply changes recursively:
chmod -R 755 directory/

I use recursive mode carefully — it can change a lot at once.

## Practical Permission Troubleshooting

Here’s how I typically solve permission issues.

If I can’t run a script:
chmod u+x script.sh

If another user needs access to a file:
sudo chown user file.txt

If a directory won’t let me enter:
chmod u+x directory

If I can enter but can’t create files:
chmod u+w directory

If a service can’t read its own config:
sudo chown serviceuser:servicegroup /etc/app/config

These small checks fix most permission problems quickly.

## Permissions on Directories in Practice

To inspect a directory’s permissions:
ls -ld directory

If I see:
drwxr-x---

I know:
- owner: full access  
- group: read + execute  
- others: no access  

If I want to give the group write access:
chmod g+w directory

If I want to give everyone execute access (so they can enter it):
chmod a+x directory

## Building Good Permission Habits

A few habits keep things secure and predictable:

- Give only the permissions needed — nothing more.
- Use groups for shared access instead of opening permissions to everyone.
- Avoid giving write access to “others.”
- Use numeric mode when you want precision.
- Use symbolic mode when you want safe, targeted adjustments.
- Use `-R` carefully — recursive changes can affect hundreds of files.

Once you understand how to read, interpret, and modify permissions, the entire Linux filesystem becomes easier to manage. You can diagnose access issues quickly, secure files properly, and control exactly who can interact with what.
