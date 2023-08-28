# 基于oracle数据库存储过程的创建及调用

大纲：

1. PLSQL编程：Hello World，程序结构，变量，流程控制，游标
2. 存储过程：概念，无参存储，有参存储（输入、输出）
3. Java调用存储存储过程



# 1.PLSQL编程

## 1.1.概念和目的

什么事PL/SQL？

1. PL/SQL（Procedure Language/SQL）
2. PLSQL是Oracle对sql语言的过程化扩展（类似于Basic）
3. 指在SQL命令语言中增加了过程处理语句（如分支、循环等），使SQL语言具有过程处理能力

## 1.2.程序结构

以下是PL/SQL程序的基本结构：

1. **DECLARE（可选）：** 在此部分声明变量、常量、游标等。这是可选的，取决于是否需要在程序中使用这些标识符。
2. **BEGIN：** 这是程序的实际开始部分，包含了实际的PL/SQL代码。程序的主要逻辑将位于这一部分。
3. **EXCEPTION（可选）：** 在这部分处理异常，如果在BEGIN部分发生了错误，系统将跳转到这里执行异常处理程序。
4. **END：** 表示程序块的结束。

## 1.3.程序示例

下面是一个简单的PL/SQL程序的结构示例：

```plsql
DECLARE
    -- 声明变量和常量
    v_name VARCHAR2(50);
    v_age NUMBER := 30;
    
    -- 声明游标
    CURSOR emp_cursor IS SELECT employee_id, first_name FROM employees;
    
    -- 声明异常
    custom_exception EXCEPTION;
    PRAGMA EXCEPTION_INIT(custom_exception, -20001);
    
BEGIN
    -- 主要逻辑
    v_name := 'John';
    DBMS_OUTPUT.PUT_LINE('Hello, ' || v_name || '!');

    -- 使用游标
    FOR emp_rec IN emp_cursor LOOP
        DBMS_OUTPUT.PUT_LINE('Employee ID: ' || emp_rec.employee_id || ', Name: ' || emp_rec.first_name);
    END LOOP;

    -- 引发异常
    IF v_age < 18 THEN
        RAISE custom_exception;
    END IF;

EXCEPTION
    -- 处理异常
    WHEN custom_exception THEN
        DBMS_OUTPUT.PUT_LINE('Custom exception handled: Underage employee.');

    WHEN OTHERS THEN
        DBMS_OUTPUT.PUT_LINE('An error occurred: ' || SQLERRM);
END;
/
```

## 1.4.变量

PLSQL编程中的常见变量分两大类：

1. 普通数据类型（char，varchar2，date，number，boolean，long）
2. 特殊变量类型（引用型变狼，记录型变量）

声明变量的方式为：

```plsql
变量 变量类型（变量长度） 例如：v_name varchar2(20)
```

### 1.4.1.普通变量

变量赋值的方式有两种：

1. 直接赋值语句：   `:=`
2. 语句赋值，使用`select...into...`赋值：（语法 select 值 into 变量）

```plsql
DECLARE
    -- 声明变量
    v_employee_name VARCHAR2(100);
    v_employee_age NUMBER := 30;
    v_salary NUMBER(10, 2) := 50000.75;
BEGIN
    -- 赋值给变量
    v_employee_name := 'John Smith';

    -- 使用变量进行计算和操作
    IF v_employee_age > 25 THEN
        v_salary := v_salary * 1.1; -- 增加10%的薪水
    ELSE
        v_salary := v_salary * 1.05; -- 增加5%的薪水
    END IF;

    -- 输出变量的值
    DBMS_OUTPUT.PUT_LINE('Employee Name: ' || v_employee_name);
    DBMS_OUTPUT.PUT_LINE('Employee Age: ' || v_employee_age);
    DBMS_OUTPUT.PUT_LINE('Employee Salary: ' || v_salary);
END;
/
```

### 1.4.2.引用型变量

在PL/SQL中，`%TYPE`和`%ROWTYPE`是特殊的标识符，用于在声明变量、参数、返回值等的数据类型时，基于数据库对象的数据类型进行引用。它们可以使代码更加灵活，避免硬编码数据类型，从而增加代码的维护性和适应性。

**%TYPE：** 使用`%TYPE`可以将一个变量的数据类型设置为某个数据库表或列的数据类型。这对于在以后更改表结构时保持代码的一致性非常有用。例如：

