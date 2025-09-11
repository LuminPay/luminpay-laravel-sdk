<p align="center">
  <img title="LuminPay" src="https://res.cloudinary.com/dqjm8yyzd/image/upload/v1757546948/logo-home_kcmfse.svg" width="280px"/>
</p>

# 💳 LuminPay Laravel SDK

The **LuminPay Laravel SDK** provides an easy way to integrate **crypto payments, wallets, onramp, and payouts** into Laravel applications.

It allows you to:  
- 🛒 Accept crypto payments (inline or hosted checkout)  
- 🪪 Manage customer wallets (WaaS)  
- 💸 Support onramp (fiat → crypto) and offramp (crypto → fiat)  
- 🏦 Handle merchant payouts  
- 📡 Process webhooks  
- 🧾 Reconcile transactions  

---

## ✨ Features
- Hosted and inline checkout support  
- Wallet-as-a-Service for customers and merchants  
- Onramp & Offramp APIs  
- Payouts to bank and wallet addresses  
- Webhooks for events  
- Transaction reconciliation  

---

## 🚀 Installation

```bash
composer require luminpay/laravel-sdk
```

Publish config:

```bash
php artisan vendor:publish --provider="LuminPay\LuminPayServiceProvider"
```

Set environment variables in `.env`:

```env
LUMINPAY_MERCHANT_ID=your_merchant_id
LUMINPAY_API_KEY=your_api_key
LUMINPAY_ENV=sandbox
```

---

## 🔧 Initialization

```php
use LuminPay\LuminPay;

$luminpay = new LuminPay(env('LUMINPAY_MERCHANT_ID'), env('LUMINPAY_API_KEY'), env('LUMINPAY_ENV'));
```

---

## 🛒 Checkout

### Hosted Checkout

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

### Inline Checkout

Use the provided data to render inline UI in your frontend (React, Vue, etc.).

---

## 🪪 Wallet-as-a-Service (WaaS)

```php
$wallet = $luminpay->wallets()->create([
  'customerId' => 'cust_001',
  'chains' => ['solana','ethereum','tron'],
]);

dd($wallet['addresses']);

$existing = $luminpay->wallets()->get('cust_001');
dd($existing);
```

---

## 💸 Onramp (Fiat → Crypto)

```php
$onramp = $luminpay->onramp()->initiate([
  'amount' => 5000,
  'currency' => 'NGN',
  'coin' => 'USDT',
  'accountId' => 'bank-account-id',
  'reference' => 'ONRAMP-001',
]);

dd($onramp['status']); // "pending"
```

---

## 🏦 Offramp (Crypto → Fiat)

Withdraw to a bank account:

```php
$payout = $luminpay->payouts()->initiate([
  'amount' => 100,
  'coin' => 'USDC',
  'chain' => 'solana',
  'destination' => [
    'type' => 'bank',
    'bankCode' => '044',
    'accountNumber' => '0123456789',
  ],
  'reference' => 'PAYOUT-001',
]);

dd($payout['status']); // "processing"
```

Withdraw to a crypto wallet:

```php
$payout = $luminpay->payouts()->initiate([
  'amount' => 50,
  'coin' => 'USDT',
  'chain' => 'ethereum',
  'destination' => [
    'type' => 'wallet',
    'address' => '0x1234abcd...',
  ],
]);

dd($payout['status']);
```

---

## 📡 Webhooks

Handle webhook events in Laravel:

```php
use Illuminate\Http\Request;
use Illuminate\Support\Facades\Log;

Route::post('/webhook', function(Request $request){
  $event = $request->input('event');
  $data = $request->input('data');

  switch($event){
    case 'payment.success':
      Log::info('Payment success', $data);
      break;
    case 'payment.failed':
      Log::warning('Payment failed', $data);
      break;
  }

  return response()->json(['ok' => true]);
});
```

Example payload:

```json
{
  "event": "payment.success",
  "data": {
    "reference": "ORDER-1234",
    "amount": 100,
    "currency": "USDC",
    "chain": "solana",
    "status": "confirmed"
  }
}
```

---

## 🧾 Reconciliation

```php
$txs = $luminpay->transactions()->list([
  'status' => 'confirmed',
  'from' => '2025-01-01',
  'to' => '2025-01-31',
]);

dd($txs);

$tx = $luminpay->transactions()->get('txn_12345');
dd($tx);
```

---


## 📜 License

MIT © 2025 [LuminPay](https://luminpay.co)
