Why Google refunded 200$ because I missed 5 lines of code
=========================================================
2019-06-13

So this is going to be a quick one. I am the maker of [LessPhone](https://play.google.com/store/apps/details?id=me.aswinmohan.nophone&hl=en_IN), it's an Android Launcher to tone down your phone use. It has one in app purchase to unlock the dark theme and the ability to set the number of custom apps on the screen and costs about a dollar. It has been running for around 10 months and averages around 700-800$ revenue each month.

I ported the App from react-native to Kotlin to Flutter to Kotlin with Android Jetpack. The app was in a pretty great usable condition when it was written in Flutter, it was fast and slick and beautiful. Still the APK was around 6MB and it was on the heavy side considering a launcher like this. So I decided to port the launcher to Kotlin and after around 40Hrs learning the Jetpack library and coding I was succesful in porting LessPhone to Kotlin. There were bugs in the initial versions but everything was ironed out. 

With the new version purchases where happening daily revenue was growing and it was good, also the App size got reduced to 1.36MB. I was happy the users were happy and everything was going well. Then LessPhone got covered on mrwhosetheboss youtube channel. It was the icing on the cake. Revenue and downloads shooted up like a lit christmas tree. It couldn't become better than this, except it did.

After three days from the day mrwhosetheboss covered the App, the revenue hit an all time low, around -57\$, yeah negative revenue. I couldn't understand what was happening, and it was only days after the global outage of Google. I thought it was a bug with the dashboard(yeah smart me :facepalm:) and decided to wait it out. The next day -28\$, the next day -12\$. The number of purchases were steady, there was money coming in but the daily revenue was going negative. Finally reality kicked me in the face and I contacted google support. They responded within two days with one of the best lessons in my limited time with Android Development.

>Google Play supports purchasing products from inside of your app (in-app) or outside of your app (out-of-app). In order for Google Play to ensure a consistent purchase experience regardless of where the user purchases your product, you must acknowledge all purchases received through the Google Play Billing Library as soon as possible after granting entitlement to the user. If you do not acknowledge a purchase within three days, the user automatically receives a refund, and Google Play revokes the purchase. For pending transactions (new in version 2.0), the three-day window starts when the purchase has moved to the SUCCESS state and does not apply while the purchase is in a PENDING state.

Yep that was sitting right there in the documentation, and since I wanted LessPhone to use the latest libraries on the planet I was affected by it. Without acknowledging the purchase the money was refunded back to the people who purchased it. And the best thing was the dark theme would be activated even if the user got the money refunded.

I have fixed the issue and the update is already live on the store. But since there is a time delay between when the user would update the App and open it, the cost would be much higher than 200\$. All that was needed was these five lines of code.

``` kotlin
  val acknowledgePurchaseResponseListener = AcknowledgePurchaseResponseListener {  }
  val acknowledgePurchaseParams = AcknowledgePurchaseParams.newBuilder()
      .setPurchaseToken(purchase.purchaseToken)
      .build()

  billingClient.acknowledgePurchase(acknowledgePurchaseParams, acknowledgePurchaseResponseListener)
```

But this has not been a bad or unhappy experience for me. Even though the money was lost I kind of loved the experience of this all. These kinds of incidents make us wonder the real cost of Code and the more real cost of messing up, and for that I am grateful. There is also a better lesson with this, always read the docs they are written for a reason. So next time you are implementing IN-App purchases in your Android App make sure you acknowledge them.

Until next time :D

P.S : Some of my users tried to bring this issue to my notice by emailing me multiple times, they are the reason I investigated sooner. Thanks people you rock !
