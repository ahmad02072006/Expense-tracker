# Expense-tracker
import json
import os
from datetime import datetime

# Step 1: File operations

def load_expenses():
    """load expenses from file"""
    if os.path.exists('expenses.json'):
        with open("expenses.json",'r') as f:
            return json.load(f)
    return[]

def save_expenses(expenses):
    """Save expenses to file"""
    with open("expenses.json",'w') as f:
        json.dump(expenses,f,indent=2)

# Step 2: Add expense
def add_expense(expenses):
    """Add a new expense """
    amount=float(input('Enter a amount:$'))
    category=input("Enter a category(food/transport/enternintment/other):")
    description=input("Enter a description for your expense:")
    
    expense={
        'amount':amount,
        'category':category,
        'description':description,
        'date':datetime.now().strftime('%Y-%m-%d %H:%M')
    }

    expenses.append(expense)
    print("Expense added")
    return expenses

# Step 3: View expenses
def view_expense(expenses):
    """Display all expenses"""
    if not expenses:
        print("No expenses found.")
        return
    
    print("\n"+"="*50)
    print("Your Expenses")
    print("="*50)

    for i,exp in enumerate(expenses,1):
        print(f"\n{i}. ${exp['amount']:.2f}-{exp['category']}")
        print(f"{exp['description']}")
        print(f" Date:{exp['date']}")
        

# Step 4: Calculate statistics
def show_statistics(expenses):
    """Show spending statistics"""
    if not expenses:
        print("Not expenese found")
        return
    
    total=sum(exp['amount']for exp in expenses)
    
    # Calculate by category
    categories={}
    for exp in expenses:
        cat=exp['category']
        categories[cat]=categories.get(cat,0)+exp['amount']

    print("\n" + "="*50)
    print("STATISTICS")
    print("\nBy Category:")
    for cat,amount in categories.items():
        percentage=(amount/total)*100
        print(f"{cat}:${amount:.2f}. {percentage:.1f}%") 
          
# Step 5: Main menu
def main():
    '''Main program loop'''
    expenses=load_expenses()

    while True:
        print("\n"+'='*50)
        print("EXPENSE TRACKER")
        print("="*50)
        print('1.Add Expense')
        print('2.View expenses')
        print('3.Show Statistics')
        print('4.Exit')

        choice=input("\n Choose an option(1-4):")
        
        if choice=='1':
            expenses=add_expense(expenses)
            save_expenses(expenses)
        elif choice=='2':
            view_expense(expenses)
        elif choice=='3':
            show_statistics(expenses)
        elif choice=='4':
            save_expenses(expenses)
            print("Goodbye!")
            break
        else:
            print("Invalid choice.Try again")

if __name__=='main':
    main()

main()
