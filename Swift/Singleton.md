### 使用类常量
```
class MTNetWorkManager {

    static let shared = MTNetWorkManager(baseURL: API.baseURL)
    
    let baseURL: URL
    
    private init(baseURL: URL) {
        self.baseURL = baseURL
    }
}

//外部调用 MTNetWorkManager.shared

```

### 使用类方法

```
class MTNetWorkManager {

    static private let sharedInstance = {
        
        let shared = MTNetWorkManager(baseURL: API.baseURL)
        
        //Configure 可以做一些初始化配置

        return shared
    
    }()
    
    let baseURL: URL
    
    private init(baseURL: URL) {
        self.baseURL = baseURL
    }
    
    
    static func shared() {
        return sharedInstance
    }
}

//外部调用 MTNetWorkManager.shared()

```



