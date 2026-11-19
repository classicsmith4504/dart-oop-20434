# Dart OOP Assignment: Kigali Bus Card

**Name:** Eutee Smith
**Student ID:** 20434

## My numbers

- D = 34, L = 4
- Starting balance = 1000 + (34 × 10) = **1340 RWF**
- Bus fare = 200 + 34 = **234 RWF**
- Student discount = 10 + 4 = **14%**

## Output of main.dart

```
Error: name too short
Owner: Eutee | Balance: 1340 RWF
Owner: Eutee Smith | Balance: 1340 RWF
Error: top-up must be 500 to 50000 RWF
Topped up 2000 RWF
Paid 234 RWF
Paid 234 RWF
Owner: Eutee Smith | Balance: 2872 RWF
Student discount: 32 RWF
Paid 202 RWF
Owner: Eutee Smith | Balance: 1138 RWF
Student card: 14% off
```

How the balance works out: 1340 + 2000 = 3340, then 3340 − 234 − 234 = 2872.
For the student card: 1340 − (234 − 32) = 1340 − 202 = 1138.

## Part 1: Why no setter for balance?

A setter would let any code write `card.balance = 999999`, so anyone could give
themselves free money, or set the balance to a negative number, and skip every
rule. Without a setter, the balance can only change through `topUp()` (which
checks the amount is 500 to 50000) and `pay()` (which refuses a fare bigger than
the balance). The `_balance` field is private and the getter only lets other
code read it, so the card protects its own money (this is called encapsulation).

## Part 2: What does ~/ do?

`~/` is **integer division**. It divides and throws away the decimal part.

```dart
print(237 * 17 / 100);   // 40.29  (normal division gives a double)
print(237 * 17 ~/ 100);  // 40     (integer division gives an int)
```

237 × 17 = 4029, and 4029 ÷ 100 = 40.29. `/` keeps the .29, while `~/` cuts it
off and keeps only 40. We need `~/` because the discount `off` is stored in an
`int` (whole RWF), and `/` would give a double that cannot be saved in an `int`.
With my numbers: `234 * 14 ~/ 100 = 3276 ~/ 100 = 32`.

## Part 4: Bug table

| Bug | The line | Why it is wrong (your words) | Your fix |
|-----|----------|------------------------------|----------|
| 1 | `String _brand;` and `int _battery;` | Fields that cannot be null must have a starting value. These two have none, so Dart gives an error. | `String _brand = '';` and `int _battery = 0;` |
| 2 | `SmartPhone(String brand, int battery, String system) {` | The parent `Phone` needs a brand and a battery, but the child never calls the parent constructor. | Add `: super(brand, battery)` before the `{` |
| 3 | `@override void restart() {` (in `SmartPhone`) | `@override` only works if the parent has the same method. `Phone` has no `restart()`. | Add a `restart()` method to `Phone` (or remove `@override`) |
| 4 | `p.battery = 100;` | `Phone` only has a getter for `battery`, so it cannot be assigned. | Add `set battery(int value)` that accepts only 0 to 100 |
| 5 | `print(p.battery());` | `battery` is a getter, so it is used like a field. Adding `()` tries to call an int as a function. | `print(p.battery);` |
| Not a bug | `Phone p = new Phone('Tecno', 50);` | `new` is old style and not needed in modern Dart, but it is still allowed, so the line works. | (none) |

The fixed `bug_hunt.dart` prints:

```
100
```

## Part 5

**My prediction:**

```
1
5
```

Why:
- `Bag a`: `add()` makes `_items` = 1. Then `a.items = -5` calls the setter, but
  the rule `value >= 0` is false, so the setter does nothing. `_items` stays 1,
  so it prints **1**.
- `BigBag b`: its `add()` is overridden and calls `super.add()` twice, so each
  `b.add()` adds 2. Two calls give 0 + 2 + 2 = 4. Then `b.items = b.items + 1`
  is `b.items = 5`, which passes the rule (5 >= 0), so it prints **5**.

**Real output:**

```
1
5
```

**Was I right?** Yes, the output matched my prediction.