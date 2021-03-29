<!-- @format -->

# ContractInstance

## 만드는 방법

- 프로그램마다 좀 다른데 보통 `abi`, `contract address`를 넣어 생성한다.
- `abi`는 인터페이스고, 주소는 실체!

## 만드는 이유

> Contract에 정의되어있는 메소드를 사용하기 위해

## 사용 방법 - cryptoViper

```js
 mounted() {
    getWeb3().then((res) => {
      this.web3 = res;
      this.contractInstance = new this.web3.eth.Contract(contractAbi, contractAddress);
      this.web3.eth.getAccounts().then((accounts) => {
        [this.account] = accounts;
        this.getVipers();
      }).catch((err) => {
        console.log(err, 'err!!');
      });
    });
  },
  methods: {
    buyViper() {
      this.isLoading = true;
      this.contractInstance.methods.buyViper().send({
        from: this.account,
        value: web3.toWei(0.02, 'ether'),
      }).then((receipt) => {
        this.addViperFromReceipt(receipt);
        this.isLoading = false;
      }).catch((err) => {
        console.log(err, 'err');
        this.isLoading = false;
      });
    },
```

- 보면 처음에 abi랑 주소로 인스턴스 생성
- 인스턴스 생성 후 컨트랙트 내부 메소드를 사용함
