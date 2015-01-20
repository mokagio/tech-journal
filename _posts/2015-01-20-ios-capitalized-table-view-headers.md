---
layout: post
title: Capitalized table view headers for iOS apps
date: 2015-01-20 22:7:42
---

So far whenever I wanted to use a table view with headers, specially grouped table views, I always wrote a dumb subview with a label from scratch. Today it hit me: capitalizing the header of a table view is way easier!

```swift
import UIKit

class CapitalizedTableViewHeader: UITableViewHeaderFooterView {

    override func layoutSubviews() {
        super.layoutSubviews()
        self.textLabel.text = self.textLabel.text?.capitalizedString
    }
}

// In the code that configures the table view
tableView.registerClass(CapitalizedTableViewHeader.self, forHeaderFooterViewReuseIdentifier: "your-identifier")

// In the UITableViewDelegate
func tableView(tableView: UITableView, viewForHeaderInSection section: Int) -> UIView? {
    let header = tableView.dequeueReusableHeaderFooterViewWithIdentifier(headerIdentifier) as CapitalizedTableViewHeader
    let text = ...
    header.textLabel.text = text
    return header
}
```

Done!