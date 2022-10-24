# Java Reflection Library · BlackReflection

![](https://img.shields.io/badge/language-java-brightgreen.svg)

BlackReflection provides a series of API to use Java Reflection easily. Developer can use annotation to assign class, field and method. Then it will generate the reflection code automatically, developer don't need to write extra code to achieve Java Reflection.

## Usage

### Preparation

#### Step 1. Configure your build.gradle (in top level directory)
```gradle
allprojects {
    repositories {
        ...
        // Add
        maven { url 'https://jitpack.io' }
    }
}
```
#### Step 2. Add BlackReflection dependencies
```gradle
implementation 'com.github.CodingGay.BlackReflection:core:1.0.9'
annotationProcessor 'com.github.CodingGay.BlackReflection:compiler:1.0.9'
```

### Demo
#### 1. If you need to reflect the methods of top.niunaijun.app.bean.TestReflection, please refer to：[MainActivity.java](https://github.com/CodingGay/BlackReflection/blob/main/app/src/main/java/top/niunaijun/app/MainActivity.java)
```java
public class TestReflection {
    public static final String TAG = "TestConstructor";

    public String mContextValue = "context value";
    public static String sStaticValue = "static value";

    public TestReflection(String a) {
        Log.d(TAG, "Constructor called :" + a);
    }

    public TestReflection(String a, String b) {
        Log.d(TAG, "Constructor called : a = " + a + ", b = " + b);
    }

    public String testContextInvoke(String a, int b) {
        Log.d(TAG, "Context invoke: a = " + a + ", b = " + b);
        return a + b;
    }

    public static String testStaticInvoke(String a, int b) {
        Log.d(TAG, "Static invoke: a = " + a + ", b = " + b);
        return a + b;
    }

    public static String testParamClassName(String a, int b) {
        Log.d(TAG, "testParamClassName: a = " + a + ", b = " + b);
        return a + b;
    }
}

```
You can write an interface like this:
```java
@BClass(top.niunaijun.app.bean.TestReflection.class)
public interface TestReflection {

    @BConstructor
    top.niunaijun.app.bean.TestReflection _new(String a, String b);

    @BConstructor
    top.niunaijun.app.bean.TestReflection _new(String a);

    @BMethod
    String testContextInvoke(String a, int b);

    @BStaticMethod
    String testStaticInvoke(String a, int b);

    @BStaticMethod
    String testParamClassName(@BParamClassName("java.lang.String") Object a, int b);

    @BField
    String mContextValue();

    @BStaticField
    String sStaticValue();
}

```
#### 2. Build your project, it will generate the code automatically.

#### 3. Write the code heartily!
Constructor:
```java
TestReflection testReflection = BRTestReflection.get()._new("a");
TestReflection testReflection = BRTestReflection.get()._new("a", "b");
```

Reflect Methods:
```java
// Static Method
BRTestReflection.get().testStaticInvoke("static", 0);

// Non-static Method
BRTestReflection.get(testReflection).testContextInvoke("context", 0);
```

Reflect Fields:
```java
// Static Field
String staticValue = BRTestReflection.get().sStaticValue();

// Non-static Field
String contextValue = BRTestReflection.get(testReflection).mContextValue();
```

Set the value of Fields:
```java
// Static Field
BRTestReflection.get()._set_sStaticValue(staticValue + " changed");

// Non-static Field
BRTestReflection.get(testReflection)._set_mContextValue(contextValue + " changed");
```
BRTestReflection is a class which generated by the program automatically.
Generation rule: BR + ClassName
- BRTestReflection.get() ------> It is used to invoke static method
- BRTestReflection.get(caller) ------> It is used to invoke non-static method
#### Annotation API

Annotation | Target | Description
---|---|---
@BClass | Type(Class) | Assign the class which you want to manipulate
@BClassName | Type(Class) | Assign the class which you want to manipulate
@BConstructor | Method | Assign the constructor
@BStaticMethod | Method | Assign the static method
@BMethod | Method | Assign the non-static method
@BStaticField | Method | Assign the static field
@BField | Method | Assign the non-static field
@BParamClass | Parameter | Assign the class of parameter
@BParamClassName | Parameter | Assign the class of parameter

### Obfuscation rules
```
-keep class top.niunaijun.blackreflection.** {*; }
-keep @top.niunaijun.blackreflection.annotation.BClass class * {*;}
-keep @top.niunaijun.blackreflection.annotation.BClassName class * {*;}
-keep @top.niunaijun.blackreflection.annotation.BClassNameNotProcess class * {*;}
-keepclasseswithmembernames class * {
    @top.niunaijun.blackreflection.annotation.BField.* <methods>;
    @top.niunaijun.blackreflection.annotation.BFieldNotProcess.* <methods>;
    @top.niunaijun.blackreflection.annotation.BFieldSetNotProcess.* <methods>;
    @top.niunaijun.blackreflection.annotation.BFieldCheckNotProcess.* <methods>;
    @top.niunaijun.blackreflection.annotation.BMethod.* <methods>;
    @top.niunaijun.blackreflection.annotation.BStaticField.* <methods>;
    @top.niunaijun.blackreflection.annotation.BStaticMethod.* <methods>;
    @top.niunaijun.blackreflection.annotation.BMethodCheckNotProcess.* <methods>;
    @top.niunaijun.blackreflection.annotation.BConstructor.* <methods>;
    @top.niunaijun.blackreflection.annotation.BConstructorNotProcess.* <methods>;
}
```
### License

> ```
> Copyright 2022 Milk
>
> Licensed under the Apache License, Version 2.0 (the "License");
> you may not use this file except in compliance with the License.
> You may obtain a copy of the License at
>
>    http://www.apache.org/licenses/LICENSE-2.0
>
> Unless required by applicable law or agreed to in writing, software
> distributed under the License is distributed on an "AS IS" BASIS,
> WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
> See the License for the specific language governing permissions and
> limitations under the License.
> ```