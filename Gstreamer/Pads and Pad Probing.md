In a GStreamer pipeline, elements are connected through their pads, and data flows from sink pads to src pads. The overall structure and functionality of a GStreamer pipeline are defined by how elements and their pads are interconnected.

Pad probing allows interception and inspection of data as it moves through a GStreamer pipeline.

**Purpose:** Inject custom logic at specific pipeline points, enabling examination or modification of data passing through an element's pad.

Probes set on pads respond to events like BUFFER (data handling), EVENT (pipeline events), or QUERY (pad capabilities queries).

GStreamer elements can have 4 types of pads, depending on the nature and functionality of the element:
- **Sink Pad:**
    - **Role:** Receives data into the element.
    - **Symbol:** Named "sink" or "sink_%d." (where %d is a numerical identifier.)
    - **Functionality:** Input pad connected to the previous element in the pipeline.
```python
# Get the element and its sink pad
sink = pipeline.get_by_name('<ELEMENT>')
sink_pad = sink.get_static_pad('sink')

# Add a BUFFER probe to the sink pad
sink_pad.add_probe(Gst.PadProbeType.BUFFER, <on_sink_pad_buffer_probe>)
```

- **Src Pad:**
    - **Role:** Sends data out of the element.
    - **Symbol:** Named "src" or "src_%d." (where %d is a numerical identifier.)
    - **Functionality:** Output pad connected to the next element in the pipeline.
```python
# Get the element and its source pad
src = pipeline.get_by_name('<ELEMENT>')
src_pad = src.get_static_pad('src')

# Add a BUFFER probe to the source pad
src_pad.add_probe(Gst.PadProbeType.BUFFER, <on_src_pad_buffer_probe>)
```

- **Request Pad:**  
    - **Role:** Dynamically created or exposed during runtime.
    - **Symbol:** Named based on the element's decision.
    - **Functionality:** Used for scenarios where the number or type of pads can change dynamically.
```python
# Get the element
source = pipeline.get_by_name('<ELEMENT>')

# Connect the "pad-added" signal to the callback function
source.connect('pad-added', <on_request_pad_created>, None)
```

- **Ghost Pad:**
    - **Role:** Exposes internal pads of an element.
    - **Symbol:** Named like the internal pads it represents.
    - **Functionality:** Simplifies the usage of an element by exposing a subset of its internal pads.
```python
# Get the element and its ghost pad
mixer = pipeline.get_by_name('<ELEMENT>')
ghost_pad = mixer.get_static_pad('src')

# Add a BUFFER probe to the ghost pad
ghost_pad.add_probe(Gst.PadProbeType.BUFFER, <on_ghost_pad_buffer_probe>)
```
