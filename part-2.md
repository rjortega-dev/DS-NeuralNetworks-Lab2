# Instructions

Discuss the following key architectural decisions you would make and why:

- What kernel sizes would you choose for different layers and why?
- How would you incorporate pooling layers, and what benefits would they provide in this specific application?
- What considerations would guide your decisions about network depth (number of layers) and width (number of filters per layer)?
- How might you leverage transfer learning in this scenario, and what would be the advantages?

Support your answers with reasoning that demonstrates your understanding of CNN principles and the practical constraints of mobile deployment.

Reminder: Your team has access to a dataset of 50,000 labeled images of parts (defective and non-defective) collected from production lines over the past year.

Your explanation should focus on the conceptual understanding of CNN architecture rather than implementation details.

Planning and communicating to your team how a project will work is imperative to a successful CNN architecture approach.

## Response

**Kernel sizes.** I would use small 3×3 kernels for most layers. Small kernels pick up fine details like thin cracks, and stacking several of them lets the network see larger areas while using fewer parameters than one big kernel. This keeps the model small and fast, which matters because it needs to run in real time on the production line. I might use a slightly larger kernel in the very first layer to pick up surface texture.

**Pooling.** I would choose a moderate number of layers. Too few and the network can’t recognize complex defects, too many and it becomes slow and may memorize the training images. With 50,000 images, a medium-sized network is a safe choice. Because it has to run in real time, I’d also use efficient designs like MobileNet that do the same work with far less computing. I’d test a few sizes and pick the smallest one that catches defects reliably.

**Depth and width.** Depth should be set so the final receptive field covers the largest defect we need to detect. Width typically doubles as resolution halves (32→64→128→256), which keeps per-layer compute balanced. With 50,000 images, a very deep network risks overfitting, so I’d rely on a moderate-depth architecture, residual connections, batch normalization, and augmentation (flips, rotations, lighting changes). Because inspection must run in real time on edge hardware, I’d use depthwise separable convolutions and 1×1 bottleneck layers, which reduce compute roughly 8-9× versus standard convolutions at similar accuracy. Latency and memory budgets set the ceiling, and validation recall on defects decides where to stop.

**Transfer learning.** I would start from a model already trained on a large image dataset, since it already knows how to detect edges and textures. I’d keep the early layers as they are and retrain the later layers on our part images. Advantages: faster training, less data needed, and better accuracy.
