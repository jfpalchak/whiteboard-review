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