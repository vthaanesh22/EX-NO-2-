## EX. NO:2 IMPLEMENTATION OF PLAYFAIR CIPHER
# NAME : Thaanesh
# REG NO : 212223230228
 

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




# Program:

~~~
def prepare_text(text):
    """ Prepare plaintext by replacing 'j' with 'i' and adding 'x' for repeated or odd characters """
    text = text.lower().replace("j", "i")
    prepared = ""
    
    i = 0
    while i < len(text):
        a = text[i]
        if i + 1 < len(text):
            b = text[i + 1]
            if a == b:
                prepared += a + "x"
                i += 1
            else:
                prepared += a + b
                i += 2
        else:
            prepared += a + "x"
            i += 1

    if len(prepared) % 2 != 0:
        prepared += "x"

    return prepared


def generate_key_matrix(key):
    """ Generate the 5x5 key matrix """
    key = key.lower().replace("j", "i")
    matrix = []
    used = set()

    # Fill with key characters
    for char in key:
        if char not in used and char.isalpha():
            used.add(char)
            matrix.append(char)

    # Fill remaining characters
    for i in range(26):
        letter = chr(i + ord('a'))
        if letter != 'j' and letter not in used:
            matrix.append(letter)

    # Convert to 5x5 matrix
    return [matrix[i * 5:(i + 1) * 5] for i in range(5)]


def find_position(matrix, char):
    """ Find the row and column of a character in the matrix """
    for row in range(5):
        for col in range(5):
            if matrix[row][col] == char:
                return row, col
    return None


def encrypt_pair(matrix, a, b):
    """ Encrypt a pair of characters """
    row1, col1 = find_position(matrix, a)
    row2, col2 = find_position(matrix, b)

    if row1 == row2:
        # Same row: move right
        return matrix[row1][(col1 + 1) % 5] + matrix[row2][(col2 + 1) % 5]
    elif col1 == col2:
        # Same column: move down
        return matrix[(row1 + 1) % 5][col1] + matrix[(row2 + 1) % 5][col2]
    else:
        # Rectangle swap
        return matrix[row1][col2] + matrix[row2][col1]


def decrypt_pair(matrix, a, b):
    """ Decrypt a pair of characters """
    row1, col1 = find_position(matrix, a)
    row2, col2 = find_position(matrix, b)

    if row1 == row2:
        # Same row: move left
        return matrix[row1][(col1 - 1) % 5] + matrix[row2][(col2 - 1) % 5]
    elif col1 == col2:
        # Same column: move up
        return matrix[(row1 - 1) % 5][col1] + matrix[(row2 - 1) % 5][col2]
    else:
        # Rectangle swap
        return matrix[row1][col2] + matrix[row2][col1]


def playfair_encrypt(matrix, plaintext):
    """ Encrypt the plaintext using the Playfair cipher """
    plaintext = prepare_text(plaintext)
    ciphertext = ""

    for i in range(0, len(plaintext), 2):
        ciphertext += encrypt_pair(matrix, plaintext[i], plaintext[i + 1])

    return ciphertext


def playfair_decrypt(matrix, ciphertext):
    """ Decrypt the ciphertext using the Playfair cipher """
    plaintext = ""

    for i in range(0, len(ciphertext), 2):
        plaintext += decrypt_pair(matrix, ciphertext[i], ciphertext[i + 1])

    return plaintext


# Main function to run the Playfair Cipher
def main():
    key = input("Enter the key: ").strip()
    plaintext = input("Enter the plaintext: ").strip()

    # Generate key matrix
    matrix = generate_key_matrix(key)

    print("\nKey Matrix:")
    for row in matrix:
        print(" ".join(row))

    # Encrypt and decrypt
    ciphertext = playfair_encrypt(matrix, plaintext)
    decrypted_text = playfair_decrypt(matrix, ciphertext)
    
    print("\nPlain Text:", decrypted_text)
    print("Encrypted Text:", ciphertext)
    


# Run the program
if __name__ == "__main__":
    main()

~~~




# Output:

![image](https://github.com/user-attachments/assets/7393de51-7d87-4024-bcfb-896cd823cfec)

# RESULT :
The playfair substitution method is implemented using c programming
