# آزمایش شماره ۷ (بازآرایی)
# **اعمال الگوی Facade**
در بسیاری از سیستم‌های پیچیده، تعاملات پیچیده با کامپوننت‌های مختلف می‌تواند باعث پیچیدگی‌های زیادی در کد شود. 

## **ساده‌سازی فرآیند راه‌اندازی (Parser)**
### **مشکل:**
کلاس `Parser` در برنامه ما مستقیماً با چندین کامپوننت مانند `ParseTable`، `Rule` و `CodeGenerator` تعامل دارد. هر یک از این کامپوننت‌ها باید قبل از استفاده به‌طور جداگانه راه‌اندازی و پیکربندی شوند. این باعث می‌شود کد مشتری پیچیده شود و سخت‌تر قابل مدیریت و درک باشد، زیرا مشتری باید جزئیات پیچیده تعاملات میان این کامپوننت‌ها را مدیریت کند.

### **راه‌حل:**
برای ساده‌سازی فرآیند راه‌اندازی، الگوی **Facade** را با ایجاد کلاس `ParserFacade` پیاده‌سازی می‌کنیم. این فاساد وظیفه راه‌اندازی و پیکربندی کامپوننت‌های زیرساختی را بر عهده می‌گیرد و یک رابط کاربری ساده و یکپارچه برای تعامل با مشتری فراهم می‌کند. دیگر مشتری نیازی به پیکربندی تک‌تک کامپوننت‌ها ندارد — `ParserFacade` همه این کارها را به‌طور داخلی مدیریت می‌کند و یک API ساده برای تجزیه‌کردن ارائه می‌دهد.

### **الگوی Facade:**
الگوی **Facade** یک رابط سطح بالاتر برای مجموعه‌ای از رابط‌ها در یک زیرسیستم فراهم می‌کند که باعث می‌شود استفاده از آن زیرسیستم ساده‌تر باشد. در این مورد، ما از فاساد برای ساده‌سازی تعامل بین کلاس `Parser` و وابستگی‌های آن (مثل `ParseTable`، `Rule`، `CodeGenerator`) استفاده می‌کنیم. فاساد پیچیدگی‌های سیستم را پنهان می‌کند و فقط متدهای ضروری را برای مشتری نمایش می‌دهد.



```java
public class ParserFacade {
    private Parser parser;

    public ParserFacade() {
        parser = new Parser();
    }

    public void parseFile(String filePath) {
        try {
            Scanner sc = new Scanner(new File(filePath));
            parser.startParse(sc);
        } catch (FileNotFoundException e) {
            ErrorHandler.printError(e.getMessage());
        }
    }
}
```
## چطور کار می‌کند:

### ParserFacade:
کلاس `ParserFacade` مسئول راه‌اندازی کامپوننت‌های مورد نیاز (`ParseTable`, `Rule`, `CodeGenerator`) است. این کلاس همچنین متد `startParsing()` را ارائه می‌دهد که مشتری می‌تواند برای شروع عملیات تجزیه از آن استفاده کند.

### کد مشتری:
مشتری‌ها دیگر نیازی به ایجاد و ارسال کامپوننت‌های فردی به `Parser` ندارند. آن‌ها تنها با `ParserFacade` که پیچیدگی‌های راه‌اندازی و تعامل میان کامپوننت‌ها را پنهان می‌کند، تعامل می‌کنند.

## مزایای استفاده از الگوی Facade:

1. **کد مشتری ساده‌تر:** مشتریان دیگر نیازی به دانستن جزئیات نحوه راه‌اندازی کامپوننت‌های تجزیه‌گر ندارند. آن‌ها می‌توانند روی فراخوانی متد `startParsing()` در فاساد تمرکز کنند.
2. **کاهش وابستگی‌ها:** کد مشتری کمتر به جزئیات داخلی `Parser` و وابستگی‌های آن وابسته است. این باعث می‌شود سیستم نگهداری و گسترش آسان‌تری داشته باشد.
3. **افزایش خوانایی:** `ParserFacade` یک رابط تمیز و سطح بالا ارائه می‌دهد که کد را برای توسعه‌دهندگان آینده خواناتر و قابل‌درک‌تر می‌کند.
4. **نگهداری آسان‌تر:** اگر تغییراتی در نحوه راه‌اندازی کامپوننت‌ها (مثلاً افزودن کامپوننت‌های جدید یا تغییرات در منطق راه‌اندازی) به وجود آید، این تغییرات تنها در کلاس `ParserFacade` اعمال می‌شوند. این کاهش ریسک تأثیر بر کد مشتری را به همراه دارد.

