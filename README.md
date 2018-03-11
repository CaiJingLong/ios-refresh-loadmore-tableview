# ios-refresh-loadmore-tableview
simple refresh loadmore tableview

## intro
a simple refresh and loadmore tableview for swift

only use UIRefreshControl and scroll observer to loadmore data

## usage

copy the KTableView.swift to your project

only dependent UIKit

min sdk is ios6

see ViewController 

`ktableview.k_delegate = self`

and

```swift
extension ViewController:KTableViewDelegate {
    
    func onRerefresh(tableView:KTableView) {
        NSLog("刷新开始")
        _ = getData()
            .delay(2, scheduler: ConcurrentMainScheduler.instance)
            .subscribe(onNext: { (result) in
                self.datas.removeAll()
                self.datas.append(contentsOf: result)
                tableView.reloadData()
                tableView.endRefresh()
            }, onError: nil, onCompleted: nil, onDisposed: nil)
    }
    
    func onLoadMore(tableView:KTableView) {
        NSLog("上拉加载开始")
        _ = getData(number: 5)
            .delay(2, scheduler: ConcurrentMainScheduler.instance)
            .subscribe(onNext: { (result) in
                self.datas.append(contentsOf: result)
                self.tableview.reloadData()
                self.tableview.endLoadMore()
            }, onError: nil, onCompleted: nil, onDisposed: nil)
    }
}
```

```swift

> view controll code 

```swift

import UIKit
import RxSwift

class ViewController: UIViewController,UITableViewDelegate,UITableViewDataSource {
    
    @IBOutlet weak var tableview: KTableView!
    let rc = UIRefreshControl()
    
    override func viewDidLoad() {
        super.viewDidLoad()
        
        tableview.delegate = self
        tableview.dataSource = self
        tableview.k_delegate = self
    }
    
    var datas = [String]()
    
    func tableView(_ tableView: UITableView, numberOfRowsInSection section: Int) -> Int {
        return datas.count
    }
    
    func tableView(_ tableView: UITableView, cellForRowAt indexPath: IndexPath) -> UITableViewCell {
        let cell = UITableViewCell()
        let label = UILabel(frame: CGRect(x: 5, y: 5, width: 220, height: 30))
        let data = datas[indexPath.row]
        label.text = data
        cell.addSubview(label)
        return cell
    }
    
    override func didReceiveMemoryWarning() {
        super.didReceiveMemoryWarning()
        // Dispose of any resources that can be recreated.
    }
    
}

extension ViewController:KTableViewDelegate {
    
    func onRerefresh(tableView:KTableView) {
        NSLog("刷新开始")
        _ = getData()
            .delay(2, scheduler: ConcurrentMainScheduler.instance)
            .subscribe(onNext: { (result) in
                self.datas.removeAll()
                self.datas.append(contentsOf: result)
                tableView.reloadData()
                tableView.endRefresh()
            }, onError: nil, onCompleted: nil, onDisposed: nil)
    }
    
    func onLoadMore(tableView:KTableView) {
        NSLog("上拉加载开始")
        _ = getData(number: 5)
            .delay(2, scheduler: ConcurrentMainScheduler.instance)
            .subscribe(onNext: { (result) in
                self.datas.append(contentsOf: result)
                self.tableview.reloadData()
                self.tableview.endLoadMore()
            }, onError: nil, onCompleted: nil, onDisposed: nil)
    }
}

```
