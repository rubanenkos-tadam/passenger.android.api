POST /31/estimate
===

### Headers
* Content-Type: application/json; charset=utf-8

Param | Man/Opt | Type | Description
----- | ------- | ---- | -----------
payment_kind | man | PaymentKind / string | Kind of payment method
tariffs | man | array of int64 | Array of tariffs ids. Maximium 4 ids.
options | man | array of int64 | Array of options ids
route | man | array of [GpsPosition](doc/types/GpsPosition.md) / json | Route points. Minimum 1 point.
time_on | opt | Time / json | Time for advanced order

#### PaymentKind / string
Value | Description
---- | ------
cash | cash money
contractor | B2B Client
bank_card | Bank card

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
  "payment_kind": "cash",
  "tariffs": [314124124,234344544],
  "options": [],
  "route": [
    {"lat": 52.095706, "lon": 23.684427}, 
    {"lat": 52.113256, "lon": 23.691464}
  ],
  "time_on": "2020-02-28 12:35:00+03"
}
```

## Answer / json
Param | Man/Opt | Type | Description
----- | ------- | ---- | -----------
estimations | man | array of Estimation / json | Array of estiomations
path | opt | GeoJSON | Route points
distance | opt | inteegr | Distance in metres

### Estimation / json
Param | Man/Opt | Type | Description
----- | ------- | ---- | -----------
tariff | man | int64 | Tariff id
cost | man | Cost / json | Cost estimate
submission_time | opt | integer | Car delivery time in seconds

#### Cost / json
Param | Man/Opt | Type | Description
----- | ------- | ---- | -----------
type | man | CostType | Type of Cost
amount | man | number | Sum total
calculation | man | CalculationType / json | Type of Calculation
modifier | opt | CostModifier / json | Object of cost modifier
fixed | opt | number | If param is set then cost is fixed
details | opt | array of CostItem / json | Details of cost

#### CostType / string
Value | Description
---- | ------
total | 
approximate | 
minimum | 

#### CalculationType / string
Value | Description
---- | ------
fixed | 
taximeter | 

#### CostModifier / json
Param | Man/Opt | Type | Description
----- | ------- | ---- | -----------
type | man | CostModifierType / string | 
value | man | number | 

#### CostModifierType / string
Value | Description
---- | ------
add | 
multiply | 

#### CostItem / json
Param | Man/Opt | Type | Description
----- | ------- | ---- | -----------
title | man | string | name of item
cost | man | number | value
