# ETHAVAX4
This project contains the program for the submission of the assessment Project: Degen Token (ERC-20): Unlocking the Future of Gaming

## Description

The goal of the project is to create my own ERC20 token and deploy it using HardHat or Remix. Once deployed, the contract owner should be able to mint tokens to a provided address and any user should be able to burn and transfer tokens. In this project:

- Only the contract owner is able to mint tokens.
- Any user can transfer tokens.
- Any user can burn tokens.

The goal of the project is to create my own ERC20 token and deploy it on the Avalanche network for Degen Gaming. The smart contract has the following functionality:

- Minting new tokens: The platform should be able to create new tokens and distribute them to players as rewards. Only the owner can mint tokens.
- Transferring tokens: Players should be able to transfer their tokens to others.
- Redeeming tokens: Players should be able to redeem their tokens for items in the in-game store.
- Checking token balance: Players should be able to check their token balance at any time.
- Burning tokens: Anyone should be able to burn tokens, that they own, that are no longer needed.

## Getting Started

### Executing program

In order to run this program, it is recommended to copy the whole code and run it in Remix or HardHat. The code can be copied below:

```
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

import "@openzeppelin/contracts/token/ERC20/ERC20.sol";
import "@openzeppelin/contracts/access/Ownable.sol";

contract DegenToken is ERC20, Ownable {

    mapping(uint256 => uint256) public prices;

    constructor() ERC20("Degen", "DGN") {
        prices[1] = 100; // Legendary Sword
        prices[2] = 50;  // Healing Potion
        prices[3] = 25;  // Magic Scroll
    }

    function mint(address _to, uint256 _value) public onlyOwner {
        _mint(_to, _value);
    }

    function transferTokens(address _to, uint256 _value) external {
        require(balanceOf(msg.sender) >= _value, "Error: Insufficient Degen Tokens");
        approve(_to, _value); // Approve the transfer
        transferFrom(msg.sender, _to, _value); // Transfer using allowance mechanism
    }

    function redeemToken(uint8 _option) external {
        require(_option >= 1 && _option <= 3, "Please select a valid item."); // Validate item selection
        require(balanceOf(msg.sender) >= prices[_option], "Error: Insufficient funds to redeem the item.");
        
        // Directly transfer tokens to the owner
        _transfer(msg.sender, owner(), prices[_option]);
    }

    function burnTokens(uint256 _value) external {
        require(balanceOf(msg.sender) >= _value, "Error: Insufficient Degen Tokens");
        approve(address(this), _value); // Approve the contract to burn tokens
        _burn(msg.sender, _value); // Burn the approved tokens
    }

    function showShopItems() external pure returns (string memory) {
        return "The items on sale are the following: [1] Legendary Sword (100) [2] Healing Potion (50) [3] Magic Scroll (25)";
    }

    function decimals() public pure override returns (uint8) {
        return 0; // No decimal places for the token
    }

    function getBalance() external view returns (uint256) {
        return balanceOf(msg.sender); // Simplified balance retrieval
    }
}
```

After copying it, compiling it will make the contract deployable and ready for interaction.

## Submitted by:

- John Alvin R. Cruz
