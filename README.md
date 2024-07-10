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

### Inventory Management
Adding/Updating Inventory
  Initialize Inventory:
- Enter product category.
- Enter product name.
- Enter product details (cost per unit, markup, quantity).
- Optionally generate and save a QR code for the product.
  
Display Inventory:
 View the current status of the inventory.
 Searching Inventory
  Search for a Product:
 - Enter the product name to fetch details from the inventory.
  Sales Checkout
- Checkout Process:
- Enter seller's name.
- Enter customer's name.
- Enter product name and quantity.
- Calculate total sales and update inventory.
- Apply discounts for existing customers.
Customer Management
Add/Update Customer Information:
Enter customer details (name, gender, email, phone number, location).

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
Functionality Description
This code block handles the initialization and management of the inventory system. It includes functions for generating QR codes, validating product details, and managing product categories and details. The process includes user prompts for input and updates to the inventory system.

           else:
                def generate_qr_code(data, filename):
                    qr = qrcode.QRCode(
                        version=1,
                        error_correction=qrcode.constants.ERROR_CORRECT_L,
                        box_size=10,
                        border=4,
                    )
                    qr.add_data(data)
                    qr.make(fit=True)
        
                    img = qr.make_image(fill_color="black", back_color="white")
                    img.save(filename)
                    print(f"QR code saved as {filename}")
                                  
                def get_valid_product_detail():
                    while True:
                        prdtDetails = input('enter prdt Cost per unit, MrkUP, Qty in the given order seperated by comma : ')
                        try:
                            cPrice, mkUp ,qty = prdtDetails.split(',')
                            cPrice, mkUp ,qty = float(cPrice.strip()), float(mkUp.strip()), float(qty.strip())
                            return cPrice, mkUp, qty
                        except:
                            print("Invalid input. Please enter numeric values for cost per unit, markup, and quantity separated by commas.")
        
        
                print('initializing inventory management system...')
                
                while True:
                    
                    try: 
                        prdtCat = input('enter product Category (or type \'done\' to finish): ').strip()
                        if prdtCat.lower() == 'done':
                            break
                        if not prdtCat.replace(' ','').isalpha():
                            raise ValueError("Invalid Category. Please enter a valid Product Category consisting of only alphabet")
                    except ValueError as e:
                        print(e)
                        continue
                        
                    if prdtCat in catSet:
                        prdtCat = prdtCat
                        
                    else:
                        #Implementing code to automatically add new categories
                        print('You havae entered a new Category, Do you want to update the Category List ?')
                        while True:
                            resp = input('enter Yes or No : ').strip().capitalize()
                            if resp in ['Yes','No']:
                                break
                            else:
                                print("Invalid Response. Please enter 'Yes' or 'No' ")
        
                        if resp == 'No':
                            invtManager()
                            return
        
                        else:
                            catSet.add(prdtCat)
                            prdtDict[prdtCat] = []
                            shelfLocations[prdtCat] = input(f'Enter shelf location for new category {prdtCat}: ')
        
                    #imputing product
                    prdtName = input ('enter product of interest : ').strip()
                    if prdtName in list(prdtDict[prdtCat]):
                        # UPDATING THE DICT
                        cPrice, mkUp, qty = get_valid_product_detail()
        
                        tempDict = prdtInv[prdtName]
                        tempDict['Qty'] += qty
                        tempDict['CPrice'] = (tempDict['CPrice'] + cPrice) / 2
                        tempDict['markUp'] = (tempDict['MarkUp'] + mkUp) / 2
                        tempDict['dateAdded'] = datetime.now().strftime('%Y-%m-%d %H:%M:%S')
                        prdtInv[prdtName] = tempDict
        
                    else:
                        print(f' you have entered a new product. Do you want me to updet the product List  for {catSet}?')
                        while True:
                            resp = input('enter (Yes) or (No) : ').strip().capitalize()
                            if resp in ['Yes', 'No']:
                                break
                            else:
                                print("Invalid Response. Please enter 'Yes' or 'No' ")
                                
        
                        if resp == 'No':
                              invtManager()
        
                        else:
                            cPrice, mkUp ,qty = get_valid_product_detail()
                            currTime = datetime.now().strftime('%Y-%m-%d %H:%M:%S')
                            shelfLocation = shelfLocations[prdtCat]
        
                            # Add new product
                            prdtDict[prdtCat].append(prdtName)
                            tempDict = {'Cat': prdtCat, 'Qty': qty, 'CPrice': cPrice, 'MarkUp': mkUp, 'DateAdded': currTime, 'ShelfLocation': shelfLocation}
                            prdtInv[prdtName] = tempDict
                            
                            # Generate QR code for the new product
                            qr_data = f"Category: {prdtCat}, Product: {prdtName}, Cost Price: {cPrice}, MarkUp: {mkUp}, Quantity: {qty}, Shelf Location: {shelfLocation}"
                            qr_filename = f"{prdtName}_QR.png"
                            generate_qr_code(qr_data, qr_filename)
        
                print(f"Product '{prdtName}' has been added/updated in category '{prdtCat}' with shelf location '{shelfLocation}'.")
                return

