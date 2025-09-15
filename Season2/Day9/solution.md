# Season 2 Day 9 Challenge - Solution

---

## Theory Answers

### Question 1: How does the shell bridge applications and the kernel?

**My Answer:**
The shell acts like a middleman between me and the computer's core system. Here's what happens:

1. I type a command like `ls -l`
2. Shell receives my command and figures out what I want
3. Shell finds the `ls` program (usually in `/bin/ls`)
4. Shell starts the `ls` program and gives it my options (`-l`)
5. The `ls` program asks the kernel: "Please show me the files in this directory"
6. Kernel talks to the hard drive and gets the file information
7. Kernel sends the info back to `ls`, then `ls` shows it to me through the shell

It's like the shell is a translator - I speak human language (commands), and it translates that into computer language for the kernel.

### Question 2: Explain `2>&1` vs. `2> file` with an example

**My Answer:**
These are about where error messages go:

- `2> file` = Put error messages in a file, normal output still goes to screen
- `2>&1` = Put error messages wherever normal output is going

**Example I tested:**
```bash
# This command will show some files and give an error for "badfile"
ls /home badfile 2> errors.txt
# Result: I see /home files on screen, but error goes to errors.txt

# This puts EVERYTHING in one place
ls /home badfile > everything.txt 2>&1
# Result: Both file list AND error message go to everything.txt
```

### Question 3: Why does `myscript` fail but `./myscript` work?

**My Answer:**
When I type just `myscript`, the computer only looks in special system folders (like `/bin`, `/usr/bin`) that are listed in the PATH variable. My current folder is not in PATH for security reasons.

When I type `./myscript`, I'm telling the computer "look in THIS folder (the dot means current folder) for the script."

The security reason: If current folder was in PATH, I might accidentally run a malicious program someone put there.

### Question 4: How does `history -c; history -w` differ from `history -c` alone?

**My Answer:**
- `history -c` = Clear the command history from memory (temporary)
- `history -w` = Write current memory history to the file
- Together `history -c; history -w` = Clear memory AND clear the file permanently

If I just use `history -c`, the history comes back when I start a new terminal because the file still has the old commands. The combined command makes sure both memory and file are cleared.

### Question 5: Why is `/etc/motd` useful for admins but not GUI users?

**My Answer:**
`/etc/motd` (Message of the Day) shows up when someone logs in through SSH or terminal. 

- **For admins:** Perfect for telling SSH users about maintenance, warnings, or system updates
- **For GUI users:** They never see it because they log in through the graphical interface, not terminal

It's like having a bulletin board at the terminal entrance - only terminal users see it.

---

## Part 2: Practical Tasks - My Results

### Overview
I successfully connected to azure ubuntu virtual machine. Here are my detailed results:

### Ubuntu Instance Results

**Connection Command Used:**
```bash
ssh -i mykey.pem ubuntu@[IP-ADDRESS]
```

**Commands Executed and Results:**

1. **`type ls`**
   - Result: `ls is /bin/ls`
   - This shows `ls` is an external program, not an alias

2. **`ls -l /etc > out.txt`**
   - Created file with directory listing
   - No output to screen (redirected to file)

3. **`ls nofile 2> err.txt`** 
   - Created error file
   - Error message: `ls: cannot access 'nofile': No such file or directory`

4. **`cat out.txt err.txt > combined.txt 2>&1`**
   - Successfully merged both files

5. **`echo "Host: $(hostname)" >> combined.txt`**
   - Added hostname to end of file

6. **`ls -R / | grep "bin" | less`**
   - Showed many directories and files containing "bin"
   - Used 'q' to exit

7. **`history -d 2`**
   - Deleted the second command from history

**Final combined.txt contents:**
```
!(Azure-ubuntu1.png)
```

**Screenshot:** !(azure-ubuntu2.png)

## Part 2: RHEL VM on Oracle virtual Box Scripting Task - My Results

### Task Overview
Successfully created and ran a monitoring script using Vim on the RHEL virtual machine.

### Step-by-Step Process:

1. **Logged into RHEL VM**
   - Started VM and logged in successfully
   
2. **Created script with Vim:**
```bash
vim ~/monitor.sh
```

3. **Script Content Created:**
```bash
#!/bin/bash
REPORT="sys_$(date +%Y%m%d).txt"
echo "Check - $(date)" > "$REPORT"
echo "User: $USER" >> "$REPORT"
uptime -p | tee -a "$REPORT"
df -h / | tail -n 1 >> "$REPORT" 2>/dev/null
```
4. **Made Script Executable:**
```bash
chmod +x monitor.sh
```

5. **Ran Script:**
```bash
./monitor.sh
```

6. **Checked Results:**
```bash
cat sys_250915.txt
```

**Script Output:**
```
!(Oraclevm.png)
!(oracle2.png)
```

8. **Created Alias:**
```bash
echo "alias m='./monitor.sh'" >> ~/.bashrc
source ~/.bashrc
```

9. **Tested Alias:**
```bash
m
```
