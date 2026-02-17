---
title: "Python: Simple Demo"
datePublished: Tue Feb 17 2026 19:39:39 GMT+0000 (Coordinated Universal Time)
cuid: cmlr0buu9000e02l83ba04qs9
slug: python-simple-demo
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1771356988425/f12e8898-a592-44b8-9d9f-bf5688b64ede.png
tags: python, cybersecurity, tryhackme, beginners-cybersecurity

---

[Python: Simple Demo](https://tryhackme.com/room/pythonsimpledemo) is a pre-security room at TryHackMe that enables beginners to understand the fundamentals of Python, a programming language primarily used for scripting in cybersecurity exploits and challenges.

## Introduction

In the first two rooms, [Data Representation](https://tryhackme.com/room/datarepresentation) and [Data Encoding](https://tryhackme.com/room/dataencoding), we learned how computers understand numbers, characters, colors, and even emojis. In this room, we will explore one of the most popular and friendly programming languages, Python.

You may have heard of the Python programming language numerous times. Python is a high-level general-purpose programming language. By *high-level*, we mean it hides most implementation details; *general-purpose* means you can use Python for a wide variety of scenarios, from web applications and automation scripts to data science and machine learning.

In this room, we will create a “Guess the Number” game. Our plan will be as follows:

1. The computer secretly picks a number between 1 and 20.
    
2. The user keeps guessing until they get it right.
    
3. The computer tells the user whether their guess is too low or too high.
    

An example session running the program is shown below:

Terminal

```bash
ubuntu@tryhackme:~$ python guess_the_number.py
I'm thinking of a number between 1 and 20
Take a guess: 10
Too high, try again.
Take a guess: 5
Too low, try again.
Take a guess: 7
Too low, try again.
Take a guess: 8
You got it in 4 tries!
```

## **Learning Objectives**

* Learn about Python variables
    
* Understand how conditional statements are used
    
* See iteration (loop) in action
    

## **Prerequisites**

* [Inside a Computer System](https://tryhackme.com/room/insideacomputer)
    
* [Data Representation](https://tryhackme.com/room/datarepresentation)
    
* [Data Encoding](https://tryhackme.com/room/dataencoding)
    

## Variables

You can follow along by starting the attached VM by clicking the **Start Machine** button shown above. Visual Studio Code should open automatically as shown in the screenshot below.

![Python demo is opened](https://tryhackme-images.s3.amazonaws.com/user-uploads/5f04259cf9bf5b57aed2c476/room-content/5f04259cf9bf5b57aed2c476-1770747209622.png align="left")

The program needs to start by picking a random **secret** number between 1 and 20. Then the user will begin making a **guess** after another till they get it right. This approach requires two variables: `secret` and `guess`. We can also track the number of **tries** (attempts) it took the user to find the number; for this, we will use the variable `tries`.

Python offers the `random.randint()` method, which returns a random integer within the specified bounds. For example, `random.randint(1, 20)` returns a random number between 1 and 20. Since the user has not made any attempts, we set `tries` to 0; moreover, we set `guess` to a value outside the valid range. To tell the user that the program picked a number, we use Python’s `print()` method to display an informational message on the screen. We show the current Python script below:

```python
import random  # gives us tools for picking random numbers

secret = random.randint(1, 20)  # a <= secret <= b
tries = 0
guess = 0  # start with a value that cannot be the secret (since secret is 1..20)

print("I'm thinking of a number between 1 and 20")
```

This code snippet achieves the following:

1. Use the `random.randint()` method from the `random` library to pick a random number between 1 and 20 and save it to `secret`.
    
2. Creates two variables, `tries` and `guess`, and sets them to `0`.
    
3. Display a message on the screen to inform the user that a number has been picked.
    

With our variables ready, we prompt the user to make a guess and set `guess` accordingly. We begin by saving the user’s input to a temporary `text` variable, then use Python’s `int()` to convert it to an integer. Furthermore, we need to increment `tries` each time the user makes a guess.

```python
text = input("Take a guess: ")  # input() returns text (a string)
guess = int(text)  # convert the text to a number

tries = tries + 1  # add 1 try (written long-form for clarity)
```

So far, our program does the following:

1. Pick a random number between `1` and `20` and save it in `secret`.
    
2. Ask the user to take a guess, convert it to an integer, and save it in `guess`.
    
3. Increment `tries` by 1.
    

To check whether the user made a correct guess, we should be able to compare against various cases; this requires conditional statements and comparison operators.

## **The Incomplete Draft**

In this task, we built a basic program that you can test for yourself and edit in the attached VM. Although this program picks a random number and reads the user’s input, it does not compare the two, which makes it practically useless as a game. In the next task, we upgrade this program so that it gives the user some feedback on how their `guess` compares to the `secret` number. The program below is saved as `guess_`[`v1.py`](http://v1.py) in the `/home/ubuntu/Python-Demo` directory.

```python
import random  # gives us tools for picking random numbers

secret = random.randint(1, 20)  # a <= secret <= b
tries = 0
guess = 0  # start with a value that cannot be the secret (since secret is 1..20)

print("I'm thinking of a number between 1 and 20")

text = input("Take a guess: ")  # input() returns text (a string)
guess = int(text)  # convert the text to a number

tries = tries + 1  # add 1 try
```

### Answer the questions below

1. What is the name of the function we used to display text on the screen? `print()`
    
2. What is the name of the function that we used to convert user input to an integer? `int()`
    

## Conditional Statements

Before starting, please note that you can download the attached zip file. It includes all versions of the program that are present on the target VM. This archive allows you to review the project's evolution, compare implementations, or test each version independently on your local machine. Having all iterations in one package should make your work more efficient.

Next, we compare the user’s `guess` to the `secret` number and give a helpful hint: whether it is out of range, too low, too high, or if they got it right. It is like asking ourselves, “Is the number less than 1 or greater than 20?” If the answer is yes, we tell the user that the *number is out of range*. If the answer is no, we pose a second question: “Is the user’s guess less than the secret number?” If the answer is yes, we tell the user that the guess is *too low*. Let’s rephrase this in pseudo-code, i.e., English language that is closer to the programming language.

* If the guess is less than 1 or greater than 20, print “Out of range.” (If this is not the case, proceed to the next step.)
    
* Else if the guess is less than the secret number, print “Too low.” (If this is not the case, proceed to the next step.)
    
* Else if the guess is larger than the secret number, print “Too high.” (If this is not the case, proceed to the next step.)
    
* Else print “You got it.”
    

By reflecting on the above steps, it is only logical that if the guess is not less than or larger than the secret number, it would be equal to the secret number. In programming, whenever the condition after the “**if**” is not true (written as `if CONDITION:` in Python), the program checks the condition after the “**else if**” (written as `elif CONDITION:` in Python), and if it is also false, it checks the next “else if”. If there are no more “else if” statements left, the program executes the final “**else**” (written as `else:` in Python). Let’s convert the above pseudo-code to working Python code.

```python
# Give a hint using if / elif / else.
if guess < 1 or guess > 20:
    print("That number is out of range. Try again.")
elif guess < secret:
    print("Too low, try again.")
elif guess > secret:
    print("Too high, try again.")
else:
    print("You got it in", tries, "tries!")
```

In this implementation, comparing the user’s `guess` to the chosen `secret` will lead to various cases:

* The user makes a `guess` outside the allowed bounds, i.e., 1 and 20
    
* The user’s `guess` is less than `secret`
    
* The user’s `guess` is greater than `secret`
    
* The user’s `guess` is correct
    

If the above looks confusing, consider the following numerical examples.

**Numerical Example 1**

We assume that the secret number is 10. If the user inputs 30, the first `if` condition will evaluate to true, and Python will execute the part after `if guess < 1 or guess > 20:` and print “That number is out of range. Try again.”

**Numerical Example 2**

We assume that the secret number is 10. Suppose the user inputs 5, the first `if` condition will evaluate to false because 5 is neither less than 1 nor greater than 20. Python will check the condition following the first `elif` and it will evaluate to true because 5 is indeed less than 10. Consequently, the program will print “Too low, try again.”

**Numerical Example 3**

We assume that the secret number is 10. Suppose the user inputs 15, the first `if` condition will evaluate to false because 15 is neither less than 1 nor greater than 20. The program will check the condition following the first `elif` and it will also evaluate to false because 15 is not less than 10. The program will check the condition following the second `elif`, and this one will evaluate to true, because 15 is larger than 10. As a result, the program will display, “Too high, try again.”

## **The First Draft**

In this task, we built a working program that you can test for yourself and edit in the attached VM. The main limitation is that this program gives the user one chance. In the next task, we upgrade this program to allow the user an infinite number of tries to make the correct guess. The program below is saved as `guess_`[`v2.py`](http://v2.py) in the `/home/ubuntu/Python-Demo` directory.

```python
import random  # gives us tools for picking random numbers

secret = random.randint(1, 20)  # a <= secret <= b
tries = 0
guess = 0  # start with a value that cannot be the secret (since secret is 1..20)

print("I'm thinking of a number between 1 and 20")

text = input("Take a guess: ")  # input() returns text (a string)
guess = int(text)  # convert the text to a number

tries = tries + 1  # add 1 try

# Give a hint using if / elif / else.
if guess < 1 or guess > 20:
    print("That number is out of range. Try again.")
elif guess < secret:
    print("Too low, try again.")
elif guess > secret:
    print("Too high, try again.")
else:
    print("You got it in", tries, "tries!")
```

### Answer the questions below

1. How does Python write “else if”? `elif`
    
    ![](https://cdn.hashnode.com/res/hashnode/image/upload/v1771356908150/c4b0f2b2-29a4-4dab-bf7c-1f347ed6a854.png align="center")
    
2. What will the program display if the user’s input is 50? `That number is out of range. Try again.`
    

## Iterations

It is too hard to guess the number when given a single chance. To make this game more interesting, it should provide the user with at least a few chances. We will be highly tolerant and allow the user as many attempts as needed to find the secret number. To give the user more than one chance, we need to use our secret weapon: **loops**! In programming, loops, or more formally, **iterations**, allow us to execute the same lines of code multiple times. In more formal phrasing, we can iterate over the same code block as long as a specific condition holds.

Let’s look at a few examples of iteration in our daily lives. Consider the case where you want to buy a new t-shirt: you will see one shop after another until you find one that suits you. The condition here is finding a suitable t-shirt. Consider another example where you are searching for an empty parking space in a vast parking lot. You will check one row of cars after another until you finally find a space. The condition here is finding a suitable parking space.

Back to our game, we will keep repeating the code we wrote in Task 3 until the user’s `guess` matches the `secret` number. In Python, “does not equal” is written as `!=`. In other words, the condition “`guess` does not equal `secret`” is written as `guess != secret`. To write this as a `while` loop, we use the form `while CONDITION:`; in our case, it is `while guess != secret:`.

Let’s paste the code from Task 3 in the body of our `while` loop.

```python
# Repeat until the user guesses the secret number.
while guess != secret:
    text = input("Take a guess: ")  # input() returns text (a string)
    guess = int(text)  # convert the text to a number
    
    tries = tries + 1  # add 1 try

    # Give a hint using if / elif / else.
    if guess < 1 or guess > 20:
        print("That number is out of range. Try again.")
    elif guess < secret:
        print("Too low, try again.")
    elif guess > secret:
        print("Too high, try again.")
    else:
        print("You got it in", tries, "tries!")
```

The code above will execute as follows. First, Python will check the condition after the `while`; if it is true, it will run all the indented lines. If the condition is false, it terminates.

**Numerical Example 1**

We assume that the secret number is 10. In the first run, the guess is 0. The `while` condition will evaluate to true because 10 is not equal to 0. The program will ask the user to “Take a guess,” and it will proceed from there.

**Numerical Example 2**

We assume that the secret number is 10. If the user entered 5 earlier, it means they did not guess the number in the previous attempt. The `while` condition will evaluate to true because 10 is not equal to 5. The program will give the user another chance and prompt them to “Take a guess.”

**Numerical Example 3**

We assume that the secret number is 10. If the user entered 10 earlier, it means they successfully guessed the number in the previous attempt. Let’s see if the program will prompt them for another guess. The `while` loop will evaluate to false because 10 is equal to 10; in other words, `10 != 10` is false. The indented code block within the `while` statement will no longer execute.

## **The Second Draft**

In this task, we have completed coding our game. You can test it for yourself and edit in the attached VM. The program below is saved as `guess_`[`v3.py`](http://v3.py) in the `/home/ubuntu/Python-Demo` directory.

```python
import random  # gives us tools for picking random numbers

# ----------------------------
# Guess the Number (Beginner Demo)
# ----------------------------
# The computer picks a secret number.
# The player keeps guessing until they find it.

secret = random.randint(1, 20)  # a <= secret <= b
tries = 0
guess = 0  # start with a value that cannot be the secret (since secret is 1..20)

print("I'm thinking of a number between 1 and 20")

# Repeat until the user guesses the secret number.
while guess != secret:
    text = input("Take a guess: ")  # input() returns text (a string)
    guess = int(text)  # convert the text to a number
    
    tries = tries + 1  # add 1 try

    # Give a hint using if / elif / else.
    if guess < 1 or guess > 20:
        print("That number is out of range. Try again.")
    elif guess < secret:
        print("Too low, try again.")
    elif guess > secret:
        print("Too high, try again.")
    else:
        print("You got it in", tries, "tries!")
```

### Answer the questions below

1. What type of loop does this program use? `while`
    
2. What will the program display if the user makes the correct guess in 3 tries? `You got it in 3 tries!`
    

## Conclusion

In this room, we covered three key pillars in imperative programming languages:

* Variables
    
* Conditionals with `if` and `else`
    
* Loops with `while`
    

If this is your first exposure to Python, don’t worry about being able to write a similar program yourself. Learning programming languages requires practice, and although you can now understand and explain this program, practice is indispensable to write something similar. We strongly encourage you to compare the code in this room with that of the JavaScript: Simple Demo room (coming soon); this will give you a good perspective on how different modern programming languages are designed and written.

In the next room, the JavaScript: Simple Demo room (coming soon), we will dive into an example program written using the JavaScript language. JavaScript runs in your web browser to make websites more interactive; moreover, it is powering an increasingly large number of web applications thanks to Node.js. You can explore it in the next room.