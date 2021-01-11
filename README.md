# Xerock Mesh

**Xerock Mesh** is a firmware to [ESP32 devices](http://espressif.com/en/products/hardware/esp32/resources), implementing a mesh network. It was intended to be used with the **Xerock Procjet**. To know more:

* *Daemon*: [https://github.com/rnascunha/xerock](https://github.com/rnascunha/xerock);

* *Interface*: [https://github.com/rnascunha/xerock_web](https://github.com/rnascunha/xerock_web);

* *Website*: [https://rnascunha.github.io/xerock](https://rnascunha.github.io/xerock).

## Compile and flash

To configure, compile and flash the firmware to your device, first install the IDF toolchain as explained [here](https://docs.espressif.com/projects/esp-idf/en/latest/esp32/get-started/index.html).

Now, download the **Xerock Mesh** firmware:

```
$ git clone https://github.com/rnascunha/xerock_mesh
$ cd xerock_mesh
```

Configure to your envirioment:

```
$ idf.py menuconfig
```

Go to *Mesh Configuration*. It's mandatory to set:

* **Router SSID**: your local router SSID;

* **Router password**: your local router password;

* **TCP Server IP**: the server IP that the mesh border router will connect;

* **TCP Server Port**: the server port that the mesh border router will connect;

Other facultative options are:

* **Enable DS**: set the configuration of external devices at the nodes (not very well explained, I know);

* **Enable root conflic**: if enabled, more than one device can connect to the server;

* **Mesh Topology**: the topology of the mesh network.

Other options are internal of the mesh network. If you don't know how to set, or how it will impact the network, just leave it at the default values.

Build and flash all devices that will be used:

```
$ idf.py build flash

#Or if you need to set the <port>

$ idf.py -p <port> build flash
```

Access [Xerock](https://rnascunha.github.io/xerock) and open sockets at *TCP Server App* accordingly how you set **TCP Server IP** and **TCP Server Port** parameters. Run the **ESP32 Mesh** script and turn on the devices. If everything was setup right, they must connect shortly.

## Custom firmware

The default firmware provided at this project, together if the [Xerock Website](https://rnascunha.github.io/xerock), will setup the network. But if you try to send any data to any device with this firmware, it will only echo it back. Not much use.

To make your own custom response, you must implement the following method:

```
namespace Mesh{
	node_data_handler(Node_Header* header, uint8_t* payload, uint8_t* buffer, void* arg);
}//Mesh
```

Where:

* **header**: received header (check *Node_Header* implmentation at `mesh/node_types.hpp`;

* **payload**: data payload received (random bytes);

* **buffer**: buffer reserved to be used;

* **arg**: argument set with `Node::fromds_cb_arg(void*)` function;

