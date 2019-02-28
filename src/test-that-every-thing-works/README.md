<div dir="rtl">

# تست کارکردن همه اجزا

اول لازمه که به فایل `activity_main.xml` مراجعه کنین و یک `id` برای `TextView` تون انتخاب کنین.

</div>

```xml
<TextView
    android:id="@+id/message"
    android:text="@string/hello_world"
    android:layout_width="wrap_content"
    android:layout_height="wrap_content"/>
```
<div dir="rtl">

حالا کافیه برگردین به MainActivity.java و در قسمت `()onCreate` این تغییرات رو بدین:

</div>

```kotlin
override fun onCreate(savedInstanceState: Bundle?) {
    super.onCreate(savedInstanceState)
    setContentView(R.layout.activity_main)
    message.text = "Hello Kotlin!"
}
```

<div dir="rtl">

به لطف همکاری کاتلین با جاوا ما میتونین متدهای `setter` و `getter` رو به صورت خصیصه ای انجام بدیم.حالا بعدا بیشتر درباره خصیصه¬ها صحبت میکنیم ولی اینجا اگه دقت کنین به جای `()message.setText` مستقیم مقدار جدید رو به `text` دادیم.درواقع کامپایلر خودش بعدا از متد جاوای واقعی استفاده میکنه و این عملکرد رو خراب نمیکنه! فقط خوبیش برای ما توسعه دهنده هاست که با راحتی بیشتر کارامون رو انجام میدیم.

یک چیز دیگه هم فراموش کردم بهتون بگم.کو `findViewById` ایمون؟ کاتلین در واقع این اجازه رو میده که ما نیازی به Bind کردن کد جاوامون با فایل xml نداشته باشیم و به صورت مستقیم و با استفاده از `id` به اون کامپوننت دسترسی داشته باشیم. اینجا هم تنها کاری که لازم بود انجام بدیم اینه که message رو صدا بزنیم.

حالا اگه برنامه رو اجرا کنیم متوجه خواهیم شد که برنامه به درستی کار میکنه و مقدار جدید رو نشون میده.

</div>
