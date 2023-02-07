# JUnit F/W

## Introduction

JUnit is a f/w used for writing unit tests in java. The two current popular versions are JUnit 4 (JUnit Vintage) and JUnit 5 (JUnit Jupiter).

## JUnit 4 Annotations

|Annotataion| Usage |
|:----------:|:----------:|
|```@Test```|Used to mark a method as a unit test|
|```@BeforeClass```| Gets executed once before the test class is run|
|```@AfterClass```| Gets executed once after the test class is run|
|```@Before```| Gets executed before each testcase method is executed in class|
|```@After```| Gets executed after each testcase method is executed in class|
|```@Ignore```| Tells Junit to ignore the test case i.e. the test case wont be run|
|```@Test(expected =Exception.class)``` | Fails if the test case does not throw the named exception|
|```@Test(timeout = 10)``` | Fails if the test case is still running after the timeout|