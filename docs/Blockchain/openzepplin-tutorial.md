# OpenZepplin开发教程

1. 新建项目，并安装依赖

```
forge init hello_coin
cd hello_coin
forge install OpenZeppelin/openzeppelin-contracts
```

2. 打开[OpenZeppelin](https://wizard.openzeppelin.com/)，选择ERC20模板，并编写如下合约：

- MyCoin.sol

```
// SPDX-License-Identifier: MIT
// Compatible with OpenZeppelin Contracts ^5.6.0
pragma solidity ^0.8.27;

import {Ownable} from "@openzeppelin/contracts/access/Ownable.sol";
import {ERC20} from "@openzeppelin/contracts/token/ERC20/ERC20.sol";

contract MyCoin is ERC20, Ownable {
    event Mint(uint256 indexed amount);
    event Burn(uint256 indexed amount);

    string public _name = "MyCoin";
    string public _symbol = "MYC";

    constructor(address initialOwner)
        ERC20(_name, _symbol)
        Ownable(initialOwner)
    {}

    function mint(uint256 _amount) public onlyOwner {
        _mint(msg.sender, _amount);
        emit Mint(_amount);
    }

    function burn(uint256 _amount) public onlyOwner {
        _burn(msg.sender, _amount);
        emit Burn(_amount);
    }
}
```

3. 编写测试代码：

- MyCoin.t.sol

```
pragma solidity ^0.8.27;

import "forge-std/Test.sol";
import { MyCoin } from "../src/MyCoin.sol";

contract MyCoinTest is Test {
    MyCoin public myc;

    address owner = vm.addr(1);
    address user = vm.addr(2);

    function setUp() public {
        myc = new MyCoin(owner);
        vm.deal(owner, 10 ether);
    }
    
    function testSuccessIfOwnerMint() public {
        vm.startPrank(owner);
        myc.mint(10 ether);
        vm.stopPrank();
        assertEq(myc.balanceOf(owner), 10 ether);
    }

    function testRevertIfUserMint() public {
        vm.startPrank(user);
        vm.expectRevert();
        myc.mint(10 ether);
        vm.stopPrank();
        assertEq(myc.balanceOf(user), 0 ether);
    }

    function testSuccessIfOwnerBurn() public {
        vm.startPrank(owner);
        myc.mint(10 ether);
        vm.stopPrank();
        assertEq(myc.balanceOf(owner), 10 ether);

        vm.startPrank(owner);
        myc.burn(5 ether);
        vm.stopPrank();
        assertEq(myc.balanceOf(owner), 5 ether);
    }

    function testRevertIfUserBurn() public {
        vm.startPrank(owner);
        myc.mint(10 ether);
        vm.stopPrank();
        assertEq(myc.balanceOf(owner), 10 ether);

        vm.startPrank(user);
        vm.expectRevert();
        myc.burn(5 ether);
        vm.stopPrank();
        assertEq(myc.balanceOf(owner), 10 ether);
        assertEq(myc.balanceOf(user), 0 ether);
    }
}
```

测试命令：

```
# 编译合约
forge compile
# 测试指定函数
forge test --mt testSuccessIfOwnerMint
# 测试函数并显示具体过程
forge test --mt testSuccessIfOwnerMint -vvvvv
# 查看测试覆盖率
forge coverage
```

4. 启动`anvil`，并在项目目录新建`.env`文件，选择两对公钥和私钥填入如下环境变量，其中`CONTRACT_ADDRESS`需要部署合约后才能生成：

```
OWNER_PRIVATE_KEY=0xac0974bec39a17e36ba4a6b4d238ff944bacb478cbed5efcae784d7bf4f2ff80
OWNER_ADDRESS=0xf39Fd6e51aad88F6F4ce6aB8827279cffFb92266

USER_PRIVATE_KEY=0x59c6995e998f97a5a0044966f0945389dc9e86dae88c7a8412f4603b6b78690d
USER_ADDRESS=0x70997970C51812dc3A010C7d01b50e0d17dc79C8

CONTRACT_ADDRESS=0x5FbDB2315678afecb367f032d93F642f64180aa3
```

5. 部署合约

```
# 使环境变量生效
source .env
# 部署合约
forge create --private-key ${OWNER_PRIVATE_KEY} src/MyCoin.sol:MyCoin --constructor-args ${OWNER_ADDRESS}
# 将gas费转成十进制
cast to-dec 0x13b35d
# 加上--broadcast才会生成合约地址
forge create --private-key ${OWNER_PRIVATE_KEY} --broadcast src/MyCoin.sol:MyCoin --constructor-args ${OWNER_ADDRESS}
```

6. 合约交互命令，执行view函数用call，其他函数用send。这里数量单位的转换器使用[Simple Unit Converter](https://eth-converter.com/)

```
# owner mint 100 ether
cast send ${CONTRACT_ADDRESS} "mint(uint256)" 100000000000000000000 --private-key ${OWNER_PRIVATE_KEY}
cast call ${CONTRACT_ADDRESS} "balanceOf(address)" ${OWNER_ADDRESS}
# owner burn 50 ether
cast send ${CONTRACT_ADDRESS} "burn(uint256)" 50000000000000000000 --private-key ${OWNER_PRIVATE_KEY}
cast call ${CONTRACT_ADDRESS} "balanceOf(address)" ${OWNER_ADDRESS}
# owner transfer 10 to user
cast send ${CONTRACT_ADDRESS} "transfer(address, uint256)" ${USER_ADDRESS} 10000000000000000000 --private-key ${OWNER_PRIVATE_KEY}
cast call ${CONTRACT_ADDRESS} "balanceOf(address)" ${USER_ADDRESS}
# user burn failed
cast send ${CONTRACT_ADDRESS} "burn(uint256)" 50000000000000000000 --private-key ${USER_PRIVATE_KEY}
# 查看错误信息
forge selectors find 0x118cdaa7
```