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
