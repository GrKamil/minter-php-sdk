<p align="center" background="black"><img src="minter-logo.svg" width="400"></p>

## About

This is a pure PHP SDK for working with <b>Minter</b> blockchain

* [Installation](#installing)
* [Minter Api](#using-minterapi)
	- [getBalance](#getbalance)
	- [getNonce](#getnonce)
	- [send](#send)
	- [getStatus](#getstatus)
	- [getValidators](#getvalidators)
	- [estimateCoinBuy](#estimatecoinbuy)
	- [estimateCoinSell](#estimatecoinsell)
	- [getCoinInfo](#getcoininfo)
	- [getBlock](#getblock)
	- [getEvents](#getevents)
	- [getTransaction](#gettransaction)
	- [getCandidate](#getcandidate)
	- [getCandidates](#getcandidates)
	- [estimateTxCommission](#estimatetxcommission)
	
* [Minter SDK](#using-minterapi)
	- [SendCoin](#example-5)
	- [SellCoinTx](#example-6)
	- [SellAllCoin](#example-7)
	- [BuyCoinTx](#example-8)
	- [CreateCoin](#example-9)
	- [DeclareCandidacy](#example-10)
	- [Delegate](#example-11)
	- [SetCandidateOn](#example-12)
	- [SetCandidateOff](#example-13)
	- [RedeemCheck](#example-14)
	- [Unbound](#example-15)
	- [MultiSend](#example-16)
	- [Get fee of transaction](#get-fee-of-transaction)
	- [Get hash of transaction](#get-hash-of-transaction)
	- [Decode Transaction](#decode-transaction)
	- [Minter Check](#create-minter-check)

## Installing

```bash
composer require minter/minter-php-sdk
```

## Using MinterAPI

You can get all valid responses and full documentation at [Minter Node Api](https://minter-go-node.readthedocs.io/en/latest/api.html)

Create MinterAPI instance

```php
use Minter\MinterAPI;

$nodeUrl = 'https://minter-node-1.testnet.minter.network:8841'; // example of a node url

$api = new MinterAPI($nodeUrl);
```

### getBalance

Returns coins list, balance and transaction count (for nonce) of an address.

``
getBalance(string $minterAddress): \stdClass
``

###### Example

```php
$api->getBalance('Mxfe60014a6e9ac91618f5d1cab3fd58cded61ee99')

// {"jsonrpc": "2.0", "id": "", "result": { "balance": { ... }, "transaction_count": "0"}}

```

### getNonce

Returns next transaction number (nonce) of an address.

``
getNonce(string $minterAddress): int
``

###### Example

```php
$api->getNonce('Mxfe60014a6e9ac91618f5d1cab3fd58cded61ee99')
```

### send

Returns the result of sending <b>signed</b> tx.

``
send(string $tx): \stdClass
``

###### Example

```php
$api->send('f873010101aae98a4d4e540000000000000094fe60014a6e9ac91618f5d1cab3fd58cded61ee99880de0b6b3a764000080801ca0ae0ee912484b9bf3bee785f4cbac118793799450e0de754667e2c18faa510301a04f1e4ed5fad4b489a1065dc1f5255b356ab9a2ce4b24dde35bcb9dc43aba019c')
```

### getStatus

Returns node status info.

``
getStatus(): \stdClass
``

### getValidators

Returns list of active validators.

``
getValidators(): \stdClass
``

### estimateCoinBuy

Return estimate of buy coin transaction.

``
estimateCoinBuy(string $coinToSell, string $valueToBuy, string $coinToBuy): \stdClass
``

### estimateCoinSell

Return estimate of sell coin transaction.

``
estimateCoinSell(string $coinToSell, string $valueToSell, string $coinToBuy): \stdClass
``

### getCoinInfo

Returns information about coin.
Note: this method does not return information about base coins (MNT and BIP).

``
getCoinInfo(string $coin): \stdClass
``

### getBlock

Returns block data at given height.

``
getBlock(int $height): \stdClass
``

### getEvents

Returns events at given height.

``
getEvents(int $height): \stdClass
``

### getTransaction

Returns transaction info.

``
getTransaction(string $hash): \stdClass
``

### getCandidate

Returns candidate’s info by provided public_key. It will respond with 404 code if candidate is not found.

``
getCandidate(string $publicKey): \stdClass
``

### getCandidates

Returns list of candidates.

``
getCandidates(): \stdClass
``

### estimateTxCommission

Return estimate of transaction.

``
estimateTxCommission(string $tx): \stdClass
``


## Using MinterSDK

### Sign transaction

Returns a signed tx.

###### Example

* Sign the <b>SendCoin</b> transaction

```php
use Minter\SDK\MinterTx;
use Minter\SDK\MinterCoins\MinterSendCoinTx;

$tx = new MinterTx([
    'nonce' => $nonce,
    'gasPrice' => 1,
    'gasCoin' => 'MNT',
    'type' => MinterSendCoinTx::TYPE,
    'data' => [
        'coin' => 'MTN',
        'to' => 'Mxfe60014a6e9ac91618f5d1cab3fd58cded61ee99',
        'value' => '10'
    ],
    'payload' => '',
    'serviceData' => '',
    'signatureType' => MinterTx::SIGNATURE_SINGLE_TYPE // or SIGNATURE_MULTI_TYPE
]);

$tx->sign('your private key')
```

###### Example
* Sign the <b>SellCoin</b> transaction

```php
use Minter\SDK\MinterTx;
use Minter\SDK\MinterCoins\MinterSellCoinTx;

$tx = new MinterTx([
    'nonce' => $nonce,
    'gasPrice' => 1,
    'gasCoin' => 'MNT',
    'type' => MinterSellCoinTx::TYPE,
    'data' => [
         'coinToSell' => 'MNT',
         'valueToSell' => '1',
         'coinToBuy' => 'TEST',
         'minimumValueToBuy' => 1
    ],
    'payload' => '',
    'serviceData' => '',
    'signatureType' => MinterTx::SIGNATURE_SINGLE_TYPE // or SIGNATURE_MULTI_TYPE
]);

$tx->sign('your private key')
```

###### Example
* Sign the <b>SellAllCoin</b> transaction

```php
use Minter\SDK\MinterTx;
use Minter\SDK\MinterCoins\MinterSellAllCoinTx;

$tx = new MinterTx([
    'nonce' => $nonce,
    'gasPrice' => 1,
    'gasCoin' => 'MNT',
    'type' => MinterSellAllCoinTx::TYPE,
    'data' => [
         'coinToSell' => 'TEST',
         'coinToBuy' => 'MNT',
         'minimumValueToBuy' => 1
    ],
    'payload' => '',
    'serviceData' => '',
    'signatureType' => MinterTx::SIGNATURE_SINGLE_TYPE // or SIGNATURE_MULTI_TYPE
]);

$tx->sign('your private key')
```

###### Example
* Sign the <b>BuyCoin</b> transaction

```php
use Minter\SDK\MinterTx;
use Minter\SDK\MinterCoins\MinterBuyCoinTx;

$tx = new MinterTx([
    'nonce' => $nonce,
    'gasPrice' => 1,
    'gasCoin' => 'MNT',
    'type' => MinterBuyCoinTx::TYPE,
    'data' => [
         'coinToBuy' => 'MNT',
         'valueToBuy' => '1',
         'coinToSell' => 'TEST',
         'minimumValueToBuy' => 1
    ],
    'payload' => '',
    'serviceData' => '',
    'signatureType' => MinterTx::SIGNATURE_SINGLE_TYPE // or SIGNATURE_MULTI_TYPE
]);

$tx->sign('your private key')
```

###### Example
* Sign the <b>CreateCoin</b> transaction

```php
use Minter\SDK\MinterTx;
use Minter\SDK\MinterCoins\MinterCreateCoinTx;

$tx = new MinterTx([
    'nonce' => $nonce,
    'gasPrice' => 1,
    'gasCoin' => 'MNT',
    'type' => MinterCreateCoinTx::TYPE,
    'data' => [
        'name' => 'TEST COIN',
        'symbol' => 'TEST',
        'initialAmount' => '100',
        'initialReserve' => '10',
        'crr' => 10
    ],
    'payload' => '',
    'serviceData' => '',
    'signatureType' => MinterTx::SIGNATURE_SINGLE_TYPE // or SIGNATURE_MULTI_TYPE
]);

$tx->sign('your private key')
```

###### Example
* Sign the <b>DeclareCandidacy</b> transaction

```php
use Minter\SDK\MinterTx;
use Minter\SDK\MinterCoins\MinterDeclareCandidacyTx;

$tx = new MinterTx([
    'nonce' => $nonce,
    'gasPrice' => 1,
    'gasCoin' => 'MNT',
    'type' => MinterDeclareCandidacyTx::TYPE,
    'data' => [
        'address' => 'Mxa7bc33954f1ce855ed1a8c768fdd32ed927def47',
        'pubkey' => 'Mp023853f15fc1b1073ad7a1a0d4490a3b1fadfac00f36039b6651bc4c7f52ba9c02',
        'commission' => 10,
        'stake' => '5'
    ],
    'payload' => '',
    'serviceData' => '',
    'signatureType' => MinterTx::SIGNATURE_SINGLE_TYPE // or SIGNATURE_MULTI_TYPE
]);

$tx->sign('your private key')
```

###### Example
* Sign the <b>Delegate</b> transaction

```php
use Minter\SDK\MinterTx;
use Minter\SDK\MinterCoins\MinterDelegateTx;

$tx = new MinterTx([
    'nonce' => $nonce,
    'gasPrice' => 1,
    'gasCoin' => 'MNT',
    'type' => MinterDelegateTx::TYPE,
    'data' => [
        'pubkey' => 'Mp0eb98ea04ae466d8d38f490db3c99b3996a90e24243952ce9822c6dc1e2c1a43',
        'coin' => 'MNT',
        'stake' => '5'
    ],
    'payload' => '',
    'serviceData' => '',
    'signatureType' => MinterTx::SIGNATURE_SINGLE_TYPE // or SIGNATURE_MULTI_TYPE
]);

$tx->sign('your private key')
```

###### Example
* Sign the <b>SetCandidateOn</b> transaction

```php
use Minter\SDK\MinterTx;
use Minter\SDK\MinterCoins\MinterSetCandidateOnTx;

$tx = new MinterTx([
    'nonce' => $nonce,
    'gasPrice' => 1,
    'gasCoin' => 'MNT',
    'type' => MinterSetCandidateOnTx::TYPE,
    'data' => [
        'pubkey' => 'Mp0eb98ea04ae466d8d38f490db3c99b3996a90e24243952ce9822c6dc1e2c1a43'
    ],
    'payload' => '',
    'serviceData' => '',
    'signatureType' => MinterTx::SIGNATURE_SINGLE_TYPE // or SIGNATURE_MULTI_TYPE
]);

$tx->sign('your private key')
```

###### Example
* Sign the <b>SetCandidateOff</b> transaction

```php
use Minter\SDK\MinterTx;
use Minter\SDK\MinterCoins\MinterSetCandidateOffTx;

$tx = new MinterTx([
    'nonce' => $nonce,
    'gasPrice' => 1,
    'gasCoin' => 'MNT',
    'type' => MinterSetCandidateOffTx::TYPE,
    'data' => [
        'pubkey' => 'Mp0eb98ea04ae466d8d38f490db3c99b3996a90e24243952ce9822c6dc1e2c1a43'
    ],
    'payload' => '',
    'serviceData' => '',
    'signatureType' => MinterTx::SIGNATURE_SINGLE_TYPE // or SIGNATURE_MULTI_TYPE
]);

$tx->sign('your private key')
```

###### Example
* Sign the <b>RedeemCheck</b> transaction

```php
use Minter\SDK\MinterTx;
use Minter\SDK\MinterCoins\MinterRedeemCheckTx;

$tx = new MinterTx([
    'nonce' => $nonce,
    'gasPrice' => 1,
    'gasCoin' => 'MNT',
    'type' => MinterRedeemCheckTx::TYPE,
    'data' => [
        'check' => 'your check',
        'proof' => 'created by MinterCheck proof'
    ],
    'payload' => '',
    'serviceData' => '',
    'signatureType' => MinterTx::SIGNATURE_SINGLE_TYPE // or SIGNATURE_MULTI_TYPE
]);

$tx->sign('your private key')
```

###### Example
* Sign the <b>Unbound</b> transaction

```php
use Minter\SDK\MinterTx;
use Minter\SDK\MinterCoins\MinterUnboundTx;

$tx = new MinterTx([
    'nonce' => $nonce,
    'gasPrice' => 1,
    'gasCoin' => 'MNT',
    'type' => MinterUnboundTx::TYPE,
    'data' => [
        'pubkey' => 'Mp....',
        'coin' => 'MNT',
        'value' => '1'
    ],
    'payload' => '',
    'serviceData' => '',
    'signatureType' => MinterTx::SIGNATURE_SINGLE_TYPE // or SIGNATURE_MULTI_TYPE
]);

$tx->sign('your private key')
```

###### Example
* Sign the <b>MultiSend</b> transaction

```php
use Minter\SDK\MinterTx;
use Minter\SDK\MinterCoins\MinterMultiSendTx;

$tx = new MinterTx([
    'nonce' => $nonce,
    'gasPrice' => 1,
    'gasCoin' => 'MNT',
    'type' => MinterMultiSendTx::TYPE,
    'data' => [
        'list' => [
            [
                'coin' => 'MTN',
                'to' => 'Mxfe60014a6e9ac91618f5d1cab3fd58cded61ee99',
                'value' => '10'
            ], [
                'coin' => 'MTN',
                'to' => 'Mxfe60014a6e9ac91618f5d1cab3fd58cded61ee92',
                'value' => '15'
            ]
        ]
    ],
    'payload' => '',
    'serviceData' => '',
    'signatureType' => MinterTx::SIGNATURE_SINGLE_TYPE // or SIGNATURE_MULTI_TYPE
]);

$tx->sign('your private key')
```

### Get fee of transaction

* Calculate fee of transaction. You can get fee AFTER signing or decoding transaction.
```php
use Minter\SDK\MinterTx;

$tx = new MinterTx([....]);
$sign = $tx->sign('your private key');

$tx->getFee();
```

### Get hash of transaction

* Get hash of encoded transaction
```php
use Minter\SDK\MinterTx;

$tx = new MinterTx([....]);
$sign = $tx->sign('your private key');

$hash = $tx->getHash();
```

* Get hash of decoded transaction
```php
use Minter\SDK\MinterTx;

$tx = new MinterTx('Mx....');

$hash = $tx->getHash();
```

### Decode transaction

Returns an array with transaction data.

###### Example

* Decode transaction

```php
use Minter\SDK\MinterTx;

$tx = new MinterTx('string tx');

// $tx->from, $tx->data, $tx->nonce ...

```

### Create Minter Check

###### Example

* Create check

```php
use Minter\SDK\MinterCheck;

$check = new MinterCheck([
    'nonce' => $nonce,
    'dueBlock' => 999999,
    'coin' => 'MNT',
    'value' => '10'
], 'your pass phrase');

echo $check->sign('your private key here'); 

// Mc.......

```

* Create proof

```php
use Minter\SDK\MinterCheck;

$check = new MinterCheck('your Minter address here', 'your pass phrase');

echo $check->createProof(); 
```