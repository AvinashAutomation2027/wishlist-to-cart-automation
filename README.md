# Scenario 5 - Wishlist to Cart Validation

## Source scenario
 
**Wishlist to Cart Validation** against `https://demowebshop.tricentis.com/`.

The PDF requires:
- Page Object Model
- explicit/fluent waits; no hardcoded `Thread.sleep`
- dynamic data
- real assertions for every validation
- structured logging/reporting
- README and GitHub submission
- meaningful incremental commits

## Tech stack
- Java 17
- Selenium WebDriver 4.25.0
- TestNG 7.10.2
- Maven
- SLF4J simple logging
- Chrome
- Selenium Manager (ChromeDriver is resolved automatically by Selenium)

## Project structure

```text
wishlist-to-cart-automation/
├── pom.xml
├── testng.xml
├── README.md
├── .gitignore
└── src/
    └── test/
        ├── java/com/dyad/automation/
        │   ├── base/
        │   │   ├── BasePage.java
        │   │   └── BaseTest.java
        │   ├── pages/
        │   │   ├── HomePage.java
        │   │   ├── RegisterPage.java
        │   │   ├── SearchResultsPage.java
        │   │   ├── WishlistPage.java
        │   │   └── CartPage.java
        │   ├── tests/
        │   │   └── WishlistToCartTest.java
        │   └── utils/
        │       ├── ConfigReader.java
        │       └── TestData.java
        └── resources/
            └── config.properties
```

## Prerequisites on Windows

1. JDK 17 installed.
2. Eclipse installed.
3. Maven installed and `mvn -version` works in Command Prompt.
4. Google Chrome installed.
5. Git installed and `git --version` works.

The project uses Selenium Manager, so you do not need to manually download chromedriver for normal Selenium 4 execution.

## Import into Eclipse

1. Open Eclipse.
2. `File` -> `Import`.
3. Choose `Maven` -> `Existing Maven Projects`.
4. Select the project root folder.
5. Click `Finish`.
6. Right-click project -> `Maven` -> `Update Project`.
7. Select the project and click `OK`.

## Run from Eclipse

Recommended:
1. Right-click `testng.xml`.
2. `Run As` -> `TestNG Suite`.

Or:
1. Right-click `WishlistToCartTest.java`.
2. `Run As` -> `TestNG Test`.

## Run from Command Prompt

Open Command Prompt in the project root:

```bash
mvn clean test
```

Run headless:

```bash
mvn clean test -Dheadless=true
```

Run with another browser setting if later supported:

```bash
mvn clean test -Dbrowser=chrome
```

## Reports

After Maven execution:
- Surefire results: `target/surefire-reports/`
- TestNG HTML report from TestNG/Surefire execution is available under the Maven report output.

When running directly in Eclipse, open the TestNG Results view.

## Scenario implementation

1. Registration uses a generated unique email on every run.
2. Search uses the required keyword `Book`.
3. Two products whose names contain `Book` are selected dynamically.
4. Products are added to the Wishlist.
5. Wishlist count is asserted as 2.
6. Both products are selected and moved to the Shopping Cart.
7. One cart quantity is changed to 2.
8. Subtotal is calculated from the live unit price x live quantity values and compared with the displayed subtotal.
9. An intentionally invalid coupon is applied and the UI error is asserted.
10. Logout is performed and the logout link is verified as no longer visible.

## Important assumptions

- The assessment's target website is `https://demowebshop.tricentis.com/`.
- The target site is a public demo site and its DOM can change. Page locators are therefore centralized in page objects.
- The implementation deliberately does not hardcode product names or prices.
- The test uses one end-to-end TestNG method because the PDF defines a single dependent business journey.
- If the demo site's HTML changes, update only the affected page object's locators where possible.

## Git / GitHub

Create a repository on GitHub first, for example:
`wishlist-to-cart-validation`

Then in this project folder:

```bash
git init
git branch -M main
git add .
git commit -m "Initialize Maven Selenium TestNG framework"
git remote add origin https://github.com/YOUR_USERNAME/wishlist-to-cart-validation.git
git push -u origin main
```

Recommended incremental commits:

```bash
git add .
git commit -m "Add POM framework and configuration"

git add .
git commit -m "Implement registration and dynamic book search"

git add .
git commit -m "Implement wishlist to cart migration"

git add .
git commit -m "Implement quantity and subtotal validation"

git add .
git commit -m "Implement invalid coupon validation and logout"

git add .
git commit -m "Add README and test execution documentation"

git push
```

Do not commit real passwords, API keys, or personal credentials.

## Troubleshooting

### `mvn is not recognized`
Install Maven and add Maven's `bin` directory to Windows PATH.

### `java is not recognized`
Install JDK 17 and configure `JAVA_HOME`, then reopen Command Prompt.

### ChromeDriver error
Selenium 4.25 uses Selenium Manager. Make sure Chrome is installed and internet access is available the first time Selenium Manager needs the browser driver.

### Element not found
Open the target website manually and inspect the page. The public demo site's DOM can change. Update the locator in the relevant page class instead of adding sleeps.

### Coupon validation fails
The PDF specifies that an invalid coupon must show an appropriate error message, but it does not specify the exact website message. The implementation therefore validates that the returned message identifies the coupon as rejected rather than hardcoding one exact sentence.
