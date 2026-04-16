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
```

## Write a simple groovy pipeline for java spring boot app that waits for user input for approvals to move the next stage, with stages for checkout, build, push & deploy.  

pipeline {  
 agent any  
 
 stages {  
  stage('Checkout') {  
    steps {  
      git 'https://github.com.git'  
      }  
}  
 stage('Build') {  
  steps{  
    sh '/mvn clean package'  
    }  
}   
stage('push')  
  steps{  
    sh'docker build t docker-repo/java-sp-boot'  
    sh'docker push docker-repo/java-sp-boot'  
    }  
}  
stage(Approval'){  
steps{  
 input'Do you want to deploy to prod?'  
 }  
}  
stage(Deploy){  
 steps{  
   sh'kubectl apply -f deployment.yaml'  
   }  
}  
}  
}  
