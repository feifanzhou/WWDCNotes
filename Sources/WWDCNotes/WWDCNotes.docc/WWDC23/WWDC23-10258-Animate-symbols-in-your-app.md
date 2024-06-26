# Animate symbols in your app

Bring delight to your app with animated symbols. Explore the new Symbols framework, which features a unified API to create and configure symbol effects. Learn how SwiftUI, AppKit, and UIKit make it easy to animate symbols in user interfaces. Discover tips and tricks to seamlessly integrate the new animations alongside other app content. To get the most from this session, we recommend first watching “What’s new in SF Symbols 5.”

@Metadata {
   @TitleHeading("WWDC23")
   @PageKind(sampleCode)
   @CallToAction(url: "https://developer.apple.com/wwdc23/10258", purpose: link, label: "Watch Video (17 min)")

   @Contributors {
      @GitHubUser(srujanc)
   }
}





# Animate symbols in your app

Bring delight to your app with animated symbols. Explore the new Symbols framework, which features a unified API to create and configure symbol effects. Learn how SwiftUI, AppKit, and UIKit make it easy to animate symbols in user interfaces. Discover tips and tricks to seamlessly integrate the new animations alongside other app content. To get the most from this session, we recommend first watching “What's new in SF Symbols 5.”.

> SF Symbols are an iconic part of Apple interfaces..

They look gorgeous in menus, toolbars, sidebars, and more. And because people are familiar with symbols, they make your app more intuitive to use. In iOS 17 and macOS Sonoma, we're enhancing symbols with animation, bringing more life into your apps than ever before..

