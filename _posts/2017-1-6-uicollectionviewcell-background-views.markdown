---
layout: post
title:  "backgroundView and selectedBackgroundView in UICollectionViewCell"
date:   2017-1-6 4:42:00
categories: ios uicollectionview
image: /assets/article_images/2017-1-6/tiles.jpg
image2: /assets/article_images/2017-1-6/tiles-mobile.jpg
---

Just wanted to share an interesting aspect I discovered in UICollectionViewCell that makes it very easy to build a game like matching pairs or similar in iOS while writing very minimal code to handle selection and changing the UICollectionView cell on selection and deselection.

UICollectionViewCell comes with a `contentView` which sits on top of `backgroundView` and `selectedBackgroundView` When you have the cell in UICollectionView as selectable then selecting a cell changes the `backgroundView` of the cell to the `selectedBackgroundView` and vice versa making it very easy for you to change a the background color of a cell on selection and deselection without messing with the background color of the `contentView` or changing the `contentView` in anyway.

Lets see how we can write the code for this. First lets start out by creating a custom Collection View Cell as such:

{% highlight objc %}
#import "CollectionViewCell.h"

@implementation CollectionViewCell

- (void)awakeFromNib {
    [super awakeFromNib];
    
    UILabel *label = [[UILabel alloc] initWithFrame:self.bounds];
    [label setTextAlignment:NSTextAlignmentCenter];
    [self.contentView addSubview:label];
    
    self.backgroundView = [[UIView alloc] initWithFrame:self.bounds];
    self.backgroundView.backgroundColor = [UIColor lightGrayColor];
    
    self.selectedBackgroundView = [[UIView alloc] initWithFrame:self.bounds];
    self.selectedBackgroundView.backgroundColor = [UIColor redColor];
}

@end
{% endhighlight %}

In the code above we set the cell's `contentView` to have a `UILabel`, `backgroundView` of the cell to be new UIView with a background color of light gray and we assigned `selectedBackgroundView` a UIView of red color. Both the `backgroundView` and `selectedBackgroundView` are `nil` by default but now setting them allows us to change the cell background color on selecting and deselecting the cell.

This helps you avoid adding a lot of logic in your CollectionViewControllers to figure out whether a cell is selected or not and change it on selection. Our CollectionViewController has very little code to handle changing of the background on selection of a cell as seen below:

{% highlight objc %}
#import "CollectionViewController.h"
#import "CollectionViewCell.h"

@implementation CollectionViewController

- (NSInteger)numberOfSectionsInCollectionView:(UICollectionView *)collectionView {
    return 1;
}

- (NSInteger)collectionView:(UICollectionView *)collectionView numberOfItemsInSection:(NSInteger)section {
    return 50;
}

- (UICollectionViewCell *)collectionView:(UICollectionView *)collectionView cellForItemAtIndexPath:(NSIndexPath *)indexPath {
    CollectionViewCell *cell = [collectionView dequeueReusableCellWithReuseIdentifier:@"Cell" forIndexPath:indexPath];
    
    UILabel *label = cell.contentView.subviews[0];
    [label setText:[NSString stringWithFormat:@"%li", indexPath.row]];

    return cell;
}

@end
{% endhighlight %}

In our CollectionViewController all we do is specify the number of sections and number of cells in each section and modify the cells label to print the cell's index to show how the `contentView`, which remains on the top of these two views, does not get affected on selection and deselection.

This opens up the possibility to have the Collection View manage the state of some parts of your application in regards to hiding and showing certain views in the cell on selection and unselection and you can also have multiple selection enabled on UICollectionView to allow for multiple cells to be selected at a time.

Source code for this is uploaded on [here on Github](https://github.com/ankurp/CellSelection) if you want to run the app locally and see how it works.
