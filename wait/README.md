# What is it?
Both utility and GO package to wait for port to open (TCP, UDP).

# Why we need it?

In dockerized applications usually we deploy 
several containers with the main program container.

We need to know if containers are started, so we
can continue to execute our program or fail it after some deadline.

There are a lot of examples through the internet 
that advise us to use several `bash` commands like `nc`, `timeout`, `curl`. But what if we have GO program minimal docker image `from scratch` that does not have `bash`? We could use this package as library in our program - just put couple lines of code to check if some ports are available.

But also this package can be donwloaded as utility and used from command line.

# Library usage

```
if !wait.Do(
    "tcp",                     // proto
    []string{"postgres:5432"}, // addresses
    time.Millisecond*100,      // delay between requests
    time.Second*15,            // deadline
    true,                      // debug 
) {
    logger.Error("db is not available")
    return
}
```

# Utility usage

```bash
  -addrs string
        address:port
  -deadline uint
        deadline in milliseconds (default 10000)
  -debug
        debug messages toggler
  -delay uint
        delay in milliseconds (default 100)
  -proto string
        tcp (default "tcp")
```

## 1 address check
```bash
net-wait-go -addrs ya.ru:443 -debug true
2020/06/30 18:07:38 ya.ru:443 is OK
```

## 2 addresses check
```bash
net-wait-go -addrs ya.ru:443,yandex.ru:443 -debug true
2020/06/30 18:09:24 yandex.ru:443 is OK
2020/06/30 18:09:24 ya.ru:443 is OK
```

## 2 addresses check fail
```bash
net-wait-go -addrs ya.ru:445,yandex.ru:445 -debug true
2020/06/30 18:09:24 yandex.ru:443 is FAILED
2020/06/30 18:09:24 ya.ru:443 is is FAILED
...
```

