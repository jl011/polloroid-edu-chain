// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

contract TransactionSystem {
    // Mapping to store the balance of each user
    mapping(address => uint256) public balances;

    // Events to log actions
    event Deposited(address indexed sender, uint256 amount);
    event Withdrawn(address indexed sender, uint256 amount);
    event Transferred(address indexed from, address indexed to, uint256 amount);

    // Deposit Ether into the contract
    function deposit() public payable {
        require(msg.value > 0, "You must send some Ether!");
        balances[msg.sender] += msg.value;  // Add the deposited Ether to the sender's balance
        emit Deposited(msg.sender, msg.value);  // Emit deposit event
    }

    // Withdraw Ether from the contract
    function withdraw(uint256 amount) public {
        require(amount <= balances[msg.sender], "Insufficient balance!");  // Ensure user has enough funds
        balances[msg.sender] -= amount;  // Subtract the amount from sender's balance
        payable(msg.sender).transfer(amount);  // Transfer the Ether to the sender
        emit Withdrawn(msg.sender, amount);  // Emit withdrawal event
    }

    // Transfer Ether to another address
    function transfer(address payable recipient, uint256 amount) public {
        require(amount <= balances[msg.sender], "Insufficient balance!");  // Ensure user has enough funds
        balances[msg.sender] -= amount;  // Subtract the amount from sender's balance
        balances[recipient] += amount;  // Add the amount to the recipient's balance
        emit Transferred(msg.sender, recipient, amount);  // Emit transfer event
    }

    // Check the balance of the caller (the user interacting with the contract)
    function getBalance() public view returns (uint256) {
        return balances[msg.sender];
    }
}
