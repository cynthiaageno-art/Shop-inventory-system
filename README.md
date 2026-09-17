
from datetime import date

class Product: # This class represents one product sold at Guild Canteen.

    def __init__(self, product_code, name, price, quantity, expiry, low_stock_threshold=10): # Example: GC001 - Rolex - UGX 5,000 - 20 pieces - N/A
        self.product_code = product_code # Unique code used to identify the product.
        self.name = name # Name of the product.
        self.price = price # Selling price of one item.
        self.quantity = quantity  # Number of items currently in stock.
        self.expiry = expiry # Expiry date or N/A.
        self.low_stock_threshold = low_stock_threshold # Minimum quantity before stock is considered low.
    
    
    # Reduces stock when a customer buys a product.
    # Example:
    # A student buys 2 Rolexes at UGX 5,000 each.
    # Total = 2 × 5,000 = UGX 10,000
    
    def sell(self, amount):

        if self.is_expired(): # Do not allow expired products to be sold.
            print("This product has expired and cannot be sold.")

        elif amount <= 0: # Do not allow zero or negative sales.
            print("Quantity must be greater than zero.")

        elif amount > self.quantity: # Do not allow the customer to buy more than available.
            print("Not enough stock.")

        else:
            self.quantity -= amount  # Reduce stock by the quantity sold.
            total = amount * self.price # Calculate total price.

            print("Sale successful.")
            print(f"Product: {self.name}")
            print(f"Quantity sold: {amount}")
            print(f"Total amount: UGX {total:,.2f}")
            print(f"Remaining stock: {self.quantity}")


    def add_stock(self, amount):  # Adds new stock to an existing product.
        if amount > 0:
            self.quantity += amount # 50 bottles + 25 bottles = 75 bottles

            print("Stock added successfully.")
            print(f"New stock: {self.quantity}")

        else:
            print("Quantity must be greater than zero.")

    def is_low_stock(self):
        return self.quantity <= self.low_stock_threshold

    # CHECK EXPIRY

    def is_expired(self):
        if self.expiry.upper() == "N/A": # Products without an expiry date use N/A.
            return False
        try:
            expiry_date = date.fromisoformat(self.expiry) # Convert the text date into a Python date.
            return expiry_date < date.today() # True when expiry date has passed.
        except ValueError: # Invalid dates are rejected when products are added,

            return False


    # DISPLAY PRODUCT

    def show(self):
        if self.is_expired():  # Determine product status.
            status = "EXPIRED"
        else:
            status = "OK"

        if self.is_low_stock():
            stock_status = "LOW STOCK"
        else:
            stock_status = "OK"

        print(
            f"Code: {self.product_code} | "
            f"Name: {self.name} | "
            f"Price: UGX {self.price:,.2f} | "
            f"Quantity: {self.quantity} | "
            f"Expiry: {self.expiry} | "
            f"Status: {status}"
            f"Stock: {stock_status}"
        )
