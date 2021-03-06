# Make Immutable (IDEA-Plugin)
## Motivation
This plugin was inspired by the project [Immutables](https://immutables.github.io/). 
- If you can use annotations in your project, then I recommend you to take a look at 
[Immutables](https://immutables.github.io/).
- If you need something really simple and rather generating cource code in place, that you
can easily modify, then take a look at this plugin.

## Description
This plugin makes a class immutable by applying the following changes:
     
1. Class modifier `final` is added if not present.
2. Fields modifier `final` is added if not present.
3. Constructor/s visibility changed to `private`. 
The class must have at least one constructor.
4. Generation of static constructor (method name: `of`) for each private constructor.
5. Generation of `getters`.
6. Generation of `withers` (`withXYZ` Methods).

## Download
This plugin can be downloaded from the JetBrains Plugins Repository: [Make Immutable](https://plugins.jetbrains.com/plugin/11436-make-immutable)

## Usage

- **Precondition**: The class must have at least one constructor.
- **Expectation**: 
    - The methods are generated only if there is no methods matching the signature.
    - `getters` and `withers` are generated based on the class fields and constructors arguments (see example below)
- **Usage**: Code | Generate (Alt + Insert) | Make Immutable

## Example
Let's assume you have the following class (no getters and setters):
```java
public class SomeClass<T, R> {

    private String value1;
    private T value2;
    private R value3;
    private boolean value4;

    public SomeClass(String value1a, String value1b, T value2, R value3, boolean value4) {
        this.value1 = value1a + value1b;
        this.value2 = value2;
        this.value3 = value3;
        this.value4 = value4;
    }
}
```
Then, if you run the plugin 'Make Immutable' you would obtain the following result:
```java
public final class SomeClass<T, R> {

    private final String value1;
    private final T value2;
    private final R value3;
    private final boolean value4;

    private SomeClass(String value1a, String value1b, T value2, R value3, boolean value4) {
        this.value1 = value1a + value1b;
        this.value2 = value2;
        this.value3 = value3;
        this.value4 = value4;
    }

    public static <T, R> SomeClass of(String value1a, String value1b, T value2, R value3, boolean value4) {
        return new SomeClass<>(value1a, value1b, value2, value3, value4);
    }

    public String getValue1() {
        return this.value1;
    }

    public T getValue2() {
        return this.value2;
    }

    public R getValue3() {
        return this.value3;
    }

    public boolean isValue4() {
        return this.value4;
    }

    public String getValue1a() {
        throw new RuntimeException("This method must be implemented."); // TODO
    }

    public String getValue1b() {
        throw new RuntimeException("This method must be implemented."); // TODO
    }

    public SomeClass withValue1a(String value1a) {
        return of(value1a, getValue1b(), getValue2(), getValue3(), isValue4());
    }

    public SomeClass withValue1b(String value1b) {
        return of(getValue1a(), value1b, getValue2(), getValue3(), isValue4());
    }

    public SomeClass withValue2(T value2) {
        return of(getValue1a(), getValue1b(), value2, getValue3(), isValue4());
    }

    public SomeClass withValue3(R value3) {
        return of(getValue1a(), getValue1b(), getValue2(), value3, isValue4());
    }

    public SomeClass withValue4(boolean value4) {
        return of(getValue1a(), getValue1b(), getValue2(), getValue3(), value4);
    }


}
``` 

## License

Copyright 2018 by Grebiel José Ifill Brito

Licensed under the Apache License, Version 2.0 (the "License");
you may not use this file except in compliance with the License.
You may obtain a copy of the License at

    http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and
limitations under the License.
