# band-icon

## Sequence of necessary commands

```bash
tbears keyinfo

tbears transfer -k keystore_test1 hx7bf03094d05f8d3a8270082586e8aa23450fc672 100e18

tbears balance hx7bf03094d05f8d3a8270082586e8aa23450fc672

tbears balance hx7bf03094d05f8d3a8270082586e8aa23450fc672 -u https://bicon.net.solidwallet.io/api/v3

tbears init hello_world HelloWorld

tbears deploy hello_world -k example_key

tbears txresult 0x0b188d283d7fc24880d5ece6c536d1e0787911ba46e29698f022ad0f67316369

tbears init mumu MUMU

tbears deploy mumu -k example_key

tbears call example_call.json

tbears sendtx example_send.json -k example_key
```

# -----------------------------------------------------

## Python script for automatically receiving fund from ICX Faucet (Testnet)

```python
import subprocess
import time

while True:
    print(subprocess.check_output("curl 'http://icon-faucet-api.ibriz.ai/api/requesticx/hx7bf03094d05f8d3a8270082586e8aa23450fc672' -H 'Connection: keep-alive' -H 'Pragma: no-cache' -H 'Cache-Control: no-cache' -H 'Origin: http://icon-faucet.ibriz.ai' -H 'User-Agent: Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_2) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/79.0.3945.79 Safari/537.36' -H 'Accept: _/_' -H 'Referer: http://icon-faucet.ibriz.ai/' -H 'Accept-Encoding: gzip, deflate' -H 'Accept-Language: en-US,en;q=0.9' --compressed --insecure", shell=True))

    time.sleep(60)
```

## Extract privateKey from example_key.json using Python

```python
from eth_keyfile import extract_key_from_keyfile

with open('./example_key.json', 'rb') as file:
    pk = extract_key_from_keyfile(file, "your_password")

```

## Test sign messageHash and recover with Python

```python
from web3.auto import w3
from base64 import b64decode as bd

bd("qW5i7TlV5lvjqqPxLYe2tc8mA57PqUjcUQeklUGOVDA=").hex()
# should get 'a96e62ed3955e65be3aaa3f12d87b6b5cf26039ecfa948dc5107a495418e5430'

w3.eth.account.signHash(bytes.fromhex("1476abb745d423bf09273f1afd887d951181d25adc66c4834a70491911b7f750"), private_key=pk)
# should get AttrDict({'messageHash': HexBytes('0x1476abb745d423bf09273f1afd887d951181d25adc66c4834a70491911b7f750'), 'r': 113583638664437022077987742275766601768479862273295672732118902445032431683380, 's': 1953055889131519193723632024050297517529600263683168252333949297926083784469, 'v': 27, 'signature': HexBytes('0xfb1e0faf8410535991f1b925ed36b72164ab512f7ab8dc7f7b4f54f1b5000734045163f52a80e4023bf7c58861cae4613a7d1f7cf1a2a2fd485bd9ff68d12f151b')})

# Do not forget to change "v" from 27 or 1b to 00 and from 28 or 1c to 01
# '0xfb1e0faf8410535991f1b925ed36b72164ab512f7ab8dc7f7b4f54f1b5000734045163f52a80e4023bf7c58861cae4613a7d1f7cf1a2a2fd485bd9ff68d12f151b' -> '0xfb1e0faf8410535991f1b925ed36b72164ab512f7ab8dc7f7b4f54f1b5000734045163f52a80e4023bf7c58861cae4613a7d1f7cf1a2a2fd485bd9ff68d12f1500'

pub = bd("A/V/OZek6B2PMh6XEJJ+IsLm0w+22PdJqeSgevs7O3kJ").hex()
# after recover the signed message, the result should be equal to "pub"

```

## Retreive icx address from publicKey using Python

```python
import hashlib

pub = bytes.fromhex("04974cac2532a62d2e2990e2945e3899038927730ca21e7b5deedb96b0a0c80c31162f96fca2e6f452fd45c640516db3e58b8dfefeb93cca51ecd36289f45a855b")

address = hashlib.sha3_256(pub[1:]).hexdigest()[-40:]
```
