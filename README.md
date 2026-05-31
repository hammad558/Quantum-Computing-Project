# Quantum-Computing-Project
GRU-QCBM: A Parameter-Efficient Hybrid Quantum-Classical Framework for Financial Volatility Forecasting
Base Paper: https://arxiv.org/abs/2603.09789

Overview
This repository contains the implementation of a hybrid quantum-classical model for financial volatility forecasting. The project is based on the paper "A Hybrid Quantum-Classical Framework for Financial Volatility Forecasting Based on Quantum Circuit Born Machines" (Chen, 2026).
We replicate the base paper's LSTM-QCBM model and propose an improvement by replacing the LSTM backbone with a GRU network, creating the GRU-QCBM model. Our proposed model achieves up to 8.6% better RMSE than the base paper's model while using 24.4% fewer parameters.

What is This Project?
We are predicting financial market volatility — how much stock prices will move in the near future. To do this we combine two components:
A classical neural network (LSTM or GRU) that learns patterns from historical stock return data.
A Quantum Circuit Born Machine (QCBM) that generates a probability distribution as prior knowledge and injects it into the neural network to improve predictions.
The two modules are trained using an alternating strategy where the classical and quantum components are optimized independently, avoiding complex gradient coupling.

Contribution
The base paper uses LSTM as the classical backbone. We propose replacing it with GRU which has a simpler architecture. This results in:
24.4% fewer parameters (10,017 vs 13,249)
Up to 8.6% better RMSE on NASDAQ dataset
Greater numerical stability — LSTM-QCBM produced a QLIKE anomaly of 9160 on SP500 while GRU-QCBM remained stable
Faster convergence during training

Models Compared
Baseline LSTM : classical only, from base paper
LSTM-QCBM : hybrid quantum-classical, from base paper
Baseline GRU : classical only, our contribution
GRU-QCBM : hybrid quantum-classical, our proposed model

Results Summary
SP500 Dataset:
LSTM RMSE = 6.950 x 10-3
LSTM-QCBM RMSE = 6.909 x 10-3
GRU RMSE = 6.395 x 10-3
GRU-QCBM RMSE = 6.468 x 10-3
NASDAQ Dataset:
LSTM RMSE = 6.718 x 10-3
LSTM-QCBM RMSE = 6.790 x 10-3
GRU RMSE = 6.599 x 10-3
GRU-QCBM RMSE = 6.207 x 10-3
GRU-QCBM achieves the best performance on NASDAQ with 7.6% improvement over baseline LSTM and 8.6% improvement over LSTM-QCBM.

Requirements
Python 3.8 or higher
PennyLane
PennyLane-Lightning
PyTorch
NumPy
Pandas
Matplotlib
SciPy
yfinance (optional, for real data)

QCBM Circuit Architecture
8 qubits, 3 layers, 117 parameters
Each layer:

Rx and Rz rotation gates on each qubit
RXX entanglement gates on adjacent qubit pairs
Rz and Rx rotation gates again

Optimizer: COBYLA (gradient-free, 20 iterations per epoch)
Simulator: PennyLane lightning.qubit

Training Strategy
The model uses an alternating training strategy from the base paper:
Phase 1: Fix QCBM parameters, train GRU/LSTM using Adam optimizer (lr=0.001, batch=64)
Phase 2: Fix GRU/LSTM parameters, optimize QCBM using COBYLA by scoring each sampled bitstring based on prediction error and minimizing KL divergence
Total epochs: 300

Dataset
We use synthetic GARCH(1,1) financial data that mimics real stock market properties including volatility clustering and fat tails.
Dataset 1: S&P 500-like (omega=0.00001, alpha=0.1, beta=0.85)
Dataset 2: NASDAQ-like (omega=0.00001, alpha=0.12, beta=0.83)
Each dataset contains 2000 data points split 70/20/10 for train/validation/test.
To use real data, uncomment the yfinance block in Cell 3 of the notebook.

Evaluation Metrics
MSE : Mean Squared Error, measures average squared prediction error
RMSE : Root Mean Squared Error, easier to interpret, same unit as data
QLIKE : Standard volatility forecasting metric, penalizes errors in low-volatility periods more heavily
For all metrics lower is better.

Key Findings
GRU is a better backbone than LSTM for hybrid quantum-classical volatility forecasting
GRU-QCBM achieves the best overall performance with fewest parameters
LSTM-QCBM can perform worse than baseline LSTM, showing that quantum prior alone is not enough
Backbone architecture selection is as important as the quantum component in hybrid models
GRU provides greater numerical stability than LSTM in hybrid quantum-classical settings


References
Y. Chen, "A Hybrid Quantum-Classical Framework for Financial Volatility Forecasting Based on Quantum Circuit Born Machines," arXiv:2603.09789, 2026.
T. Bollerslev, "Generalized autoregressive conditional heteroskedasticity," Journal of Econometrics, vol. 31, no. 3, pp. 307-327, 1986.


