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

# Question #2: Array Deduping
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
