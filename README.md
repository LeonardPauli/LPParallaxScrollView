MYParallaxScrollView
====================

Parallax scrolling in iOS made easy!!


##Setup the project

- Open Xcode, create new project (iOS: Single View Application).
- Select MYViewController.xib and delete it (Move to trash)
- In MYAppDelegate.m, change

		self.viewController = [[MYViewController alloc] initWithNibName:@"MYViewController" bundle:nil];

	to

		self.viewController = [[MYViewController alloc] init];

- Download this repo as zip, drag the folder MYParallaxScrollView into the project (sidebar)
- Check "Copy items.." and "Create Groups..", hit return.
- Click your project name in the sidebar
- Make sure that portrait only is selected for now


MYViewController.m:

- At top:

		#import "MYParallaxScrollView.h"

- Before viewDidLoad:

		MYParallaxScrollView *autumnboard;

- Inside viewDidLoad:

		self.view = [[UIView alloc] init];
		self.view.backgroundColor = [UIColor whiteColor];
		self.view.autoresizingMask = UIViewAutoresizingFlexibleHeight | UIViewAutoresizingFlexibleWidth;
		
		// For iPhone 4 and 5 portrait
		CGSize size = [[UIScreen mainScreen] bounds].size;
		CGFloat width = MIN(size.width, size.height);
		CGFloat height = MAX(size.width, size.height)-20;

- And now, initiate the scrollview! (Still in viewDidLoad)

		autumnboard = [[MYParallaxScrollView alloc]
			initInView:self.view
			frame:CGRectMake(0,0,width,height)
			// Optional, pass nil if you don't want a scaled-to-fit parallax bg-image
			backgroundImage:[UIImage imageNamed:@"autumnboard-bg.png"]];
		autumnboard.backgroundColor = [UIColor whiteColor];
		autumnboard.delegate = self;
		autumnboard.numberOfPages = 3;
		[autumnboard setPagingEnabled:YES];
		[self.view addSubview:autumnboard.pageControl];

- Some other candies!

		// Set this delegate if you would like to...
		@property (retain) id parallaxDelegate;
		@protocol MYParallaxScrollViewDelegate <NSObject>
		@optional
		// ...do the parallax yourself! Just position your view in there!
		- (void)willLayoutSubviewsInScrollView:(MYParallaxScrollView*)scrollView;
		@end

		@property (nonatomic, readonly) UIImageView *backgroundImageView;
		@property (nonatomic) UIImage *backgroundImage;
		@property (nonatomic) int backgroundImageViewOffset;
		@property (nonatomic) CGFloat backgroundImageViewOffsetFactor;
		
		@property (nonatomic, readonly) UIPageControl *pageControl;
		
		// Short for content size
		- (CGFloat)cwidth;
		- (CGFloat)cheight;
		
		- (int)numberOfPages;
		- (int)currentPageIndex;
		- (void)setNumberOfPages:(int)nr;
		
		- (CGFloat)backgroundImageViewWidth;
		- (CGFloat)backgroundImageViewOriginXFromScrollX:(CGFloat)scrollX;
		- (CGRect)backgroundImageViewFrame;
		
		// Instead of the delegate, you could use a block!!!
		- (void)setUseWillLayoutSubviewsBlock:(BOOL)useIt;
		- (void)setWillLayoutSubviewsBlock:(void (^)(CGPoint offset, MYParallaxScrollView *scrollView))block;
		
		// These ones will do [inView addSubview:self] them self
		- (id)initInView:(UIView*)inView frame:(CGRect)frame backgroundImage:(UIImage*)bgImg;
		- (id)initInView:(UIView*)inView frame:(CGRect)frame willLayoutSubviewsBlock:
			(void (^)(CGPoint offset, MYParallaxScrollView *scrollView))block;
		

 - Run and enjoy! :)



##More

I've also included UIView+myExt.h/m that has some nice stuff in it. If you import it to a file, you will be able to use:

- view.x = 10; // Short for view.frame.origin.x, settable!!
- view.y
- view.width
- view.height
- view.frameWithoutOrigin // CGRectMake(0, 0, self.width, self.height);
- view.frameWithoutOrigin = rect; // Will just change width and height
- - (UIImage*)captureViewAsImage // YES! You will ba able to take a retina/non-retina snapshoot of the view in less than one line!
- view.forwardsTouches = NO; // Moahaha! The subviews will no longer feel touches!! (Usable for like image overlays)
- - (void)setPointInsideBlock:(BOOL (^)(CGPoint point, UIEvent *event))pointInside; // Easiest way to make non-rectangle touchable buttons!


The project is licenced under the MIT Licence included in this repo. If you plan to make an app of it, i would be glad if you told me about it!
This text was written 31 july 2013, but the code was originally made by me 28 april, 2013.


Happy coding!

// Leonard Pauli