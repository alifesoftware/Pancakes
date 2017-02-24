Project Status
----

There are a million other libraries for this use case, and I have no reason to believe this one is the best. This started as a fun experiment and got a bunch of github stars after it got shared through various channels. It works and you're free to use it, but I have no plans for maintaining or improving it.

Pancakes
----

Pancakes is a View-based navigation stack library for Android. While it does aim to replace **some** of FragmentManager's functionality, its feature set is not one-to-one with that of FragmentManager, as Views and Fragments are inherently different. Unlike FragmentManager, it also supports the use of Animators for View transitions.

Download
----

```java
compile 'me.mattlogan.pancakes:pancakes:4.0.0'
```

Usage
-----

Create a `ViewStack` instance with a `ViewGroup` container and a `ViewStackDelegate`:

```java
ViewStack viewStack = ViewStack.create(container, delegate);
```

Add a `View` to your container by pushing a layout id:

```java
viewStack.push(R.layout.red_view);
```

Call `pop()` to go back one `View`:

```java
viewStack.pop();
```

Or, use an `AnimatorFactory` along with `pushWithAnimation(int, AnimatorFactory)` and `popWithAnimation(AnimatorFactory)` to add remove a `View` with a transition animation.

```java
public class CircularReveal implements AnimatorFactory {
    @Override
    public Animator createAnimator(View view) {
        int cx = view.getWidth() / 2;
        int cy = view.getHeight() / 2;

        int finalRadius = Math.max(view.getWidth(), view.getHeight());

        return ViewAnimationUtils.createCircularReveal(view, cx, cy, 0, finalRadius);
    }
}
```

You can also call `peek()` to get the `View` at the top of the navigation stack.

Add a `StackChangedListener` (or several) if you want to be notified of changes in the navigation stack:

```java
viewStack.addStackChangedListener(listener);
```

You can also remove individual listeners with `removeStackChangedListener(StackChangedListener)` or remove all of them with `clearStackChangedListeners()`.

Persist your navigation stack across configuration changes:

```java
@Override
public void onSaveInstanceState(Bundle outState) {
    viewStack.saveToBundle(outState, STACK_TAG);
    super.onSaveInstanceState(outState);
}
```

Rebuild the stack from a `Bundle`:
```java
viewStack.rebuildFromBundle(savedInstanceState, STACK_TAG);
```

Additionally, you may use `View.onSaveInstanceState(Bundle)` and `View.onRestoreInstanceState(Bundle)` to save the state of any `View` in the navigation stack so long as it has an ID.

Finally, implement `ViewStackDelegate.finishStack()` to take appropriate action when the stack is finished:
```java
@Override
public void finishStack() {
    finish();
}
```

See the [sample app](https://github.com/mattlogan/Pancakes/tree/master/app) for an example implementation.

Tests
----

Unit tests located in [/library/src/androidTest/](https://github.com/mattlogan/Pancakes/blob/master/library/src/androidTest/java/me/mattlogan/library/ViewStackTest.java)

License
-----

```
The MIT License (MIT)

Copyright (c) 2015 Matthew Logan

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.
```
