# cs522 . 


Following are the most commonly used annotations and their usage in a basic unit test written in JUnit 4:

@Test - Marks the method as a test method.
@Before and @After sandwiches each test method in the class.
@BeforeClass and @AfterClass sandwiches all of the test methods in a JUnit test class.
So when you run the JUnit test class below, the execution order is:
Method annotated with @BeforeClass
Method annotated with @Before
First method annotated with @Test i.e. test1().
Method annotated with @After
Method annotated with @Before
Second method annotated with @Test i.e. test2().
Method annotated with @After
Method annotated with @AfterClass


public class SampleTest {

        @BeforeClass
        public static void setUpBeforeClass() throws Exception {
            
            
            //Method annotated with `@BeforeClass` will execute once before any of the test methods in this class.

            //This method could be used to set up any test fixtures that are computationally expensive and shared by several test methods. e.g. establishing database connections 

            //Sometimes several tests need to share computationally expensive setup (like logging into a database). While this can compromise the independence of tests, sometimes it is a necessary optimization. From http://junit.sourceforge.net/javadoc/org/junit/BeforeClass.html
           
        }

        @AfterClass
        public static void tearDownAfterClass() throws Exception {
            
            //Method annotated with `@AfterClass` will execute once after all of the test methods are executed in this class.

            //If you allocate expensive external resources in a BeforeClass method you need to release them after all the tests in the class have run. Annotating a public static void method with @AfterClass causes that method to be run after all the tests in the class have been run. All @AfterClass methods are guaranteed to run even if a BeforeClass method throws an exception. From http://junit.sourceforge.net/javadoc/org/junit/AfterClass.html
        }

        @Before
        public void setUp() throws Exception {
             //Method annotated with `@Before` will execute before each test method in this class is executed.

             //If you find that several tests need similar objects created before they can run this method could be used to do set up those objects (aka test-fixtures).
        }
        
        @After
        public void tearDown() throws Exception {
             
             //Method annotated with `@After` will execute after each test method in this class is executed.

             //If you allocate external resources in a Before method you must release them in this method.
        }

        @Test
        public void test1() {
           
           //A public void method annotated with @Test will be executed as a test case.
        }

        @Test
        public void test2() {
   
            //Another test cases
        }

    }
    
    
    
    
    
    
    
    
    
    Assertion:
    
    
    
    
    
When it comes to assertions, there is the set of old JUnit assertions like:
org.junit.Assert.assertArrayEquals
org.junit.Assert.assertEquals
org.junit.Assert.assertFalse
org.junit.Assert.assertNotNull
org.junit.Assert.assertNotSame
org.junit.Assert.assertNull
org.junit.Assert.assertSame
org.junit.Assert.assertTrue


And the org.junit.Assert.assertThat method (available in JUnit4) which uses matchers and is better than old style assertions because it provides:

1.Better readability
   a.assertThat(actual, is(equalTo(expected))); is better than assertEquals(expected, actual);
   b.assertThat(actual, is(not(equalTo(expected)))); is better than assertFalse(expected.equals(actual));
2.Better failiure messages
   a.java.lang.AssertionError: Expected: is "hello" but: was "hello world" is better than

     org.junit.ComparisonFailure: expected:<hello[]> but was:<hello[ world]>

3.Flexbility
    a.Multiple conditions could be asserted using matchers like anyOf or allOf.

eg: assertThat("hello world", anyOf(is("hello world"), containsString("hello"))); In this case, the test will pass if either the actual string is “hello world” or if it contains the word “hello”.


Example useage of a few of the above matchers

@Test
public void testAssetThatExamples() {

    // 'theString' should contain 'S' and 'r'
    assertThat("theString", both(containsString("S")).and(containsString("r")));

    List<String> items = Arrays.asList("John", "James", "Julia", "Jim");

    // items list should have James and Jim
    assertThat(items, hasItems("James", "Jim"));

    // Every item in the list should have the character 'J'
    assertThat(items, everyItem(containsString("J")));

    // check all of the matchers
    assertThat("Once", allOf(equalTo("Once"), startsWith("O")));

    // negation of all of the matchers
    assertThat("Once", not(allOf(equalTo("test"), containsString("test"))));
}
