# cosmosjs Tutorial
HackAtom 발표 'CosmosJS로 Kava의 USDX 결제연동하기'의 시연 코드입니다.

## Requirement
cosmosjs Tutorial requires Node.js >=8.0.0

## 튜토리얼 코드 설치방법
```
git clone https://github.com/cosmostation/cosmosjs-tutorial
npm install
```

## npm install Error
```
sudo npm install --unsafe-perm=true --allow-root 
```

## cosmosjs-tutorial/router/main.js
```
// line 1
const cosmosjs = require("@cosmostation/cosmosjs");

// line 3
const mnemonic = "";
const chainId = "kava-testnet-1.1";
const kava = cosmosjs.network("https://lcd-kava.cosmostation.io", chainId);
kava.setBech32MainPrefix("kava");
kava.setPath("m/44'/118'/0'/0/0");
const address = kava.getAddress(mnemonic);

res.render('index', {
	paymentAddress: address
})
```

## cosmosjs-tutorial/public/js/usdx.js
```
// line 2
console.log("Open a modal");
var getDepositBalanceId = setInterval(getDepositBalance, 3000);

function getDepositBalance() {
	console.log("Account balance checking...");

	// kava1qfwytl6p8hcn6pjtup787lsmzdthyfwc9k8h7x
	var settingsBalance = {
		"async": false,
		"crossDomain": true,
		"url": "https://lcd-kava.cosmostation.io/bank/balances/" + userAddress,
		"method": "GET"
	}

	$.ajax(settingsBalance).done(function (response) {
		if( !$.isArray(response) || !response.length ) {
			// 0 Balance
			console.log("No response");
			return;
		}

		if (response[0].denom == "ukva") {
			var amount = (response[0].amount * 0.000001).toFixed(6) * 1;

			// 사용가능 수량: amount
			console.log("amount: ", amount);

			if (amount > 0) {
				// 찾는거 그만두기
				clearInterval(getDepositBalanceId);

				// 결제 금액과 일치하는가?
				if (amount != 99) {
					console.log("결제 금액 일치하지 않음");
				} else {
					console.log("결제 금액 일치!");
				}

				// 결제 완료 화면으로 변경
				$(".song-modal-coin-info").hide();
				$(".song-modal-result").show();
			}
		}
	})
}
```

## Kava 전송: node_modules/@cosmostation/cosmosjs/example
```
// line 5
const mnemonic = "YOUR_MNEMONICS";
const chainId = "kava-testnet-1.1";
const kava = cosmosjs.network("https://lcd-kava.cosmostation.io", chainId);
kava.setBech32MainPrefix("kava");
const address = kava.getAddress(mnemonic);
```

## Kava faucet URL
- https://faucet.kava.io/

## 랜덤 니모닉 생성
- https://iancoleman.io/bip39/



