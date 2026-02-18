# simpleWallet

// SPDX-License-Identifier: GPL-3.0

pragma solidity >=0.7.0 <0.9.0;
contract SimpleVault {

    struct User {
        uint balance;
        uint totalDeposited;
        uint timestamp;
    }

    mapping(address => User) public users;

    // Deposit Ether into contract

    function deposit() public payable {
        require(msg.value > 0, "Must Send eth");

        users[msg.sender].balance += msg.value;
       // users[msg.sender].totalDeposited += msg.value;
        users[msg.sender].totalDeposited++;
        users[msg.sender].timestamp = block.timestamp;
    }

    // Withdraw Ether using low-level call

    function withdraw( uint _amount) public {
        require(users[msg.sender].balance >= _amount, "Insufficient balance");
        
        users[msg.sender].balance -= _amount;
        

        (bool success,) = msg.sender.call{value: _amount}("");
        require(success, "Transfer Failed");
    }

      // Check user balance
    function checkBalance() public view returns (uint256) {
        return users[msg.sender].balance;
    }

}
