Excellent. That is the perfect approach. A well-structured, sequential business idea document is the foundation of a successful project. It ensures clarity for you as the business owner and for the development team that will build the application.

### Deferred features (MVP)

The following lower-priority, higher-effort features are intentionally deferred from the initial MVP to reduce scope and accelerate delivery. We'll plan them for follow-up releases and keep them in the product backlog:

- Product image uploads and variant-specific image management — use placeholders or external ImageURL fields for MVP.
- Self-serve Excel/CSV product import and export — onboarding team will handle guided imports for the launch.
- Advanced loyalty programs (tiered cashback, points systems) — MVP will support simple POS discounts only.
- Automated Telegram notifications (daily summaries, low-stock alerts) — reports and alerts will be available inside the web app at launch.

These deferrals will be revisited during iteration planning; if you want any to move back into MVP scope, tell me which and I will update the roadmap.

Let's begin with the foundational pillar of the application: **Core System Settings**. Before any sales can be made or products can be added, the system needs to understand the basic rules of your business environment.

### **Part 1: Core System Settings**

This module is the control panel for the entire OsonSavdo application. It's where the business owner or administrator defines the fundamental operational parameters. A logical and thorough setup here is critical for accurate reporting and smooth daily operations.

---

#### **1.1. Currency Management**

**Purpose:** To define the currencies your business operates with, ensuring all financial transactions and reports are accurate.

**User Story:** As a business owner, I need to set up the currencies I accept and use for pricing so that all sales, costs, and reports are calculated correctly.

**Feature Breakdown:**

- **Primary Currency:** The system must allow the user to designate a primary "Receiving Currency." This will be the default currency for most operations and financial reports.
- **Sales Currency:** The user must be able to specify a "Sales Currency," which can be the same as or different from the receiving currency.
- **Multi-Currency Support:** The application should support a list of common international and local currencies that the user can enable for their business.
- **Manual Exchange Rate:**
  - **Requirement:** If the "Receiving Currency" and "Sales Currency" are different, the system _must_ require the user to enter a manual exchange rate for conversion. This is a critical step to prevent calculation errors.
  - **Example:** If your products are priced in U.S. Dollars (Sales Currency) but you manage your primary accounts in Uzbekistani Som (Receiving Currency), you must be able to set an exchange rate (e.g., 1 USD = 12,500 UZS).
  - **User Flow:** The user selects the currencies. If they are different, a field for the exchange rate appears, which they must fill in before they can save the settings.

**Edge Cases & Considerations:**

- **Fluctuating Rates:** For this initial version, the system will rely on a manually updated exchange rate. We are acknowledging that the business owner is responsible for keeping this rate current. (A future enhancement could be to integrate an automatic exchange rate service).
- **Historical Reporting:** When an exchange rate is updated, how will it affect past sales reports? The system should calculate transactions based on the exchange rate that was active _at the time of the sale_.

---

#### **1.2. Payment Type Configuration**

**Purpose:** To define all the methods of payment a customer can use, from standard options like cash to unique, business-specific methods.

**User Story:** As a business owner, I need to configure all the payment methods I accept so that my cashiers can accurately record sales and I can track revenue by payment type.

**Feature Breakdown:**

- **Standard Payment Types:** The system will come pre-configured with universal payment types, such as:
  - Cash
  - Card (covering debit, credit, etc.)
- **Custom Payment Types:**
  - **Requirement:** The system must provide the ability for the user to create and name their own custom payment methods. This is essential for business flexibility.
  - **User Flow:** The user navigates to the "Payment Types" section, clicks an "Add New" button, enters a name for the new payment method (e.g., "Sberbank Transfer," "Payme," "Click," "Store Credit"), and saves it.
  - **Example:** A clothing store might want to add a "Gift Certificate" payment type. A B2B business might add "Bank Transfer" or "Payment on Account."

**Edge Cases & Considerations:**

- **Reporting:** All reports that show revenue must be able to be filtered by these payment types, including any custom ones created.
- **Split Payments:** The system will need to handle a single transaction paid for with multiple payment types (e.g., part cash, part card). This will be detailed further in the "Sales & Checkout" module.
- **Deactivating Types:** The user should be able to deactivate a payment type they no longer use, without deleting it, to preserve historical data.

---

#### **1.3. Telegram Bot Integration for Automated Reporting**

**Purpose:** To provide business owners with key performance metrics and notifications directly on their mobile devices via a Telegram bot, without needing to log into the main application.

**User Story:** As a busy business owner, I want to receive daily and monthly sales summaries automatically on my Telegram so I can stay informed about my business performance on the go.

**Feature Breakdown:**

- **Simple Activation:** The user can enable or disable Telegram notifications with a single checkbox in the settings.
- **Authentication:**
  - **Requirement:** To connect the application to the user's Telegram account, a secure, one-time setup process is required.
  - **User Flow:** The user is instructed to find the official "OsonSavdo Bot Manager" in their Telegram application. They will then provide their account credentials (e.g., phone number and a password/code from the OsonSavdo app) to the bot to link the accounts.
- **Configurable Reports:** The user can choose what information they receive and when.
  - **Daily Sales Statistics:** The user can set a specific time (e.g., 8:10 AM) to receive a summary of the previous day's sales.
  - **Periodic Reports:** The system can also be configured to send weekly and/or monthly summary reports.
  - **Selective Content:** The user should have the option to turn specific notifications on or off, ensuring they only receive the information that is most important to them.

**Edge Cases & Considerations:**

- **Security:** The connection process must be secure. We need to ensure that only the authorized user can link their account.
- **Multiple Users:** Can multiple managers or owners receive notifications? The system should ideally support sending notifications to multiple authorized Telegram users.
- **Notification Failures:** What happens if a notification fails to send (e.g., due to a network issue)? The system should have a mechanism to either retry or log the failure.

- Note (MVP): Automated Telegram notifications (daily sales summaries, low-stock alerts) will be deferred for the initial MVP. All reports and alerts will be available inside the web app at launch; Telegram integration will be implemented in a follow-up release.

This concludes the first, foundational part of our business idea document. Once you confirm you are ready, we will proceed to the next logical step: **Product & Inventory Management**.

Of course. We have established the foundational settings. Now, let's define how products are created, managed, and tracked within the system. This is the heart of your inventory.

### **Part 2: Product & Inventory Management**

