# What should I do?

## Create the SUPPLIER in the system
Company name
Contact name
Contact title
Phone number

## Create a screen that shows all suppliers
A screen with a table listing all of them

> USE the PRD code as an example and change PRODUCT >> SUPPLIER

> Change the column names

## Create the NEW function for the existing supplier
Same idea as the create screen, but for editing
It doesn't have an ID yet, so it's an Unvalidated

## Create the EDIT function for the existing supplier
Same idea as the create screen, but for editing
Since it will already be editing an existing supplier, it now has an ID (Validated)

## Create a DOUBLE CLICK on the table to open the edit screen
When you double-click on a supplier, open their form

## SHOW and HIDE the delete button
If the supplier HAS products >> cannot delete
If it DOES NOT have products >> can delete

deleteButton.setVisible(viewModel.canDelete());
deleteButton.setManaged(viewModel.canDelete());

This is the equivalent of:

if (canDelete) { 
deleteButton.setVisible(true);
} else { 
deleteButton.setVisible(false);
}

## Make the SAVE function work
Save new suppliers and save changes to existing suppliers
Move to the RIGHT

## Make the DELETE function work
Delete the supplier from the database (when allowed)

## UPDATE the product section
Each product now needs to have a supplier chosen
The product screen needs to have a menu to choose the supplier
The product list should show the supplier's company name
When editing a product, the correct supplier should already be selected

## UPDATE the program MENU
VIEW >> supplier
NEW >> supplier

## Create the VALIDATION RULES
Company name (required)
Contact name (required)
Phone number (required)
Contact title (optional)
Phone numbers can only have numbers and some symbols (no spaces)

# CHECKLIST OF STEPS:
Create supplier // OK
List screen // ok
New screen // OK
Edit screen // OK
Double click // OK
Show/hide delete // OK
Save // ​​OK
Delete // OK
Update products //ok
Update menu // ok
Data validation ok

# Understanding the project files

SALES FOLDER: MAIN
CORE: CENTRAL TYPES (description of what category is) "defines the data (models)"
FEATURE: COMPLETE FUNCTIONALITIES (validation, screen, control) "and what makes it work"
BASE FEATURE: Use as an example (ready-made examples from the professor)
CATEGORY FEATURE: Use as an example to create the supplier

ui > JavaFX screens

validation > validation rules

CategoryController > controls the screens

CategoryRepository > communicates with the database
PRODUCT FEATURE: Same as the CATEGORY FEATURE folder but for products

# Where should I create the SUPPLIER files?

SupplierData > goes to core
SupplierController > go to feature/supplier
SupplierRepository > go to feature/supplier
SupplierNewView, SupplierEditView, SuppliersView > go to feature/supplier/ui
SupplierValidator > go to feature/supplier/validation

# Supplier Fields:
companyName
contactName
contactTitle
phone