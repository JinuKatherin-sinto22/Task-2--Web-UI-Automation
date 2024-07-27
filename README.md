//Initialize a Maven project
Add dependencies to your pom.xml file
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>
    <groupId>com.banking</groupId>
    <artifactId>/Task-2--Web-UI-Automation</artifactId>
    <version>1.0-SNAPSHOT</version>
    <dependencies>
        <dependency>
            <groupId>org.seleniumhq.selenium</groupId>
            <artifactId>selenium-java</artifactId>
            <version>4.1.0</version>
        </dependency>
        <dependency>
            <groupId>org.testng</groupId>
            <artifactId>testng</artifactId>
            <version>7.4.0</version>
            <scope>test</scope>
        </dependency>
        <dependency>
            <groupId>io.github.bonigarcia</groupId>
            <artifactId>webdrivermanager</artifactId>
            <version>5.0.3</version>
        </dependency>
    </dependencies>
</project>
//Create BaseTest.java
package com.banking.utils;

import io.github.bonigarcia.wdm.WebDriverManager;
import org.openqa.selenium.WebDriver;
import org.openqa.selenium.chrome.ChromeDriver;
import org.testng.annotations.AfterMethod;
import org.testng.annotations.BeforeMethod;

public class BaseTest {
    protected WebDriver driver;

    @BeforeMethod
    public void setUp() {
        WebDriverManager.chromedriver().setup();
        driver = new ChromeDriver();
        driver.manage().window().maximize();
        driver.get("https://www.globalsqa.com/angularJs-protractor/BankingProject/#/login");
    }

    @AfterMethod
    public void tearDown() {
        if (driver != null) {
            driver.quit();
        }
    }
}
//Create CustomerLoginTest.java//
package com.banking.tests;

import com.banking.utils.BaseTest;
import org.openqa.selenium.By;
import org.openqa.selenium.WebElement;
import org.testng.Assert;
import org.testng.annotations.Test;

public class CustomerLoginTest extends BaseTest {

    @Test
    public void verifyCustomerLogin() {
        // Click on "Customer Login"
        WebElement customerLoginButton = driver.findElement(By.cssSelector("button[ng-click='customer()']"));
        customerLoginButton.click();

        // Select a customer from the dropdown
        WebElement customerDropdown = driver.findElement(By.id("userSelect"));
        customerDropdown.click();
        WebElement customerName = driver.findElement(By.xpath("//option[text()='Harry Potter']"));
        customerName.click();

        // Click on "Login"
        WebElement loginButton = driver.findElement(By.cssSelector("button[type='submit']"));
        loginButton.click();

        // Verify login success
        WebElement welcomeMessage = driver.findElement(By.cssSelector("span[ng-show='user']"));
        Assert.assertTrue(welcomeMessage.isDisplayed(), "Login was not successful.");
    }
}
//Create DepositTest.java
package com.banking.tests;

import com.banking.utils.BaseTest;
import org.openqa.selenium.By;
import org.openqa.selenium.WebElement;
import org.testng.Assert;
import org.testng.annotations.BeforeMethod;
import org.testng.annotations.Test;

public class DepositTest extends BaseTest {

    @BeforeMethod
    public void loginAsCustomer() {
        // Click on "Customer Login"
        WebElement customerLoginButton = driver.findElement(By.cssSelector("button[ng-click='customer()']"));
        customerLoginButton.click();

        // Select a customer from the dropdown
        WebElement customerDropdown = driver.findElement(By.id("userSelect"));
        customerDropdown.click();
        WebElement customerName = driver.findElement(By.xpath("//option[text()='Harry Potter']"));
        customerName.click();

        // Click on "Login"
        WebElement loginButton = driver.findElement(By.cssSelector("button[type='submit']"));
        loginButton.click();
    }

    @Test
    public void verifyDeposit() {
        // Click on "Deposit"
        WebElement depositTab = driver.findElement(By.cssSelector("button[ng-click='deposit()']"));
        depositTab.click();

        // Enter amount and deposit
        WebElement amountInput = driver.findElement(By.cssSelector("input[ng-model='amount']"));
        amountInput.sendKeys("100");
        WebElement depositButton = driver.findElement(By.cssSelector("button[type='submit']"));
        depositButton.click();

        // Verify success message
        WebElement successMessage = driver.findElement(By.cssSelector("span[ng-show='message']"));
        Assert.assertTrue(successMessage.isDisplayed(), "Deposit was not successful.");
    }
}
//Create WithdrawalTest.java
package com.banking.tests;

import com.banking.utils.BaseTest;
import org.openqa.selenium.By;
import org.openqa.selenium.WebElement;
import org.testng.Assert;
import org.testng.annotations.BeforeMethod;
import org.testng.annotations.Test;

public class WithdrawalTest extends BaseTest {

    @BeforeMethod
    public void loginAsCustomer() {
        // Click on "Customer Login"
        WebElement customerLoginButton = driver.findElement(By.cssSelector("button[ng-click='customer()']"));
        customerLoginButton.click();

        // Select a customer from the dropdown
        WebElement customerDropdown = driver.findElement(By.id("userSelect"));
        customerDropdown.click();
        WebElement customerName = driver.findElement(By.xpath("//option[text()='Harry Potter']"));
        customerName.click();

        // Click on "Login"
        WebElement loginButton = driver.findElement(By.cssSelector("button[type='submit']"));
        loginButton.click();
    }

    @Test
    public void verifyWithdrawal() {
        // Click on "Withdrawl"
        WebElement withdrawalTab = driver.findElement(By.cssSelector("button[ng-click='withdrawl()']"));
        withdrawalTab.click();

        // Enter amount and withdraw
        WebElement amountInput = driver.findElement(By.cssSelector("input[ng-model='amount']"));
        amountInput.sendKeys("50");
        WebElement withdrawButton = driver.findElement(By.cssSelector("button[type='submit']"));
        withdrawButton.click();

        // Verify success message
        WebElement successMessage = driver.findElement(By.cssSelector("span[ng-show='message']"));
        Assert.assertTrue(successMessage.isDisplayed(), "Withdrawal was not successful.");
    }
}

