---
layout: post
title:  "Easy Decorating in Ruby with SimpleDelegator"
date:   2017-1-20 10:34:00
categories: feature ruby decorator simpledelegator
image: /assets/article_images/2017-1-20/decorate.jpg
image2: /assets/article_images/2017-1-20/decorate-mobile.jpg
---

Decorating a class is another way of extending the functionality of a class instead of subclassing. In Ruby there is an elegant way to decorate a class using [SimpleDelegator](https://ruby-doc.org/stdlib-2.2.1/libdoc/delegate/rdoc/SimpleDelegator.html).

Lets say you have a `Coffee` class and want to decorate the class by adding a `Milk` and `Sugar` version of it without subclassing, then you can do so as such in Ruby:

{% highlight ruby %}
class Coffee
  def get_cost
    2.0
  end

  def ingredients
    "Coffee"
  end
end

class Milk < SimpleDelegator
  def ingredients
    super + ",Milk"
  end
end

class Sugar < SimpleDelegator
  def ingredients
    super + ",Sugar"
  end
end

plain_coffee = Coffee.new
milk_coffee = Milk.new(plain_coffee)
milk_and_sugar_coffee = Sugar.new(milk_coffee)

puts plain_coffee.ingredients
# => Coffee
puts milk_coffee.ingredients
# => Coffee,Milk
puts milk_and_sugar_coffee.ingredients
# => Coffee,Milk,Sugar
{% endhighlight %}

In the example above, you have `Coffee` class which returns `Coffee` for `ingredients` but when you decorate the `plain_coffee` object with `Milk` decorator then it returns `Coffee,Milk` for `ingredients`. And when we decorate the `milk_coffee` object with `Sugar` then it returns `Coffee,Milk,Sugar` for `ingredients`.

I found decorating objects very easy in Ruby due to `SimpleDelegator` as it delegates all methods to the object being decorated that is passed in the contrustor of the decorator. It is able to do so using `method_missing` available in Ruby which `SimpleDelegator` uses to invoke the same method on the object being decorated. Compare this with writing a similar decorator in language like Swift or Java where you need to write a Decorator class and then subclass the decorator class for Milk and Sugar coffee:

{% highlight swift %}
protocol Coffee {
    func getIngredients() -> String;
}

class SimpleCoffee : Coffee {
    func getIngredients() -> String {
        return "Coffee"
    }
}

class CoffeeDecorator : Coffee {
    private let decoratedCoffee: Coffee

    required init(decoratedCoffee: Coffee) {
        self.decoratedCoffee = decoratedCoffee
    }

    func getIngredients() -> String {
        return decoratedCoffee.getIngredients()
    }
}

final class Milk: CoffeeDecorator {
    override func getIngredients() -> String {
        return super.getIngredients() + ",Milk"
    }
}

final class Sugar: CoffeeDecorator {
    override func getIngredients() -> String {
        return super.getIngredients() + ",Sugar"
    }
}

let plainCoffee = SimpleCoffee()
print(plainCoffee.getIngredients())
let milkCoffee = Milk(decoratedCoffee: plainCoffee)
print(milkCoffee.getIngredients())
let milkSugarCoffee = Sugar(decoratedCoffee: milkCoffee)
print(milkSugarCoffee.getIngredients())
{% endhighlight %}
