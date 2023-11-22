#1. Which version of java is using? why?

#2. Java memory model : Java memory areas, where below variables will store?
    Class metadata
    Static string
    Long sal
    Volatile double

#3. What is the difference between stack overflow exception and out of memory error?

#4. Fail fast Vs Failsafe iterator?

#5. Difference between collection and string?

#6. Map and Flatmap stream?

#7. Where Flatmap can be used?

#8. What is Optional and where can be used?

#9. Functional Interface? where can be used?

#10. In what scenarios will you use Abstract Class vs Interfaces? Give Examples.

#11. There are some exceptions that cannot caught by try catch. How to catch such exceptions? Can we prevent our program to crash if we are not able to catch such exceptions.

#12. What is try-with-resources in java?
    In Java, the try-with-resources statement is a feature introduced in Java 7 to simplify resource management. It is designed to automatically close or release resources, such as files or network connections, when they are no longer needed. This helps in reducing the likelihood of resource leaks and makes the code more concise.
    
    Here's the basic syntax of the try-with-resources statement:
        try (ResourceType1 resource1 = new ResourceType1(); ResourceType2 resource2 = new ResourceType2()) {
                // Code that uses the resources
        } catch (ExceptionType e) {
                // Exception handling
        }
    In this syntax:
    
    ResourceType1 and ResourceType2 are classes that implement the AutoCloseable or Closeable interface. These interfaces have a method named close() that is invoked when the try block is exited.
    
    The resources are declared within the parentheses after the try keyword. They are initialized in the try-with-resources statement and automatically closed when the try block is exited, either normally or due to an exception.
    
    You can catch any exceptions that may be thrown in the catch block as usual.
    
    By using try-with-resources, you can avoid explicit finally blocks to close resources and make your code cleaner and less error-prone. The resources are closed in the reverse order in which they were opened. If multiple resources are declared, each resource is separated by a semicolon (;). It's important to note that the resource classes must implement the AutoCloseable or Closeable interface for this feature to work.



