
from selenium import webdriver
from selenium.webdriver.common.by import By
from selenium.webdriver.support.ui import Select

driver = webdriver.Chrome()
driver.get("https://assertqa.com/practice/webtables")
driver.maximize_window()

# TC01: Print all column headings
headings = driver.find_elements(By.XPATH, "//table/thead/tr/th")

print("TC01 - Column Headings")
for heading in headings:
    print(heading.text)

# TC02: Print the first data row
rows = driver.find_elements(By.XPATH, "//table/tbody/tr")

print("\nTC02 - First Data Row")
if rows:
    print(rows[0].text)

# TC03: Print the last data row
print("\nTC03 - Last Data Row")
if rows:
    print(rows[-1].text)

# TC04: Search for an employee by last name
search = driver.find_element(
    By.XPATH, "//input[contains(@placeholder,'Search')]"
)
search.send_keys("Smith")

print("\nTC04 - Search by Last Name")
results = driver.find_elements(By.XPATH, "//table/tbody/tr")

for row in results:
    print(row.text)

search.clear()

# TC05: Extract all email addresses
emails = driver.find_elements(
    By.XPATH, "//table/tbody/tr/td[3]"
)

print("\nTC05 - Email Addresses")
for email in emails:
    print(email.text)

# TC06: Find the employee with the highest Due amount
print("\nTC06 - Highest Due Amount")

headers = driver.find_elements(By.XPATH, "//table/thead/tr/th")
due_column = -1

for i, header in enumerate(headers):
    if header.text.strip().lower() == "due":
        due_column = i + 1
        break

if due_column != -1:
    rows = driver.find_elements(By.XPATH, "//table/tbody/tr")
    highest_due = -1
    employee = ""

    for row in rows:
        cells = row.find_elements(By.TAG_NAME, "td")
        amount = float(
            cells[due_column - 1].text.replace("$", "").replace(",", "")
        )

        if amount > highest_due:
            highest_due = amount
            employee = row.text

    print("Employee:", employee)
    print("Highest Due:", highest_due)
else:
    print("FAIL - Due column not found")

# TC07: Verify that a website link exists
print("\nTC07 - Website Link Verification")

links = driver.find_elements(
    By.XPATH, "//table//a[@href]"
)

if links:
    print("PASS - Website link exists")
    for link in links:
        print(link.get_attribute("href"))
else:
    print("FAIL - Website link not found")

# TC08: Count data rows without the header
rows = driver.find_elements(By.XPATH, "//table/tbody/tr")

print("\nTC08 - Data Row Count")
print("Number of displayed data rows:", len(rows))

driver.quit()
