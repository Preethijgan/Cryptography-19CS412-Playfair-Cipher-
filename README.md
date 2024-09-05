# PlayFair Cipher
Playfair Cipher using with different key values

# AIM:

To implement a program to encrypt a plain text and decrypt a cipher text using play fair Cipher substitution technique.

 
## DESIGN STEPS:

### Step 1:

Design of PlayFair Cipher algorithm 

### Step 2:

Implementation using C or pyhton code

### Step 3:

Testing algorithm with different key values. 

ALGORITHM DESCRIPTION:
The Playfair cipher uses a 5 by 5 table containing a key word or phrase. To generate the key table, first fill the spaces in the table with the letters of the keyword, then fill the remaining spaces with the rest of the letters of the alphabet in order (usually omitting "Q" to reduce the alphabet to fit; other versions put both "I" and "J" in the same space). The key can be written in the top rows of the table, from left to right, or in some other pattern, such as a spiral beginning in the upper-left-hand corner and ending in the centre.
The keyword together with the conventions for filling in the 5 by 5 table constitutes the cipher key. To encrypt a message, one would break the message into digrams (groups of 2 letters) such that, for example, "HelloWorld" becomes "HE LL OW OR LD", and map them out on the key table. Then apply the following 4 rules, to each pair of letters in the plaintext:
1.	If both letters are the same (or only one letter is left), add an "X" after the first letter. Encrypt the new pair and continue. Some   
   variants of Playfair use "Q" instead of "X", but any letter, itself uncommon as a repeated pair, will do.
2.	If the letters appear on the same row of your table, replace them with the letters to their immediate right respectively (wrapping 
   around to the left side of the row if a letter in the original pair was on the right side of the row).
3.	If the letters appear on the same column of your table, replace them with the letters immediately below respectively (wrapping around 
   to the top side of the column if a letter in the original pair was on the bottom side of the column).
4.	If the letters are not on the same row or column, replace them with the letters on the same row respectively but at the other pair of 
   corners of the rectangle defined by the original pair. The order is important – the first letter of the encrypted pair is the one that 
    lies on the same row as the first letter of the plaintext pair.
To decrypt, use the INVERSE (opposite) of the last 3 rules, and the 1st as-is (dropping any extra "X"s, or "Q"s that do not make sense in the final message when finished).


## PROGRAM:
```C
#include <stdio.h>
#include <string.h>
#include <ctype.h>

#define SIZE 5

void prepareKeyTable(char key[], char keyTable[SIZE][SIZE]) {
    int i, j, k, flag = 0, *dicty;
    dicty = (int*)calloc(26, sizeof(int));

    for (i = 0; i < SIZE; i++) {
        for (j = 0; j < SIZE; j++) {
            keyTable[i][j] = '\0';
        }
    }

    for (i = 0; i < strlen(key); i++) {
        if (key[i] != 'j') {
            if (isalpha(key[i])) {
                if (dicty[tolower(key[i]) - 'a'] == 0) {
                    keyTable[flag / SIZE][flag % SIZE] = tolower(key[i]);
                    dicty[tolower(key[i]) - 'a'] = 1;
                    flag++;
                }
            }
        }
    }

    for (i = 0; i < 26; i++) {
        if (dicty[i] == 0 && i != ('j' - 'a')) {
            keyTable[flag / SIZE][flag % SIZE] = i + 'a';
            flag++;
        }
    }
}

void search(char keyTable[SIZE][SIZE], char a, char b, int pos[]) {
    int i, j;

    for (i = 0; i < SIZE; i++) {
        for (j = 0; j < SIZE; j++) {
            if (keyTable[i][j] == a) {
                pos[0] = i;
                pos[1] = j;
            } else if (keyTable[i][j] == b) {
                pos[2] = i;
                pos[3] = j;
            }
        }
    }
}

void encrypt(char str[], char keyTable[SIZE][SIZE]) {
    int i, pos[4];

    for (i = 0; i < strlen(str); i += 2) {
        search(keyTable, str[i], str[i + 1], pos);

        if (pos[0] == pos[2]) {
            str[i] = keyTable[pos[0]][(pos[1] + 1) % SIZE];
            str[i + 1] = keyTable[pos[2]][(pos[3] + 1) % SIZE];
        } else if (pos[1] == pos[3]) {
            str[i] = keyTable[(pos[0] + 1) % SIZE][pos[1]];
            str[i + 1] = keyTable[(pos[2] + 1) % SIZE][pos[3]];
        } else {
            str[i] = keyTable[pos[0]][pos[3]];
            str[i + 1] = keyTable[pos[2]][pos[1]];
        }
    }
}

void prepare(char str[], char ptrs[]) {
    int i, j;

    j = 0;
    for (i = 0; i < strlen(str); i++) {
        if (str[i] == 'j') {
            str[i] = 'i';
        }
        if (str[i] != ' ') {
            ptrs[j++] = tolower(str[i]);
        }
    }
    ptrs[j] = '\0';

    if (strlen(ptrs) % 2 != 0) {
        ptrs[j++] = 'x';
        ptrs[j] = '\0';
    }
}

int main() {
    char key[SIZE * SIZE], str[100], keyTable[SIZE][SIZE], strPrepared[100];

    printf("Enter key: ");
    scanf("%s", key);

    prepareKeyTable(key, keyTable);

    printf("Enter message: ");
    scanf(" %[^\n]", str);

    prepare(str, strPrepared);

    encrypt(strPrepared, keyTable);

    printf("Encrypted message: %s\n", strPrepared);

    return 0;
}

```
## OUTPUT:

![Screenshot 2024-08-31 215006](https://github.com/user-attachments/assets/790a5be2-d202-4123-9d16-d81a08f9e457)

## RESULT:
The program is executed successfully
