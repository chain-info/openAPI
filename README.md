# openAPI
Open API of chain.info
---

### open.chain.info/v1/socket
listen to this url to get realtime big trade notification


**method: socket.io**


*event: newnotification*

*data:*

```
// Data is string which can be JSON.parse() to json. It contains the specific fields of big trade notification, and you can parse the field according to the type matching the template.

{
    success: true, 
    type: int, // type of big trade notification, you will see 0~8, and check it according to template
    date: int, // timestamp when we generate this notification
    txid: string, // transaction id 
    value: Number, // value of this big trade notification
    input_address: string // address of input, understand it according to type and template
    input_entity: string // exchange that input address belongs to
    input_address_type: int // type of input address, -1 for unknown address, 0 for deposit wallet address, 1 for hot wallet address, 2 for cold wallet address
    output_address: string // address of output, understand it according to type and template
    output_entity: string // exchange that output address belongs to
    output_address_type: int // type of output address, -1 for unknown address, 0 for deposit wallet address, 1 for hot wallet address, 2 for cold wallet address
}
```

*'type' in data:*

```
{
	0 ==> Deposit. It means that a unknown address transfers BTC to an exchange deposit wallet address
	1 ==> Deposit. It means that multi unknown addresses transfer BTC to an exchange deposit wallet address
	2 ==> Withdraw. It means that an exchange hot wallet address transfers BTC to a unknown address
	3 ==> Withdraw. It means that multi exchange hot/deposit wallet addresses transfer BTC to a unknown address
	4 ==> Deposit&Withdraw. It means that an exchange hot wallet addresses transfer BTC to an another exchange deposit wallet address
	5 ==> Deposit&Withdraw. It means that multi exchange hot/deposit wallet addresses transfer BTC to an another exchange deposit wallet address
	6 ==> Cold To Hot. It means an exchange transfer its BTC from its cold wallet to its hot wallet
	7 ==> Huge Transaction. It means a unknown address transfers more than 5000 BTC to an another unknown address
	8 ==> Huge Transaction. It means multi unknown addresses transfer more than 5000 BTC to an another unknown address
}
```

*'address type' in data:*

```
{
	-1 ==> unknown address. This address doesn't belong to any exchange
	0 ==> exchange deposit wallet address
	1 ==> exchange hot wallet address
	2 ==> exchange cold wallet address
}
```
