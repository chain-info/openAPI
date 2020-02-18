# openAPI
Open API of chain.info
---

### open.chain.info/v1/socket
listen to this url to get realtime big trade notification


**method: socket.io**


*event: newNotification*

*data:*

```
// Data is string which can be JSON.parse() to json. It contains the specific fields of big trade notification, and you can parse the field according to the type matching the template.

{
    success: true, 
    type: string, // type of big trade notification, check it according to template
    date: timestamp, // timestamp when we generate this notification
    txid: string, // transaction id 
    url: string, // see transcation in Chain.info blockchian explorer
    value: Number, // value of this big trade notification
    input_address: string // address of input, understand it according to type and template
    input_entity: string // exchange that input address belongs to
    input_address_type: string // type of input address
    output_address: string // address of output, understand it according to type and template
    output_entity: string // exchange that output address belongs to
    output_address_type: string // type of output address
}
```

*'type' in data:*

```
{
	Deposit ==> It means that a unknown address transfers BTC to an exchange deposit wallet address
	Multi-Deposit ==> It means that multi unknown addresses transfer BTC to an exchange deposit wallet address
	Withdraw ==> It means that an exchange hot wallet address transfers BTC to a unknown address
	Multi-Withdraw ==> It means that multi exchange hot/deposit wallet addresses transfer BTC to a unknown address
	Deposit&Withdraw ==> It means that an exchange hot wallet addresses transfer BTC to an another exchange deposit wallet address
	Multi-Deposit&Withdraw ==> It means that multi exchange hot/deposit wallet addresses transfer BTC to an another exchange deposit wallet address
	ColdToHot ==> It means an exchange transfer its BTC from its cold wallet to its hot wallet
	Huge ==> It means a unknown address transfers more than 5000 BTC to an another unknown address
	Multi-Huge ==> It means multi unknown addresses transfer more than 5000 BTC to an another unknown address
}
```

*'address type' in data:*

```
{
	'' ==> unknown address. This address doesn't belong to any exchange
	Deposit ==> exchange deposit wallet address
	Hot ==> exchange hot wallet address
	Cold ==> exchange cold wallet address
}
```


**method: [WEB GET] https://open.chain.info/v1/entity/bigTrade**
```
params:
			name=entity name, like "Huobi" and "Binance"
			date=query date, like "20191001", which can be ignore and will use current date
```

```
{
  "data": [
    {
      "date": str, // UTC time is timestamp
      "inputAddress": str, // transaction input side address
      "inputAddressType": str, // transaction input side address type, which follow by 'address type' in data
      "inputEntity": str, // which address belong to, like "Huobi"
      "outputAddress": str, // same as input side
      "outputAddressType":  str, // same as input side
      "outputEntity":  str, // same as input side
      "timestamp": int, // the transaction happended in this timestamp
      "txid": str, // transaction id
      "type": str, // transaction type which followed by 'type' in data
      "url": str, // the web site of the tx in chain.info
      "value": -200 // the BTC value of the transaction if the entity on input side the value will be negative
    }
  ]
  "success": true
}
```

**method: [WEB GET] https://open.chain.info/v1/entity/list**


```
{
  
  "data": {
    "blockHeight": int, // update at this block height
    "date": int, // update at this time
    "entityList": [
      {
        "addressCount": int , // the number of address in this entity at this height
        "balance": float(2), // the amount of BTC balance in this entity at this height
        "bigTotalExpense": float(2), // the amount of BTC in big trade output side in this entity during the last day
        "bigTotalIncome": float(2), // the amount of BTC in big trade intput side in this entity during the last day
        "bigTotalNum": int ,// the num of big trade in this entity during the last day
        "coldBalance": float(2), // the amount of BTC balance in this entity cold wallet at this height
        "coldNum": int, // the number of cold wallet address
        "hotBalance": float(2), // the amount of BTC balance in this entity hot wallet at this height
        "hotNum": int, // the number of hot wallet address
        "name": str, // entity name,like "Huibo"
        "netIncome": float(2), // the difference of BTC balance in this entity at this height and last day height
        "rank": int , // the rank of the entity in the list, the list order will follow rank 
        "rechargeBalance": float(2), // the amount of BTC balance in this entity deposit wallet at this height
        "rechargeNum": int, // the number of deposit wallet address
      },
  ]
  "success": true
}
```
