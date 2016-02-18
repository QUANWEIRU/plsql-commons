# PLSQL Commons Log package #

Log Framework, for logging application behavior, a 100% PLSQL Tool


# plsql\_log (pLog) #

_What is it all about?_
  * Application behavior can be easily monitored through logging.
  * Inserting log statements into your code is a low-tech method for debugging it. It may also be the only way because debuggers are not always available or applicable.
  * Logging behavior can be controlled by switching on and off, setting levels, layouts, labels and outputs.
  * Out of the box layout can be customized the way you like, printf style.
  * The target of the log output can be a DBMS\_OUTPUT, table, DBMS\_PIPE, HTP.
  * The package is being constantly improved thanks to input from users and code contributed by authors in the community.

_How to?_

See for more details TST\_PLSQL\_LOG unit test suite for PLSQL\_LOG.

**Example:**


Run you code in logging mode
```
begin
    pLog.switchOn;
    pLog.setDFormat ( pLog.getDFormat ); 
    pLog.pushStack ('first');
    pLog.pushStack ('second');
    pLog.pushStack ('thirth');
    pTest.assert ( pLog.getDFormat = 'YYYYMMDD-HH24:MI:SS.FF2' , 'Date Format is not set correctly'); 
    pLog.debug (to_char ( systimestamp , pLog.getDFormat ));
    pLog.popStack;
    pTest.assert ( pLog.getStack = 'first.second', pUtil.printf('Log stack is not correct, "{1}"', StringList(pLog.getStack)));
    pLog.emptyStack;
    pLog.switchOff;
end; 
```
or
```
declare
     identifier type.string;
begin 
    pLog.switchOn;
    pLog.setOutput ( pLog.OUTPUT_PIPE );
    identifier := pLog.setRandomIdentifier;
    pPipe.setTimeOut (1);
    pLog.setLayout ( pLog.FORMAT_TEXT );
    pLog.debug ('teststring');
    pTest.assert (trim(pPipe.recieve (identifier)) = 'teststring', 'Debug data should be recieved');
    pLog.switchOff;
end; 
```