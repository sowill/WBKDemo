//
//  ViewController.m
//  WBKDemo
//
//  Created by LiangYingfeng on 8/28/15.
//  Copyright (c) 2015 微位（上海）网络科技有限公司. All rights reserved.
//

#import "ViewController.h"
#import "WBKBeaconManager.h"
#import "WBKApi.h"

@interface ViewController ()
<WBKBeaconManagerDelegate, UITableViewDataSource, UITableViewDelegate>
{
    NSMutableArray *arr;
}

@property(nonatomic, retain) UITableView *table;
@property(nonatomic, retain) UIButton *cancelBtn;

@end

@implementation ViewController

- (void)viewDidLoad {
    [super viewDidLoad];
    
    arr = [[NSMutableArray alloc] init];
    
    {
        _cancelBtn = [UIButton buttonWithType:UIButtonTypeCustom];
        _cancelBtn.tag = 100;
        [_cancelBtn setFrame:CGRectMake(10, 20, 100, 44)];
        [_cancelBtn setTitle:@"取消扫描" forState:UIControlStateNormal];
        [_cancelBtn setBackgroundColor:[UIColor redColor]];
        [_cancelBtn addTarget:self action:@selector(clickForCancel:) forControlEvents:UIControlEventTouchUpInside];
        [self.view addSubview:_cancelBtn];
        
    }
    
    {
        _table = [[UITableView alloc] init];
//        _table.frame = [UIScreen mainScreen].bounds;
        CGSize size = [UIScreen mainScreen].bounds.size;
        _table.frame = CGRectMake(0, 65, size.width, size.height - 65);
        _table.dataSource = self;
        _table.delegate = self;
        [self.view addSubview:_table];
        [_table release];
    }
    
    
    [WBKApi sharedInstance].debugModeActive = YES;
    [WBKApi sharedInstance].powerAlertEnable = YES;
    [WBKBeaconManager sharedInstance].delegate = self;
    [WBKBeaconManager sharedInstance].outOfRangeDelay = 8.0;
//    [[WBKBeaconManager sharedInstance] requestWhenInUseAuthorization];
    [[WBKBeaconManager sharedInstance] requestAlwaysAuthorization];
    
}

- (void)viewDidAppear:(BOOL)animated
{
    [super viewDidAppear:animated];
    
}

- (void)didReceiveMemoryWarning {
    [super didReceiveMemoryWarning];
}

- (void)dealloc
{
    [_table release];
    [arr release];    
    [super dealloc];
}


#pragma mark - cancel button event

- (void)clickForCancel:(UIButton *)sender
{
    NSUUID *uuid = [[NSUUID alloc] initWithUUIDString:@"0771B5A9-BF79-444F-A751-DC4C2C067232"];
    WBKBeaconID *beaconID = [WBKBeaconID beaconIDWithProximityUUID:uuid];
    //[WBKBeaconID beaconIDWithProximityUUID:uuid major:10019 minor:55291];
    if (sender.tag == 100)
    {
        sender.tag = 101;
        [sender setTitle:@"开始扫描" forState:UIControlStateNormal];
        [[WBKBeaconManager sharedInstance] stopRangingBeaconsWithID:beaconID];
    }else
    {
        [[WBKBeaconManager sharedInstance] startRangingBeaconsWithID:beaconID];
        sender.tag = 100;
        [_cancelBtn setTitle:@"取消扫描" forState:UIControlStateNormal];
    }
    [uuid release];
}


#pragma mark - table delegate

- (NSInteger)numberOfSectionsInTableView:(UITableView *)tableView
{
    return 1;
}

- (NSInteger)tableView:(UITableView *)tableView numberOfRowsInSection:(NSInteger)section
{
    return [arr count];
}

- (UITableViewCell *)tableView:(UITableView *)tableView cellForRowAtIndexPath:(NSIndexPath *)indexPath
{
    static NSString *identifier = @"mycell";
    UITableViewCell *cell = [tableView dequeueReusableHeaderFooterViewWithIdentifier:identifier];
    if (cell == nil)
    {
        cell = [[[UITableViewCell alloc] initWithStyle:UITableViewCellStyleDefault reuseIdentifier:identifier] autorelease];
        cell.textLabel.lineBreakMode = NSLineBreakByCharWrapping;
        cell.textLabel.numberOfLines = 0;
    }
    
    NSInteger row = [indexPath row];
    WBKBeacon *beacon = [arr objectAtIndex:row];
    
    cell.textLabel.text = [NSString stringWithFormat:@"%li:uuid:%@, major:%@, minor:%@, proximity:%li, accuracy=%.2f rssi:%li", (long)(row + 1), beacon.proximityUUID.UUIDString, beacon.major, beacon.minor, (long)beacon.proximity, beacon.accuracy, (long)beacon.rssi];
    
    return cell;
}

- (CGFloat)tableView:(UITableView *)tableView heightForRowAtIndexPath:(NSIndexPath *)indexPath
{
    return 100.0f;
}


#pragma mark - WBKBeaconManagerDelegate

- (void)beaconManager:(WBKBeaconManager *)beaconManager didChangeAuthorizationStatus:(WBKAuthorizationStatus)status
{
    NSLog(@"didChangeAuthorizationStatus");
//    [[WBKBeaconManager sharedInstance] stopRangingAllBeacons];
    if (status == WBKAuthorizationStatusAuthorizedAlways)
    {
        NSLog(@"to start ranging beacons always.");
        NSUUID *uuid = [[NSUUID alloc] initWithUUIDString:@"0771B5A9-BF79-444F-A751-DC4C2C067232"];
        WBKBeaconID *beaconID = [WBKBeaconID beaconIDWithProximityUUID:uuid];
        [[WBKBeaconManager sharedInstance] startRangingBeaconsWithID:beaconID];
        [uuid release];
    }else if (status == WBKAuthorizationStatusAuthorizedWhenInUse)
    {
        NSLog(@"to start ranging beacons when in need.");
        NSUUID *uuid = [[NSUUID alloc] initWithUUIDString:@"0771B5A9-BF79-444F-A751-DC4C2C067232"];
        WBKBeaconID *beaconID = [WBKBeaconID beaconIDWithProximityUUID:uuid];
        [[WBKBeaconManager sharedInstance] startRangingBeaconsWithID:beaconID];
        [uuid release];
    }
}

- (void)beaconManager:(WBKBeaconManager *)beaconManager didEnterBeacon:(WBKBeacon *)beacon
{
    NSLog(@"didEnterBeacon:%@",beacon.identifier);
    
}

- (void)beaconManager:(WBKBeaconManager *)beaconManager didExitBeacon:(WBKBeacon *)beacon
{
    NSLog(@"didExitBeacon:%@",beacon.identifier);
}

- (void)beaconManager:(WBKBeaconManager *)beaconManager didRangeBeacons:(NSArray *)beacons
{
    NSLog(@"didRangeBeacons");
    [arr setArray:[[WBKBeaconManager sharedInstance] beaconsInRange]];
    [_table reloadData];
}

- (void)beaconManager:(WBKBeaconManager *)beaconManager didEnterBeaconID:(WBKBeaconID *)beaconID
{
    NSLog(@"didEnterBeaconID:%@",beaconID.identifier);
}

- (void)beaconManager:(WBKBeaconManager *)beaconManager didExitBeaconID:(WBKBeaconID *)beaconID
{
    NSLog(@"didExitBeaconID:%@",beaconID.identifier);
}

- (void)beaconManager:(WBKBeaconManager *)beaconManager didRangeError:(NSError *)error
{
    NSLog(@"error:\n%@", [error localizedDescription]);
}

@end