I recommend checking out the ["What's new in SF Symbols 5"](https://developer.apple.com/videos/play/wwdc2023/10197) session to dive deeper into the animations themselves, including best practices for designing interfaces with them.

## Symbol Effects

In the API, these animations are called "symbol effects," and the new Symbols framework is home to all of them. It's included for free when you use SwiftUI, AppKit, or UIKit to build your app. A really cool feature of the Symbols framework is that each effect has a simple dot-separated name. So to create a bounce effect, you can simply write ".bounce" in your code.

These dot-separated names also extend to the way you configure effects. For example, you can specify that the symbol should bounce upwards or downwards, but most of the time, you won't need to specify anything. The frameworks will automatically use the most appropriate direction. Some effects feature many configuration options. For example, Variable Color has three different settings. By chaining options together, you can configure very specific effects with ease.

The effect names are real Swift code. There's no strings attached. Xcode will autocomplete each part of the name, and if an effect is configured incorrectly, you'll get an error at compile time. The best way to explore all the new animations is the SF Symbols app. In the new animation tab, you can learn about all the available configuration options for each effect. You can even copy a dot-separated effect name to be used directly in your code. With all of the effect types and configuration options, there's a massive variety of animations available. But all of these effects actually encompass a small set of behaviors.

Bounce, for example, plays a one-off animation on the symbol. This is considered discrete behavior. Adding a Scale effect, on the other hand, changes the symbol's scale level and keeps it there indefinitely. Scale is said to support indefinite behavior. Unlike discrete effects, indefinite effects only end when explicitly removed.

Appear and Disappear support transition behavior. They can transition the symbol in and out of view.

And finally, Replace is a content transition. It animates from one symbol to another.

So that's the four different behaviors: discrete, indefinite, transition, and content transition. In the Symbols framework, each behavior corresponds to a protocol. Effects declare their supported behaviors by conforming to these protocols.
Here is a breakdown of all available effects, as well as their supported behaviors. I'll cover this in more detail in this session. Just know that an effect's behavior determines which UI framework APIs can work with them. And speaking of UI framework APIs, let's talk about how to add all of these cool effects in your SwiftUI, UIKit, and AppKit apps.


### Symbol effects in SwiftUI

> In SwiftUI, there is a new view modifier, symbolEffect.

```javascript

// Symbol effects in SwiftUI
Image(systemName: "wifi.router")
    .symbolEffect(.variableColor.iterative.reversing)
    .symbolEffect(.scale.up)

```

Simply add the modifier and pass in the desired effect. Here, I pass in variableColor, and now the symbol is playing the default variable color animation.
It's easy to do this in AppKit and UIKit too. Just use the new addSymbolEffect method on an image view to add a variable color effect. I can configure the variable color effect using the dot syntax. Here, I change the effect to variableColor.iterative.reversing, resulting in a different variable color animation. It's a great way to show that my app is connecting to the network. It's even possible to combine different effects. Here, I add a scale.up effect. Now the symbol is animating variable color while also scaled up.

These APIs provide a simple way to add indefinite effects to symbol images. Recall that indefinite effects change some aspect of a symbol indefinitely, until the effect is removed.

So using the symbolEffect modifier, I can apply a variable color effect, which continuously plays an animation.

### Symbol effects in AppKit and UIKit

> In AppKit and UIKit, use addSymbolEffect and pass in .disappear or .appear.

The takeaway here is that indefinite effects don't change the layout at all. They only alter the rendering of the symbol within the image view.
So that covers the first behavior. How do I jump to the parallel universe, where the surrounding layout changes? This is where the transition behavior comes in. Transition effects can be used with SwiftUI's built-in transition modifier, which animates a view's insertion or removal from the view hierarchy.
Let's convert the previous code to use the transition behavior. Instead of conditionally applying a Disappear effect, I'll instead conditionally add the symbol to the view hierarchy.

> You can also use a unique transition effect called Automatic. This effect will automatically perform the most appropriate transition animation for this symbol.until the effect is removed.

> So using the symbolEffect modifier, I can apply a variable color effect, which continuously plays an animation.

```javascript

// Symbol effects in AppKit and UIKit
let imageView: NSImageView = ...

imageView.addSymbolEffect(.variableColor.iterative.reversing)
imageView.addSymbolEffect(.scale.up)

```

###  Indefinite symbol effects in SwiftUI

But I also need a way to control when the effect is active. I wouldn't want this animation to keep playing after my app successfully connects to the network.

This can be done by adding the boolean isActive parameter. Here, I apply the effect only when connecting to the internet. Once the app finishes connecting, the symbol animation seamlessly ends.

In AppKit and UIKit, use the removeSymbolEffect method to end indefinite effects. What about discrete effects, which perform one-off animations? I mentioned Bounce as an example of this earlier. Your app may trigger Bounce effects in response to certain events.


```javascript

// SwiftUI
struct ContentView: View {
    @State var isConnectingToInternet: Bool = true
    
    var body: some View {
        Image(systemName: "wifi.router")
            .symbolEffect(
                .variableColor.iterative.reversing,
                isActive: isConnectingToInternet
            )
    }
}

// UIKit
let imageView: NSImageView = ...

imageView.addSymbolEffect(.variableColor.iterative.reversing)

// Later, remove the effect
imageView.removeSymbolEffect(ofType: .variableColor)

```

### Discrete Effects

What about Discrete effects, which perform one-off animations? I mentioned Bounce as an example of this earlier. Your app may trigger Bounce effects in response to certain events.

In SwiftUI, I can use the same symbolEffect modifier to add discrete effects. However, I must also provide SwiftUI a value. Whenever the value changes, SwiftUI triggers the discrete effect.

Let's add a button that, when pressed, bounces the symbol. The button's handler simply needs to increment bounceValue. SwiftUI will see the change in bounceValue and trigger the bounce. I can do this in AppKit and UIKit by adding a Bounce effect to the image view. Because Bounce only supports discrete behavior, then adding the effect performs a single bounce. There's no need to remove the effect afterwards.

```javascript

 // Discrete symbol effects in SwiftUI

 struct ContentView: View {
    @State var bounceValue: Int = 0
    
    var body: some View {
        VStack {
            Image(systemName: "antenna.radiowaves.left.and.right")
                .symbolEffect(
                    .bounce,
                    options: .repeat(2),
                    value: bounceValue
                )
            
            Button("Animate") {
                bounceValue += 1
            }
        }
    }
}


// Discrete symbol effects in AppKit and UIKit
let imageView: NSImageView = ...

// Bounce
imageView.addSymbolEffect(.bounce, options: .repeat(2))

```

Now, let's say I don't want the symbol to bounce just once. How about bouncing twice? SwiftUI, AppKit, and UIKit support an options parameter, where I can specify a preferred repeat count. Now, the symbol bounces twice when the effect is triggered. Bounce isn't the only effect which can have discrete behavior. Two of the effects I covered earlier, Pulse and Variable Color, support not only indefinite behavior, but also discrete behavior. In other words, they can play one-off animations, just like Bounce. That means I can take the earlier Bounce example and change it to variableColor. Variable Color switches to use its discrete behavior, since it's applied in a non-repeating fashion.


## Content transition effects

 Next, let's talk about content transition effects. 
 
 The Replace effect, which animates between two different symbol images, is the main example of this. Here, I have an image that switches between a pause symbol and a play symbol.

 SwiftUI has a new contentTransition type called symbolEffect, which can be used with Replace. So if I put the Image in a Button that toggles which symbol is displayed, the change is now animated. In AppKit and UIKit, you can use the new setSymbolImage method to change the image using a symbol content transition.

```javascript
// Content transition symbol effects in SwiftUI
    struct ContentView: View {
        @State var isPaused: Bool = false
    
        var body: some View {
            Button {
               isPaused.toggle()
            } label: {
               Image(systemName: isPaused ? "pause.fill" : "play.fill")
                   .contentTransition(.symbolEffect(.replace.offUp))
            }
        }
    }
```

Finally, we have Appear and Disappear, which can show and hide symbols with unique animations. These effects are uniquely classified as transition effects. But before we get into that, we need to talk about parallel universes. Don't worry, though. It's not as complicated as it seems. In one universe, the image disappears, but the image view is still in the hierarchy. In other words, there's no change to the layout. The square and circle remain the same distance to each other. In the parallel universe, the image view is truly added and removed from the hierarchy. As a result, the layout of surrounding views may change.

The great news is that Appear and Disappear support both behaviors.

The first behavior is possible because Appear and Disappear are indefinite effects.

You know how to use indefinite effects already. In SwiftUI, use the .symbolEffect modifier and pass in .disappear. As the value of isMoonHidden updates, the Disappear effect is applied.

In AppKit and UIKit, use addSymbolEffect and pass in .disappear or .appear.

The takeaway here is that indefinite effects don't change the layout at all. They only alter the rendering of the symbol within the image view.
So that covers the first behavior. How do I jump to the parallel universe, where the surrounding layout changes? This is where the transition behavior comes in. Transition effects can be used with SwiftUI's built-in transition modifier, which animates a view's insertion or removal from the view hierarchy.

```javascript
    // Content transition symbol effects in AppKit and UIKit
        let imageView: UIImageView = ...
        imageView.image = UIImage(systemName: "play.fill")

    // Change the image with a Replace effect
        let pauseImage = UIImage(systemName: "pause.fill")!
        imageView.setSymbolImage(pauseImage, contentTransition: .replace.offUp)

```

## variable value.

iOS 16 and macOS Ventura introduced variable value as another dimension for symbols, representing concepts like volume levels and signal strengths.

In iOS 17 and macOS Sonoma, we are making it super easy to crossfade between arbitrary variable values.

In SwiftUI, you don't need to do anything at all. Here, I have a Wi-Fi symbol whose variable value is based on some state– in this case, the current signal strength. As the signal strength changes, the Wi-Fi symbol automatically updates, while also animating across variable values. In AppKit and UIKit, use the automatic symbol content transition. It detects if the new symbol image just has a different variable value, and, if so, crossfades to the new value.

```javascript

    // Variable value animations in SwiftUI
        struct ContentView: View {
            @State var signalLevel: Double = 0.5
    
            var body: some View {
               Image(systemName: "wifi", variableValue: signalLevel)
            }
        }

    // Variable value animations in AppKit and UIKit
        
        let imageView: UIImageView = ...
        imageView.image = UIImage(systemName: "wifi", variableValue: 1.0)

    // Animate to a different Wi-Fi level
        let currentSignalImage = UIImage(
            systemName: "wifi",
            variableValue: signalLevel
        )!
        imageView.setSymbolImage(currentSignalImage, contentTransition: .automatic)

```


Thanks so much. There's a lot of ways to animate symbols, so use the SF Symbols app to discover what's possible. Explore the Symbols framework, and try the new symbol effect APIs in SwiftUI, AppKit, and UIKit. And finally, adopt the animations to make your app's interface more delightful than ever.

Check out the other symbols sessions, too, for Human Interface guidelines on symbol animation, as well as updating custom symbols to support all the effects. 

[Create animated symbols](https://developer.apple.com/videos/play/wwdc2023/10257)

Thanks.
