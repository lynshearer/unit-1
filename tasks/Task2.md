# Task 2: EV Calculator

```.py
from my_library import validate_int_input
end_code = "\033[00m"
colors = ["\033[0;32m", "\33[0;31m"]
bold_green = "\33[1;32m"
welcome_msg = "Welcome to the EV calculator".center(50, "=")
prompt_msg = "Please enter an option [1-4]: "

print(f"{colors[0]}{welcome_msg}{end_code}")
print("Options".center(50))

menu = """1. Average time per kWh
2. Total kWh
3. Total charge time
4. Show all data
"""

print(menu)
option = validate_int_input(prompt_msg)
while option > 4 or option < 1:
    option = validate_int_input((f"Invalid option. {colors[1]}{prompt_msg}{end_code}"))
with open("charging_log.csv", "r") as file:
    ev_logs = file.readlines()

#option 1: average time per kWh
if option == 1:
    average_time = 0
    total_energy = 0
    index = 0
    for log in ev_logs:
        if index > 0:
            data = log.split(",")
            time = data[2]
            time = time.split(":")
            time = int(time[0]) * 60 + int(time[1])
            average_time += time
            kWh = data[1]
            total_kwh = float(kWh[0:5])
        index += 1
    print(f"The average time per kwh is {int(average_time / total_kwh)} minutes.")

#option 2: calculate total energy
if option == 2:
    index = 0
    total_energy = 0
    for log in ev_logs:
        if index >= 0:
            values = log.split(",")
            date = values[0]
            energy = values[1]
            time = values[2]
            total_energy += float(energy[0:5])
        index += 1
    print(f"{colors[0]}The total energy charged is {total_energy}kWh")

#option 3: Total charge time
if option == 3:
    total_time = 0
    index = 0
    for log in ev_logs:
        if index > 0:
            data = log.split(",")
            time = data[2]
            time = time.split(":")
            time = int(time[0]) * 60 + int(time[1])
            total_time += time
        index += 1
    print(f"{total_time // 60}{total_time % 60} minutes")

#option 4: show all data
with open("charging_log.csv", "r") as file:
    ev_logs = file.readlines()
if option  == 4:
    print(f"{bold_green}4. Showing all data{end_code}")
    index = 0
    for log in ev_logs:
        if index >= 0:
            print(f"No.{index}: {log}", end="")
        index += 1
```
