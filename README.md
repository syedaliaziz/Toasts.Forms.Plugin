Toasts Plugin for Xamarin.Forms
===================

A simple way of showing some notifications inside your Xamarin.Forms application. In windows phone world we call them "Toasts".

#### Android
Android implementation is based on https://github.com/keyboardsurfer/Crouton with several changes and ported to C# without bindings.

![Alt text](http://habrastorage.org/files/b72/3c4/362/b723c436271941309939da500f1e2abb.gif)

#### iOS
iOS implementation is based on https://github.com/terryworona/TWMessageBarManager with several changes (thanks to prashantvc with his https://github.com/prashantvc/Xamarin.iOS-MessageBar)

![Alt text](http://habrastorage.org/files/d1e/dd7/cbd/d1edd7cbdfe141cfb8f7be36f692b1a1.gif)

#### WP8
Unlike others WP8's version supports multiply toasts (can be limited to 1).

![Alt text](http://habrastorage.org/files/e96/4fd/8c5/e964fd8c5cb14ad08d4dab3cb6f36e73.gif)

Setup and usage
===================
#### Setup
* Available on NuGet: https://www.nuget.org/packages/Toasts.Forms.Plugin
* Install into your PCL project and Client projects.

In your iOS, Android, and Windows Phone projects please call:

```csharp
Xamarin.Forms.Init();//platform specific init
DependencyService.Register<ToastNotificatorImplementation>(); // Register your dependency
ToastNotificatorImplementation.Init(); //you can pass additional parameters here
```

You must do this AFTER you call Xamarin.Forms.Init();
By the way, toasts on each platform support customization - you can pass your own style (custom renderer) as an argument in Init() method.

#### Usage
Use dependency service in order to resolve IToastNotificator
```csharp
var notificator = DependencyService.Get<IToastNotificator>();
bool tapped = await notificator.Notify(ToastNotificationType.Error, 
	"Error", "Something went wrong", TimeSpan.FromSeconds(2));
```
#### Customization
On all three platforms you can completely override toast UI. However, there is also an easy way to add a new status with custom icon and background, let's take a look on a Windows Phone example. In this example we want to add a few more types of messages with custom icons. Our code in PCL will look like this:
```csharp
var notificator = DependencyService.Get<IToastNotificator>();
bool tapped = await notificator.Notify(
	type: ToastNotificationType.Custom, 
	title: "Level up!", 
	description: "Congratulations!", 
	duration: TimeSpan.FromSeconds(2), 
	context: MyCustomTypes.LevelUp);
```			
So we have just set the type to Custom and passed an additional argument to Object context (last argument) - our custom enum MyCustomTypes.LevelUp.
Now we have to configure default renderer (or replace it by your own if you want to add more changes to the layout):
```csharp
Xamarin.Forms.Forms.Init();
ToastNotificatorImplementation.Init(stackSize: 2, 
	customRenderer: new DefaultToastLayoutRenderer(
		context =>
		{
			switch ((MyCustomTypes)context)
			{
				case MyCustomTypes.LevelUp:
					return new BitmapImage(new Uri("level_up.png", UriKind.Relative));
				...
			}
		}, 
		context => new SolidColorBrush(Colors.Magenta)));
```
iOS and Android api also have similar extensibility

#### Contributors
* [EgorBo](https://github.com/EgorBo)
* [AdamPed](https://github.com/AdamPed)

Thanks!

#### License
Licensed under MIT
