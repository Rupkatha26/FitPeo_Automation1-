# FitPeo_Automation1-
package fitpeo;

import org.openqa.selenium.By;
import org.openqa.selenium.JavascriptExecutor;
import org.openqa.selenium.WebDriver;
import org.openqa.selenium.WebElement;
import org.openqa.selenium.chrome.ChromeDriver;
import org.openqa.selenium.interactions.Actions;

import java.time.Duration;
//import java.util.concurrent.TimeUnit;

public class FitPeoAutomation {

	public static void main(String[] args) {

		WebDriver driver = new ChromeDriver();

		try {
			// Maximize browser and set implicit wait
			driver.manage().window().maximize();
			driver.manage().timeouts().implicitlyWait(Duration.ofSeconds(15));
//	            driver.manage().timeouts().implicitlyWait(10, TimeUnit.SECONDS);

			// Step 1: Navigate to FitPeo  
			driver.get("https://www.fitpeo.com");

			// Step 2: Navigate to Revenue Calculator Page
			WebElement calculatorLink = driver.findElement(By.linkText("Revenue Calculator"));
			calculatorLink.click();

			JavascriptExecutor js = (JavascriptExecutor) driver;
			WebElement sliderSection = driver.findElement(By.id("slider-section")); // Replace with the correct ID
			js.executeScript("arguments[0].scrollIntoView();", sliderSection);

			WebElement slider = driver.findElement(By.id("slider")); // Replace with correct slider ID
			Actions action = new Actions(driver);
			action.dragAndDropBy(slider, 50, 0).perform(); // Adjust offset based on actual slider range
			Thread.sleep(1000); // Wait to observe action

			// Validate slider value
			WebElement sliderValue = driver.findElement(By.id("slider-value")); // Replace with correct ID
			if (!sliderValue.getAttribute("value").equals("820")) {
				throw new Exception("Slider value not updated to 820.");
			}

			// Step 5: Update Text Field to 560
			sliderValue.clear();
			sliderValue.sendKeys("560");
			Thread.sleep(1000);

			// Validate slider updated to 560
			String updatedSliderValue = sliderValue.getAttribute("value");
			if (!updatedSliderValue.equals("560")) {
				throw new Exception("Slider not updated to 560 after text field change.");
			}

			String[] cptCodes = { "CPT-99091", "CPT-99453", "CPT-99454", "CPT-99474" };
			for (String code : cptCodes) {
				WebElement checkbox = driver.findElement(By.id(code)); // Replace with correct IDs
				if (!checkbox.isSelected()) {
					checkbox.click();
				}
			}

			WebElement totalReimbursement = driver.findElement(By.id("total-reimbursement")); // Adjust ID
			if (!totalReimbursement.getText().equals("$110700")) {
				throw new Exception("Total reimbursement value mismatch.");
			}

			System.out.println("Test Passed Successfully!");

		} catch (Exception e) {
			System.out.println("An error occurred: " + e.getMessage());
		} finally {
			// Close the browser
			driver.quit();
		}
	}
}
