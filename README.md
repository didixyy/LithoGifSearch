# Search GIFs with Litho and Giphy

Facebook recently open sourced [Litho](http://fblitho.com/) - A declarative UI framework for Android. From what I have
heard, Litho is inspired by React. React has concept of components so that it can be easily reused and
rendered (with jsx). To follow that I have made some components.

Here's an elaborated blogpost series about exploring Litho - **[Android GIF search engine with Litho](http://www.jayrambhia.com/blog/android-litho-gifs)**

![Screenshot](https://raw.githubusercontent.com/jayrambhia/LithoGifSearch/master/art/demo1.jpg)

## Components

In Litho, component describes a set of views or components put together to get something meaningful. Litho generates code for the component based on the spec that you write.
There are two different types of component specs available.

 - LayoutSpec: Logical equivalent of a composite view on Android.
 - MountSpec: A component that can render views or drawables. We can think of it as a custom view / viewgroup.

### LayoutSpec
We need to annotate the class with `@LayoutSpec` and provide a static method annotated with `@OnCreateLayout` which returns `ComponentLayout` and we need to
define the layout and return it.

### MountSpec
We need to annotate the class with `@MountSpec` and provide following methods.

 - static method annotated with `@OnMeasure`. We get a parameter `Size` in your method call and we need to, basically, update its value for measurement.
 - static method annotated with `@OnCreateMountContent` which return a `View` or a `Drawable`. This is where we create your drawable or view.
 - static method annotated with `@OnMount`. We get the reference to the object that we returned in `@OnCreateMountContent`. We do our stuff here. eg. set bitmap to ImageView, etc.

This app has 4 components.

### HomeComponent
This component is generated by `HomeComponentSpec`. It includes an EditText and a Recyler as stacked vertically. We listen to changes in EditText content and make API calls to get the GIFs. Once we
get the GIFs, we update RecyclerBinder.

### GifItemView
This component is generated by `GifItemViewSpec`. It includes `GifImageView` component and an Image component to display favorite/unfavorite icon.

### GifImageView
This component is generated by `GifImageViewSpec`. It's a `MountSpec` because we need to use `ImageView` to display GIFs using Glide.

### FullScreenComponent
This component is generated by `FullScreenComponentSpec`. It includes `GifImageView` component and an Image component to display favorite/unfavorite icon.

You can read more about components used in this project - [Android GIF search engine with Litho](http://www.jayrambhia.com/blog/android-litho-gifs). You can find the code here - [v1](https://github.com/jayrambhia/LithoGifSearch/tree/v1).
Please ignore the README file in this tag.

## Props

Litho supports properties which we can pass to components so that they display what we want. We need to annotate the parameter with `@Prop`. There are optional properties also available. We can define
them with `@Prop(optional = true)` annotation. If properties are not optional, we need to pass non null value to the builder to avoid Runtime exception. Props are immutable and once passed to a component,
can't be changed.

## State

To update how and what the component renders, Litho provides State. eg. Change like button drawable when the user clicks on it. To make the value as State, we need to annotate it with `@State`. We can have multiple
values defining a state. We should take care of following things when working with State.

 - Provide a static method annotated with `@OnCreateInitialState`. This is important so that we have some initial state when the component is first rendered.
 - Provide a static method annotated with `@OnUpdateState`. This is the method invoked internally by Litho when we want to update the state.

You can read more about state used in this project - [Managing State in Litho](http://www.jayrambhia.com/blog/android-litho-state). You can find the code here - [v2](https://github.com/jayrambhia/LithoGifSearch/tree/v2).
Please ignore the README file in this tag.

## Navigation
To use Litho efficiently, it provides `LithoView` which we set as the root view of our Activity. We can set the component to LithoView. If we want to navigate to another component, we can just set it in our root LithoView.
In the app, when the user clicks on a GIF (GifItemView component), we send a callback to MainActivity to show full screen. It creates a `FullScreenComponent` with `GifItem` passed in the callback and updates the root LithoView
by setting the FullScreenComponent. When the user clicks back, we check if the user is in full screen mode. If yes, we set HomeComponent to root LithoView. We do not require a new activity or fragment for navigation.

You can read more about the navigation - [Navigation with Litho](http://www.jayrambhia.com/blog/android-litho-navigation). You can find the code here - [v4-navigation](https://github.com/jayrambhia/LithoGifSearch/tree/v4-navigation). Please ignore the README file in this tag.

![Screenshot](https://raw.githubusercontent.com/jayrambhia/LithoGifSearch/master/art/demo2.jpg)

## Events

Litho provides a general-purpose API to connect components through events. To have an event, we need to create a POJO and annotate it with `@Event`. For the component to dispatch that event,
we need to annotate it like - `@LayoutSpec(events = { MyEvent.class })` or `@MountSpec(events = { MyEvent.class })`. We can specify multiple events. Litho will generate the required code for us.
Each Event is associated with an `EventHandler` and we need to pass an `EventHandler` to the component builder. Events are also used as callbacks to handle events dispatched by other components.

You can read more about events - [Events with Litho](http://www.jayrambhia.com/blog/android-litho-events). You can find the code here - [v5-events](https://github.com/jayrambhia/LithoGifSearch/tree/v5-events). Please ignore the README file in this tag.