```plsql
DECLARE
    v_employee_name employees.first_name%TYPE;
    v_employee_salary employees.salary%TYPE;
BEGIN
    -- 使用%TYPE声明的变量将会自动采用相应表列的数据类型
    -- 这里v_employee_name的数据类型将与employees表的first_name列相同
    SELECT first_name INTO v_employee_name FROM employees WHERE employee_id = 101;
    
    -- v_employee_salary的数据类型将与employees表的salary列相同
    SELECT salary INTO v_employee_salary FROM employees WHERE employee_id = 101;
    
    -- 后续对变量的操作和处理将不需要关心具体的数据类型
END;
/
```

### 1.4.3.记录型变量

**%ROWTYPE：** 使用`%ROWTYPE`可以声明一个变量，其数据类型与表的某一行的数据类型相匹配。这使得在处理整行数据时非常方便。例如：

```plsql
DECLARE
    v_employee_record employees%ROWTYPE;
BEGIN
    -- 使用%ROWTYPE声明的变量将与employees表的一行数据的结构相同
    SELECT * INTO v_employee_record FROM employees WHERE employee_id = 101;
    
    -- 可以直接访问v_employee_record的各个字段，就像访问表中的列一样
    DBMS_OUTPUT.PUT_LINE('Employee Name: ' || v_employee_record.first_name);
    DBMS_OUTPUT.PUT_LINE('Employee Salary: ' || v_employee_record.salary);
    
    -- 后续操作不需要逐个访问字段，直接操作v_employee_record即可
END;
/
```

`%TYPE`和`%ROWTYPE`可以在声明变量、参数、返回值等的时候使用，让你的代码与数据库对象的变化保持同步，并提高代码的可维护性和灵活性。

## 1.5.流程控制

### 1.5.1条件分支

- **IF-THEN：** 用于在满足条件时执行特定的代码块。

```plsql
IF condition THEN
    -- Code to execute if condition is true
END IF;
```

- **IF-THEN-ELSE：** 用于在满足条件时执行一个代码块，否则执行另一个代码块。

```plsql
IF condition THEN
    -- Code to execute if condition is true
ELSE
    -- Code to execute if condition is false
END IF;
```

- **ELSIF：** 用于处理多个条件的情况。

```plsql
IF condition1 THEN
    -- Code to execute if condition1 is true
ELSIF condition2 THEN
    -- Code to execute if condition2 is true
ELSE
    -- Code to execute if none of the conditions are true
END IF;
```

### 1.5.2.循环

- **LOOP：** 创建一个无限循环，通常需要使用`EXIT`语句来退出循环。

```plsql
LOOP
    -- Code to repeat indefinitely
    EXIT WHEN condition; -- Exit the loop when the condition is true
END LOOP;
```

- **WHILE：** 在满足条件时重复执行循环。

```plsql
WHILE condition LOOP
    -- Code to execute while condition is true
END LOOP;
```

- **FOR：** 遍历一个数值范围或游标结果集。

```plsql
FOR i IN lower_bound..upper_bound LOOP
    -- Code to execute for each iteration
END LOOP;
```

- **CURSOR FOR LOOP：** 用于遍历游标的结果集。

```plsql
FOR rec IN cursor_name LOOP
    -- Code to execute for each row in the cursor result set
END LOOP;
```

### 1.5.3.异常处理

- **EXCEPTION：** 用于捕获和处理异常。

```plsql
BEGIN
    -- Code that may raise an exception
EXCEPTION
    WHEN exception_type THEN
        -- Code to handle the specific exception
    WHEN OTHERS THEN
        -- Code to handle all other exceptions
END;
```

- **RAISE：** 用于显式引发异常。

```plsql
IF condition THEN
    RAISE exception_type;
END IF;
```



# 2.游标

## 2.1.什么是游标

用于临时存储一个查询返回的多行数据（结果集合，类似于Java的Jdbc连接返回的ResultSet集合），通过遍历游标，可以逐行访问处理该结果集的数据。

游标的使用方式：声明-->打开-->读取-->关闭

## 2.2.语法

游标的声明：

CURSOR 游标名(参数列表) is 查询语句；

- **DECLARE游标：** 在`DECLARE`部分声明游标，定义查询语句和返回的列。
- **OPEN游标：** 在`BEGIN`部分使用`OPEN`语句打开游标，将查询结果放入游标中。
- **FETCH游标：** 使用`FETCH`语句逐行获取游标中的数据，并将数据存储在变量中。
- **CLOSE游标：** 使用`CLOSE`语句关闭游标，释放资源。

