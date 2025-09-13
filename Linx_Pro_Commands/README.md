                                             # Linux Text Processing Commands for DevOps Engineers



---

## Table of Contents

* [Quick Start / Cheatsheet](#quick-start--cheatsheet)
* [grep – Pattern Searching](#grep--pattern-searching)
* [awk – Text Processing](#awk--text-processing)
* [sed – Stream Editing](#sed--stream-editing)
* [Other Useful DevOps Commands](#other-useful-devops-commands)
* [awk vs sed](#awk-vs-sed)
* [Frequently Used One-Liners](#frequently-used-one-liners)

---

## Quick Start / Cheatsheet

```bash
# grep
grep -i error app.log             # Case-insensitive search
grep -i -c info app.log           # Count INFO entries

# awk
awk '{print $1, $2}' app.log      # Show first two fields
awk '/INFO/ {count++} END {print count}' app.log

# sed
sed 's/INFO/LOG/g' app.log        # Replace INFO with LOG
sed -n '/DEBUG/p' app.log         # Print only DEBUG lines

grep – Pattern Searching
What

Search plain text for lines matching a pattern.

grep INFO app.log                   # Find INFO
grep -i info app.log                # Case-insensitive search
grep -i -c info app.log             # Count matches
ps aux | grep ubuntu                # Find ubuntu processes
ps aux | grep ubuntu | awk '{print $2}'   # Extract process IDs
ps aux | grep ubuntu | awk '{print $3}'   # Extract CPU usage

awk – Text Processing
What

Field-based text extraction and reporting.

awk '{print}' app.log                         # Print all lines
awk '{print $1, $2}' app.log                  # Show columns 1 & 2
awk '{print $1, $2, $5}' app.log              # Show columns 1, 2, 5
awk '/INFO/ {print $1, $2, $5}' app.log       # Extract INFO logs
awk '/DEBUG/ {print $1, $2, $3, $5}' app.log  # Extract DEBUG logs
awk '/INFO/ {count++} END {print count}' app.log
awk '/INFO/ {count++} END {print "Count of INFO:", count}' app.log
awk '$2 >= "07:51:54" && $2 <= "07:52:01" {print $2,$3,$4}' app.log
awk 'NR >=2 && NR <=10 {print NR}' app.log

sed – Stream Editing
What

Modify and transform text line by line.

sed -n '/INFO/p' app.log               # Show lines with INFO
sed 's/INFO/LOG/g' app.log             # Replace INFO with LOG
sed -n -e '/INFO/=' app.log            # Show line numbers
sed -n -e '/INFO/=' -e '/INFO/p' app.log
sed '1,10 s/INFO/LOG/g' app.log        # Replace in lines 1–10 only
sed '1,10 s/INFO/LOG/g; 1,10p; 11q' app.log

Other Useful DevOps Commands

head -n 5 app.log        # First 5 lines
tail -n 10 app.log       # Last 10 lines
tail -f app.log          # Live stream logs
sort app.log | uniq -c   # Unique entries with counts
cut -d' ' -f1 app.log    # Extract first field
df -h                    # Disk usage
du -sh /var/log          # Directory size
top                      # CPU/memory monitor

awk vs sed
Feature	awk	sed
Purpose	Field/column extraction & reporting	Line-by-line text editing
Works Best	Structured text (logs, CSV, tables)	Raw streams or config files
Output	Reports, counters, filtered fields	Substituted or filtered text
DevOps Use	Log parsing, analytics, monitoring	Config edits, cleanup, quick fixes
Frequently Used One-Liners

# Count INFO logs in real time
tail -f app.log | grep INFO | awk '{print $2}'

# Extract process IDs of ubuntu user
ps aux | grep ubuntu | awk '{print $2}'

# Replace DEBUG with TRACE and show first 20 lines
sed 's/DEBUG/TRACE/g' app.log | head -n 20

License