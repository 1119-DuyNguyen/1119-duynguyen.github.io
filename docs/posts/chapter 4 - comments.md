---
authors:
  - duy
share: true
date: 2024-01-31
modified_at: 2024-03-06
---

TARGET DECK: CLEAN_CODE_BOOK_ROBERT_C_MARTIN::c4
FILE TAGS: clean-code-c4-comments

---

Nothing can be quite so helpful as a well-placed comment. Nothing can clutter up a module more than frivolous dogmatic comments. Nothing can be quite so damaging as an old comment that propagates lies and misinformation.

Q: Why comments is bad ?
A: 2 things
- **compensate for our failure to express ourself in code**. If our programming languages were expressive enough, or if we had the talent to subtly wield those languages to express our intent, we would not need comments very much—perhaps not at all.
- they can **lie** because the older a comment is, and the farther away it is from the code it describes, the more likely it is to be just plain wrong. The reason is simple. Programmers can't realistically maintain them.
<!--ID: 1706753127412-->

# Comments Do Not Make Up for Bad Code

Clear and expressive code with few comments is far superior to cluttered and complex code with lots of comments. Rather than spend your time writing the comments that explain the mess you've made, spend it cleaning that mess.

# Explain Yourself in Code

Q: Is comment necessary in this study case ? Explain
```java
// Check to see if the employee is eligible for full benefits
if ((employee.flags & HOURLY_FLAG) && (employee.age > 65))
```
A: No because you can Explain Yourself in Code
```java
if (employee.isEligibleForFullBenefits())
```
<!--ID: 1706753127427-->

## Good Comments

Some comments are necessary or beneficial. However the only truly good comment is the comment you found a way not to write.

### Legal Comments

Sometimes our corporate coding standards force us to write certain comments for legal reasons. For example, copyright and authorship statements are necessary and reasonable things to put into a comment at the start of each source file.
Q: Why is this comment good ?
```
// Copyright (C) 2003,2004,2005 by Object Mentor, Inc. All rights reserved.  
// Released under the terms of the GNU General Public License version 2 or later.
```
A: Legal comments- Comments like this should not be contracts or legal tomes. Where possible, **refer to a standard license or other external document rather than putting all the terms and conditions into the comment.**
<!--ID: 1706753127438-->

### Informative Comments

It is sometimes useful to provide basic information with a comment. For example, consider this comment that explains the return value of an abstract method:

```java
// Returns an instance of the Responder being tested.
protected abstract Responder responderInstance();
```

A comment like this can sometimes be useful, but it is better to use the name of the function to convey the information where possible. For example, in this case the comment could be made redundant by renaming the function: `responderBeingTested`.

### Explanation of Intent

Sometimes a comment goes beyond just useful information about the implementation and provides the intent behind a decision. Example:

```java
public int compareTo(Object o)
{
  if(o instanceof WikiPagePath)
  {
    WikiPagePath p = (WikiPagePath) o;
    String compressedName = StringUtil.join(names, "");
    String compressedArgumentName = StringUtil.join(p.names, "");
    return compressedName.compareTo(compressedArgumentName);
  }
  return 1; // we are greater because we are the right type.
}
```

### Clarification

Sometimes it is just helpful to translate the meaning of some obscure argument or return value into something that's readable. In general it is better to find a way to make that argument or return value clear in its own right; but when its part of the standard library, or in code that you cannot alter, then a helpful clarifying comment can be useful.

### Warning of concequences

Q: Why is this comment good ?
```java
public static SimpleDateFormat makeStandardHttpDateFormat()  
{  
//SimpleDateFormat is not thread safe,  
//so we need to create each instance independently.  
SimpleDateFormat df = new SimpleDateFormat("EEE, dd MMM yyyy HH:mm:ss z");  
df.setTimeZone(TimeZone.getTimeZone("GMT"));  
return df;  
}
```
A: Warning of concequences - warn other programmers about certain consequences. It will prevent some overly  
eager programmer from using a static initializer in the name of efficiency
<!--ID: 1706753127449-->

### TODO Comments

It is sometimes reasonable to leave "To do" notes in the form of //TODO comments. In the following case, the TODO comment explains why the function has a degenerate implementation and what that function's future should be.

