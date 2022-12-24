# Junit-Mockito-Course-Notes
Udemy - Learn Java Unit Testing with Junit &amp; Mockito in 30 Steps Course Notes

+ Eclipse shortcuts - refactoring in Eclipse:
- Rename Alt+Shift+R.
- Rename the package, classes and interfaces from the Project Explorer view by pressing F2
- Extract method => Alt+Shift+M
- Refactor > Extract Local Variable
- Refactor > Extract Constant
- Inline => Alt+Shift+I
- Refactor > Change Method Signature = Alt+Shift+C.
- Ctrl + 1 on errors

+ Notes:
+ Introduction and Unit Testing with JUnit
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
      
+ Getting Ready for Mockito and Need For Mockito:
- SUT (System Under Test), Dependency
- Stub - class that returns dummy data
- Stub Example - Test cases couple tightly due to the hard-coding of data.:

TodoService.java
```    
    // External Service - Lets say this comes from WunderList
    public interface TodoService {
	    public List<String> retrieveTodos(String user);
    }
```   
TodoServiceStub.java
```  
    package com.in28minutes.data.stub;
    import java.util.Arrays;
    import java.util.List;
    import com.in28minutes.data.api.TodoService;
    public class TodoServiceStub implements TodoService {
	    public List<String> retrieveTodos(String user) {
		    return Arrays.asList("Learn Spring MVC", "Learn Spring",
				    "Learn to Dance");
	    }
    }
```   
TodoBusinessImplStubTest.java
```    
 package com.in28minutes.business;

import static org.junit.Assert.assertEquals;

import java.util.List;

import org.junit.Test;

import com.in28minutes.data.api.TodoService;
import com.in28minutes.data.stub.TodoServiceStub;

public class TodoBusinessImplStubTest {

	@Test
	public void usingAStub() {
		TodoService todoService = new TodoServiceStub();
		TodoBusinessImpl todoBusinessImpl = new TodoBusinessImpl(todoService);
		List<String> todos = todoBusinessImpl
				.retrieveTodosRelatedToSpring("Ranga");
		assertEquals(2, todos.size());
	}
}   
 ```  
- Disadvantages of Stubbing:
    - Real dummy implementation of interface
    - When a new method is added to the service, it should also be added to the Stub version.
    - Dummy condition, service definition
- Mockito:
    - Window > Preferences > Java > Editor > Content Assist > Favorites
        org.junit.Assert
        org.mockito.BDDMockito
        org.mockito.Mockito
        org.hamcrest.Matchers
        org.hamcrest.CoreMatchers
     -  Example:
        ``` 
        TodoService todoService = mock(TodoService.class);
		List<String> allTodos = Arrays.asList("Learn Spring MVC",
				"Learn Spring", "Learn to Dance");
		when(todoService.retrieveTodos("Ranga")).thenReturn(allTodos);
		TodoBusinessImpl todoBusinessImpl = new TodoBusinessImpl(todoService);
		List<String> todos = todoBusinessImpl
				.retrieveTodosRelatedToSpring("Ranga");
		assertEquals(2, todos.size());
        ``` 
      
+ Mockito Basics:
