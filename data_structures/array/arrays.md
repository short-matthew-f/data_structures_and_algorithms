# What is an array?

An array is a data structure which stores data in a linear way.  Arrays have two things going on: the data they hold and the index for each piece of data.  Generally arrays are used for storing similar data, for example an array of integers, an array of `User`s, etc.

Below is a list of people I know, and their position on the list.

Count | Name
------|------
1     | Mark Campbell
2     | Stephen Markov
3     | Jane Warellen
4     | Candace Freshdinger

Since this data is just a list, if we wish to represent it within the context of a program it makes sense to do so in an array, like so:

```ruby
# my_friends.rb

my_friends = [
  "Mark Campbell",
  "Stephen Markov",
  "Jane Warellen",
  "Candace Freshdinger"
]

```

Generally we access specific elements in an array by bracketing into the array with a specific index.  Furthermore, arrays are usually indexed starting with the number 0.  So, for example, `my_friends[0]` will return `Mark Campbell`, and `my_friends[3]` will return `Candace Freshdinger`.

## Common tasks

In addition to accessing already stored data, we often want to use our structure to add or remove data, iterate over existing data, search for data, sort the data, etc.  Each data structure that we will use will have distinct advantages and disadvantages for completing each of these tasks.

### Adding data:

For arrays, there are three places you might want to insert new data: at the beginning, in the middle, or at the end.  Adding to the beginning or in the middle are identical processes.  The end is easiest, so let's start there.

1. Adding to the end, **aka** `push`  

   Adding to the end of an array is simply associating a new piece of data to the next largest index.  If, above, I wish to add "Lara Bloom" to the end of my list of friends, I will simply need to look at which index is the smallest unused one, and assign to it her name.  

   ```ruby
   my_friends[4] = "Lara Bloom"

   my_friends == [
     "Mark Campbell",
     "Stephen Markov",
     "Jane Warellen",
     "Candace Freshdinger",
     "Lara Bloom"
   ]
   ```

   Another name for adding to the end of an array is `push`, and in ruby we additionally have the shovel operator (`<<`) which adds the input to the end of the array.  So, for example, `my_friends.push("Lara Bloom")` and `my_friends << "Lara Bloom"` would have done the same thing as the above code.  

   What's great about that is that it happens so very quickly.  Accessing and assigning data to a specific index in an array is fast, it happens in constant time.

2. Adding to anywhere else, **aka** `insert`, `splice`, `unshift`, and more!  

   If you want to add an item anywhere but the end, you will be assigning to an existing index a new piece of data.  This would be fine if you are planning to overwrite that data.  If you want to keep it, however, you will have to bump up the index of every other piece of data after it, one at a time.  

   Convenience methods will do this for you, but let's write an `insert(value, index)` method to understand what it's doing.  Here we'll add the method to the array type in javascript:

   ```js
   // array_insert.js

   Array.prototype.insert = function(newValue, index) {
     if (index > this.length) {
       return false;
     } else {
       var cachedValue;
       var initialLength = this.length;

       for (var i = index; i <= initialLength; i++) {
         cachedValue = this[i];
         this[i]     = newValue;
         newValue    = cachedValue;
       };
     };
   };

   var a = [0, 1, 2];
   a.insert(12, 3); // a == [0, 1, 2, 12];
   a.insert("purple", 3); // a == [0, 1, 2, "purple", 12];
   ```

   So for each pass through our for loop we grab the thing we're replacing and cache it, reassign the current index to it's new value, and then set the next new value to the one we cached.  This means each value will move "one space to the right" starting at the index we're inserting at.  

   How bad is this?  Not very, the worst case scenario is inserting a new value at the beginning of an array (which Ruby lets you do with `unshift`).  This will require the for loop to execute once for every value in the array at start, and thus algorithm time is a linear function of the initial input (the array).  

3. Deleting  

   Deleting is similar to inserting, in that if you want to remove from the end (for example, `pop`), it happens in constant time.  If you wish to delete from the start or middle and reassign indices to the values after the deleted one, it will be a linear time 