This module is the digital warehouse for OsonSavdo. It's where every item you sell is defined, priced, and tracked. A well-organized catalog is essential for a fast checkout process, accurate inventory counts, and insightful sales reporting.

---

#### **2.1. Product Catalog Configuration (The Rules)**

**Purpose:** To set up the universal rules and attributes that will apply to all products in your catalog, ensuring consistency and enabling powerful features.

**User Story:** As an administrator, I need to define my product rules, such as units of measure and low stock alerts, before I start adding products, so that my inventory is managed consistently and I am alerted to important issues.

**Feature Breakdown:**

- **Units of Measure:**
  - **Requirement:** The system must allow the user to define the units in which products are sold. This is crucial for businesses that sell items by weight, length, or volume, not just by the piece.
  - **User Flow:** The user can add new units by specifying a name (e.g., "kilogram") and a short code (e.g., "kg"). The system should support fractional quantities for these units (e.g., selling 0.250 kg of an item).
  - **Examples:** "Piece," "kg," "gram," "liter," "meter."
- **Wholesale Pricing:**
  - **Requirement:** The user must be able to enable or disable a separate "Wholesale Price" field for products. This allows for dual pricing strategies (retail vs. wholesale) within the same system.
  - **User Flow:** A simple checkbox in the settings, "Enable Wholesale Pricing," will activate this feature across the product catalog.
- **Low Stock Threshold (Trigger):**
  - **Requirement:** The system must allow the user to define what "low stock" means. This is a numerical threshold that triggers alerts.
  - **User Flow:** The user enters a number (e.g., 5). When the stock level of any product drops to 5 or below, it will be flagged as "low stock" in the catalog and in reports. This is a global setting but can be overridden on a per-product basis if needed.
- **Negative Margin Control:**
  - **Requirement:** The user must have the option to either allow or prohibit selling a product for less than its cost price (i.e., at a negative margin).
  - **User Flow:** A setting like "Allow Negative Margin" (defaulting to "Off"). When turned off, the system will prevent a user from setting a sale price lower than the cost price, preventing costly mistakes. When turned on, it allows for strategic decisions like "loss leader" promotions.

---

#### **2.2. Adding and Managing Products (The Items)**

**Purpose:** To provide a clear and comprehensive interface for creating and editing individual products in the catalog.

**User Story:** As a store manager, I need to easily add new products, including their variations like size and color, define their prices, and set their initial stock levels, so they are ready to be sold.

**Feature Breakdown:**

- **Product Types:** The system must support two fundamental types of products:
  1.  **Simple Product:** An item that is sold as a single unit without any variations. _Example: A specific brand of bottled water._
  2.  **Variable Product:** An item that has different attributes, such as size, color, or material. Each combination of these attributes is a unique variant with its own stock count and potentially its own SKU. _Example: A T-shirt that comes in different sizes and colors._
- **Core Product Information (The "Add New Product" Form):**
  - **Product Name:** The primary name of the item (e.g., "Men's Cotton T-Shirt").
  - **SKU / Article:** A unique internal code for the product model. The system should offer a "Generate" button to create a unique SKU automatically, or allow the user to enter their own.
  - **Barcode:** The user can input the product's scannable barcode number.
  - **Unit of Measure:** A dropdown menu populated from the settings defined in section 2.1.
  - **Product Photos:** Ability to upload one or more images for the product. For variable products, the user should be able to assign specific images to specific variations (e.g., a picture of the red shirt for the "Red" variant).
  - Note (MVP): Product image upload UI and file storage will be deferred for the initial MVP to reduce scope and speed up delivery. For MVP the product form will allow using placeholder images or specifying external image URLs in the import template; native image upload and variant-specific image management will be planned for a follow-up release.
- **Product Variations (Attributes):**
  - **Requirement:** For Variable Products, the user must be able to define attributes and their corresponding values.
  - **User Flow:** The user first defines an attribute (e.g., "Color"). Then, they add the values for that attribute (e.g., "Red," "Blue," "Black"). They can add multiple attributes (e.g., "Size" with values "S," "M," "L").
  - **Variant Generation:** The system will then automatically generate a list of all possible combinations (Red-S, Red-M, Red-L, Blue-S, etc.). For each unique variant, the user can manage:
    - A unique barcode/SKU (optional).
    - A specific photo.
    - The stock quantity.
    - The ability to activate/deactivate a specific variant (e.g., if you no longer sell the "Blue-L" version).
- **Pricing:**
  - **Cost Price:** What the item costs the business to purchase.
  - **Sale Price / Markup:** The user can either enter a final "Sale Price" directly or enter a "Markup" percentage (e.g., 25%), and the system will calculate the sale price automatically. This provides flexibility.
  - **Apply to All:** For variable products, a button to "Apply price to all variants" is a crucial time-saver.
- **Initial Stock Quantity:**
  - **Requirement:** When adding a product, the user must specify the initial quantity on hand.
  - **Multi-Store Functionality:** If the business has multiple store locations (a feature we can detail later), the system must allow the user to input the stock quantity for _each location_ from a single screen.

---

#### **2.3. Catalog Management & Viewing**

**Purpose:** To provide a powerful and intuitive interface for viewing, searching, and managing the entire product catalog.

**User Story:** As a manager, I need to be able to quickly search my product catalog, see which items are low on stock, and make bulk changes, so I can manage my inventory efficiently.

**Feature Breakdown:**

- **Catalog Views:** The main catalog screen should have quick-filter tabs to view different segments of the inventory:
  - **Active Products:** All items currently available for sale.
  - **Inactive Products:** Items that are temporarily not for sale (e.g., held for a customer, in transit between stores).
  - **Low Balance:** A list of all products that have fallen below the "Low Stock Threshold" defined in settings. This is a critical action list for reordering.
  - **Zero Balance:** All products that are completely out of stock.
  - **Archived Products:** Products that are no longer sold but are kept in the system for historical reporting purposes. This is different from deleting a product, which should be discouraged.
- **Search and Filtering:** A powerful search bar that can find products by Name, SKU, or Barcode. Additional filters should allow the user to narrow down the view by Category, Brand, or other attributes.
- **Customizable Columns:** The user should be able to customize the list view to show or hide different columns of information (e.g., Cost Price, Margin, Quantity, Category), tailoring the view to their needs.
- **Bulk Actions:** The user should be able to select multiple products at once and perform an action on all of them, such as:
  - **Archive:** Move selected products to the archive.
  - **Print Price Tags:** Generate and print price tags for all selected items.
  - **Modify Category:** Assign all selected products to a new category.

