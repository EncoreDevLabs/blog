---
layout: post
title:  "Adding Promocodes to your Rails App"
date:   2016-12-31 10:48:00
categories: feature rails
image: /assets/article_images/2016-12-31-promocodes/code.png
image2: /assets/article_images/2016-12-31-promocodes/code.png
---

Recently I came across a problem of how to add Promocodes to an ecommerce site built using Rails. Thinking about the problem I thought of good programming practices that I can apply so that the code is clean and maintainable in the future as more different types of promocodes are added to this system. The following are the list of requirements for which I had to build a solution:

1. Promocodes support both percent off (10% off) or a fixed amount off ($10 off) the purchase
2. The other requirement was that more than one Promocode can be associated with one promotion. For example 10% off has a promocode of 10OFF or TENOFF which both result in 10% off
3. The promocodes also have a start and end date to them so that they cannot be used before or after a certain period in time.

Considering this and maybe different types of discounts that can be given in the future I had to make a model that would be able to calculate the discount value given a price. So this is how I implemented it [https://github.com/ankurp/Promocodes](https://github.com/ankurp/Promocodes)


## Models

Considering we have an Order model I created a `Promocode` model whose primary key is a string which is the actual promocode (ex: `10OFF` or `FIFTYOFF`) since the promocode itself will be unique identifier to figure out which promotion we are talking about.

The `Promocode` model also is a polymorphic association with `promotion` which gives us a model of either type `PercentOffPromo` or `FixedPriceOffPromo`. Both of these models contain within each the logic to figure out how to calculate the discount without having one class take care of this responsibility with giant if else conditions if the number of types of promotions increases in the future. Different types of promotions that could be offered in future may include certain percent off on an order of $100 or more, Free Shipping, etc.

Having Promotions be subclassed also makes it easier to test using rspec and makes the code highly cohesive and reduces coupling between orders and promotions.

## Sourcecode

[PromoCodes](https://github.com/ankurp/PromoCodes)
