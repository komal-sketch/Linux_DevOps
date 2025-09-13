                                               # Linux Text‑Processing Commands for DevOps Engineers

Efficient log analysis and configuration management are essential in DevOps. This guide groups common text‑processing commands — grep, sed, awk and complementary tools — into logical sections with short explanations and examples. Use it as a quick reference when investigating logs, transforming files or automating one‑liners.

# Table of Contents

Quick Start / Cheatsheet

Install the Tooling (Ubuntu/Debian)

Pattern Matching & Filtering

Search & Replace (sed)

Field Extraction & Reports (awk)

Viewing & Summarising Files

Disk & Process Tools

Safety Notes

Frequently Used One‑Liners

Quick Start / Cheatsheet

The following examples assume a log file named app.log. Adjust file names and patterns as needed.

# Show lines containing ERROR

grep 'ERROR' app.log

# Show lines not containing DEBUG

grep -v 'DEBUG' app.log

# Replace INFO with LOG everywhere

sed 's/INFO/LOG/g' app.log

# Extract timestamp and level (fields 1 & 2)

awk '{print $1, $2}' app.log

# Count number of WARNING lines

awk '/WARNING/ {c++} END {print c}' app.log

# Follow the last 20 lines of a growing log

tail -f app.log | tail -n 20

Install the Tooling (Ubuntu/Debian)

These utilities are usually installed by default. If you need to install or update them, run:

sudo apt update
sudo apt install -y gawk sed grep coreutils


This installs GNU awk (gawk), sed, grep and the standard core utilities containing head, tail, sort, uniq and cut.

Pattern Matching & Filtering
grep

What: Scan files or streams for lines matching a regular expression and print them
man7.org
. It’s the go‑to tool when you need to quickly find patterns or exclude noise
baeldung.com
.

# Find all lines containing ERROR
grep 'ERROR' app.log

# Find lines matching a regex (case insensitive)
grep -i 'failed\|timeout' app.log

# Show 3 lines of context around each match
grep -C 3 'CRITICAL' app.log

# Exclude INFO lines (invert match)
grep -v 'INFO' app.log

awk as a filter

Awk is a full scripting language but can imitate grep by specifying only a pattern
baeldung.com
:

# Print lines containing ERROR (equivalent to grep)
awk '/ERROR/ {print}' app.log

sed as a filter

Sed can also act like grep by printing only matching lines
baeldung.com
:

# Print lines containing ERROR
sed -n '/ERROR/p' app.log


Use grep when you only need to match; use sed or awk when you plan to transform the output further.

Search & Replace (sed)

sed is a non‑interactive stream editor used for filtering and transforming text
man7.org
. It processes lines sequentially and applies editing commands to matching lines or ranges.
baeldung.com

Basic substitution
# Replace INFO with LOG globally on each line
sed 's/INFO/LOG/g' app.log

# Replace first occurrence of WARNING with WARN on each line
sed 's/WARNING/WARN/' app.log

Conditional substitution
# Replace INFO with LOG only on lines 1‑10
sed '1,10 s/INFO/LOG/g' app.log

# Replace ERROR with CRITICAL on lines containing ERROR
sed '/ERROR/ s/ERROR/CRITICAL/g' app.log

Display line numbers
# Print the line number of each INFO match
sed -n '/INFO/=' app.log

# Show line number and content together
sed -n -e '/INFO/=' -e '/INFO/p' app.log


Sed can also delete lines (d), insert or append text (i/a) and quit early (q). Use it to perform batch edits on configuration files or logs.

Field Extraction & Reports (awk)

awk reads records (usually lines), splits them into fields and executes actions when patterns match
man7.org
. Fields are referenced as $1, $2, …
man7.org
.

Printing columns
# Print entire line (same as cat)
awk '{print}' app.log

# Print first two fields (timestamp and level)
awk '{print $1, $2}' app.log

# Print fields 1, 2 and 5
awk '{print $1, $2, $5}' app.log

Filtering by content
# Show INFO messages (fields 1, 2, 5)
awk '/INFO/ {print $1, $2, $5}' app.log

# Show DEBUG messages (fields 1‑3 and 5)
awk '/DEBUG/ {print $1, $2, $3, $5}' app.log

Counting and summarising
# Count number of INFO lines
awk '/INFO/ {c++} END {print c}' app.log

# Count and label the result
awk '/INFO/ {c++} END {print "Count of INFO:", c}' app.log

# Summarise occurrences by log level (assumes level in field 2)
awk '{count[$2]++} END {for (level in count) printf "%s\t%s\n", level, count[level]}' app.log

Conditional selection
# Select lines where field 2 (time) is between 07:51:54 and 07:52:01
awk '$2 >= "07:51:54" && $2 <= "07:52:01" {print $0}' app.log

