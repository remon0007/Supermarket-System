# Local Supermarket Management System

A complete C# ASP.NET Console application for managing small supermarket operations with product inventory, stock tracking, sales management, and reporting.

##  Project Overview

This system replaces manual paper-based records with a digital solution supporting:
-  Product Management (Add, Update, Remove, Search)
-  Stock Control & Low-Stock Alerts
-  Sales Tracking & Transactions
-  Supplier Management
-  Advanced Reporting
-  Custom Data Structures (BST, Hash Table)
-  SQL Server Database with Entity Framework
-  Comprehensive Unit Tests

---

## 📋 Requirements Fulfillment

### Core Requirements ✓
- [x] C# .NET Console Application
- [x] Custom Data Structures (Binary Search Tree, Hash Table)
- [x] SQL Server Database with Entity Framework
- [x] Intuitive Console UI
- [x] Time Complexity Analysis (COMPLEXITY_ANALYSIS.md)

### Functional Requirements ✓
- [x] Store product details (ID, Name, Brand, Barcode, Price, Quantity, Category, Supplier, Expiry Date)
- [x] CRUD operations for products, suppliers, and stock
- [x] Two search algorithms implemented (BST for name, Hash Table for barcode)
- [x] Stock level tracking and low-stock alerts
- [x] Sales transaction recording
- [x] Multiple reports (Low Stock, Sales, Category, Supplier)

### Technical Requirements ✓
- [x] C# .NET implementation
- [x] Custom data structures (no built-in collections for main structure)
- [x] SQL Server + Entity Framework
- [x] Data validation (unique IDs, positive prices, valid dates)
- [x] Unit tests (21 test cases)
- [x] Time complexity analysis documentation

---

## 📁 Project Structure

```
SupermarketSystem/
├── Program.cs                          # Entry point
├── SupermarketSystem.csproj            # Project file
│
├── Models/
│   └── Entities.cs                     # EF Core entities
│
├── DataStructures/
│   └── CustomDataStructures.cs         # BST & Hash Table
│
├── Data/
│   └── SupermarketDbContext.cs         # Database context
│
├── Repositories/
│   └── Repositories.cs                 # Data access layer
│
├── Services/
│   └── SupermarketService.cs           # Business logic
│
├── UI/
│   └── SupermarketUI.cs                # Console interface
│
├── Tests/
│   └── SupermarketServiceTests.cs      # 21 unit tests
│
├── COMPLEXITY_ANALYSIS.md              # Time complexity analysis
└── README.md                           # This file
```

---

##  Setup & Installation

### Prerequisites
- .NET 6.0 SDK or higher
- SQL Server (LocalDB or Express)
- Visual Studio 2022 or VS Code

### Installation Steps

1. **Clone/Download the project:**
   ```bash
   cd SupermarketSystem
   ```

2. **Restore NuGet packages:**
   ```bash
   dotnet restore
   ```

3. **Create database (Entity Framework Migration):**
   ```bash
   dotnet ef database update
   ```
   This will create `SupermarketDB` in your LocalDB instance.

4. **Run the application:**
   ```bash
   dotnet run
   ```

5. **Run tests:**
   ```bash
   dotnet test
   ```

---

##  Database Setup

### Connection String
Located in `SupermarketDbContext.cs`:
```csharp
@"Server=(localdb)\mssqllocaldb;Database=SupermarketDB;Trusted_Connection=true;"
```

### Seed Data
Default data is automatically created:

**Categories:**
- Dairy
- Beverages
- Snacks
- Frozen

**Suppliers:**
- Dairy Farm Co
- Beverage Distributor
- Snack Supplies Inc

**Products:** 5 sample products with stock

---

## User Interface

### Main Menu
```
╔══════════════════════════════════════════════════════╗
║   LOCAL SUPERMARKET MANAGEMENT SYSTEM                ║
╚══════════════════════════════════════════════════════╝

[1] Product Management
[2] Supplier Management
[3] Sales Management
[4] Reports
[5] Search Products
[0] Exit
```

### Key Features

#### 1. Product Management
- **Add Product** - Input: Name, Barcode, Brand, Price, Quantity, Category, Supplier, Expiry Date
- **View All Products** - Display all products with stock status
- **Update Stock** - Modify quantity for existing product
- **Remove Product** - Delete product from system
- **Low Stock Items** - View products below 10 units

#### 2. Supplier Management
- **View All Suppliers** - List all suppliers with contact info
- **Supplier Stock List** - See all products from specific supplier

#### 3. Sales Management
- **Record Sale** - Create transaction with multiple items
- **Automatic Stock Deduction** - Stock reduces automatically on sale

#### 4. Reports
- **Low Stock Report** - Products needing reorder
- **Sales Report** - Revenue and transaction history
- **Category Report** - Products grouped by category
- **Supplier Report** - Inventory by supplier

#### 5. Search Products
- **By Name (BST - O(log n))** - Fast name-based search
- **By Barcode (Hash Table - O(1))** - Instant barcode lookup
- **By Category (Linear - O(n))** - Filter by category
- **By Supplier (Linear - O(n))** - Filter by supplier

---

##  Data Structures

### 1. Binary Search Tree (ProductBST)
**Used For:** Fast product name searching
- **Average Time:** O(log n)
- **Worst Time:** O(n) if unbalanced
- **Space:** O(n)

