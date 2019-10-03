[![SensioLabsInsight](https://insight.sensiolabs.com/projects/67c84e89-dfd1-4cd6-8604-53504f5dd101/mini.png)](https://insight.sensiolabs.com/projects/67c84e89-dfd1-4cd6-8604-53504f5dd101)
[![Build Status](https://travis-ci.org/massimo-me/satispay-php-sdk.svg?branch=master)](ttps://travis-ci.org/massimo-me/satispay-php-sdk?branch=master)

# Satispay PHP SDK

- [X] Check Bearer
- [X] Users
- [X] Charges
- [X] Amounts
- [X] Refunds

# Install
[Download composer](https://getcomposer.org/download)

```
$ composer require chiarillomassimo/satispay-php-sdk
```

## API: Init

```php
use ChiarilloMassimo\Satispay\Authorization\Bearer;
use ChiarilloMassimo\Satispay\Satispay;

$satispay = new Satispay(
    new Bearer('osh_...'),
    'sandbox'
);
```

## API: Check Bearer

### Check Authorization

```php
if ($satispay->getBearerHandler()->isAuthorized()) {
 ....
};
```

## API: Users

### Creation

```php
$satispay->getUserHandler()->createByPhoneNumber('+39 yourphone')

$user = new User(null, '+39 yourphone');
$satispay->getUserHandler()->persist($user)
```

### Get

```php
$satispay->getUserHandler()->findOneById('id')
```

### Find

```php
$satispay->getUserHandler()->find()
$satispay->getUserHandler()->find(50, 'starting_id', 'ending_id')

$users = $satispay->getUserHandler()->find();

foreach ($users as $user) {
    //...
}
```

## API: Charge

### Create

```php
use ChiarilloMassimo\Satispay\Model\Charge;

$charge = new Charge();

$user = $satispay->getUserHandler()->createByPhoneNumber('+39 yourphone');

$charge
    ->setUser($user)
    ->setAmount(15) // 0.15 €
    ->setCallbackUrl('http://fakeurl.com/satispay-callback')
    ->setCurrency('EUR')
    ->setDescription('Test description')
    ->setExpireMinutes(20)
    ->setExtraFields([
        'orderId' => 'id',
        'extra' => 'extra'
    ])
    ->setSendMail(false);
    
$satispay->getChargeHandler()->persist($charge, true);

$charge->isPaid(); //false
$charge->getStatus(); //Charge::STATUS_REQUIRED
```

### Get

```php
$satispay->getChargeHandler()->findOneById('charge_id')
```

### Update

```php
$charge = $satispay->getChargeHandler()->findOneById('charge_id')
$charge->setDescription('My fantastic description!!')

$satispay->getChargeHandler()->update($charge)
```

### Find

```php
$satispay->getChargeHandler()->find()
$satispay->getChargeHandler()->find(50, 'starting_id', 'ending_id')

$charges = $satispay->getChargeHandler()->find();

foreach ($charges as $charge) {
    //...
}
```

## API: Refund

### Creation

```php
use \ChiarilloMassimo\Satispay\Model\Refund;

$charge = $satispay->getChargeHandler()->findOneById('id');

$refund = new Refund();
$refund
    ->setCharge($charge)
    ->setDescription('Test')
    ->setAmount(15)
    ->setReason(Refund::DUPLICATE);

$satispay->getRefundHandler()->persist($refund);
```

### Get

```php
$satispay->getRefundHandler()->findOneById('id'));
```

### Update

```php
$refund = $satispay->getRefundHandler()->findOneById('id');
$refund->setDescription('My fantastic description!!');

$satispay->getRefundHandler()->update($refund);
```

### Find

```php
$satispay->getRefundHandler()->find()
$satispay->getRefundHandler()->find(50, 'starting_id', 'ending_id')

$refunds = $satispay->getRefundHandler()->find();

foreach ($refunds as $refund) {
    //...
}
```

## API: Amounts

### Get

```php
$amount = $satispay->getAmountHandler()->findBy(
    new DateTime('2018-01-01'),
    new DateTime('now')
);
```

