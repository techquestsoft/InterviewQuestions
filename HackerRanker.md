#### 1. Given inorder and post order traversal of a tree find its preorder traversal.
````
inorder = [6,3,8,1,7,5,9,2,4,10]
postorder = [6,8,7,1,3,9,10,4,2,5]

options:
1. [5,3,6,1,8,7,2,9,4,10]
2. [5,3,6,1,8,7,9,2,4,10]
3. [5,3,6,1,8,7,2,9,10,4]
4. none
````
#### 2. Java Executor service and callable interface usage, consider the following code
```` java
import java.util.concurrent.*;
public class JavaExecutorService {
private Double whatsInFuture(ExecutorService executor,
Callable<Double> task){
//lines of code will be place here
try{
return executor.submit(task).get();
}catch (Exception ex){
return null;
}
//3 option compilation error, executer.submit method returns Future,
// but required double
/*try{
return executor.submit(task);
}catch (Exception ex){
return null;
}*/
//3 option compilation error, execute method takes Runnable as
// parameter not callable
/* try{
return executor.execute(task);
}catch (Exception ex){
return null;
}*/

        // 4. executor service cannot invoke
    }
}
````

#### 3. WINDOW function frame specification: which expression has the correct syntax?
1. SELECT SUM(amount) OVER (ROWS BETWEEN UNKNOWN PRECEDING AND CURRENT ROW) FROM transacations;
2. SELECT SUM(amount) OVER (ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW) FROM transacations;
3. SELECT SUM(amount) OVER (ROWS BETWEEN UNKNOWN PRECEDING AND UNBOUNDED ROW) FROM transacations;

#### 4. Which response will cause the following test to fail?
````
@Test
    @DisplayName("Filter price check 3")
    public void FilterPriceCheck3() throws Exception{
        mockMvc.perform(get("/filter/price/150/100"))
            .andDo(print())
            .andExpect(jsonPatch("$[*].product", containsInAnyOrder("product1","product2",
            "product3")))
    }
Answers:
    [{"product":"Product 5"},{"product":"Product 1"},{"product":"Product 2"}
    [{"product":"Product 5"},{"product":"Product 2"},{"product":"Product 1"}
    [{"product":"Product 1"},{"product":"Product 5"},{"product":"Product 2"}
    [{"product":"product 5"},{"product":"product 1"},{"product":"product 2"}
````

#### 5. Given an integer  k and a list of integers, count the number of distinct valid pairs of integers (a,b) in the list of for which a + k = b. Two paris of integers (a,b), and (c,d) are considered distinct if at least one element of (a,b) does not belong to (c,d). Note that the elements in a pair might be the same element in the array. An instance of this is below where k=0.
    Example:
        n=4
        numbers = [1,1,1,2]
        k=1
This array has 3 different valid paris: (1,1), (1,2), (2,2)
For k=1 there is only one valid pair which satisfies a+k=b, the pair (a,b)=(1,2)

    Example:
        n=2
        numbers = [1,2]
        k=0
This array has 3 different valid paris: (1,1), (1,2), (2,2)
For k=0 there are two valid pairs which satisfies a+k=b, the pair (a,b)=(1,1)  and (2,2)

Constraints:
    2 <= n <= 2*10^5
    0 <= numbers[i] <= 10^9, where 0 <= i < n
    0 <= k <= 10^9

#### 6. Can you make a palindrome? A function receives a string and a series of queires.
    Example:
        s = cdecd
        startIndex = [0,0,0,0]
        endIndex = [0,1,2,3]
        substitution = [0,1,1,0]

        query 1: s[0,0] = 'c'
                 substitution[0] = 0
                the substring 'c' is a palindrome after 0 substitutions, so result = '1'

        query 2: s[0,1] = 'cd'
                 substitution[1] = 1
                change 'c' to 'd' or 'd' to 'c' in 1 substitution to get a palindrome 'dd' or 
                'cc', so result = '11'

        query 3: s[0,2] = 'cde'
                 substitution[2] = 1
                change 'c' to 'e' or 'e' to 'c' in 1 substitution to get a palindrome 'cdc' or 
                'ede', so result = '111'

#### 7. Hibernate N+1 Solution
````java
    @Entity(name="pistol")
    @Table(name="pistol")
    public class Pistol {
        @Id
        private Long id,
        
        private String name,
        
        @onetomany
        List<Ammunition> ammunitions;

        //getters and setters
    }

//Ammunition.java
    @Entity(name="Ammunition")
    @Table(name="Ammunition")
    public class Ammunition {
        @Id
        private Long id;
        
        private String name;
        
        @manytoone
        private Pistol pistol;

        //getters and setters
    }

//the following JPA query causes N+1 problem, which of the following can be used to solve this N+1 
//problem.

List<Ammunition> ammunitions = entityManager.createQuery("select a from Ammunition a, Ammunition.
class).getResult());

for(Ammunition ammunition: ammunitions){
    log.info("The pistol had Ammunition", ammunition.getPistol(),getName());
}
````
#### 8. Number of Binary Search Trees: How many ways can a binary search tree be formed 
containing the integers from 1 through 5 inclusive?

#### 9. Rearrange the Array: Modify the elements of an array such that there is no element that has higher value element to its left and to its right.

    For ex:
        [2,1,1,1,3] is not valid because the 1s have higher value to their left and rights.
        [1,1,1,2,3] valid since valuees higher than 1 or 2 occur only to their right side.

In one operation any element can be reduced by 1, looking at example array [2,1,1,1,3], the 
first element can be reduced to 1 in 1 move. this result [1,1,1,1,3] which is in result designed 
arrangement.

select the answer that shows the final modified array and the sum of its values based on the 
following initial array.

arr[7] = {1,2,1,2,1,3,1}
Answers:
1. {1,1,2,1,1,2,1} sum=9
2. {1,1,1,1,1,3,1} sum=9
3. {1,1,1,1,1,1,1} sum=7

#### 10. Static Analysis: output of the following program:

````java
public class X {
    public static void main(String[] args) throws java.io.IOException {
        try{
            badMethod();
            System.out.println("A");

        }catch(Exception e){
            System.out.println("B");
        }
        finally {
            System.out.println("C");
        }

        System.out.println("D");


    }

    public static void badMethod(){

    }
}

````