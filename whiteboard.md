# Question #1: Turning Strings to URLS
URLs cannot have spaces. Instead, all spaces in a string are replaced with `%20`. 
Write an algorithm that replaces all spaces in a string with `%20`.

You may not use the `replace()` method or regular expressions to solve this problem. 
Solve the problem with and without recursion.

Example:
* Input: "Jasmine Ann Jones"
* Output: "Jasmine%20Ann%20Jones"
  
## With Recursion

const toUrl = function(string) {
  // termination case
  if(typeof(string) != 'string') {
    return;
  }

  // base case
  if (!string.includes(" ")){
    return string;
  }

  if (string[0] === " ")
  {
    return "%20" + toUrl(string.slice(1));
  }
  else 
  {
    return string[0] + toUrl(string.slice(1));
  }
}

## Without Recursion

const toUrl = function(string) {
  // handle exceptions:
  if(typeof(string) != 'string') {
    return;
  }
  if (!string.includes(" ")) {
    return string;
  }

  // we'll turn it into an array so we can iterate over each element:
  const stringArray = string.split("");

  // then, we'll use map to check for and replace each space in the array
  const urlArray = stringArray.map((char) => {
    return (char === " ") ? "%20" : char;
  });

  // we'll turn our new array back into a string
  const urlString = urlArray.join("");

  return urlString;
}

// THIS IS ASSUMING ANY NUMBER OF SPACES IS ACCEPTABLE
// if we want to ignore whitespace in the beginning and end, we'll use trim
// if we want to ignore multiple spaces between words, we can use trim after 
// replacing each space

###
# Question #2: Array Deduping
###

Write an algorithm that removes duplicates from an array. 
Do not use a function like filter() to solve this. 
Once you have solved the problem, demonstrate how it can be solved with filter(). 
Solve the problem with and without recursion.

Example:
* Input: [7, 9, "hi", 12, "hi", 7, 53]
* Output: [7, 9, "hi", 12, 53]

## With Recursion
const dedupe = function(array) {
  if (!array[0]) { // if the array is empty, return an empty array
    return [];
  }

  // we'll grab the first element
  const firstElement = array[0];

  // then separate it from the rest:
  const remainingElements = array.slice(1);

  // and we'll want to go through EACH element of the array
  // we know we're working with LIFO, so, we should probably 
  // start recursion here to start from an array of 1 and check elements
  // from the back to the front:

  const shrinkArray = dedupe(remainingElements);

  // so we'll recursively get to the end of the array,
  // where it will find an empty array and stop the recursion

  // when the recursion stops, we'll check to see if 53 is included in an empty array
  // because we know it's not, we'll return 53 and the empty array
  // and will then check, is 7 included in [53], no? return both and move on
  
  if (shrinkArray.includes(firstElement)) {
    return shrinkArray; // if that first Element IS in the array, just return the array as is
  }
  else {
    return [firstElement, ...shrinkArray]; // if it's NOT in the array, return them both
  }
}

## Without Recursion

const dedupe = function(array) {
  // handle exceptions:
  if (array.length <= 1) {
    return array;
  }

  // instantiate empty array
  let dedupArray = [];

  // loop through each element and push to empty array
  // but we check to make sure we haven't already pushed into the array
  // if we have, we ignore the element and move on
  array.forEach((element) => {
    if (!dedupArray.includes(element)) {
      dedupArray.push(element);
    }
  });

  return dedupArray;
}

# With Filter

const dedupe = function(array) {
  if (!array[0] || typeof(array) === 'string'){
    return array;
  }

  // indexOf returns the index of the FIRST APPEARANCE of that element,
  // so let's return only the elements whose index is the SAME as the 
  // index where it FIRST APPEARS, and ignore any element who's index
  // is NOT the same as the index of it's first appearance:

  const cleanArray = array.filter((element, index, list) => list.indexOf(element) === index); 

  return cleanArray;
}

###
# Question #3: Compressing Strings
###

