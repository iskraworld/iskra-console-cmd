# ISKRA Game Token(ERC20) Contract

## 개요
- `contracts/token/GameToken.sol` 파일은 ERC20 표준을 따르는 샘플 토큰 구현체로서, 이름(name), 심볼(symbol), 총 통화량(initial supply)을 배포 때 정할 수 있도록 구현된 토큰입니다.
- 배포시 초기 발행량이 정해지면, 추가 mint나 burn 기능이 불가능한 고정 통화량 토큰입니다.
- decimal은 18로 고정입니다.
- 초기 배포되면 owner가 총 통화량을 모두 소유하게 됩니다.
- IERC20, IKIP13, IKIP7, IERC165 표준 인터페이스를 따릅니다.
- `console-cmd`에서는 ERC20 토큰의 `배포`, `approve`, `transfer` 기능을 수행할 수 있습니다.

## 빌드
소스 코드를 다운 받습니다. npm, git 등은 설치되어 있다고 가정합니다.
```
git clone https://github.com/iskraworld/iskra-console-cmd.git
```
<br>
그런 후 의존성 모듈들을 설치합니다.

```
npm install
```
<br>
그리고 컴파일 합니다.

```
npx hardhat compile
```

## 지갑 준비
- 기본적으로 hardhat config에 배포 계정이 세팅되어 있어, 테스트 목적으로 배포할 땐 별도의 배포 계정을 준비할 필요가 없습니다.
- `--signer`, `--password` 옵션을 주면 hardhat config에 설정된 계정이 아닌, 지정한 계정으로 배포를 수행합니다. 사전에 wallet을 생성해야 가능합니다.
- hardhat node가 아닌 외부 블록체인(baobab, ethereum, goerli)에 배포하려면 기본 계정에 base coin(Klay 또는 Ether 등)이 준비되어 있어야 합니다.
- 별도의 지갑으로 배포하거나 트랜잭션을 수행하려면 지갑을 미리 준비하시고 `--signer` 옵션을 사용하시기 바랍니다.

```
npx hardhat wallet:add --name [name] --password [password]

ex)
npx hardhat wallet:add --name deployer --password 1234
wallet [deployer] is added

npx hardhat gametoken:deploy --name "GameMeso" --symbol GMM --supply 1000000000 --signer deployer --password 1234
```
- 보다 자세한 wallet 이용법은 [Wallet](wallet.md)를 참조하세요.

## 네트워크 선택
- 모든 커맨드에 네트워크 옵션(`--network`)을 줄 수 있습니다. 네트워크 옵션을 주지 않으면 hardhat node(local)에서 수행됩니다.
- 네트워크 설정은 `harhat.config.js`에 있습니다. 기본적으로 다음 네트워크를 지원합니다. 필요시 네트워크 정보를 추가하시기 바랍니다.
  - `baobab`: Klaytn testnet
  - `cypress`: Klaytn mainnet
  - `goerli`: Ethereum testnet
  - `ethereum`: Ethereum mainnet
- 예로 Baobab에서 수행하려면 아래처럼 옵션을 주면 됩니다.

```
npx hardhat gametoken:deploy --network baobab --name "GameMeso" --symbol GMM --supply 1000000000
```
- [주의] hardhat node는 매번 수행될 때마다 체인이 리셋(초기화) 됩니다. 따라서 커맨드 실행과 실행 사이에 연속성이 없기 때문에 배포한 컨트랙트를 새로운 커맨드로 실행할 수가 없습니다.

## 배포

다음 명령어로 Game token 컨트랙트를 배포할 수 있습니다.

```
npx hardhat gametoken:deploy [--signer [signer name] --password [password]] --name [name] --symbol [symbol] --supply [supply; 1 = 10^decimals]

ex)
npx hardhat gametoken:deploy --network baobab --name "GameMeso" --symbol GMM --supply 1000000000
============Args================
{
  name: 'GameMeso',
  symbol: 'GMM',
  supply: '1000000000',
  signer: undefined,
  password: undefined
}
================================
============TxResult============
Tx Success
[
  {
    transactionIndex: 1,
    blockNumber: 102257853,
    transactionHash: '0x02d51ad0d94fe33fc272ea4f18170ada3858ca01c42706c46dd9f7d169b52be2',
    address: '0xCf723d52d2b121d829209109903F7BF823F3a5C8',
    topics: [
      '0x8be0079c531659141344cd1fd0a4f28419497f9722a3daafe3b4186f6b6457e0',
      '0x0000000000000000000000000000000000000000000000000000000000000000',
      '0x000000000000000000000000c4417f73dac656337cecfee8c784130f08be4fa7'
    ],
    data: '0x',
    logIndex: 1,
    blockHash: '0x87990ec6bd533c93b2d6ae740596d44aa3db2198ab462eb83576415accaf7a2a'
  },
  {
    transactionIndex: 1,
    blockNumber: 102257853,
    transactionHash: '0x02d51ad0d94fe33fc272ea4f18170ada3858ca01c42706c46dd9f7d169b52be2',
    address: '0xCf723d52d2b121d829209109903F7BF823F3a5C8',
    topics: [
      '0xddf252ad1be2c89b69c2b068fc378daa952ba7f163c4a11628f55a4df523b3ef',
      '0x0000000000000000000000000000000000000000000000000000000000000000',
      '0x000000000000000000000000c4417f73dac656337cecfee8c784130f08be4fa7'
    ],
    data: '0x0000000000000000000000000000000000000000033b2e3c9fd0803ce8000000',
    logIndex: 2,
    blockHash: '0x87990ec6bd533c93b2d6ae740596d44aa3db2198ab462eb83576415accaf7a2a'
  }
]
================================
 Game token GameMeso was deployed to: 0xCf723d52d2b121d829209109903F7BF823F3a5C8
```

