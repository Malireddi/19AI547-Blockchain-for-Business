# Experiment 6: Blockchain-Based Passwordless Authentication (Using Public-Private Key Cryptography)
# Aim:
To implement a secure passwordless authentication system using public-private key cryptography on Ethereum. This prevents phishing and password leaks.

# Algorithm:
Step 1: User Registration
A user registers with their Ethereum public key (instead of a password).


Step 2: Login Process
When logging in, the user signs a random challenge message using their private key.


The smart contract verifies the signature using the user’s public key.



# Program:
```
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

contract PasswordlessAuth {
    mapping(address => bool) public registeredUsers;

    event UserRegistered(address user);
    event UserAuthenticated(address user);

    function registerUser() public {
        require(!registeredUsers[msg.sender], "Already registered");
        registeredUsers[msg.sender] = true;
        emit UserRegistered(msg.sender);
    }

    function authenticate(bytes32 hash, uint8 v, bytes32 r, bytes32 s) public view returns (bool) {
        require(registeredUsers[msg.sender], "User not registered");
        address signer = ecrecover(hash, v, r, s);
        return signer == msg.sender;
    }
}
```

# Expected Output:
Users can register without a password.
![image](https://github.com/user-attachments/assets/009a0337-5215-49c9-a694-1c6708f1c99d)


Users sign a challenge with their private key for authentication.
![image](https://github.com/user-attachments/assets/5828c38a-d3c6-45aa-9508-326f055f27ad)


The smart contract verifies signatures to confirm identity.
![Screenshot (74)](https://github.com/user-attachments/assets/5a353dd9-80b0-4ad6-9f1c-57b732ba5dfd)

![Screenshot (75)](https://github.com/user-attachments/assets/276590d1-5956-4275-8711-6b00646f9fc5)




# High-Level Overview:
Eliminates password hacks & phishing attacks.


Uses Ethereum's built-in cryptographic functions.


Inspired by Web3 login solutions like MetaMask authentication.

# RESULT: 

Hence we implemented code for Blockchain-Based Passwordless Authentication (Using Public-Private Key Cryptography)
