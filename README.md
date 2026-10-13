# tdd-rational-exercise

An exercise for practicing Test-Driven Development (TDD) with JUnit.

The project is a small Java program for doing arithmetic on rational numbers
(fractions). The `Rational` class is only partly written. Your job is to
**write the tests first**, then finish the implementation until every test passes.

This exercise is to be done **individually**.

## Project layout

```
.
├── src/main/java/Rational.java      # the class under test (partial implementation)
├── src/test/java/RationalTest.java  # JUnit 4 tests (one example test provided)
└── pom.xml                          # Maven build (JUnit 4.13)
```

## Exercise

1. You need a Java IDE and a JDK (any version) for this exercise. Use either:

   - **IntelliJ IDEA**: download it from <https://www.jetbrains.com/idea/download>.
     The free Community Edition is enough.
   - **Visual Studio Code**: download it from <https://code.visualstudio.com>,
     then install the
     [Extension Pack for Java](https://marketplace.visualstudio.com/items?itemName=vscjava.vscode-java-pack)
     from the Extensions view. It adds Maven and JUnit support.

2. Clone this repository to your local machine:

   ```sh
   git clone https://github.com/cragkhit/tdd-rational-exercise.git
   ```

3. Open the project:

   - **IntelliJ IDEA**: open the cloned folder. When asked which type of
     project to open, select **Maven**.
   - **VS Code**: choose **File > Open Folder...** and select the cloned
     folder. The Java extension detects `pom.xml` and imports the project
     automatically; wait until the status bar shows that Java is ready.

   Any Java version installed on your machine will work.

4. Look at the `Rational` class in
   [`src/main/java/Rational.java`](src/main/java/Rational.java). Its partial
   implementation is below.

   ```java
   class Rational {
       long numerator,denominator;

       class Illegal extends Exception {
           String reason;
           Illegal (String reason) {
               this.reason = reason;
           }
       }

       Rational() {
           // to be completed
       }

       Rational(long numerator, long denominator) throws Illegal {
           // to be completed
       }

       // find the simplest form of a Rational number
       private void simplestForm() {
           long computeGCD;
           computeGCD = GCD(Math.abs(numerator), denominator);
           numerator /= computeGCD;
           denominator /= computeGCD;
       }

       // find the greatest common denominator
       private long GCD(long a, long b) {
           if (a%b ==0) return b;
           else return GCD(b,a%b);
       }

       public void add(Rational x) {
           numerator = (numerator * x.denominator) + (x.numerator * denominator);
           denominator = (denominator * x.denominator);
           simplestForm();
       }

       public void subtract(Rational x) {
           // to be completed
       }

       public void multiply(Rational x) {
           // to be completed
       }

       public void divide(Rational x) {
           // to be completed
       }

       public boolean equals(Object x) {
           if (!(x instanceof Rational)) return false;
           Rational other = (Rational) x;
           // a/b == c/d  <=>  a*d == c*b, so 1/2 equals 2/4 even if neither is simplified
           return numerator * other.denominator == other.numerator * denominator;
       }

       public long compareTo(Object x) {
           Rational other = (Rational) x;
           // compare a/b with c/d by the sign of a*d - c*b; multiplying both sides by b*d
           // flips the result when b*d is negative, so correct for that sign as well
           long difference = numerator * other.denominator - other.numerator * denominator;
           return Long.signum(difference) * Long.signum(denominator * other.denominator);
       }

       public String toString() {
           return numerator + "/" + denominator;
       }

       public static void main(String[] args) {
           System.out.println("This is Rational class.");
       }
   }
   ```

   What each method should do:

   | Method | Status | Expected behaviour |
   | --- | --- | --- |
   | `Rational()` | To do | Creates a rational number with default values. |
   | `Rational(long numerator, long denominator)` | To do | Creates `numerator/denominator`. Throws `Illegal` when the inputs are invalid. |
   | `add(Rational x)` | Provided | Sets the current number to `this + x`. |
   | `subtract(Rational x)` | To do | Sets the current number to `this − x`. |
   | `multiply(Rational x)` | To do | Sets the current number to `this × x`. |
   | `divide(Rational x)` | To do | Sets the current number to `this ÷ x`. |
   | `equals(Object x)` | Provided | Returns `true` if `x` is equal to the current number, `false` otherwise. |
   | `compareTo(Object x)` | Provided | Returns `-1` if the current number is less than `x`, `0` if they are equal, `1` if it is greater. |
   | `toString()` | Provided | Returns the number as a string, e.g. `"1/2"`, `"3/4"`. |

   `equals()`, `compareTo()` and `toString()` are already implemented for you,
   so you can use them in your tests (for example,
   `Assert.assertEquals(expected, actual)` uses `equals()`). They still count
   as public methods, so test them too: you may find cases they get wrong.

5. Write JUnit test cases in `RationalTest` under `src/test/java` to test the
   class above **before** you finish implementing `Rational`. Use the
   *input space partitioning* technique you learned in class to design the
   test cases.

6. `RationalTest` already contains one test case:

   ```java
   import org.junit.After;
   import org.junit.Assert;
   import org.junit.Before;
   import org.junit.Test;

   public class RationalTest {
       Rational r1;
       Rational r2;

       @Before
       public void createRationals() {
           r1 = new Rational();
           r2 = new Rational();
       }

       @Test
       public void testAdd() {
           r1.numerator = 1;
           r1.denominator = 2;
           r2.numerator = 1;
           r2.denominator = 4;
           r1.add(r2);
           Assert.assertEquals(3, r1.numerator);
           Assert.assertEquals(4, r1.denominator);
       }

       @After
       public void clearUp() {
           r1 = null;
           r2 = null;
       }
   }
   ```

7. Run this test and check the result:

   - **IntelliJ IDEA**: click the green arrow next to the method name.
   - **VS Code**: click **Run Test** just above the method name, or open the
     **Testing** view (the flask icon in the Activity Bar) and run it from
     there.

   ![JUnit execution result in IntelliJ](docs/images/junit-1.png)

8. Once you have test cases for **all** the public methods of `Rational`,
   finish implementing `Rational`. Writing the tests before the code like
   this is called Test-Driven Development (TDD).

9. Make sure all tests pass and that your implementation of `Rational` is
   complete.

## Running the tests from the command line

```sh
mvn test
```

## Submission

When you have finished every step, zip the project and submit it as your
instructor directs.
