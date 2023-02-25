# اصول کد نویسی تمیز در جاوااسکریپت

در ترجمه این داکیومنت، تلاش شده تا محتوا برای برنامه‌نویس‌های مبتدی بهینه بشه و در عین حال محتوا به درستی در دسترس قرار بگیره. پاراگراف‌هایی که با ** شروع شدن مواردی هستن که برای درک بهتر مطالب اضافه شدن و جزئی از داکیومنت اصلی نیستن.

اگر درک مطلبی براتون سخته و پیشنهادی برای بهتر شدنش دارید، ممنون میشم باهام درمیون بذارید:

Telegram: [moein_mirkiani](https://t.me/moein_mirkiani)

Email: moein.mirkiani@gmail.com

## فهرست محتوا

1. [مقدمه](#مقدمه)
2. [متغیرها](#متغیرها)
3. [تابع‌ها](#تابعها)
4. [آبجکت‌ها و ساختارهای داده](#آبجکتها-و-ساختارهای-داده)
5. [کلاس‌ها](#کلاسها)
6. [اصول SOLID](#اصول-solid)
7. [تست](#تست)
8. [همزمانی](#همزمانی)
9. [مدیریت خطا](#مدیریت-خطا)
10. [قالب بندی](#قالب-بندی)
11. [توضیحات](#توضیحات)
12. [ترجمه](#ترجمه)

## مقدمه

![Humorous image of software quality estimation as a count of how many expletives
you shout when reading code](https://www.osnews.com/images/comics/wtfm.jpg)

اصول مهندسی نرم افزار، از کتاب [Clean Code](https://www.amazon.com/Clean-Code-Handbook-Software-Craftsmanship/dp/0132350882) رابرت سی. مارتین، اقتباس شده برای جاوا اسکریپت. این اصول، یک سبک برنامه‌نویسی نیستن، بلکه راهنمای نوشتن نرم‌افزارهایی هستن که خوانایی دارن ([readable](https://github.com/ryanmcdermott/3rs-of-software-architecture))، قابلیت استفاده مجدد دارن ([reusable](https://github.com/ryanmcdermott/3rs-of-software-architecture)) و قابلیت تغییر ساختار کد رو دارن بدون اینکه عملکردشون تغییر کنه  ([refactorable](https://github.com/ryanmcdermott/3rs-of-software-architecture)).

لزومی نداره همه این اصول به طور دقیق رعایت بشن، حتی تعداد کمی از اون‌ها مورد توافق جهانی قرار می‌گیرن. این اصول صرفا تعدادی دستورالعمل هستند و نه بیشتر، اما این دستورالعمل‌ها طی سال‌ها تجربه جمعی، توسط نویسندگان Clean Code تدوین شدن.

عمر مهندسی نرم افزار کمی بیش از 50 سال هست و ما هنوز در حال یادگیری هستیم. وقتی معماری نرم افزار به اندازه خود معماری قدیمی بشه، شاید قوانین سخت‌تری برای پیروی داشته باشیم. اما تا اون موقع، بیاید این دستورالعمل‌ها رو به عنوان سنگ محک برای ارزیابی کیفیت کد جاوا اسکریپتی که شما و تیم‌تون تولید می کنید، در نظر بگیریم.

یک نکته دیگه: دونستن این موارد فوراً شما رو به یک توسعه‌دهنده نرم‌افزار بهتر تبدیل نمی‌کنه و داشته تجربه چند ساله در استفاده از اون‌ها هم به این معنی نیست که اشتباه نخواهید کرد. هر قطعه کد رو مثل خاک رس مرطوب در نظر بگیرید که قراره یه شکلی به خودش بگیره. در نهایت، وقتی که با شکل (قطعه کد) های هم‌تراز خودش مقایسه بشه، متوجه نواقص و عیوبش میشیم و اون‌هارو رفع می‌کنیم. برای اولین کدهایی که نوشتین و نیاز به بهبود دارند، خودتون رو سرزنش نکنید، در عوض کد رو بهتر کنید!

## **متغیرها**

### از اسم‌های معنادار و قابل تلفظ برای اسم متغیرها استفاده کنید

**بد:**

```javascript
const yyyymmdstr = moment().format("YYYY/MM/DD");
```

**خوب:**

```javascript
const currentDate = moment().format("YYYY/MM/DD");
```

**[⬆ بازگشت](#فهرست-محتوا)**

### از واژ‌ه‌های یکسان برای یک نوع متغیر استفاده کنید

**بد:**

```javascript
getUserInfo();
getClientData();
getCustomerRecord();
```

**خوب:**

```javascript
getUser();
```

**[⬆ بازگشت](#فهرست-محتوا)**

### از اسم‌های قابل جستجو استفاده کنید

ما خیلی بیشتر از اون که کد بنویسم، کد می‌خونیم. بسیار مهمه که کدی که می‌نویسیم قابل خوندن و  قابل جستجو باشه. اگر متغیرهارو به صورت معنادار و قابل درک نامگذاری نمی‌کنیم، داریم خواننده‌های اون کد رو اذیت می‌کنیم. مقادیر ثابتی (constant) که استفاده می‌کنید رو قابل جستجو کنید. ابزارهایی مثل
[buddy.js](https://github.com/danielstjules/buddy.js) و [ESLint](https://github.com/eslint/eslint/blob/660e0918933e6e7fede26bc675a0763a6b357c94/docs/rules/no-magic-numbers.md) می‌تونن به شناسایی مقادیر ثابت (constant) که اسم ندارن کمک کنن.

**بد:**

```javascript
// What the heck is 86400000 for?
setTimeout(blastOff, 86400000);
```

**خوب:**

```javascript
// Declare them as capitalized named constants.
const MILLISECONDS_PER_DAY = 60 * 60 * 24 * 1000; //86400000;

setTimeout(blastOff, MILLISECONDS_PER_DAY);
```

**[⬆ بازگشت](#فهرست-محتوا)**

### از متغیرهای توضیحی استفاده کنید

**بد:**

```javascript
const address = "One Infinite Loop, Cupertino 95014";
const cityZipCodeRegex = /^[^,\\]+[,\\\s]+(.+?)\s*(\d{5})?$/;
saveCityZipCode(
  address.match(cityZipCodeRegex)[1],
  address.match(cityZipCodeRegex)[2]
);
```

**خوب:**

```javascript
const address = "One Infinite Loop, Cupertino 95014";
const cityZipCodeRegex = /^[^,\\]+[,\\\s]+(.+?)\s*(\d{5})?$/;
const [_, city, zipCode] = address.match(cityZipCodeRegex) || [];
saveCityZipCode(city, zipCode);
```

** استفاده از regex با متد match، همیشه یه آرایه برمی‌گردونه. اولین المنت این آرایه همیشه عبارت کلی‌ای هست که regex اون رو شناسایی کرده و المنت‌های بعدی این آرایه، به ترتیب گروه‌های کوچک‌تری از اون عبارت هستن که توسط regex شناسایی شد. این گروه‌ها با پرانتزهای باز و بسته تعیین میشن. به عنوان مثال همونطور که در کد بالا می‌بینید، regex تعریف شده دو گروه داره که با رنگ سفید مشخص شدن:

```javascript
const cityZipCodeRegex = /^[^,\\]+[,\\\s]+(.+?)\s*(\d{5})?$/;
```

** به دلایل زیر کد دوم، کد بهتری به حساب میاد:

•	از `destructuring` برای مقداردهی به متغیرهای `city` و `zipCode` استفاده شده.

•	در انتهای خط سوم از یک آرایه خالی استفاده کرده. در صورتی که متد `match` مقدار `null` رو برگردوند (وقتی متغیر `address` با `regex` ما همخوانی نداشت)، این آرایه خالی به عنوان مقدار پیشفرض قرار می‌گیره و از بروز خطاهای احتمالی جلوگیری می‌کنه.

•	همونطور که گفته شد، متد `match` یه آرایه برمی‌گردونه که در این مثال این آرایه به این شکل خواهد بود:

```javascript
["One Infinite Loop, Cupertino 95014","Cupertino","95014"]
```

در خط سوم که با `destructuring` سه متغیر `_` و `city` و `zipCode` مقداردهی شدن، در واقع متغیر `_` بدون استفاده است و با این روش به راحتی از ساختار کد حذف می‌شه. مقدار این متغیر برابر با اولین المنت آرایه بالاست که نیازی بهش نداریم. دو المنت بعدی این آرایه همون گروه‌های کوچک‌تر `regex` هستند که بالاتر صحبتش رو کردیم و به عنوان مقادیر متغیرهای `city` و `zipCode` قرار می‌گیرن و به این ترتیب فقط با یک بار فراخوانی متد `match،` دو متغیر `city` و `zipCode` مقداردهی میشن.


**[⬆ بازگشت](#فهرست-محتوا)**

### از نامگذاری‌های ذهنی پرهیز کنید

اینکه صراحتا از یه واژه برای نامگذاری استفاده کنید، بهتر از اینکه اشاره ضمنی به اون واژه داشته باشید. در مثال زیر برای لوکیشن بهتره از خود واژه `location` استفاده کنیم و استفاده از تک حرف `l` توصیه نمیشه.

**بد:**

```javascript
const locations = ["Austin", "New York", "San Francisco"];
locations.forEach(l => {
  doStuff();
  doSomeOtherStuff();
  // ...
  // ...
  // ...
  // Wait, what is `l` for again?
  dispatch(l);
});
```

**خوب:**

```javascript
const locations = ["Austin", "New York", "San Francisco"];
locations.forEach(location => {
  doStuff();
  doSomeOtherStuff();
  // ...
  // ...
  // ...
  dispatch(location);
});
```

**[⬆ بازگشت](#فهرست-محتوا)**

### محتوای غیر ضروری اضافه نکنید

اگر نام کلاس/آبجکت شما نشون دهنده چیزی هست، اون رو در اسم متغیرهاتون تکرار نکنید.

**بد:**

```javascript
const Car = {
  carMake: "Honda",
  carModel: "Accord",
  carColor: "Blue"
};

function paintCar(car, color) {
  car.carColor = color;
}
```

**خوب:**

```javascript
const Car = {
  make: "Honda",
  model: "Accord",
  color: "Blue"
};

function paintCar(car, color) {
  car.color = color;
}
```

**[⬆ بازگشت](#فهرست-محتوا)**

### به جای استفاده از عبارت‌های شرطی و میانبر، از پارامترهای پیشفرض استفاده کنید

استفاده از پارامترهای پیشفرض، جمع و جورتر از وقتیه که مثلا با منطق‌های شرطی به یه پارامتر مقدار میدیم. دقت داشته باشید که این مقدار پیشفرض فقط درحالی کاربرد داره که پارامتر مقدار `undefined` داشته باشه. دیگر مقادیر اصطلاحا `falsy` مثل `""`  ، `''` ، `false` ، `null` ، `0` و `NaN` با مقدار پیفرض جایگزین نخواهند شد.

**بد:**

```javascript
function createMicrobrewery(name) {
  const breweryName = name || "Hipster Brew Co.";
  // ...
}
```

**خوب:**

```javascript
function createMicrobrewery(name = "Hipster Brew Co.") {
  // ...
}
```

**[⬆ بازگشت](#فهرست-محتوا)**

## **تابع‌ها**

### تعداد پارامترهای تابع (در حالت ایده آل 2 یا کمتر)

محدود کردن تعداد پارامترهای تابع از این جهت مهمه که فرآیند تست تابع رو آسون‌تر می‌کنه. داشتن بیش از سه پارامتر منجر به افزایش تصاعدی ترکیب‌های ممکن می‌شه که در این صورت باید هزاران حالت مختلف رو با هر استدلال جداگانه تست کنید.

یک یا دو پارامتر ایده آله و تا حد امکان بهتره از پارامتر سوم اجتناب شه. هر چیزی بیش از این باید تجمیع شود. معمولاً اگه پارامترهاتون بیشتر از دوتاست، یعنی تابع شما دارای کارای زیادی رو انجام میده. در غیر این صورت، فقط یه آبجکت سطح بالا (high-level object) برای تباع کافی خواهد بود.

** آبجکت سطح بالا در جاوااسکریپت به آبجکت‌هایی گفته میشه که عملکردهای بیشتری نسبت به آبجکت‌هایی که ما تعریف می‌کنیم دارن. به عنوان مثال آبجکت‌هایی که خود جاوااسکریپت در اختیار ما می‌ذاره (Array، String، Number و غیره) که عملکردهای بیشتر و متدهایی مثل sort، map و toFixed دارن و مختص همین آبجکت‌هاست.

از اونجایی که جاوااسکریپت این امکان رو میده که بدون درگیری با کلاس‌های زیاد، آبجکت بسازین، اگر دیدین به پارامترهای زیادی نیاز دارین، می‌تونین از یک آبجکت استفاده کنین.

برای تعیین اینکه تابع چه پارامترهایی رو می‌پذیره، می تونید از ساختارشکنی ES2015/ES6 یا همون destructuring استفاده کنید. این کار چند تا مزیت داره:

۱.	وقتی کسی به ساختار تابع نگاه می‌کنه، به سرعت متوجه میشه که از چه پارامترهایی استفاده شده.

۲.	میشه ازش برای شبیه سازی پارامترهای نامگذاری شده استفاده کرد.

** پارامتر نامگذاری شده در جاوااسکریپت به پارامتری گفته میشه که وقتی در یک تابع از اون استفاده میشه، به جای اینکه فقط مقدار اون به تابع پاس داده بشه، اسم و مقدارش همزمان به تابع پاس داده میشه. یه مثال ازش:

```javascript
function calculateArea({ width, height }) {
  return width * height;
}

calculateArea({ width: 3, height: 4 });
```

** تو این مثال وقتی میخوایم تابع رو فراخوانی کنیم، پارامترهای اون هم اسم و هم مقدار دارن که بهشون پارامترهای نامگذاری شده گفته میشه. اما تابع‌هایی که پارامترهای نامگذاری نشده دارن، موقع فراخوانی فقط مقدار پارامترهارو می‌پذیرن (مثال پایین). یکی از مزایای پارامترهای نامگذاری شده اینه که ترتیب پارامترها در فراخوانی تابع مهم نیست. پارمترهای نامگذاری نشده:

```javascript
function calculateArea(width, height) {
  return width * height
}

calculateArea(10 , 25);
```


۳.	عمل destructuring مقادیر اولیه (primitive values: number, string, boolean) رو اصطلاحا clone میکنه؛ به این معنی که یه متغیر جدید از روی اون‌ها ساخته میشه که عملیات روی این متغیر جدید، تأثیری روی متغیر اصلی نخواهد داشت و این باعث کاهش اثرات جانبی این تغییرات میشه. دقت داشته باشید که این اتفاق برای آبجکت‌ها و آرایه‌ها نمیفته!

۴.	لینترها می‌تونن پارامترهای استفاده نشده رو تشخیص بدن که بدون destructuring این امر ممکن نخواهد بود.

** لینتر (Linter) ها ابزارهایی هستن که به دولوپرها کمک می‌کنن کد تمیزتر و منظم‌تری بنویسن و معمولا به صورت extension برای IDE یا package در جاوااسکریپت در دسترس هستن.

**بد:**

```javascript
function createMenu(title, body, buttonText, cancellable) {
  // ...
}

createMenu("Foo", "Bar", "Baz", true);

```

**خوب:**

```javascript
function createMenu({ title, body, buttonText, cancellable }) {
  // ...
}

createMenu({
  title: "Foo",
  body: "Bar",
  buttonText: "Baz",
  cancellable: true
});
```

**[⬆ بازگشت](#فهرست-محتوا)**

### هر تابع فقط باید یک کار انجام بده

این مورد از مهم‌ترین اصول مهندسی نرم افزاره، چرا که نوشتن توابعی که بیش از یک کار رو انجام میدن و همچنین تست کردن و توضیح دادن این توابع سخت‌تره. وقتی تابعی محدود به یه عملیات خاص میشه، ریفکتور کردن اون ساده‌تر میشه و خوانایی کد افزایش پیدا میکنه. اگر از کل این راهنما، فقط همین یک مورد رو در کدنویسی خودتون رعایت کنید، از بسیاری از برنامه‌نویس‌ها جلو خواهید بود!

**بد:**

```javascript
function emailClients(clients) {
  clients.forEach(client => {
    const clientRecord = database.lookup(client);
    if (clientRecord.isActive()) {
      email(client);
    }
  });
}
```

**خوب:**

```javascript
function emailActiveClients(clients) {
  clients.filter(isActiveClient).forEach(email);
}

function isActiveClient(client) {
  const clientRecord = database.lookup(client);
  return clientRecord.isActive();
}
```

**[⬆ بازگشت](#فهرست-محتوا)**

### اسم تابع باید به کاری که اون تابع انجام میده اشاره کنه

**بد:**

```javascript
function addToDate(date, month) {
  // ...
}

const date = new Date();

// It's hard to tell from the function name what is added
addToDate(date, 1);
```

**خوب:**

```javascript
function addMonthToDate(month, date) {
  // ...
}

const date = new Date();
addMonthToDate(1, date);
```

**[⬆ بازگشت](#فهرست-محتوا)**

### هر تابع فقط باید یک برداشت (abstraction) روی ورودی خودش انجام بده

وقتی یک تابع برداشت‌های زیادی از یک ورودی میکنه، معمولا به این معنیه که کارهای زیادی رو داره روی اون ورودی انجام میده. تقسیم این کارها و برداشت‌ها به تابع‌های مختلف، می‌تونه کد رو خواناتر و همچنین امکان استفاده مجدد از اون تابع‌هارو رو فراهم می‌کنه.

** منظور از برداشت/انتزاع (abstraction) تابع در جاوااسکریپت چیه؟ وقتی ما تابعی رو پیاده‌سازی می‌کنیم، یک عملیاتی رو پشت این تابع پنهان می‌کنیم و از اون به بعد برای دسترسی به اون عملیات، فقط اون تابع رو فراخوانی می‌کنیم و دیگه کاری نداریم که اون تابع داره داخل خودش چیکار میکنه، به این مفهوم برداشت (abstraction) گفته میشه.

**بد:**

```javascript
function parseBetterJSAlternative(code) {
  const REGEXES = [
    // ...
  ];

  const statements = code.split(" ");
  const tokens = [];
  REGEXES.forEach(REGEX => {
    statements.forEach(statement => {
      // ...
    });
  });

  const ast = [];
  tokens.forEach(token => {
    // lex...
  });

  ast.forEach(node => {
    // parse...
  });
}
```

**خوب:**

```javascript
function parseBetterJSAlternative(code) {
  const tokens = tokenize(code);
  const syntaxTree = parse(tokens);
  syntaxTree.forEach(node => {
    // parse...
  });
}

function tokenize(code) {
  const REGEXES = [
    // ...
  ];

  const statements = code.split(" ");
  const tokens = [];
  REGEXES.forEach(REGEX => {
    statements.forEach(statement => {
      tokens.push(/* ... */);
    });
  });

  return tokens;
}

function parse(tokens) {
  const syntaxTree = [];
  tokens.forEach(token => {
    syntaxTree.push(/* ... */);
  });

  return syntaxTree;
}
```

**[⬆ بازگشت](#فهرست-محتوا)**

### کدهای تکراری رو حذف کنید

همه تلاش خودتون رو بکنید تا کد تکراری نداشته باشید. در غیر این صورت اگه نیاز به تغییر منطق (logic) داشته باشین، مجبورید بیش از یک قسمت از کد رو تغییر بدین.

تصور کنید اگر یک رستوران دارید و نیازد ارید موجودی انبار رو دائم بروز کنید: همه گوجه‌فرنگی‌ها، پیاز، سیر، ادویه‌ها و غیره. اگر برای این کار چند فهرست داشته باشید، با هر بشقاب غذا مجبورید همه اون فهرست‌هارو بروز کنید. اما وقتی فقط یک فهرست وجود داره، بروزرسانی همون یک لیست کافی خواهد بود.

دلیل داشتن کد تکراری اغلب اینه که دو یا چند چیز داریم که تفاوت‌های جزئی دارن ولی اشتراک‌های زیادی دارن. این تفاوت‌ها باعث میشه دو یا چند تابع مجزا بنویسیم که بتونه این تفاوت‌هارو مدیریت کنه در حالی که بیشتر کاری که انجام میدن مشابه و عین هم هست. حذف کد تکراری به این معنیه که با ایجاد یک برداشت (abstraction) بتونیم این تفاوت‌هارو فقط با یک تابع/ماژول/کلاس مدیریت کنیم.

خیلی مهمه که بتونیم برداشت‌ها (abstractions) رو به درستی درک و اجرا کنیم. به همین دلیل باید اصول SOLID که در بخش کلاس‌ها مطرح شده رو رعایت کرد و اجرای نادرست abstraction میتونه حتی از کد تکراری هم بدتر باشه. پس اگر می‌تونید یک abstraction ایجاد کنید، حتما این کارو بکنید، در غیر این صورت خودتون رو اسیر کدهای تکراری خواهید کرد و هر بار به جای تغییر یک بخش از کد، باید چند قسمت از کد رو تغییر بدید!


**بد:**

```javascript
function showDeveloperList(developers) {
  developers.forEach(developer => {
    const expectedSalary = developer.calculateExpectedSalary();
    const experience = developer.getExperience();
    const githubLink = developer.getGithubLink();
    const data = {
      expectedSalary,
      experience,
      githubLink
    };

    render(data);
  });
}

function showManagerList(managers) {
  managers.forEach(manager => {
    const expectedSalary = manager.calculateExpectedSalary();
    const experience = manager.getExperience();
    const portfolio = manager.getMBAProjects();
    const data = {
      expectedSalary,
      experience,
      portfolio
    };

    render(data);
  });
}
```

**خوب:**

```javascript
function showEmployeeList(employees) {
  employees.forEach(employee => {
    const expectedSalary = employee.calculateExpectedSalary();
    const experience = employee.getExperience();

    const data = {
      expectedSalary,
      experience
    };

    switch (employee.type) {
      case "manager":
        data.portfolio = employee.getMBAProjects();
        break;
      case "developer":
        data.githubLink = employee.getGithubLink();
        break;
    }

    render(data);
  });
}
```

**[⬆ بازگشت](#فهرست-محتوا)**

### آبجکت‌های پیشفرض رو با استفاده از Object.assign مقداردهی کنید

**بد:**

```javascript
const menuConfig = {
  title: null,
  body: "Bar",
  buttonText: null,
  cancellable: true
};

function createMenu(config) {
  config.title = config.title || "Foo";
  config.body = config.body || "Bar";
  config.buttonText = config.buttonText || "Baz";
  config.cancellable =
    config.cancellable !== undefined ? config.cancellable : true;
}

createMenu(menuConfig);
```

**خوب:**

```javascript
const menuConfig = {
  title: "Order",
  // User did not include 'body' key
  buttonText: "Send",
  cancellable: true
};

function createMenu(config) {
  let finalConfig = Object.assign(
    {
      title: "Foo",
      body: "Bar",
      buttonText: "Baz",
      cancellable: true
    },
    config
  );
  return finalConfig
  // config now equals: {title: "Order", body: "Bar", buttonText: "Send", cancellable: true}
  // ...
}

createMenu(menuConfig);
```

**[⬆ بازگشت](#فهرست-محتوا)**

### از flag به عنوان پارامتر تابع استفاده نکنید

وجود flag در تابع به این معنیه که تابع داره بیش از یک کار رو انجام میده. اگر تابعی داره دو کار متفاوت رو بر اساس مقدار یک boolean انجام میده، بهتره که اون تابع رو به دو تابع مجزا تقسیم کنید.

** در جاوااسکریپت flag به یک مقدار boolean گفته میشه که معمولا از مقدار اون برای تصمیم‌گیری در مورد مسیر ادامه محاسبات استفاده میشه.

**بد:**

```javascript
function createFile(name, temp) {
  if (temp) {
    fs.create(`./temp/${name}`);
  } else {
    fs.create(name);
  }
}
```

**خوب:**

```javascript
function createFile(name) {
  fs.create(name);
}

function createTempFile(name) {
  createFile(`./temp/${name}`);
}
```

**[⬆ بازگشت](#فهرست-محتوا)**

### از اثرات جانبی پرهیز کنید (بخش اول)

یک تابع اگه کاری غیر از دریافت یک مقدار و برگردوندن مقدار یا مقادیر دیگه انجام بده، داره یک اثر جانبی ایجاد می‌کنه. این اثر جانبی می‌تونه نوشتن در یک فایل، ایجاد تغییر در متغیرهای global یا انتقال تصادفی پول شما به حساب یه غریبه باشه.

حالا ممکنه شما در مواردی واقعا نیاز به اثر جانبی داشته باشید. مثل نمونه‌اش توی مثال قبلی، ممکنه نیاز داشته باشید که در یک فایل بنویسید. کاری که باید انجام بدید اینه که این عملکرد رو متمرکز به یک تابع بکنید. نه که چندین تابع و کلاس داشته باشید که همشون قابلیت نوشتن در فایل‌هارو دارن. فقط و فقط یک سرویس داشته باشید این کارو انجام بده.

نکته اصلی اینه که از تله‌های رایج مثل اشتراک‌گذاری وضعیت بین آبجکت‌ها بدون هیچ ساختاری، استفاده از انواع داده‌های قابل تغییر (mutable data types) که می‌تونن توسط هر چیزی تغییر کنن، و متمرکز نکردن مکان‌هایی که اثرات جانبی شما رخ می‌دن، اجتناب کنید. اگر بتونید این کارو انجام بدید، از اکثر برنامه‌نویس‌ها راضی‌تر خواهید بود.

**بد:**

```javascript
// Global variable referenced by following function.
// If we had another function that used this name, now it'd be an array and it could break it.
let name = "Ryan McDermott";

function splitIntoFirstAndLastName() {
  name = name.split(" ");
}

splitIntoFirstAndLastName();

console.log(name); // ['Ryan', 'McDermott'];
```

**خوب:**

```javascript
function splitIntoFirstAndLastName(name) {
  return name.split(" ");
}

const name = "Ryan McDermott";
const newName = splitIntoFirstAndLastName(name);

console.log(name); // 'Ryan McDermott';
console.log(newName); // ['Ryan', 'McDermott'];
```

**[⬆ بازگشت](#فهرست-محتوا)**

### از اثرات جانبی پرهیز کنید (بخش دوم)

در جاوااسکریپت، برخی از مقادیر غیرقابل تغییر (immutable) و برخی قابل تغییر (mutable) هستن. آبجکت‌ها و آرایه‌ها دو نوع مقدار قابل تغییر هستن، بنابراین خیلی مهمه که وقتی به عنوان پارامتر به یک تابع پاس داده میشن، اونهارو با دقت مدیریت کنید. یک تابع جاوااسکریپت می‌تونه ویژگی‌های یک آبجکت یا محتویات یک آرایه رو تغییر بده که این موضوع به راحتی می‌تونه باعث ایجاد اشکال در بخش‌های دیگه کد بشه.

فرض کنید تابعی داریم که یک آرایه رو که نشون دهنده سبد خرید هست رو به عنوان ورودی می‌پذیره. اگر تابع تغییری در آرایه سبد خرید ایجاد کنه – مثلا یک کالا رو به این سبد اضافه کنه - هر تابع دیگه‌ای که از همون آرایه سبد خرید استفاده می‌کنه هم تحت تأثیر این اضافه شدن قرار می‌گیره. ممکنه این موضوع اتفاق خوبی باشه، ولی با این حال می‌تونه اثرات بدی هم داشته باشه. بیایید یک سناریوی بد رو تصور کنیم:

کاربر روی دکمه "خرید" کلیک می‌کنه و تابع خرید رو فراخوانی می‌کنه که منجر به ارسال یک درخواست شبکه میشه و آرایه سبد خرید رو به سرور ارسال می‌کنه. به دلیل اشکال در اتصال شبکه، عملکرد خرید باید درخواست رو دوباره ارسال کنه. حالا اگه در این مدت کاربر به‌طور تصادفی دکمه «افزودن به سبد خرید» رو روی کالایی که قصد خریدش رو نداره، قبل از شروع درخواست بعدی شبکه کلیک کنه، چه اتفاقی میفته؟ تابع خرید، کالایی که تصادفاً اضافه شده رو ارسال می‌کنه چون که آرایه سبد خرید اصلاح شده.

یک راه حل عالی برای جلوگیری از چنین اتفاقاتی اینه که تابع addItemToCart همیشه یک نسخه برگردان (clone) از سبد خرید ایجاد کنه، اون رو ویرایش کنه و همون نسخه کلون رو برگردونه. به این شکل میشه تضمین کرد که عملکردهایی که هنوز از سبد خرید قدیمی استفاده می‌کنن، تحت تأثیر تغییرات جدید یا اتفاقی قرار نخواهند گرفت.

دو نکته در مورد این رویکرد رو باید در نظر گرفت:

۱.	ممکنه مواردی وجود داشته باشه که واقعاً بخواید آبجکت ورودی رو تغییر بدید. وقتی این رویکرد برنامه‌نویسی رو در پیش می‌گیرید، متوجه خواهید شد که این موارد بسیار نادر هستن و بیشتر چیزها رو میشه بازسازی (refactor) کرد تا هیچ اثر جانبی‌ای نداشته باشه!

۲.	ایجاد برگردان (clone) آبجکت‌های بزرگ می‌تونه از نظر عملکرد هزینه داشته باشه. خوشبختانه، این مسئله در عمل مشکل بزرگی نیست، چرا که کتابخانه‌های بزرگی وجود دارند که به کمک این نوع رویکرد برنامه‌نویسی میان تا سریع اجرا بشه و اونقدر حافظه رو درگیر نمی‌کنه که شما رو مجبور به clone دستی آبجکت‌ها و آرایه‌ها بکنه.

**بد:**

```javascript
const addItemToCart = (cart, item) => {
  cart.push({ item, date: Date.now() });
};
```

**خوب:**

```javascript
const addItemToCart = (cart, item) => {
  return [...cart, { item, date: Date.now() }];
};
```

**[⬆ بازگشت](#فهرست-محتوا)**

### روی تابع‌های سراسری (global) تغییری ایجاد نکنید

دستکاری کردن مقادیر سراسری (global) در جاوااسکریپت حرکت درستی نیست، چون ممکنه با کتابخانه‌های دیگه تداخل ایجاد کنه و کاربری که از API شما استفاده میکنه، تا وقتی به خطایی برنخوره، متوجه اون نخواهد شد. بیایید یه مثال رو بررسی کنیم: میخوایم متد آرایه اصلی جاوااسکریپت را گسترش بدیم (extend کنیم) تا یک متد diff داشته باشه که بتونه اختلاف بین دو آرایه رو نشون بده. شما می‌تونید تابع جدید رو در Array.prototype بنویسید، اما ممکنه با کتابخانه دیگه‌ای که سعی در انجام همین کار دارد، تداخل کنه. ممکنه اون کتابخانه از متد diff برای پیدا کردن تفاوت بین اولین و آخرین عناصر یک آرایه استفاده کنه. به همین دلیله که بهتره از کلاس‌های ES2015/ES6 استفاده کنید و به سادگی آرایه سراسری را گسترش بدید (extend کنید).

**بد:**

```javascript
Array.prototype.diff = function diff(comparisonArray) {
  const hash = new Set(comparisonArray);
  return this.filter(elem => !hash.has(elem));
};
```

**خوب:**

```javascript
class SuperArray extends Array {
  diff(comparisonArray) {
    const hash = new Set(comparisonArray);
    return this.filter(elem => !hash.has(elem));
  }
}
```

**[⬆ بازگشت](#فهرست-محتوا)**

### برنامه‌نویسی تابعی (functional programming) رو به برنامه‌نویسی دستوری (imperative programming) ترجیح بدید

جاوااسکریپت مثل Haskell یک زبان برنامه‌نویسی تابعی نیست، اما چاشنی تابعی داره. زبان‌های برنامه‌نویسی تابعی می‌تونن برای تست، تمیزتر و آسون‌تر باشن. تا جایی که می‌تونید از این سبک برنامه‌نویسی استفاده کنید.
** در برنامه‌نویسی دستوری، ما لیستی از دستورات رو به کامپیوتر میدیم و با هر دستور، وضعیت (state) اون کد آپدیت میشه. در هر دستور، جزئیات و چگونگی تغییر وضعیت رو می‌نویسیم. زبان‌هایی مثل C، C++ و Java دستوری هستن. در کنارش برنامه‌نویسی تابعی رو داریم که در اون تعیین می‌کنیم برنامه چه چیزی رو باید تغییر بده و با چگونگی این تغییرات کاری نداریم. در این الگو، تابع‌ها ورودی می‌گیرن و خروجی میدن و اثرات جانبی (side effects) ایجاد نمی‌کنن. زبان‌هایی مثل Haskell، Lisp و Scheme از این الگو استفاده می‌کنن.

**بد:**

```javascript
const programmerOutput = [
  {
    name: "Uncle Bobby",
    linesOfCode: 500
  },
  {
    name: "Suzie Q",
    linesOfCode: 1500
  },
  {
    name: "Jimmy Gosling",
    linesOfCode: 150
  },
  {
    name: "Gracie Hopper",
    linesOfCode: 1000
  }
];

let totalOutput = 0;

for (let i = 0; i < programmerOutput.length; i++) {
  totalOutput += programmerOutput[i].linesOfCode;
}
```

**خوب:**

```javascript
const programmerOutput = [
  {
    name: "Uncle Bobby",
    linesOfCode: 500
  },
  {
    name: "Suzie Q",
    linesOfCode: 1500
  },
  {
    name: "Jimmy Gosling",
    linesOfCode: 150
  },
  {
    name: "Gracie Hopper",
    linesOfCode: 1000
  }
];

const totalOutput = programmerOutput.reduce(
  (totalLines, output) => totalLines + output.linesOfCode,
  0
);
```

**[⬆ بازگشت](#فهرست-محتوا)**

### عبارات شرطی رو کپسوله کنید

** کپسوله کردن (encapsulation) در جاوااسکریپت به این معنیه که جزئیات و پیچیدگی‌های پیاده‌سازی رو از دید بیرونی مخفی کنیم و اطلاعات که نیاز هستن رو در دسترس قرار بدیم. که نهایتا به پیاده‌سازی abstraction ها در جاوااسکریپت کمک می‌کنه.

**بد:**

```javascript
if (fsm.state === "fetching" && isEmpty(listNode)) {
  // ...
}
```

**خوب:**

```javascript
function shouldShowSpinner(fsm, listNode) {
  return fsm.state === "fetching" && isEmpty(listNode);
}

if (shouldShowSpinner(fsmInstance, listNodeInstance)) {
  // ...
}
```

**[⬆ بازگشت](#فهرست-محتوا)**

### از شرط‌های منفی پرهیز کنید

**بد:**

```javascript
function isDOMNodeNotPresent(node) {
  // ...
}

if (!isDOMNodeNotPresent(node)) {
  // ...
}
```

**خوب:**

```javascript
function isDOMNodePresent(node) {
  // ...
}

if (isDOMNodePresent(node)) {
  // ...
}
```

**[⬆ بازگشت](#فهرست-محتوا)**

### از شرط‌ها پرهیز کنید

شاید کار غیر ممکنی به نظر برسه و اولین سوال ما این باشه که اصلا چطور میشه بدون استفاده از شرط‌ها، برنامه‌نویسی کرد؟ پاسخ این سوال اینه که شما می‌تونید از خاصیت چندشکلی (polymorphism) در جاوااسکریپت برای رسیدن به همون عملکرد در بسیاری از موارد استفاده کنید. سؤال دوم معمولاً اینه: خب خوبه، اما چرا من باید این کارو بکنم؟ پاسخ این سوال یکی از مفاهیم کد تمیزه که قبلا صحبتش رو کردیم: یک تابع فقط باید یک کار رو انجام بده. وقتی کلاس‌ها و تابع‌هایی دارید که دستور if دارن، تابع شما داره بیش از یک کار رو انجام میده. یادتون باشه، با هر تابع فقط یک کار رو انجام بدید.

**بد:**

```javascript
class Airplane {
  // ...
  getCruisingAltitude() {
    switch (this.type) {
      case "777":
        return this.getMaxAltitude() - this.getPassengerCount();
      case "Air Force One":
        return this.getMaxAltitude();
      case "Cessna":
        return this.getMaxAltitude() - this.getFuelExpenditure();
    }
  }
}
```

**خوب:**

```javascript
class Airplane {
  // ...
}

class Boeing777 extends Airplane {
  // ...
  getCruisingAltitude() {
    return this.getMaxAltitude() - this.getPassengerCount();
  }
}

class AirForceOne extends Airplane {
  // ...
  getCruisingAltitude() {
    return this.getMaxAltitude();
  }
}

class Cessna extends Airplane {
  // ...
  getCruisingAltitude() {
    return this.getMaxAltitude() - this.getFuelExpenditure();
  }
}
```

**[⬆ بازگشت](#فهرست-محتوا)**

### از بررسی نوع متغیر یا پارامتر پرهیز کنید (بخش اول)

جاوااسکریپت یک زبان بدون تایپ هست، به این معنی که تابع‌های شما می‌تونن هر نوع پارامتر رو بگیرن. گاهی اوقات صرفا چون توانایی‌اش رو دارید، چک کردن تایپ در تابع‌ها وسوسه انگیز میشه. راه‌های زیادی برای اجتناب از انجام این کار وجود داره که اولین چیزی که باید در نظر گرفت API های ثابت (consistent APIs) هست.
** API های ثابت (consistent APIs) در جاوااسکریپت به مجموعه‌ای از تابع‌ها یا روش‌هایی اشاره می‌کنه که ساختار، رفتار و مستندات یکسان و قابل پیش‌بینی دارن. یعنی اگه نحوه استفاده از یک تابع یا متد از اون API رو می‌دونید، باید بتونید از همه تابع‌ها یا روش‌های دیگه اون API به همون روش استفاده کنید. این موضوع استفاده، درک و نگهداری از API رو به خصوص برای برنامه‌نویس‌های مبتدی آسن‌تر میکنه.

**بد:**

```javascript
function travelToTexas(vehicle) {
  if (vehicle instanceof Bicycle) {
    vehicle.pedal(this.currentLocation, new Location("texas"));
  } else if (vehicle instanceof Car) {
    vehicle.drive(this.currentLocation, new Location("texas"));
  }
}
```

**خوب:**

```javascript
function travelToTexas(vehicle) {
  vehicle.move(this.currentLocation, new Location("texas"));
}
```

**[⬆ بازگشت](#فهرست-محتوا)**

### از بررسی نوع متغیر یا پارامتر پرهیز کنید (بخش دوم)

اگه با مقادیر ابتدایی (primitive values) ساده مانند رشته‌ها و اعداد صحیح کار می‌کنید و نمی‌توانید از خاصیت چند شکلی (polymorphism) استفاده کنید، اما هنوز نیاز به بررسی تایپ دارید، باید از TypeScript استفاده کنید. تایپ اسکریپ یه جایگزین عالی برای جاوااسکریپت معمولیه، چرا که علاوه بر جاوااسکریپت، قابلیت تعریف یک تایپ ثابت رو برای شما فراهم می‌کنه. مشکل چک کردن دستی تایپ در جاوااسکریپت معمولی اینه که نیاز به کد اضافه داره و خوانایی رو کاهش میده. با جاوااسکریپت کدهای تمیز و قابل تست بنویسید و کدهارو به خوبی مرور کنید. در صورتی که به چیزهای بیشتری نیاز داشتید، همه اون کارهارو با TypeScript انجام بدید که جایگزین بسیار عالی و مناسب برای جاوااسکریپت هست.

**بد:**

```javascript
function combine(val1, val2) {
  if (
    (typeof val1 === "number" && typeof val2 === "number") ||
    (typeof val1 === "string" && typeof val2 === "string")
  ) {
    return val1 + val2;
  }

  throw new Error("Must be of type String or Number");
}
```

**خوب:**

```javascript
function combine(val1, val2) {
  return val1 + val2;
}
```

**[⬆ بازگشت](#فهرست-محتوا)**

### بیش از حد بهینه سازی نکنید

مرورگرهای مدرن در زمان اجرای اپلیکیشن، بهینه‌سازیهای زیادی رو در پشت پرده انجام میدن. بیشتر مواقع، با بهینه‌سازی بیش از حد، فقط وقتتون رو تلف می‌کنید! منابع خوبی وجود دارن که به شما می‌کنن نقاط ضعف بهینه‌سازی رو پیدا کنید، سعی کنید فقط این نقاط ضعف رو در صورت امکان برطرف کنید.

**بد:**

```javascript
// On old browsers, each iteration with uncached `list.length` would be costly
// because of `list.length` recomputation. In modern browsers, this is optimized.
for (let i = 0, len = list.length; i < len; i++) {
  // ...
}
```

**خوب:**

```javascript
for (let i = 0; i < list.length; i++) {
  // ...
}
```

**[⬆ بازگشت](#فهرست-محتوا)**

### کدهای بلا استفاده رو پاک کنید

کد مرده (dead code) به اندازه کد تکراری بده. اگر کدی فراخوانی نمیشه، دلیلی نداره نگهش دارید، از شرش خلاص بشید. در صورتی که بهش نیاز پیدا کنید در تاریخچه ورژن کنترل (version control history) بهش دسترسی خواهید داشت.

**بد:**

```javascript
function oldRequestModule(url) {
  // ...
}

function newRequestModule(url) {
  // ...
}

const req = newRequestModule;
inventoryTracker("apples", req, "www.inventory-awesome.io");
```

**خوب:**

```javascript
function newRequestModule(url) {
  // ...
}

const req = newRequestModule;
inventoryTracker("apples", req, "www.inventory-awesome.io");
```

**[⬆ بازگشت](#فهرست-محتوا)**

## **آبجکت‌ها و ساختارهای داده**

### از getter ها و setter ها استفاده کنید

استفاده از getter ها و setter ها برای دسترسی به داده‌های روی آبجکت‌ها می‌تونه بهتر از جستجوی ساده یک ویژگی روی یه آبجکت باشه. ممکنه بپرسید "چرا؟" که چند تا دلیل رو در ادامه براش مطرح میشه:
•	وقتی کاری بیشتر از دریافت یک ویژگی آبجکت دارید، لازم نیست همه کدتون رو بررسی کنید و هر جا که اون ویژگی استفاده شده رو تغییر بدید.
•	استفاده از set افزودن اعتبارسنجی رو ساده می‌کنه.
•	وضعیت داخلی آبجکت رو کپسوله (encapsulate) می‌کنه. ** به این معنیه که دسترسی به ویژگی‌های آبجکت و تغییر اون‌ها فقط از طریق get و set انجام میشه و این موضوع به نگهداری و گسترش کد کمک می‌کنه.
•	لاگ گرفتن و مدیریت خطاها موقع get و set آسون‌تره.
•	می‌تونید ویژگی‌های آبجکت‌تون رو به صورت تنبل (lazy loading) بارگذاری کنید، مثلا وقتی که دارید اطلاعاتش رو از یک سرور دریافت کنید.

**بد:**

```javascript
function makeBankAccount() {
  // ...

  return {
    balance: 0
    // ...
  };
}

const account = makeBankAccount();
account.balance = 100;
```

**خوب:**

```javascript
function makeBankAccount() {
  // this one is private
  let balance = 0;

  // a "getter", made public via the returned object below
  function getBalance() {
    return balance;
  }

  // a "setter", made public via the returned object below
  function setBalance(amount) {
    // ... validate before updating the balance
    balance = amount;
  }

  return {
    // ...
    getBalance,
    setBalance
  };
}

const account = makeBankAccount();
account.setBalance(100);
```

**[⬆ بازگشت](#فهرست-محتوا)**

### آبجکت‌های با ویژگی‌های خصوصی بسازید

با استفاده از closure ها میشه بهش دست پیدا کرد.
** خصوصی کردن ویژگی در یک آبجکت به این معنیه که نشه از بیرون از اون آبجکت اون ویژگی رو حذف کرد یا تغییر داد. به این ترتیب میشه وضعیت (state) کلی آبجکت رو حفظ کرد که نگهداری کد رو آسون‌تر و قابل کنترل میکنه.

**بد:**

```javascript
const Employee = function(name) {
  this.name = name;
};

Employee.prototype.getName = function getName() {
  return this.name;
};

const employee = new Employee("John Doe");
console.log(`Employee name: ${employee.getName()}`); // Employee name: John Doe
delete employee.name;
console.log(`Employee name: ${employee.getName()}`); // Employee name: undefined
```

**خوب:**

```javascript
function makeEmployee(name) {
  return {
    getName() {
      return name;
    }
  };
}

const employee = makeEmployee("John Doe");
console.log(`Employee name: ${employee.getName()}`); // Employee name: John Doe
delete employee.name;
console.log(`Employee name: ${employee.getName()}`); // Employee name: John Doe
```

**[⬆ بازگشت](#فهرست-محتوا)**

## **کلاس‌ها**

### کلاس‌های ES2015/ES6 رو به تابع‌های ساده ES5 ترجیح بدید

از سختی‌های کلاس‌های کلاسیک ES5 اینه که به سختی میشه وراثت (inheritance)، ساختار (construction) و متد (method) های خوانا باهاشون پیاده‌سازی کرد. اگه به وراثت نیاز دارید (توجه داشته باشید که ممکنه نیاز نداشته نباشید)، کلاس‌های ES2015/ES6 را ترجیح بدید. با این حال، تابع‌های کوچک رو به کلاس‌ها ترجیح بدید مگر اینکه واقعا به آبجکت‌های بزرگتر و پیچیده‌تر نیاز پیدا کنید.

**بد:**

```javascript
const Animal = function(age) {
  if (!(this instanceof Animal)) {
    throw new Error("Instantiate Animal with `new`");
  }

  this.age = age;
};

Animal.prototype.move = function move() {};

const Mammal = function(age, furColor) {
  if (!(this instanceof Mammal)) {
    throw new Error("Instantiate Mammal with `new`");
  }

  Animal.call(this, age);
  this.furColor = furColor;
};

Mammal.prototype = Object.create(Animal.prototype);
Mammal.prototype.constructor = Mammal;
Mammal.prototype.liveBirth = function liveBirth() {};

const Human = function(age, furColor, languageSpoken) {
  if (!(this instanceof Human)) {
    throw new Error("Instantiate Human with `new`");
  }

  Mammal.call(this, age, furColor);
  this.languageSpoken = languageSpoken;
};

Human.prototype = Object.create(Mammal.prototype);
Human.prototype.constructor = Human;
Human.prototype.speak = function speak() {};
```

**خوب:**

```javascript
class Animal {
  constructor(age) {
    this.age = age;
  }

  move() {
    /* ... */
  }
}

class Mammal extends Animal {
  constructor(age, furColor) {
    super(age);
    this.furColor = furColor;
  }

  liveBirth() {
    /* ... */
  }
}

class Human extends Mammal {
  constructor(age, furColor, languageSpoken) {
    super(age, furColor);
    this.languageSpoken = languageSpoken;
  }

  speak() {
    /* ... */
  }
}
```

**[⬆ بازگشت](#فهرست-محتوا)**

### از زنجیره‌ای کردن متدها استفاده کنید (method chaining)

این یه الگوی خیلی کاربردی در جاوااسکریپت هست که در کتابخانه‌های معروفی مثل jQuery و Lodash ازش استفاده شده. کد شما رو معنادارتر و همینطور کوتاه‌تر میکنه. بعد ازا ستفاده از method chaining خواهید دید که چقدر کدتون تمیزتر شده. برای اینکار کافیه تا در تابع‌هایی که برای کلاس‌ها تعریف می‌کنید، در انتهای هر تابع this رو برگردونید و بعد از اون می‌تونید متدهای بعدی کلاس رو هم روی همون مقدار پیاده سازی کنید.
** در واقع این روش خروجی هر متد رو به عنوان ورودی متد بعدی قرار میده، که نهایتا بهمون اجازه میده تا همه متدهای یک کلاس رو در یک خط کد و پشت سر هم فراخوانی کنیم.

**بد:**

```javascript
class Car {
  constructor(make, model, color) {
    this.make = make;
    this.model = model;
    this.color = color;
  }

  setMake(make) {
    this.make = make;
  }

  setModel(model) {
    this.model = model;
  }

  setColor(color) {
    this.color = color;
  }

  save() {
    console.log(this.make, this.model, this.color);
  }
}

const car = new Car("Ford", "F-150", "red");
car.setColor("pink");
car.save();
```

**خوب:**

```javascript
class Car {
  constructor(make, model, color) {
    this.make = make;
    this.model = model;
    this.color = color;
  }

  setMake(make) {
    this.make = make;
    // NOTE: Returning this for chaining
    return this;
  }

  setModel(model) {
    this.model = model;
    // NOTE: Returning this for chaining
    return this;
  }

  setColor(color) {
    this.color = color;
    // NOTE: Returning this for chaining
    return this;
  }

  save() {
    console.log(this.make, this.model, this.color);
    // NOTE: Returning this for chaining
    return this;
  }
}

const car = new Car("Ford", "F-150", "red").setColor("pink").save();
```

**[⬆ بازگشت](#فهرست-محتوا)**

### ترکیب کلاس‌ها (composition) رو به وراثت (inheritance) ترجیح بدید

همونطور که در کتاب معروف [Design Patterns](https://en.wikipedia.org/wiki/Design_Patterns) هم بهش اشاره شده، هر جا که می‌تونید، به جای وراثت از کلاس‌های ترکیبی استفاده بکنید. دلایل زیادی  هم برای استفاده از وراثت و هم برای استفاده از ترکیب در جاوااسکریپت وجود داره. دلیل مطرح کردن این موضوع هم اینه که اگر ذهن شما به صورت غریزی برای حل مسائل به سمت وراثت میره، سعی کنید تا روشی که ذهنتون رسیده رو با ترکیب (composition) مدل کنید چرا که در بعضی موارد شدنی هست.

شاید بپرسید که دیگه وراثت چه کاربردی داره و چرا باید استفاده کنیم؟ در ادامه چند دلیل مطرح میشه که چرا باید از وراثت استفاده کنیم:

۱.	وراثت شما نشون دهنده یه رابطه is-a (= است) هست، نه یک رابطه has-a (= دارد).
به عنوان مثال دو عبارت Human is an animal و User has a user detail رو مقایسه کنید. جمله اول میگه که انسان یک حیوان "است" و جمله دوم میگه که کاربر یک اطلاعات کاربر "دارد".

۲.	کدهای کلاس‌های مبنا قابل استفاده هستند.
مثال Human در بند قبلی رو در نظر بگیرید، حالا می‌تونیم بگیم Humans can move like all animals به این معنی که انسان‌ها می‌تونن مثل همه حیوانات حرکت بکنن.

۳.	شما می‌تونید با ایجاد تغییر در یک کلاس مبنا، اون تغییرات رو در همه کلاس‌هایی که از این کلاس مبنا وراثت گرفتن رو اعمال کنید. به عنوان مثال اگر مصرف کالری حیوانات رو تغییر بدید، این تغییرات روی کلاس انسان هم اعمال خواهد شد.
** به زبان ساده، مفهوم ترکیب (composition) در جاوااسکریپت میگه که می‌تونیم آبجکتی از یک کلاس رو به عنوان یک ویژگی (property) برای کلاس دیگر استفاده کنیم که نتیجه اون ایجاد آبجکت‌های پیچیده‌تر (مرکب) هست. آبجکتی که به عنوان یه ویژگی ازش استفاده شده رو کماکان میشه به همه عملکردهاش دسترسی داشت و از متدهاش استفاده کرد.

**بد:**

```javascript
class Employee {
  constructor(name, email) {
    this.name = name;
    this.email = email;
  }

  // ...
}

// Bad because Employees "have" tax data. EmployeeTaxData is not a type of Employee
class EmployeeTaxData extends Employee {
  constructor(ssn, salary) {
    super();
    this.ssn = ssn;
    this.salary = salary;
  }

  // ...
}
```

**خوب:**

```javascript
class EmployeeTaxData {
  constructor(ssn, salary) {
    this.ssn = ssn;
    this.salary = salary;
  }

  // ...
}

class Employee {
  constructor(name, email) {
    this.name = name;
    this.email = email;
  }

  setTaxData(ssn, salary) {
    this.taxData = new EmployeeTaxData(ssn, salary);
  }
  // ...
}
```

**[⬆ بازگشت](#فهرست-محتوا)**

## **اصول SOLID**

** اصول SOLID مجموعه‌ای از 5 قانون هست که رعایت اون‌ها باعث میشه کدی که می‌نویسیم، خوانا و قابل درک باشه و همچنین نگهداری و توسعه‌اش راحت‌تر باشه.

### اصل مسئولیت واحد

همونطور که در Clean Code اشاره شده، "هرگز نباید بیش از یک دلیل برای تغییر کلاس وجود داشته باشه". این که یک کلاس با کارایی زیاد بنویسید وسوسه انگیزه، مثل زمانی که می‌خواید فقط یه چمدون به مسافرت ببرید. مشکلی که پیش میاد اینه که کلاس شما از نظر مفهومی، منسجم نخواهد بود و دلایل زیادی برای تغییر به خواهد داشت. به حداقل رساندن تعداد دفعات تغییر یک کلاس، اهمیت بسیار زیادی داره. چرا که اگر یک کلاس بیش از یک عملکرد داشته باشه و شما بخشی از اون رو تغییر بدید، درک اینکه چه اثری روی دیگر ماژول‌های کد شما میذاره، دشوار خواهد بود.

**بد:**

```javascript
class UserSettings {
  constructor(user) {
    this.user = user;
  }

  changeSettings(settings) {
    if (this.verifyCredentials()) {
      // ...
    }
  }

  verifyCredentials() {
    // ...
  }
}
```

**خوب:**

```javascript
class UserAuth {
  constructor(user) {
    this.user = user;
  }

  verifyCredentials() {
    // ...
  }
}

class UserSettings {
  constructor(user) {
    this.user = user;
    this.auth = new UserAuth(user);
  }

  changeSettings(settings) {
    if (this.auth.verifyCredentials()) {
      // ...
    }
  }
}
```

**[⬆ بازگشت](#فهرست-محتوا)**

### اصل باز/بسته

همونطور که Bertrand Meyer گفته، "موجودات نرم افزاری (کلاس‌ها، ماژول‌ها، تابع‌ها و غیره) باید برای توسعه باز باشن، اما برای اصلاح بسته شن." این به چه معناست؟ این اصل اساساً بیان میگه که باید به کاربران اجازه بدید بدون تغییر کد موجود، قابلیت‌های جدید رو اضافه کنن.

**بد:**

```javascript
class AjaxAdapter extends Adapter {
  constructor() {
    super();
    this.name = "ajaxAdapter";
  }
}

class NodeAdapter extends Adapter {
  constructor() {
    super();
    this.name = "nodeAdapter";
  }
}

class HttpRequester {
  constructor(adapter) {
    this.adapter = adapter;
  }

  fetch(url) {
    if (this.adapter.name === "ajaxAdapter") {
      return makeAjaxCall(url).then(response => {
        // transform response and return
      });
    } else if (this.adapter.name === "nodeAdapter") {
      return makeHttpCall(url).then(response => {
        // transform response and return
      });
    }
  }
}

function makeAjaxCall(url) {
  // request and return promise
}

function makeHttpCall(url) {
  // request and return promise
}
```

**خوب:**

```javascript
class AjaxAdapter extends Adapter {
  constructor() {
    super();
    this.name = "ajaxAdapter";
  }

  request(url) {
    // request and return promise
  }
}

class NodeAdapter extends Adapter {
  constructor() {
    super();
    this.name = "nodeAdapter";
  }

  request(url) {
    // request and return promise
  }
}

class HttpRequester {
  constructor(adapter) {
    this.adapter = adapter;
  }

  fetch(url) {
    return this.adapter.request(url).then(response => {
      // transform response and return
    });
  }
}
```

**[⬆ بازگشت](#فهرست-محتوا)**

### اصل جایگزینی لیسکوف

بر خلاف ظاهر سختش، در واقع اصل ساده‌ای هست که به این شکل تعریف میشه: "اگر S زیرنوع T باشه، آبجکت‌های نوع T ممکنه با آبجکت‌هایی از نوع S جایگزین شن (یعنی آبجکت‌های نوع S ممکنه جایگزین آبجکت‌های نوع T شن)، بدون اینکه هیچ کی از ویژگی‌های مطلوب برنامه تغییری بکنن (درستی، وظایف انجام شده و غیره)".

بهترین توضیح برای این موضوع اینه که اگر یک کلاس والد و یک کلاس فرزند دارید، کلاس پایه و کلاس فرزند میتونن به جای هم استفاده شن بدون اینکه نتایج نادرستی تولید کنن. ممکنه این مفهوم هنوز گیج کننده باشه، بنابراین بیاید نگاهی به مثال کلاسیک مربع-مستطیل بندازیم. از نظر ریاضی، مربع یک مستطیله، اما اگه اون رو با استفاده از رابطه is-a (= است) از طریق وراثت مدل کنید، به سرعت به مشکل میخورید.

**بد:**

```javascript
class Rectangle {
  constructor() {
    this.width = 0;
    this.height = 0;
  }

  setColor(color) {
    // ...
  }

  render(area) {
    // ...
  }

  setWidth(width) {
    this.width = width;
  }

  setHeight(height) {
    this.height = height;
  }

  getArea() {
    return this.width * this.height;
  }
}

class Square extends Rectangle {
  setWidth(width) {
    this.width = width;
    this.height = width;
  }

  setHeight(height) {
    this.width = height;
    this.height = height;
  }
}

function renderLargeRectangles(rectangles) {
  rectangles.forEach(rectangle => {
    rectangle.setWidth(4);
    rectangle.setHeight(5);
    const area = rectangle.getArea(); // BAD: Returns 25 for Square. Should be 20.
    rectangle.render(area);
  });
}

const rectangles = [new Rectangle(), new Rectangle(), new Square()];
renderLargeRectangles(rectangles);
```

**خوب:**

```javascript
class Shape {
  setColor(color) {
    // ...
  }

  render(area) {
    // ...
  }
}

class Rectangle extends Shape {
  constructor(width, height) {
    super();
    this.width = width;
    this.height = height;
  }

  getArea() {
    return this.width * this.height;
  }
}

class Square extends Shape {
  constructor(length) {
    super();
    this.length = length;
  }

  getArea() {
    return this.length * this.length;
  }
}

function renderLargeShapes(shapes) {
  shapes.forEach(shape => {
    const area = shape.getArea();
    shape.render(area);
  });
}

const shapes = [new Rectangle(4, 5), new Rectangle(4, 5), new Square(5)];
renderLargeShapes(shapes);
```

**[⬆ بازگشت](#فهرست-محتوا)**

### اصل جداسازی رابط

جاوااسکریپت رابط (interface) نداره، بنابراین این اصل به شدت سایر اصل‌ها اعمال نمیشه. با این حال، حتی با عدم وجود نوع (type) در جاوا اسکریپت، اهمیت خودش رو داره.

این اصل میگه "کاربرها نباید مجبور به وابستگی به رابط‌هایی بشن که استفاده‌ای ازشون نمیکنن". از اونجایی که در جاوااسکریپت نوع (type) نداریم، رابط‌ها به صورت قراردادهای ضمنی تعریف میشن.

یک مثال خوب برای این اصل در جاوااسکریپت، کلاس‌هایی هستن که به آبجکت‌های تنظیمات (settings objects) بزرگ نیاز دارن. اینکه نیاز نباشه کاربرها همه مقادیر رو تنظیم کنن، بسیار مفیده، چرا که در اکثر مواقع به همه اون تنظیمات نیازی نیست. اختیاری کردن این تنظیمات، از داشتن رابط‌های بزرگ جلوگیری میکنه.

**بد:**

```javascript
class DOMTraverser {
  constructor(settings) {
    this.settings = settings;
    this.setup();
  }

  setup() {
    this.rootNode = this.settings.rootNode;
    this.settings.animationModule.setup();
  }

  traverse() {
    // ...
  }
}

const $ = new DOMTraverser({
  rootNode: document.getElementsByTagName("body"),
  animationModule() {} // Most of the time, we won't need to animate when traversing.
  // ...
});
```

**خوب:**

```javascript
class DOMTraverser {
  constructor(settings) {
    this.settings = settings;
    this.options = settings.options;
    this.setup();
  }

  setup() {
    this.rootNode = this.settings.rootNode;
    this.setupOptions();
  }

  setupOptions() {
    if (this.options.animationModule) {
      // ...
    }
  }

  traverse() {
    // ...
  }
}

const $ = new DOMTraverser({
  rootNode: document.getElementsByTagName("body"),
  options: {
    animationModule() {}
  }
});
```

**[⬆ بازگشت](#فهرست-محتوا)**

### اصل وارونگی وابستگی

این اصل دو چیز اساسی رو بیان میکنه:

۱.	ماژول‌های سطح بالا نباید به ماژول‌های سطح پایین وابسته باشن. هر دو باید به برداشت‌ها (abstractions) بستگی داشته باشن.

۲.	برداشت‌ها (abstractions) نباید به جزئیات بستگی داشته باشن. جزئیات باید به برداشت‌ها بستگی داشته باشن.

درک این موضوع ممکنه در وهله اول سخت باشه، اما اگه با AngularJS کار کرده باشید، اجرای این اصل را در قالب Dependency Injection (DI) مشاهده کردید. در حالی که این مفاهیم یکسان نیستن، اصل وارونگی وابستگی، ماژول‌های سطح بالا رو از دونستن جزئیات ماژول‌های سطح پایین خودشون و راه اندازی اونها من میکنه. میشه این اصل رو از طریق DI پیاده سازی کرد. یکی از بزرگترین مزیت‌های این اصل اینه که وابستگی (coupling) بین ماژول‌ها را کاهش میده. کوپلینگ یک الگوی توسعه بسیار بده که باعث میشه کد شما به‌سختی تغییر داده (refactor) بشه.

همونطور که قبلاً گفته شد، جاوااسکریپت رابط (interface) نداره، بنابراین برداشت (abstraction) های وابسته به اونها، قراردادهای ضمنی هستن. یعنی متدها و ویژگی‌هایی که یک آبجکت/کلاس در معرض آبجکت/کلاس دیگر قرار میده. در مثال پایین، قرارداد ضمنی اینه که هر ماژول Request برای کلاس InventoryTracker یک متد requestItems خواهد داشت:

**بد:**

```javascript
class InventoryRequester {
  constructor() {
    this.REQ_METHODS = ["HTTP"];
  }

  requestItem(item) {
    // ...
  }
}

class InventoryTracker {
  constructor(items) {
    this.items = items;

    // BAD: We have created a dependency on a specific request implementation.
    // We should just have requestItems depend on a request method: `request`
    this.requester = new InventoryRequester();
  }

  requestItems() {
    this.items.forEach(item => {
      this.requester.requestItem(item);
    });
  }
}

const inventoryTracker = new InventoryTracker(["apples", "bananas"]);
inventoryTracker.requestItems();
```

**خوب:**

```javascript
class InventoryTracker {
  constructor(items, requester) {
    this.items = items;
    this.requester = requester;
  }

  requestItems() {
    this.items.forEach(item => {
      this.requester.requestItem(item);
    });
  }
}

class InventoryRequesterV1 {
  constructor() {
    this.REQ_METHODS = ["HTTP"];
  }

  requestItem(item) {
    // ...
  }
}

class InventoryRequesterV2 {
  constructor() {
    this.REQ_METHODS = ["WS"];
  }

  requestItem(item) {
    // ...
  }
}

// By constructing our dependencies externally and injecting them, we can easily
// substitute our request module for a fancy new one that uses WebSockets.
const inventoryTracker = new InventoryTracker(
  ["apples", "bananas"],
  new InventoryRequesterV2()
);
inventoryTracker.requestItems();
```

**[⬆ بازگشت](#فهرست-محتوا)**

## **تست**

تست نرم افزار مهم‌تر از انتشار اون هست. اگه هیچ تستی ندارید یا تستی که دارید به اندازه کافی مناسب نیست، هر بار که کد رو منتشر می‌کنید، مطمئن نخواهید بود که چیزی رو خراب کردید یا نه. تصمیم گیری در مورد اینکه این مقدار مناسب تست کردن چقدره، به تیم شما بستگی داره، اما با پوشش 100 درصدی (همه دستورالعمل‌ها و شاخه‌ها = statements and branches) اعتماد به نفس و آرامش خاطر کافی خواهید داشت. بنابراین علاوه بر داشتن یک چارچوب (framework) تست عالی، باید از یک 
[ابزار پوشش](https://gotwarlost.github.io/istanbul/)
 خوب هم استفاده کنید.

هیچ بهانه‌ای برای ننوشتن تست وجود نداره. [فریمورک های تست JS](https://jstherightway.org/#testing-tools) خوب زیادی وجود دارن، پس می‌تونید فریمورکی که تیم‌تون ترجیح میده رو پیدا کنید. وقتی فریمورکی که به درد تیم شما میخوره رو پیدا کردید، سعی کنید همیشه برای هر ویژگی/ماژول جدیدی که معرفی می‌کنید، تست بنویسید. روش ترجیحی شما توسعه تست محور (TDD) هم عالیه، اما نکته اصلی اینه که مطمئن شید قبل از راه‌اندازی هر ویژگی یا بازسازی (refactor) یک ویژگی موجود، به اهداف پوشش (coverage) می‌رسید.

### یک مفهوم در ازای هر تست

** این اصل به این موضوع اشاره میکنه که هر تست منحصربفرد، باید یک مفهوم یا رفتار از کد رو بسنجه. این اصل باعث میشه تست‌ها خواناتر، متمرکزتر و راحت‌تر قابل درک باشن.

**بد:**

```javascript
import assert from "assert";

describe("MomentJS", () => {
  it("handles date boundaries", () => {
    let date;

    date = new MomentJS("1/1/2015");
    date.addDays(30);
    assert.equal("1/31/2015", date);

    date = new MomentJS("2/1/2016");
    date.addDays(28);
    assert.equal("02/29/2016", date);

    date = new MomentJS("2/1/2015");
    date.addDays(28);
    assert.equal("03/01/2015", date);
  });
});
```

**خوب:**

```javascript
import assert from "assert";

describe("MomentJS", () => {
  it("handles 30-day months", () => {
    const date = new MomentJS("1/1/2015");
    date.addDays(30);
    assert.equal("1/31/2015", date);
  });

  it("handles leap year", () => {
    const date = new MomentJS("2/1/2016");
    date.addDays(28);
    assert.equal("02/29/2016", date);
  });

  it("handles non-leap year", () => {
    const date = new MomentJS("2/1/2015");
    date.addDays(28);
    assert.equal("03/01/2015", date);
  });
});
```

**[⬆ بازگشت](#فهرست-محتوا)**

## **همزمانی**

### به جای callback ها از promise ها استفاده کنید

** در جاوااسکریپت callback به تابعی گفته میشه که به عنوان ورودی تابع دیگه‌ای مورد استفاده قراره میگیره و توسط تابع اصلی فراخوانی میشه.

استفاده از callback ها باعث میشه کد خیلی تو در تو بشه و تمیز نباشه. با ES2015/ES6 میشه از Promises استفاده کرد که یک تایپ سراسری (global type) در جاوااسکریپت هست.

**بد:**

```javascript
import { get } from "request";
import { writeFile } from "fs";

get(
  "https://en.wikipedia.org/wiki/Robert_Cecil_Martin",
  (requestErr, response, body) => {
    if (requestErr) {
      console.error(requestErr);
    } else {
      writeFile("article.html", body, writeErr => {
        if (writeErr) {
          console.error(writeErr);
        } else {
          console.log("File written");
        }
      });
    }
  }
);
```

**خوب:**

```javascript
import { get } from "request-promise";
import { writeFile } from "fs-extra";

get("https://en.wikipedia.org/wiki/Robert_Cecil_Martin")
  .then(body => {
    return writeFile("article.html", body);
  })
  .then(() => {
    console.log("File written");
  })
  .catch(err => {
    console.error(err);
  });
```

**[⬆ بازگشت](#فهرست-محتوا)**

### استفاده از Async/Await حتی از Promises هم تمیزتره

با اینکه promise ها جایگزین بسیار تمیزی برای callback ها هستن، اما ES2017/ES8 راهکار async و await رو معرفی میکنه که راهکار تمیزتری هم هست. تنها کاری که باید بکنید اینه که قبل از اسم تابع کلیدواژه async رو اضافه کنید و بعدش میتونید بدون نوشتن زنجیره‌ای از then ها، همون منطق رو برنامه نویسی کنید. توصیه میشه هرجا که تونستید از این روش استفاده کنید.

**بد:**

```javascript
import { get } from "request-promise";
import { writeFile } from "fs-extra";

get("https://en.wikipedia.org/wiki/Robert_Cecil_Martin")
  .then(body => {
    return writeFile("article.html", body);
  })
  .then(() => {
    console.log("File written");
  })
  .catch(err => {
    console.error(err);
  });
```

**خوب:**

```javascript
import { get } from "request-promise";
import { writeFile } from "fs-extra";

async function getCleanCodeArticle() {
  try {
    const body = await get(
      "https://en.wikipedia.org/wiki/Robert_Cecil_Martin"
    );
    await writeFile("article.html", body);
    console.log("File written");
  } catch (err) {
    console.error(err);
  }
}

getCleanCodeArticle()
```

**[⬆ بازگشت](#فهرست-محتوا)**

## **مدیریت خطا**

دریافت خطا خیلی چیز خوبیه چون به این معنیه که زمان اجرای کد، وقتی مشکلی پیش اومده، با موفقیت شناسایی شدن. با توقف اجرای ادامه کد و از بین بردن فرآیند (در Node)، با اطلاعاتی که در کنسول به شما نمایش داده میشه، امکان ردیابی مشکل رو خواهید داشت.

### از خطاهایی که دریافت کردید چشم پوشی نکنید

واکنش نشون ندادن به خطا، هیچوقت قرار نیست اون خطارو رفع کنید، پرینت کردن اون در کنسول هم خیلی در حد چشم پوشی کردن اشتباهه، چرا که ممکنه بین دریایی از اطلاعاتی که در کنسول پرینت می‌کنید گم بشه. وقتی حتی یک کد کوتاه رو در try/catch قرار میدید، یعنی پیش بینی می‌کنید خطایی در اونجا رخ بده و بنابراین حتما باید برای مدیریت این خطا برنامه داشته باشید یا به وسیله اون خطا، دستورالعمل‌های دیگه‌ای رو درگیر کنید.

**بد:**

```javascript
try {
  functionThatMightThrow();
} catch (error) {
  console.log(error);
}
```

**خوب:**

```javascript
try {
  functionThatMightThrow();
} catch (error) {
  // One option (more noisy than console.log):
  console.error(error);
  // Another option:
  notifyUserOfError(error);
  // Another option:
  reportErrorToService(error);
  // OR do all three!
}
```

### از promise هایی که reject شدن غافل نشید

دقیقا به همون دلایلی که در مورد قبلی اشاره شد، باید promise های reject شده در try/catch رو هم به درستی مدیریت کرد.

**بد:**

```javascript
getdata()
  .then(data => {
    functionThatMightThrow(data);
  })
  .catch(error => {
    console.log(error);
  });
```

**خوب:**

```javascript
getdata()
  .then(data => {
    functionThatMightThrow(data);
  })
  .catch(error => {
    // One option (more noisy than console.log):
    console.error(error);
    // Another option:
    notifyUserOfError(error);
    // Another option:
    reportErrorToService(error);
    // OR do all three!
  });
```

**[⬆ بازگشت](#فهرست-محتوا)**

## **قالب بندی**

قالب بندی کد یه موضوع ذهنی/شخصی مثل بیشتر اصول دیگه اینجاست، هیچ قانون سخت و سریعی وجود نداره که ازش پیروی کنید. نکته اصلی اینه که سر قالب بندی بحث نکنید. هزاران ابزار برای خودکارسازی این کار وجود داره. از یکی از این ابزار استفاده کنید چون بحث در مورد قالب بندی برای مهندسان نرم افزار فقط اتلاف وقت و پوله.

برای گرفتن راهنمایی در مواردی که در حوزه قالب‌بندی خودکار قرار نمی‌گیرن (تورفتگی (indent)، تب یا فاصله (tab vs. space)، نشانه نقل‌قول دوتایی یا تکی (single vs. double quote)، و غیره) می‌تونید همینجارو دنبال کنید.

### از حروف بزرگ به صورت یکدست استفاده کنید

در جاوااسکریپت نوع (type) نداریم، برای همین حروف بزرگ خیلی میتونه در تفکیک متغیرها، تابع‌ها و غیره بهمون کمک کنه. این قواعد شخصیه و مهم اینه که تیم‌تون با چه ساختاری داره پیش میره، ولی هر ساختاری که انتخاب کردید، مطمئن بشید که بهش پایبند هستید.

**بد:**

```javascript
const DAYS_IN_WEEK = 7;
const daysInMonth = 30;

const songs = ["Back In Black", "Stairway to Heaven", "Hey Jude"];
const Artists = ["ACDC", "Led Zeppelin", "The Beatles"];

function eraseDatabase() {}
function restore_database() {}

class animal {}
class Alpaca {}
```

**خوب:**

```javascript
const DAYS_IN_WEEK = 7;
const DAYS_IN_MONTH = 30;

const SONGS = ["Back In Black", "Stairway to Heaven", "Hey Jude"];
const ARTISTS = ["ACDC", "Led Zeppelin", "The Beatles"];

function eraseDatabase() {}
function restoreDatabase() {}

class Animal {}
class Alpaca {}
```

**[⬆ بازگشت](#فهرست-محتوا)**

### تابع‌هایی که همدیگه رو فراخوانی میکنن، نزدیک هم بنویسید

اگه تابع‌هایی دارید که همدیگه رو فراخوانی میکنن، اون‌هارو در فایل کدتون به صورت عمودی نزدیک به هم بنویسید. در حالت ایده آل، تابعی که فراخوانی میکنه رو بالای تابعی که فراخوانی میشه نگه دارید. دلیلش اینه که معمولا کد رو مثل روزنامه از بالا به پایین میخونیم.

**بد:**

```javascript
class PerformanceReview {
  constructor(employee) {
    this.employee = employee;
  }

  lookupPeers() {
    return db.lookup(this.employee, "peers");
  }

  lookupManager() {
    return db.lookup(this.employee, "manager");
  }

  getPeerReviews() {
    const peers = this.lookupPeers();
    // ...
  }

  perfReview() {
    this.getPeerReviews();
    this.getManagerReview();
    this.getSelfReview();
  }

  getManagerReview() {
    const manager = this.lookupManager();
  }

  getSelfReview() {
    // ...
  }
}

const review = new PerformanceReview(employee);
review.perfReview();
```

**خوب:**

```javascript
class PerformanceReview {
  constructor(employee) {
    this.employee = employee;
  }

  perfReview() {
    this.getPeerReviews();
    this.getManagerReview();
    this.getSelfReview();
  }

  getPeerReviews() {
    const peers = this.lookupPeers();
    // ...
  }

  lookupPeers() {
    return db.lookup(this.employee, "peers");
  }

  getManagerReview() {
    const manager = this.lookupManager();
  }

  lookupManager() {
    return db.lookup(this.employee, "manager");
  }

  getSelfReview() {
    // ...
  }
}

const review = new PerformanceReview(employee);
review.perfReview();
```

**[⬆ بازگشت](#فهرست-محتوا)**

## **توضیحات**

### فقط در مواردی توضیحات اضافه کنید که به لحاظ منطقی (business logic) دارای پیچیدگی هستن

توضیحات حکم عذرخواهی رو دارن و یک نیازمندی نیستن. کدی که خوب نوشته شده باشه در بیشتر مواقع خودش، خودش رو توضیح میده.

**بد:**

```javascript
function hashIt(data) {
  // The hash
  let hash = 0;

  // Length of string
  const length = data.length;

  // Loop through every character in data
  for (let i = 0; i < length; i++) {
    // Get character code.
    const char = data.charCodeAt(i);
    // Make the hash
    hash = (hash << 5) - hash + char;
    // Convert to 32-bit integer
    hash &= hash;
  }
}
```

**خوب:**

```javascript
function hashIt(data) {
  let hash = 0;
  const length = data.length;

  for (let i = 0; i < length; i++) {
    const char = data.charCodeAt(i);
    hash = (hash << 5) - hash + char;

    // Convert to 32-bit integer
    hash &= hash;
  }
}
```

**[⬆ بازگشت](#فهرست-محتوا)**

### کدهای comment شده رو پاک کنید

کنترل ورژن (version control) به همین دلیل وجود داره، کد قدیمی رو بذارید فقط در تاریخچه کنترل ورژن بمونه.

**بد:**

```javascript
doStuff();
// doOtherStuff();
// doSomeMoreStuff();
// doSoMuchStuff();
```

**خوب:**

```javascript
doStuff();
```

**[⬆ بازگشت](#فهرست-محتوا)**

### توضیحات روزنامه وار ننویسید

یادتون باشه که از کنترل ورژن استفاده کنید! هیچ دلیلی برای داشتن کد مرده (dead code)، کدهای کامنت شده و مخصوصا توضیحاتی روزنامه وار وجود نداره. از دستوری `git log` برای دریافت تاریخچه کنترل ورژن استفاده کنید.

**بد:**

```javascript
/**
 * 2016-12-20: Removed monads, didn't understand them (RM)
 * 2016-10-01: Improved using special monads (JP)
 * 2016-02-03: Removed type-checking (LI)
 * 2015-03-14: Added combine with type-checking (JR)
 */
function combine(a, b) {
  return a + b;
}
```

**خوب:**

```javascript
function combine(a, b) {
  return a + b;
}
```

**[⬆ بازگشت](#فهرست-محتوا)**

### از نوشتن نشانگرهای تعیین موقعیت پرهیز کنید

این نشانگرها معمولا در خوانایی کد نویز ایجاد میکنن. بذارید اسامی تابع‌ها و متغیرها، تورفتگی‌ها (indentations) و قالب بندی (formatting) کدتون یک ساختار بصری برای کدی که نوشتین ایجاد کنه.

**بد:**

```javascript
////////////////////////////////////////////////////////////////////////////////
// Scope Model Instantiation
////////////////////////////////////////////////////////////////////////////////
$scope.model = {
  menu: "foo",
  nav: "bar"
};

////////////////////////////////////////////////////////////////////////////////
// Action setup
////////////////////////////////////////////////////////////////////////////////
const actions = function() {
  // ...
};
```

**خوب:**

```javascript
$scope.model = {
  menu: "foo",
  nav: "bar"
};

const actions = function() {
  // ...
};
```

**[⬆ بازگشت](#فهرست-محتوا)**

## ترجمه

این داکیومنت همچنین به زبان‌های زیر در دسترس هست:

- ![am](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Armenia.png) **Armenian**: [hanumanum/clean-code-javascript/](https://github.com/hanumanum/clean-code-javascript)
- ![bd](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Bangladesh.png) **Bangla(বাংলা)**: [InsomniacSabbir/clean-code-javascript/](https://github.com/InsomniacSabbir/clean-code-javascript/)
- ![br](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Brazil.png) **Brazilian Portuguese**: [fesnt/clean-code-javascript](https://github.com/fesnt/clean-code-javascript)
- ![cn](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/China.png) **Simplified Chinese**:
  - [alivebao/clean-code-js](https://github.com/alivebao/clean-code-js)
  - [beginor/clean-code-javascript](https://github.com/beginor/clean-code-javascript)
- ![tw](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Taiwan.png) **Traditional Chinese**: [AllJointTW/clean-code-javascript](https://github.com/AllJointTW/clean-code-javascript)
- ![fr](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/France.png) **French**: [eugene-augier/clean-code-javascript-fr](https://github.com/eugene-augier/clean-code-javascript-fr)
- ![de](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Germany.png) **German**: [marcbruederlin/clean-code-javascript](https://github.com/marcbruederlin/clean-code-javascript)
- ![id](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Indonesia.png) **Indonesia**: [andirkh/clean-code-javascript/](https://github.com/andirkh/clean-code-javascript/)
- ![it](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Italy.png) **Italian**: [frappacchio/clean-code-javascript/](https://github.com/frappacchio/clean-code-javascript/)
- ![ja](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Japan.png) **Japanese**: [mitsuruog/clean-code-javascript/](https://github.com/mitsuruog/clean-code-javascript/)
- ![kr](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/South-Korea.png) **Korean**: [qkraudghgh/clean-code-javascript-ko](https://github.com/qkraudghgh/clean-code-javascript-ko)
- ![pl](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Poland.png) **Polish**: [greg-dev/clean-code-javascript-pl](https://github.com/greg-dev/clean-code-javascript-pl)
- ![ru](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Russia.png) **Russian**:
  - [BoryaMogila/clean-code-javascript-ru/](https://github.com/BoryaMogila/clean-code-javascript-ru/)
  - [maksugr/clean-code-javascript](https://github.com/maksugr/clean-code-javascript)
- ![es](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Spain.png) **Spanish**: [tureey/clean-code-javascript](https://github.com/tureey/clean-code-javascript)
- ![es](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Uruguay.png) **Spanish**: [andersontr15/clean-code-javascript](https://github.com/andersontr15/clean-code-javascript-es)
- ![rs](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Serbia.png) **Serbian**: [doskovicmilos/clean-code-javascript/](https://github.com/doskovicmilos/clean-code-javascript)
- ![tr](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Turkey.png) **Turkish**: [bsonmez/clean-code-javascript](https://github.com/bsonmez/clean-code-javascript/tree/turkish-translation)
- ![ua](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Ukraine.png) **Ukrainian**: [mindfr1k/clean-code-javascript-ua](https://github.com/mindfr1k/clean-code-javascript-ua)
- ![vi](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Vietnam.png) **Vietnamese**: [hienvd/clean-code-javascript/](https://github.com/hienvd/clean-code-javascript/)

**[⬆ بازگشت](#فهرست-محتوا)**