## نتیجه‌گیری:
با استفاده از الگوی Facade، ما موفق شدیم کد مشتری را برای راه‌اندازی و تعامل با `Parser` ساده کنیم. حالا مشتری تنها باید با یک کلاس ساده (`ParserFacade`) تعامل کند و پیچیدگی‌های راه‌اندازی کامپوننت‌ها از طریق این رابط یکپارچه پنهان شده است. این باعث می‌شود کد آسان‌تر نگهداری، گسترش و استفاده شود.



# **اعمال الگوی Facade (دومین مورد): SymbolTableFacade**


## **مقدمه**

در اینجا ما از الگوی **(Facade)** استفاده می‌کنیم تا تعاملات پیچیده با جدول نمادها (SymbolTable) را پنهان کرده و یک رابط کاربری ساده‌تر برای کاربران فراهم کنیم.

هدف از پیاده‌سازی این فاساد، حفظ عملکرد اصلی جدول نمادها است، اما نحوه تعامل با آن را ساده و بهینه می‌کنیم. به این ترتیب کد مشتری ساده‌تر و قابل‌فهم‌تر خواهد شد.


## **پیاده‌سازی SymbolTableFacade**

کلاس `SymbolTableFacade` مسئولیت‌های زیر را بر عهده دارد:

1.  افزودن کلاس‌ها، متدها، فیلدها و متغیرها.
2.  انجام پرس‌وجو از نمادها، پارامترها و جزئیات متدها.
3.  تعامل با `SymbolTable` برای حفظ تمامی عملکردهای اصلی آن.

### **کد کامل SymbolTableFacade.java**
```java
public class SymbolTableFacade {
    private SymbolTable symbolTable;

    public SymbolTableFacade(Memory memory) {
        this.symbolTable = new SymbolTable(memory);
    }

    // Adds a class to the symbol table
    public void defineClass(String className) {
        symbolTable.addClass(className);
    }

    // Sets the superclass for a given class
    public void setSuperClass(String className, String superClass) {
        symbolTable.setSuperClass(superClass, className);
    }

    // Adds a field to a class
    public void defineField(String className, String fieldName, SymbolType fieldType) {
        symbolTable.setLastType(fieldType);
        symbolTable.addField(fieldName, className);
    }

    // Adds a method to a class
    public void defineMethod(String className, String methodName, SymbolType returnType, int codeAddress) {
        symbolTable.setLastType(returnType);
        symbolTable.addMethod(className, methodName, codeAddress);
    }

    // Adds a parameter to a method
    public void defineMethodParameter(String className, String methodName, String parameterName,
            SymbolType parameterType) {
        symbolTable.setLastType(parameterType);
        symbolTable.addMethodParameter(className, methodName, parameterName);
    }

    // Adds a local variable to a method
    public void defineMethodLocalVariable(String className, String methodName, String variableName,
            SymbolType variableType) {
        symbolTable.setLastType(variableType);
        symbolTable.addMethodLocalVariable(className, methodName, variableName);
    }

    // Retrieves the address of a keyword from the symbol table
    public Address getAddress(String keywordName) {
        return symbolTable.get(keywordName);
    }

    // Retrieves a field symbol from a class
    public Symbol getField(String className, String fieldName) {
        return symbolTable.get(fieldName, className);
    }

    // Retrieves a variable symbol from a method in a class
    public Symbol getVariable(String className, String methodName, String variableName) {
        return symbolTable.get(className, methodName, variableName);
    }

    // Retrieves the next parameter of a method
    public Symbol getNextParameter(String className, String methodName) {
        return symbolTable.getNextParam(className, methodName);
    }

    // Consumes the next parameter of a method
    public void consumeNextParameter(String className, String methodName) {
        symbolTable.startCall(className, methodName);
    }

    // Retrieves the return type of a method
    public SymbolType getMethodReturnType(String className, String methodName) {
        return symbolTable.getMethodReturnType(className, methodName);
    }

    // Retrieves the return address of a method
    public int getMethodReturnAddress(String className, String methodName) {
        return symbolTable.getMethodReturnAddress(className, methodName);
    }

    // Retrieves the address of a method in a class
    public int getMethodAddress(String className, String methodName) {
        return symbolTable.getMethodAddress(className, methodName);
    }

    // Retrieves the caller address of a method
    public int getMethodCallerAddress(String className, String methodName) {
        return symbolTable.getMethodCallerAddress(className, methodName);
    }

    public void setLastType(SymbolType type) {
        symbolTable.setLastType(type);
    }

}
```

## مزایای استفاده از SymbolTableFacade

1. **محصورسازی:** تمامی تعاملات با `SymbolTable` اکنون از طریق `SymbolTableFacade` انجام می‌شود که پیچیدگی‌های داخلی آن را پنهان می‌کند.
2. **خوانایی بیشتر:** کد مشتری (مانند `CodeGenerator`) ساده‌تر و خواناتر می‌شود، زیرا نیازی به تعامل مستقیم با متدهای داخلی `SymbolTable` ندارد.
3. **انعطاف‌پذیری:** تغییرات آینده در کلاس `SymbolTable` تأثیری بر کد مشتری نخواهد داشت، زیرا این تغییرات در داخل فاساد محصور می‌شوند.
4. **سادگی در استفاده:** توسعه‌دهندگان می‌توانند با استفاده از یک API ساده با `SymbolTable` کار کنند که این باعث کاهش خطاها و افزایش بهره‌وری می‌شود.