Q: Why this comment is good ?
```java
//TODO-MdM these are not needed
// We expect this to go away when we do the checkout model
protected VersionInfo makeVersion() throws Exception {
  return null;
}
```
A: TODOs are **jobs that the programmer thinks should be done, but for some reason can't do at the moment**. It might be a reminder to delete a deprecated feature or a plea for someone else to look at a problem. It might be a request for someone else to think of a better name or a reminder to make a change that is dependent on a planned event. Whatever else a TODO might be, it is not an excuse to leave bad code in the system.
<!--ID: 1706753127460-->

### Amplification

Q: Why we need this comment ?
```java
String listItemContent = match.group(3).trim();
// the trim is real important. It removes the starting
// spaces that could cause the item to be recognized
// as another list.
new ListItemWidget(this, listItemContent, this.level + 1);
return buildList(text.substring(match.end()));
```
A: A comment may be used to amplify the importance of something that may otherwise seem inconsequential.

### Javadocs in Public APIs
<!--ID: 1706753127473-->

There is nothing quite so helpful and satisfying as a well-described public API. The javadocs for the standard Java library are a case in point. It would be difficult, at best, to write Java programs without them.

## Bad Comments

Most comments fall into this category. Usually they are crutches or excuses for poor code or justifications for insufficient decisions, amounting to little more than the programmer talking to himself.

### Mumbling

Plopping in a comment just because you feel you should or because the process requires it, is a hack. If you decide to write a comment, then spend the time necessary to make sure it is the best comment you can write. Example:

```java
public void loadProperties() {

  try {
    String propertiesPath = propertiesLocation + "/" + PROPERTIES_FILE;
    FileInputStream propertiesStream = new FileInputStream(propertiesPath);
    loadedProperties.load(propertiesStream);
  }
  catch(IOException e) {
    // No properties files means all defaults are loaded
  }
}
```

What does that comment in the catch block mean? Clearly meant something to the author, but the meaning not come though all that well. Apparently, if we get an `IOException`, it means that there was no properties file; and in that case all the defaults are loaded. But who loads all the defaults?

### Redundant Comments

Q: Why is this comment redundant ?

```java
// Utility method that returns when this.closed is true. Throws an exception
// if the timeout is reached.
public synchronized void waitForClose(final long timeoutMillis) throws Exception {
  if(!closed) {
    wait(timeoutMillis);
    if(!closed)
      throw new Exception("MockResponseSender could not be closed");
  }
}
```
A: 2 things :
- **not more informative** than the code.
- It **does not justify the code, or provide intent or rationale**.

### Mandated Comments

It is just plain silly to have a rule that says that every function must have a javadoc, or every variable must have a comment. Comments like this just clutter up the code, propagate lies, and lend to general confusion and disorganization.

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

### Journal Comments

Sometimes people add a comment to the start of a module every time they edit it. Example:

```java
* Changes (from 11-Oct-2001)
* --------------------------
* 11-Oct-2001 : Re-organised the class and moved it to new package com.jrefinery.date (DG);
* 05-Nov-2001 : Added a getDescription() method, and eliminated NotableDate class (DG);
* 12-Nov-2001 : IBD requires setDescription() method, now that NotableDate class is gone (DG); Changed getPreviousDayOfWeek(),
getFollowingDayOfWeek() and getNearestDayOfWeek() to correct bugs (DG);
* 05-Dec-2001 : Fixed bug in SpreadsheetDate class (DG);
```

Today we have source code control systems, we don't need this type of logs.

### Scary Noise

Javadocs can also be noisy. What purpose do the following Javadocs (from a well-known  
open-source library) serve? Answer: nothing. They are just redundant noisy comments  
written out of some misplaced desire to provide documentation
```java
/** The name. */  
private String name;  
/** The version. */  
private String version;  
/** The licenceName. */  
private String licenceName;  
/** The version. */  
private String info;
```

### Noise Comments

The comments in the follow examples doesn't provides new information.

```java
/**
* Default constructor.
*/
protected AnnualDateRule() {
}
```

```java
/** The day of the month. */
private int dayOfMonth;
```

Javadocs comments could enter in this category. Many times they are just redundant noisy comments written out of some misplaced desire to provide documentation.

### Position Markers

This type of comments are noising

```java
// Actions //////////////////////////////////
```

### Closing Brace Comments

Example:

