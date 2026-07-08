# Instructions

- Following Playwright test failed.
- Explain why, be concise, respect Playwright best practices.
- Provide a snippet of code with the fix, if possible.

# Test info

- Name: LoginDataDriven.spec.ts >> Login Test with JSON Data: Valid login @datadriven
- Location: tests\LoginDataDriven.spec.ts:14:7

# Error details

```
Error: page.goto: net::ERR_NAME_NOT_RESOLVED at https://tutorialsninja.com/demo/
Call log:
  - navigating to "https://tutorialsninja.com/demo/", waiting until "load"

```

# Test source

```ts
  1  | import { test, expect } from "@playwright/test";
  2  | import { LoginPage } from "../pages/LoginPage";
  3  | import { MyAccountPage } from "../pages/MyAccountPage";
  4  | import { DataProvider } from "../utils/dataProvider";
  5  | import { TestConfig } from "../test.config";
  6  | import { HomePage } from "../pages/HomePage";
  7  | 
  8  | //Load JSON test data logindata.json
  9  | 
  10 | const jsonPath = "testdata/logindata.json";
  11 | const jsonTestData = DataProvider.getTestDataFromJson(jsonPath);
  12 | 
  13 | for (const data of jsonTestData) {
  14 |   test(`Login Test with JSON Data: ${data.testName} @datadriven`, async ({
  15 |     page,
  16 |   }) => {
  17 |     const config = new TestConfig(); // create instance
> 18 |     await page.goto(config.appUrl); // getting appURL from test.config.ts file
     |                ^ Error: page.goto: net::ERR_NAME_NOT_RESOLVED at https://tutorialsninja.com/demo/
  19 | 
  20 |     const homePage = new HomePage(page);
  21 |     await homePage.clickMyAccount();
  22 |     await homePage.clickLogin();
  23 | 
  24 |     const loginPage = new LoginPage(page);
  25 |     await loginPage.login(data.email, data.password);
  26 | 
  27 |     if (data.expected.toLowerCase() === "success") {
  28 |       const myAccountPage = new MyAccountPage(page);
  29 |       const isLoggedIn = await myAccountPage.isMyAccountPageExists();
  30 |       expect(isLoggedIn).toBeTruthy();
  31 |     } else {
  32 |       const errorMessage = await loginPage.getloginErrorMessage();
  33 |       //expect(errorMessage).toBe('Warning: No match for E-Mail Address and/or Password.');
  34 |       expect(errorMessage).toContain("Warning: No match");
  35 |     }
  36 |   });
  37 | }
  38 | 
  39 | //Load CSV test data logindata.json
  40 | 
  41 | const csvPath = "testdata/logindata.csv";
  42 | const csvTestData = DataProvider.getTestDataFromCsv(csvPath);
  43 | 
  44 | for (const data of csvTestData) {
  45 |   test(`Login Test with CSV Data: ${data.testName} @datadriven`, async ({
  46 |     page,
  47 |   }) => {
  48 |     const config = new TestConfig(); // create instance
  49 |     await page.goto(config.appUrl); // getting appURL from test.config.ts file
  50 | 
  51 |     const homePage = new HomePage(page);
  52 |     await homePage.clickMyAccount();
  53 |     await homePage.clickLogin();
  54 | 
  55 |     const loginPage = new LoginPage(page);
  56 |     await loginPage.login(data.email, data.password);
  57 | 
  58 |     if (data.expected.toLowerCase() === "success") {
  59 |       const myAccountPage = new MyAccountPage(page);
  60 |       const isLoggedIn = await myAccountPage.isMyAccountPageExists();
  61 |       expect(isLoggedIn).toBeTruthy();
  62 |     } else {
  63 |       const errorMessage = await loginPage.getloginErrorMessage();
  64 |       //expect(errorMessage).toBe('Warning: No match for E-Mail Address and/or Password.');
  65 |       expect(errorMessage).toContain("Warning: No match");
  66 |     }
  67 |   });
  68 | }
  69 | 
```