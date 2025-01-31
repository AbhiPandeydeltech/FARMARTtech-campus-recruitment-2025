

### Solutions Approach

#### **1. My Solution / Naive Solution**

- For each query, read the entire file (1 TB) line by line.  
- Match lines that start with the requested date (YYYY-MM-DD).  
- Write matching lines to the output.

##### **Pros**

- Very simple to implement.

##### **Cons**

- Highly inefficient for large files (1 TB).  
- Repeatedly scanning the file is time-consuming.



#### **2. Index-Based Approach (Preprocessing + Random Access)**

**a. Index Creation (Preprocessing Phase)**

- **Single Pass Through the Log File**: We iterate through the log file once, reading it line by line.
- **Extract Date**: From each log line, the date (formatted as `YYYY-MM-DD`) is extracted from the beginning of the line.
- **Record Offsets**: The position (or byte offset) of each date in the file is recorded.
- **Store in Index File**: This data is saved in a separate index file. Each line of the index file is formatted as:
  - `YYYY-MM-DD, byte_offset`
  - For example, `2024-12-01, 12345` means the log entries for `2024-12-01` begin at byte offset `12345` in the log file.

**b. Log Extraction for a Specific Date**

- **Lookup in Index**: To retrieve logs for a specific date, the index file is checked to find the byte offset corresponding to the requested date.
- **Direct File Access**: The program then accesses the log file directly at that offset, skipping all the irrelevant data.
- **Read Logs**: Once at the correct position, it reads the logs for that date until it encounters the next date or reaches the end of the file.

**Pros**
- **Quick Querying**: Once the index is created, fetching logs for a specific date becomes very fast. The system only needs to read the relevant logs for that day, eliminating the need to scan the entire file.

**Cons**
- **Initial Processing Time**: Building the index requires an initial pass through the entire log file, which can take time based on the file size.
- **Additional Storage Required**: The index file, which stores the byte offsets, requires extra storage. While it’s much smaller than the log file itself, it still adds some overhead.