This covers the core of creating and managing your product list. When you are ready, we will move on to the highly practical and time-saving feature of **Bulk Product Import**, which is essential for businesses with large inventories.

Excellent. Let's proceed.

After defining individual products, the next critical challenge for any business with a significant inventory is adding those products to the system efficiently. Manually entering hundreds or thousands of items is not feasible. This is where a bulk import function becomes essential.

### **Part 3: Bulk Product Import via File**

**Purpose:** To provide a fast, efficient, and reliable method for adding or updating a large number of products at once, saving hours of manual data entry and reducing the risk of human error. This feature is crucial for initial system setup and for processing large new shipments from suppliers.

- Note (MVP): Easy data import/export (uploading a product catalog from Excel/CSV) will be deferred from the initial MVP to reduce delivery scope. For MVP the team will rely on manual product entry and small, guided imports handled by the onboarding team; full self-serve Excel/CSV upload and export will be planned for a follow-up release.

**User Story:** As a business owner receiving a new shipment of 200 different items, I need to upload a single file with all the product details to add them to my inventory instantly, so I can start selling them without delay.

---

#### **3.1. The Import Process**

The import process is designed to be a guided, step-by-step experience to ensure data accuracy.

**Feature Breakdown:**

- **Step 1: Download the Template**
  - **Requirement:** The system must provide a pre-formatted Excel template for the user to download. This template will contain all the necessary columns in the correct order, eliminating guesswork.
  - **User Flow:** In the "Import" module, the user clicks a "Download Template" button. An Excel file (`import_template.xlsx`) is downloaded to their computer.

- **Step 2: Populate the Template**
  - **Requirement:** The user fills the Excel template with their product data. The system will be designed to read specific columns.
  - **Key Columns in the Template:**
    - `ProductName`: The name of the item.
    - `SKU`: The unique article/code for the product.
    - `Barcode`: The scannable barcode number.
    - `Quantity`: The number of units being added.
    - `CostPrice`: The cost of the item to the business.
    - `SalePrice`: The price at which the item will be sold.
    - `Category`: The product category (e.g., "Men's Clothing").
    - `Brand`: The product brand (e.g., "Nike").
  - **Handling Product Variations (Crucial Detail):**
    - To import variable products (e.g., a T-shirt with sizes and colors), the template will use a special column naming convention. Attributes are defined by columns with a leading underscore.
    - **Example:** To import a T-shirt, the user would have the `ProductName` as "Cotton T-Shirt" and then add two extra columns: `_Size` and `_Color`. Each row in the Excel file would represent a unique variant:
      | ProductName | \_Size | \_Color | Quantity | SalePrice |
      | :--- | :--- | :--- | :--- | :--- |
      | Cotton T-Shirt | S | Red | 10 | 150000 |
      | Cotton T-Shirt | M | Red | 15 | 150000 |
      | Cotton T-Shirt | S | Blue | 12 | 150000 |

- **Step 3: Upload the File**
  - **User Flow:** The user returns to the "Import" module. They select the specific store location for the import (if multi-store is enabled), choose their completed Excel file, and click an "Upload and Preview" button.

- **Step 4: Validation and Preview (The Safety Net)**
  - **Requirement:** This is the most critical step. The system _must not_ immediately add the data. Instead, it processes the file and shows the user a preview of the data it has read.
  - **Error Handling:** The system will automatically validate the data and highlight any rows with errors. It will provide clear, simple explanations for each error.
    - _Example Error 1:_ A row is missing a `ProductName`. The system highlights the row and displays the message: "Product Name is required."
    - _Example Error 2:_ The user tries to import a product with an `SKU` that already exists in the catalog. The system highlights this and notes: "SKU already exists. This will update the existing product."
  - **Handling New Categories/Brands:** If the Excel file contains a category or brand that doesn't exist in the system yet, the system should identify this. It will then give the user a choice for that column: "Create 'New Brand' as a new brand" or "Ignore." This prevents import failures and makes setup much faster.

- **Step 5: Finalize the Import**
  - **Requirement:** Once the user has reviewed the preview and corrected any errors in their file, they click a "Confirm Import" button.
  - **User Flow:** The system processes the validated data in the background. A progress bar is shown for large files. Upon completion, a summary message is displayed: "Success! 195 products were added and 5 products were updated."

---

#### **3.2. Import Types**

The system should be intelligent enough to handle different import scenarios based on the data provided.

**Feature Breakdown:**

- **Initial Stock Import (First-time setup):** This is used to populate the catalog for the first time. The system creates new products and sets their initial stock levels.
- **Stock Replenishment (Adding new stock):** If a user uploads a file where the SKUs _already exist_, the system should understand this as an instruction to _add_ the new quantity to the existing stock level, not replace it.
- **Price Updates:** The import can also be used to perform bulk updates of `CostPrice` or `SalePrice` for existing products.

**Edge Cases & Considerations:**

- **Large Files:** For files with thousands of rows, the processing should be handled as a background job. The user should be able to navigate away from the page and receive a notification within the app once the import is complete.
- **Accidental Duplicates:** The validation step is key to preventing the accidental creation of duplicate products if a user mistakenly gives a new product an existing SKU.
- **Reversibility:** A "rollback" or "undo" feature for an import is a complex, advanced function. For the initial version, we will rely on a robust validation and preview step to prevent errors from happening in the first place, rather than trying to undo them after the fact.

This concludes the bulk import functionality. When you are ready, we will proceed to the most active part of the application: **The Sales & Checkout Interface**.

Of course. This is the central hub of daily activity, and its design must prioritize speed, accuracy, and ease of use for the cashier. Let's break down the entire sales and post-sales process.

### **Part 4: The Sales & Checkout Interface (Point of Sale - POS)**

**Purpose:** To provide a fast, intuitive, and robust interface for cashiers to conduct sales transactions. This screen is the most frequently used part of the application, and its efficiency directly impacts customer satisfaction and operational speed.

**User Story:** As a cashier, I need a simple screen where I can quickly add products, handle various payment scenarios, and complete a sale with minimal clicks, so I can serve customers efficiently, especially during busy periods.

---

#### **4.1. The Point of Sale (POS) Screen Layout**

This is the cashier's main workspace. The layout should be clean and logically organized.

