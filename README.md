# Socket Activation for SystemD

## Installation

```sh
$ go get github.com/lemenkov/systemd.go # ignore the following error
stat github.com/lemenkov/systemd.go: no such file or directory
$
```

## Usage

```golang
import (
  "github.com/lemenkov/systemd.go"
  "log"
  "net"
  "net/http"
)

func main() {
  var listener net.Listener
  var err error

  sockets := systemd.ListenFds()
  if sockets == nil {
    listener, err = net.Listen("unix", "/tmp/servicename.socket")
  } else {
    listener, err = net.FileListener(sockets[0])
  }

  if err != nil {
    log.Fatalf("error setting up socket: %s", err)
  }

  // TODO: Set up your HTTP handlers here.

  http.Serve(listener, http.DefaultServeMux)
}
```
