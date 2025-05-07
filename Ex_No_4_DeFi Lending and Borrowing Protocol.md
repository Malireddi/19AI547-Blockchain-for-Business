# Experiment 4: DeFi Lending and Borrowing Protocol
# Aim:
To build a decentralized lending protocol where users can deposit assets to earn interest and borrow assets by providing collateral. This experiment introduces concepts like overcollateralization, liquidity pools, and interest accrual in DeFi.

# Algorithm:
1.Initialize the blockchain network and deploy smart contracts for lending and borrowing.

2.Users deposit assets into the protocol to provide liquidity.

3.Borrowers provide collateral that exceeds the loan value (over-collateralization).

4.Smart contracts assess collateral and approve loans based on predefined parameters.

5.Borrowers receive funds and repay loans with interest over time.

6.Interest is distributed to lenders as a return on their deposited assets.

7.Monitor collateral values and liquidate positions if collateral falls below required thresholds

Program:
```
//SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

contract DeFiLending {
    address public owner;
    uint256 public interestRate = 5; // 5% interest per cycle
    uint256 public liquidationThreshold = 150; // 150% collateralization
    mapping(address => uint256) public deposits;
    mapping(address => uint256) public borrowed;
    mapping(address => uint256) public collateral;

    event Deposited(address indexed user, uint256 amount);
    event Borrowed(address indexed user, uint256 amount, uint256 collateral);
    event Liquidated(address indexed user, uint256 debtRepaid, uint256 collateralSeized);

    constructor() {
        owner = msg.sender;
    }

    function deposit() public payable {
        require(msg.value > 0, "Deposit must be greater than zero");
        deposits[msg.sender] += msg.value;
        emit Deposited(msg.sender, msg.value);
    }

    function borrow(uint256 amount) public payable {
        require(msg.value >= (amount * liquidationThreshold) / 100, "Nota enough collateral");
        borrowed[msg.sender] += amount;
        collateral[msg.sender] += msg.value;
        payable(msg.sender).transfer(amount);
        emit Borrowed(msg.sender, amount, msg.value);
    }
    function reduceCollateral(address user, uint256 amount) public {
    require(msg.sender == owner, "Only owner can reduce");
    require(collateral[user] >= amount, "Not enough collateral to reduce");
    collateral[user] -= amount;
    }

    function liquidate(address borrower) public {
        require(collateral[borrower] < (borrowed[borrower] * liquidationThreshold) / 100, "Not eligible for liquidation");
        uint256 debt = borrowed[borrower];
        uint256 seizedCollateral = collateral[borrower];

        borrowed[borrower] = 0;
        collateral[borrower] = 0;
        payable(msg.sender).transfer(seizedCollateral);
        emit Liquidated(borrower, debt, seizedCollateral);
    }
}

```
# Expected Output:
Users can deposit ETH and earn interest.
![image](https://github.com/user-attachments/assets/dc82d9e0-c177-4359-90a1-4195ad73b540)


Users can borrow ETH by providing collateral.
![image](https://github.com/user-attachments/assets/333169d1-c66c-471f-82bc-1b8f6ae5ce94)


![image](https://github.com/user-attachments/assets/5ee99a90-e93d-41bf-ac94-99ce6bd8f232)


If collateral < 150% of borrowed amount, liquidators can seize the collateral.
![image](https://github.com/user-attachments/assets/458ac8b3-42a7-4f4c-aeb7-25a7ee9ab2b4)



# High-Level Overview:
Teaches key DeFi concepts: lending, borrowing, collateral, liquidation.


Introduces risk management: overcollateralization and liquidation.


Directly related to DeFi protocols like Aave and Compound.

# RESULT : 
Hence we implemented code for DeFi Lending and Borrowing Protocol