## نتیجه‌گیری

پیاده‌سازی کامل `SymbolTableFacade` اطمینان حاصل می‌کند که عملکرد سیستم همانطور که در کد اصلی بود حفظ می‌شود، در حالی که رابط ساده‌تری برای کاربران فراهم می‌آید. این رویکرد همچنین به کد شما کمک می‌کند که تمیزتر و قابل نگهداری‌تر باشد. اگر نیاز به تغییرات بیشتری دارید یا سوالی دارید، خوشحال می‌شوم که کمک کنم!


# الگوی استراتژی / وضعیت (State/Strategy Pattern) یا پلی‌مورفیسم به جای شرط‌ها

### مقدمه
در این refactoring، هدف حذف دستورات شرطی پیچیده (مانند دستورات `switch` یا `if-else`) از داخل متد `semanticFunction` در کلاس `CodeGenerator` است. به جای استفاده از شرایط مختلف، ما از الگوی **State/Strategy** یا **پلی‌مورفیسم** استفاده می‌کنیم تا هر عمل معنایی (semantic action) را در یک کلاس خاص محصور کنیم. این کار باعث می‌شود که کد ما تمیزتر، خواناتر و قابل نگهداری‌تر شود.

### نحوه عملکرد

1. **تعریف رابط استراتژی (Strategy Interface):** 
   رابط `SemanticAction` رفتار مشترک برای تمامی اعمال معنایی را تعریف می‌کند. این رابط متدی به نام `execute` دارد که به‌طور عمومی توسط تمامی کلاس‌های استراتژی پیاده‌سازی می‌شود.

2. **ایجاد کلاس‌های استراتژی (Concrete Strategy Classes):**
   برای هر نوع عمل معنایی (مثلاً عملیات جمع، تفریق، انتساب و غیره)، یک کلاس جداگانه ایجاد می‌شود که رابط `SemanticAction` را پیاده‌سازی می‌کند. این کلاس‌ها مسئول پیاده‌سازی هر عمل معنایی هستند.

3. **نگاشت اعمال معنایی به کدها:**
   در کلاس `CodeGenerator` یک نقشه (`Map`) نگهداری می‌شود که کدهای مربوط به اعمال معنایی را به پیاده‌سازی‌های کلاس‌های استراتژی نگاشت می‌کند. به‌این‌ترتیب، هر کد به یک عمل معنایی خاص ارجاع می‌دهد که به‌صورت یک شیء استراتژی درآورده می‌شود.

4. **استفاده از استراتژی‌ها:**
   هرگاه متد `semanticFunction` فراخوانی می‌شود، به‌طور خودکار عمل معنایی مربوطه از نقشه پیدا می‌شود و اجرای آن به کلاس استراتژی ارجاع داده می‌شود.

```java
‍‍‍public interface SemanticAction {
    void execute(CodeGenerator context, Token next);
}
```
```java
‍‍‍public class PrintAction implements SemanticAction {
    @Override
    public void execute(CodeGenerator context, Token next) {
        context.print();
    }
}
```
```java
‍‍‍public class SubAction implements SemanticAction {
    @Override
    public void execute(CodeGenerator context, Token next) {
        context.sub();
    }
}
```
```java
‍‍‍public class AssignAction implements SemanticAction {
    @Override
    public void execute(CodeGenerator context, Token next) {
        context.assign();
    }
}
```
```java
‍‍‍public class AddAction implements SemanticAction {
    @Override
    public void execute(CodeGenerator context, Token next) {
        context.add();
    }
}
```
### مزایای این Refactoring

- **حذف بلوک‌های شرطی بزرگ:** 
   دستورات شرطی (مانند `switch` یا `if-else`) که باعث پیچیدگی کد می‌شدند با یک الگوی استراتژی ساده و قابل توسعه جایگزین شده است.

- **اصل باز/بسته (Open/Closed Principle):** 
   در این روش، افزودن اعمال معنایی جدید فقط به ایجاد کلاس جدید و افزودن آن به نقشه نیاز دارد. بدون نیاز به تغییرات در کدهای موجود، می‌توان به راحتی عملکردهای جدیدی به سیستم اضافه کرد.

- **افزایش خوانایی کد:** 
   هر عمل معنایی در کلاس خود محصور می‌شود و این باعث می‌شود کد به‌راحتی قابل فهم‌تر و سازمان‌دهی‌شده‌تر باشد.