```csharp
public class ProductBST
{
    public void Insert(Product product)         // O(log n)
    public List<Product> SearchByName(string) // O(log n)
    public List<Product> GetAll()             // O(n)
}
```

### 2. Hash Table (BarcodeHashTable)
**Used For:** Ultra-fast barcode lookups
- **Average Time:** O(1)
- **Collision Handling:** Chaining
- **Table Size:** 100 buckets

```csharp
public class BarcodeHashTable
{
    public void Add(string barcode, Product)    // O(1)
    public Product Search(string barcode)       // O(1)
    public void Remove(string barcode)          // O(1)
}
```

---

## Search Algorithms

### Algorithm 1: BST Search by Name
```
SearchProductByName("Milk")
├── Compare "Milk" with root
├── If less, search left subtree
├── If greater, search right subtree
└── Collect matching results
Time: O(log n) average
```

### Algorithm 2: Hash Table Search by Barcode
```
SearchProductByBarcode("1001")
├── Hash barcode to index: hash("1001") = 42
├── Access bucket 42
├── Linear search in bucket (usually 1 item)
└── Return product or null
Time: O(1) average
```

---

##  Unit Tests (21 Test Cases)

Run tests with:
```bash
dotnet test
```

### Test Coverage
- **Product Operations:** Add, Update, Remove (3 tests)
- **Search Algorithms:** Name, Barcode, Category, Supplier (4 tests)
- **Stock Management:** Low stock, Restock (2 tests)
- **Sales:** Record sale, Validation, Stock deduction (3 tests)
- **Data Structures:** BST, Hash Table operations (4 tests)
- **Validation:** Price, Quantity, Duplicate barcode (4 tests)

### Example Tests
```csharp
[Fact]
public void SearchProductByBarcode_ExistingBarcode_ReturnsProduct()
{
    var service = new SupermarketService();
    var product = service.SearchProductByBarcode("1001");
    Assert.NotNull(product);
}

[Fact]
public void RecordSale_InsufficientStock_ThrowsException()
{
    var service = new SupermarketService();
    var items = new List<(int, int)> { (1, 10000) };
    Assert.Throws<ArgumentException>(() => service.RecordSale(items));
}
```

---

##  Reports

### Low Stock Report
Shows products with quantity < 10 units
```
╔════════════════════════════════════════════════════════════╗
║           LOW STOCK ALERT REPORT                            ║
╚════════════════════════════════════════════════════════════╝
ID    Product Name            Quantity  Status
─────────────────────────────────────────────────────────────
4     Potato Chips            8         LOW
```

### Sales Report
Shows all transactions with total revenue
```
╔════════════════════════════════════════════════════════════╗
║              SALES TRANSACTION REPORT                      ║
╚════════════════════════════════════════════════════════════╝
Sale ID   Date                 Items      Total
─────────────────────────────────────────────────
1         27-Jun-2026 10:30    5          $24.95
────────────────────────────────────────────────
Total Revenue: $24.95
```

---

##  Data Validation

| Field | Validation |
|-------|-----------|
| Product ID | Unique, auto-generated |
| Barcode | Unique, required |
| Name | Required, max 150 chars |
| Price | > 0, decimal format |
| Quantity | ≥ 0, integer |
| Expiry Date | Optional, any past/future date |
| Supplier | Must exist |
| Category | Must exist |

---

##  Database Schema

### Tables
- **Products** - Core product information
- **Categories** - Product categories
- **Suppliers** - Supplier details
- **Stock** - Stock transaction history
- **Sales** - Sales transactions
- **SaleItems** - Line items in sales

### Key Relationships
```
Product ──┬── Category (Many-to-One)
          ├── Supplier (Many-to-One)
          └── SaleItem (One-to-Many)

Sale ──── SaleItem ──── Product

Stock ── Product
```

---

##  Performance Metrics

### Search Performance
| Operation | n=100 | n=1000 | n=10000 |
|-----------|-------|--------|---------|
| Name Search (BST) | 7 ops | 10 ops | 14 ops |
| Barcode Search | 1 op | 1 op | 1 op |
| Category Filter | 100 ops | 1K ops | 10K ops |

### Scalability
- **Optimal:** 100-1000 products
- **Good:** 1000-10000 products
- **Recommended:** Implement caching beyond 10K products

---

##  Troubleshooting

### Issue: Database not found
**Solution:**
```bash
dotnet ef database update
```

### Issue: Connection string error
**Check:** SQL Server LocalDB is installed
```bash
sqllocaldb info
```

### Issue: Port conflicts
**Solution:** Modify connection string in `SupermarketDbContext.cs`

---

##  Additional Resources

- **Complexity Analysis:** See `COMPLEXITY_ANALYSIS.md`
- **API Documentation:** Inline code comments
- **Entity Framework Docs:** https://learn.microsoft.com/ef/

---

##  Learning Outcomes

This project demonstrates:
- ✓ Data structure design (BST, Hash Tables)
- ✓ Algorithm complexity analysis (O notation)
- ✓ Entity Framework ORM
- ✓ Console application development
- ✓ Repository pattern
- ✓ Unit testing with xUnit
- ✓ SQL database design
- ✓ Input validation and error handling

---

##  License

Educational project for supermarket management demonstration.

---

##  Author
Local Supermarket Management System v1.0
Designed for small retail operations

---

**Last Updated:** June 2026
**Status:** Production Ready ✓
