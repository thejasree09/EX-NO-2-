## EX. NO:2 IMPLEMENTATION OF PLAYFAIR CIPHER

 

## AIM:
 

 

To write a C program to implement the Playfair Substitution technique.

## DESCRIPTION:

The Playfair cipher starts with creating a key table. The key table is a 5×5 grid of letters that will act as the key for encrypting your plaintext. Each of the 25 letters must be unique and one letter of the alphabet is omitted from the table (as there are 25 spots and 26 letters in the alphabet).

To encrypt a message, one would break the message into digrams (groups of 2 letters) such that, for example, "HelloWorld" becomes "HE LL OW OR LD", and map them out on the key table. The two letters of the diagram are considered as the opposite corners of a rectangle in the key table. Note the relative position of the corners of this rectangle. Then apply the following 4 rules, in order, to each pair of letters in the plaintext:
1.	If both letters are the same (or only one letter is left), add an "X" after the first letter
2.	If the letters appear on the same row of your table, replace them with the letters to their immediate right respectively
3.	If the letters appear on the same column of your table, replace them with the letters immediately below respectively
4.	If the letters are not on the same row or column, replace them with the letters on the same row respectively but at the other pair of corners of the rectangle defined by the original pair.
## EXAMPLE:
![image](https://github.com/Hemamanigandan/EX-NO-2-/assets/149653568/e6858d4f-b122-42ba-acdb-db18ec2e9675)

 

## ALGORITHM:

STEP-1: Read the plain text from the user.
STEP-2: Read the keyword from the user.
STEP-3: Arrange the keyword without duplicates in a 5*5 matrix in the row order and fill the remaining cells with missed out letters in alphabetical order. Note that ‘i’ and ‘j’ takes the same cell.
STEP-4: Group the plain text in pairs and match the corresponding corner letters by forming a rectangular grid.
STEP-5: Display the obtained cipher text.




Program:
```
#include <stdio.h>
#include <string.h>
#include <ctype.h>

char keyTable[5][5];

void generateKeyTable(char key[]) {
    int used[26] = {0};
    int i, j, k = 0;

    for (i = 0; key[i] != '\0'; i++) {
        char ch = tolower(key[i]);
        if (ch == 'j') ch = 'i';

        if (ch >= 'a' && ch <= 'z' && !used[ch - 'a']) {
            keyTable[k / 5][k % 5] = ch;
            used[ch - 'a'] = 1;
            k++;
        }
    }

    for (i = 0; i < 26; i++) {
        if (i + 'a' == 'j') continue;
        if (!used[i]) {
            keyTable[k / 5][k % 5] = i + 'a';
            k++;
        }
    }
}


void findPosition(char ch, int *row, int *col) {
    int i, j;
    if (ch == 'j') ch = 'i';

    for (i = 0; i < 5; i++) {
        for (j = 0; j < 5; j++) {
            if (keyTable[i][j] == ch) {
                *row = i;
                *col = j;
                return;
            }
        }
    }
}


void encrypt(char text[]) {
    int i, r1, c1, r2, c2;
    char a, b;

    for (i = 0; i < strlen(text); i += 2) {
        a = text[i];
        b = text[i + 1];

        findPosition(a, &r1, &c1);
        findPosition(b, &r2, &c2);

        if (r1 == r2) {
            printf("%c%c", keyTable[r1][(c1 + 1) % 5],
                           keyTable[r2][(c2 + 1) % 5]);
        } 
        else if (c1 == c2) {
            printf("%c%c", keyTable[(r1 + 1) % 5][c1],
                           keyTable[(r2 + 1) % 5][c2]);
        } 
        else {
            printf("%c%c", keyTable[r1][c2],
                           keyTable[r2][c1]);
        }
    }
}

int main() {
    char key[50], text[100];
    int i, j = 0;

    printf("Enter the keyword: ");
    scanf("%s", key);

    printf("Enter the plaintext: ");
    scanf("%s", text);

    
    for (i = 0; i < strlen(text); i++) {
        text[i] = tolower(text[i]);
        if (text[i] == 'j') text[i] = 'i';
    }

    char processed[100];
    for (i = 0; i < strlen(text); i++) {
        processed[j++] = text[i];
        if (text[i] == text[i + 1]) {
            processed[j++] = 'x';
        }
    }

    if (j % 2 != 0) {
        processed[j++] = 'z';
    }
    processed[j] = '\0';

    generateKeyTable(key);

    printf("\nKey Matrix:\n");
    for (i = 0; i < 5; i++) {
        for (j = 0; j < 5; j++) {
            printf("%c ", keyTable[i][j]);
        }
        printf("\n");
    }

    printf("\nCipher Text: ");
    encrypt(processed);

    return 0;
}
```




Output:
<img width="517" height="418" alt="image" src="https://github.com/user-attachments/assets/81868db7-f4f2-4344-94b1-1b3be2a121a4" />

