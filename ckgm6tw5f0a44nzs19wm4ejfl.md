---
title: "Common Data Structures and Algorithms used in JavaScript"
datePublished: Thu Oct 22 2020 11:15:00 GMT+0000 (Coordinated Universal Time)
cuid: ckgm6tw5f0a44nzs19wm4ejfl
slug: common-data-structures-and-algorithms-used-in-javascript
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1603453135178/gdnrSKPOj.png
tags: javascript, data-structures

---

Most hiring managers use data structures and algorithms as the first step of technical interviews for testing their potential employees. They send them tasks to do either self-set by the company or scheduled in sites like [Hackerrank for Companies](https://www.hackerrank.com/), [Codewars](https://www.codewars.com/), etc to be done within a limited time period. Nonetheless, OOP, data structures, and algorithms are said to be computer science topics that every developer should cover.

In this article, I'm going to cover some common data structures and algorithms and how it's solved using JavaScript:-

- *Anagrams*

Basically *anagram* is a word, phrase, or sentence formed from another by rearranging its letters. For example, we'll be checking these two sets if they're anagrams or not using Boolean (true/false), loops, and regex 


```
'ROAD! SAFETY', 'fairy tales'
'rail safety', 'fairy tales'
``` 
Solution

./index.js
```
function anagrams(stringA, stringB) {
    const aCharMap = buildCharMap(stringA);
    const bCharMap = buildCharMap(stringB);

    if (Object.keys(aCharMap).length !== Object.keys(bCharMap).length) {
        return false;
    }

    for (let char in aCharMap) {
        if (aCharMap[char] !== bCharMap[char]) {
            return false;
        }
    }
    return true
}

function buildCharMap(str) {
    const CharMap = {};
    for (let char of str.replace(/[^\w]/g, '').toLowerCase()) {
        CharMap[char] = CharMap[char] + 1 || 1
    }
    return CharMap;
}

anagrams('ROAD! SAFETY', 'fairy tales') //false
anagrams('rail safety', 'fairy tales') //true
``` 
- *Fibonacci*

Most people will remember this term from the Prison Break series back then but ideally, the *Fibonacci sequence* starts like this: 0, 1, 1, 2, 3, 5, 8, 13, 21, 34, 55, and so on forever. Each number is the sum of the two numbers that precede it.


```

function fib(n) {
    const result = [0, 1];

    for (let i = 2; i<= n; i++) {
        const a = result[i - 1]
        const b = result[i - 2];
        result.push(a + b)
    }
    var data = result.map((data) => {
        return data;
    }) 
    console.log(data);
}
(fib(12)) // [0,  1,  1,  2,  3,  5, 8, 13, 21, 34, 55, 89, 144]
``` 
- *FizzBuzz*

They are common in actual interviews or interview prep the most common *FizzBuzz* is being given the task to "write a program that prints the numbers from 1 to a given number. ... For numbers which are multiples of both three and five print “FizzBuzz"

Here is an example of a JavaScript solution:-

./index.js file
```
function fizzBuzz(n) {
    for (let i = 1; i <= n; i++) {
        //Is the number a multiple of 3 and 5
        if (i % 3 === 0 && i % 5 === 0) {
            console.log('fizzbuzz')
        } else if (i % 3 === 0) {
            //is the number a multiple of 3
            console.log('fizz')
        } else if (i % 5 === 0) {
            //Is the number a multiple of 5
            console.log('buzz')
        } else {
            console.log(i)
        }
    }
}

(fizzBuzz(15)) //fizzbuzz
``` 
-  *Lettersmax / CharMax *

*CharMax* comes from CHAR data type which stores character data in a fixed-length field. This data structure determines the number of characters in a string.

./index.js
```
function maxChar(str) {
    const charMap = {};
    let max = 0;
    let maxChar = '';

    for (let char of str) {
        if (charMap[char]) {
            charMap[char]++
        } else {
            charMap[char] = 1
        }
    }
    
    for (let char in charMap) { //let in used in an object
        if (charMap[char] > max) {
             max = charMap[char]
             maxChar = char;
        }
    }
    return maxChar
}
maxChar('windows'); //w
```
- *LinkedList*

A linked list is a linear collection of data elements whose order is not given by their physical placement in memory. At times people compare this with how someone can say
> "I know someone who knows someone who knows someone else😂".
Here is an example:-

./index.js
```
class Node {
    constructor(data, next = null) {
        this.data = data;
        this.next = next;
    }
}

class LinkedList {
    constructor() {
        this.head = null;
    }
    insertFirst(data) {
        this.head = new Node(data, this.head);
    }
    size() {
        let counter = 0;
        let node = this.head;

        while (node) {
            counter++
            node = node.next;
        }
        return counter;
    }
    getFirst() {
        return this.head
    }
    getLast() {
        if (!this.head) {
            return null;
        }
        let node = this.head;
        while (node) {
            if(!node.next) {
                return node;
            }
            node = node.next;
        }
    }
    clear() {
        this.head = null;
    }
    removeFirst() {
        if (!this.head) {
            return;
        }
        this.head = this.head.next;
    }
    removeLast() {
        if (!this.head) { 
            return;
        }
        if (!this.head.next) {
            this.head = null;
            return;
        }
        let previous = this.head;
        let node = this.head.next;
        while (node.next) {
            previous = node;
            node = node.next
        }
        previous.next = null;
    }
    insertLast(data) {
        const last = this.getLast();

        if (last) {
            last.next = new Node(data);
        } else {
            this.head = new Node(data);
        }
    }
    getAt(index) {
        let counter = 0;
        let node = this.head;
        while (node) {
            if (counter === index) {
                return node;
            }
            counter++;
            node = node.next;
        }
        return null;
    }
    removeAt(index) {
        if (!this.head) {
            return;
        } 
        if (index === 0) {
            this.head = this.head.next;
            return;
        }
        const previous = this.getAt(index - 1)
        if (!previous || !previous.next) {
            return;
        } 
        previous.next = previous.next.next;
    }
    insertAt(data, index) {
        if (!this.head) {
            this.head = new Node(data);
            return;
        }
        if (index === 0) {
            this.head = new Node(data, this.head);
        }
        const previous = this.getAt(index - 1) || this.getLast();
        const node = new Node(data, previous.next); 
        previous.next = node;
    }
    forEach(fn) {
        let node = this.head;
        let counter = 0;
        while (node) {
            fn(node,counter);
            node = node.next;
        }
    }
}

const list = new LinkedList();
list .insertFirst(1)
list.insertFirst(2)
list.insertFirst(3)
list.insertFirst(4)
list.insertLast(8)

list.forEach(node => {
    node.data += 2
})
(list.head)
list.size() //5
list.getFirst().data; //6
list.getLast().data; //10
console.log(list); //LinkedList {head: Node { data: 6, next: Node { data: 5, next: [Node] } }}
```
- *Reversestring*

```
function reverse(str) {
    return str.split('').reduce((rev, char) => char + rev, '')
}
(reverse('cathy'));
```
this is how reveresInt it simply works:-

```
['c', 'a', 't', 'h', 'y'] //yhtac

''+c   
c+a
a+t
t+h
h+y

y+''
h+t
t+a
a+c
```
- *Reverse Integer*

When given a reverse task of an integer you're expected to return an integer that is its reverse.

```
function reverseInt (num) {
    const reversed = num.toString().split('').reverse().join('');
   // return reversed "string"
    return parseInt(reversed) * Math.sign(num)
}
reverseInt(675); //576
reverseInt(-675); //-576
```
- *Matrix*

A *matrix* is a two-dimensional data structure and all of its elements are of the same type. It has rows and columns. Here is how it is solved with little explanation of how it's achieved.

./index.js file
```

function matrix(n) {
    const results = [];

    for(let i = 0; i<n; i++) {
        results.push([])
    }

    let counter = 1;
    let startColumn = 0;
    let endColumn = n - 1;
    let startRow = 0;
    let endRow = n - 1;

    while(startColumn <= endColumn && startRow <= endRow) {
        //TOP ROW 
        for (let i = startColumn; i <= endColumn; i++) {
            results[startRow][i] = counter;
            counter++;
            //results[1][0] = 12;
            //results[1][1] = 13;
            //results[1][2] = 14;
        }
        startRow++;
        //results[0][0] = 1;
        //results[0][1] = 2;
        //results[0][2] = 3;
        //results[0][3] = 4;

        //RIGHT COLUMN
        for (let i = startRow; i<= endRow; i++) {
            results[i][endColumn] = counter;
            counter++
        }
        endColumn--;
        //results[1][3] = 5;
        //results[2][3] = 6;
        //results[3][3] = 7;

        //result[2][2] = 15;
        //results[3][2] = 16;


        //BOTTOM ROW
        for (let i = endColumn; i >= startColumn; i-- ) {
            results[endRow][i] = counter;
            counter++;
        }
        endRow--;
        //results[3][2] = 8;
        //results[3][1] = 9;
        //results[3][0] = 10;

        //START COLUMN
        for (let i = endRow; i >= startRow; i--) {
            results[i][startColumn] = counter;
            counter++;
        }
        startColumn++;
        //results[2][0] = 11;
        //results[1][0] = 12;

        //results[][] = 15;
        //results[][] = 16;
    }
    return results;
}
matrix(4);
matrix(3);
```
- *Pyramid*

 A pyramid structure consists of three main parts: the several layers of pyramidal data; for example, points, lines, and faces.

Here is an example of a solution:-

./index.js

```
function pyramid(n) {
    const midpoint = Math.floor((2 * n - 1) / 2)

    for (let row = 0; row < n; row++) {
        let level = '';
        for (let column = 0; column < 2 * n - 1; column++) {
            if (midpoint - row <= column && midpoint + row >= column) {
                level += '#'
            } else {
                level += ' '
            }
        }
        console.log(level);
    }
}
(pyramid(3))
```
- *Steps*

In the layman language *step* a flat surface, especially one in a series, on which to place one's foot when moving from one level to another. This data structure aims to achieve numbers or data organized in step structure.

./index.js
```
function steps(n, i = 1) {
    if (i > n) {
        return;
    } 
    console.log('#'.repeat(i) + ' '.repeat(n - i))
    steps(n, i + 1) //recursion-technique of repeating
}
(steps(3)) //
#  
## 
###
```
    
- *Vowels*

 This data structure checks and counts for the number of vowels in a string.

./index.js
```
function vowels(str) {
    let count = 0;
    const checker = ['a','e','i','o','u'];
    for (let char of str.toLowerCase()) {
        if(checker.includes(char)) {
            count++
        }
    }
    return count;
}
vowels('school') //2
```
- *Weave*

For this data structure, you'll need to create a folder(weave/) with two files (./index.js and ./queue.js)

weave/queue.js
```
class Queue {
    constructor() {
        this.data = []
    }
    add(record) {
        this.data.unshift(record);
   } 
    remove() {
        return this.data.pop()
   }

   peek() {
       return this.data[this.data.length - 1]
   }
}
console.log(Queue)
module.exports = Queue;
```
weave/index.js
```
const Queue = require('./queue');

function weave(sourceOne, sourceTwo) {
    const q = new Queue();
  
  // while there is an item in first source or an item in second source
  while (sourceOne.peek() || sourceTwo.peek()) {
    // if there is an item in source one 
    if (sourceOne.peek()) {
      // remove the item from source one and add it to queue 
      q.add(sourceOne.remove());
    }
    
    // if there is an item in source two
    if (sourceTwo.peek()) {
      // remove the item from source two and add it to queue 
      q.add(sourceTwo.remove());
    }
  }
  
  // return the queue
  return q;
}
const queueOne = new Queue();
queueOne.add(1);
queueOne.add(2);
queueOne.add(3);

const queueTwo = new Queue();
queueTwo.add('Hi');
queueTwo.add('There');
//var q = new Queue();

const q = weave(queueOne, queueTwo);
q.remove(); //=> 1
q.remove() //=> 2 
q.peek()
q.remove();  
q.peek() 
console.log(q) //Queue { data: [ 3, 'There', 2 ] }
```
- *Stack*

```
class Stack {
    constructor() {
        this.data = [];
    }
    push(record) {
        this.data.push(record)
    }
    pop() {
        return this.data.pop()
    }
    peek() {
        return this.data[this.data.length - 1]
    }
} 
const s = new Stack()
s.push(1)
s.push(2)
s.push(3)
console.log(s.pop()) //3
```
- *QfromStack*

In this stack, we consider first-in last-out. We'll need a folder (stack) with two files (./index.js and ./stack.js)

stack/stack.js
```
class Stack {
    constructor() {
        this.data = [];
    }
    push(record) {
        this.data.push(record)
    }
    pop() {
        return this.data.pop()
    }
    peek() {
        return this.data[this.data.length - 1]
    }
} 

module.exports = Stack;
```

stack/index.js
```
const Stack = require('./stack'); 
class Queue {
    constructor() {
        this.first = new Stack()
        this.second = new Stack();
    }

    add(record) {
        this.first.push(record)
    }
// 1['a'] //2['a'] 1 ['']
// 2['a'] 1['']
    remove() {
        while (this.first.peek()) {
            this.second.push(this.first.pop())
        }

        const record = this.second.pop()

        while (this.second.peek()) {
            this.first.push(this.second.pop())
        }
        return record;
    }
    peek() {
        while (this.first.peek()) {
            this.second.push(this.first.pop())
        }
    }
}
const q = new Queue();
q.add(1);
q.add(2);
q.add(3);
q.peek();
q.remove();
q.remove();
q.remove();
console.log(q);  //Queue { first: Stack { data: [] }, second: Stack { data: [] } }

```
Thanks for reading my article, if you're interested in going further you can check my [data structures repository on GitHub](https://github.com/jebitok-dev/data-structures-js) for the more!