
# mr-gateway
<p dir="rtl">
شما به وسیله mr-gateway میتوانید یک درگاه تستی برای پرداخت های سمت develop ایجاد کنید و بدون استفاده از کارت بانکی با مبالغ تستی سیستم خود را امتحان کنید.
بزرگترین مزیت mr-gateway این است که سرور پرداخت تستی را روی لوکال خود ایجاد میکنید و به راحتی مقادیری که برای پرداخت لازم دارید را متناسب با نیاز خود شخصی سازی میکنید.
</p>
<p dir="rtl">
ما برای راحتی کار سعی کردیم که از روال پرداخت به پرداخت ملت استفاده کنیم. ولی شما میتوانید به هر صورتی که خواستی پارامتر های ورودی و خروجی رو متناسب با نیاز خود تغییر دهید.
</p>

<h2 dir="rtl">پیش نیاز</h2>
<p dir="rtl">
1 - ابتدا mongoDb را نصب کنید و یک دیتابیس با هر نامی که دوست دارید بسازید.
</p>
<p dir="rtl">
2- پروژه را از مسیر `https://github.com/MostafaRastegar/mr-gateway.git` کلون کنید.
</p>
<p dir="rtl">
3- پروژه را توسط `npm install` یا `yarn` نصب کنید.
</p>
<p dir="rtl">
4- یک کپی با نام env از روی env.example بسازید. و مقادیر داخل آن را متناسب با اطلاعات دیتابیس خود تغییر دهید. به صورت پیش فرض پروژه روی (http://localhost:4002) اجرا می شود. در صورتی که mongoDb نصب شده فاقد نام کاربری و رمز عبور است, میتوانید این مقادیر را در فایل env به صورت خالی قرار دهید.
</p>
<p dir="rtl">
5- از فولدر postman در مسیر اصلی پروژه ,میتوانید پکیج مربوط به api ها را بردارید و استفاده کنید.
</p>
<p dir="rtl">
6- پس از نصب پروژه و اتصال به دیتابیس برای قرارگرفتن مقادیر اولیه و ساخت کالکشن های مربوط به آن در دیتابیس, کافی است یک بار آدرس `/` را اجرا کنید. (InitCollections در پکیج postman همین وظیفه را بر عهده دارد)
<p>
<p dir="rtl">
در این لحظه برای شما یک نام کاربری و رمز عبور به همراه شماره ترمینال ایجاد شد. (برای مشاهده کاربر ساخته شده میتوانید به `/users` مراجهه کنید.)
</p>

<h2 dir="rtl">شروع به کار</h2>
<p dir="rtl">
برای راه اندازی mr-gateway کار شما به پایان رسید. از الان میتوانید پرداخت های تستی خود را انجام دهید.
</p>

<h2 dir="rtl">شرح متد های اصلی:</h2>
<h2 dir="rtl">1- متد درخواست تراكنش پرداخت payRequet</h2>
<p dir="rtl">
با استفاده از این متود شما درخواست یک تراکنش را ایجاد میکنید.
</p>

| نوع | توضيح |مثال  | نام پارامتر | رديف |
|--|--|--|--|--|
| Long |  ترمینال پذیرنده|0 |terminalId  | 1 |
|  String|نام كاربري پذيرنده  | mr_gateway | userName |  2|
| String | كلمه عبور پذيرنده | mr_gateway_123456 |userPassword  |3  |
| Long | شماره فاکتور | 3355 | orderId |  4|
| Long |مبلغ  | 650000 | amount |  5|
| String | تاریخ |20201606  | localDate |6  |
| String | ساعت | 101920 | localTime | 7 |
| String | توضبحات پرداخت | this is for test | additionalData | 8 |
| String | آدرس بازگشتی به سایت پذیرنده | http://localhost:4001 | callBackUrl | 9 |
| long  | شناسه پرداخت | 10 | payerId | 10 |

<p dir="rtl">
در صورتی که پارامتر های ارسالی شما درست باشد. یک کد یکتا برای درخواست شما ایجاد میشود.
</p>

    {
		"success": "true",
		"message": "Create refId successfully",
		"data": {
			"resCode": 0,
			"refId": "93485c37-7f59-4426-9838-348db81c3bf0"
		}
	}

<p dir="rtl">
شما با دریافت resCode صفر متوجه میشوید که اطلاعات ارسالی تایید شده است و یک کد یکتا به عنوان refId برای ادامه کار دارید.
</p>

<h2 dir="rtl">2 - انتقال کاربر به صفحه payAction</h2>
<p dir="rtl">
با refId ای که در اختیار دارید کاربر را به صفحه payAction پست میکنید. در اینجا کاربر مبلغ درخواستی خود را مشاهده میکند و با زدن دکمه تایید یا انصراف به صفحه completePayment منتقل شده و پرداخت, نهایی میشود و به صورت خودکار به callback ارسالی در payRequest بر میگردد.
</p>

<h2 dir="rtl">3- درخواست completePayment </h2>
<p dir="rtl">
بعد از نهایی شدن پرداخت توسظ completePayment مقادیر زیر به عنوان تایید پرداخت برای سایت پذیرنده post می شود.
</p>

| نوع | توضيح |مثال  | نام پارامتر | رديف |
|--|--|--|--|--|
| String |  کد یکتا تولید شده توسط mrRequest| 93485c37-7f59-4426-9838-348db81c3bf0 |refId  | 1 |
|  String|پرداخت موفق  | 0 | resCode |  2|
| Long | شماره فاکتور | 3355 |saleOrderId  |3  |
| Long |كد مرجع تراكنش | 8107178 | saleReferenceId |  4|
