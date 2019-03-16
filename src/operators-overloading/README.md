<div dir="rtl">

## 

کاتلین تعداد مشخصی از آوپراتور های نمادین دارد که میتونیم به راحتی بر روی هر کلاسی استفاده کنیم. روشش هم ایجاد یک تابع با نام رزرو شده و مپ کردن اون به سمبله. Overload کردن این اوپراتور ها ساده نویسی و خانایی کد رو بیشتر کنه.

اگر بخوایم کامپایلر رو آگاه کنیم که یک اوپراتور رو میخوایم overload کنیم، باید اون تابع رو با اصلاح کننده  `operator` مشخص کنیم.

### جدول اوپراتور ها

در جدول زیر هم میتونید یک سری از جدول ها رو ببینید  که شامل اوپراتورها و دستور العمل های مطابق آن است. یک تابع که پیاده سازی شده باشد برای استفاده از آن اوپراتور در کلاس مشخص نیاز است.

</div>

##### Unary Operations

|                |                               |
|----------------|-------------------------------|
|`+a`            |`a.unaryPlus()`                |
|`-a`            |`a.unaryMinus()`               |
|`!a`            |`a.not()`                      |
|`a++`           |`a.inc()`                      |
|`a--`           |`a.dec()`                      |


##### Binary Operations

|                |                               |
|----------------|-------------------------------|
|`a + b`         |`a.plus(b)`                    |
|`a - b`         |`a.minus(b)`                   |
|`a * b`         |`a.times(b)`                   |
|`a / b`         |`a.div(b)`                     |
|`a % b`         |`a.mod(b)`                     |
|`a..b`          |`a.rangTo(b)`                  |
|`a in b`        |`b.contains(a)`                |
|`a !in b`       |`!b.contains(a)`               |
|`a += b`        |`a.plusAssign(b)`              |
|`a -= b`        |`a.minusAssign(b)`             |
|`a *= b`        |`a.timesAssign(b)`             |
|`a /= b`        |`a.divAssign(b)`               |
|`a %= b`        |`a.modAssign(b)`               |

##### Array-like operations

|                       |                               |
|-----------------------|-------------------------------|
|`a[i]`                 |`a.get(i)`                     |
|`a[i, j]`              |`a.get(i, j)`                  |
|`a[i_1,..., i_n]`      |`a.get(i_1,..., i_n)`          |
|`a[i] = b`             |`a.set(i, b)`                  |
|`a[i, j] = b`          |`a.set(i, j, b)`               |
|`a[i_1,..., i_n] = b`  |`a.set(i_1, …, i_n, b)`        |


##### Equals operation

|                       |                                    |
|-----------------------|------------------------------------|
|`a == b`               |`a?.equals(b) ?: b === null`        |
|`a != b`               |`!(a?.equals(b) ?: b === null)`     |


<div dir="rtl">

عملگرهای `equals` مقداری متفاوتند، زیرا برای بهتر چک کردن تساوی از ترجمه ی پیچیده تری استفاده میکند که در نتیجه تابع باید دقیقا به صورت زیر پیاده سازی شود.

</div>

````kotlin
operator fun equals(other: Any?): Boolean
````

##### Function invocation

|                       |                                    |
|-----------------------|------------------------------------|
|`a(i)`                 |`a.invoke(i)`                       |
|`a(i, j)`              |`a.invoke(i, j)`                    |
|`a(i_1, …, i_n)`       |`a.invoke(i_1,..., i_n)`            |

