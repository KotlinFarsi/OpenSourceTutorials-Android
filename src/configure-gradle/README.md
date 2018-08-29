<div dir="rtl">

# تنظیمات گردل

پلاگین کاتلین شامل ابزار هایی میشه که تنضیمات gradle رو برامون انجام میدن ولی من ترجیح میدم که کنترلی بر چیزی که در فایل گردلم مینویسم داشته باشم در غیر این صورت ممکنه کمی بهم ریخته بشن. به هرصورت این ایده خوبیه که بدونیم چجوری کارها انجام میشن قبل از این که از ابزار های اتومات استفاده کنیم، بنابراین این دفعه دستی کارمون رو انجام میدیم.
اولین کار اینه که فایل build.gradle امون رو پیدا کنیم که تقریبا میشه گفت شبیه اینه :

</div>

```groovy
buildscript {
    ext.support_version = '23.1.1'
    ext.kotlin_version = '1.1.3'
    ext.anko_version = '0.8.2'
    repositories {
        jcenter()
    }
    dependencies {
        classpath 'com.android.tools.build:gradle:1.5.0'
        classpath "org.jetbrains.kotlin:kotlin-gradle-plugin:$kotlin_version"
    }
}
allprojects {
    repositories {
        jcenter()
    }
}
```

<div dir="rtl">

همینطور که میبینین ما داریم یک متغیر ایجاد میکنیم که ورژن درحال حاظر کاتلینمون رو بگه. حتما چک کنید که این ورژن با ورژن در حال حاظر استفاده شما یکی باشه.سعی کنید همیشه از اخرین ورژن استفاده کنید.ما به این شماره ورژن در چندین جا نیاز داریم.به عنوان مثال وقتی که از kotlin standard library استفاده میکنیم باید مشخص کنیم که از چه ورژنی استفاده میشه.
عین همین کار رو برای لایبری support و anko انجام میدیم.این بهمون این مزیت رو میده که از شماره ورژن­هامون هرجایی استفاده کنیم.
ما بعدا وابستگی هایی مثل kotlin standard library و anko library رو مثل kotlin و kotlin android extensions رو به برناممون اضافه میکنیم.

</div>

```groovy
apply plugin: 'com.android.application'
apply plugin: 'kotlin-android'
apply plugin: 'kotlin-android-extensions'
android {
    …
}
dependencies {
    compile "com.android.support:appcompat-v7:$support_version"
    compile "org.jetbrains.kotlin:kotlin-stdlib:$kotlin_version"
    compile "org.jetbrains.anko:anko-common:$anko_version"
}
buildscript {
    repositories {
        jcenter()
    }
    dependencies {
        classpath "org.jetbrains.kotlin:kotlin-android-extensions:$kotlin_version"
    }
}
```

<div dir="rtl">

درواقع Anko یک کتابخونه است که با استفاده از قدرت Kotlin بعضی کارهارو برامن ساده­تر میکنه.این کتابخونه به چندین قسمت مختلف تقسیم شده تا اگر از قسمتی استفاده نمیکنیم الکی اونو به پروژه اضافه نکنیم.