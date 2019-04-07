# Image Transformer (1802.05751)
- Image generation -> autoregressive sequence generation or transformation problem.
- Generalize self-attetion to a sequence modeling formulation of image generation with tractable likelihood.
- Likelihood is made tractable by modeling the joint distribution of the pixels in the image as the product of conditional distributions.
## Related work
- RNN/CNN: sequentially predicts each next pixel given all previously generated pixels.
- PixelCNN: paralized, growing receptive field comes at cost in computational performance.
- GAN: generator trained in opposition to a discriminator network; unstable, mode collapse (generated images fail to reflect diversity in the training set).
- Self-attention: locally restricted form of nulti-head self-attentiont ~ sparsely parameterized form of gated convolution.
## Architecture
### Image Representation
- Pixel internsity -> Discrete categories
    - encoding: color-channel-specific set of 256 d-dim embedding vectors.
    - output intensities share the same embeddings across the channels
    - image [w, h, 3] -> [w$\times$h$\times$3, c]
- Pixel intensity -> Ordinal values 
    - with 1$\times$3 strided convolution: [h, w, 3] -> [h, w, d]
    - sine/cosine functions of coordinates -> d-dim encoding (d/2 ~ row index, d/2 ~ col index)
### Self-Attention
- Encoder generates contextualized, per-pixel-channel representation of the source image.
- Decoder autoregressively outputs pixel intensities, one channel per pixel at each time step; consuming previously generated pixels and encoded input image.
### Local Self-Attention
- Restrict positions in the memory matrix M to a local neighborhood around the query position -> 1D/2D local attention. 
- Partition the image into query blocks associated with large memory block. Self-attention is computed for all query blocks in parallel. 
- Feed-forward networks and layer normalizations are parallelized for all positions.
### Loss Function
- Categorical distribution across each channel: 256$\times$3$\times$h$\times$w-dim output.
- Discretized mixture of logistics (DMOL) over three channels.

# Pixel Recurrent Neural Network
- estimate a distribution over natural images -> tractably compute likelihood -> image generation
- sequentially scans the pixel and predicts conditional distribution given the scanned context
- joint distribution over image pixels is factorized into product of conditional distributions
- parameters for prediction are shared across pixels
- distributions computed in parallel, generation in sequential
- distribution is discrete with x in [0, 255]
## PixelRNN
### Row LSTM
- Input-to-state component
    - computed for the entire 2-dim input map
    - k $\times$ 1 convolution size, weights shared across rows (1-dim convolution 
