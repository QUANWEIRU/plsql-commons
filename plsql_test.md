# PLSQL Commons Unit Testing Framework #

Strong in simplicity

# PLSQL\_TEST (pTest) #

_What is it all about?_

pTest is a Unit Testing Framework for writing and performing automatized unit tests for applications written in PLSQL. It follows the tradition of the "xUnit" framework that is available for almost all
programming languages. Automatized tests are one of the cornerstones of a methodology called extreme programming, which tries to solve many of the traditional problems with software development. You can read more about it at http://www.xprogramming.org or http://www.extremeprogramming.org/.

Writing the tests before you write the stored procedure makes you think carefully about its interface, and may improve the design. Each unit test can be seen as a design element specifying package and observable behaviour. Automatic tests will give you the courage to refactoring your code without being afraid of breaking anything. The tests also serve the purpose of documentation. They explain in detail what is expected to happen in the stored procedures you write.


_How to?_

Your test suite is a package, which can contain a setup procedure so you can first prepare the 'world' to make an isolated environment for testing. In the end, whether succeed or fail we should tear down our 'world' to not disturb other tests or code. t`_` distinguish unit tests from other procedures in your package.

The Framework checks the contract for implementing the Unit Test interface in any package you desire: setUp, tearDown and prefix t`_` for any package procedure.

**Example:**

_Running the unit test_
```
begin 
   pLog.switchOn; 
   pTest.testSuite( 'TST_PLSQL_LOG' ); 
end; 
```

_Creating a unit test, within a package_
```
procedure t_StringLists
is 
  vt1 StringList:= new StringList('A','B');
  vt2 StringList:= new StringList('A','B','');
  vt3 StringList:= new StringList('A','B','',null);
begin
  pTest.assert(vt1.count = 2,'That is not enough');
  pTest.assert(vt2.count = 3,'That is not enough');
  pTest.assert(vt3.count = 4,'That is not enough');
end;  
```