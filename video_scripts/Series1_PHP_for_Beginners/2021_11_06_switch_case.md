## Hooks
প্রিয় দর্শক,

আশা করি সবাই ভাল আছেন। আজকে আমরা switch-case নিয়ে আলোচনা করব। টারনারি অপারেটর

## Content
switch-case php-তে কন্ডিশন লিখার আরেকটা ধরন। চলুন প্রথমে আমরা PHP-তে if-else দিয়ে একটি কন্ডিশন দেখি।
```php
$userType = 'admin';

if($userType == 'super-admin'){
  echo("Hello super-admin, you have the admin controll<br>");
}else if($userType == 'admin'){
  echo("Hello admin, you have the admin control<br>");
}elseif($userType == 'visitor' || $userType == 'user'){
  echo("Hello user/visitor, you have user access");
}else{
  echo("Hello, welcome to the website");
}
```

এই কন্ডিশনটি আমরা চাইলে `switch-case` দিয়েও লিখতে পারি। কিভাবে, চলুন দেখিঃ

```php
$userType = 'admin';

switch($userType):
  case "super-admin":
    echo("Hello super-admin, you have the admin control<br>");
    berak;
  case "admin":
    echo("Hello admin, you have the admin control<br>");
    break;
  case "visitor":
  case "user":
    echo("Hello user/visitor, you have user access");
    break;
  default:
    echo("Hello, welcome to the website");
    break;
```

এখানে দুইটা বিষয় লক্ষ্য করবেন, `switch-case` এ আমরা কন্ডিশন লিখার জন্য একটি ভ্যারিয়েবল এখানে `$userType` এর উপর নির্ভর করেছি। পাশাপাশি আমরা কেইসে যা লিখছি তা কিন্তু সব সময় সমান কিনা চেক করা হচ্ছে, বড় কিংবা ছোট কিনা তা চেক করা হয় না। তবে আমি অনেক নতুন প্রোগ্রামাদের দেখেছি কেইসের মধ্যে গ্রেটার দেন কিংবা লেস দেন সাইন ব্যাবহার করারা চেষ্টা করতে। `switch-case` এর ক্ষেত্রে এটা করা উচিত নয়। লেস দেন, গ্রেটার দেন এর প্রয়োজন হলে আমরা `if-else-if` কন্ডিশন ব্যাবহার করব। 

## Call to Action
আরেকটা বিষয় নিশ্চয় আপনারা বুঝতে পেরেছেন, আমরা চাইলে `switch-case` কন্ডিশন ব্যাবহার না করে if-else দিয়ে কাজ চালাতে পারি। এই কারনে আসলে `switch-case` খুব যে বেশি ব্যাবহার হয় তা না। তবে এই রকম সিঙ্গেল ভারিয়াবলের উপর ডিপেন্ডেন্ট কন্ডিশন লিখার জন্য এটা বেশ কার্যকরি। আশা করি `switch-case` নিয়ে আপনাদের ধারণা পরিষ্কার। দেখা হবে পরের লেকচারে, Happy Coding!