## 2.3.游标的属性

在PL/SQL中，游标具有一些属性，这些属性提供了关于游标状态和结果集的信息。以下是一些常见的游标属性：

- **%ISOPEN：** 这个属性用于检查游标是否已经打开。

```plsql
IF emp_cursor%ISOPEN THEN
    -- Cursor is open
ELSE
    -- Cursor is closed
END IF;
```

- **%FOUND：** 这个属性用于检查在最近的`FETCH`操作后是否找到了匹配的行。

```plsql
IF emp_cursor%FOUND THEN
    -- At least one row was fetched
ELSE
    -- No rows were fetched
END IF;
```

- **%NOTFOUND：** 这个属性与`%FOUND`相反，用于检查在最近的`FETCH`操作后是否未找到匹配的行。

```plsql
IF emp_cursor%NOTFOUND THEN
    -- No rows were fetched
ELSE
    -- At least one row was fetched
END IF;
```

- **%ROWCOUNT：** 这个属性返回在最近的`SELECT`语句或`FETCH`操作中检索或影响的行数。

```plsql
IF emp_cursor%ROWCOUNT > 0 THEN
    -- Rows were fetched or affected
ELSE
    -- No rows were fetched or affected
END IF;
```

这些属性用于获取游标的状态信息，以便在程序中根据需要进行逻辑判断和处理。它们能够帮助你更好地控制游标的操作和结果集的处理。

## 2.4.创建和使用

```plsql
DECLARE
    CURSOR emp_cursor IS
        SELECT employee_id, first_name, last_name FROM employees WHERE department_id = 10;
        
    v_employee_id employees.employee_id%TYPE;
    v_first_name employees.first_name%TYPE;
    v_last_name employees.last_name%TYPE;
BEGIN
    OPEN emp_cursor;
    
    LOOP
        FETCH emp_cursor INTO v_employee_id, v_first_name, v_last_name;
        EXIT WHEN emp_cursor%NOTFOUND;
        
        -- Process the retrieved data
        DBMS_OUTPUT.PUT_LINE('Employee ID: ' || v_employee_id || ', Name: ' || v_first_name || ' ' || v_last_name);
    END LOOP;
    
    CLOSE emp_cursor;
END;
/
```

## 2.5.游标(接受参数)

```plsql
DECLARE
    CURSOR emp_cursor (dept_id NUMBER) IS
        SELECT employee_id, first_name, last_name 
        FROM employees 
        WHERE department_id = dept_id;
        
    v_employee_id employees.employee_id%TYPE;
    v_first_name employees.first_name%TYPE;
    v_last_name employees.last_name%TYPE;
    
    v_dept_id NUMBER := 10; -- Department ID to pass to the cursor
BEGIN
    OPEN emp_cursor(v_dept_id);
    
    LOOP
        FETCH emp_cursor INTO v_employee_id, v_first_name, v_last_name;
        EXIT WHEN emp_cursor%NOTFOUND;
        
        -- Process the retrieved data
        DBMS_OUTPUT.PUT_LINE('Employee ID: ' || v_employee_id || ', Name: ' || v_first_name || ' ' || v_last_name);
    END LOOP;
    
    CLOSE emp_cursor;
END;
/
```

# 3.存储过程

## 3.1概念作用

存储过程是一种在数据库中存储的、预编译的、可重用的数据库对象。它是由一组SQL语句和控制结构（例如条件、循环等）组成的，通常以单个命令进行执行。存储过程具有输入参数和输出参数，可以通过传递参数来调用并返回结果。存储过程通常由数据库管理系统编译并存储在数据库中，以便随后执行。

存储过程在数据库中的作用和优势包括：

1. **提高性能：** 存储过程可以在数据库服务器上执行，减少了网络通信的开销。由于存储过程通常是预编译的，它们可以提供更快的执行速度，尤其是对于复杂的操作。
2. **减少数据传输：** 存储过程可以在服务器上执行，只传输结果，而不需要传输大量的数据到客户端进行处理。
3. **代码重用：** 存储过程可以被多个应用程序共享，从而实现代码的重用，减少代码的冗余。
4. **安全性：** 存储过程可以设定访问权限，只允许授权用户执行特定的操作，从而提高数据库的安全性。
5. **简化复杂操作：** 存储过程可以用于执行复杂的数据操作，如事务管理、数据转换、数据清洗等，从而简化应用程序中的代码。
6. **减少数据库负载：** 存储过程可以在服务器端执行，减少了客户端的负载，从而提高系统的整体性能。
7. **批处理操作：** 存储过程可以实现批处理操作，从而一次性处理多条SQL语句，提高数据库的效率。

