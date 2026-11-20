# Neural Surrogate Modeling of the Lorenz System

A data-driven neural surrogate model for learning and reproducing the dynamics of the Lorenz-63 chaotic system.

## Overview

This project investigates the use of a neural network to approximate the vector field of the Lorenz-63 system.

A multilayer perceptron (MLP) is trained to predict the time derivatives of the system variables from their current states. The learned vector field is then integrated using a fourth-order Runge–Kutta (RK4) method to generate autonomous trajectories.

The objective is to evaluate how accurately the neural surrogate reproduces the geometric and dynamical properties of the original chaotic system.

## 1. The Lorenz System

The Lorenz-63 system is defined by:

\[
\frac{dx}{dt} = \sigma(y-x)
\]

\[
\frac{dy}{dt} = x(\rho-z)-y
\]

\[
\frac{dz}{dt} = xy-\beta z
\]

The parameters used in this project are:

| Parameter | Value |
|---|---:|
| \(\sigma\) | 10 |
| \(\rho\) | 28 |
| \(\beta\) | 8/3 |
| Time step | 0.01 |
| Integration steps | 10,000 |
| Number of trajectories | 30 |

The trajectories are generated using the fourth-order Runge–Kutta (RK4) method, starting from randomly selected initial conditions.

## 2. Dataset Preparation

Thirty independent trajectories are generated from random initial conditions sampled uniformly from the interval [0.5, 2.0] for each state variable.

The dataset is divided by trajectory:

- Training: 20 trajectories
- Validation: 4 trajectories
- Testing: 6 trajectories

The network inputs are the system states:

\[
\mathbf{x}=(x,y,z)
\]

The targets are the corresponding derivatives:

\[
\mathbf{f}(\mathbf{x})=(\dot{x},\dot{y},\dot{z})
\]

Input and target variables are standardized using statistics calculated from the training dataset only.

## 3. Neural Network Architecture

A fully connected multilayer perceptron is used to approximate the Lorenz vector field.

| Layer | Configuration |
|---|---|
| Input | 3 neurons |
| Hidden layer 1 | 128 neurons, tanh |
| Hidden layer 2 | 128 neurons, tanh |
| Output | 3 neurons |

The network is trained using:

- Optimizer: Adam
- Learning rate: 0.001
- Loss function: Mean Squared Error (MSE)
- Batch size: 256
- Maximum epochs: 100
- Early stopping based on validation loss

## 4. Neural RK4 Integration

After training, the neural network is used as a substitute for the original Lorenz vector field.

The predicted derivatives are integrated using the RK4 algorithm:

\[
\mathbf{x}_{t+\Delta t}
=
\operatorname{RK4}
\left(
\mathbf{x}_t,\mathbf{f}_{\theta}
\right)
\]

where \(\mathbf{f}_{\theta}\) denotes the learned neural vector field.

This approach allows the neural model to generate trajectories without using the original Lorenz equations during the prediction stage.

## 5. Evaluation and Dynamical Analysis

The neural surrogate is evaluated using several complementary approaches.

### One-step derivative prediction

The derivative predictions are evaluated using:

- Mean Squared Error (MSE)
- Mean Absolute Error (MAE)
- Coefficient of determination (R²)

### Attractor reconstruction

The reconstructed neural trajectories are compared with the original Lorenz attractor in three-dimensional phase space.

### Probability distributions

The probability distributions of x, y, and z are compared between the reference system and the neural surrogate.

### Autocorrelation functions

Temporal correlations are examined to assess whether the model reproduces the temporal structure of the dynamics.

### Lyapunov exponents

The largest Lyapunov exponent is estimated to investigate sensitivity to initial conditions.

A numerical method based on QR decomposition is also implemented to estimate the full Lyapunov spectrum.

### Phase-space divergence

The divergence of the learned vector field is calculated using automatic differentiation and compared with the analytical divergence of the original Lorenz system.

For the Lorenz parameters used here:

\[
\nabla\cdot\mathbf{f}=-(\sigma+1+\beta)
=-13.6667
\]

## 6. Repository Structure

```text
Lorenz-Neural-Surrogate/
│
├── README.md
│
├── notebooks/
│   └── lorenz_neural_surrogate.ipynb
│
└── figures/
    ├── training_history.png
    ├── trajectory.png
    └── lorenz_probability_distributions.png
```

## 7. Installation and Usage

### Requirements

The project uses Python and the following libraries:

- NumPy
- Pandas
- Matplotlib
- Scikit-learn
- TensorFlow

Install the required packages using:

```bash
pip install numpy pandas matplotlib scikit-learn tensorflow
```

### Running the project

1. Clone the repository:

```bash
git clone https://github.com/YOUR_USERNAME/Lorenz-Neural-Surrogate.git
```

2. Navigate to the repository:

```bash
cd Lorenz-Neural-Surrogate
```

3. Open the Jupyter Notebook:

```bash
jupyter notebook notebooks/lorenz_neural_surrogate.ipynb
```

4. Run all cells to generate the trajectories, train the neural network, and perform the dynamical analyses.

## 8. Methodological Note

The derivative targets in this project are computed from the known analytical Lorenz equations at the generated trajectory points.

Therefore, this work investigates supervised neural approximation of a known dynamical vector field. It does not claim to discover unknown governing equations solely from observational data.

## 9. Future Directions

Potential extensions include:

- Testing generalization across different Lorenz parameters.
- Evaluating longer autonomous rollouts.
- Improving the accuracy of long-term statistical predictions.
- Investigating alternative neural network architectures.
- Applying the methodology to dynamical systems with unknown governing equations.

## Author

**Azadeh Malekan**

Computational Physics | Statistical Physics | Complex Systems | Scientific Machine Learning

ORCID: [0000-0001-7929-1960](https://orcid.org/0000-0001-7929-1960)

GitHub: [azadehmalekan](https://github.com/azadehmalekan)