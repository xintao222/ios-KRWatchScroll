## Supports

KRWatchScroll can easy watching UIScrollView is scrolling to top or bottom when it stopped, and KRWatchScroll can judge the UITableView scrolling to top or bottom.

## How To Get Started

This sample is using with UITableView.

``` objective-c
#import "KRWatchScroll.h"

@interface ViewController ()<KRWatchScrollDelegate, UITableViewDataSource, UITableViewDelegate>

@property (nonatomic, strong) KRWatchScroll *_krWatchScroll;
@property (nonatomic, strong) NSMutableArray *_datas;
@property (nonatomic, weak) IBOutlet UITableView *outTableView;

@end

@implementation ViewController

- (void)viewDidLoad
{
    [super viewDidLoad];
    _krWatchScroll = [[KRWatchScroll alloc] init];
    _datas         = [[NSMutableArray alloc] initWithCapacity:0];
    for( int i=1; i<=20; ++i )
    {
        [_datas addObject:[NSString stringWithFormat:@"%i", i]];
    }
    self.outTableView.dataSource = self;
    self.outTableView.delegate   = self;
}

#pragma UITableView Delegate
-(NSInteger)numberOfSectionsInTableView:(UITableView *)tableView
{
    return 1;
}

-(NSInteger)tableView:(UITableView *)tableView numberOfRowsInSection:(NSInteger)section
{
    return [self._datas count];
}

-(UITableViewCell *)tableView:(UITableView *)tableView cellForRowAtIndexPath:(NSIndexPath *)indexPath
{
    static NSString *cellIdentifier     = @"_someCell";
    static NSString *tempCellIdentifier = @"_tempCell";
    NSInteger row = [indexPath row];
    int dataCount = [self._datas count];
    if (dataCount == 0 && row == 0)
    {
        UITableViewCell *cell = [tableView dequeueReusableCellWithIdentifier:tempCellIdentifier];
        if ( !cell )
        {
            cell = [[UITableViewCell alloc] initWithStyle:UITableViewCellStyleSubtitle
                                          reuseIdentifier:tempCellIdentifier];
            cell.selectionStyle = UITableViewCellSelectionStyleNone;
        }
        cell.detailTextLabel.text = @"Loading ...";
        return cell;
    }
    UITableViewCell *cell = [tableView dequeueReusableCellWithIdentifier:cellIdentifier];
    if ( !cell )
    {
        cell = [[UITableViewCell alloc] initWithStyle:UITableViewCellStyleSubtitle reuseIdentifier:cellIdentifier];
    }
    if( [self._datas count] > 0 )
    {
        cell.textLabel.text = [self._datas objectAtIndex:row];
    }
    return cell;
}

-(void)tableView:(UITableView *)tableView willDisplayCell:(UITableViewCell *)cell forRowAtIndexPath:(NSIndexPath *)indexPath
{
    //...
    
}

-(void)tableView:(UITableView *)tableView didSelectRowAtIndexPath:(NSIndexPath *)indexPath
{
    [tableView deselectRowAtIndexPath:indexPath animated:YES];
}

-(float)tableView:(UITableView *)tableView heightForRowAtIndexPath:(NSIndexPath *)indexPath
{
    return 88.0f;
}

#pragma --mark UIScrollViewDelegate
-(void)scrollViewDidEndDecelerating:(UIScrollView *)scrollView
{
    KRWatchScrollType _krWatchScrollType = [self._krWatchScroll findScrollingToWhereWithScrollView:scrollView];
    switch (_krWatchScrollType)
    {
        case KRWatchScrollToTop:
            //捲到頂
            NSLog(@"Scrolling to Top.");
            break;
        case KRWatchScrollToBottom:
            //捲到底
            NSLog(@"Scrolling to Bottom.");
            break;
        case KRWatchScrollNothing:
        default:
            //Nothing
            NSLog(@"Scrolling Nothing.");
            break;
    }
}

@end
```

## Version

KRWatchScroll now is V0.9 beta.

## License

KRWatchScroll is available under the MIT license ( or Whatever you wanna do ). See the LICENSE file for more info.
