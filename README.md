<p align="center">
  <img title="LuminPay" src="https://res.cloudinary.com/dqjm8yyzd/image/upload/v1757546948/logo-home_kcmfse.svg" width="280px"/>
</p>

# 💳 LuminPay Laravel SDK

The **LuminPay Laravel SDK** provides official integration of [LuminPay](https://luminpay.io) into your Laravel applications.

---
It helps businesses and developers:  
- Accept **crypto payments** (stablecoins like USDC/USDT and crypto)  
- Provide **inline checkout experiences** or hosted checkout links  
- Enable **onramp (fiat → crypto)** and **offramp (crypto → fiat)**  

---

## ✨ Features

- 🛒 **Checkout Integration** – Inline widget or hosted checkout links  
- 🪪 **Wallet-as-a-Service (WaaS)** – Dynamically create wallets per merchant/customer  
- 🌍 **Multi-chain Support** – Solana, Ethereum, Tron, BSC, Bitcoin, Waves  
- 💸 **Onramp / Offramp** – Convert fiat to crypto and withdraw to bank/crypto addresses  
- 🔄 **Stablecoin Settlement** – Accept USDC/USDT, Crypto and auto-settle into your wallet  
- 📡 **Webhooks** – Receive real-time notifications on payments and settlement  
- 🧾 **Reconciliation** – API for transaction history and settlement reporting  

---

## 🚀 Installation
```bash
composer require luminpay/laravel-sdk
```

Publish config:
```bash
php artisan vendor:publish --provider="LuminPay\LuminPayServiceProvider"
```

.env:
```env
LUMINPAY_MERCHANT_ID=your_merchant_id
LUMINPAY_API_KEY=your_api_key
LUMINPAY_ENV=sandbox
```

---

## 🔧 Usage
```php
use LuminPay\LuminPay;

$luminpay = new LuminPay(env('LUMINPAY_MERCHANT_ID'), env('LUMINPAY_API_KEY'));
```

### Checkout
```php
$session = $luminpay->checkout()->createSession([
  'amount' => 100,
  'currency' => 'USDC',
  'chain' => 'solana',
  'reference' => 'ORDER-1234',
  'customerEmail' => 'customer@example.com',
  'successUrl' => 'https://yourapp.com/success',
  'cancelUrl' => 'https://yourapp.com/cancel',
]);
return redirect($session['checkoutUrl']);
```

### Wallets
```php
$wallet = $luminpay->wallets()->create([
  'customerId' => 'cust_1',
  'chains' => ['solana','ethereum']
]);
```

### Onramp
```php
$onramp = $luminpay->onramp()->initiate([
  'amount' => 5000,
  'currency' => 'NGN',
  'coin' => 'USDT',
  'accountId' => 'bank-account-id',
  'reference' => 'ONRAMP-001'
]);
```

### Offramp/Payout
```php
$payout = $luminpay->payouts()->initiate([
  'amount' => 100,
  'coin' => 'USDC',
  'chain' => 'solana',
  'destination' => [
    'type'=>'bank',
    'bankCode'=>'044',
    'accountNumber'=>'0123456789'
  ],
  'reference' => 'PAYOUT-001'
]);
```

### Webhooks
```php
Route::post('/webhook', function(Request $request){
  $event = $request->input('event');
  if($event === 'payment.success'){
    // handle success
  }
  return response()->json(['ok'=>true]);
});
```

---

## 📜 License
MIT © 2025 [LuminPay](https://luminpay.io)
