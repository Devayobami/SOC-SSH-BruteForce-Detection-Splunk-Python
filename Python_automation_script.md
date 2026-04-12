# Step 1: Open and read log file
with open("Linux_2k.log", "r", encoding="utf-8") as file:
    logs = file.readlines()

    # Step 2: Focus on lines 200-500
    subset_logs = logs[199:500] #Python is 0-based index

    # Step 3: Search for suspicious patterns
    suspicious_entries =[]
    for line in subset_logs:
        if "failed password" in line:
            suspicious_entries.append(("Failed login", line.strip()))
        elif "authentication failure" in line:
            suspicious_entries.append(("Auth Failure", line.strip()))
        elif "user unknown" in line:
            suspicious_entries.append(("unknown user", line.strip()))

            # Step 4: Print Results 

            print("=== Suspicious Log Entries (Lines 200-500) ===")
            for entry_type, entry in suspicious_entries:
                print(f"[{entry_type}] {entry}")

                print(f"\Total  suspicious   entries   found {len(suspicious_entries)}")

                # Step 5: Save results to a CSV file

                import csv
                with open("suspicious_logs.csv", "w", newline="") as csvfile:
                          writer = csv.writer(csvfile)
                          writer.writerow(["type", "Log Entry"])
                          writer.writerows(suspicious_entries)
                          print("Results saved to suspicious_log.csv")
