## Hooks
প্রিয় দর্শক,

আশা করি ভাল আছেন, আজকের লেকচারে আমরা দেখব কিভাবে ডাটাবেইসের সাথে আমাদের PHP কে কানেক্ট করতে হয়। এটি করার জন্য আমরা MySQL ডাটাবেইস ব্যাবহার করব। MySQL ছাড়াও আরও বেশ কিছু জনপ্রিয় রিশেনলা ডাটাবেইস আছে, যেমন PostgreSQL, SQL Server ইত্যাদি। কিন্তু PHP ডেভলাপারদের কাছে সম্ভবত MySQL সবচেয়ে জনপ্রিয় Database সিস্টেম।

## Content
PHP-এর সাথে MySQL কানেক্ট করার আগেে আমাদের জানা দরকার PHP ছাড়া আমরা কিভাবে MySQL ম্যানেজ করব বা ব্যাবহার করব। MySQL ডাটাবেইজ ব্যাবহার করার জন্য, বা ডাটাবাসের ডাটা দেখার জন্য যেমন টেবিল ক্রিয়েট করার জন্য, টেবিলের রো এড আপডেট ডিলেটার করার জন্য, কিছু সফটওয়ার থাকে, সেই সফটোয়ার গুলোকে বলা হয় MySQL ক্লায়েন্ট। আমরা আমাদের WAMP ইন্সটলেশনের ভিডিওতে WAMP এর মাধ্যমে আমাদের কম্পিউটারে Apache Server, MySQL Database এবং PHP ইন্সটল দিয়েছি। এদের পাশাপাশি আমরা PHPMyAdmin ও ইন্সটল দিয়েছিলাম। এই PHPMyAdmin MySQL এর একটি ক্লায়েন্ট। এমন আরও ক্লায়েন্ট আছে, যেমন MySQL Workbench, DBeaver, SQLYog, TablePlus ইত্যাদি, এদের মধ্যে MySQL Workbench এবং PHPMyAdmin সবচেয়ে বহুলব্যাবহৃত এবং ফ্রি। আমাদের WAMP এর সাথে যেহেতু PHPMyAdmin ইন্সটল দেওয়া আছে, সেহেতু আমরা PHPMyAdmin ব্যাবহার করব। কিন্তু আমি যখন MacOS এ কাজ করি তখন আমি TablePlus ব্যাবহার করি। তাই আপনাদের বুঝার সুবিধার্তে এই লেকচারটি আমি Windows এ রেকর্ড করছি। একবার আপনারা ব্যাপারটা বুঝে গেলে, এর পর যে কোন MySQL ক্লায়েন্টে, আপনাদের কোন অসুবিধা হওয়ার কথা না। তখন আপনারা নিজেদের পছন্দ মত যেন কোন MySQL Client ব্যাবহার করতে পারবেন। তাহলে চলুন আমরা প্রথমে আমাদের PHPMyAdmin থেকে একটু ঘুরে আসি
...
..........

এখন আমাদের কাজ হচ্ছে, এই যে এই ডাটা গুলো আমরা আমাদের টেবিলে ইনসার্ট করলাম, এই ডাটাগুলো আমাদের ডাটাবেইজে আছে। এখন এই ডাটাগুলোকে আমরা কিভাবে PHP - এর মাধ্যমে পেতে পারি?

PHP এর মাধ্যমে এই ডাটা গুলো পেতে হলে আমাদের দুইটা ধাপে আগাতে হবে। প্রথমত, PHPMyAdmin যেমন আমাদের ডাটাবেইসের ইউজারনেম এবং পাসওয়ার্ড দিয়ে ডাটাবেইজে কানেক্ট করেছে সেভাবে আমরা আমাদের PHP কোড দিয়ে MySQL এ কানেক্ট হতে হবে। দ্বিতীয়ত, PHPMyAdmin যেমন একটি MySQL কুয়েরী রান করে ডাটা আমাদেরকে দেখিয়েছে, তেমন করে PHP কোড দিয়ে আমরা কুয়েরী রান করে ডাটা দেখব। আপনাদের নিশ্চয় মনে আছে, আমি বলেছিলাম, এই সব কিছু করার জন্য আমাদের প্রিয় বন্ধু ফাংশন এর ব্যাবহার করতে হবে। Database এ কানেক্ট করা, ডাটাবেইজে কুয়েরি রান করা ইত্যাদি করার জন্য echo, count ইত্যাদি ফাংশনের মতই আলাদা আলাদা ফাংশন PHP-দের মধ্যে দেওয়া আছে। আমরা সেই ফাংশন গুলোই ব্যাবহার করব। চলুন দেখিঃ

....................................

..................

### Call to Action
আজকের লেকচারটি বড় হয়েছে গিয়েছে, তবে আশা করিছি এই লেকচার ফলো করে আপনরা PHP দিয়ে MySQL ডাটাবেইজ কানেক্ট করতে পেরেছেন। আপনারা কিন্তু অবশ্যই এই সিরিজের প্রত্যেকটা কোড নিজে নিজে লিখবেন, নাহলে শধু বসে বসে দেখলে প্রোগ্রামিং শিখতে পারবে না। পরের লেকচারে আমরা দেখব কিভাবে আমরা আমাদের যে সার্চ করার ফর্মে বানিয়ে ছিলাম, সেই ফর্মের সার্চ অনুয়াযী ডাটাবেইজ থেকে ডাটা আনতে পারি। সবাই ভাল থাকবেন, Happy Coding!
