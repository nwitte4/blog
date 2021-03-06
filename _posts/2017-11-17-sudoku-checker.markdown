---
layout: post
title:  "Creating a Javascript Sudoku Checker"
date:   2017-11-14 17:25:31 -0500
---

Today, during lunch, I completed my first 4kyu codewars challenge and I was PUMPED. I was pumped for two reasons: 1) my ranking on codewars went from a 6kyu to a 5kyu *\*nerd alert\** and 2) I had been tasked with this challenge back in September when I was taking a night class on JS basics and I got stuck on validating the 3x3 subgrids.

Today though, I figured it out and now I'm going to share it with you because I'm pretty proud of myself. Go team.

First thing's first I stared at it. After staring, I thought, "Okay, well I guess I should start by checking the rows one by one" and so off I go:

<pre><code class="javascript">
  for(let i = 0; i < board.length; i++){
    var currentRow = board[i];
    // this grabs each row, one at a time

    let numChecker = [];
    // this is an empty array I will put each number in currentRow, one at a time

    for(let i = 0; i < currentRow.length; i++){
      let currentNum = currentRow[i];
      // I grab the current number of the current row and set it equal to a variable called currentNum, fitting

      if(numChecker.includes(currentNum)){
        // I check if numChecker (an array) contains the currentNum (an individual number)
        // If numChecker already contains that number, it means that the number is a repeat in that row
        // because numChecker is emptied after each iteration of the loop
        // So, I return false
        return false;
      } else {
        // If numChecker does not contain the current number,
        // I add the current number to it and move onto the next number
        numChecker.push(currentNum);
      }
    }
  }
</code></pre>

