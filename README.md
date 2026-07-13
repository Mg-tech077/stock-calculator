# stock-calculator
import csv

# Hard-coded stock prices
stock_prices = {
    "AAPL": 180,
    "TSLA": 250,
    "GOOGL": 140,
    "MSFT": 330,
    "AMZN": 130
}

portfolio = {}
total_investment = 0

print("Available Stocks:")
for stock, price in stock_prices.items():
    print(f"{stock}: ${price}")

print("\nEnter your stock holdings (type 'done' to finish):")

while True:
    stock = input("Stock Name: ").upper()

    if stock == "DONE":
        break

    if stock not in stock_prices:
        print("Stock not found! Please choose from the available stocks.")
        continue

    try:
        quantity = int(input("Quantity: "))
    except ValueError:
        print("Please enter a valid number.")
        continue

    portfolio[stock] = quantity

print("\nPortfolio Summary")
print("-" * 30)

for stock, quantity in portfolio.items():
    investment = quantity * stock_prices[stock]
    total_investment += investment
    print(f"{stock}: {quantity} shares × ${stock_prices[stock]} = ${investment}")

print("-" * 30)
print(f"Total Investment Value: ${total_investment}")

# Save option
choice = input("\nSave portfolio? (yes/no): ").lower()

if choice == "yes":
    file_type = input("Save as (txt/csv): ").lower()

    if file_type == "txt":
        with open("portfolio.txt", "w") as file:
            file.write("Portfolio Summary\n")
            file.write("-" * 30 + "\n")
            for stock, quantity in portfolio.items():
                investment = quantity * stock_prices[stock]
                file.write(
                    f"{stock}: {quantity} shares × ${stock_prices[stock]} = ${investment}\n"
                )
            file.write("-" * 30 + "\n")
            file.write(f"Total Investment Value: ${total_investment}\n")
        print("Portfolio saved as portfolio.txt")

    elif file_type == "csv":
        with open("portfolio.csv", "w", newline="") as file:
            writer = csv.writer(file)
            writer.writerow(["Stock", "Quantity", "Price", "Investment"])

            for stock, quantity in portfolio.items():
                investment = quantity * stock_prices[stock]
                writer.writerow(
                    [stock, quantity, stock_prices[stock], investment]
                )

            writer.writerow([])
            writer.writerow(["Total Investment", "", "", total_investment])

        print("Portfolio saved as portfolio.csv")

    else:
        print("Invalid file type. Portfolio not saved.")
