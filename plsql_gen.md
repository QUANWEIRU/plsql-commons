# Template Engine PL/SQL style #

_What is it all about?_

Template engine are designed to separate program logic and presentation into two independent parts. This makes the development of both logic and presentation easier, improves flexibility and eases modification and maintenance.

_How to?_

Template engine depence on other packages, for example you need to read a template file from operating system and therefore it requires PLSQL\_FILE. See emp\_adr.sql and emp\_adr.tpl example including a template file to generating for example table logging triggers.

```
declare 
     lTemplate StringList := StringList ();
     lContext StringList := StringList ();
     lOutput StringList := StringList (); 
     tplName type.string := 'HelloWorld';
begin
     plsql_log.switchOn;
     lTemplate := plsql_gen.getTemplate (tplName);
     plsql_log.debug(pUtil.join (lTemplate)); 
     lContext := plsql_gen.addContext (lContext, 'value2', 'Dimitri Lambrou');   
     lContext := plsql_gen.addContext (lContext, 'value1', 'World');  
     lOutput  := plsql_gen.merge (lContext, lTemplate); 
     plsql_log.debug(pUtil.join (lOutput)); 
     plsql_file.putLines ('output.tpl', 'TEMPLATE', lOutput, plsql_file.WE8ISO8859P1); 
end;  
```

input:

`Hello %value1%, this is the %value1% according to %value2%.`

output:

`Hello World, this is the World according to Dimitri Lambrou.`

In addition the latest feature is to render a template on multiple levels generating output based on a query. The column order is used in the template mapping defined by {...}.
```
   select plsql_gen.SqlTemplate ( 
     'select ''04'', deptno, dname, loc from dept'                                                                        
   ,'<input type="radio" name="f{1}" id="f{1}-{2}" value="{4}" checked="checked" /> {3}'
   ,'{1}','<div data-role="fieldcontain"><fieldset data-role="controlgroup" data-type="horizontal">
<legend>Department Location:</legend>{1}</fieldset></div>') 
 from dual;
```