I was using repl.it and figured I should throw a valid sudoku board and a non-valid sudoku board in as tests to see if this was working. Good news, it works. When I passed in a valid board it returned undefined (because the only thing I've returned so far is false IF there are duplicate numbers). At this point I figured I would put a "return true" at the very bottom of the function so that if it runs through the loop without ever hitting the first return false; then it must be a valid board.

Next I went for the columns and I figured it would be similar to grabbing the rows where I grab nine numbers at a time and check for duplicates. So I start with the first column. I knew I needed to grab each row one by one so I needed a for loop for the board that would go from board[0] to board[8] but instead of grabbing the full row I only need the first number of each row so board[0][0] to board[0][9]. I create a counter called colCounter that I set to zero. I also needed to store these variables somewhere temporarily because once I extract the first column, I still need to check for duplicates so I create a variable called currentCol:

<pre><code class="javascript">
let colCounter = 0;
 for(let i = 0; i < board.length; i++){
   let currentCol = [];
</code></pre>

Then I create a for loop and I push the first number of each row (i.e. board[i][colCounter]) (which is board[i][0] at this point because I only need one column at a time) into my currentCol variable. After the loop runs, I will have an array of nine numbers representing the first column of the board.

<pre><code class="javascript">
for(let i = 0; i < board.length; i++){
  var temporaryRow = board[i][colCounter];
  currentCol.push(temporaryRow)
}
</code></pre>

At this point I realize I'm about to repeat myself with my previous numChecker logic to check the current array so it's about time to make a helper method called checkNums:


<pre><code class="javascript">
function checkNums(temporaryNumber, temporaryArray){
  if(!temporaryArray.includes(temporaryNumber)){
    temporaryArray.push(temporaryNumber);
    return true;
  } else {
    return false;
  }
}
</code></pre>

This function will take in two arguments, a number and an array, and proceed to check if the number passed to it exists in the array passed to it. If it does not include that number, the function returns true and if it does include that number the function returns false. The way it works is I place it inside of a for loop that checks each column one by one. It then takes an empty array called numChecker, and the current number the loop is on. I then use an if statement that checks if checkNums returns a falsy or truthy value. If it returns a falsey value it means that the number I passed into checkNums is already included in the array I passed into checkNums and the board is not valid. If it returns a truthy value it means I can move onto the next number. With each iteration, the array gets larger but there is still only one number passed at a time. Cool? Cool.

<pre><code class="javascript">
  let numChecker = [];
      for(let i = 0; i < currentCol.length; i++){
        let currentNum = currentCol[i];
        if(!checkNums(currentNum, numChecker)){
          return false;
        }
      }
      colCounter ++
</code></pre>

To be clear, this for loop (and the previous for loop) are both running inside of the *original* for loop I created. So basically there is one large for loop that runs 9 times and inside *that* for loop there are two more separate for loops. *For those interested that means it's an O(n2+n2) = O(2n2) = O(n2)* Anyways, the reason for this is because first, I need to grab all the 0th indexes of 9 different rows which means I need to run through board[0][0]-board[8][0] before even moving onto the next column which runs through board[0][1]-board[8][1] and then to the *next* column board[0][2]-board[8][2] and so on and so forth. So what haven't I addressed? That's right, the part where I increment the second indexes, SO, I need my function to include a colCounter++ at the very end of the loop before it ends so when it runs a second time, it will move onto the next column.

So after this, I commented out my row validation to make sure that when I passed in a invalid board that it was returning false for the right reasons. So I checked this and changed a few different numbers in different columns and it worked again. Yippie. Onto the subgrids.

So the subgrids originally had me sweating. After staring down my computer for a few minutes I said to myself, "just start small, what do you need first?" Well first I need the first grid, so:

<pre><code class="javascript">
  var tempRow = [];
  // empty array
    for(let i = 0; i < 3; i++){
      var getRow = board[i];
      // this loop runs 3 times
      // it grabs the first three rows
      for(let i = 0; i < 3; i++){
        tempRow.push((getRow[i]));
        // for every row, this adds the first three numbers
      }
    }
</code></pre>

So tempRow now contains an array of 9 numbers representing the first 3x3 grid in the sudoku grid. Next I just need to throw that into the numChecker function.

<pre><code class="javascript">
  var tempRow = [];
  for(let i = 0; i < 3; i++){
    var getRow = board[i];
    for(let i = 0; i < 3; i++){
      tempRow.push((getRow[i]));
    }
  }

  let numChecker = [];
  for(let i = 0; i < tempRow.length; i++){
    let currentNum = tempRow[i];
    if(!checkNums(currentNum, numChecker)){
      return false;
    }
  }
</code></pre>

Works like a charm, now I just needed to figure out how to grab the next 8 boxes and I'll be smooth sailing. To start, I throw the entire thing in a bigger for loop that runs 9 times. This is a reasonable start because now I am getting a 9x9 sudoku looking board, but all the rows are the same. However, this makes sense because I'm only grabbing these pieces:

<pre><code class="javascript">
board[0][0]board[0][1]board[0][2]
board[1][0]board[1][1]board[1][2]
board[2][0]board[2][1]board[2][2]
</code></pre>

So what to change? Well, I have to change the first index it grabs and the second index it grabs to repeat 8 more times in the proper order. First I did this:

<pre><code class="javascript">
var secondIndex = 0;
var secondIndexMax = 3;
// these will go into the for loops later

for(let i = 0; i < 3; i++){
  var tempRow = [];
  // initializes an empty array to make a temporary 'row' to check
  // the numbers inside it will still be representing a 3x3 grid

  for(let i = 0; i < 3; i++){
    var getRow = board[i];
    // on the first iteration: board[0]
    // on the second iteration: board[1]
    // on the third iteration: board[2]

    for(let i = secondIndex; i < secondIndexMax; i++){
      tempRow.push((getRow[i]));
      // on the first iteration: getRow[0]
      // on the second iteration: getRow[1]
      // on the third iteration: getRow[2]
    }
    // important to remember that this inner loop runs 3 times for every iteration of the outer loop
    // meaning that when the outer loop runs once with board[0]
    // the inner loop runs three times with board[0]
    // on the first iteration: board[0][0]
    // on the second iteration: board[0][1]
    // on the third iteration: board[0][2]
  }
  // so when this outer loop runs 3 times you get:
  // on the first iteration: board[0][0] board[0][1]board[0][2]
  // on the second iteration: board[1][0]board[1][2]board[1][2]
  // on the third iteration: board[2][0] board[2][1]board[2][2]
  // this comprises our temporary row


  // this is our usual check number situation
  let numChecker = [];
  for(let i = 0; i < tempRow.length; i++){
    let currentNum = tempRow[i];
    if(!checkNums(currentNum, numChecker)){
      return false;
    }
  }

  // however! this is all running inside a third loop that runs 3 times
  // after each iteration, we add 3 to the secondIndex, and the secondIndexMax

  secondIndex+=3;
  secondIndexMax+=3;
}
// so second iteration we get:
//  board[0][3] board[0][4]board[0][5]
//  board[1][3]board[1][4]board[1][5]
//  board[2][3] board[2][4]board[2][5]
// and third iteration we get:
//  board[0][6] board[0][7]board[0][8]
//  board[1][6]board[1][7]board[1][8]
//  board[2][6] board[2][7]board[2][8]
</code></pre>

I swear, I'm almost done. By this point in time. I've grabbed the top three 3x3 grids. All we need to do, is get this baby running for the middle 3x3 grids and the bottom. So what do we do? Another for loop! This time though, we create two variables called firstIndex and firstIndexMax. We follow the same pattern as we did for the second index. You can see the final product below.

The Finale:

<pre><code class="javascript">
function validSolution(board){

// Validates Rows

  for(let i = 0; i < board.length; i++){
    var currentRow = board[i];
    let numChecker = [];

    for(let i = 0; i < currentRow.length; i++){
      let currentNum = currentRow[i];
      if(!checkNums(currentNum, numChecker)){
        return false;
      }
    }
  }

// Validates Columns

  let colCounter = 0;
  for(let i = 0; i < board.length; i++){
    var currentCol = [];

    for(let i = 0; i < board.length; i++){
      var temporaryRow = board[i][colCounter];
      currentCol.push(temporaryRow)

    let numChecker = [];
      for(let i = 0; i < currentCol.length; i++){
        let currentNum = currentCol[i];
        if(!checkNums(currentNum, numChecker)){
          return false;
        }
      }
    }
    colCounter ++
  }

  // Validates Subgrids

  let firstIndex = 0;
  let firstIndexMax = 3;

  for(let i = 0; i < 3; i++){
    var secondIndex = 0;
    var secondIndexMax = 3;

    for(let i = 0; i < 3; i++){
      var tempRow = [];
      for(let i = firstIndex; i < firstIndexMax; i++){
        var getRow = board[i];
        for(let i = secondIndex; i < secondIndexMax; i++){
          tempRow.push((getRow[i]));
        }
      }

      let numChecker = [];
      for(let i = 0; i < tempRow.length; i++){
        let currentNum = tempRow[i];
        if(!checkNums(currentNum, numChecker)){
          return false;
        }
      }
      secondIndex+=3;
      secondIndexMax+=3;
    }
    firstIndex+=3;
    firstIndexMax+=3;
  }

  function checkNums(temporaryNumber, temporaryArray){
    if(!temporaryArray.includes(temporaryNumber)){
      temporaryArray.push(temporaryNumber);
      return true;
    } else {
      return false;
    }
  }

 return true;
}
</code></pre>

My next logical move would be figuring out how to *create* a functional sudoku game. If I could do that, I would probably use javascript to remove/hide a certain amount of numbers and allow a user to insert their own numbers.

At the end there would be an option to "check board", where this code would run, and if they won the function would return true and I'd probably have something fun happen and if they lost it would return false and I would probably have something else fun happen that still informed them they lost.

One step further would be implementing something into numChecker that returned the part of the board that was breaking, although.. some sudoku games I've played don't even go that far so I'd be going above and beyond, classic. Anyways, this was fun let's do it again sometime. I can't believe you read this far.