```java
public class wc {
  public static void main(String[] args) {
    BufferedReader in = new BufferedReader(new InputStreamReader(System.in));
    String line;
    int lineCount = 0;
    int charCount = 0;
    int wordCount = 0;
    try {
      while ((line = in.readLine()) != null) {
        lineCount++;
        charCount += line.length();
        String words[] = line.split("\\W");
        wordCount += words.length;

      } //while
      System.out.println("wordCount = " + wordCount);
      System.out.println("lineCount = " + lineCount);
      System.out.println("charCount = " + charCount);

    } // try
    catch (IOException e) {
      System.err.println("Error:" + e.getMessage());

    } //catch

  } //main
```

You could break the code in small functions instead to use this type of comments.

#### Attributions and Bylines

Example:

`/* Added by Rick */`

The VCS can manage this information instead.

### Commented-Out Code

Q: This is Commented-Out Code, Why should i remove it ?
```java
InputStreamResponse response = new InputStreamResponse();
response.setBody(formatter.getResultStream(), formatter.getByteCount());
// InputStream resultsStream = formatter.getResultStream();
// StreamReader reader = new StreamReader(resultsStream);
// response.setContent(reader.read(formatter.getByteCount()));
```
A: If you don't need anymore, please delete it, you **can back later with your VCS** ((Version Control System)) if you need it again.
<!--ID: 1706969071510-->

### HTML Comments

HTML in source code comments is an abomination, as you can tell by reading the code below.

```java
/**
* Task to run fit tests.
* This task runs fitnesse tests and publishes the results.
* <p/>
* <pre>
* Usage:
* &lt;taskdef name=&quot;execute-fitnesse-tests&quot;
* classname=&quot;fitnesse.ant.ExecuteFitnesseTestsTask&quot;
* classpathref=&quot;classpath&quot; /&gt;
* OR
* &lt;taskdef classpathref=&quot;classpath&quot;
* resource=&quot;tasks.properties&quot; /&gt;
* <p/>
* &lt;execute-fitnesse-tests
* suitepage=&quot;FitNesse.SuiteAcceptanceTests&quot;
* fitnesseport=&quot;8082&quot;
* resultsdir=&quot;${results.dir}&quot;
* resultshtmlpage=&quot;fit-results.html&quot;
* classpathref=&quot;classpath&quot; /&gt;
* </pre>
*/
```

### Nonlocal Information

If you must write a comment, then make sure it describes the code it appears near. Don't offer systemwide information in the context of a local comment.
```java
/**  
* Port on which fitnesse would run. Defaults to <b>8082</b>.  
*  
* @param fitnessePort  
*/  
public void setFitnessePort(int fitnessePort)  
{  
this.fitnessePort = fitnessePort;  
}
```

### Too Much Information

Don't put interesting historical discussions or irrelevant descriptions of details into your comments.

```
/*  
RFC 2045 - Multipurpose Internet Mail Extensions (MIME)  
Part One: Format of Internet Message Bodies  
section 6.8. Base64 Content-Transfer-Encoding  
The encoding process represents 24-bit groups of input bits as output  
strings of 4 encoded characters. Proceeding from left to right, a  
24-bit input group is formed by concatenating 3 8-bit input groups.  
These 24 bits are then treated as 4 concatenated 6-bit groups, each  
of which is translated into a single digit in the base64 alphabet.  
When encoding a bit stream via the base64 encoding, the bit stream  
must be presumed to be ordered with the most-significant-bit first.  
That is, the first bit in the stream will be the high-order bit in  
the first 8-bit byte, and the eighth bit will be the low-order bit in  
the first 8-bit byte, and so on.  
*/
```

### Inobvious Connection

The connection between a comment and the code it describes should be obvious. If you are going to the trouble to write a comment, then at least you'd like the reader to be able to look at the comment and the code and understand what the comment is talking about

Consider, for example, this comment drawn from apache commons:
```java
/*  
* start with an array that is big enough to hold all the pixels  
* (plus filter bytes), and an extra 200 bytes for header info  
*/  
this.pngBytes = new byte[((this.width + 1) * this.height * 3) + 200];
```
What is a filter byte? Does it relate to the +1? Or to the \*3? Both? Is a pixel a byte? Why
200? The purpose of a comment is to explain code that does not explain itself. It is a pity
when a comment needs its own explanation.