创建存储过程通常需要使用数据库的存储过程语言（如PL/SQL对于Oracle、T-SQL对于SQL Server等）。存储过程可以通过调用其名称并传递参数来执行。存储过程在应用程序开发中是一个非常重要的概念，它可以帮助提高性能、减少开发工作量，并促进数据的安全管理。

## 3.2.语法

以下是一个典型的存储过程的语法示例，用于展示其基本结构和组成部分：

```plsql
CREATE OR REPLACE PROCEDURE procedure_name (parameter1 datatype, parameter2 datatype, ...) 
IS
    -- 声明变量
    variable1 datatype;
    variable2 datatype;

BEGIN
    -- 存储过程主体，包括SQL语句、控制结构等
    -- 可以使用参数和变量来操作数据

    -- 示例：输出参数值到控制台
    DBMS_OUTPUT.PUT_LINE('Parameter 1: ' || parameter1);
    
    -- 示例：执行SQL语句
    SELECT column INTO variable1 FROM table WHERE condition;

    -- 示例：循环处理数据
    FOR record IN (SELECT * FROM table) LOOP
        -- 处理每条记录
    END LOOP;

    -- 示例：异常处理
    EXCEPTION
        WHEN exception_type THEN
            -- 处理特定异常
        WHEN OTHERS THEN
            -- 处理其他异常
END;
/
```

在这个示例中，`CREATE OR REPLACE PROCEDURE`用于创建或替换存储过程。`procedure_name`是存储过程的名称。`parameter1`、`parameter2`等是输入参数的定义，它们用于在调用存储过程时传递值。

`IS`标记表示存储过程的主体部分的开始。在主体部分中，你可以声明变量、执行SQL语句、使用控制结构等。在这个示例中，使用了`DBMS_OUTPUT.PUT_LINE`来在控制台输出信息，使用了`SELECT`语句来检索数据，并使用了循环来处理数据。

`EXCEPTION`部分用于处理异常。你可以根据需要指定特定的异常类型进行处理，或者使用`WHEN OTHERS`来处理其他未指定的异常。

这只是一个基本的存储过程示例，实际的存储过程可能会更加复杂，根据特定的业务逻辑和需求进行设计和实现。请根据你使用的数据库管理系统和语言来适当调整语法。

## 3.3.输入参数和输出参数

在存储过程中，`IN` 和 `OUT` 是用于指定参数的传递方向的关键字。

1. **IN 参数：** `IN` 参数是存储过程的输入参数，它们用于将值传递给存储过程。存储过程可以使用这些参数进行计算和处理，但不能修改它们的值。`IN` 参数的值在调用存储过程时传递，并在存储过程内部使用。
2. **OUT 参数：** `OUT` 参数是存储过程的输出参数，它们用于从存储过程返回值。存储过程可以在内部修改 `OUT` 参数的值，这些修改将在存储过程执行完成后传递给调用方。调用存储过程时，传递给 `OUT` 参数的是一个变量，存储过程会将结果存储在该变量中。

此外，还有 `IN OUT` 参数，它可以作为输入和输出参数同时使用。`IN OUT` 参数在调用存储过程时需要提供初始值，存储过程会修改这个值，并在执行完成后将修改后的值传递回来。

以下是示例，展示了如何在 Oracle 的 PL/SQL 存储过程中使用 `IN` 和 `OUT` 参数：

```plsql
sqlCopy code
CREATE OR REPLACE PROCEDURE example_proc(
    p_input IN NUMBER,
    p_output OUT NUMBER
)
IS
BEGIN
    -- 使用输入参数 p_input 进行计算
    p_output := p_input * 2; -- 修改输出参数的值
END;
/
```

在这个示例中，`p_input` 是一个输入参数，`p_output` 是一个输出参数。在存储过程内部，输入参数 `p_input` 用于计算，然后结果被存储在输出参数 `p_output` 中。在调用存储过程时，需要传递一个变量来存储输出参数的结果。