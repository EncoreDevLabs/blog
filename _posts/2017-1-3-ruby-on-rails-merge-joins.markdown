---
layout: post
title:  "Merging Queries in Rails"
date:   2017-1-3 9:01:00
categories: feature rails activerecord merge
image: /assets/article_images/2017-1-3/merge.jpg
image2: /assets/article_images/2017-1-3/merge-mobile.jpg
---

Have you tried to construct a complex query in Rails to do joins and order by different columns and have fallen into writing SQL because you could not construct a query using ActiveRecord's querying DSL, then this blog post might help.

Rails provides a way to merge complex queries using a method called `merge` and it works like this.

To understand what kind of queries we can create lets consider few models: `Person`, `Location`, `Region`

These models are associated in the follow way. A Person has `belongs_to :location`, a Location `has_many :people` and `belongs_to :region` and a Region `has_many :locations`. Person, Location and Region also have a `name` field.

Lets say we want to get all the people that are in a specific Region given a region name. This requires us to do an INNER and a LEFT JOIN across three tables and the query to do so looks like this:

{% highlight sql %}
SELECT "people".* FROM "people"
INNER JOIN "locations" ON "locations"."id" = "people"."location_id"
LEFT OUTER JOIN "regions" ON "regions"."id" = "locations"."region_id"
WHERE "regions"."name" = 'Asia'
{% endhighlight %}

To construct such a query you can split the problem into smaller queries and merge them using `merge`:

Here is the code:

{% highlight ruby %}
class Person < ActiveRecord::Base
  belongs_to :location

  def self.in_region(region)
    joins(:location).merge(Location.in_region(region))
  end
end

class Location < ActiveRecord::Base
  belongs_to :region
  has_many :people

  def self.in_region(region)
    joins(:region).where(regions: { name: region })
  end
end

class Region < ActiveRecord::Base
  has_many :locations
end
{% endhighlight %}

If you look at the problem now it is split in such a way that Location now has a method called `in_region` which returns all Locations within a Region. Using that we can `merge` this query with the query for finding people within locations that are within a region:

{% highlight ruby %}
def self.in_region(region)
  joins(:location).merge(Location.in_region(region))
end
{% endhighlight %}

This opens up the possibility to do even more advance queries while using ActiveRecord's DSL. It introduces smaller queries that can be reused by your Rails app, makes your code more cohesive by having Location class take care of the responsibility of finding Locations within a region and also makes the code easy to test by introducing smaller queries that can be tested instead of constructing a big monolothic query.