- **قابلیت استفاده مجدد:** 
   کلاس‌های استراتژی می‌توانند در بخش‌های مختلف سیستم مورد استفاده قرار بگیرند، بدون اینکه نیاز به کپی‌کردن کد باشد.

- **نگهداری و گسترش آسان‌تر:** 
   تغییرات در نحوه پیاده‌سازی هر عمل معنایی به راحتی در کلاس استراتژی مربوطه انجام می‌شود. این تغییرات تأثیری در سایر بخش‌های کد نخواهند داشت و نگهداری سیستم ساده‌تر می‌شود.
### نتیجه‌گیری

استفاده از الگوی استراتژی (یا پلی‌مورفیسم به جای شرط‌ها) باعث شده که کد ما از بلوک‌های شرطی پیچیده پاک شود و به یک ساختار ساده‌تر و قابل توسعه تبدیل شود. این تغییر باعث افزایش خوانایی، قابلیت نگهداری و انعطاف‌پذیری سیستم می‌شود و همچنین امکان افزودن ویژگی‌های جدید بدون تغییر در ساختار موجود را فراهم می‌کند.

اگر نیاز به اقدامات معنایی اضافی یا تغییرات بیشتر دارید، خوشحال می‌شوم که راه‌حل‌های خاصی برای شما فراهم کنم!


## Separate Query from Modifier (جداسازی پرسش از تغییرات)

### مقدمه
الگوی **جداسازی پرسش از تغییرات** (Separate Query From Modifier) به معنای جدا کردن عمل‌هایی است که وضعیت یک شیء را تغییر می‌دهند (یعنی متدهایی که باعث تغییر داده‌ها می‌شوند) از عمل‌هایی که فقط اطلاعات را باز می‌گردانند (یعنی متدهایی که تنها وضعیت شیء را بازخوانی می‌کنند).

این الگو معمولاً برای اجتناب از سردرگمی و ایجاد کدی واضح‌تر و تمیزتر استفاده می‌شود. در برخی موارد، ترکیب پرسش و تغییر وضعیت می‌تواند منجر به بروز اشتباهات یا رفتار غیرمنتظره در برنامه شود، بنابراین این الگو برای تفکیک این دو نوع عمل به کار می‌آید.

### نحوه عملکرد

1. **عملکرد پرسش (Query):**  
   متدهای پرسشی تنها اطلاعات را از شیء باز می‌گردانند بدون اینکه هیچ تغییری در وضعیت آن ایجاد کنند. این متدها معمولاً مقدار فیلدها یا ویژگی‌های شیء را بدون هیچ‌گونه تغییر یا تأثیر جانبی بر وضعیت شیء بازمی‌گردانند.

2. **عملکرد تغییر (Modifier):**  
   متدهای تغییر، وضعیت شیء را تغییر می‌دهند و ممکن است ویژگی‌های آن را به‌روزرسانی کنند یا سایر عملیات تأثیرگذار بر داده‌ها را انجام دهند.

3. **جدا کردن این دو نوع متد:**  
   در این الگو، متدهایی که برای پرسش از وضعیت شیء استفاده می‌شوند (مثل گرفتن مقدار یک فیلد) از متدهایی که تغییراتی در وضعیت شیء اعمال می‌کنند (مثل تنظیم مقدار یک فیلد) جدا می‌شوند. به این ترتیب، از بروز اشتباهات ناشی از تغییر وضعیت شیء در حین پرسش جلوگیری می‌شود.

### مزایای این Refactoring

1. **کاهش اشتباهات:**  
   با جدا کردن پرسش از تغییرات، احتمال بروز اشتباهات که در آن‌ها به‌طور تصادفی وضعیت شیء در حین پرسش تغییر کند، کاهش می‌یابد. این باعث می‌شود که کد قابل پیش‌بینی‌تر و قابل اطمینان‌تر باشد.

2. **کد تمیزتر و قابل خواندن‌تر:**  
   این تکنیک باعث می‌شود که کد شما خواناتر و منظم‌تر شود. متدهای پرسش و تغییر وضعیت به‌وضوح از هم متمایز هستند و هرکدام وظایف خاص خود را انجام می‌دهند، که باعث تسهیل در درک کد می‌شود.

3. **ساده‌تر شدن تست‌ها:**  
   زمانی که پرسش و تغییر از هم جدا شوند، تست‌های واحد (unit tests) نیز آسان‌تر می‌شوند. به‌عنوان مثال، می‌توانید متدهای پرسش را به‌طور جداگانه تست کنید بدون اینکه نگران تغییرات وضعیت شیء باشید.

4. **افزایش انعطاف‌پذیری:**  
   با جدا کردن این دو عملکرد، سیستم شما انعطاف‌پذیرتر می‌شود. اگر بخواهید نحوه تغییر وضعیت شیء را تغییر دهید، این تغییرات به راحتی انجام می‌شود بدون اینکه تأثیر منفی بر روی بخش‌های دیگر کد داشته باشد.

