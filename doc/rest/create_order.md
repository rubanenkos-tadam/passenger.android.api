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
id | opt | int64 | Bank card Id or B2B Fare Id
name | opt | string | Pan for Bank card for kind = credit_card
enoughMoney | opt | boolean | If **True** - user can using cashless payment kind

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
{
  "paymentMethod": {
    "kind": "cash"
  },
  "tariff": 230000010102102,
  "options": [
    4546476474,
    84943093,
    32323
  ],
  "route": [
    {
      "address": {
        "name": "Республика Беларусь, обл Бресткая, г Брест, б-р Космонавтов, 60 (Школа № 3)",
        "components": [
          {
            "level": 0,
            "name": " Республика Беларусь"
          },
          {
            "level": 1,
            "name": "обл Бресткая"
          },
          {
            "level": 4,
            "name": "г Брест"
          },
          {
            "level": 7,
            "name": "б-р Космонавтов"
          },
          {
            "level": 8,
            "name": "60"
          },
          {
            "level": 9,
            "name": "Школа № 3"
          }
        ],
        "types": {
          "pointType": 63,
          "aliasType": 3
        },
        "position": {
          "lat": 52.096807,
          "lon": 23.697629
        }
      },
      "comment": "парковка перед школой"
    }
  ],
  "time": "2019-11-14T13:00:00+03:00"
}
}
```

## Answer / json
Param | Man/Opt | Type | Description
----- | ------- | ---- | -----------
id | man | int 64| Order Id
