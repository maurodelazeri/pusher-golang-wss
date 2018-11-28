# Pusher Websocket Client Go Library

Golang library for connecting to a Pusher App over websockets.

## Installing

```
$ go get github.com/maurodelazeri/pusher-golang-wss/
```


## Getting Started

```go
package main

import (
	"fmt"

	"github.com/maurodelazeri/pusher-golang-wss/pusher"
)

// Example Handler object
type chanHandler struct{}

func (o *chanHandler) HandleEvent(event pusher.Event) {
	fmt.Println("Handler: Got Event:", event.Channel, event.Event, event.Data)
}

// Example Handler function
func eventHandler(event pusher.Event) {
	fmt.Println("HandlerFunc: Got Event:", event.Channel, event.Event, event.Data)
}

func main() {
	// Create pusher client connection for "app_key"
	client := pusher.NewClient("c0eef4118084f8164bec65e6253bf195", "notifier.bitskins.com:443")

	// Subscribe to a channel
	ch := client.Subscribe("inventory_changes")

	// Bind to events on the channel.  Can use either a Handler object.
	ch.Bind("delisted_or_sold", &chanHandler{})
	ch.BindFunc("delisted_or_sold", eventHandler)

	// bind to all channel events
	client.BindAll(&chanHandler{})
	client.BindAllFunc(eventHandler)
	select {}
}
```

## TODO

* Support private & presence channels.  Need auth handler/callback.

