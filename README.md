import random

class BankAccount:
    def __init__(self, owner, balance=0):
        self.owner = owner
        self.balance = balance
        self.transaction_history = []
        self.account_code = self.generate_account_code()

    def generate_account_code(self):
        """Generate a unique 6-digit account code."""
        return str(random.randint(100000, 999999))

    def deposit(self, amount):
        if amount > 0:
            self.balance += amount
            self.transaction_history.append(f"Deposited {amount}")
            print(f"Deposited {amount}. New balance is: {self.balance}")
        else:
            print("Deposit amount must be positive.")

    def withdraw(self, amount):
        if amount <= 0:
            print("Withdrawal amount must be positive.")
        elif amount > self.balance:
            print("Insufficient balance!")
        else:
            self.balance -= amount
            self.transaction_history.append(f"Withdrew {amount}")
            print(f"Withdrew {amount}. New balance is: {self.balance}")

    def check_balance(self):
        print(f"Current balance: {self.balance}")

    def view_transactions(self):
        print("Transaction History:")
        for transaction in self.transaction_history:
            print(transaction)

def open_account():
    """Allow the user to open a new bank account."""
    print("\nOpening a new account...")
    name = input("Enter your name: ")
    account = BankAccount(name)
    print(f"Account successfully created! Your unique account code is: {account.account_code}")
    return account

def get_account_by_code(accounts, account_code):
    """Search for an account using the account code."""
    for account in accounts:
        if account.account_code == account_code:
            return account
    return None

def main():
    print("Welcome to the Bank!")
    
    accounts = []  
    current_account = None  

    while True:
        print("\nMenu:")
        print("1. Open New Account")
        print("2. Access Existing Account")
        print("3. Exit")
        
        choice = input("Enter your choice: ")

        if choice == '1':  
            account = open_account()
            accounts.append(account)

        elif choice == '2':
            if current_account:
                print(f"\nCurrently logged in with account code: {current_account.account_code}")
            account_code = input("Enter your account code: ")
            account = get_account_by_code(accounts, account_code)
            
            if account:
                current_account = account
                print(f"\nWelcome back, {current_account.owner}!")
                
                while True:
                    print("\nAccount Menu:")
                    print("1. Deposit")
                    print("2. Withdraw")
                    print("3. Check Balance")
                    print("4. View Transaction History")
                    print("5. Log Out")
                    choice = input("Enter your choice: ")
                    
                    if choice == '1':
                        amount = float(input("Enter deposit amount: "))
                        current_account.deposit(amount)
                    elif choice == '2':
                        amount = float(input("Enter withdrawal amount: "))
                        current_account.withdraw(amount)
                    elif choice == '3':
                        current_account.check_balance()
                    elif choice == '4':
                        current_account.view_transactions()
                    elif choice == '5':
                        print("Logged out successfully.")
                        current_account = None
                        break
                    else:
                        print("Invalid choice. Please try again.")
            else:
                print("Account not found. Please check the account code and try again.")

        elif choice == '3':  # Exit
            print("Thank you for using the bank. Goodbye!")
            break
        
        else:
            print("Invalid choice. Please try again.")

if __name__ == "__main__":
    main()
