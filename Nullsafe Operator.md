
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