- `--network` 옵션으로 체인을 선택할 수 있습니다.
- `--supply` 옵션으로 총 통화량을 설정할 수 있습니다. 단위는 1이 10^18개 입니다. 위 예처럼 `1000000000`를 입력하면 10억개를 초기 발행합니다.
- 성공적으로 배포되면 위 처럼 배포된 컨트랙트 주소가 표시됩니다. 예에서는 `0x32DC9b60F80ac37Af2c8D9837ab90De3E6d23D1B`
- 또한 `~/.iskra-console/deployed/gametoken-address.json`에 주소가 남겨집니다.

## approve
- 스크립트로 ERC20의 approve 기능을 수행할 수 있습니다.
```
npx hardhat gametoken:approve [--signer [signer] --password [password]] --spender [spender] --amount [amount]

ex)
npx hardhat --network baobab gametoken:approve --spender 0xc4417F73DaC656337cEcfee8c784130f08be4FA7 --amount 1000
============Args================
{
  spender: '0xc4417F73DaC656337cEcfee8c784130f08be4FA7',
  amount: '1000',
  token: '',
  signer: undefined,
  password: undefined
}
================================
============TxResult============
Tx Success
[
  {
    transactionIndex: 0,
    blockNumber: 3969231,
    transactionHash: '0x06bee89298754efddaf609113720fdae1cde0b804155618907efe83b4b2a13b7',
    address: '0x32DC9b60F80ac37Af2c8D9837ab90De3E6d23D1B',
    topics: [
      '0x8c5be1e5ebec7d5bd14f71427d1e84f3dd0314c0f7b2291e5b200ac8c7c3b925',
      '0x00000000000000000000000058ee279901af7d9a0070be1976c25e6caf776590',
      '0x000000000000000000000000c4417f73dac656337cecfee8c784130f08be4fa7'
    ],
    data: '0x00000000000000000000000000000000000000000000003635c9adc5dea00000',
    logIndex: 0,
    blockHash: '0xfbfa6aad2aeef7d3d0ee96b2b90c72a605dac97819b6669f460b1075686c4a8e'
  }
]
================================
```
- `--network` 옵션으로 체인을 선택할 수 있습니다.
- `--token` 옵션으로 `approve`할 토큰을 설정할 수 있습니다. 생략하면 `~/.iskra-console/deployed/gametoken-address.json`에 기록된 토큰을 `approve` 합니다.
- `--amount` 옵션의 단위는 1=10^18개 입니다.

## transfer
- 스크립트로 ERC20의 transfer 기능을 수행할 수 있습니다.

```
npx hardhat gametoken:transfer [--signer [signer] --password [password]] --recipient [recipient] --amount [amount]

ex)
npx hardhat --network baobab gametoken:transfer --recipient 0xc4417F73DaC656337cEcfee8c784130f08be4FA7 --amount 1000
============Args================
{
  recipient: '0xc4417F73DaC656337cEcfee8c784130f08be4FA7',
  amount: '1000',
  token: '',
  signer: undefined,
  password: undefined
}
================================
============TxResult============
Tx Success
[
  {
    transactionIndex: 0,
    blockNumber: 3969416,
    transactionHash: '0x7bebcb2ede90aef16d33f90ddd60c4828d28dfc0333ad1a8f2b6ba62a3fd2a0d',
    address: '0x32DC9b60F80ac37Af2c8D9837ab90De3E6d23D1B',
    topics: [
      '0xddf252ad1be2c89b69c2b068fc378daa952ba7f163c4a11628f55a4df523b3ef',
      '0x00000000000000000000000058ee279901af7d9a0070be1976c25e6caf776590',
      '0x000000000000000000000000c4417f73dac656337cecfee8c784130f08be4fa7'
    ],
    data: '0x00000000000000000000000000000000000000000000003635c9adc5dea00000',
    logIndex: 0,
    blockHash: '0x46405303909fadf9797769c543dd5e20287bd301999dba23647a8ecd4be059d6'
  }
]
================================

```
- `--network` 옵션으로 체인을 선택할 수 있습니다.
- `--token` 옵션으로 `transfer`할 토큰을 설정할 수 있습니다. 생략하면 `~/.iskra-console/deployed/gametoken-address.json`에 기록된 토큰을 `approve` 합니다.
- `--amount` 옵션의 단위는 1=10^18개 입니다.
