# Onion Monero Blockchain Explorer

Currently available Monero blockchain explorers have several limitations which are of 
special importance to privacy-oriented users:

 - they use JavaScript,
 - have images which might be used for [cookieless tracking](http://lucb1e.com/rp/cookielesscookies/),
 - track users activates through google analytics,
 - are closed sourced,
 - are not available as hidden services,
 - do not support Monero testnet,
 - have limited JSON API.


In this example, these limitations are addressed by development of
an Onion Monero Blockchain Explorer. The example not only shows how to use 
Monero C++ libraries, but also demonstrates how to use:

 - [crow](https://github.com/ipkn/crow) - C++ micro web framework 
 - [mstch](https://github.com/no1msd/mstch) - C++ {{mustache}} templates
 - [json](https://github.com/nlohmann/json) - JSON for Modern C++
 - [fmt](https://github.com/fmtlib/fmt) - Small, safe and fast string formatting library

## Addresses

Tor users:
 
 - [http://dvwae436pd7nt4bc.onion](http://dvwae436pd7nt4bc.onion)
  
Clearnet versions:

 - [http://139.162.32.245:8081/](http://139.162.32.245:8081/) - bleeding edge version, no https.
 - [https://xmrchain.net/](https://xmrchain.net/) - https enabled, most popular and very stable.
 - [https://monerohash.com/explorer/](https://monerohash.com/explorer/) - nice looking one, https enabled.
 - [http://explore.MoneroWorld.com](http://explore.moneroworld.com) - same as the second one.
 - [https://explorer.xmr.my/](https://explorer.xmr.my/) - https enabled.
 - [https://explorer.monero-otc.com/](https://explorer.monero-otc.com/) - https enabled.
  
Clearnet testnet Monero version:

 - [http://139.162.32.245:8082/](http://139.162.32.245:8082/) - bleeding edge version, no https. 
 - [https://testnet.xmrchain.com/](https://testnet.xmrchain.com/) - https enabled.


i2p users (main Monero network) - down for now:

 - [http://monerotools.i2p](http://monerotools.i2p)

 
## Onion Monero Blockchain Explorer features

The key features of the Onion Monero Blockchain Explorer are:

 - no javascript, no cookies, no web analytics trackers, no images,
 - open sourced,
 - made fully in C++,
 - the only explorer showing encrypted payments ID,
 - the only explorer showing ring signatures,
 - the only explorer showing transaction extra field,
 - the only explorer showing public components of Monero addresses,
 - the only explorer that can show which outputs and mixins belong to the given Monero address and viewkey,
 - the only explorer that can be used to prove that you send Monero to someone,
 - the only explorer showing detailed information about mixins, such as, mixins'
 age, timescale, mixin of mixins,
 - the only explorer showing number of amount output indices,
 - the only explorer supporting Monero testnet network,
 - the only explorer providing tx checker and pusher for online pushing of transactions,
 - the only explorer able to estimate possible spendings based on address and viewkey.


## Compilation on Ubuntu 16.04

##### Compile latest Monero

Download and compile recent Monero release into your home folder:

```bash
# first install monero dependecines
sudo apt update

sudo apt install git build-essential cmake libboost-all-dev miniupnpc libunbound-dev graphviz doxygen libunwind8-dev pkg-config libssl-dev libcurl4-openssl-dev libgtest-dev

# go to home folder 
cd ~

git clone https://github.com/monero-project/monero

cd monero/

# checkout last monero version
git checkout -b v0.10.3.1

make
```

##### Compile and run the explorer
Once the Monero is compiles, the explorer can be downloaded and compiled
as follows:

```bash
# go to home folder if still in ~/monero
cd ~
# download the source code
git clone https://github.com/moneroexamples/onion-monero-blockchain-explorer.git

# enter the downloaded sourced code folder
cd onion-monero-blockchain-explorer

# make a build folder and enter it
mkdir build && cd build

# create the makefile
cmake ..

# compile
make
```

When compilation finishes executable `xmrblocks` should be created. Before running
please make sure that  `~/Downloads` folder exists and is writable. 
Time zone library that explorer is using, puts there 
its database of time zone offsets

To run it:
```
./xmrblocks
```

By default it will look for blockchain in its default location i.e., `~/.bitmonero/lmdb`.
You can use `--bc-path` option if its in different location. 
Example output:

```bash
[mwo@arch onion-monero-blockchain-explorer]$ ./xmrblocks
2016-May-28 10:04:49.160280 Blockchain initialized. last block: 1056761, d0.h0.m12.s47 time ago, current difficulty: 1517857750
(2016-05-28 02:04:49) [INFO    ] Crow/0.1 server is running, local port 8081
```

Go to your browser: http://127.0.0.1:8081

## The explorer's command line options

```
./xmrblocks -h
xmrblocks, start Onion Monero Blockchain Explorer:
  -h [ --help ] [=arg(=1)] (=0)         produce help message
  -t [ --testnet ] [=arg(=1)] (=0)      use testnet blockchain
  --enable-pusher [=arg(=1)] (=0)       enable pushing signed tx
  --enable-mixin-details [=arg(=1)] (=0)
                                        enable mixin details for key images, 
                                        e.g., timescale, mixin of mixins, in tx
                                        context
  --enable-key-image-checker [=arg(=1)] (=0)
                                        enable key images file checker
  --enable-output-key-checker [=arg(=1)] (=0)
                                        enable outputs key file checker
  --enable-mempool-cache arg (=1)       enable caching txs in the mempool
  --enable-json-api arg (=1)            enable JSON REST api
  --enable-tx-cache [=arg(=1)] (=0)     enable caching of tx details
  --show-cache-times [=arg(=1)] (=0)    show times of getting data from cache 
                                        vs no cache
  --enable-block-cache [=arg(=1)] (=0)  enable caching of block details
  --enable-autorefresh-option [=arg(=1)] (=0)
                                        enable users to have the index page on 
                                        autorefresh
  -p [ --port ] arg (=8081)             default port
  --testnet-url arg                     you can specifiy testnet url, if you 
                                        run it on mainet. link will show on 
                                        front page to testnet explorer
  --mainnet-url arg                     you can specifiy mainnet url, if you 
                                        run it on testnet. link will show on 
                                        front page to mainnet explorer
  --no-blocks-on-index arg (=10)        number of last blocks to be shown on 
                                        index page
  -b [ --bc-path ] arg                  path to lmdb blockchain
  --ssl-crt-file arg                    A path to crt file for ssl (https) 
                                        functionality
  --ssl-key-file arg                    A path to key file for ssl (https) 
                                        functionality
  -d [ --deamon-url ] arg (=http:://127.0.0.1:18081)
                                        monero address string
```

Example usage, defined as bash aliases.

```bash
# for mainnet explorer
alias xmrblocksmainnet='~/onion-monero-blockchain-explorer/build/xmrblocks    --port 8081 --no-blocks-on-index 49 --testnet-url "http://139.162.32.245:8082" --enable-block-cache=1 --enable-tx-cache=1 --enable-mempool-cache=1 --show-cache-times=1 --enable-pusher'

# for testnet explorer
alias xmrblockstestnet='~/onion-monero-blockchain-explorer/build/xmrblocks -t --port 8082 --no-blocks-on-index 24 --mainnet-url "http://139.162.32.245:8081" --enable-block-cache=1 --enable-tx-cache=1 --enable-mempool-cache=1 --show-cache-times=1 --enable-pusher'
```

These are explorer commands used for http://139.162.32.245:8081/ and http://139.162.32.245:8082/, respectively.

## Enable SSL (https)

By default, the explorer does not use ssl. But it has such a functionality. 

As an example, you can generate your own ssl certificates as follows:

```bash
cd /tmp # example folder
openssl genrsa -out server.key 1024
openssl req -new -key server.key -out server.csr
openssl x509 -req -days 3650 -in server.csr -signkey server.key -out server.crt
```

Having the `crt` and `key` files, run `xmrblocks` in the following way:

```bash
./xmrblocks --ssl-crt-file=/tmp/server.crt --ssl-key-file=/tmp/server.key 
```

Note: Because we generated our own certificate, modern browsers will complain
about it as they cant verify the signatures against any third party. So probably
for any practical use need to have properly issued ssl certificates. 

## JSON API

The explorer has JSON api. For the API, it uses conventions defined by [JSend](https://labs.omniti.com/labs/jsend).

#### api/transaction/<tx_hash>

```bash
curl  -w "\n" -X GET "http://139.162.32.245:8081/api/transaction/6093260dbe79fd6277694d14789dc8718f1bd54457df8bab338c2efa3bb0f03d"
```

Partial results shown:

```json
{
  "data": {
    "block_height": 1268252,
    "coinbase": false,
    "confirmations": 1,
    "current_height": 1268253,
    "extra": "01be23e277aed6b5f41f66b05244bf994c13108347366ec678ae16657f0fc3a22b",
    "inputs": [
      {
        "amount": 0,
        "key_image": "67838fd0ffd79f13e735830d3ec60412aed59e53e1f997feb6f73d088b949611",
        "mixins": [
          {
            "block_no": 1238623,
            "public_key": "0a5b853c55303c10e1326acfb085b9e246e088b1ccac7e37f7a810d46a28a914"
          },
          {
            "block_no": 1246942,
            "public_key": "527cf86f5abbfb006c970f7c6eb40493786d4751306f8985c6a43f98a88c0dff"
          }
        ]
      }
    ],
    "mixin": 9,
    "outputs": [
      {
        "amount": 0,
        "public_key": "525779873776e4a42f517fd79b72e7c31c3ba03e730fc32287f6414fb702c1d7"
      },
      {
        "amount": 0,
        "public_key": "e25f00fceb77af841d780b68647618812695b4ca6ebe338faba6e077f758ac30"
      }
    ],
    "payment_id": "",
    "payment_id8": "",
    "rct_type": 1,
    "timestamp": 1489753456,
    "timestamp_utc": "2017-03-17 12:24:16",
    "tx_fee": 12517785574,
    "tx_hash": "6093260dbe79fd6277694d14789dc8718f1bd54457df8bab338c2efa3bb0f03d",
    "tx_size": 13323,
    "tx_version": 2,
    "xmr_inputs": 0,
    "xmr_outputs": 0
  },
  "status": "success"
}
```

#### api/transactions

Transactions in last 25 blocks


```bash
curl  -w "\n" -X GET "http://139.162.32.245:8081/api/transactions"
```

Partial results shown:

```json
{
  "data": {
    "blocks": [
      {
        "age": "33:16:49:53",
        "height": 1268252,
        "size": 105390000000000000,
        "timestamp": 1489753456,
        "timestamp_utc": "2017-03-17 12:24:16",
        "txs": [
          {
            "coinbase": true,
            "mixin": 0,
            "outputs": 8491554678365,
            "rct_type": 0,
            "tx_fee": 0,
            "tx_hash": "7c4286f64544568265bb5418df84ae69afaa3567749210e46f8340c247f4803f",
            "tx_size": 151000000000000,
            "tx_version": 2
          },
          {
            "coinbase": false,
            "mixin": 5,
            "outputs": 0,
            "rct_type": 2,
            "tx_fee": 17882516700,
            "tx_hash": "2bfbccb918ee5f050808dd040ce03943b7315b81788e9cdee59cf86b557ba48c",
            "tx_size": 19586000000000000,
            "tx_version": 2
          }
        ]
      }
    ],
    "limit": 25,
    "page": 0
  },
  "status": "success"
}
```

#### api/transactions?page=<page_no>&limit=<tx_per_page>


```bash
curl  -w "\n" -X GET "http://139.162.32.245:8081/api/transactions?page=2&limit=10"
```

Result analogical to the one above.

#### api/block/<block_number|block_hash>


```bash
curl  -w "\n" -X GET "http://139.162.32.245:8081/api/block/1293257"
```

Partial results shown:

```json
{
  "data": {
    "block_height": 1293257,
    "block_reward": 0,
    "current_height": 1293264,
    "hash": "9ef6bb8f9b8bd253fc6390e5c2cdc45c8ee99fad16447437108bf301fe6bd6e1",
    "size": 141244,
    "timestamp": 1492761974,
    "timestamp_utc": "2017-04-21 08:06:14",
    "txs": [
      {
        "coinbase": true,
        "extra": "018ae9560eb85d5ebd22d3beaed55c21d469eab430c5e3cac61b3fe2f5ad156770020800000001a9030800",
        "mixin": 0,
        "payment_id": "",
        "payment_id8": "",
        "rct_type": 0,
        "tx_fee": 0,
        "tx_hash": "3ff71b65bec34c9261e01a856e6a03594cf0472acf6b77db3f17ebd18eaa30bf",
        "tx_size": 95,
        "tx_version": 2,
        "xmr_inputs": 0,
        "xmr_outputs": 8025365394426
      }
    ]
  },
  "status": "success"
}
```


#### api/mempool


```bash
curl  -w "\n" -X GET "http://139.162.32.245:8081/api/mempool"
```

Partial results shown:

```json
{
  "data": [
    {
      "coinbase": false,
      "extra": "02210001c32d313b74a859b904079c69dbc04ea6e37eddcf4aeb34e9400cc12831da5401b34082a9ff7476fe29a19fa6a1735a9c59db226b9ddcf715928aa71625b13062",
      "mixin": 7,
      "payment_id": "01c32d313b74a859b904079c69dbc04ea6e37eddcf4aeb34e9400cc12831da54",
      "payment_id8": "",
      "rct_type": 1,
      "timestamp": 1492763220,
      "timestamp_utc": "2017-04-21 08:27:00",
      "tx_fee": 4083040000,
      "tx_hash": "6751e0029558fdc4ab4528896529e32b2864c6ad43c5d8838c8ebe156ada0514",
      "tx_size": 13224,
      "tx_version": 2,
      "xmr_inputs": 0,
      "xmr_outputs": 0
    }
  ],
  "status": "success"
}
```

#### api/search/<block_number|tx_hash|block_hash>

```bash
curl  -w "\n" -X GET "http://139.162.32.245:8081/api/search/1293669"
```

Partial results shown:

```json
{
  "data": {
    "block_height": 1293669,
    "current_height": 1293670,
    "hash": "5d55b8fabf85b0b4c959d66ad509eb92ddfe5c2b0e84e1760abcb090195c1913",
    "size": 118026,
    "timestamp": 1492815321,
    "timestamp_utc": "2017-04-21 22:55:21",
    "title": "block",
    "txs": [
      {
        "coinbase": true,
        "extra": "01cb7fda09033a5fa06dc601b9295ef3790397cf3c645e958e34cf7ab699d2f5230208000000027f030200",
        "mixin": 0,
        "payment_id": "",
        "payment_id8": "",
        "rct_type": 0,
        "tx_fee": 0,
        "tx_hash": "479ba432f5c88736b438dd4446a11a13046a752d469f7828151f5c5b86be4e9a",
        "tx_size": 95,
        "tx_version": 2,
        "xmr_inputs": 0,
        "xmr_outputs": 7992697599717
      }
    ]
  },
  "status": "success"
}
```

#### api/outputs?txhash=<tx_hash>&address=<address>&viewkey=<viewkey>&txprove=<0|1>

For `txprove=0` we check which outputs belong to given address and corresponding viewkey.
For `txprove=1` we use to prove to the recipient that we sent them founds.
For this, we use recipient's address and our tx private key as a viewkey value,
 i.e., `viewkey=<tx_private_key>` 

Checking outputs:

```bash
# we use here official Monero project's donation address as an example
curl  -w "\n" -X GET "http://139.162.32.245:8081/api/outputs?txhash=17049bc5f2d9fbca1ce8dae443bbbbed2fc02f1ee003ffdd0571996905faa831&address=44AFFq5kSiGBoZ4NMDwYtN18obc8AemS33DBLWs3H7otXft3XjrpDtQGv7SqSsaBYBb98uNbr2VBBEt7f2wfn3RVGQBEP3A&viewkey=f359631075708155cc3d92a32b75a7d02a5dcf27756707b47a2b31b21c389501&txprove=0"
```

```json
{
  "data": {
    "address": "42f18fc61586554095b0799b5c4b6f00cdeb26a93b20540d366932c6001617b75db35109fbba7d5f275fef4b9c49e0cc1c84b219ec6ff652fda54f89f7f63c88",
    "outputs": [
      {
        "amount": 34980000000000,
        "match": true,
        "output_idx": 0,
        "output_pubkey": "35d7200229e725c2bce0da3a2f20ef0720d242ecf88bfcb71eff2025c2501fdb"
      },
      {
        "amount": 0,
        "match": false,
        "output_idx": 1,
        "output_pubkey": "44efccab9f9b42e83c12da7988785d6c4eb3ec6e7aa2ae1234e2f0f7cb9ed6dd"
      }
    ],
    "tx_hash": "17049bc5f2d9fbca1ce8dae443bbbbed2fc02f1ee003ffdd0571996905faa831",
    "tx_prove": false,
    "viewkey": "f359631075708155cc3d92a32b75a7d02a5dcf27756707b47a2b31b21c389501"
  },
  "status": "success"
}
```

Proving transfer:

We use recipient's address (i.e. not our address from which we sent xmr to recipient).
For the viewkey, we use `tx_private_key` (although the GET variable is still called `viewkey`) that we obtained by sending this txs. 

```bash
# this is for testnet transaction 
curl  -w "\n" -X GET "http://139.162.32.245:8082/api/outputs?txhash=94782a8c0aa8d8768afa0c040ef0544b63eb5148ca971a024ac402cad313d3b3&address=9wUf8UcPUtb2huK7RphBw5PFCyKosKxqtGxbcKBDnzTCPrdNfJjLjtuht87zhTgsffCB21qmjxjj18Pw7cBnRctcKHrUB7N&viewkey=e94b5bfc599d2f741d6f07e3ab2a83f915e96fb374dfb2cd3dbe730e34ecb40b&txprove=1"
```

```json
{
  "data": {
    "address": "71bef5945b70bc0a31dbbe6cd0bd5884fe694bbfd18fff5f68f709438554fb88a51b1291e378e2f46a0155108782c242cc1be78af229242c36d4f4d1c4f72da2",
    "outputs": [
      {
        "amount": 1000000000000,
        "match": true,
        "output_idx": 0,
        "output_pubkey": "c1bf4dd020b5f0ab70bd672d2f9e800ea7b8ab108b080825c1d6cfc0b7f7ee00"
      },
      {
        "amount": 0,
        "match": false,
        "output_idx": 1,
        "output_pubkey": "8c61fae6ada2a103565dfdd307c7145b2479ddb1dab1eaadfa6c34db65d189d5"
      }
    ],
    "tx_hash": "94782a8c0aa8d8768afa0c040ef0544b63eb5148ca971a024ac402cad313d3b3",
    "tx_prove": true,
    "viewkey": "e94b5bfc599d2f741d6f07e3ab2a83f915e96fb374dfb2cd3dbe730e34ecb40b"
  },
  "status": "success"
}
```

## Other monero examples

Other examples can be found on  [github](https://github.com/moneroexamples?tab=repositories).
Please know that some of the examples/repositories are not
finished and may not work as intended.

## How can you help?

Constructive criticism, code and website edits are always good. They can be made through github.

