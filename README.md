<p align="center"> <img width="default" height="300"
        src="https://cdn.discordapp.com/attachments/736359303744585821/1141103483181867088/image-removebg-preview.png">
</p>

# pascal - Game Networking Library for Windows

Pascal is a single-header, UDP networking library for Windows applications. It provides an easy-to-use interface for creating both client and server applications. Pascal works similarly to [RakNet](https://github.com/facebookarchive/RakNet).

## Features

- Simple single-header library
- Supports both client and server networking
- Handles connection establishment, data transmission, and disconnection
- Provides lambda callbacks for events such as connection, disconnection, and received packets
- Supports packet handling and parsing
- Utilizes UDP for efficient and low-latency communication
- Ideal for game developers and any programs that need fast communication

## Installation

1. Download the single-header library file `pascal.hpp`.
2. Include the library in your project using `#include "pascal.hpp"`.

## Examples

### Creating a Client

```cpp
#include "pascal.hpp"

int main() {
    pascal::client client;
    pascal::result initResult = client.init();
    if (initResult != pascal::PASCAL_OK) {
        std::cerr << "Client initialization failed" << std::endl;
        return 1;
    }

    pascal::result connectionResult = client.connect("127.0.0.1", 12345);
    if (connectionResult != pascal::PASCAL_OK) {
        std::cerr << "Connection to server failed" << std::endl;
        return 1;
    }

    // Define callbacks for connection, disconnection, and received packets
    client.on_connect([]() { std::cout << "Connected to server" << std::endl; });
    client.on_disconnect([]() { std::cout << "Disconnected from server" << std::endl; });
    client.on_packet([](pascal::packet_type type, std::string data) {
        std::cout << "Received packet of type " << type << ": " << data << std::endl;
    });

    // Run client-specific logic

    return 0;
}
```

### Creating a Server

```cpp
#include "pascal.hpp"

int main() {
    pascal::server server;
    pascal::result initResult = server.init();
    if (initResult != pascal::PASCAL_OK) {
        std::cerr << "Server initialization failed" << std::endl;
        return 1;
    }

    pascal::result startResult = server.start(12345);
    if (startResult != pascal::PASCAL_OK) {
        std::cerr << "Server start failed" << std::endl;
        return 1;
    }

    // Define callbacks for connection, disconnection, and received packets
    server.on_connect([]() { std::cout << "Client connected to server" << std::endl; });
    server.on_disconnect([]() { std::cout << "Client disconnected from server" << std::endl; });
    server.on_packet([](pascal::peer peer, pascal::packet_type type, std::string data) {
        std::cout << "Received packet of type " << type << " from client " << peer.guid << ": " << data << std::endl;
    });

    // Run server-specific logic

    return 0;
}
```

## That's All
If you have any questions or issues, feel free to create an issue or pull request. This library is in its early stages, so bugs are very likely to occur. Thanks!
