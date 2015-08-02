---
layout: post
comments: true
title: Is this string a palindrome? - Down the rabbit hole.
tags:
- interview questions
- the basics
- java
- haskell
---
It's that time of year in the graduate calendar where you really should have job interviews lined up and plenty of applications sent in. I'm running behind on that front for a few reasons, some good, some not so good.

During the first term of my MSc I thought that FizzBuzz was an atrocious interview question - it's incredibly simple, easy to structure and easy to reason about. If someone is applying for a job in development those basics should be a given, right? 

The more I thought about it the more I realised it was similar to asking a musician to play a scale. Any musician worth their salt can play a scale, but you can tell a lot from *how* they play a scale. Is their tone even? Is their rhythm consistent? Is their articulation consistent? Dynamics? Do they use vibrato? What about a crescendo? The scale itself isn't really the point - but you can sure tell a lot about a musician just from listening to them play one.

One of the classic programming assignments (according to the internet) is checking to see if a string is a palindrome. Technically there are many ways.... but really? These start off awful and get better as they go. 

The Obfuscator.

When I started learning Java I did it using an old academic website. A lot of it was excellent - there were plenty of quizzes and exercises, everything was explained clearly, in general it was a site for actually learning how to code. However it didn't really stress things like meaningful naming conventions. They really can make the difference between nice clean code and an entry in [The Daily WTF](https://thedailywtf.com) 

```java
	public boolean method1(String x) {
		StringBuilder y = new StringBuilder(x);
		String z = y.reverse().toString();

		x = x.replace(" ", "");
		z = z.replace(" ", "");

		return x.equalsIgnoreCase(z);
	}

```

Recursion.

I love recursion. I've been learning Haskell for a month or so and currently tail recursion is one of my favourite things about it. It's such a joy to write and read and Haskell makes it so easy, even for problems that would seem a more natural fit for iteration. Java, not so much. Oh boy.

```java
	public boolean isPalindrome(String string) {
		// if we've got all the way to the middle it's a palindrome
		if (string.length() == 0 || string.length() == 1) {
			return true;
		}

		if (string.charAt(0) == string.charAt(string.length() - 1)) {
			// remove the outer characters and try again
			return isPalindrome(string.substring(1, string.length() - 1));
		} else {
			return false;
		}
	}
```

No Object Creation.

This is more involved with index matching and book-keeping. It compares the character at each end of the string and moves in one character with each loop. It stops at the exact halfway point for strings with an even length, and one before for an odd-length string. It will be quicker though, probably useful if your string is over a million characters long and the method is called every millisecond. 

```java
	public boolean isPalindrome(String string) {
		string = string.replace(" ", "");
		string = string.toUpperCase();

		for (int count = 0; count < string.length() / 2; count++) {
			if (string.charAt(count) != string.charAt(string.length()
					- (count - 1))) {
				return false;
			}
		}

		return true;
	}

```

Baby's First Loop

This is the way I did it on [Coding Bat] (http://codingbat.com). It reverses the string using multiple String concatenations. It's not exactly good form to create a new object on every iteration of the loop...... but it does the job. The loop contains a length()-1 in the for loop inviting an off by one error. A little bit less readable, but still ok.

```java
	public boolean isPalindrome(String string) {
		String reversedString = "";

		for (int count = string.length()-1; count >= 0; count--) {
			reversedString += string.charAt(count);
		}

		return string.equalsIgnoreCase(reversedString);
	}
```

K.I.S.S. Java 

This way is one of the simplest possible. It's extremely readable, short, uses well known implementations and avoids loops and boilerplate book-keeping. It's not going to be the most efficient code, and it completely ignores special characters and non-ASCII characters so it's not exactly exhaustive, but right now I'm willing to sacrifice that for some nice code as long as the assumptions are documented properly.

```java
	/**
	 * Checks if a string is palindromic. This method is case insensitive and
	 * ignores spaces. This is currently only applicable to ASCII characters.
	 * 
	 * @param string
	 * @return true if the string is a palindrome
	 */
	public boolean isPalindrome(String string) {
		// remove white space from the string
		string = string.replace(" ", "");

		StringBuilder builder = new StringBuilder(string);
		String reversedString = builder.reverse().toString();

		return string.equalsIgnoreCase(reversedString);
	}

```

K.I.S.S. Haskell

Did I mention I like Haskell? I like Haskell. A lot. Excluding type declarations (which aren't necessarily needed) there are four lines of code and hardly any boilerplate. I could have written it in two lines if I was to sacrifice readability.

```haskell
-- longer readable way
isPalindrome :: String -> Bool
isPalindrome string = preparedString == (reverse preparedString)
                where preparedString = toUpperCase (removeSpaces string)

removeSpaces:: String -> String
removeSpaces = filter (/= ' ')

toUpperCase :: String -> String
toUpperCase = map (toUpper) 

-- two lines! but less readable
isPalindrome :: String -> Bool
isPalindrome string = preparedString == (reverse preparedString)
                where preparedString = ( map (toUpper) . filter (/= ' ') ) string

```

Hopefully you stopped slowly dying inside as you got down the list. It still seems to me that anything like above is the bare minimum required to not be thrown out the door mid-interview, not an indication of an excellent candidate.

