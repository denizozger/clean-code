# Software Craftmanship Guide() {

*My personal notes of best clean coding practices, from book Clean Code: A Handbook of Agile Software Craftsmanship by Robert C. Martin. My intent is not to provide a summary, but to share my notes that I want to revise from time to time*.

## <a name='TOC'>Table of Contents</a>

1. [Introduction](#introduction)
2. [Clean Code](#cleanCode)
3. [Meaningful Names](#meaningfulNames)

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










