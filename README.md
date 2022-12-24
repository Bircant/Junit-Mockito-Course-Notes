# Junit-Mockito-Course-Notes
Udemy - Learn Java Unit Testing with Junit &amp; Mockito in 30 Steps Course Notes

Eclipse shortcuts - refactoring in Eclipse:
- Rename Alt+Shift+R.
- Rename the package, classes and interfaces from the Project Explorer view by pressing F2
- Extract method => Alt+Shift+M
- Refactor > Extract Local Variable
-Refactor > Extract Constant
-Inline => Alt+Shift+I
-Refactor > Change Method Signature = Alt+Shift+C.

Notes:
- Test methods should be public and void
- Testing only one unit test => highlight the name of the method and select run as junit test
- assertEquals, assertFalse, assertTrue, assertArrayEquals( Comparing arrays, values), 
- Each test method takes a String as a parameter for clarification in case the test fails
- If you right-click on the failed method, the line with the error will be returned.
- @Before ans @After (For all test methods)
    @Before
    public void before() {
- @BeforeClass and @AfterClass (For class, only once) (should be static)
- Exceptions => @Test(expected = NullPointerException.class)
- Testing performance => @Test(timeout = 1000) (miliseconds)
- Parameterized Tests: 
```
  @RunWith(Parameterized.class)
  public class TestParameterizedTest {
      private String input;
      private String expectedOutput;
      
      public TestParameterizedTest( String input, String expectedOutput) {   // constructor
            this.input = input;
            this.expectedOutput = expectedOutput;
      }
      @Parameters 
      public static Collection<String[][]> testConditions() {
          String expectedOutputs[][] = {{ "AACD", "CD"},  // input, output
                                        { "ACD", "CD"}};
          return Arrays.asLists(expectedOutputs);

      }
      
      @Test
      public void testTruncateAInFirst2Positions() {
         assertEquals(expectedOutput, helper.truncateAInFirst2Positions(input));
      }
  }
 ``` 
  - JUnit Test Suite - choose which classes should include in suite for organization.
      
