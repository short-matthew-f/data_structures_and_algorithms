# Algorithms and Data Structures (a mini-course)

Large companies like Google or smaller companies that work with large amounts of data will be curious about your ability to solve problems.  They have already reached the edge of current optimization techniques, and need people that can do more than move a `div` three pixels to the right.  They're concerned with skills that often reach into the realm of Computer Science, and will have been introductory course work to candidates with relevant degrees.

During the interview process they will often bombard you with questions that push your own boundaries, trying to see how you behave when you are out of your own comfort zone.  They're also looking to see if you're conversant in the appropriate language, and know enough background to have correct or near-correct approaches to solving these problems.

## Start with an example

You've made a web-based game.  The game supplies the user with a constant stream of enemies.  In order to keep the game interesting, you want to ensure that new enemies are slightly stronger than the most recent batch.

You've been given the following code:

```js
var FightEm = function () {
  this.enemyStream = [];
};

FightEm.prototype.fightEnemy = function(enemy) {
  /* fighting happens here, and then... */
  if (enemy.isDead()) {
    this.updateStream(enemy);
  };
};

var Enemy = FightEm.Enemy = function () {};

Enemy.prototype.isWeakerThan = function (enemyTwo) {
  /* returns true if the enemy is weaker than enemyTwo */
};

Enemy.duplicateMostlyStronger = function(enemy) {
  /* returns a new enemy which is between 5% weaker and 25% stronger than the given enemy */
};
```

and have been asked to implement the function below:

```js
FightEm.prototype.weakestInEnemyStream = function () {
  /* should return the weakest enemy out of the last 10 killed by player */
};
```

How would you write `weakestInEnemyStream`?  How much memory and how much time do you need compared to the data you're given?  Is your code optimal, or could it be improved upon?

### A naïve solution

```js
FightEm.prototype.updateStream = function (enemy) {
  var enemies = this.enemyStream;

  if (enemies.length == 20) {
    enemies.shift();
  };

  this.enemyStream.push(enemy);
};

FightEm.prototype.weakestInEnemyStream = function (enemy) {
  var enemies = this.enemyStream;
  var weakestEnemy = enemies[0];

  for (var i = 1; i < enemies.length; i++) {
    if ( enemies[i].isWeakerThan(weakestEnemy) ) {
      weakestEnemy = enemies[i];
    };
  };

  return weakestEnemy;
}
```

### How'd we do?

There's a lot of ways to determine the success of your code.  Here we're concerned with the performance of `weakestEnemyInStream`.

1. Does it work?  
   Don't underestimate the value of this question.  Writing clean code which accomplishes a task, even inefficiently, means you've satisfied your main goal.  There's often much more to do after this point, but remember 'technically correct is still correct'.  
2. Does it require lots of time?  
   This question and the next question require a bit of understanding of complexity.  The big idea in complexity is that things should be compared to the amount of data available and a worst case scenario for your algorithm.  
   Here, we iterate over the entire list of enemies when finding the weakest one, so we use an amount of time proportionate to the input size.  This is called 'linear time', and it's usually considered pretty good.  We'll see a way to make it better, later.
3. Does it require lots of memory?
   So we don't worry about the stream of enemies we're storing, but we are going to compare the extra memory used as a function of the size of that data.  So when we look at the enemies list and keep track of the weakest enemy, we're only really storing **one** extra piece of data.  Even if our stream had thousands of enemies, we would still only keep that one extra piece of data.  Since this is a constant value, not dependant on input, we say the memory requirement for this algorithm is constant.

It's hard to tell, at first, how good this is.  It turns out we can do much better, but often there are tradeoffs to do so.  Sometimes to gain speed, we have to keep more things in memory, or vice-versa.  We'll see more examples as we go.

# What's next?

First we'll start off by [playing with arrays](/data_structures/array/arrays.md), and exploring many of the different things you'll want to use them for. 
