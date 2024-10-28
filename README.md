# expense-tracker-project
import datetime
import json

class ExpenseTracker:
    def __init__(self):
        self.expenses = {}
        self.categories = set()

    def add_expense(self):
        date = input("Enter date (YYYY-MM-DD): ")
        category = input("Enter category (e.g., food, transportation, entertainment): ")
        amount = float(input("Enter amount spent: "))
        description = input("Enter brief description: ")

        if category not in self.expenses:1

            self.expenses[category] = []

        self.expenses[category].append({
            "date": date,
            "amount": amount,
            "description": description
        })

        self.categories.add(category)

    def view_expenses(self):
        for category in self.expenses:
            print(f"Category: {category}")
            for expense in self.expenses[category]:
                print(f"Date: {expense['date']}, Amount: {expense['amount']}, Description: {expense['description']}")
            print()

    def view_monthly_summary(self):
        year = int(input("Enter year: "))
        month = int(input("Enter month: "))

        monthly_expenses = {}

        for category in self.expenses:
            monthly_expenses[category] = 0
            for expense in self.expenses[category]:
                expense_date = datetime.datetime.strptime(expense["date"], "%Y-%m-%d")
                if expense_date.year == year and expense_date.month == month:
                    monthly_expenses[category] += expense["amount"]

        for category, amount in monthly_expenses.items():
            print(f"Category: {category}, Amount: {amount}")

    def view_category_wise_expenditure(self):
        for category in self.expenses:
            total_amount = sum(expense["amount"] for expense in self.expenses[category])
            print(f"Category: {category}, Total Amount: {total_amount}")

    def save_data(self):
        with open("expenses.json", "w") as file:
            json.dump(self.expenses, file)

    def load_data(self):
        try:
            with open("expenses.json", "r") as file:
                self.expenses = json.load(file)
        except FileNotFoundError:
            pass

def main():
    tracker = ExpenseTracker()
    tracker.load_data()

    while True:
        print("1. Add Expense")
        print("2. View Expenses")
        print("3. View Monthly Summary")
        print("4. View Category-wise Expenditure")
        print("5. Save Data")
        print("6. Exit")

        choice = input("Enter choice: ")

        if choice == "1":
            tracker.add_expense()
        elif choice == "2":
            tracker.view_expenses()
        elif choice == "3":
            tracker.view_monthly_summary()
        elif choice == "4":
            tracker.view_category_wise_expenditure()
        elif choice == "5":
            tracker.save_data()
        elif choice == "6":
            break
        else:
            print("Invalid choice. Please try again.")

if __name__ == "__main__":
    main()
