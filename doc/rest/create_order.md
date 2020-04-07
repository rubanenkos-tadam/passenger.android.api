POST /41/order/add
===

### Headers
* Content-Type: application/json; charset=utf-8

Param | Man/Opt | Type | Description
----- | ------- | ---- | -----------
paymentMethod | man | PaymentMethod / json | Payment method
tariff | man | number	| Fare Id
options | man | array int64 | Selected options of tariff
route |	man | array ClientAddress / json | List of route points
time | opt | Time / json | Order Time (for order on exect time)
comment | opt | string | Comment for order
fixCost | opt | number | Fixed Cost
useBonuses | opt | number | Use bonuses to pay for order
disableSms | opt | boolean | Disable SMS notification
disableVoice | opt | boolean | Disable Voice notification

#### PaymentMethod / json
Param | man/opt | Type | Description
----- | ------- | ---- | -----------
kind | man | PaymentKind / string | Kind of payment
id | opt | number | 
name | opt | string | 
enoughMoney | opt | boolean | 

#### PaymentKind / string
Value | Description
---- | ------
cash | Cash
contractor | Cashless, B2B customer
credit_card | Cashless, bank card

#### Time / json
Param | Man/Opt | Type | Description
----- | ------- | ---- | -----------
type | man | TimeType / string | Type of value
value | man | string | Value

#### TimeType / string
Value | Description
---- | ------
absolute | absolute time, format [LocalDateTime](doc/types/times.md#LocalDateTime)
relative | relative time, format [Duration](doc/types/times.md#Duration)

### Request Example
```json
{
  "paymentMethod": "cash",
  "tariff": 230000010102102,
  "options": [4546476474,84943093,32323],
  "route": [
  ],
  "time": "2020-03-05 12:30:00+03"
}
```

## Answer / json
Param | Man/Opt | Type | Description
----- | ------- | ---- | -----------
id | man | int 64| Order Id
