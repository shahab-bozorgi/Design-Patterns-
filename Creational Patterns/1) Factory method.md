

## تعریف ساده‌ی Factory Method

**هدف:**  
جدا کردن فرآیند _ساخت آبجکت_ از کدی که از اون آبجکت استفاده می‌کنه، به‌طوری‌که کلاس‌های فرزند بتونن نوع شیء ساخته‌شده رو تغییر بدن.


## تعریف رسمی

> Factory Method یک الگوی ساختاری (Creational) است که **یک اینترفیس برای ساخت آبجکت ارائه می‌دهد، ولی پیاده‌سازی این ساختن را به کلاس‌های فرزند می‌سپارد**.




## ساختار کلی

### 1. **Creator (سازنده):**

کلاس پایه‌ای که متدی به نام `factoryMethod()` داره — این متد وظیفه ساخت آبجکت رو داره (اما معمولاً پیاده‌سازی نمی‌شه یا پیاده‌سازی پایه داره).

### 2. **ConcreteCreator (سازنده‌ی واقعی):**

کلاس فرزند که متد `factoryMethod()` رو override می‌کنه و نوع خاصی از آبجکت رو می‌سازه.

### 3. **Product (محصول):**

اینترفیس یا کلاس پایه‌ای برای آبجکت‌هایی که ساخته می‌شن.

### 4. **ConcreteProduct:**

پیاده‌سازی‌های مختلف برای `Product`


## چرا از Factory Method استفاده کنیم؟

- وقتی نمی‌خوایم کد کلاینت به کلاس خاصی وابسته باشه (loose coupling)
    
- وقتی نوع شیء باید قابل تغییر باشه در آینده
    
- وقتی کد ساختن پیچیده‌ست یا نیاز به تنظیمات خاص داره


------------


# **قدم به قدم پیاده‌سازی**

. `Transport` interface (یا کلاس پایه)

```
from abc import ABC, abstractmethod

class Transport(ABC):
    @abstractmethod
    def deliver(self):
        pass

```

---


. `Truck` و `Ship` که Transport رو پیاده‌سازی می‌کنن

```
class Truck(Transport):
    def deliver(self):
        print("Deliver by land in a box")

class Ship(Transport):
    def deliver(self):
        print("Deliver by sea in a container")

```

----

`Logistics` کلاس سازنده (Creator)

```
class Logistics(ABC):
    @abstractmethod
    def create_transport(self) -> Transport:
        pass

    def plan_delivery(self):
        transport = self.create_transport()
        transport.deliver()

```

این کلاس توابع عمومی مثل `plan_delivery()` رو داره، ولی ساختن وسیله‌ی نقلیه رو می‌سپره به کلاس‌های فرزند

----
کلاس‌های Concrete Creator (Road و Sea)

```
class RoadLogistics(Logistics):
    def create_transport(self) -> Transport:
        return Truck()

class SeaLogistics(Logistics):
    def create_transport(self) -> Transport:
        return Ship()

```

---


کلاینت — بدون دانستن نوع وسیله‌ی نقلیه!

```
def client_code(logistics: Logistics):
    logistics.plan_delivery()

# استفاده
client_code(RoadLogistics())  # خروجی: Deliver by land in a box
client_code(SeaLogistics())   # خروجی: Deliver by sea in a container

```



-----
---
## مزایای این ساختار:

- کد کلاینت **وابسته نیست** به کلاس‌های خاص مثل `Truck` یا `Ship`
    
- اضافه کردن نوع جدید (مثلاً هواپیما یا پهپاد) فقط نیاز به ساخت یک کلاس جدید داره، نه تغییر در کد قبلی
    
- اصل **Open/Closed Principle** رعایت می‌شه



خلاصه تصویری ساختار:
```
         +-------------------+
         |    Logistics      |  <- Creator
         +-------------------+
         | + create_transport() -> Transport (abstract)
         | + plan_delivery()  # common logic
         +-------------------+
              /            \
             /              \
+----------------+   +----------------+
| RoadLogistics  |   | SeaLogistics   | <- ConcreteCreators
| create_transport() | create_transport()
| return Truck()     | return Ship()
+----------------+   +----------------+

          ↑                    ↑
        Truck()             Ship()
        deliver()           deliver()

          ↑                    ↑
       implements Transport interface

```



----
----
---

# **در صورت گنگ بودن**:


## چرا Factory Method و کلاس‌های انتزاعی مفیدند؟

### ۱. **جداسازی ساختن اشیا از استفاده از آنها**

- کلاینت (کدی که می‌خواد کاری انجام بده) نیاز نداره بدونه دقیقاً چه کلاسی داره ساخته می‌شه.
    
- فقط باید بدونه شیء یه «رابط» یا اینترفیس مشخص رو پشتیبانی می‌کنه.
    
- این باعث می‌شه هر وقت خواستی نوع جدیدی از کلاس‌ها رو اضافه کنی (مثلاً هواپیما یا قطار یا هر نوع وسیله‌ی نقلیه دیگه)
    
- لازم نباشه کد اصلی رو تغییر بدی، فقط کلاس جدید رو درست می‌کنی.
    

---

### ۲. **رعایت اصل Open/Closed Principle (OCP)**

- کد شما «باز برای توسعه» ولی «بسته برای تغییر» می‌مونه.
    
- یعنی می‌تونی قابلیت جدید اضافه کنی بدون اینکه کد قبلی رو تغییر بدی و خطا یا باگ بزنی.
    

---

### ۳. **کاهش کدهای شرطی (if-else های متعدد)**

- به جای اینکه توی برنامه پر از شرط بزاری برای ساختن انواع مختلف اشیا، همه‌ی این مسئولیت ساخت رو به یک متد می‌سپاری که توی کلاس‌های فرزند روش کار خودش رو داره.
    
- این کار باعث تمیزی و خوانایی کد می‌شه.
    

---

### ۴. **سادگی نگهداری و توسعه**

- وقتی پروژه بزرگ‌تر شد و هزاران خط کد داشت، این مدل باعث می‌شه راحت‌تر بتونی تغییر بدی و توسعه بدی بدون اینکه بخش‌های دیگه رو خراب کنی.
    

---

### ۵. **مثال دنیای واقعی**

فرض کن داری یه برنامه بازی می‌سازی که شخصیت‌های مختلف داره:

- یه بار قهرمان `Hero` می‌سازیم
    
- یه بار دشمن `Enemy` می‌سازیم
    
- یه بار شخصیت کمکی `Support` می‌سازیم
    

با Factory Method، وقتی قراره شخصیت ساخته بشه، کد بازی فقط می‌گه:  
`character = create_character()`  
و نوع دقیقش رو کلاس‌های فرزند مشخص می‌کنن.

اینطوری هر وقت بخوای شخصیت جدید اضافه کنی، نیازی نیست بری همه‌جا کد رو تغییر بدی.

---

## نتیجه نهایی:

**Factory Method باعث می‌شه برنامه‌ات انعطاف‌پذیرتر، قابل توسعه‌تر، و منظم‌تر بشه و کلی دردسرهای تغییر کد رو کم می‌کنه.**