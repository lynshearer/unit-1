```.py
colors = ["\033[0;32m", "\33[0;31m", "\33[1;30m", "\33[0;97m", "\33[0;35m", "\33[1;91m"]
end_code = "\033[00m"
with open("wallet.csv", "r") as file:
    wallet_log = file.readlines()

## Registration
def register(uname: str, password: str):
    '''
    db.csv
    :param uname: username a string
    :param password: password a string
    :return: nothing
    '''
    file = open("db.csv", "a")
    file.write(f"{uname},{password}\n")
new_user = input("Enter what you would like to be your username: ")
new_pass = input("Enter what you would like to be your password: ")
register(password=new_pass, uname=new_user)
print("Registration completed. Please continue to the digital ledger.")

## Login
def login(user, password)->bool:
    '''
    Function for a simple user login
    needs db.csv
    :param user: string
    :param password: string
    :return: true or false
    '''
    with open("db.csv") as file:
        database = file.readlines()
    output = False
    for line in database:
        clear_line = line.strip()
        separated_line = clear_line.split(",")
        if user == separated_line[0] and password == separated_line[1]:
            welcome_msg = "*:･ﾟ✧*:･ﾟ Welcome to your IOTA digital ledger *:･ﾟ✧*:･ﾟ".center(50)
            output = (f"{colors[2]}{welcome_msg}{end_code}")
        else:
            print(f"{colors[5]} Incorrect password. Please try again. {end_code}")
            exit()
    return output
user = input("Please enter your username: ")
password = input("Please enter your password: ")
test = login(user, password)
print(test)

## Return to the main menu
def restart():
    restart_option = input("""Would you like to continue browsing the ledger? 
    1: Yes
    2: No
    Input '1' or '2' here: """)
    if restart_option == "1":
        print("Menu Options".center(50))
        print(menu)
        Main()
    if restart_option == "2":
        print(f"{colors[5]} Logging out... {end_code}")
        exit()

## Start of the menu

from my_library import validate_int_input
prompt_msg = "Please enter an option [1-4]: "
print("Menu Options".center(50))

menu = """1. Information regarding IOTA and current conversion rate
2. Create a transaction
3. View past transactions
4. Overall transaction data
"""
print(menu)

def Main():
    option = validate_int_input(prompt_msg)
    while option > 4 or option < 1:
        option = validate_int_input((f"Invalid option. {colors[0]}{prompt_msg}{end_code}"))
    #option 1: Information regarding IOTA
    if option == 1:
        print(f"{colors[2]}*:･ﾟ✧*:･ﾟ Information regarding IOTA cryptocurrency *:･ﾟ✧*:･ﾟ{end_code}")
        IOTA = '''IOTA (MIOTA) is a cryptocurrency that strives to eradicate the limitations that blockchains have on the world 
    of crypto. Blockchains are a way that data about crypto transactions can be stored, but has a multitude of 
    downfalls, like it's lack of privacy and potential costliness. In order to combat this issue, IOTA operates on a 
    network called the Internet of Things which claims to allow for feeless and safe transactions. The current 
    conversion rate of IOTA (MIOTA) is 1 MIOTA to 39.06 JPY.
    '''
        print(f"{colors[3]}{IOTA}{end_code}")

    #option 2: Create a transaction
    from my_library import validate_number_input
    def transaction(date:str,amount:str,reason:str):
        '''
        wallet.csv
        :param date: date a string
        :param amount: amount a string
        :param reason: reason a string
        :return: nothing
        '''
        file = open("wallet.csv", "a")
        file.write(f"{date},{amount},{reason}\n")
    if option == 2:
        transaction_type = int(input("Would you like to make a deposit or a withdrawal? Type '1' for deposit and '2' for a withdrawal: "))
        if transaction_type == 1:
            new_date = input("Enter today's date: ")
            new_deposit = input("Enter deposit amount: ")
            new_reason = input("Reason for deposit: ")
            transaction(date=new_date,amount=new_deposit,reason=new_reason)
            print(f"{colors[2]} Deposit completed.{end_code}")
        if transaction_type == 2:
            new_date = input("Enter today's date: ")
            new_withdrawal = input("Enter withdrawal amount: ")
            minus = f"-{new_withdrawal}"
            new_reason = input("Enter reason for withdrawal: ")
            transaction(date=new_date,amount=minus,reason=new_reason)
            print(f"{colors[2]} Withdrawal completed.{end_code}")
    #option 3: view past transactions
    if option == 3:
        import csv
        file = open('wallet.csv')
        csvreader = csv.reader(file)
        rows = []
        for row in csvreader:
            rows.append(row)
        print(f"{colors[2]}Date: Amount: Reason:{end_code}")
        for i in rows:
            print(*i, sep=", ")
    #option 4: overall transaction data
    if option == 4:
        total_wallet = 0
        index = 0
        with open("wallet.csv", "r") as file:
            wallet_log = file.readlines()
            for log in wallet_log:
                if index >= 0:
                    values = log.split(",")
                    deposit = values[1]
                    total_wallet += int(deposit)
                index += 1
        print(f"{colors[2]}Your total balance is {total_wallet} MIOTA.{end_code}")
        print(f"{colors[2]}This is equal to {total_wallet * 39.38} JPY. {end_code}")
    restart()
Main()
```
