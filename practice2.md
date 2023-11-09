# Question 1: Turn String to URL
URLs cannot have spaces. 
Instead, all spaces in a string are replaced with %20. 
Write an algorithm that replaces all spaces in a string with %20.

You may not use the replace() method or regular expressions to solve this problem. 
Solve the problem with and without recursion.

Example
Input: "Jasmine Ann Jones"
Output: "Jasmine%20Ann%20Jones"

## Without Recursion:

```javascript
const toUrl = function(string) {
  if (typeof(string) != 'string') {
    return string;
  }
  if (!string.includes(" ")){
    return string;
  }
  // we know we'll need to loop through each character,
  // and replace each whitespace with %20,
  // so let's use Map:
  
  // map is an array method, so we turn our string into an array:
  const stringArray = string.split("");

  // we'll look at each character in our new array, and if it's a space,
  // transform it into the necessary characters, otherwise just return that character
  const mappedArray = stringArray.map((char) => (char === " ") ? "%20" : char);

  // it's still an array, so let's join it back together as a string, and return it
  return mappedArray.join("");
}
```
## With Recursion:

```javascript
const toUrl = function(string) {
  // termination case
  if (typeof(string) != 'string') {
    return string;
  }
  // base case
  if (!string.includes(" ")){
    return string;
  }
  // is the first element a space?
  // if it is, we'll start our recursion here,
  // and send in the rest of our string without the first element,
  // and replace our first element with the necessary characters
  if (string[0] === " ") {
    return "%20" + toUrl(string.substring(1));
  } else {
    // otherwise, if the first character IS NOT a space,
    // keep it, then recursively loop through the rest of the string:
    return string[0] + toUrl(string.substring(1));
  }
}
```

# Question 2: Array Deduping
Write an algorithm that removes duplicates from an array. 
Do not use a function like filter() to solve this. 
Once you have solved the problem, demonstrate how it can be solved with filter(). 
Solve the problem with and without recursion.

Example
Input: [7, 9, "hi", 12, "hi", 7, 53]

Output: [7, 9, "hi", 12, 53]

# Without Recursion:

```javascript
const dedupe = function(array) {
  if (array.length <= 1) {
    return array;
  }

  let cleanArray = [];

  array.forEach((element) => {
    if(!cleanArray.includes(element)) {
      cleanArray.push(element);
    }
  });

  return cleanArray;
}
```

# With Recursion:

```javascript
const dedupe = function(array) {
  // base case
  if (array.length === 0) {
    return [];
  }

  // with recursion, we know we're working with LIFO,
  // so we're going to rebuild our array from zero:
  // hence our base case

  const firstElement = array[0];
  const remainingElements = array.split(1);
  // we're going to work backwords from out empty array,
  // so we know we'll kick out of our recursion when we hit 
  // an empty array and work back through our layers of dedupe()'s
  const filteredArray = dedupe(remainingElements);

  // by the time we hit that empty array, 
  // we check: is 53 included in []? no? okay, add it then move on
  // is 7 in [53]? no? add it, then move on
  if (!filteredArray.includes(firstElement)) {
    return [firstElement, ...filteredArray]
  } else {
    return filteredArray;
  }
}
```

# With Filter: 

```javascript
const dedupe = function(array) {

  // with filter, we just want to get the elements that aren't repeated:
  // so if we use indexOf, we know it returns the FIRST APPEARANCE of that element
  // if we loop through our array, and the current index of our element is not the 
  // same as the index returned from indexOf, then we'll ignore it:
  const cleanArray = array.filter((element, index) => {
    return array.indexOf(element) === index;
  })

  return cleanArray;
}
```

# Question 3: Compress Strings

Write an algorithm that takes a string with repeated characters and compresses them, using a number to show how many times the repeated character has been compressed. 
For instance, aaa would be written as 3a. 
Solve the problem with and without recursion.

Example
Input: "aaabccdddda"

