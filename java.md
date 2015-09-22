java hints

JDK The software for programmers who want to write Java programs;
JRE The software for consumers who want to run Java programs;

A class is immutable if none of its methods ever mutate its objects. For exam-
ple, the String class is immutable.

the static belongs to the class, not to any individual object.

Static Constants static final , Math.PI , System.out.

static methods can't not access non-static fields in their class but non static method can access static field in their class.

Overloading occurs if several methods have the
same name (in this case, the GregorianCalendar constructor method) but different parame-
ters. The compiler must sort out which method to call. It picks the correct method by
matching the parameter types in the headers of the various methods with the types of
the values used in the specific method call. A compile-time error occurs if the compiler
cannot match the parameters or if more than one match is possible. (This process is
called overloading resolution.)
The return type is not part of the method signature. That is, you cannot have two methods
with the same names and parameter types but different return types.

The initialization block runs first, and
then the body of the constructor is executed.

construction process. Here is what happens in detail when a con-structor is called:
	1. All data fields are initialized to their default value ( 0 , false , or null ).
	2. All field initializers and initialization blocks are executed, in the order in which they
	occur in the class declaration.
	3. If the first line of the constructor calls a second constructor, then the body of the sec-
	ond constructor is executed.
	4. The body of the constructor is executed.

Static initialization occurs when the class is first loaded , and has default values 0 , false , null.


some objects utilize a resource other than memory, such as a file or a handle
to another object that uses system resources. In this case, it is important that the resource
be reclaimed and recycled when it is no longer needed.
The finalize method will be called before the
garbage collector sweeps away the object. In practice, do not rely on the finalize method for
recycling any resources that are in short supply—you simply cannot know when this
method will be called.

The fact that an object variable can refer to multiple actual types is called polymorphism.


Manager boss = new Manager(. . .);
Employee[] staff = new Employee[3];
staff[0] = boss;
boss.setBonus(5000); // OK
staff[0].setBonus(5000); // ERROR
Manager m = staff[i]; // ERROR
Manager[] managers = new Manager[10];
Employee[] staff = managers; // OK
staff[0] = new Employee("Harry Hacker", ...); why staff[0].setBonus(5000); will make error

an interface is not a class but a set of requirements for classes that want to conform to the interface.


Notice that all exceptions descend from Throwable , but the hierarchy immediately splits
into two branches: Error and Exception .

The Error hierarchy describes internal errors and resource exhaustion inside the Java
runtime system. You should not throw an object of this type.

The Exception hierarchy also splits into two branches: exceptions that derive from RuntimeException and those that do not.

A RuntimeException happens because you made a programming error. Any other exception occurs because a bad thing, such as an I/O error,happened to your otherwise good program.

**RuntimeException** is your responability to avoid them by checking say for array size before accessing it .i.e. A bad cast , out-of-bounds array access ArrayIndexOutOfBoundsException , null pointer access NullPointerException.

The Java Language Specification calls any exception that derives from the class Error or the class RuntimeException an **unchecked exception** , All other exceptions are called **checked exception** The compiler checks that you provide exception handlers for all checked exceptions.

a method must declare all the checked exceptions that it might throw.Unchecked exceptions are either beyond your control ( Error ) or result from conditions that you should not have allowed in the first place ( RuntimeException )

finally clause : the code throws an exception that is caught in a catch clause, in our case, an IOException . For this, the program executes all code in the try block, up to the point at which the exception was thrown. The remaining code in the try block is skipped. The program then executes the code in the matching catch clause, then the code in the finally clause.

