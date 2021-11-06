package test;

import java.math.BigDecimal;
import java.sql.Connection;
import java.sql.DriverManager;
import java.sql.ResultSet;
import java.sql.SQLException;
import java.sql.Statement;
import java.util.List;
import java.util.concurrent.TimeUnit;
import org.openqa.selenium.By;
import org.openqa.selenium.WebDriver;
import org.openqa.selenium.WebElement;
import org.openqa.selenium.chrome.ChromeDriver;
import org.openqa.selenium.support.ui.Select;


public class AmazonSearch {

	public static void main(String[] args) {
		// TODO Auto-generated method stub
		String category = null;
		String term = null;
		
		System.setProperty("webdriver.chrome.driver", "chromedriver");
		
		WebDriver driver = new ChromeDriver();
		
		driver.get("https://www.amazon.in/");
		
		driver.manage().window().maximize();
		
		driver.manage().timeouts().implicitlyWait(5000, TimeUnit.MILLISECONDS);
		
		
		try {
			Class.forName("com.mysql.cj.jdbc.Driver");
			Connection con=DriverManager.getConnection("jdbc:mysql://localhost:3306/ecommerce","root","root");
			//Connection con=DriverManager.getConnection("jdbc:mysql://3.95.238.133:3306/ecommerce","root","root");
			Statement stmn=con.createStatement();
			ResultSet rs=stmn.executeQuery("select * from AMAZON");
			while(rs.next()){
				//System.out.println(rs.getString(2)+ " "+rs.getString(3));
				category=rs.getString(2);
				term=rs.getString(3);
			}
			
			
		}
		catch(ClassNotFoundException e)
		{
			System.out.println("Class Not Found");
		}
		catch (SQLException e) {
			System.out.println("SQL Exception");
		}
		WebElement searchDD = driver.findElement(By.xpath("//*[@id='searchDropdownBox']"));
		Select optionToSelect = new Select(searchDD);
		optionToSelect.selectByVisibleText(category);
		WebElement searched= driver.findElement(By.xpath("//*[@id='twotabsearchtextbox']"));
		searched.sendKeys(term);
		
		WebElement searchButton=driver.findElement(By.xpath("//*[@id='nav-search-submit-button']"));
		searchButton.click();
		
		List<WebElement> results = driver.findElements(By.xpath("//h2[@class='a-size-mini a-spacing-none a-color-base s-line-clamp-2']"));
		System.out.println("Total Results are: " + results.size());
		
		for(WebElement e : results) {
			  System.out.println(e.getText());
		}
		driver.close();
	}

}
