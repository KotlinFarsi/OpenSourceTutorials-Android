<div dir="rtl">

# تجزیه دیتا

## تبدیل JSON به کلاس های دیتا

حالا که میدونیم چگونه کلاس های دیتا را درست کنیم، آماده ی شروعِ تجزیه اطلاعات هستیم. در پکیج `data`، فایلی به نام `ResponseClasses.kt` ایجاد کنید. به وسیله `url` ای که در بخش 8 استفاده کردیم میتوانید ساختار فایل `json` رو ببینید که تشکیل شده از یک object است که یک شهر را داخل خودش دارد و یک لیست از پیشبینی ها. شهر یک `id` ، یک `name` ، مختصات و این که متعلق به چه کشوری هست نیز را دارا میباشد. هر پیشبینی به همراه اطلاعاتی مانند `date` و دماهای متفاوت و همچنین یک object از آب و هوا را داراست، که اون آبجکت توضیحات و همچنین یک `id` از یک `icon` رو به همراه خودش داره.


</div>

```kotlin
data class ForecastResult(val city: City, val list: List<Forecast>)

data class City(val id: Long, val name: String, val coord: Coordinates,
                val country: String, val population: Int)
                
data class Coordinates(val lon: Float, val lat: Float)

data class Forecast(val dt: Long, val temp: Temperature, val pressure: Float,
                    val humidity: Int, val weather: List<Weather> ,
                    val speed: Float, val deg: Int, val clouds: Int, 
                    val rain: Float)
                    
data class Temperature(val day: Float, val min: Float, val max: Float, 
                       val night: Float, val eve: Float, val morn: Float)
                        
data class Weather(val id: Long, val main: String, val description: String, 
                   val icon: String)
```

<div dir="rtl">

همینطور که از Gson برای تجزیه به کلاس هامون استفاده کردیم،  باید در نظر داشته باشیم که خصیصه ها باید هم نامِ یکی های خود در فایل json باشند یا یک نام serialized شده را معرفی کنند. یکی از روش هایی که در اکثر معماری های برنامه نویسی توضیح داده میشود، استفاده از مدل های متفاوت برای لایه های متفاوت به جهت مجزا سازی آنها در برنامه امان است. بنابراین من ترجیح میدهم که کلاس ها را به صورت ساده declare کنم، به این دلیل که من آنها را قبل از این که در بقیه برنامه استفاده شوند تبدیل خواهم کرد. نام های خصیصه ها در اینجا دقیقا به مانند نام های موجود در json پاسخ داده شده است.

حالا، کلاس `Request` برای برگرداندن جواب تجزیه شده نیاز به تغییرات دارد. همچنین برای خوانایی بیشتر، تنها نیاز به `zipcode` شهر دارد و از `url` کامل استفاده نخواهد شد. پس فعلا `static url` را به `companion object` خواهیم داد چون که شاید بعدا برای درخواست های بیشتر نیازی به آن پیدا کنیم.

* 	`Companion object` ها: کاتلین اجازه میده که ابجکت هایی رو تعریف کنیم که رفتار `static` طوری داشته باشند. ما نمیتونیم خصیصه ها و یا توابعی `static` درست کنیم، پس باید به object ها روی بیاریم. اگرچه این کار نحوه پیاده سازی Singleton را ساده تر خواهند کرد. اگر به تعدادی خصیصه، `constant` و یا توابع `static` در کلاس نیاز داشته باشیم، از `companion object` استفاده میکنیم. این ابجکت در سرتاسر نسل های ساخته شده از کلاس قابل اشتراک گذاری است، دقیقا به مانند static در جاوا.  

به کد نتیجه گیری شده زیر نگاه کنید:

</div>

```kotlin
class ForecastRequest(val zipCode: String) {

    companion object {
        private val APP_ID = "15646a06818f61f7b8d7823ca833e1ce"
        private val URL = "http://api.openweathermap.org/data/2.5/" +
                "forecast/daily?mode=json&units=metric&cnt=7"
        private val COMPLETE_URL = "$URL&APPID=$APP_ID&q="
    }

    fun execute(): ForecastResult {
        val forecastJsonStr = URL(COMPLETE_URL + zipCode).readText()
        return Gson().fromJson(forecastJsonStr, ForecastResult::class.java)
    }
}
```

<div dir="rtl">

فراموش نکنید که کتابخانه Gson را به `build.gradle` اضافه کنید

</div>

```groovy
compile "com.google.code.gson:gson:2.4"
```




