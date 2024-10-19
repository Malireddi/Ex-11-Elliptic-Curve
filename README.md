# Ex-11: Implementation of Elliptic Curve Cryptography (ECC) 
## AIM: 
To write a C program to implement the Elliptic Curve Cryptography (ECC) algorithm for secure key exchange. 
## ALGORITHM: 
1. Select Parameters: Choose an elliptic curve defined over a finite field, including a prime number " p ", the coefficients " a " and " b " of the curve, and a base point " G ". 2. Key Generation: 
- Alice selects a private key " d_A " (a random integer). 
- Compute Alice's public key as " P_A = d_A \cdot G " (point multiplication). - Bob selects a private key " d_B " and computes his public key as " P_B = d_B \cdot G ". 3. Key Exchange: 
- Alice and Bob exchange their public keys. 
4. Shared Secret Calculation: 
- Alice computes the shared secret as " S_A = d_A \cdot P_B ". 
- Bob computes the shared secret as " S_B = d_B \cdot P_A ". 
5. Both shared secrets should be equal. 
## PROGRAM: 
```
#include <stdio.h> 

// A simple structure to represent points on the elliptic curve 
typedef struct { 
    long long int x, y; 
} Point; 

// Function to compute modular inverse (using Extended Euclidean Algorithm) 
long long int modInverse(long long int a, long long int m) { 
    long long int m0 = m, t, q; 
    long long int x0 = 0, x1 = 1; 

    if (m == 1) return 0; 

    while (a > 1) { 
        q = a / m; 
        t = m; 
        m = a % m; 
        a = t; 
        t = x0; 
        x0 = x1 - q * x0; 
        x1 = t; 
    } 

    if (x1 < 0) x1 += m0; 
    return x1; 
} 

// Function to perform point addition on elliptic curves 
Point pointAddition(Point P, Point Q, long long int a, long long int p) { 
    Point R;
    long long int lambda; 

    // Check if P == Q (Point doubling) 
    if (P.x == Q.x && P.y == Q.y) { 
        lambda = (3 * P.x * P.x + a) * modInverse(2 * P.y, p) % p; 
    } else { // Point addition 
        lambda = (Q.y - P.y) * modInverse(Q.x - P.x, p) % p; 
    } 

    // Calculate resulting point 
    R.x = (lambda * lambda - P.x - Q.x) % p; 
    R.y = (lambda * (P.x - R.x) - P.y) % p; 

    // Ensure values are positive 
    R.x = (R.x + p) % p; 
    R.y = (R.y + p) % p; 

    return R; 
} 

// Function to perform scalar multiplication (Elliptic Curve Point Multiplication) 
Point scalarMultiplication(Point P, long long int k, long long int a, long long int p) { 
    Point result = P; 
    k--; // Subtract 1 because we start with the base point 

    while (k > 0) { 
        result = pointAddition(result, P, a, p); // Add the point to itself (k times)
        k--; 
    } 

    return result; 
} 

int main() { 
    long long int p, a, b, privateA, privateB; 
    Point G, publicA, publicB, sharedSecretA, sharedSecretB; 

    printf("Nandyala Malyadri Reddy - 212223100037\n"); 
    
    // Step 1: Input parameters of the elliptic curve 
    printf("Enter the prime number (p): "); 
    scanf("%lld", &p); 
    
    printf("Enter the curve parameters (a and b) for equation y^2 = x^3 + ax + b: "); 
    scanf("%lld %lld", &a, &b); 
    
    printf("Enter the base point G (x and y): "); 
    scanf("%lld %lld", &G.x, &G.y); 
    
    // Step 2: Alice and Bob input private keys 
    printf("Enter Alice's private key: "); 
    scanf("%lld", &privateA);
    
    printf("Enter Bob's private key: "); 
    scanf("%lld", &privateB); 
    
    // Step 3: Compute public keys (Elliptic Curve Point Multiplication) 
    publicA = scalarMultiplication(G, privateA, a, p); // Alice's public key
    publicB = scalarMultiplication(G, privateB, a, p); // Bob's public key 
    
    printf("Alice's public key: (%lld, %lld)\n", publicA.x, publicA.y); 
    printf("Bob's public key: (%lld, %lld)\n", publicB.x, publicB.y); 
    
    // Step 4: Compute shared secrets (Elliptic Curve Point Multiplication) 
    sharedSecretA = scalarMultiplication(publicB, privateA, a, p); // Alice's shared secret
    sharedSecretB = scalarMultiplication(publicA, privateB, a, p); // Bob's shared secret 
    
    // Step 5: Display shared secret
    printf("Shared secret computed by Alice: (%lld, %lld)\n", sharedSecretA.x, sharedSecretA.y); 
    printf("Shared secret computed by Bob: (%lld, %lld)\n", sharedSecretB.x, sharedSecretB.y); 
    
    if (sharedSecretA.x == sharedSecretB.x && sharedSecretA.y == sharedSecretB.y) { 
        printf("Key exchange successful. Both shared secrets match!\n"); 
    } else { 
        printf("Key exchange failed. Shared secrets do not match.\n"); 
    } 
    
    return 0; 
}
```
## OUTPUT: 

![Screenshot 2024-10-19 133847](https://github.com/user-attachments/assets/ecea0569-2fce-4c2d-af17-5569a5ca5cb1)

## RESULT: 
The program for Elliptic Curve Cryptography is executed successfully.
