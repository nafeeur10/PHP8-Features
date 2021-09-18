
# Nullsafe Operator

কোন ধরনের সমস্যা সমাধানের জন্য nullsafe operator আসসে সেটা আগে আমরা দেখি নিচের program এর মাধ্যমে।

```
$country =  null;
 
if ($session !== null) {
    $user = $session->user;
 
    if ($user !== null) {
        $address = $user->getAddress();
 
        if ($address !== null) {
            $country = $address->country;
        }
    }
}
```

উপরের program টা বলতেছে যদি Session null না হয় তাহলে Session থেকে user নিবে। user যদি null না হয় তাহলে user এর getAddress function কে কল করে address নিবে। আর address যদি null না হয় তাহলে address থেকে country নিবে। 

এই Nested if else দিয়ে Null checking এর সমস্যা সমাধানের জন্যই মুলত Nullsafe Operator আসছে। Null Safe Operator এর Symbol `?->` তাহলে আমরা উপরের Program টার সমাধান Null Safe দিয়ে করে ফেলব এখন। 

```
$country = $session?->user?->getAddress()?->country;
```

উপরের Program টা দেখে মনে হতে পারে যে তাহলে মনে হয় Property checking এও আমরা Nullsafe Operator ব্যবহার করতে পারি। কিন্তু তা নয়। শুধুমাত্র Object Checking এর ক্ষেত্রে আমরা এই Operator ব্যবহার করতে পারি। এটার Symbol দেখেও ব্যাপারটা আন্দাজ করা যায়। Property/Method Access এর Symbol `->` আর Nullsafe এর `?->`।

বিষয়টা আরও একটু পরিষ্কার হই নিচের উদাহরণের দ্বারা। 

```
class User 
{
    public ?string $user_token;

    public function __construct()
    {
        $this->user_token = $_SESSION["user_token"];
    }
}

class profile
{
    public function employment() 
    {
        return "You are a valid user!";
    }
}

$user = new User();
$profile = new profile();
echo $user?->user_token?->$profile->employment();
```

উপরের উদাহরনে `User` এর একটা property হল `$user_token` যেটাকে আমরা `__construct()` এর মাধ্যমে initialize করেছি। `$user?->user_token?->$profile->employment();` এই লাইনে আমরা check করতেছি `$user_token` টা NULL কি না। এই Program টা আমাকে একটা Error দিবে। 

```
Fatal error: Uncaught Error: Object of class profile could not be converted to string
```

কারন, Nullsafe Operator এর কাজ property/method এর value null কিনা এটা check করা না। এটার কাজ হল, Object যদি Null না হয় তাহলে তার Property/Method Access করতে পারা। Object Null হলে আর সামনে আগাবে না। তার কোন Property/Method সে আর Access করতে পারবে না। এবার নিচের আরো একটি উদাহরণের দ্বারা আমরা বিষয়টা পরিষ্কার হবো ইনশাআল্লাহ। 

```
class User 
{
    public function getAddress() 
    {
        return new Address;
    }
}
class Address 
{
    public function getCountry()
    {
        return new Country;
    }
}

class Country 
{
    public function getCountryName()
    {
        return "YES! It's BD";
    }
}

$customer = new User();
echo $customer?->getAddress()?->getCountry()?->getCountryName();
```

আমরা প্রথমে দেখতেছি `$customer` Object টা Null কি না। তারপর এটার Method এ যাচ্ছি। এই Method আবার আমাকে `Address` Class এর Object এ নিয়ে যাচ্ছে, সেটা Null না হলে আমাকে `Country` ক্লাসে এর Object এ নিয়ে যাচ্ছে এবং সেটা Null না হলে আমাকে আমার কাঙ্ক্ষিত ফলাফল return করে দিচ্ছে। এর ভেতরের কোন একটা Object `(new ClassName)` Null হলে আমাকে Error না দিয়ে Null দিয়ে দিত। 


আশা করি বিষয়টা পরিষ্কার হয়েছে। 

Ref: 
* https://wiki.php.net/rfc/nullsafe_operator
* https://php.watch/versions/8.0/null-safe-operator
