---
tags: 深度学习 PyTorch Python
---
# Optimizer

### General

$$g_t = \nabla loss = \frac{\partial loss}{\partial (w_t)}$$

$$\text{The first-order momentum:}m_t,\\\text{The second-order momentum:}V_t$$

$$\eta_t = lr\frac{m_t}{\sqrt{V_t}}$$

$$w_{t+1} = w_t - \eta_t = w_t - lr * \frac{m_t}{\sqrt{V_t}}$$

### SGD - Stochastic gradient descent

$$w_{t+1} = w_t - lr\frac{\partial loss}{\partial w_t}$$

### SGDM - Stochastic gradient descent With Momentum

$$m_t = \beta m_{t-1} + (1 - \beta) g_t, V_t = 1\\n_t = lr * \frac{m_t}{\sqrt{V_t}} = lr * m_t\\w_{t+1} = w_t - lr(\beta m_{t-1} + (1 - \beta) g_t)$$

### Adagrad

$$m_t = g_t, V_t = V_{t-1} + g_t^2 = \sum_{\tau = 1}^t g_\tau^2\\
w_{t+1} = w_t - \eta_t = w_t - lr\frac{g_t}{\sqrt{\sum_{\tau = 1}^t g_\tau^2}}$$

### RMSProp

$$m_t = g_t, V_t = \beta V_{t-1} + (1-\beta) g_t^2\\w_{t+1} = w_t - lr\frac{g_t}{\sqrt{\beta V_{t-1} + (1-\beta)g_t^2}}$$

### Adam

$$m_t = \beta_1 m_{t-1} + (1-\beta_1)g_t\\\hat m_t = \frac{m_t}{1-\beta_1^t}\\V_t = \beta_2 V_{step-1} + (1 - \beta_2) g_t^2\\\hat V_t = \frac{V_t}{1-\beta_2^t}\\n_t = lr\frac{\hat m_t}{\sqrt{\hat V_t}}\\w_{t+1} = w_t - lr\frac{\hat m_t}{\sqrt{\hat V_t}}$$