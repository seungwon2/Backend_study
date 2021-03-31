<!-- @format -->

# ERC721 contract

> 💡 컨트랙트에 대체 뭘 써야하는가..?

## 참고 링크

[클레이튼 docs](https://ko.docs.klaytn.com/smart-contract/sample-contracts/erc-721/1-erc721)

## 기본

- `ERC721`는 `IERC721` 및 `ERC165` 인터페이스를 구현한다.

## ERC721에서 제공하는 메소드

### constructor of ERC721 and \_INTERFACE_ID_ERC721

```js
    /*
     *     bytes4(keccak256('balanceOf(address)')) == 0x70a08231
     *     bytes4(keccak256('ownerOf(uint256)')) == 0x6352211e
     *     bytes4(keccak256('approve(address,uint256)')) == 0x095ea7b3
     *     bytes4(keccak256('getApproved(uint256)')) == 0x081812fc
     *     bytes4(keccak256('setApprovalForAll(address,bool)')) == 0xa22cb465
     *     bytes4(keccak256('isApprovedForAll(address,address)')) == 0xe985e9c
     *     bytes4(keccak256('transferFrom(address,address,uint256)')) == 0x23b872dd
     *     bytes4(keccak256('safeTransferFrom(address,address,uint256)')) == 0x42842e0e
     *     bytes4(keccak256('safeTransferFrom(address,address,uint256,bytes)')) == 0xb88d4fde
     *
     *     => 0x70a08231 ^ 0x6352211e ^ 0x095ea7b3 ^ 0x081812fc ^
     *        0xa22cb465 ^ 0xe985e9c ^ 0x23b872dd ^ 0x42842e0e ^ 0xb88d4fde == 0x80ac58cd
     */
    bytes4 private constant _INTERFACE_ID_ERC721 = 0x80ac58cd;

    constructor () public {
        // ERC165를 통한 ERC721의 확인을 위한 지원 인터페이스 등록
        _registerInterface(_INTERFACE_ID_ERC721);
    }
```

- `constructor`는 아래 `ERC721` 인터페이스로부터 구해진 4바이트 해시인 인터페이스 아이디를 등록한다.

## balanceOf

```js
    function balanceOf(address owner) public view returns (uint256) {
        require(owner != address(0), "ERC721: balance query for the zero address");

        return _ownedTokensCount[owner].current();
    }
```

- `ERC721`의 필수 메소드로 `owner`의 계정에 들어있는 `NFT`의 개수를 반환

```js
    // 소유자로부터 소유한 토큰의 개수로의 매핑
    mapping (address => Counters.Counter) private _ownedTokensCount;
```

- 이 함수를 돌리려면 `owner`가 `_ownedTokensCount`에서 유지하는 `counter` 객체로부터 현재 카운트를 받아서 반환해야 한다.

## \_mint

```js
    function _mint(address to, uint256 tokenId) internal {
        require(to != address(0), "ERC721: mint to the zero address");
        require(!_exists(tokenId), "ERC721: token already minted");

        _tokenOwner[tokenId] = to;
        _ownedTokensCount[to].increment();

        emit Transfer(address(0), to, tokenId);
    }
```

- `_mint`는 토큰의 일부가 아니고 컨트랙트 내부의 메소드이다.
- 토큰을 생성하기 위해서 필요한 컨트랙트 내부 메소드이다.
