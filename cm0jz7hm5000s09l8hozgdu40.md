---
title: "Python Basics  (TryHackMe)"
datePublished: Sun Sep 01 2024 19:40:03 GMT+0000 (Coordinated Universal Time)
cuid: cm0jz7hm5000s09l8hozgdu40
slug: python-basics-tryhackme
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1725221550966/c7cecd19-3b2d-4cc1-a2a4-778f80dc9b93.png
ogImage: https://cdn.hashnode.com/res/hashnode/image/upload/v1725219465987/52246631-6db6-4b4d-b176-33810baae075.png
tags: scripting

---

I'm currently learning the fundamentals and introduction to cybersecurity pathways on TryHackMe(THM). This has given me a chance to learn the fundamentals of Python and answer some questions. With time being limited I won't be able to break the sections into detail this will be more of answers to the various sections of the Python Basics room.

1. On the code editor, print "Hello World". What is the flag? `THM{PRINT_STATEMENTS}`
    
    ```python
    print("Hello World")
    ```
    
2. In the code editor, print the result of 21 + 43. What is the flag? `THM{ADDITI0N}`
    
    ```python
    print(21 + 43)
    ```
    
    Print the result of 142 - 52. What is the flag? `THM{SUBTRCT}`
    
    ```python
    print(142 - 52)
    ```
    
    Print the result of 10 \* 324. What is the flag? `THM{MULTIPLICATION_PYTHON}`
    
    ```python
    print(10 * 324)
    ```
    
    Print the result of 5 squared. What is the flag? `THM{EXP0N3NT_POWER`}
    
    ```python
    print(5 ** 2)
    ```
    
3. On another new line, print out the value of height. What is the flag that appears? `THM{VARIABL3S}`
    
    ```python
    height = 200
    height = height + 50
    print(height)
    ```
    
4. On the code editor, click back on the "[script.py](http://script.py/)" tab and code a loop that outputs every number from 0 to 50. `THM{L00PS_WHILE_FOR}`
    
    ```python
    for i in range(51):
        print(i)
    ```
    
5. You've invested in Bitcoin and want to write a program that tells you when the value of Bitcoin falls below a particular value in dollars. `THM{BITC0IN_INVESTOR}`
    
    In the code editor, click on the [bitcoin.py](http://bitcoin.py/) tab. Write a function called **bitcoinToUSD** with two parameters: **bitcoin\_amount**, the amount of Bitcoin you own, and **bitcoin\_value\_usd**, the value of bitcoin in USD. The function should return usd\_value, which is your bitcoin value in USD (to calculate this, in the function, you times bitcoin\_amount variable by bitcoin\_value\_usd variable and return the value). The start of the function should look like this:
    
    ```python
    def bitcoinToUSD(bitcoin_amount, bitcoin_value_usd):
        usd_value = bitcoin_amount * bitcoin_value_usd
        return usd_value 
    ```
    
    ```python
    investment_in_bitcoin = 1.2
    bitcoin_to_usd = 40000
    
    # Calculate the value of your Bitcoin in USD
    my_bitcoin_value = bitcoinToUSD(investment_in_bitcoin, bitcoin_to_usd)
    
    # Check if the value falls below $30,000
    if my_bitcoin_value < 30000:
        print("Alert: Your Bitcoin value (${my_bitcoin_value}) has fallen below $30,000!")
    else:
        print("Your Bitcoin value is ${my_bitcoin_value}")
    ```
    
6. In the code editor, write Python code to read the flag.txt file. What is the flag in this file? `THM{F1LE_R3AD}`
    

```python
# Write your python code here
f = open("flag.txt", "r")
print(f.read())
```

Thanks for reading through my article. At the moment I hope that I will remain consistent in this learning journey and keep practicing as the days and months come by. You can leave a comment and we can also connect further either on \[X\]([https://x.com/SharonJebitok](https://x.com/SharonJebitok)) or \[LinkedIn\]([https://www.linkedin.com/in/sharon-jebitok/](https://www.linkedin.com/in/sharon-jebitok/))