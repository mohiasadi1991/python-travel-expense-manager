import json
import os
from datetime import dat

FILE = "travel_expenses.json"

def load_expenses():
    if os.path.exists(FILE):
        try:
            with open(FILE, "r", encoding="utf-8") as f:
                return json.load(f)
        except (json.JSONDecodeError, OSError):
            return []
    return []

def save_expenses(expenses):
    with open(FILE, "w", encoding="utf-8") as f:
        json.dump(expenses, f, indent=4, ensure_ascii=False)

def add_expense(expenses):
    title = input("Expense title: ").strip()
    category = input("Category: ").strip()

    try:
        amount = float(input("Amount: "))
        if amount <= 0:
            print("Amount must be positive.")
            return
    except ValueError:
        print("Enter a valid amount.")
        return

    if not title or not category:
        print("Title and category are required.")
        return

    expenses.append({
        "title": title,
        "category": category,
        "amount": amount,
        "date": date.today().isoformat()
    })

    save_expenses(expenses)
    print("Expense added successfully!")

def show_expenses(expenses):
    if not expenses:
        print("No expenses recorded.")
        return

    for i, item in enumerate(expenses, 1):
        print(
            f"{i}. {item['title']} | "
            f"{item['category']} | "
            f"{item['amount']:.2f} | "
            f"{item['date']}"
        )

def show_total(expenses):
    total = sum(item["amount"] for item in expenses)
    print(f"Total travel expenses: {total:.2f}")

def category_summary(expenses):
    if not expenses:
        print("No expenses recorded.")
        return

    totals = {}

    for item in expenses:
        category = item["category"]
        totals[category] = totals.get(category, 0) + item["amount"]

    print("\n--- Category Summary ---")
    for category, amount in sorted(totals.items()):
        print(f"{category}: {amount:.2f}")

def delete_expense(expenses):
    show_expenses(expenses)

    if not expenses:
        return

    try:
        number = int(input("Expense number to delete: "))

        if 1 <= number <= len(expenses):
            removed = expenses.pop(number - 1)
            save_expenses(expenses)
            print(f"Deleted: {removed['title']}")
        else:
            print("Invalid number.")

    except ValueError:
        print("Enter a valid number.")

def main():
    expenses = load_expenses()

    while True:
        print("\n===== TRAVEL EXPENSE MANAGER =====")
        print("1. Add expense")
        print("2. View expenses")
        print("3. Show total")
        print("4. Category summary")
        print("5. Delete expense")
        print("6. Exit")

        choice = input("Choose an option: ")

        if choice == "1":
            add_expense(expenses)
        elif choice == "2":
            show_expenses(expenses)
        elif choice == "3":
            show_total(expenses)
        elif choice == "4":
            category_summary(expenses)
        elif choice == "5":
            delete_expense(expenses)
        elif choice == "6":
            print("Goodbye!")
            break
        else:
            print("Invalid choice. Try again.")

if __name__ == "__main__":
    main()
