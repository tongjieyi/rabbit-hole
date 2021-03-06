# Rabbit Hole, a RabbitMQ HTTP API Client for Go

This library is a [RabbitMQ HTTP API](http://hg.rabbitmq.com/rabbitmq-management/raw-file/450b7ea22cfd/priv/www/api/index.html) client for the Go language.

## Supported Go Versions

Rabbit Hole requires Go 1.1+.


## Supported RabbitMQ Versions

 * RabbitMQ 3.x
 * RabbitMQ 2.x

All versions require [RabbitMQ Management UI plugin](http://www.rabbitmq.com/management.html) to be installed and enabled.


## Project Maturity

Rabbit Hole is a **young** project and breaking API changes are not out of the question.

It is, however, about 90% feature complete and decently documented.


## Installation

```
go get github.com/michaelklishin/rabbit-hole
```


## Documentation

### Overview

To import the package:

``` go
import (
       "github.com/michaelklishin/rabbit-hole"
)
```

All HTTP API operations are accessible via `rabbithole.Client`, which
should be instantiated with `rabbithole.NewClient`:

``` go
// URI, username, password
rmqc, _ = NewClient("http://127.0.0.1:15672", "guest", "guest")
```

[API reference](http://godoc.org/github.com/michaelklishin/rabbit-hole) is available on [godoc.org](http://godoc.org).

### Getting Overview

``` go
res, err := rmqc.Overview()
```

### Node and Cluster Status

``` go
xs, err := rmqc.ListNodes()
// => []NodeInfo, err

node, err := rmqc.GetNode("rabbit@mercurio")
// => NodeInfo, err
```


### Operations on Connections

``` go
xs, err := rmqc.ListConnections()
// => []ConnectionInfo, err

conn, err := rmqc.GetConnection("127.0.0.1:50545 -> 127.0.0.1:5672")
// => ConnectionInfo, err

// Forcefully close connection
_, err := rmqc.CloseConnection("127.0.0.1:50545 -> 127.0.0.1:5672")
// => *http.Response, err
```


### Operations on Channels

``` go
xs, err := rmqc.ListChannels()
// => []ChannelInfo, err

ch, err := rmqc.GetChannel("127.0.0.1:50545 -> 127.0.0.1:5672 (1)")
// => ChannelInfo, err
```


### Operations on Exchanges

``` go
xs, err := rmqc.ListExchanges()
// => []ExchangeInfo, err

// list exchanges in a vhost
xs, err := rmqc.ListExchangesIn("/")
// => []ExchangeInfo, err

// information about individual exchange
x, err := rmqc.GetExchange("/", "amq.fanout")
// => ExchangeInfo, err

// declares an exchange
resp, err := rmqc.DeclareExchange("/", "an.exchange", ExchangeSettings{Type: "fanout", Durable: false})
// => *http.Response, err

// deletes individual exchange
resp, err := rmqc.DeleteExchange("/", "an.exchange")
// => *http.Response, err
```


### Operations on Queues

``` go
xs, err := rmqc.ListQueues()
// => []QueueInfo, err

// list queues in a vhost
xs, err := rmqc.ListQueuesIn("/")
// => []QueueInfo, err

// information about individual queue
x, err := rmqc.GetQueue("/", "a.queue")
// => QueueInfo, err

// declares a queue
resp, err := rmqc.DeclareQueue("/", "a.queue", QueueSettings{Durable: false})
// => *http.Response, err

// deletes individual queue
resp, err := rmqc.DeleteQueue("/", "a.queue")
// => *http.Response, err
```


### Operations on Bindings

TBD


### Operations on Vhosts

``` go
xs, err := rmqc.ListVhosts()
// => []VhostInfo, err

// information about individual vhost
x, err := rmqc.GetVhost("/")
// => VhostInfo, err

// creates or updates individual vhost
resp, err := rmqc.PutVhost("/")
// => *http.Response, err

// deletes individual vhost
resp, err := rmqc.DeleteVhost("/")
// => *http.Response, err
```


### Managing Users

``` go
xs, err := rmqc.ListUsers()
// => []UserInfo, err

// information about individual user
x, err := rmqc.GetUser("my.user")
// => UserInfo, err

// creates or updates individual user
resp, err := rmqc.PutUser("my.user", UserSettings{Password: "s3krE7", Tags: "management policymaker"})
// => *http.Response, err

// deletes individual user
resp, err := rmqc.DeleteUser("my.user")
// => *http.Response, err
```


### Managing Permissions

``` go
xs, err := rmqc.ListPermissions()
// => []PermissionInfo, err

// permissions of individual user
x, err := rmqc.ListPermissionsOf("my.user")
// => []PermissionInfo, err

// permissions of individual user in vhost
x, err := rmqc.GetPermissionsIn("/", "my.user")
// => PermissionInfo, err

// updates permissions of user in vhost
resp, err := rmqc.UpdatePermissionsIn("/", "my.user", Permission{Configure: ".*", Write: ".*", Read: ".*"})
// => *http.Response, err

// revokes permissions in vhost
resp, err := rmqc.ClearPermissionsIn("/", "my.user")
// => *http.Response, err
```




## Contributing

1. Fork it
2. Create your feature branch (`git checkout -b my-new-feature`)
3. Commit your changes (`git commit -am 'Add some feature'`)
4. Push to the branch (`git push -u origin my-new-feature`)
5. Create new Pull Request


## License & Copyright

2-clause BSD license.

(c) Michael S. Klishin, 2013.


[![Bitdeli Badge](https://d2weczhvl823v0.cloudfront.net/michaelklishin/rabbit-hole/trend.png)](https://bitdeli.com/free "Bitdeli Badge")