2. Checkout Process
This function handles the checkout process by reducing inventory quantity, generating sales invoices, and applying discounts for existing customers.

Functionality Description
This code block handles the checkout procedure in the inventory management system. It collects details such as the seller's name, customer's name, and the products being purchased. It also validates inputs, updates inventory, applies discounts for existing customers, and generates a sales invoice.

            print('Initializing checkout procedure')
            while True:
                # Get seller's name
                sellerName = input("Enter seller's Name: ").strip()
                if sellerName.replace(' ','').isalpha():
                    break
                else:
                    print("Invalid name. Please enter a valid full name consisting of only alphabet")
        
        
            while True:
                # Get customer's name
                cust_name = input("Enter customer's Name:  ").strip()
                if cust_name.replace(' ','').isalpha():
                    break
                else:
                    print("Invalid name. Please enter a valid full name consisting of only alphabet")
        
        
            while True:
                prdtName = input('Enter product of interest (or type \'done\' to finish): ') 
        
                if prdtName.lower() == 'done':
                    break
        
                # Iterate over each category in prdDict to find the product
                for prdtCat, products in prdtDict.items():
                    if prdtName in products:
        
                        while True:
                            salesCart = input('Quantity are you interested in: ')
                            try:
                                qty = int(salesCart)
                                if qty <= 0:
                                    print("Quantity must be positive.")
                                    continue  # Restart the loop to ask for quantity again
                                break
                            except ValueError:
                                print("Invalid input. Please enter a valid quantity.")
        
                        # Check if there's enough quantity in the inventory                
                        if prdtInv[prdtName]['Qty'] >= qty:
                            tempDict = prdtInv[prdtName]  # Get product details
                            pPrice = tempDict['CPrice']
                            markUp = tempDict['MarkUp']
                            salePrice = round(pPrice * (1 + markUp), 2)
                            totalSales = qty * salePrice
        
                            for cust in custDict.values():
                                if cust_name in cust['Name']:
                                    totalSales = round(totalSales * (1 - 0.2), 2) #discount for existing customer
                                    print(f"Discount applied for existing customer. Total sales: ${totalSales:.2f}")
                            else:
                                print(f"Total sales: #{totalSales:.2f}")
        
                            salesTime = datetime.now().strftime('%Y-%m-%d %H:%M:%S')
        
                            # Update inventory quantity
                            prdtInv[prdtName]['currQty'] =prdtInv[prdtName]['Qty']- qty
                            prdtInv[prdtName]['salesDate'] = datetime.now().strftime('%Y-%m-%d %H:%M:%S')
                            prdtInv[prdtName]['soldBy'] = sellerName
        
        
                            # Add to sales invoice
                            salesInvoice.append([cust_name, prdtName, qty, salePrice, totalSales, salesTime, sellerName])
                            print(f"{prdtName} has been added to the sales invoice from the {prdtCat} category.")
                            print(f"Total sales: #{totalSales:.2f}")
                            print(f"{prdtName} sold by {sellerName}")
        
                        else:
                            print(f"Not enough quantity of {prdtName} in stock.")
                    else:
                        continue
            else:
                # If the product is not found in any category
                print(f"{prdtName} is not available in any category.")
            import pandas as pd
            df = pd.DataFrame(salesInvoice, columns =['Customer Name','Product','Quantity','salePrice', 'totalSales', 'Time', 'soldBy'])
            return df   


