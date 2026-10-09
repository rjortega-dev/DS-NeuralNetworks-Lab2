# Instructions

Explain how CNNs build a hierarchy of features through their layer structure. In your answer:

- Describe what types of features are typically detected at early, middle, and deep layers of a CNN

- Explain the concept of a receptive field and how it changes as we move deeper into the network

- Illustrate with a specific example of how this hierarchical feature extraction might work in developing a Convolutional Neural Network (CNN) approach, as CNNs excel at extracting meaningful visual features from images.

Reminder: Your team has access to a dataset of 50,000 labeled images of parts (defective and non-defective) collected from production lines over the past year.

You may refer to TensorFlow, Keras, or PyTorch documentation for terminology.

Your explanation should focus on the conceptual understanding rather than implementation details. Planning out how a project will work is imperative to a successful CNN approach.

## Response

**Feature hierarchy.** A CNN stacks convolutional layers so each builds on the previous layer’s output. Early layers detect low-level features: edges, corners, color gradients, and fine textures. On a part image, that means machined edges or metal grain. Middle layers combine these into patterns and shapes such as curves, grooves, holes, and repeated surface textures. Deep layers combine those into high-level, task-specific concepts, such as a crack running across a weld or a misaligned component, which a final classifier layer maps to “defective” or “non-defective.”

**Receptive field.** A neuron’s receptive field is the region of the input image that can influence its activation. A layer-1 neuron with a 3×3 kernel sees 3×3 pixels. Each layer widens the field: RF*l = RF*(l-1) + (k*l − 1) × jump*(l-1), where k is kernel size and jump is the product of earlier strides. Two stacked 3×3 convolutions give a 5×5 RF, and three give 7×7. Pooling or strided convolutions increase the jump, so the RF grows faster. This is why deeper neurons can integrate context across large spatial regions rather than responding only to local pixels, which is what lets the network recognize structures bigger than any single edge.

**Application.** Take 224×224 images from our 50,000-part dataset. In Block 1 (two 3×3 convs, RF 5×5), filters respond to edges and fine line segments, such as the thin edge of a scratch. After 2×2 max pooling, Block 2 (RF ≈ 14×14) combines these into pits, burrs, and curvature irregularities. Block 3 (RF ≈ 32×32) and deeper blocks, whose RFs exceed 100 pixels, respond to larger structures like cracks spanning a surface or a deformed flange. Global average pooling and a sigmoid output then give a defect probability. Using a pretrained model (e.g., ResNet50 in Keras), I’d freeze the early layers, since edge and texture detectors transfer well, and fine-tune the deeper layers on our defect labels. A key design check is that the final RF must be large enough to cover the biggest defect we need to detect.
