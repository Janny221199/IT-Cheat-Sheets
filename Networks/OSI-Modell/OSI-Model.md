# OSI-Model
- **O**pen **S**ystems **I**nterconnection
- 1980s
- The complexity of networks must be reduced and delegated (similar to the [Divide and Conquer](../../Algorithms/Divide-and-Conquer.md)) Algorithm
- standardized reference model

## Layers
- 7 layers
	- Layers are arranged vertically on top of each other regarding to the hardware proximity
	- Each layer provides services to the next higher layer via an interface
	- Each layer uses services of the next lower one
	- technical implementation of lower layers know known for upper layers
	- each layer is responsible for a sprecific tasks
	- network devices can be related to a specific layer
		- A device operates on level n exactly when it understands the [Protocol](../Protocol.md)s up to level n.

Layer | 
--- | ---
[7-Application-Layer](7-Application-Layer.md) | 
[6-Presentation-Layer](6-Presentation-Layer.md) |
[5-Sesseion-Layer](5-Sesseion-Layer.md) |
[4-Transport-Layer](4-Transport-Layer.md) |
[3-Network-Layer](3-Network-Layer.md) |
[2-Data-Link-Layer](2-Data-Link-Layer.md) |
[1-Physical-Layer](1-Physical-Layer.md) |

## Encapsulation
- takes information of the layer above and adds information (e.g. Header) to it
- Characteristic for the layer model is the packaging of one data unit of level n into a data unit of level n - 1 when sending.
- decapsulation is the process of receiving data and retrieving the data of the specific layer (removing the headers of lower layers)

```
         sender                                                                                receiver
┌───────────────────────┐                                                              ┌───────────────────────┐
│                       │                     Messages                                 │                       │
│   Application  Layer  │                                        ┌────┐                │   Application  Layer  │
│   Presentation Layer  ├────────────────────────────────────────┤Data├───────────────►│   Presentation Layer  │
│   Session      Layer  │                                        └────┘                │   Session      Layer  │
│                       │                     HTTP, SMTP, etc.                         │                       │
└───────────┬───────────┘                                                              └───────────▲───────────┘
            │                                                                                      │
┌───────────▼───────────┐                                                              ┌───────────┴───────────┐
│                       │                     Segments                                 │                       │
│                       │                         ┌──────────────┬────┐                │                       │
│   Transport Layer     ├─────────────────────────┤Segment Header│Data├───────────────►│   Transport Layer     │
│                       │                         └──────────────┴────┘                │                       │
│                       │                     TCP, UDP, etc.                           │                       │
└───────────┬───────────┘                                                              └───────────▲───────────┘
            │                                                                                      │
┌───────────▼───────────┐                                                              ┌───────────┴───────────┐
│                       │                     Packets                                  │                       │
│                       │               ┌─────────┬──────────────┬────┐                │                       │
│   Network Layer       ├───────────────┤IP Header│Segment Header│Data├───────────────►│   Network Layer       │
│                       │               └─────────┴──────────────┴────┘                │                       │
│                       │                     IP, etc.                                 │                       │
└───────────┬───────────┘                                                              └───────────▲───────────┘
            │                                                                                      │
┌───────────▼───────────┐                                                              ┌───────────┴───────────┐
│                       │                     Frames                                   │                       │
│                       │  ┌────────────┬─────────┬──────────────┬────┬─────────────┐  │                       │
│   Data Link Layer     ├──┤Frame Header│IP Header│Segment Header│Data│Frame Trailer├─►│   Data Link Layer     │
│                       │  └────────────┴─────────┴──────────────┴────┴─────────────┘  │                       │
│                       │                     Ethernet, WiFi, etc.                     │                       │
└───────────┬───────────┘                                                              └───────────▲───────────┘
            │                                                                                      │
┌───────────▼───────────┐                                                              ┌───────────┴───────────┐
│                       │                     Signale                                  │                       │
│                       │  ┌────────────────────────────────────────────────────────┐  │                       │
│   Pysical Layer       ├──┤01110000110100011110011001101101101010101010100110101011├─►│   Pysical Layer       │
│                       │  └────────────────────────────────────────────────────────┘  │                       │
│                       │                     DSL, Ethernet, etc.                      │                       │
└───────────┬───────────┘                                                              └───────────▲───────────┘
            │                                                                                      │
            │                                                                                      │
            │                                                                                      │
            │                                                                                      │
            └──────────────────────────────────────────────────────────────────────────────────────┘
                                              physical medium
                                              cables etc.
```