### Function Headers

Short functions don't need much description. A well-chosen name for a small function that does one thing is usually better than a comment header.

### Javadocs in Nonpublic Code

Javadocs are for public APIs, in nonpublic code could be a distraction more than a help.

GeneratePrimes.java

```java
/**
* This class Generates prime numbers up to a user specified
* maximum. The algorithm used is the Sieve of Eratosthenes.
* <p>
* Eratosthenes of Cyrene, b. c. 276 BC, Cyrene, Libya --
* d. c. 194, Alexandria. The first man to calculate the
* circumference of the Earth. Also known for working on
* calendars with leap years and ran the library at Alexandria.
* <p>
* The algorithm is quite simple. Given an array of integers
* starting at 2. Cross out all multiples of 2. Find the next
* uncrossed integer, and cross out all of its multiples.
* Repeat untilyou have passed the square root of the maximum
* value.
*
* @author Alphonse
* @version 13 Feb 2002 atp
*/
import java.util.*;
public class GeneratePrimes
{
/**
* @param maxValue is the generation limit.
*/
public static int[] generatePrimes(int maxValue)
{
	if (maxValue >= 2) // the only valid case
	{
	// declarations
	int s = maxValue + 1; // size of array
	boolean[] f = new boolean[s];
	int i;
	// initialize array to true.
	for (i = 0; i < s; i++)
	f[i] = true;
	// get rid of known non-primes
	f[0] = f[1] = false;
	// sieve
	int j;
	for (i = 2; i < Math.sqrt(s) + 1; i++)
	{
	if (f[i]) // if i is uncrossed, cross its multiples.
	{
	for (j = 2 * i; j < s; j += i)
	f[j] = false; // multiple is not prime
	}
	}
	// how many primes are there?
	int count = 0;
	for (i = 0; i < s; i++)
	{
	if (f[i])
	count++; // bump count.
	}
	int[] primes = new int[count];
	// move the primes into the result
	for (i = 0, j = 0; i < s; i++)
	{
	if (f[i]) // if prime
	primes[j++] = i;
	}
	return primes; // return the primes
	}
	else // maxValue < 2
	return new int[0]; // return null array if bad input.
	}
}
```

GeneratePrimes.java(refactory)
```java
/**
* This class Generates prime numbers up to a user specified
* maximum. The algorithm used is the Sieve of Eratosthenes.
* Given an array of integers starting at 2:
* Find the first uncrossed integer, and cross out all its
* multiples. Repeat until there are no more multiples
* in the array.
*/
public class PrimeGenerator
{
private static boolean[] crossedOut;
private static int[] result;
public static int[] generatePrimes(int maxValue)
{
	if (maxValue < 2)
		return new int[0];
	else
	{
		uncrossIntegersUpTo(maxValue);
		crossOutMultiples();
		putUncrossedIntegersIntoResult();
		return result;
	}
}
private static void uncrossIntegersUpTo(int maxValue)
{
	crossedOut = new boolean[maxValue + 1];
	for (int i = 2; i < crossedOut.length; i++)
		crossedOut[i] = false;
	}
private static void crossOutMultiples()
{
	int limit = determineIterationLimit();
	for (int i = 2; i <= limit; i++)
		if (notCrossed(i))
			crossOutMultiplesOf(i);
}
private static int determineIterationLimit()
{
	// Every multiple in the array has a prime factor that
	// is less than or equal to the root of the array size,
	// so we don't have to cross out multiples of numbers
	// larger than that root.
	double iterationLimit = Math.sqrt(crossedOut.length);
	return (int) iterationLimit;
}
private static void crossOutMultiplesOf(int i)
{
	for (int multiple = 2*i;
	multiple < crossedOut.length;
	multiple += i)
	crossedOut[multiple] = true;
}
private static boolean notCrossed(int i)
{
	return crossedOut[i] == false;
}
private static void putUncrossedIntegersIntoResult()
{
	result = new int[numberOfUncrossedIntegers()];
	for (int j = 0, i = 2; i < crossedOut.length; i++)
	if (notCrossed(i))
	{
		result[j++] = i;
}
private static int numberOfUncrossedIntegers()
{
	int count = 0;
	for (int i = 2; i < crossedOut.length; i++)
		if (notCrossed(i))
			count++;
		return count;
	}
}
```

# References