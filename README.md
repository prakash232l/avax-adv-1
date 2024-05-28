# ERC20 and Vault Smart Contracts

## Overview

This repository contains two Solidity smart contracts: an ERC20 token contract and a Vault contract. The ERC20 contract implements a standard ERC20 token, while the Vault contract provides secure storage and management for these tokens.

## ERC20 Contract

### Features

- **ERC-20 Compliance:** Implements the standard ERC-20 interface.
- **Mintable:** Allows the owner to mint new tokens.
- **Burnable:** Allows token holders to burn their tokens.
- **Transferable:** Enables token transfers between addresses.
- **Event Logging:** Emits events for minting, burning, and transferring tokens.

### Usage

#### Deployment

Deploy the contract with an initial supply:

```solidity
constructor(uint256 initialSupply) ERC20("My Token", "MTK") {
    ownerAddress = msg.sender;
    _mint(ownerAddress, initialSupply);
}
```

#### Minting Tokens

Only the owner can mint tokens:

```solidity
function mintTokens(address beneficiary, uint256 amount) external onlyOwner {
    _mint(beneficiary, amount);
    emit TokensTransferred(address(0), beneficiary, amount);
}
```

#### Burning Tokens

Token holders can burn their tokens:

```solidity
function burnTokens(uint256 amount) external {
    require(balanceOf(msg.sender) >= amount, "Insufficient tokens balance to burn");
    _burn(msg.sender, amount);
    emit TokensTransferred(msg.sender, address(0), amount);
}
```

#### Transferring Tokens

Token holders can transfer tokens:

```solidity
function transferTokens(address receiver, uint256 amount) external returns (bool) {
    _transfer(msg.sender, receiver, amount);
    emit TokensTransferred(msg.sender, receiver, amount);
    return true;
}
```

## Vault Contract

### Features

- **Token Storage:** Securely stores ERC20 tokens.
- **Deposits:** Allows users to deposit tokens into the vault.
- **Withdrawals:** Allows users to withdraw tokens from the vault.
- **Ownership:** Managed by a contract owner for secure operations.

### Usage

#### Deposit Tokens

Users can deposit tokens into the vault:

```solidity
function depositTokens(address token, uint256 amount) external {
    IERC20(token).transferFrom(msg.sender, address(this), amount);
    // Update vault records
}
```

#### Withdraw Tokens

Users can withdraw tokens from the vault:

```solidity
function withdrawTokens(address token, uint256 amount) external {
    require(balanceOf(msg.sender) >= amount, "Insufficient balance in vault");
    IERC20(token).transfer(msg.sender, amount);
    // Update vault records
}
```

## License

This project is licensed under the MIT License.