ما این بخش را در کد:
```java
‍‍‍public void semanticFunction(int func, Token next) {
        SemanticAction action = semanticActionsMap.get(func);
        if (action != null) {
            action.execute(this, next);
        } else {
            throw new IllegalArgumentException("Undefined semantic action: " + func);
        }
    }
```

به query و modifier تقسیم کردیم:

```java
‍‍‍public void performSemanticAction(int func, Token next) {
        if (isActionDefined(func)) {
            SemanticAction action = getSemanticAction(func);
            action.execute(this, next);
        } else {
            throw new IllegalArgumentException("Undefined semantic action: " + func);
        }
    }
```
```java
‍‍‍public void printMemory() {
        memory.pintCodeBlock();
    }
```
```java
public boolean isActionDefined(int func) {
    return semanticActionsMap.containsKey(func);
}

public SemanticAction getSemanticAction(int func) {
    return semanticActionsMap.get(func);
    }
```

## Self Encapsulated Field (فیلد محصور شده)

### مقدمه
**Self Encapsulated Field** یکی از تکنیک‌های Refactoring است که در آن به‌جای دسترسی مستقیم به فیلدهای یک کلاس، تمامی دسترسی‌ها به آن فیلد از طریق متدهای getter و setter داخلی کلاس انجام می‌شود. این کار موجب می‌شود که فیلدهای کلاس محصور (Encapsulated) شوند و امکان کنترل بیشتر و انعطاف‌پذیری در نحوه دسترسی به داده‌های کلاس فراهم گردد.

### نحوه عملکرد

1. **دسترسی به فیلدها از طریق متدها:**  
   در این الگو، به‌جای استفاده مستقیم از فیلدهای کلاس در سایر بخش‌های کد، تمامی دسترسی‌ها از طریق متدهای **getter** و **setter** انجام می‌شود. به این ترتیب، فیلدها به‌صورت **محصور** در کلاس باقی می‌مانند.

2. **محدود کردن دسترسی به فیلدها:**  
   متدهای getter و setter می‌توانند شامل منطق اضافی برای اعتبارسنجی و کنترل تغییرات روی فیلدها باشند. برای مثال، می‌توان قبل از اعمال تغییرات، داده‌ها را بررسی کرد تا اطمینان حاصل شود که شرایط خاصی رعایت می‌شوند.

3. **ساده‌سازی تغییرات در آینده:**  
   وقتی که دسترسی به فیلدها از طریق متدها انجام می‌شود، می‌توان به‌راحتی نحوه دسترسی به فیلدها را تغییر داد بدون اینکه به سایر بخش‌های کد نیازی به تغییرات باشد. مثلاً اگر بخواهیم نوع داده‌ی فیلد را تغییر دهیم، یا منطق پیچیده‌تری را برای دسترسی به فیلد اضافه کنیم، تنها باید در متدهای getter و setter تغییرات را اعمال کنیم.

### مزایای این Refactoring

1. **کنترل بیشتر:**  
   با استفاده از getter و setter، می‌توان کنترل بیشتری روی نحوه دسترسی به داده‌ها و اعمال تغییرات آن‌ها داشت. این باعث می‌شود که تغییرات کنترل‌شده‌تری در داده‌ها صورت گیرد.

2. **افزایش انعطاف‌پذیری:**  
   در صورت نیاز به تغییرات در نحوه دسترسی به فیلدها (مثلاً تغییر در نوع داده یا اضافه کردن منطق خاص به هنگام دریافت یا تغییر مقدار فیلد)، دیگر نیازی به تغییرات گسترده در سایر بخش‌های کد نخواهد بود.

3. **افزایش خوانایی و نگهداری آسان‌تر:**  
   استفاده از متدهای getter و setter باعث می‌شود که دسترسی به فیلدهای کلاس واضح‌تر باشد. این می‌تواند کد را تمیزتر و خواناتر کند و همچنین نگهداری و توسعه‌ی آن را آسان‌تر کند.

4. **حفاظت از داده‌ها:**  
   فیلدهایی که به‌طور مستقیم قابل دسترسی نیستند، می‌توانند از تغییرات نادرست محافظت شوند. از این طریق، تضمین می‌شود که فقط تغییرات معتبر و منطقی بر روی داده‌ها اعمال می‌شود.

```java
public class Symbol {

    // Private fields to ensure encapsulation
    private SymbolType type;
    private int address;

    // Constructor to initialize the fields
    public Symbol(SymbolType type, int address) {
        this.type = type;
        this.address = address;
    }

    // Getter for type
    public SymbolType getType() {
        return type;
    }

    // Setter for type
    public void setType(SymbolType type) {
        // Optionally, you could add validation here to control how the type is set
        this.type = type;
    }

    // Getter for address
    public int getAddress() {
        return address;
    }

    // Setter for address
    public void setAddress(int address) {
        // Optionally, you could add validation to ensure a valid address is set
        this.address = address;
    }
}
```


