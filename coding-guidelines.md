#Coding guidlines

This document describes the coding rule that need to be applied in our projects.

##General
Android projects must follow the guidelines specified by the platform to insure maintainability over time and developers.

https://source.android.com/source/code-style.html

Most of the rules described in this document come from the Roadhouse Coding Standards and Best Practices document available here.
http://home.roadhouse.com.au/Development/CodingGuidelines/

Even if there is C# specific rules, most of the content is still up to date.

##Readability

- Put class fields at the start of the class.
- Methods should be ordered so the reader can read the code from top to bottom.
Ex:
```
methodA(){
    methodB();
    methodC();
}

methodB(){
    methodD();
}

methodD(){
}

methodC(){
}
```


- Use Meaningful, descriptive words to name variables, methods and classes. Do not use abbreviations.

Good:
```
string address
int salary 
```
Not Good:
```
string nam
string addr
int sal
int s
```


Do not use single character variable names like i, n, s etc. Use names like index, temp 

It is hard to catch at first sight what `s` represents, what its type is and it is difficult to spot in the code.

One exception in this case would be variables used for iterations in loops: 

```
for (int i = 0; i < count; i++)
{
    ...
}
```

  
- Avoid using a misleading name
```
string date
```

- Prefix boolean variables, properties and methods with “is” or “has”.
```
private bool isFinished
```

##Best practices

- Method's length should be between 1 to ~25 lines.
- Classes should be < 200 lines in most cases.
- Use static inner class to avoid memory leaks and use WeakReference to keep a reference to the outer class.
```
public class Outer {
    private int mField;
    private static class SampleThread extends Thread {

        private final WeakReference<Outer> mOuter;

        SampleThread(Outer outer) {
            mOuter = new WeakReference<Outer>(outer);
        }

        @Override
        public void run() {
            // Do execution and update outer class instance fields.
            // Check for null as the outer instance may have been GC'd.
            if (mOuter.get() != null) {
                mOuter.get().mField = 1;
            }
        }
    }
}

Excerpt From: Anders Göransson. “Efficient Android Threading.” 
```

- Use immutable classes in multi-threading environment.
This will spare a lot of time debugging why an object has an inconsistent state.

http://javarevisited.blogspot.com.au/2013/03/how-to-create-immutable-class-object-java-example-tutorial.html

##Comments
- Comment carefully. There is no need to comment every line.
Here are the requirements:
    + a description for the class
    + for public methods
    + to explain a technical behavior

- Avoid noise comments as explained by Robert C. Martin:
```
"Sometimes you see comments that are nothing but noise. They restate the obvious and provide no new information.

/**
* Default constructor.
*/
protected AnnualDateRule() {
}

/** The day of the month. */
private int dayOfMonth;
And then there’s this paragon of redundancy:

/**
* Returns the day of the month.
*
* @return the day of the month.
*/
public int getDayOfMonth() {
    return dayOfMonth;
}
"
Excerpt From: Unknown. “Clean Code.”  
```

- Avoid writing comment where you can make a method:
```
“Don’t Use a Comment When You Can Use a Function or a Variable

Consider the following stretch of code:
// does the module from the global list <mod> depend on the
// subsystem we are part of?
if (smodule.getDependSubsystems().contains(subSysMod.getSubSystem()))

This could be rephrased without the comment as

ArrayList moduleDependees = smodule.getDependSubsystems();
String ourSubSystem = subSysMod.getSubSystem();
if (moduleDependees.contains(ourSubSystem))

The author of the original code may have written the comment first (unlikely) and then written the code to fulfill the comment. However, the author should then have refactored the code, as I did, so that the comment could be removed.”

Excerpt From: Unknown. “Clean Code.”
```


##Architecture
- Always use multi layer architecture. 
Wrap third party libraries into a class. This allows to keep the code clean, to respect the Single responsability principle and to improve maintainability. (see SOLID (https://en.wikipedia.org/wiki/SOLID_(object-oriented_design)))

For example, in a UI class, no database code, network, object's model processing should be in the UI class, only calls to the class responsible for the database or network or model etc...

- Heavy processes need to be executed in a background thread.
“So, long operations should be handled on a background thread. Long-running tasks typically include:
Network communication
Reading or writing to a file
Creating, deleting, and updating elements in databases
Reading or writing to SharedPreferences
Image processing
Text parsing”

Excerpt From: Anders Göransson. “Efficient Android Threading.” 

##Tests
Apps need to be tested to validate the app's behavior.
- Unit tests
- Integration tests
- UI tests

##References

Here are some references about how to write clean and maintainable code.

- Clean Code by Robert C. Martin (ISBN 0132350882 (ISBN13: 9780132350884)) http://www.goodreads.com/book/show/3735293-clean-code
- Refactoring: Improving the Design of Existing Code by Martin Fowler, Kent Beck, Don Roberts (ISBN 0201485672 (ISBN13: 9780201485677)) http://www.goodreads.com/book/show/44936.Refactoring
- Working Effectively with Legacy Code by Michael C. Feathers (ISBN 0131177052 (ISBN13: 0076092025986)) http://www.goodreads.com/book/show/44919.Working_Effectively_with_Legacy_Code
- Design Patterns: Elements of Reusable Object-Oriented Software by Erich Gamma, Ralph Johnson, John Vlissides, Richard Helm (ISBN 0201633612 (ISBN13: 9780201633610)) http://www.goodreads.com/book/show/85009.Design_Patterns
- Efficient Android Threading: Asynchronous Processing Techniques for Android Applications by Anders Goransson (ISBN 1449364136 (ISBN13: 9781449364137)) http://www.goodreads.com/book/show/20578421-efficient-android-threading?from_search=true&search_version=service_impr
- Effective Java Programming Language Guide by Joshua Bloch, Guy L. Steele Jr.(ISBN 0201310058 (ISBN13: 0785342310054)) http://www.goodreads.com/book/show/105099.Effective_Java_Programming_Language_Guide

