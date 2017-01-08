---
layout: post
title:  "Building tvOS App and things I learnt"
date:   2017-1-8 10:15:00
categories: tvos ios uicollectionview
image: /assets/article_images/2017-1-8/tvos-game.jpg
image2: /assets/article_images/2017-1-8/tvos-game-mobile.jpg
---

I took a dive into building a tvOS game called Matching Pairs. This game is same as the iOS game but there were some subtle differences that I want to bring up that may help others out if they decide to build a game for tvOS.

Just like iOS, tvOS apps uses UIKit and you can share almost all of the code between iOS and tvOS Apps including the View Controllers, Views, Cell, etc. Since I had already built an iOS game app I decided to build a tvOS target. Xcode create some sample view controllers and Storyboard which I quickly modified. I got rid of the sample View Controller and made a new one which inherits from the view controller of iOS so I can modify some of the behavior of my iOS UICollecitonView Cell so I can add the nice hover effect provided by tvOS on cells that have an UIImageView.

In the Xcode project to reuse the iOS code I had to check the `Target Memebership` to include the tvOS App target for the source code files I wanted to share. After that I was able to get the tvOS App working in a matter of minutes without having to write all of the logic for the game, transitions between view controllers and logic to handle touch on cells, etc. as I could share all of that between both apps.

Apple has made it very easy for you to share code between iOS and tvOS which encourages developers to build for both targets. I had to subclass at most two classes (one UICollection Header View, and one UICollectionView controller) but was able to share all of my other code across both iOS and tvOS app.

The game does feel as if it was designed for tvOS thanks to the parallax effect provided for free on UIImageView which gets enabled when you set the `adjustsImageWhenAncestorFocused` property on the UIImageView to `YES`. I had to use the `#if TARGET_OS_TV` preprocessor to set the `adjustsImageWhenAncestorFocused` only for tvOS target and not for iOS.

Below is how the app looks and works on tvOS platform

![tvOS Game App](/assets/article_images/2017-1-8/game.gif)
