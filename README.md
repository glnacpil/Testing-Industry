
using System;
using System.Threading;
using OpenQA.Selenium;
using OpenQA.Selenium.Chrome;
using OpenQA.Selenium.Support.UI;

namespace TurnUpHomework
{
    class Program
    {
        
        static void Main(string[] args)
        {
            IWebDriver driver = new ChromeDriver();
            driver.Navigate().GoToUrl("http://horse-dev.azurewebsites.net/Account/Login?ReturnUrl=%2f");

            // homework 1
            // to maximize the window
            driver.Manage().Window.Maximize();


            IWebElement UserName = driver.FindElement(By.Id("UserName"));
            IWebElement PassWord = driver.FindElement(By.Id("Password"));
            IWebElement ClickBtn = driver.FindElement(By.XPath("//input[@class='btn btn-default']"));

            UserName.SendKeys("ray");
            PassWord.SendKeys("123123");
            ClickBtn.Click();

            //home 2 - Validate that the home page title contains - "Dashboard - Dispatching System"
            if ("Dashboard - Dispatching System" == driver.Title)
                Console.WriteLine("Title assertion passed");
            else
            {
                Console.WriteLine("Title assertion failed");
                return;
            }

            //StringAssert.Contains(""Dashboard - Dispatching System",driver.Title")

            IWebElement LnkAdministration = driver.FindElement(By.XPath("//a[contains(.,'Administration ')]"));
            IWebElement LnkTimenMaterials = driver.FindElement(By.XPath("//a[@href='/TimeMaterial']"));

            LnkAdministration.Click();
            LnkTimenMaterials.Click();

            IWebElement CreateNew = driver.FindElement(By.XPath("//a[@class='btn btn-primary']"));
            CreateNew.Click();

            string codefromuser = "Code123";
            driver.FindElement(By.XPath("//input[@id='Code']")).SendKeys(codefromuser);
            driver.FindElement(By.XPath("//input[@id='Description']")).SendKeys("desc");


            // home work 3 
            // select Time from dropdown list

            //Click Time and Materials dropdown list

            driver.FindElement(By.XPath("//span[@class='k-icon k-i-arrow-s']")).Click();
            Thread.Sleep(2000);

            //select Time (for  (intitialization; termination; incrementation) { }
            for (int i = 1; i <= 2; i++)
            {
                                                                            //Firepath /li[2]
                IWebElement timeMaterialText = driver.FindElement(By.XPath(".//*[@id='TypeCode_listbox']/li["+i+"]"));
                if (timeMaterialText.Text == "Time")
                {
                    timeMaterialText.Click();
                    break;
                 }
                Thread.Sleep(1000);
            }

            driver.FindElement(By.XPath("//input[@id='SaveButton']")).Click();


            //homework 5 
            // Impliment Explicit wait for pages
            Thread.Sleep(3000);

            // homework 4
            //Validate that the item created is on the table

            try
            {
                while (true)
                {
                    for (int j = 1; j <= 2; j++)
                    {
                        IWebElement codeTextfromUI = driver.FindElement(By.XPath(".//*[@id='tmsGrid']/div[3]/table/tbody/tr[" + j + "]/td[1]"));

                        Console.WriteLine(codeTextfromUI.Text);
                        if (codeTextfromUI.Text == codefromuser)
                        {
                            Console.WriteLine("Test Success");
                            return;
                        }
                        driver.FindElement(By.XPath("//span[contains(.,'Go to the next page')]")).Click();
                    }
                }
             }
            catch (Exception e)
            {
                Console.WriteLine("Test failed, element not found" + e.Message);
            }

            driver.Quit();


        }
    }
}
            