- **Left Pane (The Cart):** A running list of all items added to the current transaction. Each line item should clearly display:
  - Product Name (including variations like "Size: M, Color: Red")
  - Quantity
  - Price per unit
  - Total price for that line
- **Right Pane / Top Bar (The Control Panel):** This area contains the primary tools for building the sale.
  - **Universal Search Bar:** The most important element. This is where the cashier will scan barcodes or type product names/SKUs.
  - **Customer Selection:** A button or search field to add a customer to the sale.
  - **Quick-Pick Grid (Optional):** For businesses with a small number of high-frequency items (like a coffee shop), a configurable grid of buttons with product images can be displayed for touch-based selection.
- **Bottom Bar (The Summary):** A persistent summary of the transaction's financial details.
  - Subtotal
  - Discounts
  - Taxes (if applicable)
  - **Grand Total (large, bold font)**
  - A prominent **"PAY"** or **"CHECKOUT"** button.

---

#### **4.2. Adding Products to the Sale**

The process of adding items to the cart must be seamless.

**Feature Breakdown:**

- **Barcode Scanning (Primary Method):**
  - **Requirement:** The system should be "always listening" for input from a standard USB barcode scanner.
  - **User Flow:** The cashier scans an item's barcode. The product is instantly identified and added as a new line item in the cart on the left. If the same item is scanned again, the quantity for that line item simply increases by one.
- **Manual Search (Secondary Method):**
  - **Requirement:** If a barcode is unreadable or missing, the cashier must be able to find the product manually.
  - **User Flow:** The cashier types in the universal search bar. The system should instantly search by Product Name, SKU, and Article number, displaying a dropdown list of matching results. The cashier can then click or use the arrow keys to select the correct product to add to the cart.

---

#### **4.3. Managing the Cart & Applying Discounts**

Once items are in the cart, the cashier needs full control over the transaction before payment.

**Feature Breakdown:**

- **Modifying Line Items:**
  - **Change Quantity:** The cashier can click on the quantity of any item in the cart and type a new number, or use simple `+` and `-` buttons.
  - **Remove Item:** A clear "X" or trash can icon next to each line item allows for easy removal.
- **Applying Discounts:**
  - **Line Item Discount:** The cashier can click on a specific item and apply a discount to just that item, either as a percentage (%) or a fixed currency amount.
  - **Cart Discount:** A dedicated "Discount" button in the summary area allows the cashier to apply a discount to the entire subtotal.
  - **Automatic Promotions:** If an active promotion is set up in the system (e.g., "Buy one, get one 50% off"), the system should automatically recognize the qualifying items in the cart and apply the discount, showing a clear label for the promotion.
  - **Wholesale/Special Pricing:** If a customer is eligible for wholesale pricing, and this feature is enabled, the system should automatically use the wholesale price for the items.

---

#### **4.4. Customer Management in the POS**

Linking a sale to a customer is vital for loyalty programs and purchase history.

**Feature Breakdown:**

- **Search for Existing Customer:** The cashier can use the customer search bar to find a customer by Name or Phone Number.
- **Add New Customer (Quick Add):**
  - **Requirement:** If the customer is new, the cashier must be able to add them without leaving the POS screen.
  - **User Flow:** A simple pop-up form appears, requesting only the essential information to keep the checkout fast: **Name** and **Phone Number**. Other details can be added later in the main "Clients" module.
- **Displaying Customer Benefits:** Once a customer is attached to the sale, the POS screen should clearly display any relevant benefits, such as:
  - Current loyalty point balance.
  - Available store credit.
  - Any special group discounts they are a part of.

---

#### **4.5. The Payment Process**

This is the final and most critical step of the transaction.

**Feature Breakdown:**

- **Initiating Payment:** After clicking the "PAY" button, a payment screen appears, showing the final amount due.
- **Payment Method Selection:** The screen will display buttons for all payment methods configured in the settings (from Part 1), such as "Cash," "Card," "Payme," etc.
- **Split Payments (A Crucial Edge Case):**
  - **Requirement:** The system must seamlessly handle a customer paying with multiple methods.
  - **User Flow:**
    1.  The total is 150,000 UZS.
    2.  The customer wants to pay 50,000 in cash and the rest by card.
    3.  The cashier clicks the "Cash" button and enters `50000`.
    4.  The system updates to show "Remaining: 100,000 UZS".
    5.  The cashier then clicks the "Card" button. The system automatically populates the card payment field with the remaining 100,000 UZS.
- **Change Calculation:**
  - **Requirement:** If a customer pays with cash, the system must automatically calculate the change due.
  - **User Flow:** The total is 85,000 UZS. The customer hands the cashier 100,000 UZS. The cashier clicks "Cash" and enters `100000`. The system displays a large, clear message: **"Change Due: 15,000 UZS"**.
- **Finalizing the Sale:** Once the full amount is covered, a "Complete Sale" button becomes active. Clicking it finalizes the transaction.
- **Receipt Generation:** Upon completion, the system will automatically generate a receipt and trigger the receipt printer. The user should also have the option to send a digital receipt via SMS or another method if the customer's details are on file.

---

#### **4.6. Post-Sale Operations: Returns & Exchanges**

Handling returns is a standard part of retail and must be integrated into the POS workflow.

**Feature Breakdown:**

- **Initiating a Return:** The cashier accesses a "Return Mode" from the main POS screen.
- **Finding the Original Transaction:**
  - **Requirement:** To process a return, the original sale must be located.
  - **User Flow:** The cashier can find the transaction by scanning the barcode on the original receipt or by manually entering the receipt number.
