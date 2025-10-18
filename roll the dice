// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

contract RollTheDice {
    address public owner;
    uint256 public minimumBet = 0.01 ether;

    event DiceRolled(address indexed player, uint256 bet, uint8 result, bool win);
    event FundsDeposited(address indexed sender, uint256 amount);
    event FundsWithdrawn(address indexed owner, uint256 amount);

    constructor() {
        owner = msg.sender;
    }

    // Function to roll the dice
    function rollDice() external payable {
        require(msg.value >= minimumBet, "Bet is below minimum");
        require(address(this).balance >= msg.value * 2, "Not enough funds to pay if you win");

        // Pseudo-random (not safe for production!)
        uint8 diceResult = uint8(uint256(keccak256(abi.encodePacked(block.timestamp, block.prevrandao, msg.sender))) % 6 + 1);

        bool win = false;
        if (diceResult >= 4) {
            win = true;
            payable(msg.sender).transfer(msg.value * 2);
        }

        emit DiceRolled(msg.sender, msg.value, diceResult, win);
    }

    // Allow contract to receive ETH
    receive() external payable {
        emit FundsDeposited(msg.sender, msg.value);
    }

    // Owner can withdraw funds
    function withdraw(uint256 amount) external {
        require(msg.sender == owner, "Only owner can withdraw");
        require(amount <= address(this).balance, "Insufficient balance");
        payable(owner).transfer(amount);
        emit FundsWithdrawn(owner, amount);
    }

    // Change minimum bet
    function setMinimumBet(uint256 newMin) external {
        require(msg.sender == owner, "Only owner can set min bet");
        minimumBet = newMin;
    }

    // Get contract balance
    function getBalance() external view returns (uint256) {
        return address(this).balance;
    }
}
