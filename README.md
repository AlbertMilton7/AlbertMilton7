packagepractice;

importjava.util.List;
importorg.testng.Assert;
importorg.testng.annotations.Test;
importio.restassured.*;
importio.restassured.builder.RequestSpecBuilder;
importio.restassured.response.Response;
importio.restassured.specification.RequestSpecification;
importorg.openqa.selenium.By;
importorg.openqa.selenium.WebDriver;
importorg.openqa.selenium.chrome.*;

 

publicclassTest1 {

 

  String url= "https://pagead2.googlesyndication.com/getconfig/sodar?sv=200&tid=gpt&tv=2021120601&st=env";

 
//This is the First Test.

 @Test

 privatevoidTest() {

  String url= "http://localhost:3000/devices";

  String uiURL= "http://localhost:3000";

  Response response= null;

  RequestSpecBuilder builder= newRequestSpecBuilder();

 builder.setBaseUri(url);

 builder.setContentType("application/json; charset=UTF-8");

  RequestSpecification requestSpec= builder.build(); 

 response= RestAssured.given(requestSpec).get();

  List<String> ids= response.jsonPath().getList("device.id");

 //From here UI part

  System.setProperty("webdriver.chrome.driver","C:\\chromeodriver.exe");

  WebDriver driver= newChromeDriver();

 driver.get(uiURL);

 driver.manage().window().maximize();

 for(inti=0;i<ids.size();i++) {

  Assert.assertTrue(driver.findElement(By.className(ids.get(i))).isDisplayed());

  Assert.assertTrue(driver.findElement(By.id("edit")).isDisplayed());

  Assert.assertTrue(driver.findElement(By.id("delete")).isDisplayed());

  }

 }

 
 
 
//This is the second test.
 @Test

 privatevoidTest2() {

  String uiURL= "http://localhost:3000";

  System.setProperty("webdriver.chrome.driver","C:\\chromeodriver.exe");

  WebDriver driver= newChromeDriver();

 driver.get(uiURL);

 driver.manage().window().maximize();

 driver.findElement(By.id("create")).click();

 driver.findElement(By.id("name")).sendKeys("name1");

 driver.findElement(By.id("capacity")).sendKeys("2345");

 driver.findElement(By.id("type")).sendKeys("R22");

 driver.findElement(By.id("submitt")).click();

  Assert.assertTrue(driver.findElement(By.id("successnotification")).isDisplayed());

  Assert.assertEquals(driver.findElement(By.id("name")).getText(), "name1");

  Assert.assertEquals(driver.findElement(By.id("type")).getText(), "2345");

  Assert.assertEquals(driver.findElement(By.id("capacity")).getText(), "R22");

 

  }

 
//This is the third test.

 @Test

 privatevoidTest3() {

  String url= "http://localhost:3000/devices/e7ocoQ2n3";

  Response response= null;

  String apiBody= "{\"system_name\":\"Renamed Device\",\"type\":\"WINDOWS_SERVER\",\"hdd_capacity\":\"500\"}";

  RequestSpecBuilder builder= newRequestSpecBuilder();

 builder.setBaseUri(url);

 builder.setBody(apiBody);

 builder.setContentType("application/json; charset=UTF-8");

  RequestSpecification requestSpec= builder.build(); 

 response= RestAssured.given(requestSpec).put();

  String uiURL= "http://localhost:3000";

  System.setProperty("webdriver.chrome.driver","C:\\chromeodriver.exe");

  WebDriver driver= newChromeDriver();

 driver.get(uiURL);

 driver.manage().window().maximize();

  Assert.assertEquals(driver.findElement(By.id("name")).getText(),"Renamed Device");

  }

 
//This is the fourth test.

 @Test

 privatevoidTest4() {

  String url= "http://localhost:3000/devices";

  String uiURL= "http://localhost:3000";

  Response response= null;

  RequestSpecBuilder builder= newRequestSpecBuilder();

 builder.setBaseUri(url);

 builder.setContentType("application/json; charset=UTF-8");

  RequestSpecification requestSpec= builder.build(); 

 response= RestAssured.given(requestSpec).get();

  List<String> ids= response.jsonPath().getList("device.id");

  String lastDevice= ids.get(ids.size() - 1);

 url= "http://localhost:3000/"+lastDevice;

 builder.setBaseUri(url);

 builder.setContentType("application/json; charset=UTF-8");

 requestSpec= builder.build();

 response= RestAssured.given(requestSpec).delete();

 

 //From here UI part

  System.setProperty("webdriver.chrome.driver","C:\\chromeodriver.exe");

  WebDriver driver= newChromeDriver();

 driver.get(uiURL);

 driver.manage().window().maximize();

  Assert.assertFalse(driver.findElement(By.id(lastDevice)).isDisplayed());

  }

 

 

}
