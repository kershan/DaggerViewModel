# DaggerViewModel

An integration Module for injecting Google Architecture Components' [ViewModel][viewmodel] into
[Dagger2][dagger2]-injected Android activities.

This library was inspired by the official [GithubBrowserSample][GithubBrowserSample] example.

## Usage

Install the library, then bind your `ViewModel`s into an abstract `@Module`-annotated class 
annotating your binding method with `@IntoMap` and `@ViewModelKey`, and giving to this
last annotation your custom `ViewModel` class as parameter.
For example:

     @Module
     abstract class ViewModelModule {
         @Binds
         @IntoMap
         @ViewModelKey(MyViewModel::class)
         abstract fun bindsMainViewModel(myViewModel: MyViewModel): ViewModel
         
         // other binds...
     }
 
Add the `DaggerViewModelModule` to your Application Component
    
    @Singleton
    @Component(modules = arrayOf(AndroidSupportInjectionModule::class, DaggerViewModelModule::class,
            your other modules...))
    interface ApplicationComponent {
        // your component definitions ...
    }

in Kotlin, or in Java:
        
    @Singleton
    @Component(modules = {AndroidSupportInjectionModule.class, DaggerViewModelModule.class, 
        your other modules...})
    public interface ApplicationComponent {
        // your component definitions ...
    }
    
And finally you can `@Inject` a `ViewModelProvider.Factory` into your Activity/Fragment and use it
to create your ViewModel.
 
Kotlin example:
    
    class MainActivity : AppCompatActivity() {
        @Inject
        lateinit var viewModelFactory: ViewModelProvider.Factory
        
        private lateinit var viewModel: MyViewModel
        
        override fun onCreate(savedInstanceState: Bundle?) {
            AndroidInjection.inject(this)
            super.onCreate(savedInstanceState)
            
            setContentView(R.layout.activity_main)
            viewModel = ViewModelProviders.of(this, viewModelFactory).get(MyViewModel::class.java)
            
            // ...
        }
        
        // ...
    }
    
or in Java:

    public class MainActivity extends AppCompatActivity {
        @Inject
        ViewModelProvider.Factory viewModelFactory;
            
        private MyViewModel viewModel;
        
        
        void onCreate(Bundle savedInstanceState) {
            AndroidInjection.inject(this);
            super.onCreate(savedInstanceState);
            
            setContentView(R.layout.activity_main);
            viewModel = ViewModelProviders.of(this, viewModelFactory).get(MyViewModel.class);
            
            // ...
        }
        
        // ...
    }

## License

Copyright 2013 Alex Facciorusso.

Licensed under the Apache License, Version 2.0 (the "License");
you may not use this file except in compliance with the License.
You may obtain a copy of the License at

   http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and
limitations under the License.


[viewmodel]: https://developer.android.com/topic/libraries/architecture/viewmodel.html
[dagger2]: https://google.github.io/dagger/
[GithubBrowserSample]: https://github.com/googlesamples/android-architecture-components/tree/master/GithubBrowserSample