# Print line numbers for records 2 through 10
awk 'NR>=2 && NR<=10 {print NR, $0}' app.log


Use awk for structured log files (CSV/TSV) where you need to extract columns, compute aggregates or reformat data on the fly.

Viewing & Summarising Files
head & tail
head -n 5 app.log    # first 5 lines
tail -n 10 app.log    # last 10 lines
tail -f app.log       # follow a growing log file in real time

sort, uniq & cut
# Sort and count identical lines
sort app.log | uniq -c

# Extract first field (space‑delimited)
cut -d' ' -f1 app.log

# Extract second field using a comma delimiter
cut -d',' -f2 data.csv


These commands let you detect repeated errors (sort | uniq), extract timestamps or hostnames (cut), and summarise logs quickly.

Disk & Process Tools

Text‑processing often goes hand‑in‑hand with checking system health. These commands come from the coreutils package.

df -h            # human‑readable disk usage
du -sh /var/log  # size of the log directory
top              # real‑time CPU and memory usage


Use them to ensure that logs aren’t filling disks and to diagnose resource bottlenecks.

Safety Notes

When editing files in place with sed -i, always create a backup (-i.bak) to avoid data loss.

Regular expressions can be greedy; test them on a copy of your data before running destructive operations.

Long‑running commands (tail -f, awk loops) may consume resources; stop them when they are no longer needed.

Frequently Used One‑Liners
# Find number of distinct IP addresses in logs (assuming IP in field 3)
awk '{print $3}' app.log | sort | uniq | wc -l

# Show top 5 most common error messages
grep 'ERROR' app.log | sort | uniq -c | sort -nr | head -n 5

# Remove ANSI colour codes from log
sed -r 's/\x1B\[[0-9;]*[mK]//g' coloured.log

# Monitor when a specific pattern appears (prints timestamp and message)
tail -f app.log | awk '/CRITICAL/ {print strftime("%F %T"), $0}'


| aspect             | awk                                                                                                   | sed                                                                                                                                                  |
| ------------------ | ----------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Purpose**        | Field‑oriented pattern scanning and processing language                                               | Stream editor for filtering and transforming text                                                                                                    |
| **Input handling** | Treats each line as a record and splits it into fields; fields are accessed via `$1`, `$2`, etc.      | Operates on a continuous stream of characters; pattern space contains the current line, and operations can be restricted by line numbers or patterns |
| **Capabilities**   | Supports arithmetic, conditionals, loops and functions; can perform summarisation and complex reports | Provides substitution (`s`), deletion (`d`), insertion and line printing (`p`); works well for simple find‑and‑replace or line‑based edits           |
| **Typical use**    | Extract or manipulate columns, perform calculations, generate structured reports                      | Substitute patterns, insert or delete lines, apply bulk edits across a file                                                                          |
| **Learning curve** | Requires understanding of scripting constructs but offers more power                                  | Easier to learn for basic substitutions and line filtering                                                                                           |


| tool     | core capability                                            | DevOps use case examples                                                                                                                                                                                 |
| -------- | ---------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **grep** | Pattern matching; prints lines that match a regex          | Quickly check if errors or keywords exist in logs (`grep "ERROR" app.log`), invert matches to exclude noisy messages (`grep -v "DEBUG" app.log`), or display surrounding context with `-A`/`-B` options. |
| **sed**  | Stream editing — search, substitution and deletion         | Perform batch updates in configuration files, normalize log entries by replacing patterns, extract line numbers or restrict edits to certain lines.                                                      |
| **awk**  | Field‑oriented text processing with scripting capabilities | Parse log fields (timestamps, levels), aggregate counts, calculate statistics, or reformat text for ingestion into dashboards.                                                                           |


| command                   | description & use case                                                                                      |
| ------------------------- | ----------------------------------------------------------------------------------------------------------- |
| `head -n 5 app.log`       | Display the first five lines; useful for checking the beginning of a log or file header.                    |
| `tail -n 10 app.log`      | Display the last ten lines; often used to see the most recent log entries.                                  |
| `tail -f app.log`         | Continuously stream appended lines; ideal for real‑time monitoring of logs during deployments.              |
| `sort app.log \| uniq -c` | Sort and count unique lines; this combination helps identify how many times each identical log line occurs. |
| `cut -d' ' -f1 app.log`   | Use whitespace as the delimiter and extract the first field; helpful for pulling timestamps or hostnames.   |
| `df -h`                   | Report disk usage in human‑readable units.  DevOps teams monitor disk consumption to avoid outages.         |
| `du -sh /var/log`         | Show the total size of the `/var/log` directory; helps estimate log storage consumption.                    |
| `top`                     | Display real‑time system resource usage (CPU, memory, load).  Useful for diagnosing performance problems.   |