Write an algorithm that takes a string with repeated characters and compresses them, using a number to show how many times the repeated character has been compressed. 
For instance, aaa would be written as 3a. 
Solve the problem with and without recursion.

Example:
* Input: "aaabccdddda"
* Output: "3ab2c4da"

# With Recursion
const compress = function(string) {
  if (string.length === 0 || typeof(string) != 'string') {
    return string;
  }

  let firstElement = string[0];
  let rest = compress(string.slice(1));

  if (!isNaN(Number(rest[0]))){
    if (firstElement === rest[1]){
      rest[0] = (Number(rest[0]) + 1);
      return rest.replace(rest[0], (Number(rest[0]) +1));
    } else {
      return firstElement + rest;
    }
  } else {
    if (firstElement === rest[0]) {
      return (2 + rest);
    } else {
      return firstElement + rest;
    }
  }

}

# Without Recursion

const compress = function(string) {
  if (typeof(string) != 'string' || string.length === 0) {
    return string;
  }

  let count = 1;
  let compressedString = '';

  for(let i=0; i < string.length; i++){
    // if current character repeats again, increase count
    if (string[i] === string[i+1]){
      count++;
    
    // if the current character does not repeat again, check the count
    } else {
      // if the count has not gone up, just concat the current character without the count
      if (count === 1){
        compressedString += string[i];

        // otherwise, if the count HAS gone up, concat the current character WITH the count
      } else {
        compressedString += count + string[i];
      }

      // then make sure to reset the count:
      count = 1;
    }

  }

  return compressedString;

}

###
# Question #4: Checking for Uniqueness
###

Write an algorithm that determines whether all the elements in a string are unique. 
You may not convert the string into an array or use array methods to solve this problem. 
The algorithm should return a boolean.

Example:
* Input: "hello"
* Output: false
* Input: "copyright"
* Output: true

# Answer

const unique = function(string) {
  if (string === '') {
    return true;
  }
  if(typeof(string) != 'string') {
    return string;
  }

  // we'll just brute force this, and loop twice:
  // for each character in the string, we'll loop through the string again,
  // and if that character is the same as any of the characters after it,
  // we'll end everything and return false,
  // otherwise continue to loop:
  // we know the next character didn't match that first one, so we'll check if 
  // THIS character matches any of the OTHER characters:

  for (let i = 0; i < string.length; i++) {
    for (let j = i + 1; i < string.length; j++) {
      if (string.charAt(i) === string.charAt(j)){
        return false;
      }
    }
  }

  return true;
}

###
# Question #5: Array Sorting
###

Write an algorithm that sorts an array without using the sort() method. 
There are many different sorting algorithms — take the time to read about the following:

Quick sort
Merge sort
Heap sort
Insertion sort
Bubble sort
Selection sort

You may implement any of the above algorithms (or your own) to solve the problem — 
as long as it doesn't use sort().

Example:
* Input: [9, 2, 7, 12]
* Output: [2, 7, 9, 12]

# Answer with QuickSort

const quickSort = function(array) {
  
  // base case: an array of 1 is already sorted!

  if (array.length <= 1) {
    return array;
  }

  // We'll partition the array into two parts:
  // numbers less than the chosen pivot point,
  // and numbers greater than the chosen pivot point:

  const pivot = array[0];
  let smaller = [];
  let greater = [];

  // using the first element in the array as our pivot,
  // we check each element in the array:
  // is it greater than or less than the pivot?
  // push that element into the appropriate array

  for(let i = 1; i < array.length; i++){
    if (array[i] < pivot) {
      smaller.push(array[i]);
    } else {
      greater.push(array[i]);
    }
  }

  // once out of the loop, we want to recursively go through our lists AGAIN,
  // until we no longer have any elements to loop through
  
  // we'll recursively loop through the smaller and greater arrays
  // until they hit the base case, at which point we'll complete 
  // each loop of our quickSort method and return a sorted array, which 
  // gets added to the array of the next quickSort up, and so on:
  // each time we return a larger array, each one sorted

  return [...quickSort(smaller), pivot, ...quickSort(greater)];
}