Assisted Injection for Android ViewModels
=========================================

This library is based on and depends heavily on [Assisted Inject](https://github.com/square/AssistedInject)

ViewModelInject supports [Dagger2](https://google.github.io/dagger/) and [SavedStateHandle](https://developer.android.com/topic/libraries/architecture/viewmodel-savedstate)

Usage
-----

```java
class MyViewModel extends ViewModel {
    @ViewModelInject
    MyViewModel(Long foo, @Assisted SavedStateHandle savedStateHandle) {}
}
```

this will generate the following:

```java
public final class MyViewModel_AssistedFactory implements ViewModelSavedStateFactory<MyViewModel> {
    private final Provider<Long> foo;

    @Inject public MyViewModel_AssistedFactory(Provider<Long> foo) {
        this.foo = foo;
    }

    @Override public MyViewModel create(SavedStateHandle savedStateHandle) {
        return new MyViewModel(foo.get(), savedStateHandle);
    }
}
```

In order to allow Dagger to use the generated factory, define an assisted dagger module anywhere in 
the same gradle module:

```java
@ViewModelModule
@Module(includes = ViewModelInject_VMModule.class)
abstract class VMModule {}
``` 

The library will generate the `ViewModelInject_VMModule` for us.

Download
--------
This library is currently only released as a snapshot.
```groovy
repositories {
    // ...
    maven { url 'https://oss.sonatype.org/content/repositories/snapshots' }
}
```
```groovy
compileOnly 'com.vikingsen.inject:viewmodel-inject:0.1.0-SNAPSHOT'
annotationProcessor 'com.vikingsen.inject:viewmodel-inject-processor:0.1.0-SNAPSHOT'
```

License
=======

    Copyright 2019 Jordan Hansen

    Licensed under the Apache License, Version 2.0 (the "License");
    you may not use this file except in compliance with the License.
    You may obtain a copy of the License at

       http://www.apache.org/licenses/LICENSE-2.0

    Unless required by applicable law or agreed to in writing, software
    distributed under the License is distributed on an "AS IS" BASIS,
    WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
    See the License for the specific language governing permissions and
    limitations under the License.