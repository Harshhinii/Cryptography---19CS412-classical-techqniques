# Cryptography---19CS412-classical-techqniques
# Caeser Cipher
Caeser Cipher using with different key values

# AIM:

To encrypt and decrypt the given message by using Ceaser Cipher encryption algorithm.

## DESIGN STEPS:

### Step 1:

Design of Caeser Cipher algorithnm 

### Step 2:

Implementation using C or pyhton code

### Step 3:

1.	In Ceaser Cipher each letter in the plaintext is replaced by a letter some fixed number of positions down the alphabet.
2.	For example, with a left shift of 3, D would be replaced by A, E would become B, and so on.
3.	The encryption can also be represented using modular arithmetic by first transforming the letters into numbers, according to the   
    scheme, A = 0, B = 1, Z = 25.
4.	Encryption of a letter x by a shift n can be described mathematically as,
                       En(x) = (x + n) mod26
5.	Decryption is performed similarly,
                       Dn (x)=(x - n) mod26

## PROGRAM:
PROGRAM:
#include <stdio.h>
#include <ctype.h>
#include <string.h>

// Function to encrypt the plaintext using Caesar Cipher
void encrypt(char *plaintext, int shift, char *ciphertext) {
    int i;
    for (i = 0; plaintext[i] != '\0'; i++) {
        char ch = plaintext[i];
        if (isalpha(ch)) {
            char offset = isupper(ch) ? 'A' : 'a';
            ciphertext[i] = (ch - offset + shift) % 26 + offset;
        } else {
            ciphertext[i] = ch; // Non-alphabet characters are not changed
        }
    }
    ciphertext[i] = '\0'; // Null-terminate the encrypted string
}

// Function to decrypt the ciphertext using Caesar Cipher
void decrypt(char *ciphertext, int shift, char *plaintext) {
    int i;
    for (i = 0; ciphertext[i] != '\0'; i++) {
        char ch = ciphertext[i];
        if (isalpha(ch)) {
            char offset = isupper(ch) ? 'A' : 'a';
            plaintext[i] = (ch - offset - shift + 26) % 26 + offset; // Add 26 to handle negative shift
        } else {
            plaintext[i] = ch; // Non-alphabet characters are not changed
        }
    }
    plaintext[i] = '\0'; // Null-terminate the decrypted string
}

