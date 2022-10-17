### Trie in SQLite

This is trie but instead of character connecting by object, it is connected by id of rows.

### Intention

To add words and it's definition.

To Find words fast.

### How

To store related character, I give each new character a unique ID.

Character are connected via ID of the row.

### Disavantages over Lexically sorted words

1. Inserting is slow since it's have to move across the table to find related ID. But the sqlite provide O(logn) search.

>> Recommend using pre-populated database when you have to add a lot of data such as dictionary.

2. Comsume a row for every non-unique character. (Too much space)

3. Search is slower compare to sorted words, since you can perform binary search.
 

### Advantages over Lexically sorted words

1. You can add word dynamically without have to worry about which row to put. In Lexically, you have to sort after adding data.

 
 ### Conclusion
 
 Not recommend using it. Too much unnecesssary space. Slower than typical sorted data. You can later pull lexically sorted data from the structure.
 
 I develop it for testing "connecting character without the object structure".
 
 ### Contribuiton
 
 I have some bugs when I show recommend words, and need a method that pull lexically sorted data from the table.
 
 You can see how data have been after putting into the table.  &#8595;  &#8595;  &#8595;

<img width="1440" alt="Table Output" src="https://user-images.githubusercontent.com/54890279/196045360-237be792-80ce-482d-b894-99671fc1d637.png">

