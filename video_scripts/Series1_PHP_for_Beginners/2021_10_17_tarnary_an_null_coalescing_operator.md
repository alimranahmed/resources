## Hooks
প্রিয় দর্শক

আশা করি সবাই ভাল আছে, আজকে আমরা টারনারি অপারেটর দিয়ে কিভাবে কিভাবে PHP তে কন্ডিশন প্রকাশ করতে হয় তা দেখব।

## Content
প্রথমে আমরা একটি সাধারণ if-else কন্ডিশন দেখি......

```php
$mark = 20;
$result = "";

if($mark >= 33){
  $result = "PASS";
}else{
  $result = "FAIL";
}
```

echo($result);

এখন এই কন্ডিশনটি আমরা টারনারি অপারেটর দিয়ে লিখতে পারিঃ
```php
$result = $mark >= 33 ? "PASS" : "FAIL";
echo($result);
```

অর্থাৎ  `condition ? statement_if_true : statement_if_false`

এখন ধরুন আমরা লিখতে চাইঃ

```php
$name = isset($user) ? $user : "guest"
```
এটা আমরা লিখতে পারিঃ

```php
$name = $user ?? 'guest';
```
এই ?? চিহ্নকে বলা হয় নাল কোলেসিং অপারেটর


## Call to Action
এই চমৎকার টারনারি অপারেটর সব জাগায় ব্যাবহার করার জন্য নয়। যখন মনে হবে এটা একটি সহজ if-else লজিক শুধু তখই আমরা টারনারি অপারেটর ব্যাবহার করব।
