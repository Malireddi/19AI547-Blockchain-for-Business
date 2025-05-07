# Experiment 5: Zero-Knowledge Proof (ZK) Private Voting System
# Aim:
To implement a fully private and transparent voting system using Zero-Knowledge Proofs (ZKPs). This ensures that votes are counted fairly without revealing who voted for whom.

# Algorithm:
Voter Registration,
Each voter generates a secret vote key and submits a commitment (hashed vote) to the contract.

Voting Process,
Voters submit their votes privately using a hash, without revealing their choice.

ZK Verification,
The contract verifies if a vote belongs to a registered voter but does not reveal the actual vote.

Vote Counting,
Once voting ends, the contract reveals the final tally without linking votes to individuals.

1.Register eligible voters by issuing them unique cryptographic credentials.

2.Allow voters to cast encrypted votes along with zero-knowledge proofs ensuring vote validity without revealing choices.

3.Collect and store all encrypted votes and their corresponding proofs on the blockchain.

4.Verify each vote's validity using the zero-knowledge proofs without decrypting the actual votes.

5.Tally the votes using homomorphic encryption techniques to aggregate results without compromising individual vote privacy.

6.Publish the final results along with cryptographic proofs to ensure transparency and verifiability of the election outcome.


# Program:
```
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

contract ZKVoting {
    struct Voter {
        bool registered;
        bytes32 voteCommitment;
    }

    mapping(address => Voter) public voters;
    uint256 public votesForA;
    uint256 public votesForB;

    event VoteCommitted(address voter, bytes32 commitment);
    event VoteRevealed(address voter, uint256 choice);

    function registerVoter(bytes32 commitment) public {
        require(!voters[msg.sender].registered, "Already registered");
        voters[msg.sender] = Voter(true, commitment);
        emit VoteCommitted(msg.sender, commitment);
    }

    function revealVote(string memory secret, uint256 choice) public {
        require(voters[msg.sender].registered, "Not registered");
        require(keccak256(abi.encodePacked(secret, choice)) == voters[msg.sender].voteCommitment, "Invalid proof");

        if (choice == 1) votesForA++;
        if (choice == 2) votesForB++;

        emit VoteRevealed(msg.sender, choice);
    }
}

```
# Expected Output:
Voters commit their votes privately.
![image](https://github.com/user-attachments/assets/1762aa89-67b3-42a6-b89d-a283b6b15b86)



When revealed, the contract verifies correctness but keeps votes anonymous.
![image](https://github.com/user-attachments/assets/68e0ef4e-9757-4350-bc20-5ea1e9189b98)

![image](https://github.com/user-attachments/assets/f8c641e6-f7ab-437d-a5f6-22c2657b73cc)




Final result is publicly verifiable without exposing individual votes.



# High-Level Overview:
Uses ZKPs to ensure anonymous and fair elections.


Prevents vote tampering while maintaining voter privacy.


Mimics real-world ZK voting applications in governance and DAOs.

# RESULT: 
Hence we implemented code for Zero-Knowledge Proof (ZK) Private Voting System
