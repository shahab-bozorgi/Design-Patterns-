
✅ Abstract Factory چیه؟
**یک رابط (interface) برای ساختن خانواده‌ای از اشیاء مرتبط با هم** فراهم کنه، **بدون اینکه نیاز باشه کلاس دقیق اون اشیاء رو مشخص کنیم.**



## چرا باید ازش استفاده کنیم؟

فرض کن یه برنامه می‌نویسی که چند نوع شیء مرتبط تولید می‌کنه (مثل صندلی، میز و مبل). این اشیاء باید از یه نوع (سبک) خاص باشن. مثلاً:

- همه Modern باشن
    
- یا همه Victorian
    

اگه خودت بخوای این رو هندل کنی، سریع کدات پر از if/else می‌شه:
```
if theme == "modern":
    chair = ModernChair()
    sofa = ModernSofa()
elif theme == "victorian":
    chair = VictorianChair()
    sofa = VictorianSofa()

```

❌ این باعث می‌شه:

- کدت پیچیده و سخت قابل نگهداری باشه
    
- اضافه‌کردن سبک جدید، به معنی تغییر تو کدهای فعلی باشه (نقض اصل Open/Closed)
    

---
## مزایای Abstract Factory

1. **سازگاری بین اشیاء**
    
    - همه محصولات تولیدی (مثلاً صندلی و مبل و میز) با هم هم‌خوانی دارن.
        
2. **انعطاف‌پذیری**
    
    - می‌تونی به راحتی سبک‌های جدید اضافه کنی (ArtDeco مثلاً)، بدون تغییر در کد اصلی.
        
3. **جداسازی وابستگی‌ها**
    
    - کد کلاینت نمی‌دونه چه کلاس خاصی داره استفاده می‌شه؛ فقط با اینترفیس‌ها کار می‌کنه.


---
## 👶 یه تشبیه ساده:

فکر کن می‌خوای یه خانه کامل از یک شرکت طراحی سفارش بدی. نمی‌خوای در رو از یه شرکت، پنجره رو از یه شرکت دیگه، و دستگیره‌ها رو از جای سوم بگیری چون هماهنگ نیستن.

**Abstract Factory مثل یه شرکت طراحی کامله** که بر اساس سبک مورد نظر تو، **همه‌ی اجزاء رو ست‌شده می‌سازه**

---



 ----
 

مثال عملی 
 ---
----

## 📦 مشکل چیه؟

فرض کن تو یه فروشگاه مبلمان داری. مشتری‌ها می‌خوان ست مبلمان بخرن، یعنی:

- صندلی (Chair)
    
- مبل (Sofa)
    
- میز قهوه (CoffeeTable)
    

اما اینا **سبک** دارن:

- **Modern**
    
- **Victorian**
    
- **ArtDeco**
    

❌ اگه مشتری یه صندلی مدرن بگیره ولی مبل و میز سبک ویکتوریایی باشه، خوشحال نمی‌شه!


## 🧠 راه‌حل چیه؟ (Abstract Factory)

### مرحله 1: تعریف اینترفیس برای هر محصول

هر نوع محصول یه Interface می‌سازیم:
‍‍
```
class Chair:
    def sit_on(self): pass

class Sofa:
    def lie_on(self): pass

class CoffeeTable:
    def put_things_on(self): pass

```

مرحله 2: پیاده‌سازی کلاس‌های مختلف برای هر سبک
--

```
class ModernChair(Chair):
    def sit_on(self):
        print("Sitting on a modern chair")

class VictorianChair(Chair):
    def sit_on(self):
        print("Sitting on a victorian chair")

```

همین رو برای Sofa و CoffeeTable هم انجام می‌دیم.


مرحله 3: ساختن Abstract Factory
--
```
class FurnitureFactory:
    def create_chair(self) -> Chair: pass
    def create_sofa(self) -> Sofa: pass
    def create_coffee_table(self) -> CoffeeTable: pass


```

مرحله 4: پیاده‌سازی Factoryهای واقعی
--

```
class ModernFurnitureFactory(FurnitureFactory):
    def create_chair(self):
        return ModernChair()

    def create_sofa(self):
        return ModernSofa()

    def create_coffee_table(self):
        return ModernCoffeeTable()

```


مرحله 5: استفاده در کد کلاینت (بدون دونستن جزئیات)
--
```
def client_code(factory: FurnitureFactory):
    chair = factory.create_chair()
    sofa = factory.create_sofa()
    table = factory.create_coffee_table()

    chair.sit_on()
    sofa.lie_on()
    table.put_things_on()

```

