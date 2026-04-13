# Autoregressive Model for Prediction of Flow Around a Cylinder
## Problem Description

The flow around a circular cylinder is a canonical benchmark problem in computational fluid dynamics (CFD), characterized by complex unsteady phenomena such as vortex shedding and wake formation. Accurately resolving these dynamics typically requires high-fidelity numerical simulations of the Navier–Stokes equations, which are computationally expensive, especially for long time horizons. In this work, we investigate a data-driven approach for predicting the temporal evolution of the flow field using a convolutional neural network (CNN) based on the U-Net architecture. 

<img width="700" height="300" alt="image" src="https://github.com/user-attachments/assets/1f203cd2-83df-42e3-8db6-744465508aa2" />

The U-Net is a convolutional neural network architecture originally developed for biomedical image segmentation, but it has proven exceptionally effective for fluid dynamics due to its ability to capture both local details and global context. Its main parts are: 
* Encoder-Decoder Structure: The encoder progressively downsamples the input, increasing the receptive field while reducing spatial resolution. Early encoder layers capture fine local features such as cylinder boundaries and small-scale velocity gradients, while deeper layers encode more abstract, global representations of the flow. The bottleneck, located at the middle of the “U”, contains the most compressed representation of the input, summarizing the overall flow structure such as wake formation and large-scale patterns. The decoder then progressively upsamples this representation to reconstruct the output field.
* Skip connections: The skip connections concatenate encoder and decoder feature maps at the same resolution level. Without skip connections the decoder only has the bottleneck to work from - spatial detail about the cylinder boundary, boundary layer, etc. is lost. With skip connections that detail is preserved and passed directly.

<img width="700" height="300" alt="image" src="https://github.com/user-attachments/assets/b578de99-5312-4f0d-8021-9e450250d988" />

The aim of this study is to develop deep learning models - specifically autoregressive models - for the prediction of velocity fields in flow around a cylinder across varying flow regimes and geometric configurations. The dataset incorporates variations in key physical and geometric parameters, including inlet velocity, fluid density, kinematic viscosity, as well as cylinder topology and radius.

The proposed models are trained to predict future flow states based on current observations. In particular, the network is designed to perform both one-step-ahead and direct multi-step (five-step-ahead) predictions of the velocity field.

Formally, the learning task can be expressed as:

Input: (mask, Re, u(t), v(t)) ⟶ Output: (u(t+1), v(t+1)), and (mask,Re,u(t),v(t)) ⟶ (u(t+5), v(t+5)).

Here, the mask represents the spatial geometry of the domain, where a value of 0 denotes the presence of an obstacle (i.e., the cylinder), and 1 indicates the fluid region. The Reynolds number Re characterizes the flow regime, while 𝑢(𝑡) and 𝑣(𝑡) correspond to the -x and -y velocity components at time 𝑡, respectively. The dataset used in this study is derived from CFDBench, a benchmark suite designed to evaluate the generalization capabilities of neural operators in computational fluid dynamics tasks.
