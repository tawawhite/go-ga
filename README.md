# go-ga

Send data to Google Analytics from Go.
This library implements the [measurement protocol] and supports a majority of the parameters.

[measurement protocol]: https://developers.google.com/analytics/devguides/collection/protocol/v1/reference

## Features

* Sending data to Google Analytics using GET or POST methods
* Encoding payloads to URLs for embedding
* A simple containerized beacon for reporting on application usage

### Parameter Support

The majority of the API has been implemented.
There is still work to be done around some array and map style data types.
The list below tries to document the current parameter support.
For more information, see the [parameter reference] documentation.

[parameter reference]: https://developers.google.com/analytics/devguides/collection/protocol/v1/parameters

- [x] General
- [x] User
- [x] Session
- [x] Traffic Sources
- [x] System Info
- [x] Hit
- [ ] Content Information
  - [x] Document Location URL
  - [x] Document Host Name
  - [x] Document Path
  - [x] Document Title
  - [x] Screen Name
  - [ ] Content Group
  - [x] Link ID
- [x] Apps
- [x] Events
- [x] E-Commerce
- [ ] Enhanced E-Commerce
- [x] Social Interactions
- [x] Timing
- [x] Exceptions
- [ ] Custom Dimensions / Metrics

## Examples

### Sending data to Google Analytics

```go
package main

import (
    "log"
    "time"

    "github.com/mjpitz/go-ga/client/v1"
    "github.com/mjpitz/go-ga/client/v1/gatypes"
)

func main() {
    client := v1.NewClient("UA-XXXXXX-1", "customUserAgent")
    ping := &gatypes.Payload{
        HitType: "event",
        NonInteractionHit: true,
        DisableAdvertisingPersonalization: true,
        Event: gatypes.Event{
            EventCategory: "beacon",
            EventAction: "heartbeat",
        },
    }
    
    for {
        if err := client.SendPost(ping); err != nil {
            log.Fatal(err)
        }
        time.Sleep(time.Minute)
    }
}
```

### Encoding a payload for embed

```go
package main

import (
    "github.com/mjpitz/go-ga/client/v1"
    "github.com/mjpitz/go-ga/client/v1/gatypes"
)

func main() {
    ping := &gatypes.Payload{
        TrackingID: "UA-XXXXXX-1",
        HitType: "event",
        Event: gatypes.Event{
            EventCategory: "email",
            EventAction: "open",
        },
    }
    
    url, err := v1.Encode(ping)
    // do something with url
}
```
