# ERC20解读

参考 [OpenZepplin文档](https://docs.openzeppelin.com/contracts/4.x/erc20) 和 [以太坊官方开发者文档](https://ethereum.org/en/developers/docs/standards/tokens/erc-20/)，结合自己的理解。

## 什么是ERC20

ERC20（Ethereum Request for Comments 20）一种代币标准。它具有一些典型功能。

ERC20 代币合约跟踪可替代代币：任何一个代币都完全等同于任何其他代币；没有任何代币具有与之相关的特殊权利或行为。这使得 ERC20 代币可用于交换货币、投票权、质押等媒介。

## 为什么要遵守ERC20

以太坊上的所有应用都默认支持 ERC20 ，如果你想自己发币，那么你的代码必须遵循 ERC20 标准，这样钱包（如MetaMask）等应用才能将你的币显示出来。

## 代码实现

需要实现以下函数和事件：

```solidity
function name() public view returns (string)
function symbol() public view returns (string)
function decimals() public view returns (uint8)
function totalSupply() public view returns (uint256)
function balanceOf(address _owner) public view returns (uint256 balance)
function transfer(address _to, uint256 _value) public returns (bool success)
function transferFrom(address _from, address _to, uint256 _value) public returns (bool success)
function approve(address _spender, uint256 _value) public returns (bool success)
function allowance(address _owner, address _spender) public view returns (uint256 remaining)

event Transfer(address indexed _from, address indexed _to, uint256 _value)
event Approval(address indexed _owner, address indexed _spender, uint256 _value)
```

使用 OpenZeppllin 提供的库能够轻松快速地构建 ERC20 Token 。

### 快速构建

这是一个 GLD token 。

```solidity
// contracts/GLDToken.sol
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

import "@openzeppelin/contracts/token/ERC20/ERC20.sol";

contract GLDToken is ERC20 {
    constructor(uint256 initialSupply) ERC20("Gold", "GLD") {
        _mint(msg.sender, initialSupply);
    }
}
```

通常，我们定义代币的发行量和代币名称及符号。

### IERC20

先来看下 ERC20 的接口（IERC20），这方便我们在开发中直接定义 ERC20 代币。

同样地，OpenZepplin 为我们提供了相应的库，方便开发者导入即用。

```solidity
import "@openzeppelin/contracts/token/ERC20/IERC20.sol";
```

**EIP 中定义的 ERC20 标准接口：**

```solidity
pragma solidity ^0.8.0;

interface IERC20 {
	event Transfer(address indexed from, address indexed to, uint256 value);
	event Approval(address indexed owner, address indexed spender, uint256 value);
	
	function totalSupply() external view returns (uint256);
	function balanceOf(address account) external view returns (uint256);
	function transfer(address to, uint256 amount) external returns (bool);
	function allowance(address owner, address spender) external view returns (uint256);
	function approve(address spender, uint256 amount) external returns (bool);
	function transferFrom(
		address from,
		address to,
		uint256 amount
	) external returns (bool);
}
```

#### 逐一分析

函数：

- `totalSupply()` ：返回总共的代币数量。
- `balanceOf(address account)` ：返回 `account` 地址拥有的代币数量。
- `transfer(address to, uint256 amount)` ：将 `amount` 数量的代币发送给 `to` 地址，返回布尔值告知是否执行成功。触发 `Transfer` 事件。
- `allowance(address owner, address spender)` ：返回授权花费者 `spender` 通过 `transferFrom` 代表所有者花费的剩余代币数量。默认情况下为零。当 `approve` 和 `transferFrom` 被调用时，值将改变。
- `approve(address spender, uint256 amount)` ：授权 `spender` 可以花费 `amount` 数量的代币，返回布尔值告知是否执行成功。触发 `Approval` 事件。
- `transferFrom(address from, address to, uint256 amount)` ：将 `amount` 数量的代币从 `from` 地址发送到 `to` 地址，返回布尔值告知是否执行成功。触发 `Transfer` 事件。

事件（定义中的 `indexed` 便于查找过滤）：

- `Transfer(address from, address to, uint256 value)` ：当代币被一个地址转移到另一个地址时触发。注意：转移的值可能是 0 。
- `Approval(address owner, address spender, uint256 value)` ：当代币所有者授权别人使用代币时触发，即调用 `approve` 方法。

#### 元数据

一般除了上述必须实现的函数外，还有一些别的方法：

- `name()` ：返回代币名称
- `symbol()` ：返回代币符号
- `decimals()` 返回代币小数点后位数

### ERC20

来看下 ERC20 代币具体是怎么写的。

同样，OpenZepplin 提供了现成的合约代码：

```solidity
import "@openzeppelin/contracts/token/ERC20/ERC20.sol";
```

这里贴一个GitHub源码链接 [OpenZepplin ERC20](https://github.com/OpenZeppelin/openzeppelin-contracts/blob/master/contracts/token/ERC20/ERC20.sol)

#### 函数概览













