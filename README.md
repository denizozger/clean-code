# Software Craftmanship Guide() {

*Extracts from book [Clean Code: A Handbook of Agile Software Craftsmanship by Robert C. Martin](http://www.amazon.co.uk/Clean-Code-Handbook-Software-Craftsmanship/dp/0132350882). My intention is not to provide a summary, but to share the notes that I want to keep in mind*.

## <a name='TOC'>Table of Contents</a>

1. [Introduction](#introduction)
2. [Clean Code](#cleanCode)
3. [Meaningful Names](#meaningfulNames)
4. [Methods](#methods)
5. [Comments](#comments)
6. [Formatting](#formatting)
7. [Objects and Data Structures](#objects)


## <a name='introduction'>Introduction</a>

- 80% or more of what we do is maintenance
- Code is never perfect
- The only valid measurement of code quality: **WTFs / minute**
- Learning to write clean code is hard work. It requires **more than just the knowledge of principles and patterns**. You must sweat over it. You must practice it yourself, and watch yourself fail. You must watch others practice it and fail. You must see them stumble and retrace their steps. You must see them agonize over decisions and see the price they pay for making those decisions the wrong way.

**[[⬆]](#TOC)**

## <a name='cleanCode'>Clean Code</a>

- LeBlanc’s law: **Later equals never**
- Most managers want the truth, even when they don’t act like it. Most managers want good code, even when they are obsessing about the schedule. They may defend the schedule and requirements with passion; but that’s their job. **It’s your job to defend the code with equal passion**. It is unprofessional for programmers to bend to the will of managers who don’t understand the risks of making messes.
- All developers feel the pressure to make messes in order to meet deadlines. True professionals know that the second part of the conundrum is wrong. You will not make the deadline by making the mess. Indeed, the mess will slow you down instantly, and will force you to miss the deadline. **The only way to make the deadline is to keep the code as clean as possible at all times.**
- **Bad code tempts the mess to grow.** When others change bad code, they tend to make it worse. Metaphor: A building with broken windows looks like nobody cares about it. So other people stop caring. They allow more windows to become broken. Eventually they actively break them.
- Clean code should be: **Pleasing to read, does one thing well, minimal, looks like it was written by someone who cares**
- If we all checked in our code a little cleaner than when we checked it out, the code simply could not rot. The cleanup doesn’t have to be something big. Change one variable name for the better, break up one function that’s a little too large, eliminate one small bit of duplication.

**[[⬆]](#TOC)**

## <a name='meaningfulNames'>Meaningful Names</a>

- Use intention revealing names. If a name requires a comment, then the name does not reveal its intent.

```java
    // bad
    int d; // elapsed time in days

    // good
    int elapsedTimeInDays;
 ```

```java
    // bad
    public List<int[]> getThem() {
		List<int[]> list1 = new ArrayList<int[]>(); 
			for (int[] x : theList)
				if (x[0] == 4) list1.add(x);
		return list1; 
	}

    // good
    public List<int[]> getFlaggedCells() {
		List<int[]> flaggedCells = new ArrayList<int[]>(); 
			for (int[] cell : gameBoard)
				if (cell[STATUS_VALUE] == FLAGGED) 
					flaggedCells.add(cell);
		return flaggedCells; 
	}
 ```
- Make meaningful distinctions

  + Imagine you have `Product` class. If you have another called `ProductInfo` or `ProductData`, you have made the names different without making them mean anything different. In the absence of specific conventions, the variable `moneyAmount` is indistinguishable from `money`, `customerInfo` is indistinguishable from `customer`, `accountData` is indistinguishable from `account`, and `theMessage` is indistinguishable from `message`. 

- Methods should have verb or verb phrase names like `postPayment`, `deletePage`, or `save`. When constructors are overloaded, use static factory methods with names that describe the arguments. Consider enforcing their use by making the corresponding constructors private.

```java
	// bad
	Complex fulcrumPoint = new Complex(23.0);

	// good
	Complex fulcrumPoint = Complex.FromRealNumber(23.0);
```

- Pick one word per concept. It’s confusing to have `fetch`, `retrieve`, and `get` as equivalent methods of different classes. 

- People are afraid of renaming things for fear that some other developers will object. Do not share that fear and be grateful when names change (for the better). 

**[[⬆]](#TOC)**

## <a name='methods'>Methods</a>

- The first rule of methods is that they should be small. The second rule of functions is that *they should be smaller than that*.

- Methods should not be 100 lines long. Methods should hardly ever be 20 lines long. 

- Methods should do only one thing. They should do it well. They should do it only.

- How to determine if a method is doing only one thing? Describe the method by describing it as a brief **TO** paragraph: *TO RenderPageWithSetupsAndTeardowns, we check to see whether the page is a test page and if so, we include the setups and teardowns. In either case we render the page in HTML.*

- **Don’t be afraid to make a name long**. A long descriptive name is better than a short enigmatic name. A long descriptive name is better than a long descriptive comment. The smaller and more focused a method is, the easier it is to choose a descriptive name. Don’t be afraid to spend time choosing a name. 

- The ideal number of arguments for a function is **zero** (niladic). Next comes one (monadic), followed closely by two (dyadic). Three arguments (triadic) should be avoided where possible. More than three (polyadic) requires very special justification—and then shouldn’t be used anyway.

```java
	// bad
	Circle makeCircle(double x, double y, double radius); 

	// good
	Circle makeCircle(Point center, double radius);
```

- Passing a `boolean` into a function is a truly terrible practice. It immediately complicates the signature of the method, loudly proclaiming that this function does more than one thing. **It does one thing if the flag is true and another if the flag is false!** The method call `render(true)` is just plain confusing to a poor reader. Mousing over the call and seeing `render(boolean isSuite)` helps a little, but not that much. We should have split the function into two: `renderForSuite()` and `renderForSingleTest()`.

- Choosing good names for a function can go a long way toward explaining the intent of the function and the order and intent of the arguments. 

```java
	// ok
	void assertEquals(expected, actual)

	// better
	void assertExpectedEqualsActual(expected, actual)
```
- Anything that forces you to check the function signature is equivalent to a double-take. In the days before object oriented programming it was sometimes necessary to have output arguments. However, much of the need for output arguments disappears in OO languages because `this` is *intended* to act as an output argument.

```java
	// Does this function append s as the footer to something? Or does it append some footer to s?
	void appendFooter(s);

	// good
	report.appendFooter();
```

- Functions should either do something or answer something, but not both.

```java
	// bad
	public boolean set(String attribute, String value);
	
	// “If the username attribute was previously set to unclebob” or 
		// “set the username attribute to unclebob and if that worked then...”
	if (set("username", "unclebob"))...

	// good
	if (attributeExists("username")) {
		setAttribute("username", "unclebob");
		...
	}
```

- Prefer exceptions to returning error codes

```java
	// Bad
	if (deletePage(page) == E_OK) {
		if (registry.deleteReference(page.name) == E_OK) {
			if (configKeys.deleteKey(page.name.makeKey()) == E_OK){
			logger.log("page deleted");
			} else {
			logger.log("configKey not deleted");
			}
		} else {
			logger.log("deleteReference from registry failed");
		}
	} else {
		logger.log("delete failed");
		return E_ERROR;
	}
	
	// Good
	try {
		deletePage(page);
		registry.deleteReference(page.name);
		configKeys.deleteKey(page.name.makeKey());
	}
	catch (Exception e) {
		logger.log(e.getMessage());
	}
```

- Functions should do one thing. Error handing is one thing. Thus, **a function that handles
errors should do nothing else**. This implies (as in the example above) that if the keyword
try exists in a function, it should be the very first word in the function and that there
should be nothing after the catch/finally blocks.

- Duplication may be the root of all evil in software.

- "When I write functions, they come out long and complicated. They have lots of
indenting and nested loops. They have long argument lists. The names are arbitrary, and
there is duplicated code. But I also have a suite of unit tests that cover every one of those
clumsy lines of code. So then I massage and refine that code, splitting out functions, 
changing names, eliminating duplication. I shrink the methods and reorder them. Sometimes I break out whole
classes, all the while keeping the tests passing. In the end, I wind up with functions that
follow the rules I’ve laid down in this chapter. I don’t write them that way to start.
I don’t think anyone could."

- **Functions are the verbs of that language, and classes are the nouns.** Master programmers think 
of systems as stories to be told rather than programs to be written.

**[[⬆]](#TOC)**

## <a name='comments'>Comments</a>

- "Don't comment bad code - rewrite it."

- **The proper use of comments is to compensate for our failure to express ourself in
code**. Comments are always failures. We must have them because we cannot always figure out how to express 
ourselves without them, but their use is not a cause for celebration. **So when you find yourself in a position where 
you need to write a comment, think it through and see whether there isn’t some way to turn 
the tables and express yourself in code**.

- The older a comment is, and the farther away it is from the code it describes,
the more likely it is to be just plain wrong. The reason is simple. Programmers can’t realistically
maintain them.

- **Comments Do Not Make Up for Bad Code!** One of the more common motivations for writing comments is bad code. 
We write a module and we know it is confusing and disorganized. We know it’s a mess. So we say to ourselves,
“Ooh, I’d better comment that!” No! You’d better clean it! Clear and expressive code with few comments is 
far superior to cluttered and complex code with lots of comments. **Rather than spend your time writing the 
comments that explain the mess you’ve made, spend it cleaning that mess.**

- Explain yourself in code

```java
	// Bad:
	
	// Check to see if the employee is eligible for full benefits
		if ((employee.flags & HOURLY_FLAG) &&
		(employee.age > 65))
	
	// Good:
	if (employee.isEligibleForFullBenefits())
```

- **Avoid redundant comments**: These comments below probably takes longer to read than the code itself

```java
	// Utility method that returns when this.closed is true. Throws an exception
	// if the timeout is reached.
	public synchronized void waitForClose(final long timeoutMillis)	throws Exception {
		if(!closed) {
			wait(timeoutMillis);
		if(!closed)
			throw new Exception("MockResponseSender could not be closed");
		}
	}
```

- **Avoid mandated comments**: It is just plain silly to have a rule that says that every function must have a javadoc, or
every variable must have a comment.

```java
	/**
	*
	* @param title The title of the CD
	* @param author The author of the CD
	* @param tracks The number of tracks on the CD
	* @param durationInMinutes The duration of the CD in minutes
	*/
	public void addCD(String title, String author, int tracks, int durationInMinutes) {
		CD cd = new CD();
		cd.title = title;
		cd.author = author;
		cd.tracks = tracks;
		cd.duration = duration;
		cdList.add(cd);
	}
```

- Don't use a comment when you can use a function or variable

```java

	// Bad:
	
	// does the module from the global list <mod> depend on the subsystem we are part of?
	if (smodule.getDependSubsystems().contains(subSysMod.getSubSystem()))
	
	// Good:
	ArrayList moduleDependees = smodule.getDependSubsystems();
	String ourSubSystem = subSysMod.getSubSystem();
	if (moduleDependees.contains(ourSubSystem))

```

- Few practices are as odious as commenting-out code. Don’t do this! **Others who see that commented-out code won’t 
have the courage to delete it. They’ll think it is there for a reason and is too important to delete**. 
So commented-out code gathers like dregs at the bottom of a bad bottle of wine. There was a time, back in the sixties, 
when commenting-out code might have been useful. But we’ve had good source code control systems for a very long time now. 
Those systems will remember the code for us.

**[[⬆]](#TOC)**

## <a name='formatting'>Formatting</a>

- **Dependent Functions**: If one function calls another, they should be vertically close,
and the caller should be above the callee, if at all possible. This gives the program a natural flow.

**[[⬆]](#TOC)**

## <a name='objects'>Objects</a>

*to be continued..*

**[[⬆]](#TOC)**

# };