Output: "3ab2c4da"

# Without Recursion:

```javascript
const compress = function(string) {
  if (typeof(string) != 'string') {
    return string;
  }
  if (string.length === 0) {
    return "";
  }

  // we want to loop through the string and compare each element
  // if the first character is the same as the next, let's add to a count
  // 
  let count = 1;
  let newString = "";

  for(let i = 0; i < string.length; i++){
    if (string[i] === string[i+1]){
      count ++;
    } // when the characters no longer match, we check our count
    else {
      if (count > 1) {
        newString += count + string[i];
      } else {
        newString += string[i];
      }

      count = 1;
    }
  }
  return newString;
}

```

# With Recursion:

```javascript
const compress = function(string) {
  if (typeof(string) != 'string')
  {
    return string;
  }
  if (string.length === 0) 
  {
    return "";
  }

  // we're working with LIFO, which means we'll be starting from
  // an empty string
  // so we should start HERE:
  let head = string[0];
  let rest = compress(string.substring(1));
  // by the time we hit our base case, we'll have a head = a,
  // and the rest = "";
  
  // what we want to do is see if a is in this substring, if it isnt',
  // we'll add them together and return it back to the next layer up,
  // if it is, we'll add a count and return that count back to the next layer up
  // which means we need to check for a count as well

  // so, is that first element of the rest of the body a number?
  // if it is, check to see if we're comparing another duplicate character
  // if it IS a duplicate, then we update the count and move on
  // if it IS NOT a duplicate, then we add the character to the body and move on
  if (Number(rest[0])){
    if (head === rest[1]) {
      return rest.replace(rest[0], (Number(rest[0]) + 1));
    } else {
      return head + rest;
    }
  // if the first element of the rest of the body is NOT a number,
  // check to see if the first element is a duplicate of our head,
  // if it is, start the count and move on,
  // if it isn't, add the head to the rest of the body and move on
  } else {
    if (head === rest[0]) {
      return (2 + rest);
    } else {
      return head + rest;
    }
  }
}
```

# Question 4: Checking Uniqueness 
Write an algorithm that determines whether all the elements in a string are unique. 
You may not convert the string into an array or use array methods to solve this problem. 
The algorithm should return a boolean.

Example
Input: "hello"
Output: false

Input: "copyright"
Output: true

# Answer:

```javascript
const unique = function(string) {

  if (typeof(string) != 'string') {
    return;
  }
  if (string.length <= 1) {
    return true;
  }

  // we'll brute force this and just loop twice,
  // checking each individual letter to see if it repeats
  // at any point:
  // starting at index 0 and moving on from there,
  // we'll see if every element after is a repeat
  // each iteration of checking element i we can ignore the previous
  // elements because we already know they aren't a match:

  for (let i = 0; i < string.length; i++){
    for (let j = i + 1; j <string.length; j++) {
      if (string.charAt(i) === string.charAt(j)) {
        return false;
      }
    }
  }

  return true;
}
```

# Question 5: Array Sorting
Write an algorithm that sorts an array without using the sort() method. 
There are many different sorting algorithms — take the time to read about the following:

Quick sort
Merge sort
Heap sort
Insertion sort
Bubble sort
Selection sort
You may implement any of the above algorithms (or your own) to solve the problem — as long as it doesn't use sort().

Example
Input: [9, 2, 7, 12]
Output: [2, 7, 9, 12]

# Quick Sort

```javascript
const quickSort = function(array) {
  // base case: we know an array of 1 element is already sorted!
  if (array.length <= 1) {
    return array;
  }

  const pivot = array[0];
  const smaller = [];
  const greater = [];

  for (let i = 1; i < array.length; i++) {
    if (array[i] < pivot) {
      smaller.push(array[i]);
    } else {
      greater.push(array[i]);
    }
  }

  return [...quickSort(smaller), pivot, ...quickSort(greater)]

}

```