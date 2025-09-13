                                            # 🐧 Linux Command Practice for DevOps Engineers


DevOps engineers rely heavily on Linux shell commands for **log analysis, monitoring, automation, and troubleshooting**. 

---

## 📑 Table of Contents
1. [Introduction](#-introduction)  
2. [grep – Pattern Searching](#-grep--pattern-searching)  
3. [awk – Text Processing](#-awk--text-processing)  
4. [sed – Stream Editing](#-sed--stream-editing)  
5. [Other Useful DevOps Commands](#-other-useful-devops-commands)  
6. [Difference Between awk and sed](#️-difference-between-awk-and-sed)  
7. [Example Workflow](#-example-workflow)  
8. [Conclusion](#-conclusion)

---

## 📖 Introduction
In this project, we practiced Linux commands on a sample log file:

```bash
app.log

These commands help in:

Extracting useful information from logs.

Searching patterns for troubleshooting.

Automating replacements in config files.

Monitoring processes and resource usage.

🔍 grep – Pattern Searching

The grep command searches for patterns inside files or streams.

grep INFO app.log                  # Find "INFO" in logs
grep -i info app.log               # Case-insensitive search
grep -i -c info app.log            # Count matches
ps aux | grep ubuntu               # Search for processes
ps aux | grep ubuntu | awk '{print $2}'   # Extract process IDs
ps aux | grep ubuntu | awk '{print $3}'   # Extract CPU usage


💡 Use Cases for DevOps

Searching for error messages in logs.

Counting how many times an event occurred.

Filtering processes based on user or keyword.

🪶 awk – Text Processing

The awk command is perfect for column-based log/data extraction.

awk '{print}' app.log                         # Print all lines
awk '{print $1, $2}' app.log                  # Print first two columns
awk '{print $1, $2, $5}' app.log              # Extract specific fields
awk '/INFO/ {print $1, $2, $5}' app.log       # Extract INFO logs
awk '/DEBUG/ {print $1, $2, $3, $5}' app.log  # Extract DEBUG logs
awk '/INFO/ {count++} END {print count}' app.log  # Count INFO occurrences
awk '/INFO/ {count++} END {print "Count of INFO:", count}' app.log
awk '$2 >= "07:51:54" && $2 <= "07:52:01" {print $2,$3,$4}' app.log
awk 'NR >=2 && NR <=10 {print NR}' app.log


💡 Use Cases for DevOps

Extract timestamps, log levels, and messages.

Count number of errors or warnings.

Filter logs within a specific time range.

Process outputs from other Linux commands.

✂️ sed – Stream Editing

The sed command is mainly used for search/replace operations.

sed -n '/INFO/p' app.log            # Print lines with INFO
sed 's/INFO/LOG/g' app.log          # Replace INFO with LOG
sed -n -e '/INFO/=' app.log         # Print line numbers
sed -n -e '/INFO/=' -e '/INFO/p' app.log  # Print line numbers + matches
sed '1,10 s/INFO/LOG/g' app.log     # Replace INFO only in lines 1-10
sed '1,10 s/INFO/LOG/g; 1,10p; 11q' app.log


💡 Use Cases for DevOps

Replace values in configuration files.

Clean/format logs before analysis.

Extract sections of large files.

🛠️ Other Useful DevOps Commands

Beyond awk, sed, and grep, a DevOps engineer often uses:

head -n 5 app.log       # Show first 5 lines of a file
tail -n 10 app.log      # Show last 10 lines of a file
tail -f app.log         # Stream logs in real-time
sort app.log | uniq -c  # Count unique occurrences
cut -d' ' -f1 app.log   # Extract first field using space delimiter
df -h                   # Show disk usage
du -sh /var/log         # Show log directory size
top                     # Monitor CPU & memory usage


⚖️ Difference Between awk and sed:

| Feature      | **awk**                                  | **sed**                            |
| ------------ | ---------------------------------------- | ---------------------------------- |
| Purpose      | Data extraction & reporting              | Stream editing (find & replace)    |
| Works On     | Columns/fields in structured text        | Entire lines of text               |
| Output Style | Customizable, can generate reports       | Mostly simple text transformations |
| Best For     | Parsing logs, analytics, field-based ops | Text substitution, cleanup         |


🚀 Example Workflow

A DevOps engineer troubleshooting logs might run:

# Get all INFO logs, extract time and message, replace INFO with LOG
grep INFO app.log | awk '{print $2, $5}' | sed 's/INFO/LOG/g'


✅ Conclusion

This repository demonstrates how grep, awk, and sed — along with a few other commands — are essential in a DevOps engineer’s toolkit.

They help in:

Log analysis

Automation

Troubleshooting

Monitoring system health


