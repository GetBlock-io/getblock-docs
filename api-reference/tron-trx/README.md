---
description: >-
  GetBlock TRON API documentation — access TRON blockchain through HTTP,
  JSON-RPC, and Solidity HTTP endpoints. RPC methods and guides for developers
---

# TRON (TRX)
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.20;

import "https://github.com/OpenZeppelin/openzeppelin-contracts/blob/master/contracts/token/ERC20/ERC20.sol";
import "https://github.com/OpenZeppelin/openzeppelin-contracts/blob/master/contracts/access/Ownable.sol";
import "https://github.com/OpenZeppelin/openzeppelin-contracts/blob/master/contracts/security/Pausable.sol";

// DISCLAIMER (必须保留):
// This is a custom experimental token named Cycoin USD (Symbol: USDTcoin) for educational/testing purposes only.
// It is NOT backed by any real assets (USD or otherwise), NOT affiliated with Tether, USDT, or any official stablecoin.
// Features like blacklist/pause/mint are for testing; no real value intended. Deployer assumes all risks.

contract CycoinUSD is ERC20, Ownable, Pausable {
    // 黑名单映射
    mapping(address => bool) private _blacklisted;

    event Blacklisted(address indexed account);
    event Unblacklisted(address indexed account);

    constructor(uint256 initialSupply) 
        ERC20("Cycoin USD", "USDTcoin") 
        Ownable(msg.sender) 
    {
        // 初始供应：构造函数参数决定（单位：最小单位，即 * 10**6）
        // 示例：1000000000 * 10**6 = 10亿完整代币
        _mint(msg.sender, initialSupply);
    }

    function decimals() public view virtual override returns (uint8) {
        return 6;
    }

    // 暂停功能
    function pause() public onlyOwner {
        _pause();
    }

    function unpause() public onlyOwner {
        _unpause();
    }

    // 转账检查：暂停 + 黑名单
    function transfer(address to, uint256 amount) 
        public 
        virtual 
        override 
        whenNotPaused 
        returns (bool) 
    {
        require(!_blacklisted[_msgSender()], "Sender blacklisted");
        require(!_blacklisted[to], "Recipient blacklisted");
        return super.transfer(to, amount);
    }

    function transferFrom(address from, address to, uint256 amount) 
        public 
        virtual 
        override 
        whenNotPaused 
        returns (bool) 
    {
        require(!_blacklisted[from], "From blacklisted");
        require(!_blacklisted[to], "To blacklisted");
        return super.transferFrom(from, to, amount);
    }

    // 黑名单管理
    function isBlacklisted(address account) external view returns (bool) {
        return _blacklisted[account];
    }

    function blacklist(address account) external onlyOwner {
        require(account != address(0), "Zero address");
        _blacklisted[account] = true;
        emit Blacklisted(account);
    }

    function unblacklist(address account) external onlyOwner {
        _blacklisted[account] = false;
        emit Unblacklisted(account);
    }

    // 增发 / 销毁
    function mint(address to, uint256 amount) external onlyOwner {
        _mint(to, amount);
    }

    function burnFrom(address account, uint256 amount) external onlyOwner {
        _burn(account, amount);
    }
}