- **Processing the Return:**
  - The original transaction's items are displayed on the screen.
  - The cashier selects the item(s) being returned and specifies the quantity.
  - The system calculates the total refund amount.
  - The cashier can then process the refund (e.g., back to the customer's card, as cash, or as store credit).
- **Inventory Update:** **This is a critical background action.** When a return is completed, the stock quantity for the returned item(s) is automatically increased in the inventory.
- **Exchanges:** An exchange is processed as a return of one item and a sale of another within the same transaction, with the system calculating the price difference the customer needs to pay or receive.

This detailed breakdown covers the complete lifecycle of a customer interaction at the point of sale. When you are ready, we will move on to **Reporting & Analytics**, which is where the data from all these sales is turned into valuable business insights.

Excellent. We have defined how sales are made; now let's focus on how to transform that raw transaction data into meaningful business intelligence. This is where the true value of a digital system is realized, helping you make smarter, data-driven decisions.

### **Part 5: Reporting & Analytics**

**Purpose:** To provide the business owner and managers with a suite of clear, comprehensive, and actionable reports. This module aggregates all the data collected from sales, inventory, and customer interactions, presenting it in a way that reveals trends, highlights performance, and identifies opportunities for growth.

**User Story:** As a business owner, I need to easily access and understand reports on my sales, top-selling products, and best employees, so I can make informed decisions about inventory, staffing, and marketing without needing to be a data expert.

---

#### **5.1. The Dashboard (The "At-a-Glance" View)**

The Dashboard is the first screen the user sees upon logging in. It's a high-level, visual summary of the business's current health.

**Feature Breakdown:**

- **Key Performance Indicators (KPIs):** A prominent display of the most critical metrics for a user-selected time period (e.g., "Today," "This Week," "This Month").
  - **Total Revenue (Net Sales):** The total amount of money brought in after returns.
  - **Gross Profit:** The revenue minus the cost of the goods sold.
  - **Number of Transactions:** The total count of individual sales.
  - **Average Transaction Value:** The average amount spent by a customer in a single transaction.
- **Visual Charts:**
  - **Sales Trend Chart:** A simple line or bar chart showing daily revenue over the selected period (e.g., the last 30 days). This makes it easy to spot busy days or upward/downward trends.
- **"Top 10" Lists:** Quick, digestible lists to show what's performing well right now.
  - **Top-Selling Products:** A list of the 10 products that have generated the most revenue.
  - **Top-Performing Employees:** A list of the 10 employees who have made the most sales.
- **Date Range Filter:** A simple, powerful date selector that controls all the data displayed on the dashboard. It should include convenient presets like "Today," "Yesterday," "This Week," "This Month," "Last Month," and a custom date range option.

---

#### **5.2. Consolidated Sales Report (The "How & When")**

This is the primary financial report, allowing for a deep dive into sales performance over any period.

**Feature Breakdown:**

- **Comprehensive Metrics:** The report will provide a detailed breakdown of:
  - Net Revenue
  - Gross Profit
  - Cost of Goods Sold (COGS)
  - Total Discounts Given (both as a value and a percentage of sales)
  - Number of items sold
  - Number of returns processed
- **Powerful Filtering:** The ability to filter the report is what makes it truly useful. Users must be able to filter the entire report by:
  - **Date Range:** The most fundamental filter.
  - **Store / Location:** (For multi-store businesses) Compare the performance of different branches.
  - **Employee / Cashier:** See the sales performance of a specific team member.
  - **Payment Type:** Analyze how much revenue is coming from Cash vs. Card vs. other custom types.
- **Data Visualization:** The report should present data in multiple formats:
  - A clear summary table with the main metrics.
  - A detailed, row-by-row list of every single transaction within the filtered period.
  - Graphical charts to visualize trends, such as sales by day of the week or by hour of the day.
- **Export Functionality:** A non-negotiable feature. A prominent **"Export to Excel"** button that allows the user to download the raw data from the report for further analysis or for use in external accounting software.

---

#### **5.3. Product Performance Reports (The "What")**

These reports focus specifically on the items being sold, helping to guide purchasing and marketing decisions.

**Feature Breakdown:**

- **Top-Sellers Report:** An in-depth report that lists products based on performance. Crucially, it must be sortable by:
  1.  **Quantity Sold:** Which items are moving the fastest.
  2.  **Revenue Generated:** Which items are bringing in the most money.
  3.  **Profit Generated:** **(The most important metric)** Which items are actually the most profitable for the business.
- **Category & Brand Performance:** This report aggregates data to show the performance of entire product categories or brands. This helps answer questions like, "Are shirts more profitable than pants?" or "Does Brand A outsell Brand B?"
- **Low-Performing Products ("Dust Collectors"):** A report that shows products that have sold the least over a specific period, helping to identify items that should be discounted, promoted, or discontinued.

---

#### **5.4. Inventory Reports (The "Where & How Much")**

These reports provide a clear financial and logistical overview of all stock.

**Feature Breakdown:**

- **Stock on Hand Report:** A real-time, filterable list of all products and their current inventory levels across all locations. This is the master list for what the business owns.
- **Inventory Valuation Report:** A critical report for accounting. It calculates the total value of all inventory currently in stock. The user must be able to calculate this value based on either:
  - **Cost Price:** What the inventory cost the business to acquire.
  - **Sale Price:** What the inventory is worth if sold at its current retail price.
- **Inventory Movement History:** A detailed log for a specific product that shows its entire history: initial stock entry, sales, returns, transfers between stores, and manual adjustments. This provides a complete audit trail for every single item.

**Edge Cases & Considerations:**

- **Data Load Times:** For businesses with a large volume of sales, reports for long date ranges (e.g., an entire year) could be slow to load. The system should be optimized to handle this, perhaps by processing the report in the background and notifying the user when it's ready.
- **Permissions:** In a multi-user system, a business owner might want to restrict access to certain reports. For example, a cashier might not need to see overall profitability reports. (This will be covered in a future "User Management" module).
- **Data Accuracy:** The value of all reports depends entirely on the accuracy of the data entered (e.g., correct cost prices, diligent recording of all sales). The system's design should always guide users toward accurate data entry.

This concludes the core reporting and analytics functionality. When you are ready, we can proceed to the **Client Management (CRM)** module, which focuses on tracking and engaging with your customers.

Of course. We've covered how to manage products and process sales. Now, let's focus on the most valuable asset for any business: the customers. This module is about understanding, organizing, and building relationships with your clientele.

### **Part 6: Client Management (CRM - Customer Relationship Management)**

**Purpose:** To create a centralized database of all your customers, track their purchase history, and build loyalty through targeted engagement and reward programs. A strong CRM turns one-time buyers into repeat, high-value customers.

**User Story:** As a business owner, I need a simple way to manage my customer list, understand their buying habits, and create a loyalty program to encourage them to shop with me more often.

---

#### **6.1. The Central Client Database**

This is the main screen of the CRM module, providing a comprehensive and searchable list of every customer who has ever been added to the system.

**Feature Breakdown:**

- **List View:** A clear, table-based view of all clients with customizable columns for key information:
  - Full Name
  - Phone Number
  - Total Purchase Amount
  - Date of Last Purchase
  - Current Balance (This is crucial, showing either debt owed to the store or loyalty points/cashback available to the customer).
- **Universal Search:** A powerful search bar that allows staff to instantly find a client by their Name or Phone Number.
- **Advanced Filtering:** The ability to segment the entire customer list based on specific criteria. This is the foundation of targeted marketing.
  - **Filter by Purchase Amount:** (e.g., Show all clients who have spent over 1,000,000 UZS).
  - **Filter by Last Purchase Date:** (e.g., Show all clients who haven't made a purchase in the last 3 months - a great list for a "We miss you!" campaign).
  - **Filter by Registration Date:** (e.g., Show all clients who signed up in the last week).
- **Export to Excel:** A vital feature allowing the business owner to download the entire customer list or a filtered segment for use in external marketing tools or for offline analysis.
- **Add New Client:** A dedicated button to open a full form for adding a new client with all their details, as opposed to the "quick add" feature in the POS.

---

#### **6.2. The Individual Client Profile (The 360° View)**

This is the detailed view for a single customer, accessed by clicking on their name in the database or searching for them in the POS.

**Feature Breakdown:**

- **Core Information:** All the essential contact and demographic details:
  - Name, Phone Number, Birthday, Gender, Address.
- **Financial Snapshot:** A summary panel showing key metrics for this specific customer:
  - Total Lifetime Spend
  - Total Number of Purchases
  - Average Check Size
  - Current Loyalty Cashback/Points Balance
- **Complete Purchase History:** A chronological list of every transaction this customer has ever made. Each entry should be clickable, showing the full receipt details of what they bought, when, and for how much. This is invaluable for customer service and understanding preferences.
- **Notes Section:** A simple text field where staff can leave important notes about the customer (e.g., "Prefers Brand X," "Always asks for gift wrapping").
- **Group & Tag Affiliation:** Clearly displays which groups or tags (see below) this customer belongs to.

---

#### **6.3. Client Segmentation: Groups & Tags**

This is the system for organizing your customers into meaningful categories for discounts and marketing.

**Feature Breakdown:**

- **Groups:**
  - **Purpose:** To assign clients to a specific category that often comes with a fixed benefit.
  - **Example:** You could create a "Family & Friends" group that automatically receives a 10% discount on all purchases. Or a "VIP" group for top spenders.
  - **Application:** A cashier can assign a customer to a group from their profile.
- **Tags:**
  - **Purpose:** To add flexible, informational labels to customers for filtering and analysis. Tags usually don't have an automatic discount attached.
  - **Example:** You could ask customers how they heard about you and tag them with "Instagram," "Facebook," or "Friend Referral." Later, you can filter your list to see which marketing channel brings in the most valuable customers.
  - **Application:** Tags can be added from the client's profile.

---

#### **6.4. Loyalty Program Management**

This is the engine for rewarding repeat business. The system should be built around a flexible, tiered cashback model.

**Feature Breakdown:**

- **Tiered Cashback System:**
  - **Requirement:** The system must allow the business owner to define multiple spending thresholds that unlock higher cashback rewards. This incentivizes customers to spend more to reach the next level.
  - **User Flow:** The owner goes to the "Loyalty Program" settings and defines the tiers.
    - **Example:**
      - **Tier 1:** For total purchases from 0 to 4,999,999 UZS, the customer earns **3% cashback** on each new purchase.
      - **Tier 2:** Once total purchases exceed 5,000,000 UZS, the customer starts earning **7% cashback**.
      - **Tier 3:** Once total purchases exceed 25,000,000 UZS, the customer starts earning **12% cashback**.
- **Cashback Redemption Rules:**
  - **Requirement:** The business owner must be able to set a rule for how much of a transaction can be paid for using the accumulated cashback balance. This protects the business's cash flow.
  - **User Flow:** A setting like "Maximum Purchase Amount Payable by Cashback" with a percentage field.
  - **Example:** If the limit is set to **50%**, and a customer has a 100,000 UZS cashback balance, on a 40,000 UZS purchase, they can only use 20,000 from their cashback balance (50% of the total).

**Edge Cases & Considerations:**

- **Onboarding Existing Customers:** How will the system handle existing customers when the loyalty program is first launched? There should be a way to assign them to a starting tier based on their past spending.
- **Cashback on Discounted Items:** The business owner should be able to decide if cashback is earned on the original price or the post-discount price of an item.
- **Returns:** If a customer returns an item, the cashback they earned from that purchase must be automatically deducted from their loyalty balance.
  - Note (MVP): Advanced loyalty features such as tiered cashback and points systems will be deferred from the initial MVP to reduce scope. The MVP will focus on core sales flows and provide the ability to apply simple discounts at the POS; full loyalty program functionality will be planned for a follow-up release.

This concludes the Client Management module. When you are ready, we can proceed to the final core module: **User & Permissions Management**, which is essential for any business with more than one employee.

Excellent. This is the final foundational module, and it's a critical one for security, accountability, and operational control. Let's define how you manage the people who will be using the OsonSavdo application.

### **Part 7: User & Permissions Management**

**Purpose:** To allow the business owner to create accounts for their employees, assign them to specific roles, and control exactly what each user can see and do within the application. This ensures that sensitive information is protected and that employees only have access to the tools necessary for their jobs.

**User Story:** As a business owner with several employees, I need to create separate logins for my manager and my cashiers, ensuring that cashiers can only make sales, while the manager can also view reports and manage inventory.

---

#### **7.1. Core Concepts: Roles and Permissions**

The system will be built on a "Role-Based Access Control" (RBAC) model. This is an industry-standard approach that is both powerful and easy to understand.

- **Permission:** A single, specific action a user can take. _Example: "Can create a new product," "Can view the sales report," "Can apply a discount over 15%."_
- **Role:** A collection of permissions that represents a job function. You create a role, assign permissions to it, and then assign users to that role. _Example: The "Cashier" role would have the permission "Can process a sale" but not "Can delete a product."_

This approach is highly efficient. If you need to change what all your cashiers can do, you simply edit the "Cashier" role once, and the change instantly applies to all users assigned to that role.

---

#### **7.2. Default System Roles**

To make setup easy, OsonSavdo will come with a set of pre-configured default roles that cover the most common business structures. These roles can be used as-is or customized.

**Feature Breakdown:**

- **Owner / Administrator:**
  - **Permissions:** Has complete, unrestricted access to every single feature, setting, and report in the application. This role can manage other users, change core business settings, and view all financial data.
  - **Purpose:** For the primary business owner(s).
- **Manager:**
  - **Permissions:** Has broad access to most operational features but is restricted from the most sensitive system settings.
    - **Can:** Add/edit products, manage inventory, view all sales reports, process returns, manage employee schedules (if that module is added later), and oversee daily cash drawer closures.
    - **Cannot:** Change core currency settings, delete the entire sales history, or create/delete other manager-level user accounts.
  - **Purpose:** For store managers or trusted senior employees who run the day-to-day operations.
- **Cashier / Sales Staff:**
  - **Permissions:** Has the most limited access, focused exclusively on the Point of Sale interface.
    - **Can:** Log in to the POS, process sales, add new customers (quick add), and perform basic returns.
    - **Cannot:** View comprehensive sales reports, see product cost prices, edit product details, or access any of the main settings modules.
  - **Purpose:** For all frontline sales employees.

---

#### **7.3. User Account Management**

This is the interface where the Administrator manages the list of all users.

**Feature Breakdown:**

- **User List:** A simple, clear list of all users with accounts in the system, showing their Name, assigned Role, and status (Active/Inactive).
- **Add New User:**
  - **User Flow:** The administrator clicks "Add User" and fills out a form with the employee's:
    1.  Full Name
    2.  A unique Login ID (e.g., their email address or a simple username).
    3.  A temporary password (the system should force the user to change this on their first login).
    4.  Assign them to a **Role** from a dropdown list (e.g., "Cashier").
- **Edit User:** The administrator can click on any user to update their details or change their assigned role.
- **Deactivate User:**
  - **Requirement:** When an employee leaves the company, their account must not be deleted. Deleting it would corrupt historical data (e.g., who made a specific sale). Instead, the account is **deactivated**.
  - **User Flow:** The administrator clicks "Deactivate." The user can no longer log in, but their name and all their past activity remain in the system for reporting purposes. This is a critical security and accounting feature.

---

#### **7.4. Custom Role Creation (Advanced Feature)**

For businesses with unique needs, the ability to create new roles from scratch is a powerful tool.

**Feature Breakdown:**

- **Role Management Interface:** A dedicated settings page where an Administrator can view all existing roles.
- **Create New Role:**
  - **User Flow:**
    1.  The administrator clicks "Create New Role" and gives it a name (e.g., "Inventory Clerk").
    2.  They are then presented with a checklist of _all possible permissions_ in the system, grouped by module (e.g., Sales, Products, Reports).
    3.  They check the boxes for every action this new role should be allowed to perform.
    4.  They save the role. It now appears in the dropdown list when creating or editing a user.
- **Example Use Case:** A business might want to hire a part-time "Inventory Clerk" whose only job is to add new products and update stock levels. They would create a role that only has permissions for the "Product Management" module and nothing else, ensuring this user cannot access the POS or see any sales data.

**Edge Cases & Considerations:**

- **Password Security:** The system must enforce strong password policies (e.g., minimum length, complexity) and provide a secure "Forgot Password" reset mechanism for all users.
- **Activity Logging:** For enhanced security, the system should maintain an internal audit log that records key actions taken by users, such as who changed a product's price or who exported the customer list. This is an advanced feature but crucial for accountability.
- **Multi-Store Access:** For businesses with multiple locations, permissions can be even more granular. For example, a Manager's role could be restricted to only see data and manage inventory for _their specific store_, while the Owner can see all stores.

This concludes the core modules necessary to build a robust and scalable business management application. We have covered the foundational settings, product and inventory management, bulk data import, the point of sale, reporting, client management, and user permissions.

With this comprehensive business idea document, you now have a clear and detailed blueprint to guide the development of OsonSavdo.

Excellent progress so far. Based on your request to include user authentication, subscription/billing, and a marketing landing page into the MVP scope, the following three parts are recommended to be added to the product roadmap. They are written as small, ordered, non-technical features that integrate with the existing modules (notably Part 1: Core System Settings, Part 3: Telegram integration, and Part 7: User & Permissions).

### Guiding principle for MVP additions

- Keep each new feature small and valuable: prefer simple, local-friendly solutions (phone-first authentication, subscription tiers that limit counts rather than fine-grained feature flags, a single responsive marketing landing page).
- Favor manual or semi-automated operations over full automation for the MVP (for example: manual exchange rate updates remain, billing notifications via Telegram initially).
- Make sure each new capability ties into existing flows: landing page → sign-up → authentication → onboarding → subscription → app access.

---

## Part 8: Authentication & Security (MVP)

Purpose: Provide a secure, user-friendly way for business owners, managers, and staff to create accounts and sign in to OsonSavdo. Make onboarding fast for local users in Uzbekistan while keeping sensitive operations protected.

Short summary of related existing features:

- Part 7 (User & Permissions) defines roles and RBAC that will be enforced after authentication.
- Part 1.3 (Telegram Bot Integration) can be used as an MFA and notification channel.

Feature breakdown (small, ordered steps):

1. Phone-based sign-up and sign-in (primary, MVP):

- Sign-up by phone number with one-time passcode (OTP, SMS). Phone-first experience is familiar and fast for local users.
- Minimal profile capture on sign-up: business name, phone number, and owner email (optional).
- Login via OTP (passwordless) as the default method for owners and cashiers.

2. Optional email/password for admin users (secondary):

- Allow setting an email and password for owners who prefer it.
- Enforce strong password rules and require password change on first sign-in when set by an administrator.

3. Account verification and onboarding flow:

- After sign-up, run a short guided setup: confirm currency & payment methods (Part 1), add first product or import a template (Part 3), configure POS basics.

4. Multi-factor and Telegram linking (MVP-light):

- Provide an optional step to link the account to a Telegram user (leveraging the Telegram Bot Manager flow from Part 1.3) for notifications and an alternative sign-in/verification channel.

5. Session and device handling:

- Keep sessions short-lived on public/shared devices and provide a simple "Log out everywhere" option for owners.

Acceptance criteria (what MVP must achieve):

- A user can create an account with a phone number and complete OTP verification.
- A user can log in with OTP and land in the guided onboarding flow.
- Administrators can optionally create email/password accounts for users, and the system enforces role-based access after login (ties to Part 7).
- Users can link Telegram for notifications and receive a test notification after linking.

Edge cases & considerations:

- SMS delivery failures: allow fallback to Telegram verification if the user links their Telegram account during onboarding, and capture retry/queue logic for OTP.
- Phone number reuse: when phone numbers are reassigned, require an account recovery step that verifies business ownership (manual verification may be needed for rare cases).

---

## Part 9: Subscriptions & Billing (MVP)

Purpose: Monetize OsonSavdo with simple, predictable subscription plans tailored for local businesses. Start with straightforward tiers and limits that are easy to understand and implement.

Short summary of related existing features:

- Subscription enforcement will primarily gate scale (number of stores, users, SKUs, or monthly transactions) rather than micro-feature toggles. This works cleanly with existing RBAC (Part 7) and inventory/product features (Parts 2–4).
- Use Telegram (Part 1.3) and email for billing notifications (invoices, renewal reminders) in the MVP.

High-level recommendation and rationale:

- Offer 3 simple tiers: Free, Basic, Pro. Each tier defines limits and a small set of included capabilities rather than toggling dozens of features. Example:
  - Free: Single store, up to 500 SKUs, 1 cashier user, basic reports, manual import.
  - Basic (paid): Up to 3 stores, 5 cashier users, 5,000 SKUs, exports & scheduled reports, priority Telegram notifications.
  - Pro (paid): Unlimited stores, up to 20 users, bulk import background jobs, advanced inventory reports, CSV/XLSX exports.

Feature breakdown (ordered MVP steps):

1. Billing model and plan definitions (administrative):

- Create an internal admin UI to define plan limits and descriptions (not public-facing). Keep the pricing table on the marketing page in sync manually at launch.

2. Subscription signup flow (customer-facing):

- From onboarding or the marketing landing page, allow the user to choose a plan and pay.
- Present a short confirmation and tie the subscription object to the user's account and organization.

3. Local payment options and fallback:

- Integrate one or two local-friendly payment options in MVP (for example Payme or Click) via the simplest possible integration (payment links or redirect flow).
- For manual/enterprise sales, provide an invoice option: owner chooses "Invoice me" and the system marks account as "Pending manual payment" until admin marks it paid.

4. Subscription enforcement in-app:

- If a tenant exceeds their plan limits, show a clear notice and an in-app upsell action (e.g., "Upgrade to Basic to add a second store").
- Implement a short grace period (e.g., 7 days) for failed payments before restricting write actions.

5. Billing notifications and receipts:

- Send payment confirmation, renewal reminders, failed payment alerts, and receipts via Telegram and email. Keep messages short and localized (Uzbek/Russian).

6. Cancellation and refunds policy (MVP-light):

- Allow users to cancel from their account. Document manual refund handling as a support workflow (initially refunds are handled by the team; later automate partial refunds if needed).

Acceptance criteria:

- A tenant can select a subscription plan, pay via a local payment method or request an invoice, and have the plan attached to their account.
- The system enforces plan limits and displays friendly upgrade prompts.
- Billing events trigger Telegram/email confirmations.

Edge cases & considerations:

- Recurring payments & failed charges: use straightforward retries and a grace period; log failures and notify the owner via Telegram.
- Taxes and receipts: for MVP, include a plain-language invoice/receipt and allow manual compliance handling by the business owner; do not attempt full tax automation for launch.
- Data portability: provide an easy export (CSV) for customers leaving the platform.

---

## Part 10: Marketing Landing Page & Lead Capture (MVP)

Purpose: Create a compact, localized marketing landing page that explains the product, shows pricing, captures leads, and funnels users into the sign-up/onboarding flow.

Why this belongs in MVP:

- Even a minimal product needs discoverability and a clear conversion path for new customers. A small landing page makes the product look credible and helps early customer acquisition.

Feature breakdown (small, ordered steps):

1. Single responsive landing page (mobile-first):

- Hero section with one-line value prop in Uzbek and Russian, short bullet list of top benefits, and a clear CTA button: "Start Free" or "View Pricing".

2. Pricing table & feature summary:

- Present Free/Basic/Pro plans with short, clear limits and a prominent purchase/signup CTA for each.

3. Lead capture form and contact CTA:

- Short form (Business name, Owner name, Phone, Optional Email) to capture leads. Store leads in a simple backend table and send an internal notification (Telegram or email) to the team.

4. Localized content and trust builders:

- Short customer testimonials, a small list of supported local payment methods (icons), and privacy/legal link (terms & contact).

5. SEO and analytics basics:

- Add meta tags, Open Graph tags, and integrate lightweight analytics (Google Analytics, or Matomo for privacy) to measure traffic and conversion.

6. CTA -> Onboarding: connect the CTA to the sign-up flow which uses phone-based OTP and takes the user through onboarding and subscription selection.

Acceptance criteria:

- The landing page is public, responsive, and localized into Uzbek and Russian.
- Visitors can submit a lead and be prompted to sign-up; the team receives a notification of new leads.
- CTAs link correctly into the sign-up/auth flow.

Edge cases & considerations:

- Slow connections: keep page assets small and optimize images for mobile.
- Privacy: ask only essential data in lead form and provide a link to privacy terms (even if brief) to build trust.

---

Next steps and suggested rollout order

1. Implement Authentication & Security (Part 8) first so all future features have a clear account model.
2. Add the Marketing Landing Page (Part 10) in parallel with Auth so you can start capturing leads and sign-ups early.
3. Build Subscriptions & Billing (Part 9) after Auth and Landing Page so new sign-ups can convert to paid plans during or after onboarding.

Quality gates and acceptance mapping

- Build: Verify sign-up/login flows for OTP and email/password; smoke test onboarding to reach the app home.
- Lint/Typecheck: Keep changes isolated to auth, billing, and the public landing page; ensure no regressions in the POS flow.
- Tests: Add unit tests for the auth flow (OTP generation/verification) and a small integration test that signs-up a user and attaches a subscription object.

Requirements coverage summary

- Add user authentication (phone-first + optional email/password): Done (documented in Part 8).
- Add subscription/billing (tiers, local payments, enforcement): Done (documented in Part 9).
- Add marketing landing page and lead capture: Done (documented in Part 10).

Completion note

I added three new, ordered, non-technical parts to the feature document that integrate naturally with existing parts of the product. If you'd like, I can:

- Draft the exact copy for the landing page in Uzbek and Russian.
- Produce a short PRD for the Subscription flow with visible wireframes and acceptance tests.
- Generate a minimal Next.js page component and API route scaffolding for the phone-based auth and landing page.

Which of those would you like me to do next?
