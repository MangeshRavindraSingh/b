package part1Selenium;
import java.io.FileInputStream;
import java.io.IOException;
import java.util.List;
import java.util.concurrent.TimeUnit;

import org.apache.commons.io.FileUtils;
import org.apache.poi.xssf.usermodel.XSSFRow;
import org.apache.poi.xssf.usermodel.XSSFSheet;
import org.apache.poi.xssf.usermodel.XSSFWorkbook;
import org.openqa.selenium.By;
import org.openqa.selenium.OutputType;
import org.openqa.selenium.TakesScreenshot;
import org.openqa.selenium.WebDriver;
import org.openqa.selenium.WebElement;
import org.openqa.selenium.chrome.ChromeDriver;
import org.openqa.selenium.support.ui.Select;
import org.testng.annotations.AfterClass;
import org.testng.annotations.AfterMethod;
import org.testng.annotations.BeforeMethod;
import org.testng.annotations.Test;

import com.gargoylesoftware.htmlunit.javascript.host.file.File;

public class Sel_1087988 {
	WebDriver driver;
	XSSFWorkbook workbook;
	
	
	@BeforeClass
	public void setup() throws IOException {
		System.setProperty("webdriver.chrome.driver", "C:\\IVS Files\\Selenium\\Drivers\\chromedriver v2.43\\chromedriver.exe");
		driver = new ChromeDriver();
		driver.get("http://10.82.181.24:81/Automation/OnlineCourse/index.html");
		driver.manage().timeouts().implicitlyWait(13, TimeUnit.SECONDS);
		driver.manage().window().maximize();
		
		FileInputStream fis = new FileInputStream("C:\\Users\\URID_\\Desktop\\d63486ty\\Artifacts\\Data.xlsx");
		XSSFWorkbook wkbk = new XSSFWorkbook(fis);
	}
	
	
	@AfterClass
	public void tearDown() throws IOException {
		FileInputStream fos = new FileInputStream("C:\\Users\\URID_\\Desktop\\d63486ty\\Artifacts\\Data.xlsx");
				workbook.write(fos);
				workbook.close();
				
				driver.close();
				driver.quit();
				
		
	}
	
	@Test
	public void testMethod() throws InterruptedException{
		driver.findElement(By.linkText("BROWSE CATALOG")).click();
		Thread.sleep(1000);
		
		List<WebElement> links = driver.findElements(By.tagName("a"));
		System.out.println("The Number of Hyper Links are :"+links.size());
		WebElement table=driver.findElement(By.id("catalogTable"));
		List<WebElement> rows =table.findElements(By.tagName("tr"));
		
		for (int x=1;x<rows.size();x++) {
			List<WebElement> cols= rows.get(x).findElements(By.tagName("td"));
			XSSFSheet sheet = workbook.getSheet("Data");
			
			int lastrownum= sheet.getLastRowNum();
			
			for(int y=1;y<lastrownum;y++) {
				XSSFRow exrow =sheet.getRow(y);
			if(exrow.getCell(0).getStringCellValue().equalsIgnoreCase(cols.get(0).getText())) {
				if (exrow.getCell(1).getStringCellValue().equalsIgnoreCase(cols.get(4).getText())) {
					
					int cost=(int)exrow.getCell(2).getNumericCellValue();
					if (String.valueOf(cost).equalsIgnoreCase(cols.get(5).getText())) {
						exrow.createCell(3).setCellValue("ok");
					}
				}
			}
			}
		}
		
		driver.findElement(By.xpath("//*[id=\"btn8\"]")).click();
		Thread.sleep(2000);
		
		driver.findElement(By.id("name")).sendKeys("John");
		driver.findElement(By.id("email")).sendKeys("john@gmail.com");
		driver.findElement(By.id("stuRadio")).click();
		Thread.sleep(2000);
		Select s1= new Select (driver.findElement(By.id("pSelect1")));
		s1.selectByVisibleText("HRTU");
		Thread.sleep(2000);
		
		Select s2= new Select(driver.findElement(By.id("pSelect2")));
		s2.selectByVisibleText("2012");
		Thread.sleep(2000);
		
		driver.findElement(By.xpath("//*[@id=\"eRadio\"]")).click();
		Thread.sleep(2000);
		driver.findElement(By.id("payNow")).click();
		Thraed.sleep(2000);
		
		
		File scrsht= ((TakesScreenshot)driver).getScreenshotAs(OutputType.FILE);
		FileUtils.copyFile(scrsht,new File("C:\\Users\\aman.shah01\\Desktop\\Reference.png") );
		}
		
	}
	
	
