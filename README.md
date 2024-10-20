# python code
import time
from datetime import datetime as dt
import os

# Variables
ip_localmachine = "127.0.0.1"
website_list = ["instagram.com", "spankbang.com"]

# Path to hosts file (This should be the actual hosts file, not a Desktop file)
hosts_path = r"C:\Windows\System32\drivers\etc\hosts"  # For Windows

# Verify if the file exists
if not os.path.exists(hosts_path):
    print("Hosts file path is incorrect!")
    exit()

start_time = dt.strptime("15:00:00", "%H:%M:%S").time()
end_time = dt.strptime("16:00:00", "%H:%M:%S").time()

while True:
    try:
        # Get current time
        now = dt.now()
        current_time = now.time()

        if start_time <= current_time <= end_time:
            print("Working hours")
            with open(hosts_path, "r+") as file:
                content = file.read()
                for website in website_list:
                    if website not in content:
                        file.write(ip_localmachine + " " + website + "\n")
        else:
            print("Non-working hours")
            with open(hosts_path, "r+") as file:
                content = file.readlines()
                file.seek(0)
                for line in content:
                    # Only write back lines that don't contain blocked websites
                    if not any(website in line for website in website_list):
                        file.write(line)
                # Truncate the file to remove the unwanted lines at the end
                file.truncate()
        
    except Exception as e:
        print(f"An error occurred: {e}")
    
    # Sleep for a while before checking again
    time.sleep(10)
