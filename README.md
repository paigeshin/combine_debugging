### print

```swift
import Combine 

let publisher = (1...20).publisher 

publisher
	.print("Debugging")
	.sink {
	print($0)	
}
```

### handleEvents

```swift
import UIKit
import Combine

guard let url = URL(string: "https://jsonplaceholder.typicode.com/posts") else {
    fatalError("Invalid URL")
}

let request = URLSession.shared.dataTaskPublisher(for: url)

let subscription = request
    .handleEvents(receiveSubscription: { _ in print("Subscrition Received") },
                  receiveOutput: { _, _ in print("Received Output") },
                  receiveCompletion: { _ in print("Received Completion") },
                  receiveCancel: { print("Received Cancel") },
                  receiveRequest: { _ in print("Received Request") })
    .sink { print($0) } receiveValue: { data, response in
    print(data)
}
```

### breakpoint

```swift
import UIKit
import Combine

class ViewController: UIViewController {

    private var cancellable: AnyCancellable?
    
    override func viewDidLoad() {
        super.viewDidLoad()
        
        let publisher = (1...10).publisher
        
        self.cancellable = publisher
            .breakpoint(receiveOutput: { value in
                return value > 9
            })
            .sink {
                print($0)
            }
        
    }

}
```
