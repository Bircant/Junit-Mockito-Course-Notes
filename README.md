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

# Notes:
# Introduction and Unit Testing with JUnit
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
      
# Getting Ready for Mockito and Need For Mockito:
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
      
# Mockito Basics:
- Mock List:
 ``` 
 public class ListTest {

	@Test
	public void letsMockListSize() {
		List list = mock(List.class);
		Mockito.when(list.size()).thenReturn(10);
		assertEquals(10, list.size());
	}

	@Test
	public void letsMockListSizeWithMultipleReturnValues() {
		List list = mock(List.class);
		Mockito.when(list.size()).thenReturn(10).thenReturn(20);
		assertEquals(10, list.size()); // First Call
		assertEquals(20, list.size()); // Second Call
	}

	@Test
	public void letsMockListGet() {
		List<String> list = mock(List.class);
		Mockito.when(list.get(0)).thenReturn("in28Minutes");
		assertEquals("in28Minutes", list.get(0));
		assertNull(list.get(1));
	}

	@Test
	public void letsMockListGetWithAny() {
		List<String> list = mock(List.class);
		Mockito.when(list.get(Mockito.anyInt())).thenReturn("in28Minutes");	// ARGUMENT MATCHER
		// If you are using argument matchers, all arguments
		// have to be provided by matchers.
		assertEquals("in28Minutes", list.get(0));
		assertEquals("in28Minutes", list.get(1));
	}
	
	@Test(excepted = RuntimeException.class)
	public void letsMockList_throwAnException() {
		List<String> list = mock(List.class);
		Mockito.when(list.get(Mockito.anyInt())).thenThrow(new RuntimeException("Something");	// Throw An Exception
		list.get(0);
	}

}
 ``` 
- BDD (Behavior Driven Development) (Given(setup) - When(actual method call) - Then(Asserts)) 
 Make more readable and organized with bdd.
 Example 1:
 ```
	@Test
	public void usingMockito_UsingBDD() {
		TodoService todoService = mock(TodoService.class);
		TodoBusinessImpl todoBusinessImpl = new TodoBusinessImpl(todoService);
		List<String> allTodos = Arrays.asList("Learn Spring MVC",
				"Learn Spring", "Learn to Dance");

		//given
		given(todoService.retrieveTodos("Ranga")).willReturn(allTodos);  // instead of when  // GIVEN WILL RETURN 

		//when
		List<String> todos = todoBusinessImpl
				.retrieveTodosRelatedToSpring("Ranga");

		//then
		assertThat(todos.size(), is(2));	// Syntax change
	}
 ```
 Example 2:
  ```
 	@Test
	public void bddAliases_UsingGivenWillReturn() {
		List<String> list = mock(List.class);

		//given
		given(list.get(Mockito.anyInt())).willReturn("in28Minutes");

		//then
		assertThat("in28Minutes", is(list.get(0)));
		assertThat("in28Minutes", is(list.get(0)));
	}
   ```
 - Verify how many times a method is called:
  ```
 	@Test
	public void letsTestDeleteNow() {

		TodoService todoService = mock(TodoService.class);

		List<String> allTodos = Arrays.asList("Learn Spring MVC",
				"Learn Spring", "Learn to Dance");

		when(todoService.retrieveTodos("Ranga")).thenReturn(allTodos);

		TodoBusinessImpl todoBusinessImpl = new TodoBusinessImpl(todoService);

		todoBusinessImpl.deleteTodosNotRelatedToSpring("Ranga");

		verify(todoService).deleteTodo("Learn to Dance");

		verify(todoService, Mockito.never()).deleteTodo("Learn Spring MVC");
		then(todoService).should(never()).deleteTodo("Learn Spring MVC");    // More readable

		verify(todoService, Mockito.never()).deleteTodo("Learn Spring");

		verify(todoService, Mockito.times(1)).deleteTodo("Learn to Dance");
		// atLeastOnce, atLeast

	}
   ```
  - How to capture an argument which is passed to a mock?
  Calling once:
  ```
  	@Test
	public void captureArgument() {
		ArgumentCaptor<String> argumentCaptor = ArgumentCaptor
				.forClass(String.class);

		TodoService todoService = mock(TodoService.class);

		List<String> allTodos = Arrays.asList("Learn Spring MVC",
				"Learn Spring", "Learn to Dance");
		Mockito.when(todoService.retrieveTodos("Ranga")).thenReturn(allTodos);

		TodoBusinessImpl todoBusinessImpl = new TodoBusinessImpl(todoService);
		todoBusinessImpl.deleteTodosNotRelatedToSpring("Ranga");
		Mockito.verify(todoService).deleteTodo(argumentCaptor.capture());

		assertEquals("Learn to Dance", argumentCaptor.getValue());
	}
```
Calling multiple times:
  ```
  	@Test
	public void captureArgument() {
		ArgumentCaptor<String> argumentCaptor = ArgumentCaptor
				.forClass(String.class);

		TodoService todoService = mock(TodoService.class);

		List<String> allTodos = Arrays.asList("Learn",
				"Learn Spring", "Learn to Dance");
		Mockito.when(todoService.retrieveTodos("Ranga")).thenReturn(allTodos);

		TodoBusinessImpl todoBusinessImpl = new TodoBusinessImpl(todoService);
		todoBusinessImpl.deleteTodosNotRelatedToSpring("Ranga");
		
		// then(todoService).should().deleteTodo(argumentCaptor.capture()); excepted to called once
		then(todoService).should(times(2)).deleteTodo(argumentCaptor.capture());

		assertThat(argumentCaptor.getAllValues().size(), is(2));
	}
```