int main() {
    char plaintext[256], ciphertext[256], decryptedtext[256];
    int shift;
    
    printf("Enter the plaintext: ");
    fgets(plaintext, sizeof(plaintext), stdin);
    
    // Remove newline character if present
    plaintext[strcspn(plaintext, "\n")] = '\0';
    
    printf("Enter the shift value: ");
    scanf("%d", &shift);
    
    // Ensure the shift value is between 0 and 25
    shift = (shift % 26 + 26) % 26;
    
    // Encrypt the plaintext
    encrypt(plaintext, shift, ciphertext);
    printf("Encrypted text: %s\n", ciphertext);
    
    // Decrypt the ciphertext
    decrypt(ciphertext, shift, decryptedtext);
    printf("Decrypted text: %s\n", decryptedtext);
    
    return 0;
}
## OUTPUT:
![image](https://github.com/user-attachments/assets/afc6e439-4405-48d1-add1-e424563b2db9)



## RESULT:
The program is executed successfully

---------------------------------

# PlayFair Cipher
Playfair Cipher using with different key values

# AIM:

To implement a program to encrypt a plain text and decrypt a cipher text using play fair Cipher substitution technique.

 
## DESIGN STEPS:

### Step 1:

Design of PlayFair Cipher algorithnm 

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
#include <stdio.h>
#include <string.h>
#include <ctype.h>

// Function to create the Playfair cipher key matrix
void createMatrix(char *key, char matrix[5][5]) {
    int used[26] = {0};
    int k = 0;
    
    for (int i = 0; key[i]; i++) {
        if (isalpha(key[i])) {
            char ch = toupper(key[i]);
            if (ch == 'J') ch = 'I'; // Treat 'J' as 'I'
            if (!used[ch - 'A']) {
                matrix[k / 5][k % 5] = ch;
                used[ch - 'A'] = 1;
                k++;
            }
        }
    }
    
    for (char ch = 'A'; ch <= 'Z'; ch++) {
        if (ch == 'J') continue; // 'J' is treated as 'I'
        if (!used[ch - 'A']) {
            matrix[k / 5][k % 5] = ch;
            k++;
        }
    }
}

// Function to find the position of a character in the matrix
void findPosition(char ch, char matrix[5][5], int *row, int *col) {
    if (ch == 'J') ch = 'I'; // Treat 'J' as 'I'
    for (int r = 0; r < 5; r++) {
        for (int c = 0; c < 5; c++) {
            if (matrix[r][c] == ch) {
                *row = r;
                *col = c;
                return;
            }
        }
    }
}

// Function to prepare the plaintext (remove spaces and replace 'J' with 'I')
void prepareText(char *text, char *preparedText) {
    int k = 0;
    for (int i = 0; text[i]; i++) {
        if (isalpha(text[i])) {
            char ch = toupper(text[i]);
            if (ch == 'J') ch = 'I'; // Treat 'J' as 'I'
            if (k > 0 && preparedText[k-1] == ch) {
                preparedText[k++] = 'X'; // Insert 'X' between duplicate letters
            }
            preparedText[k++] = ch;
        }
    }
    preparedText[k] = '\0';
    if (k % 2 != 0) {
        preparedText[k++] = 'X'; // Append 'X' if the length is odd
    }
    preparedText[k] = '\0';
}

// Function to encrypt plaintext using the Playfair cipher
void encrypt(char *plaintext, char *key, char *ciphertext) {
    char matrix[5][5];
    createMatrix(key, matrix);
    
    char preparedText[256];
    prepareText(plaintext, preparedText);
    
    int i = 0;
    int k = 0;
    while (preparedText[i]) {
        int row1, col1, row2, col2;
        findPosition(preparedText[i], matrix, &row1, &col1);
        findPosition(preparedText[i+1], matrix, &row2, &col2);
        
        if (row1 == row2) {
            ciphertext[k++] = matrix[row1][(col1 + 1) % 5];
            ciphertext[k++] = matrix[row2][(col2 + 1) % 5];
        } else if (col1 == col2) {
            ciphertext[k++] = matrix[(row1 + 1) % 5][col1];
            ciphertext[k++] = matrix[(row2 + 1) % 5][col2];
        } else {
            ciphertext[k++] = matrix[row1][col2];
            ciphertext[k++] = matrix[row2][col1];
        }
        i += 2;
    }
    ciphertext[k] = '\0';
}

// Function to decrypt ciphertext using the Playfair cipher
void decrypt(char *ciphertext, char *key, char *plaintext) {
    char matrix[5][5];
    createMatrix(key, matrix);
    
    int i = 0;
    int k = 0;
    while (ciphertext[i]) {
        int row1, col1, row2, col2;
        findPosition(ciphertext[i], matrix, &row1, &col1);
        findPosition(ciphertext[i+1], matrix, &row2, &col2);
        
        if (row1 == row2) {
            plaintext[k++] = matrix[row1][(col1 - 1 + 5) % 5];
            plaintext[k++] = matrix[row2][(col2 - 1 + 5) % 5];
        } else if (col1 == col2) {
            plaintext[k++] = matrix[(row1 - 1 + 5) % 5][col1];
            plaintext[k++] = matrix[(row2 - 1 + 5) % 5][col2];
        } else {
            plaintext[k++] = matrix[row1][col2];
            plaintext[k++] = matrix[row2][col1];
        }
        i += 2;
    }
    plaintext[k] = '\0';
    
    // Remove trailing 'X' used for padding if any
    int len = strlen(plaintext);
    if (len > 0 && plaintext[len - 1] == 'X') {
        plaintext[len - 1] = '\0';
    }
}

int main() {
    char plaintext[256], ciphertext[256], decryptedtext[256], key[256];
    
    printf("Enter the key: ");
    fgets(key, sizeof(key), stdin);
    key[strcspn(key, "\n")] = '\0'; // Remove newline character
    
    printf("Enter the plaintext: ");
    fgets(plaintext, sizeof(plaintext), stdin);
    plaintext[strcspn(plaintext, "\n")] = '\0'; // Remove newline character
    
    encrypt(plaintext, key, ciphertext);
    printf("Encrypted text: %s\n", ciphertext);
    
    decrypt(ciphertext, key, decryptedtext);
    printf("Decrypted text: %s\n", decryptedtext);
    
    return 0;
}
## OUTPUT:
![image](https://github.com/user-attachments/assets/33e1d540-7948-4c08-963b-dd188dc33340)

## RESULT:
The program is executed successfully

---------------------------

# Hill Cipher
Hill Cipher using with different key values

# AIM:

To develop a simple C program to implement Hill Cipher.

## DESIGN STEPS:

### Step 1:

Design of Hill Cipher algorithnm 

### Step 2:

Implementation using C or pyhton code

### Step 3:

Testing algorithm with different key values. 
ALGORITHM DESCRIPTION:
The Hill cipher is a substitution cipher invented by Lester S. Hill in 1929. Each letter is represented by a number modulo 26. To encrypt a message, each block of n letters is multiplied by an invertible n × n matrix, again modulus 26.
To decrypt the message, each block is multiplied by the inverse of the matrix used for encryption. The matrix used for encryption is the cipher key, and it should be chosen randomly from the set of invertible n × n matrices (modulo 26).
The cipher can, be adapted to an alphabet with any number of letters. All arithmetic just needs to be done modulo the number of letters instead of modulo 26.

## PROGRAM:
PROGRAM:
#include<stdio.h>
#include<string.h>
int main() {
    unsigned int a[3][3] = { { 6, 24, 1 }, { 13, 16, 10 }, { 20, 17, 15 } };
    unsigned int b[3][3] = { { 8, 5, 10 }, { 21, 8, 21 }, { 21, 12, 8 } };
    int i, j;
    unsigned int c[20], d[20];
    char msg[20];
    int determinant = 0, t = 0;
    ;
    printf("Enter plain text\n ");
    scanf("%s", msg);
    for (i = 0; i < 3; i++) {
        c[i] = msg[i] - 65;
        printf("%d ", c[i]);
    }
    for (i = 0; i < 3; i++) {
        t = 0;
        for (j = 0; j < 3; j++) {
            t = t + (a[i][j] * c[j]);
        }
        d[i] = t % 26;
    }
    printf("\nEncrypted Cipher Text :");
    for (i = 0; i < 3; i++)
        printf(" %c", d[i] + 65);
    for (i = 0; i < 3; i++) {
        t = 0;
        for (j = 0; j < 3; j++) {
            t = t + (b[i][j] * d[j]);
        }
        c[i] = t % 26;
    }
    printf("\nDecrypted Cipher Text :");
    for (i = 0; i < 3; i++)
        printf(" %c", c[i] + 65);
    return 0;
}
## OUTPUT:
![image](https://github.com/user-attachments/assets/298f2511-b948-4fba-a3d0-d107cf20d290)

## RESULT:
The program is executed successfully

-------------------------------------------------

# Vigenere Cipher
Vigenere Cipher using with different key values

# AIM:

To develop a simple C program to implement Vigenere Cipher.

## DESIGN STEPS:

### Step 1:

Design of Vigenere Cipher algorithnm 

### Step 2:

Implementation using C or pyhton code

### Step 3:

Testing algorithm with different key values. 

ALGORITHM DESCRIPTION:

The Vigenere cipher is a method of encrypting alphabetic text by using a series of different Caesar ciphers based on the letters of a keyword. It is a simple form of polyalphabetic substitution.To encrypt, a table of alphabets can be used, termed a Vigenere square, or Vigenere table. It consists of the alphabet written out 26 times in different rows, each alphabet shifted cyclically to the left compared to the previous alphabet, corresponding to the 26 possible Caesar ciphers. At different points in the encryption process, the cipher uses a different alphabet from one of the rows used. The alphabet at each point depends on a repeating keyword.



## PROGRAM:
PROGRAM:
#include <stdio.h>
#include <ctype.h>
#include <string.h>
#include <stdlib.h>

void encrypt() {
    char plaintext[128];
    char key[16];
    printf("\nEnter the plaintext (up to 128 characters): ");
    scanf(" %[^\n]", plaintext); // Read input with spaces
    printf("Enter the key (up to 16 characters): ");
    scanf(" %[^\n]", key);

    printf("Cipher Text: ");
    for (int i = 0, j = 0; i < strlen(plaintext); i++, j++) {
        if (j >= strlen(key)) {
            j = 0;
        }
        int shift = toupper(key[j]) - 'A';
        char encryptedChar = ((toupper(plaintext[i]) - 'A' + shift) % 26) + 'A';
        printf("%c", encryptedChar);
    }
    printf("\n");
}

void decrypt() {
    char ciphertext[128];
    char key[16];
    printf("\nEnter the ciphertext: ");
    scanf(" %[^\n]", ciphertext);
    printf("Enter the key: ");
    scanf(" %[^\n]", key);

    printf("Deciphered Text: ");
    for (int i = 0, j = 0; i < strlen(ciphertext); i++, j++) {
        if (j >= strlen(key)) {
            j = 0;
        }
        int shift = toupper(key[j]) - 'A';
        char decryptedChar = ((toupper(ciphertext[i]) - 'A' - shift + 26) % 26) + 'A';
        printf("%c", decryptedChar);
    }
    printf("\n");
}

int main() {
    int option;
    while (1) {
        printf("\n1. Encrypt");
        printf("\n2. Decrypt");
        printf("\n3. Exit\n");
        printf("\nEnter your option: ");
        scanf("%d", &option);

        switch (option) {
            case 1:
                encrypt();
                break;
            case 2:
                decrypt();
                break;
            case 3:
                exit(0);
            default:
                printf("\nInvalid selection! Try again.\n");
                break;
        }
    }
    return 0;
}

## OUTPUT:
![image](https://github.com/user-attachments/assets/6774007f-d0a2-431c-bf41-d628d5a570b4)

## RESULT:
The program is executed successfully

-----------------------------------------------------------------------

# Rail Fence Cipher
Rail Fence Cipher using with different key values

# AIM:

To develop a simple C program to implement Rail Fence Cipher.

## DESIGN STEPS:

### Step 1:

Design of Rail Fence Cipher algorithnm 

### Step 2:

Implementation using C or pyhton code

### Step 3:

Testing algorithm with different key values. 
ALGORITHM DESCRIPTION:
In the rail fence cipher, the plaintext is written downwards and diagonally on successive "rails" of an imaginary fence, then moving up when we reach the bottom rail. When we reach the top rail, the message is written downwards again until the whole plaintext is written out. The message is then read off in rows.

## PROGRAM:

PROGRAM:
#include<stdio.h>
#include<string.h>

void encryptMsg(char msg[], int key){
    int msgLen = strlen(msg), i, j, k = -1, row = 0, col = 0;
    char railMatrix[key][msgLen];

    for(i = 0; i < key; ++i)
        for(j = 0; j < msgLen; ++j)
            railMatrix[i][j] = '\n';

    for(i = 0; i < msgLen; ++i){
        railMatrix[row][col++] = msg[i];

        if(row == 0 || row == key-1)
            k= k * (-1);

        row = row + k;
    }

    printf("\nEncrypted Message: ");

    for(i = 0; i < key; ++i)
        for(j = 0; j < msgLen; ++j)
            if(railMatrix[i][j] != '\n')
                printf("%c", railMatrix[i][j]);
}

void decryptMsg(char enMsg[], int key){
    int msgLen = strlen(enMsg), i, j, k = -1, row = 0, col = 0, m = 0;
    char railMatrix[key][msgLen];

    for(i = 0; i < key; ++i)
        for(j = 0; j < msgLen; ++j)
            railMatrix[i][j] = '\n';

    for(i = 0; i < msgLen; ++i){
        railMatrix[row][col++] = '*';

        if(row == 0 || row == key-1)
            k= k * (-1);

        row = row + k;
    }

    for(i = 0; i < key; ++i)
        for(j = 0; j < msgLen; ++j)
            if(railMatrix[i][j] == '*')
                railMatrix[i][j] = enMsg[m++];

    row = col = 0;
    k = -1;

    printf("\nDecrypted Message: ");

    for(i = 0; i < msgLen; ++i){
        printf("%c", railMatrix[row][col++]);

        if(row == 0 || row == key-1)
            k= k * (-1);

        row = row + k;
    }
}

int main(){
    char msg[] = "Hello World";
    char enMsg[] = "Horel ollWd";
    int key = 3;

    printf("Original Message: %s", msg);

    encryptMsg(msg, key);
    decryptMsg(enMsg, key);

    return 0;
}
## OUTPUT:
![image](https://github.com/user-attachments/assets/50574021-e039-431d-afc6-29ceced5533a)

## RESULT:
The program is executed successfully
