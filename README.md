## EX. NO:2 IMPLEMENTATION OF PLAYFAIR CIPHER

## NAME : DAPPILI VASAVI
## REGISTER NUMBER : 212223040030

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




## Program:
```
#include <stdio.h>
#include <string.h>
#include <ctype.h>

#define SIZE 5

char matrix[SIZE][SIZE];

void prepareKeyMatrix(char key[]) {
    int used[26] = {0};
    used['j' - 'a'] = 1; 
    int idx = 0;

    for (int i = 0; i < strlen(key); i++) {
        char ch = tolower(key[i]);
        if (ch < 'a' || ch > 'z') continue;
        if (ch == 'j') ch = 'i';

        if (!used[ch - 'a']) {
            matrix[idx / SIZE][idx % SIZE] = ch;
            used[ch - 'a'] = 1;
            idx++;
        }
    }

    for (char ch = 'a'; ch <= 'z'; ch++) {
        if (!used[ch - 'a']) {
            matrix[idx / SIZE][idx % SIZE] = ch;
            used[ch - 'a'] = 1;
            idx++;
        }
    }
}


void displayMatrix() {
    printf("\nKey Matrix:\n");
    for (int i = 0; i < SIZE; i++) {
        for (int j = 0; j < SIZE; j++) {
            printf("%c ", matrix[i][j]);
        }
        printf("\n");
    }
}


void findPosition(char ch, int *row, int *col) {
    if (ch == 'j') ch = 'i';
    for (int i = 0; i < SIZE; i++) {
        for (int j = 0; j < SIZE; j++) {
            if (matrix[i][j] == ch) {
                *row = i;
                *col = j;
                return;
            }
        }
    }
}


void preparePlainText(char *input, char *output) {
    int len = 0;
    for (int i = 0; input[i]; i++) {
        if (isalpha(input[i])) {
            output[len++] = tolower(input[i]) == 'j' ? 'i' : tolower(input[i]);
        }
    }
    output[len] = '\0';

    char temp[100];
    int k = 0;

    for (int i = 0; i < len; i++) {
        temp[k++] = output[i];
        if (i + 1 < len) {
            if (output[i] == output[i + 1]) {
                temp[k++] = 'x';
            } else {
                temp[k++] = output[++i];
            }
        }
    }
    if (k % 2 != 0) {
        temp[k++] = 'x'; 
    }
    temp[k] = '\0';
    strcpy(output, temp);
}

void encrypt(char *plaintext, char *ciphertext) {
    int i = 0, idx = 0;
    while (plaintext[i] && plaintext[i + 1]) {
        char a = plaintext[i];
        char b = plaintext[i + 1];
        int row1, col1, row2, col2;
        findPosition(a, &row1, &col1);
        findPosition(b, &row2, &col2);

        if (row1 == row2) {
            ciphertext[idx++] = matrix[row1][(col1 + 1) % SIZE];
            ciphertext[idx++] = matrix[row2][(col2 + 1) % SIZE];
        } else if (col1 == col2) {
            ciphertext[idx++] = matrix[(row1 + 1) % SIZE][col1];
            ciphertext[idx++] = matrix[(row2 + 1) % SIZE][col2];
        } else {
            ciphertext[idx++] = matrix[row1][col2];
            ciphertext[idx++] = matrix[row2][col1];
        }
        i += 2;
    }
    ciphertext[idx] = '\0';
}

int main() {
    char key[100], plaintext[100], cleanText[100], ciphertext[100];

    printf("Enter the keyword: ");
    scanf("%s", key);

    printf("Enter the plaintext: ");
    scanf("%s", plaintext);

    prepareKeyMatrix(key);
    displayMatrix();

    preparePlainText(plaintext, cleanText);

    encrypt(cleanText, ciphertext);

    printf("\nEncrypted Cipher Text: %s\n", ciphertext);

    return 0;
}
```
## Output:
<img width="686" height="450" alt="image" src="https://github.com/user-attachments/assets/e2b3cbb3-84cc-417e-8ef1-0b800228b4ca" />

## Result:
Thus, the given C program to implement the Playfair Cipher encryption technique was successfully executed.
