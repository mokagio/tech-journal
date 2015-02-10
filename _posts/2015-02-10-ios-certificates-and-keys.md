---
layout: post
title: On the Certificates and keys for iOS development
date: 2015-02-10 18:59:42
---

_**Disclaimer** what said in this post is my current understanding of the involed processes and entities. It is certainly incomplete, and it is totally possible that this is partially or entirely wrong too. If that's the case I'd be incredibly thankful to start a coversation about it. Please tweet me [@mokagio](https://twitter.com/mokagio)._

Ask any iOS or Mac developer and they'll all agree on one thing. Dealing with code signing is the hardest part of a developer on the Apple platform stack.

It never works the first time, ever. Every time I approach it and think to myself "this is like the 1000th time you do this, you know what you're doing and everything will be smooth" something goes wrong.

My latest issue was due to my ignorance though, rather than to the stack being crap.

Last week I had to change Mac, and setup a new development environment. Everything worked out fine thanks to my [dotfiles](https://github.com/mokagio/dotfiles), but once I tried to run an app on a device with Xcode this happened:

```
CodeSign error: code signing is required for product type 'Application' in SDK 'iOS 8.1'
```

I'll skip the half an hour of head banging against the standing desk and italian swearing, and jump straight to the cause of the issue. The app couldn't run on my device because Xcode coudln't sign the app, and the reason was that **no private key was present in the keychain**. Thanks for not telling me from the start...

Here's a quick, simplified, scheme of how the signing works:

* An application needs to be signed to run on a device, even if in Development/Debug mode.
* To sign an application Xcode uses a Provisioning Profile.
* The Provisioning Profile needs to point to that specific application. This means it's App ID value needs to be the same as the application Bundle Identifier. They of course have different names, because otherwise our life would be too easy.
* Xcode needs to be able to verify that the machine/person asking to run (and therefore sign) the app is entitled to do it. It does so using the Certificate embedded in the Provisioning Profile.
* A Certificate can be use to sign an app only if the private key used to create it is present in the keychain.

There you go. Because the laptop was a new one the private key I used to create the Certificate, embedded in the Provisioning Profile, for that specific App ID/Bundle Identifier, wasn't in the keychain, and the whole process failed.

Once the key had been added everything went fine. Happy days.

### Food for thoughts

* First of all do further research to make sure what said here is true.
* What exactly is a Certificate, `.pem`?
* What excatly is the private key used for the Certificate,`.p12`?
* How does the `.p12` key differ from an ssh key?