حالا اگر از `ModernFurnitureFactory` استفاده کنی، همه چیز مدرن خواهد بود. اگه `VictorianFurnitureFactory` بدی، همه چیز سبک ویکتوریایی خواهد بود. بدون اینکه کد `client_code` رو تغییر بدی! ✅



## 🎯 نتیجه:

**Abstract Factory** بهت کمک می‌کنه که:

- **ست کامل** از اشیاء مرتبط بسازی
    
- بدون اینکه کلاینت بفهمه دقیقا داره از چه کلاس‌هایی استفاده می‌کنه
    
- تغییر سبک (variant) راحت باشه و نیاز به تغییر در کد اصلی نباشه



----

---

---



```
from abc import ABC, abstractmethod

# --------------------------
# Abstract Product Interfaces
# --------------------------

class CPU(ABC):
    @abstractmethod
    def info(self):
        pass

class GPU(ABC):
    @abstractmethod
    def info(self):
        pass

class RAM(ABC):
    @abstractmethod
    def info(self):
        pass

class Storage(ABC):
    @abstractmethod
    def info(self):
        pass

# --------------------------
# Concrete Products - Gaming
# --------------------------

class GamingCPU(CPU):
    def info(self):
        return "Gaming CPU: 4.5GHz, 8 cores, unlocked"

class GamingGPU(GPU):
    def info(self):
        return "Gaming GPU: RTX 4090, 24GB VRAM"

class GamingRAM(RAM):
    def info(self):
        return "Gaming RAM: 32GB DDR5, 6000MHz"

class GamingStorage(Storage):
    def info(self):
        return "Gaming Storage: 2TB NVMe SSD"

# --------------------------
# Concrete Products - Office
# --------------------------

class OfficeCPU(CPU):
    def info(self):
        return "Office CPU: 2.0GHz, 4 cores, low power"

class OfficeGPU(GPU):
    def info(self):
        return "Office GPU: Integrated Graphics"

class OfficeRAM(RAM):
    def info(self):
        return "Office RAM: 8GB DDR4, 2666MHz"

class OfficeStorage(Storage):
    def info(self):
        return "Office Storage: 500GB SATA SSD"

# --------------------------
# Abstract Factory
# --------------------------

class ComputerFactory(ABC):
    @abstractmethod
    def create_cpu(self) -> CPU: pass

    @abstractmethod
    def create_gpu(self) -> GPU: pass

    @abstractmethod
    def create_ram(self) -> RAM: pass

    @abstractmethod
    def create_storage(self) -> Storage: pass

# --------------------------
# Concrete Factories
# --------------------------

class GamingComputerFactory(ComputerFactory):
    def create_cpu(self) -> CPU:
        return GamingCPU()

    def create_gpu(self) -> GPU:
        return GamingGPU()

    def create_ram(self) -> RAM:
        return GamingRAM()

    def create_storage(self) -> Storage:
        return GamingStorage()

class OfficeComputerFactory(ComputerFactory):
    def create_cpu(self) -> CPU:
        return OfficeCPU()

    def create_gpu(self) -> GPU:
        return OfficeGPU()

    def create_ram(self) -> RAM:
        return OfficeRAM()

    def create_storage(self) -> Storage:
        return OfficeStorage()

# --------------------------
# Client Code
# --------------------------

def assemble_computer(factory: ComputerFactory):
    cpu = factory.create_cpu()
    gpu = factory.create_gpu()
    ram = factory.create_ram()
    storage = factory.create_storage()

    print(cpu.info())
    print(gpu.info())
    print(ram.info())
    print(storage.info())


# --------------------------
# Test
# --------------------------

print("🖥️ Gaming PC:")
assemble_computer(GamingComputerFactory())

print("\n💼 Office PC:")
assemble_computer(OfficeComputerFactory())

```


output:
```
🖥️ Gaming PC:
Gaming CPU: 4.5GHz, 8 cores, unlocked
Gaming GPU: RTX 4090, 24GB VRAM
Gaming RAM: 32GB DDR5, 6000MHz
Gaming Storage: 2TB NVMe SSD

💼 Office PC:
Office CPU: 2.0GHz, 4 cores, low power
Office GPU: Integrated Graphics
Office RAM: 8GB DDR4, 2666MHz
Office Storage: 500GB SATA SSD

```