### نتیجه‌گیری

الگوی **Self Encapsulated Field** به شما این امکان را می‌دهد که دسترسی به فیلدهای کلاس را از طریق متدهای خاص و کنترل‌شده مدیریت کنید. این امر باعث می‌شود که تغییرات در نحوه دسترسی به داده‌ها ساده‌تر شود، کد خواناتر و نگهداری آن آسان‌تر گردد. در کل، این تکنیک به‌ویژه در پروژه‌های بزرگ و پیچیده می‌تواند به بهبود کیفیت کد و انعطاف‌پذیری آن کمک کند.

## انتقال متد (Moving Method)

### مقدمه
الگوی **انتقال متد** (Moving Method) زمانی استفاده می‌شود که متدی که در یک کلاس قرار دارد، بیشتر مرتبط با کلاسی دیگر است. در اینجا، متد از کلاسی که در آن قرار دارد به کلاسی دیگر منتقل می‌شود تا عملکرد سیستم بهینه‌تر و قابل فهم‌تر شود.

### نحوه عملکرد

در این الگو، متدهایی که از یک کلاس به کلاس دیگری منتقل می‌شوند، معمولاً ویژگی‌ها یا داده‌هایی دارند که به‌طور مستقیم به کلاس مقصد مرتبط هستند. این کار باعث می‌شود که کلاس‌ها تمرکز بیشتری روی وظایف خود داشته باشند و وابستگی‌ها به دیگر کلاس‌ها کاهش یابد.

### مزایای استفاده از انتقال متد

1. **کاهش پیچیدگی:** با انتقال متد به کلاسی که بیشتر با آن مرتبط است، پیچیدگی کلاس‌ها کاهش می‌یابد و هر کلاس تنها مسئولیت‌های خاص خود را بر عهده می‌گیرد.
2. **افزایش خوانایی:** کد خواناتر می‌شود زیرا متدها در کلاسی قرار می‌گیرند که به‌طور طبیعی انتظار می‌رود آن‌ها را در آنجا ببینید.
3. **کاهش وابستگی‌ها:** وابستگی‌ها بین کلاس‌ها کاهش می‌یابد و طراحی نرم‌افزار تمیزتر و ساده‌تر می‌شود.


### در پروژه
برای مثال در کد پروژه ما توابعی که به نظر میرسید بهتر از در کلاس klass باشد را از symbolTable خارج کردیم. مثلا توابع زیر را انتقال دادیم و آن ها را در توابع symbolTable صدا زدیم:
```java
public void addMethodParameter(String methodName, String parameterName) {
    Method method = Methodes.get(methodName);
    if (method != null) {
        method.addParameter(parameterName);
    }
}

public void addMethod(String methodName, int address, SymbolType returnType) {
    if (Methodes.containsKey(methodName)) {
        ErrorHandler.printError("This method already defined");
    }
    Methodes.put(methodName, new Method(address, returnType));
}
```

# ترکیب متدها (Composing Methods)

## مقدمه
الگوی **ترکیب متدها** (Composing Methods) زمانی استفاده می‌شود که بخواهیم متدهای کوچک و مجزای برنامه را به یک متد بزرگتر و جامع‌تر ترکیب کنیم. این کار باعث کاهش تکرار کد و بهبود خوانایی آن می‌شود.

## نحوه عملکرد
در این الگو، متدهای کوچک که وظایف مشابهی دارند و در چندین قسمت از کد فراخوانی می‌شوند، به یک متد جدید ترکیب می‌شوند. این ترکیب می‌تواند به کاهش تکرار و بهبود عملکرد برنامه کمک کند.

## مزایای استفاده از ترکیب متدها
- **کاهش تکرار کد:** با ترکیب متدهای مشابه و تکراری به یک متد، از نوشتن کدهای مشابه در قسمت‌های مختلف جلوگیری می‌شود.
- **ساده‌سازی کد:** ترکیب متدها باعث می‌شود کد ساده‌تر و خواناتر باشد، زیرا تکرار و پیچیدگی‌های اضافی از بین می‌روند.
- **افزایش قابلیت نگهداری:** زمانی که متدهای مشابه در یک مکان تجمع پیدا کنند، مدیریت و نگهداری آن‌ها ساده‌تر می‌شود.

