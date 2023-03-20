# irantether-doc

# راهنمای نصب و استفاده اسکریپت ایران تتر

<br>
<br>

### پیش نیاز های نصب

<br>
<br>

```
php ^8.1

node js

npm 

php-gmp extention

redis

```

## مراحل نصب


### بارگذاری پروژه

سرور و یا هاست خود آپلود کنید public_html را در public_html در مرحله اول محتویات پوشه

![public_html](/img/01.PNG "public_html")


.پس از آپلود باید ب ه شکل بالا باشد


ب ه همراه محتویات را application سپس در هاست یا سرور خود ب ه یک پوش ه عقب تر رفت ه و پوش ه
.آپلود کنید


### ایجاد دیتابیس

به مدیریت هاست خود رفته و یک دیتابیس بسازید

![database](/img/02.PNG "database")

را در دیتابیس سا خته database.sql رفته و فایل (phpmyadmin) به مدیریت دیتابیس سایت
شده در مراحل قبل آپلود کنید.

![phpmyadmin](/img/03.PNG "phpmyadmin")


وجود دارد اطلاعات خود را جایگزین کنید .env یک فایل کانفیگ به نام application در پوشه

<br>

.در این مرحل ه با وارد کردن آدرس سایت خود سایت شما با موفقت نمایش داده می شود

<br>
<br>

### نصب موارد مورد نیاز در سرور

* توجه این دستورات به صورت پیش فرض برای سیستم اوبونتو میباشد

#### نصب redis
<br>
<br>

```
sudo apt-get install redis
```

<br>
<br>

#### نصب nodejs

<br>
<br>

```
curl -sL https://deb.nodesource.com/setup_18.x -o nodesource_setup.sh

sudo bash nodesource_setup.sh & sudo 

sudo apt-get install -y nodejs

```

#### نصب pm2 , laravel-echo-server

```
npm install -g pm2

npm install -g laravel-echo-server

```
<br>
<br>

#### نصب supervisor

<br>
<br>

```
sudo apt-get install supervisor
```


### .پس از نصب موارد بالا کد زیر را اجرا کنید

```
pm2 start /home/USER/domains/DOMAIN/application/nodejs/tronNetwork.js

pm2 save

pm2 startup
```
* توجه به جای USER و DOMAIN اطلاعات خود را وار کنید

* .کد بالا سرویس شبک ه ترون را برای بررسی تراکنش ها فعال میکند

<br>
<br>

#### راه اندازی سرویس سوکت

<br>

در پوشه application یک فایل با نام laravel-echo-server.conf وجود دارد آن را ویرایش و اطلاعات خود را وارد کنید.

<br>


#### راه اندازی سرویس supervisor

<br>
<br>

```
nano /etc/supervisor/conf.d/irantether.conf
```

<br>

و سپس اطلاعات زیر را در فایل بالا وارد کنید


<br>
<br>

```
[group:laravel]
programs=queue,schedule,echo

[program:queue]
command=php /home/USER/domains/DOMAIN/application/artisan queue:work
numprocs=1
autostart=true
autorestart=true
user=root

[program:schedule]
command=php /home/USER/domains/DOMAIN/application/artisan schedule:run
numprocs=1
autostart=true
autorestart=true
user=root
startsecs=0

[program:echo]
command=laravel-echo-server start --dir=/home/USER/domains/DOMAIN/application
numprocs=1
autostart=true
autorestart=true
user=root
startsecs=0

```
<br>
<br>

* اطلاعات خودتون را وارد کنید . DOMIN و USER به جای

<br>

و در نهایت دستورات زیر را اجرا کنید

<br>

```
supervisorctl reread
supervisorctl update
```