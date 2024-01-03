- **Purpose:** Gst-nvinfer plugin perform inference (making predictions) on input data using NVIDIA TensorRT.

- **Input Handling:**
    - Processes batches in [[NV12]]/[[RGBA]] formats.
    - Expects batches with [[NvDsBatchMeta]] metadata.

- **Transformations:**
    - Converts and scales input frames.
    - Sends transformed data to libnvds_infer.
    - Customize and extend the behavior using [[Pads and Pad Probing|Pad Probing]]

- **Preprocessing:**
    - libnvds_infer normalizes and subtracts mean.
    - Pre-processing involves a formula for each pixel:
      `y = net scale factor * (x - mean).`
        - `x`: Input pixel value, represented as an int8 with a range of \[0, 255\].
        - `mean`: Corresponding mean value, either read from a mean file or calculated from offsets.
        - `net-scale-factor`: Pixel scaling factor specified in the configuration file. Sample values could include:
          - `0.003922` (approximately 1/255) for normalizing pixel values in the range [0, 255] to [0, 1].
          - `0.007843` (approximately 1/127.5) for normalizing pixel values in the range [0, 127.5] to [-1, 1].
        - `y`: Output pixel value, represented as a float.
    - Customize and extend the behavior using [[Pads and Pad Probing|Pad Probing]]

- **Output:**
    - Output type depends on neural network.

