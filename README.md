Learning RxJava for Android by example
==============

This is a repository with real-world useful examples of using RxJava with Android. [It usually will be in a constant state of "Work in Progress" (WIP)](http://nerds.weddingpartyapp.com/tech/2014/09/15/learning-rxjava-with-android-by-example/).

## Examples:

### [Concurrency using schedulers](https://github.com/kaushikgopal/Android-RxJava/blob/master/app/src/main/java/com/morihacky/android/rxjava/ConcurrencyWithSchedulersDemoFragment.java)

A common requirement is to offload lengthy heavy I/O intensive operations to a background thread (non-UI thread), and feed the results back to the UI/main thread, on completion. This is a demo of how long running operations can be offloaded to a background thread. After the operation is done, we resume back on the main thread. All using RxJava! Think of this as a replacement to AsyncTasks.

The long operation is simulated by a blocking Thread.sleep call (since this is done in a background thread, our UI is never interrupted).

To really see this example shine. Hit the button multiple times and see how the button click (which is a ui operation) is never blocked because the long operation only runs in the background.

### [Accumulate calls (buffer)](https://github.com/kaushikgopal/Android-RxJava/blob/master/app/src/main/java/com/morihacky/android/rxjava/BufferDemoFragment.java)

This is a demo of how events can be accumulated using the "buffer" operation.

A button is provided and we accumulate the number of clicks on that button, over a span of time and then spit out the final results.

If you hit the button once. you'll get message saying the button was hit once. If you hit it 5 times continuosly within a span of 2 seconds, then you get a single log, saying you hit that button 5 times (vs 5 individual logs saying "Button hit once").

Two possible implementations:

2a. Using a traditional observable - but encompassing the OnClick within the observable (as demoed here)
2b. Using PublishSubject and sending single clicks to the Observable, which in-turn then sends it to the Observer

### [Instant/Auto searching (subject + debounce)](https://github.com/kaushikgopal/Android-RxJava/blob/master/app/src/main/java/com/morihacky/android/rxjava/SubjectDebounceSearchEmitterFragment.java)

This is a demo of how events can be swallowed in a way that only the last one is respected. A typical example of this is instant search result boxes. As you type the word "Bruce Lee", you don't want to execute searches for B, Br, Bru, Bruce, Bruce , Bruce L ... etc. But rather intelligently wait for a couple of moments, make sure the user has finished typing the whole word, and then shoot out a single call for "Bruce Lee".

As you type in the input box, it will not shoot out log messages at every single input character change, but rather only pick the lastly emitted event (i.e. input) and log that.

This is the debounce/throttleWithTimeout method in RxJava.

### [Retrofit and RxJava (zip, flatmap)](https://github.com/kaushikgopal/Android-RxJava/blob/master/app/src/main/java/com/morihacky/android/rxjava/RetrofitFragment.java)

[Retrofit from Square](http://square.github.io/retrofit/) is an amazing library that helps with easy networking (even if you haven't made the jump to RxJava just yet, you really should check it out). It works even better with RxJava and these are examples hitting the github api, taken straight up from the android demigod-developer Jake Wharton's talk at Netflix. You can [watch the talk](https://www.youtube.com/watch?v=aEuNBk1b5OE#t=2480) at this link. Incidentally, my motiviation to use RxJava was from attending this talk at Netflix.

Since it was a presentation, Jake only put up the most important code snippets in [his slides](https://speakerdeck.com/jakewharton/2014-1). Also he uses Java 8 in them, so I flushed those examples out in ~~good~~ old Java 6. (Note: you're most likely to hit the github api quota pretty fast so send in an oauth-token as a parameter if you want to keep running these examples often).

### Orchestrating Observables. Make parallel network calls, then combine the result into a single data point  (flatmap + zip)

The below ascii diagram expresses the intention of our next example with panache. f1,f2,3,f4,f5 are essentially network calls that when made, give back a result that's needed for a future calculation.


             (flatmap)
    f1 ___________________ f3 _______
             (flatmap)               |    (zip)
    f2 ___________________ f4 _______| ___________  final output
            \                        |
             \____________ f5 _______|

The code for this example has already been written by one Mr.skehlet in the interwebs. Head over to [the gist](https://gist.github.com/skehlet/9418379) for the code. It's written in pure Java (6) so it's pretty comprehensible if you've understood the previous examples. I'll flush it out here again when time permits or I've run out of other compelling examples.

### [Double binding with TextViews](https://github.com/kaushikgopal/Android-RxJava/blob/master/app/src/main/java/com/morihacky/android/rxjava/DoubleBindingTextViewFragment.java)

Auto-updating views are a pretty cool thing. If you've dealt with Angular JS before, they have a pretty nifty concept called "two way data binding", where when an HTML element is bound to a model/entity object, it constantly "listens" to changes on that entity and auto-updates its state based on the model. Using the technique in this example, you could potentially use a pattern like the [Presentation View Model pattern](http://martinfowler.com/eaaDev/PresentationModel.html) with great ease.

While the example here is pretty rudimentary, the technique used to achieve the double binding using a `Publish Subject` is much more interesting.

### [Polling with Schedulers](https://github.com/kaushikgopal/Android-RxJava/blob/master/app/src/main/java/com/morihacky/android/rxjava/PollingFragment.java)

This is an example of polling using RxJava Schedulers. This is useful in cases, where you want to constantly poll a server and possibly get new data. The network call is "simulated" so it forces a delay before return a resultant string.


## Work in Progress:

Examples that I would like to have here, but haven't found the time yet to flush out.

### First retrieve from cached data, if no cache found make a network call if you can't find your data (concat) (wip)
[Courtesy: gist](https://gist.github.com/adelnizamutdinov/7483969)

### Pagination (wip)

a. Simple pagination
b. Optimized pagination

### Event Bus with RxJAva (wip)

http://stackoverflow.com/questions/19266834/rxjava-and-random-sporadic-events-on-android


## Contributing:

I try to ensure the examples are not overly contrived but reflect a real-world usecase. If you have similar useful examples demonstrating the use of RxJava, feel free to send in a pull request.

I'm wrapping my head around RxJava too so if you feel there's a better way of doing one of the examples mentioned above, open up an issue explaining how. Even better, send a pull request.

## License

Licensed under the Apache License, Version 2.0 (the "License").
You may obtain a copy of the License at

   http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and
limitations under the License.

You agree that all contributions to this repository, in the form of fixes, pull-requests, new examples etc. follow the above mentioned license.
