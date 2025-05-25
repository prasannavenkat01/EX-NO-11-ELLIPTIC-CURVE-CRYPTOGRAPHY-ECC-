# EX-NO-11-ELLIPTIC-CURVE-CRYPTOGRAPHY-ECC

## Aim:
To Implement ELLIPTIC CURVE CRYPTOGRAPHY(ECC)


## ALGORITHM:

1. Elliptic Curve Cryptography (ECC) is a public-key cryptography technique based on the algebraic structure of elliptic curves over finite fields.

2. Initialization:
   - Select an elliptic curve equation \( y^2 = x^3 + ax + b \) with parameters \( a \) and \( b \), along with a large prime \( p \) (defining the finite field).
   - Choose a base point \( G \) on the curve, which will be used for generating public keys.

3. Key Generation:
   - Each party selects a private key \( d \) (a random integer).
   - Calculate the public key as \( Q = d \times G \) (using elliptic curve point multiplication).

4. Encryption and Decryption:
   - Encryption: The sender uses the recipient’s public key and the base point \( G \) to encode the message.
   - Decryption: The recipient uses their private key to decode the message and retrieve the original plaintext.

5. Security: ECC’s security relies on the Elliptic Curve Discrete Logarithm Problem (ECDLP), making it highly secure with shorter key lengths compared to traditional methods like RSA.

## Program:
```
#include <stdio.h>

#define MAX 10000

typedef struct {
    int x;
    int y;
    int infinity; // 1 if point at infinity
} Point;

// Function prototypes
int modinv(int a, int p);
Point point_add(Point p1, Point p2, int a, int p);
Point scalar_mult(int d, Point G, int a, int p);

// Modular inverse using Fermat's Little Theorem
int modinv(int a, int p) {
    int result = 1;
    int exp = p - 2;

    while (exp) {
        if (exp % 2 == 1)
            result = (result * a) % p;
        a = (a * a) % p;
        exp /= 2;
    }
    return result;
}

// Point addition
Point point_add(Point p1, Point p2, int a, int p) {
    Point result;

    if (p1.infinity) return p2;
    if (p2.infinity) return p1;

    if (p1.x == p2.x && (p1.y != p2.y || p1.y == 0)) {
        result.infinity = 1;
        return result;
    }

    int m;
    if (p1.x == p2.x && p1.y == p2.y) {
        m = ((3 * p1.x * p1.x + a) * modinv(2 * p1.y, p)) % p;
    } else {
        int num = (p2.y - p1.y + p) % p;
        int den = (p2.x - p1.x + p) % p;
        m = (num * modinv(den, p)) % p;
    }

    result.x = (m * m - p1.x - p2.x + 2 * p) % p;
    result.y = (m * (p1.x - result.x) - p1.y + p) % p;
    result.infinity = 0;

    return result;
}

// Scalar multiplication (d * G)
Point scalar_mult(int d, Point G, int a, int p) {
    Point R = {0, 0, 1}; // point at infinity
    for (int i = 0; i < d; i++) {
        R = point_add(R, G, a, p);
    }
    return R;
}

int main() {
    int a, b, p;
    Point G;
    int d;

    printf("Enter elliptic curve parameters (a, b): ");
    scanf("%d %d", &a, &b);

    printf("Enter prime number (p): ");
    scanf("%d", &p);

    printf("Enter base point G (x y): ");
    scanf("%d %d", &G.x, &G.y);
    G.infinity = 0;

    printf("Enter private key (d): ");
    scanf("%d", &d);

    Point Q = scalar_mult(d, G, a, p);

    printf("\n--- ECC Key Generation ---\n");
    printf("Private Key (d): %d\n", d);
    printf("Public Key (Q = d*G): (%d, %d)\n", Q.x, Q.y);

    return 0;
}


```


## Output:
![image](https://github.com/user-attachments/assets/53b85042-644e-4e78-8530-afb422dd7f86)


## Result:
The program is executed successfully

