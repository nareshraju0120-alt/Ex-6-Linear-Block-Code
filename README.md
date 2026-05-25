# Ex: 06 - Linear Block Code

## AIM:
Write a simple Python program to generate Matrix, Codeword, Hamming weight, Syndrome matrix and find the error on recieved codeword using Linear Block Code.

## TOOLS REQUIRED:
Google colab

## PROGRAM:
import numpy as np

pb = []

col = int(input("Enter the Parity bits : "))   # n-k
row = int(input("Enter the Message bits : "))  # k

for i in range(row):
    P = list(map(int, input(f"Enter row {i+1} (space separated) : ").split()))
    pb.append(P)

P = np.array(pb, dtype=int)

I_k = np.eye(row, dtype=int)
G = np.hstack((P, I_k))

print("\nGenerator Matrix (G):")
for r in G:
    print(" ".join(map(str, r)))

# Code parameters
k = row
n = G.shape[1]

messages = np.array([[int(x) for x in format(i, f'0{k}b')]
                     for i in range(2**k)])

# Generate codewords
codewords = np.mod(np.dot(messages, G), 2)

print("\nMessage   Codeword   Weight")
weights = []

for i in range(len(messages)):
    wt = np.sum(codewords[i])
    weights.append(wt)

    print(" ".join(map(str, messages[i])), " - ",
          " ".join(map(str, codewords[i])), " - ", wt)

# Minimum Hamming distance
d_min = np.min([w for w in weights if w != 0])

print("\nMinimum Hamming Distance:", d_min)

I_r = np.eye(col, dtype=int)
H = np.hstack((I_r, P.T))

print("\nParity Check Matrix (H):")
for r in H:
    print(" ".join(map(str, r)))

H_T = H.T

rc = list(map(int, input("\nEnter the received codeword : ").split()))
r = np.array(rc)

# Syndrome
S = np.mod(np.dot(r, H_T), 2)

print("Syndrome : ", " ".join(map(str, S)))

error = np.zeros(n, dtype=int)

for i in range(n):
    if np.array_equal(H_T[i], S):
        error[i] = 1
        break

print("Error Vector : ", " ".join(map(str, error)))

# Corrected codeword
corrected = np.mod(r + error, 2)

print("Corrected Codeword : ", " ".join(map(str, corrected)))
## OUTPUT WAVEFORM:
<img width="951" height="502" alt="image" src="https://github.com/user-attachments/assets/f7037875-975f-4c8a-acf3-db9e3772b0cf" />

## RESULT:
Thus, Linear Block Code operation for the given input is successfully verified.
