## Write a python file to backup all files older than 30 days from a directory.

```python
import os
import shutil
import time

# Configuration
SOURCE_DIR = "/path/to/source"      # Directory to check
BACKUP_DIR = "/path/to/backup"      # Directory to store backups
DAYS_OLD = 30                        # Files older than 30 days

# Convert days to seconds
time_threshold = time.time() - (DAYS_OLD * 24 * 60 * 60)

# Ensure backup directory exists
os.makedirs(BACKUP_DIR, exist_ok=True)

# Iterate through files in the source directory
for filename in os.listdir(SOURCE_DIR):
    file_path = os.path.join(SOURCE_DIR, filename)
    
    # Skip directories, only process files
    if os.path.isfile(file_path):
        file_mtime = os.path.getmtime(file_path)
        
        # Check if file is older than threshold
        if file_mtime < time_threshold:
            shutil.move(file_path, os.path.join(BACKUP_DIR, filename))
            print(f"Backed up: {filename}")

print("Backup completed!")
