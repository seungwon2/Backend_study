<!-- @format -->

# mintToken

## 정의

> Mint : 화폐 등을 주조, 발행, 새로 만들어내다

- mint Token : 토큰을 추가적으로 발행, 주조해내는 것이 가능한 토큰

## zepelin-solidity 기본 Mintable Token 코드

```js
pragma solidity ^0.4.18;

import "./StandardToken.sol";  // ERC20 을 따르는 기본 Token Contract
import "../../ownership/Ownable.sol";
// contract 의 owner 를 지정, 수정할 수 있게하는 역시 zeppelin-solidity 에 포함된 contract


contract MintableToken is StandardToken, Ownable {
/* 위에서 import 한 .sol 들에 포함된 StandardToken, Ownable Contract 를 다중상속하여
   그 특징들을 그대로 해당 MintableToken Contract 에서 사용,
   만약 MintableToken 을 또 다른 Contract 에서 상속하여 사용한다면
   MintableToken, StandardToken, Ownable Contract 들의 모든 특징을 상속받게 된다.
*/

  event Mint(address indexed to, uint256 amount);
  event MintFinished();
  // 선언해둔 이벤트를 호출함으로써 logging 용도나 web3.js 등을 통해 DApp 에서 callback 으로서 활용 가능

  bool public mintingFinished = false;
  /* 토큰 추가 발행하는 행위가 끝났는지 확인하기위한 Flag 변수,
     해당 변수가 false 라는건 아직 아직 계속 mint(추가발행) 가 가능한 상태라는 것
     아래 선언된 finishMinting() 를 contract owner 가 호출 함으로써
     이를 True 로 변경해 더이상 mint(추가발행) 불가능하게 할 수 있다.
  */

  modifier canMint() {
    require(!mintingFinished);
    _;
  } /*
    mintingFinished 를 체크하여 현재 mint 가능상태인지 체크하는 Modifier,
    아래처럼 function 선언시 canMint Modifier 를 붙여주면 mintingFinished 가 False 일 때만
    해당 함수가 실행되게 할 수 있다.  */


  /**
   * 실질적으로 token 추가 발행을 실행하는 함수, public 으로서 누구나 호출 가능하지만 onlyOwner modifier 를 통해
   * 해당 contract 의 owner 만 정상적으로 실행시킬 수 있다. canMint 로 mintingFinished 가 False 인지 체크
   * _to : 추가발행 될 토큰을 받을 account 의 address
   * _amount : 추가 발행할 Token 의 양
   */
  function mint(address _to, uint256 _amount) onlyOwner canMint public returns (bool) {
    totalSupply_ = totalSupply_.add(_amount); // Token 의 총 발행량을 _amount 만큼 증감
    balances[_to] = balances[_to].add(_amount);  // _to address 의 토큰 잔액을 _amount 만큼 증감
    Mint(_to, _amount);  // 위에서 선언한 Event를 호출함으로써 logging 혹은 DApp 에서 callback 으로 활용
    Transfer(address(0), _to, _amount);  // StandardToken 이 상속받은 ERC20Basic에 포함된 Event
    return true;  // 정상적으로 Mint 됐을 시 treu 를 리턴
  }

  // 더이상 Mint 가 불가능하도록 mintingFinished 를 True 로 변경, owner 만 호출가능
  // 한번 True 가 된 mintingFinished 를 다시 False 로 되돌리는 함수는 없음으로, 한번 끄면 다시는 추가발행 불가
  function finishMinting() onlyOwner canMint public returns (bool) {
    mintingFinished = true;
    MintFinished();  // event
    return true;
  }
}

```

## 참고할만한 링크

- [zepelin-solidity](https://github.com/OpenZeppelin/openzeppelin-contracts)

- [mintable token abi](https://github.com/seungwon2/non-fungible-token-abi)
