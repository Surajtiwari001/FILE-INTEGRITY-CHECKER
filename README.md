# FILE-INTEGRITY-CHECKER
FILE INTEGRITY CHECKER

**COMPANY** : CODTECH IT -SOLUTION

**NAME**  :

**INTERN ID** :

**DOMAIN** : 

**BATCH DURATION** :

**MENTOR NAME** :

# A PYTHON SCRIPTUSING LIBRARIES LIKE HASHLIB TO ENSURE FILE INTEGRITY.


THIS IS CODE FOR CHEACKING THE FILE-INTEGRITY-CHECKER.
CODE :-

import hashlib
import os
import json
import time
def calculate_file_hash(file_path):
 sha256_hash = hashlib.sha256()
 with open(file_path, "rb") as f:
 for byte_block in iter(lambda: f.read(4096), b""):
 sha256_hash.update(byte_block)
 return sha256_hash.hexdigest()
def load_hash_database(db_file):
 if os.path.exists(db_file):
 with open(db_file, "r") as f:
 return json.load(f)
 return {}
 
def save_hash_database(db_file, hash_db):
 with open(db_file, "w") as f:
 json.dump(hash_db, f, indent=4)
def monitor_files(directory, db_file):
 hash_db = load_hash_database(db_file)
 
 while True:
 for root, _, files in os.walk(directory):
 for file in files:
 file_path = os.path.join(root, file)
 current_hash = calculate_file_hash(file_path)
 
 if file_path in hash_db:
 if hash_db[file_path] != current_hash:
 print(f"File changed: {file_path}")
 hash_db[file_path] = current_hash
 else:
 print(f"New file detected: {file_path}")
 hash_db[file_path] = current_hash
 
 save_hash_database(db_file, hash_db)
 time.sleep(10) # Check every 10 seconds
if __name__ == "__main__":
 directory_to_monitor = "path/to/directory" # Replace with t
 hash_database_file = "hash_database.json"
 
 print(f"Monitoring directory: {directory_to_monitor}")
 print("Press Ctrl+C to stop monitoring")
 
 try:
 monitor_files(directory_to_monitor, hash_database_file)
 except KeyboardInterrupt:
 print("\nMonitoring stopped")
