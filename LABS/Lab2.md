
## 1. Lab Objective

### A. UI Tests

- Test `CategoryNewView`
- Test `CategoriesView`
- Verify callbacks and displayed data

### B. Repository Tests (SQLite)

- Create a temporary database
- Test: create > update > delete

### C. Controller Tests

- Create a mock of the repository
- Create a stub of MainWindow
- Verify controller method calls

---

## 2. What Needs to Be Done?

### A. UI

- Ensure ViewModel data appears correctly
- Ensure buttons trigger the correct callbacks
- Test user interaction (clicking SAVE)

---

### B. Repository

- Create a temporary database for tests
- Test operations: create; update; delete; all

---

### C. Controller

- Verify navigation between screens
- Verify repository calls
- Verify interaction with MainWindow (title and scene)

---

## 3. Delivery Checklist

### A. UI Tests

- [ ] SAVE button test
- [ ] Callback data sending test
- [ ] Table displaying data test

### B. Repository Tests

- [ ] Temporary database created
- [ ] Create test
- [ ] Update test
- [ ] Delete test
- [ ] List (all) test

### C. Controller Tests

- [ ] Repository mock created
- [ ] MainWindow stub created
- [ ] showCategories test
- [ ] Window title verification
- [ ] Scene switching verification

---

## 4. Project Structure

 `main/java` — application
 `test/java` — tests
 `core` — models (not tested)

---

## 5. Components Used
### 5.1 createScene(ViewModel vm)

- Pure function
- Returns a Node
- Does not open a window
- Easy to test

---

### 5.2 ViewModel

- Contains data and callbacks: name; description; onSave

---

### 5.3 CategoryRepository

- SQLite access
- Methods: create; update; delete; all

---

### 5.4 CategoryController

- Controls application flow
- Methods:
  - showCategories
  - newCategory
  - saveCategory

---

### 5.5 MainWindowStub

- Replaces the real UI during tests

`String lastTitle;
Node lastScene;
Exception lastError;`
## 6. Implemented Tests

### 6.1 UI — Save sends data correctly

Uses `AtomicReference` to capture the callback.  
Simulates user input.  
Verifies the values sent to the callback.

`AtomicReference<Category.Unvalidated> received = new AtomicReference<>();`
`var vm = new CategoryNewView.ViewModel(
    "", "",
    unvalidated -> received.set(unvalidated));`

`nameField.setText("Beverages");
descField.setText("Liquid products");
saveButton.fire();`

`assertThat(received.get().name()).isEqualTo("Beverages");`

### 6.2 UI — Table displays data

Uses lookup("#id") to find the table.

Verifies that the table contains exactly the items from the ViewModel.

`var table = (TableView<Category>) scene.lookup("#categoriesTable");`

`assertThat(table.getItems()).containsExactlyElementsOf(list);`

### 6.3 Repository — Temporary database
Creates a new database for each test.

Prevents interference between tests.

`tempDb = Files.createTempFile("northwind-test", ".db");
Files.copy(original, tempDb, StandardCopyOption.REPLACE_EXISTING);`

### 6.4 Controller — Updates window
Verifies that the controller sets the correct window title.

Verifies that the scene is updated.

`controller.showCategories();
assertThat(window.lastTitle).isEqualTo("Categories");`

### 7. Problems and Solutions
A. NullPointer in UI
Cause: missing fx:id in UI elements
Solution: add the correct ids

B. Callback not working
Cause: normal variable cannot capture callback values
Solution: use AtomicReference

C. Controller not updating scene
Cause: incomplete MainWindowStub (missing lastScene)
Solution: add lastScene and override setMainScene

D. Repository failing between tests
Cause: shared database being modified by multiple tests
Solution: create a temporary database in BeforeEach

8. Key Points
- UI is a pure function (easy to test)
- Controller should be tested with mock + stub
 - Repository must use an isolated temporary database
- Always use fx:id for UI lookup
 -Use AtomicReference to capture callbacks
- Tests should be small, direct, and independent


# 1. Comparative

## Mock vs Stub vs Pure Function

| Type | What it is | Where I used it | Reason |
|------|------------|-----------------|--------|
| Mock | Fake object that records method calls | CategoryRepository | Check if the controller called the correct methods |
| Stub | Simple implementation that captures data | MainWindowStub | Avoid opening a real window during tests |
| Pure function | Does not depend on external state | createScene | Easy to test, does not require JavaFX running |

---

## Types of Tests

| Type | What it tests | Depends on | Example |
|------|---------------|------------|---------|
| UI | Layout + callbacks | ViewModel | SAVE button sends data to callback |
| Controller | Navigation logic | Mock + Stub | showCategories updates the window title |
| Repository | SQLite database | Temporary file | create/update/delete working correctly |

---

# 2. Important Functions and Methods (explained individually)


## createScene(ViewModel vm)

**Where it appears:** `CategoryNewView` and `CategoriesView`  
**Type:** pure function  

**What it does:**
- Builds the interface  
- Connects UI fields to the ViewModel  
- Returns a Node (does not open a window)

**Why it is easy to test:**
- Does not depend on external state  
- Does not use the database  
- Does not open a real window  

---

## ViewModel

**Purpose:** hold data + callbacks  

Example from `CategoryNewView`:

| Field | Type | Purpose |
|-------|------|---------|
| name | String | text typed by the user |
| description | String | text typed by the user |
| onSave | callback | function executed when SAVE is clicked |

---

## CategoryRepository

**Purpose:** access the SQLite database  


| Method | What it does |
|--------|--------------|
| create(Unvalidated) | inserts a category |
| update(id, Unvalidated) | updates a category |
| delete(id) | removes a category |
| all() | lists all categories |

---

## CategoryController

**Purpose:** coordinate everything

- Calls the repository  
- Switches scenes  
- Sets window title  
- Creates ViewModels  
- Handles errors  

Important methods:

| Method | What it does |
|--------|--------------|
| showCategories() | shows the list of categories |
| newCategory() | opens the create category screen |
| saveCategory() | saves a category using the repository |

---

## MainWindowStub

**Purpose:** replace the real window during tests  

**Why:**  
- The controller calls `setTitle`, `setMainScene`, `showError`  
- We do not want to open a real window in tests  

Fields needed:

```java
String lastTitle;
Node lastScene;
Exception lastError;
