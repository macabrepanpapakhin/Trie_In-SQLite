### Trie but in SQLite

This is trie but instead of character connecting by object, it is connected by id of rows.

### Intention

To add words and it's definition.

It's fast when you want to search or find  words.

### How

To store related character, I give each new character a unique ID.

Character are connected via ID.

## Disavantages

1. Inserting is slow since it's have to move across the table to find related ID.

>> Recommend using pre-populated database when you have to add a lot of data such as dictionary.

2. Comsume a row for every non-unique character.

## Advantages

1. You can add word dynamically without have to worry about which row to put.

2. Search is fast since it's costs Only O(52*k), where k is the word you want to find.

<img width="1440" alt="Table Output" src="https://user-images.githubusercontent.com/54890279/196045360-237be792-80ce-482d-b894-99671fc1d637.png">

