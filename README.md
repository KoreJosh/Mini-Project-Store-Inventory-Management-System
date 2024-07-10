# Provision Store/Grocery Store Lite Management System

![](https://github.com/KoreJosh/Mini-Project-Store-Inventory-Management-System/blob/main/store2.jpg)


# Overview
The Provision Store/Grocery Store Lite Management System is designed to help store managers efficiently manage their inventory, sales, financials, and customer information. It includes functionalities for tracking product inventory, managing sales, recording financials, handling customer data, managing the checkout process, and generating QR codes for products.

# Introduction
Managing inventory effectively is crucial for the smooth operation of any store, whether it's a grocery store or a provision store. A reliable inventory management system helps keep track of products, manage sales, handle customer details, and ensure financial transparency. In this article, I'll guide you through the development of a Provision Store/Grocery Store Lite Management System using Python.

# Key Features
Inventory Management: Track and update product inventory with categories, product codes, and shelf locations.
Sales Recording: Record and manage sales transactions, including quantity, price, and seller information.
Financial Management: Keep detailed records of costs, markups, and profits.
Customer Management: Store and update customer information, including names, gender, email, phone number, and location.
Checkout Process: Handle the checkout process, apply discounts for existing customers, and update inventory quantities.
Returns and Refunds: Manage returns and refunds efficiently.
QR Code Generation: Generate and store QR codes for products with details including category, cost price, markup, quantity, and shelf location.


# Predefined Containers
First, we define our product categories and set up the necessary data structures to store inventory, customer details, and sales records.

    catSet = {'Dairies', 'Toiletries', 'Stationaries', 'Cereals', 'Pastries', 'Drinks'}
    prdtDict = {'Dairies': [], 'Toiletries':[], 'Stationaries':[], 'Cereals':[], 'Pastries':[], 'Drinks':[]}
    prdtInv = {}  # stores {product: {Qty: xx, cat: xx, cost: xxx, updateDate: xx, Markup: xx}, ...}
    
    custDict = {}  # stores customer details [name, email, phone, gender]
    custId = 10022001  # customer Id
    
    # Define shelf locations for each category
    shelfLocations = {
        'Dairies': 'A1',
        'Toiletries': 'B1',
        'Stationaries': 'C1',
        'Cereals': 'D1',
        'Pastries': 'E1',
        'Drinks': 'F1'
    }
    
    salesInvoice = []


Functionality Implementation
We'll implement the core functionalities in a main function, invtManager, which handles different modes: inventory management, checkout, customer management, and inventory search.

1. Inventory Initialization
This function initializes the inventory management system, handles product input, and updates the inventory.

![](https://github.com/KoreJosh/Mini-Project-Store-Inventory-Management-System/blob/main/Product%20By%20Category%20Entry.png)

2. Checkout Process
This function handles the checkout process by reducing inventory quantity, generating sales invoices, and applying discounts for existing customers.

![](https://github.com/KoreJosh/Mini-Project-Store-Inventory-Management-System/blob/main/CheckOut.png)

3. Customer Management
This function manages customer details by allowing new customer registration and searching for existing customers.

![](https://github.com/KoreJosh/Mini-Project-Store-Inventory-Management-System/blob/main/customer%20Detail.png)

4. Product Inventory Tracking
Effective product inventory tracking is the backbone of any inventory management system. It ensures that store owners can maintain optimal stock levels, avoid overstocking or stockouts, and make informed decisions based on real-time inventory data. Let's dive into how we can implement this feature. This service as the heart of the Inventory management system.

Inventory DataFrame
The invtManager function using the checkout Process and Inventory Initialization data to compute the product, price, customer name, checkout time, InitiLal Product Quantity, product quantity after purchase, and updates the inventory.

![](https://github.com/KoreJosh/Mini-Project-Store-Inventory-Management-System/blob/main/Inventory.png)

Inventory Search
The invtManager function also supports searching products in the inventory based on various attributes. This functionality allows for quick access to product details and aids in decision-making.

                
            elif mode == 'searchInventory':
                # Merge the dictionaries into a DataFrame
                def merge_to_dataframe(prdDict, prdInv):
                    data = []
                    for products in prdDict.values():
                        for product in products:
                            if product in prdtInv:
                                details = prdtInv[product]
                                details['Product'] = product
                                data.append(details)
                    import pandas as pd
                    df = pd.DataFrame(data)
                    return df
                # Create the DataFrame
                invt_df = merge_to_dataframe(prdtDict, prdtInv)
                # Function to search for a product and display its details
                product_name = input('product_name ')
                result = invt_df[invt_df['Product'] == product_name]
                if not result.empty:
                    print(f"Details of '{product_name}':")
                    print(result.to_string(index=False))
                else:
                    print(f"Product '{product_name}' not found in the inventory.")
                    return

Conclusion
Building an inventory management system requires careful planning and understanding of the store's operational needs. This project provides a solid foundation for managing store inventory, sales, and customer details using Python. The full code and additional details can be found in the [GitHub repository](https://github.com/KoreJosh/Mini-Project-Store-Inventory-Management-System/blob/main/Techtern%20Project%20%5BStore%20Inventory%20Management%5D.ipynb).
    


