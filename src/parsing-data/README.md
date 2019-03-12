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

<div dir="rtl">

## شکل دادن به لایه ی `domain`

حالا نوبت ساختن پکیج نمایش دهنده ی لایه ی `domain` هه که شامل چند `command` هه! ولی برای شروع اول باید `command` رو تعریف کنیم.

</div>

```kotlin
public interface Command<T> {
    fun execute(): T
}
```

<div dir="rtl">

این دستورات (`command`) یک فعالیت رو اجرا میکنن و یک آبجکت رو که جنس کلاسش به صورت `generic type` مشخص شده رو برمیگردونن. یکی از ویژگی های جالب اینه که هر تابعی در کاتلین یک مقدار برمیگردونه! به صورت پیشفرض، اگر جنس مقداری که برگردانده خواهد شد مشخص نبود، یک آبجکت از جنس کلاس `Unit` رو برمیگدونه. بنابراین اگر میخوایم که `command` مون چیزی برنگردونه، میتونیم جنسش رو `Unit` نیز معرفی کنیم.

`Interface` ها در کاتلین، قدرتمند تر از جاوا هستند چون میتونن شامل کد هم بشن ولی برای فعلا ما کاری به این ویژگی نداریم. در فصل های آینده بیشتر در مورد این موضوع توضیح خواهیم داد.

در دستور اول، نیازه که اول پیشبینی رو از API درخواست کنیم و بعد به کلاس `domain` تبدیل کنیمش. کد کلاس های `domain` رو در زیر ببینیم :


</div>

```kotlin
data class ForecastList(val city: String, val country: String,
                        val dailyForecast:List<Forecast>)
data class Forecast(val date: String, val description: String, val high: Int,
                    val low: Int)
```

<div dir="rtl">

حالا احتمالا این کلاس ها رو بعدا وقتی ویژگی های دیگه ای اضافه شد، بازدید میکنیم ولی فعلا برای دیتایی که میخوایم نگه داریم کافیه.

کلاس ها باید از `data` به مدل `domain` مپ بشن، بنابراین فعالیت بعدی ساخت `DataMapper` اهه.


</div>

```kotlin
public class ForecastDataMapper {

    fun convertFromDataModel(forecast: ForecastResult): ForecastList {
        return ForecastList(forecast.city.name, forecast.city.country,
                convertForecastListToDomain(forecast.list))
    }
    
    private fun convertForecastListToDomain(list: List<Forecast>):
            List<ModelForecast> {
        return list.map { convertForecastItemToDomain(it) }
    }
    
    private fun convertForecastItemToDomain(forecast: Forecast): ModelForecast {
        return ModelForecast(convertDate(forecast.dt),
                forecast.weather[0].description, forecast.temp.max.toInt(),
                forecast.temp.min.toInt())
    }
    
    private fun convertDate(date: Long): String {
        val df = DateFormat.getDateInstance(DateFormat.MEDIUM,
                Locale.getDefault())
        return df.format(date * 1000)
    }
}
```

<div dir="rtl">

در این حین که داریم از دو کلاس با نام های یکسان استفاده میکنیم، باید به یکی، نام مشخص و مجزایی بدیم تا نیازی به نوشتن نام کامل پکیج نداشته باشیم

</div>

```kotlin
import com.antonioleiva.weatherapp.domain.model.Forecast as ModelForecast
```

<div dir="rtl">

یکی از بخش های جالب این کد، نحوه ی تبدیل لیست پیشبینی، از `data` به مدل `domain` است.

</div>

```kotlin
return list.map { convertForecastItemToDomain(it) }
```


<div dir="rtl">

در یک خط، ما میتونیم بر روی یک collection حلقه ای رو تکرار کنیم و یک لیست جدید با ایتم های کانورت شده برگردونیم. کاتلین یک سری از توابع اعمالی خوبی بر روی لیست ها مهیا کرده که یک عمل را بر روی تمامی ایتم های لیست اجرا میکند و به نحو دلخواه انتقال میده! این یکی از قدرتمندترین ویژگی های کاتلین برای توسعه دهندگانی است که از java 7 استفاده میکردند. به زودی یک نگاهی به این توابع خواهیم انداخت. این مهمه که بدونیم آنها وجود دارن، زیرا که مواقع زیادی پیدا میشوند که استفاده از این توابع باعث صرفه جویی در زمان و خط های بیهوده میشود.

</div>



