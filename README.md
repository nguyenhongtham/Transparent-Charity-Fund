# Transparent-Charity-Fund
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.26;

import "@openzeppelin/contracts/access/Ownable.sol";

contract CharityFund is Ownable {
    uint256 public totalDonated;
    mapping(address => uint256) public donations;

    event Donated(address donor, uint256 amount);
    event Withdrawn(address to, uint256 amount, string reason);

    receive() external payable {
        totalDonated += msg.value;
        donations[msg.sender] += msg.value;
        emit Donated(msg.sender, msg.value);
    }

    function withdraw(uint256 amount, string calldata reason) external onlyOwner {
        payable(owner()).transfer(amount);
        emit Withdrawn(owner(), amount, reason);
    }

    function getBalance() external view returns (uint256) {
        return address(this).balance;
    }
}