## در کد پروژه
ما در پروژه Split Temporary Variable کردیم:
متوجه شدیم که از یک متغیر موقت برای نگهداری مقادیر مختلف در نقاط مختلف روش استفاده می شود. آن را به دو متغیر تقسیم کردیم. این زمانی مفید است که از یک متغیر برای اهداف متعدد استفاده می شود و دنبال کردن کد را سخت تر می کند.
```java
public Symbol get(String className, String methodName, String variable) {
        Symbol res = klasses.get(className).Methodes.get(methodName).getVariable(variable);
        if (res == null) res = get(variable, className);
        return res;
    }
```
به این کد تبدیل کردیم
```java
public Symbol get(String className, String methodName, String variable) {
        Symbol firstRes = klasses.get(className).Methodes.get(methodName).getVariable(variable);
        Symbol secondRes = firstRes == null ? get(variable, className) : firstRes;
        return secondRes;
    }
```

همچنین Temp را با Query جایگزین کردیم:
اگر از یک متغیر موقت برای ذخیره مقداری استفاده شود که بتوان آن را مستقیماً توسط یک متد محاسبه کرد، می‌توانیم متغیر temp را با یک فراخوانی متد جایگزین کنیم. این باعث می‌شود کد مستقیم‌تر و شفاف‌تر شود.
برای این کار، کد زیر را:
```java
public void setSuperClass(String superClass, String className) {
        Klass superclass = klasses.get(superClass);
        if (superclass != null) {
            klasses.get(className).setSuperClass(superclass);
        }
}
```
به دلیل استفاده تک باره از supperclass به کد زیر تبدیل کردیم:
```java
public void setSuperClass(String superClass, String className) {
        klasses.get(className).superClass = klasses.get(superClass);
    }
```

##سوالات
###سوال اول
- **کد تمیز (Clean Code):** کدی است که خوانا، قابل فهم و نگهداری باشد، و پیچیدگی‌های غیرضروری را حذف کرده باشد.
- **بدهی فنی (Technical Debt):** مشکلات و تصمیمات سریع و موقت در کدنویسی که در آینده نیاز به بازآرایی یا اصلاح دارند.
- **بوی بد (Code Smell):** نشانه‌ها و مشکلاتی در کد که ممکن است باعث سختی در نگهداری، توسعه یا فهم آن شود و نیاز به بازآرایی داشته باشد.

###سوال دوم
# بوهای بد در کدنویسی (Code Smells)

## 1. **Bloaters (کدهای حجیم)**
این دسته شامل کدهایی است که به مرور زمان بزرگ و پیچیده می‌شوند و کار با آن‌ها سخت می‌شود. این بوها معمولاً به مرور زمان و بدون توجه به آن‌ها شکل می‌گیرند.
- **Long Method**: متدهایی که بیش از حد طولانی هستند و وظایف زیادی را انجام می‌دهند.
- **Large Class**: کلاس‌هایی که بیش از حد بزرگ شده‌اند و بیش از یک وظیفه را بر عهده دارند.
- **Primitive Obsession**: استفاده بیش از حد از انواع داده‌ای ابتدایی (primitives) به جای کلاس‌های پیچیده‌تر.
- **Long Parameter List**: استفاده از پارامترهای زیاد در متدها که باعث کاهش خوانایی کد می‌شود.
- **Data Clumps**: گروه‌های مشابه از داده‌ها که به طور مکرر در کد تکرار می‌شوند.

## 2. **Object-Orientation Abusers (سوء استفاده از شی‌گرایی)**
این دسته شامل مواردی است که اصول شی‌گرایی به درستی پیاده‌سازی نشده‌اند و به نوعی شی‌گرایی به درستی در کد رعایت نمی‌شود.
- **Alternative Classes with Different Interfaces**: استفاده از کلاس‌های مختلف با رابط‌های مشابه یا متفاوت که می‌توانستند از یک رابط عمومی مشترک استفاده کنند.
- **Refused Bequest**: کلاس‌هایی که از کلاس پایه ارث‌بری می‌کنند اما برخی از ویژگی‌های آن را رد می‌کنند.
- **Switch Statements**: استفاده بیش از حد از دستورات `switch` که می‌تواند باعث پیچیدگی و کاهش انعطاف‌پذیری سیستم شود.
- **Temporary Field**: فیلدهای موقتی در کلاس‌ها که تنها برای مدت کوتاهی مورد استفاده قرار می‌گیرند و باعث کاهش انسجام کلاس می‌شوند.

## 3. **Change Preventers (موانع تغییرات)**
این دسته بوها به این معناست که برای اعمال یک تغییر کوچک در کد باید تغییرات زیادی در قسمت‌های مختلف کد انجام دهید. این موارد باعث پیچیدگی و افزایش هزینه توسعه و نگهداری سیستم می‌شوند.
- **Divergent Change**: زمانی که یک کلاس یا ماژول به دلیل تغییرات مختلفی که به طور مستقل اعمال می‌شوند، پیچیده می‌شود.
- **Parallel Inheritance Hierarchies**: زمانی که دو سلسله‌مراتب ارث‌بری موازی وجود دارند که نیاز به تغییرات همزمان دارند.
- **Shotgun Surgery**: زمانی که برای اعمال یک تغییر در سیستم باید تغییرات زیادی در چندین کلاس انجام دهید.