3. Customer Management
This function manages customer details by allowing new customer registration and searching for existing customers.
Functionality Description
This code block is responsible for entering and storing customer details. It collects customer information such as name, gender, email, phone number, and location. The data is then stored in a dictionary and converted into a DataFrame for further use.


            while True:
                #entring  customer details
                try:
                    custName = input('Enter customer full name (or type \'done\' to finish): ').strip()
                    if custName.lower() == 'done':
                        break
                    if not custName.replace(' ','').isalpha():
                        raise ValueError("Invalid name. Please enter a valid full name consisting of only alphabet")
                except ValueError as e:
                    print(e)
                    continue
        
                while True:
                    custGender = input('Gender (Male (M), Female(F)): ').strip().upper()
                    if custGender in ['M','F']:
                        break
                    else:
                        print("Invalid gender. Please enter 'M' for Male and 'F' for Female")
                while True:
                    custEmail = input('Email: ').strip()
                    pattern = r'^[a-zA-Z0-9]+@[a-zA-Z0-9]+\.com$'
        
                    if re.match(pattern, custEmail):
                        break
                    else:
                        print("Invalid email format. Please enter the email in the format *************@*********.com")
                while True:
                    custPhone = input('Phone number: ').strip()
                    if custPhone.isdigit():
                        break
                    else:
                        print("Inavlid phone number. Please enter a Valid Phone Number")
                while True:
                    custLoc = input('Location (City/Town Name): ').strip()
                    if custLoc.replace(' ','').isalpha():
                        break
                    else:
                        print("Invalid Location Input, Please enter correct input")
        
                tempDict = {'Customer ID': custId,'Name':custName,'Gender': custGender, 'Email':custEmail, 'Phone Number':custPhone, 'Location':custLoc}
                custDict[custId] = tempDict
                custId +=1
                print(f"Name '{custName}' has been added/updated to the Customer Information Database.")
        
            custList = list(custDict.values())
            import pandas as pd
            cust_df = pd.DataFrame(custList)
            return cust_df


5. Product Inventory Tracking
Effective product inventory tracking is the backbone of any inventory management system. It ensures that store owners can maintain optimal stock levels, avoid overstocking or stockouts, and make informed decisions based on real-time inventory data. Let's dive into how we can implement this feature. This service as the heart of the Inventory management system.

Inventory DataFrame
The invtManager function using the checkout Process and Inventory Initialization data to compute the product, price, customer name, checkout time, InitiLal Product Quantity, product quantity after purchase, and updates the inventory.
Functionality Description
This code block is responsible for displaying the current inventory status. It retrieves product details from the prdtInv dictionary, structures the data into a DataFrame using Pandas, and pivots the table for better readability. The function returns a pivot table of the inventory, displaying each product along with its attributes and values.


            elif mode == 'inventory': 
                print('Displaying current inventory status')
                data = []
                
                # Collecting product details into a list
                for prdt, detail in prdtInv.items():
                    for key, value in detail.items():
                        data.append([prdt, key, value])
                
                # Importing pandas for data manipulation
                import pandas as pd
                
                # Creating a DataFrame from the collected data
                df = pd.DataFrame(data, columns=['Products', 'Attribute', 'Value'])
                
                # Pivoting the DataFrame to make it more readable
                pivot_df = df.pivot(index='Products', columns='Attribute', values='Value')
                
                # Resetting the index to make 'Products' a column again
                pivot_df.reset_index(inplace=True)
                pivot_df.columns.name = None
            
                # Removing the 'Product' column if it exists to avoid duplicates
                col = 'Product'
                if col in pivot_df:
                    pivot_df = pivot_df.drop(columns=col)
                
                # Returning the pivot table
                return pivot_df


Inventory Search
The invtManager function also supports searching products in the inventory based on various attributes. This functionality allows for quick access to product details and aids in decision-making.
Functionality Description
This code block is responsible for searching the inventory for a specific product. It merges product data from the product dictionary (prdDict) and inventory dictionary (prdtInv) into a DataFrame, and then allows users to search for a product by name. If the product is found, its details are displayed; otherwise, a message indicating that the product is not found is shown.
                
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
                product_name = input('Enter product name: ')
                result = invt_df[invt_df['Product'] == product_name]
                if not result.empty:
                    print(f"Details of '{product_name}':")
                    print(result.to_string(index=False))
                else:
                    print(f"Product '{product_name}' not found in the inventory.")


The user will be prompted to enter the name of the product they want to search for:
The resultant output is displayed below.

        Enter product name: Milk
        Details of 'Milk':
        Product       Cat  Qty  CPrice  MarkUp             DateAdded ShelfLocation
        0    Milk  Dairies   50    1.50    0.20  2024-07-08 12:34:56           A1


Conclusion
Building an inventory management system requires careful planning and understanding of the store's operational needs. This project provides a solid foundation for managing store inventory, sales, and customer details using Python. The full code and additional details can be found in the [GitHub repository](https://github.com/KoreJosh/Mini-Project-Store-Inventory-Management-System/blob/main/Techtern%20Project%20%5BStore%20Inventory%20Management%5D.ipynb).
    


