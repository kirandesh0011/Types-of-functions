// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

import "@openzeppelin/contracts/token/ERC20/ERC20.sol";
import "@openzeppelin/contracts/access/Ownable.sol";

contract CurrencyToken is ERC20, Ownable {

    constructor(address initialOwner) ERC20("CurrencyToken", "CUR") Ownable(initialOwner) {}

    function mintTokens(address beneficiary, uint256 value) external onlyOwner {
        require(value > 0, "CurrencyToken: value must be greater than zero"); // Require a positive value
        _mint(beneficiary, value);
    }

    function burnTokens(uint256 value) external {
        require(value > 0 && value <= balanceOf(msg.sender), "CurrencyToken: invalid value for burning"); // Require a positive and valid value
        _burn(msg.sender, value);
    }

    function transferTokens(address beneficiary, uint256 value) public returns (bool) {
        require(beneficiary != address(0) && value > 0 && value <= balanceOf(msg.sender), "CurrencyToken: invalid transfer parameters"); // Require a valid beneficiary, positive value, and sufficient balance
        _transfer(msg.sender, beneficiary, value);
        return true;
    }
}