## 4. **Dispensables (چیزهای اضافی)**
این بوها به مواردی اشاره دارند که بی‌فایده و غیرضروری هستند و حذف آن‌ها باعث بهبود کیفیت کد می‌شود.
- **Comments**: نظرات بی‌معنی یا بیش از حد که کد را شلوغ می‌کنند.
- **Duplicate Code**: کدهای تکراری که باید به یک مکان مرکزی منتقل شوند.
- **Data Class**: کلاس‌هایی که تنها برای نگهداری داده‌ها استفاده می‌شوند و هیچ عملیاتی بر روی داده‌ها انجام نمی‌دهند.
- **Dead Code**: کدهایی که دیگر استفاده نمی‌شوند و باعث افزایش حجم کد می‌شوند.
- **Lazy Class**: کلاس‌هایی که وظایف بسیار کمی دارند و معمولاً به دلیل طراحی نادرست به وجود آمده‌اند.
- **Speculative Generality**: تلاش برای عمومی‌سازی و پیچیده کردن کد در حالتی که این کار ضروری نیست.

## 5. **Couplers (اتصال‌دهنده‌ها)**
این دسته شامل بوهایی است که باعث اتصال بیش از حد کلاس‌ها به یکدیگر یا استفاده نادرست از delegation می‌شوند.
- **Feature Envy**: زمانی که یک کلاس به جای انجام وظایف خود، بیشتر به ویژگی‌های کلاس‌های دیگر وابسته است.
- **Inappropriate Intimacy**: زمانی که دو کلاس بیش از حد به یکدیگر وابسته می‌شوند و اطلاعات داخلی یکدیگر را به طور مستقیم دستکاری می‌کنند.
- **Incomplete Library Class**: زمانی که یک کلاس کتابخانه‌ای ناقص یا غیر قابل استفاده است.
- **Message Chains**: زمانی که برای دسترسی به یک ویژگی باید سلسله‌ای از متدها را فراخوانی کنید.
- **Middle Man**: زمانی که یک کلاس تنها به منظور انتقال داده‌ها یا فراخوانی متدهای دیگر کلاس‌ها ساخته می‌شود و هیچ عملکرد خاصی ندارد.

---


# Lazy Class (کلاس تنبل)

## سوالات سوم

### 1. این بوی بد در کدام یک از دسته‌بندی‌های پنج‌گانه قرار می‌گیرد؟
**Lazy Class** یکی از بوهای بد کدنویسی است که در دسته‌بندی **Dispensables (چیزهای اضافی)** قرار می‌گیرد. این دسته شامل موارد غیرضروری و بی‌فایده‌ای است که حذف آن‌ها باعث تمیزتر شدن کد می‌شود.

---

### 2. برای برطرف‌کردن این بو، استفاده از کدام بازآرایی‌ها پیشنهاد می‌شود؟
برای برطرف کردن بوی بد Lazy Class، می‌توان از بازآرایی‌های زیر استفاده کرد:

- **Inline Class:** اگر کلاس وظایف کمی دارد و استفاده از آن توجیهی ندارد، می‌توان تمامی متدها و فیلدهای آن را به کلاسی که از آن استفاده می‌کند منتقل کرد و کلاس را حذف کرد.
- **Collapse Hierarchy:** اگر کلاس Lazy Class بخشی از یک سلسله‌مراتب ارث‌بری است و کاربرد خاصی ندارد، می‌توان آن را با کلاس والد یا فرزند ترکیب کرد.

---

### 3. در چه مواقعی باید این بو را نادیده گرفت؟
در برخی موارد، ممکن است **Lazy Class** به دلایل زیر قابل نادیده گرفتن باشد:

- **در فاز توسعه اولیه:** اگر کلاسی طراحی شده اما هنوز به طور کامل پیاده‌سازی نشده است و در آینده استفاده‌های بیشتری خواهد داشت.
- **آمادگی برای توسعه‌های آینده:** اگر کلاس برای ویژگی‌هایی طراحی شده که هنوز پیاده‌سازی نشده‌اند، می‌توان آن را حفظ کرد.
- **ساختار پروژه:** گاهی اوقات وجود کلاس‌هایی با وظایف کوچک برای خوانایی و تفکیک مسئولیت‌ها ضروری است، حتی اگر عملکرد کمی داشته باشند.

---

### نتیجه‌گیری
بوی بد **Lazy Class** معمولاً نشانه‌ای از وجود کلاس‌های غیرضروری است که باید حذف یا ترکیب شوند. اما در برخی شرایط خاص، این بو می‌تواند به دلیل آماده‌سازی برای توسعه‌های آینده توجیه شود. در هر صورت، تصمیم‌گیری برای رفع یا نادیده گرفتن آن باید با در نظر گرفتن ساختار کلی پروژه انجام شود.


