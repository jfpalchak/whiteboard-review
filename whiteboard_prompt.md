# Prompt:
Turn string to URL

URL cannot have spaces.
Instead all spaces in a string are replaced with: %20

replace all spaces in a string with %20

may NOT use replace() OR regular expressions

With and without recursion.

example:
Input: "Jasmine Ann Jones"
Output: "Jasmine%20Ann%20Jones"

Input: " Extra Space"
Output: "%20Extra%20Space"


# Planning:

# Non-recursively:
const toUrl = function(string) {

  if doesnt string has any spaces
    return string
  
  turn our string into array

  use map to transform in our array into %20

  turn our array back into a string

  return string
}

# Recursively:

given a string

check it is a string,
base case: if the given string has no spaces, return string

check if a character is a space, if it is replace with %20 and move on
if it isn't a space, keep character, move on 

return:


# Execution

# Non-Recursive Function
const toUrl = function(string) {
  if (typeof(string) != 'string') {
    return;
  }
  if (!string.includes(" ")) {
    return string;
  }

  const stringArray = string.split("");

  const mappedArray = stringArray.map(function(char) => {
    return (char === " ") ? "%20" : char;
  });

  const stringResult = mappedArray.join("");

  return stringResult;
}

# Recursive Function:

# Input: " Extra Space"

const toUrl = function(string) {
  if(typeof(string) != 'string'){
    return;
  }
  if(!string.includes(" ")) {
    return string;
  }

  
  if (string[0] === " "){
    return "%20" + toUrl(string.substring(1));
  } else {
    return string[0] + toUrl(string.substring(1))
  }
}