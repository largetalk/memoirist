

RNN:


.. math::
  h = \Phi(XW + b)

.. math::
  h_{t} = \Phi(X_{t}W_{xh} + h_{t-1}W_{hh} + b_{h})

.. math::
  \hat{y}_{t} = softmax(h_{t}W_{hy} + b_{y})

LSTM:

.. math::
  g_{t} = tanh(X_{t}W_{xg} + h_{t-1}W_{hg} + b_{g})

.. math::
  i_{t} = \sigma(X_{t}W_{xi} + h_{t-1}W_{hi} + b_{i})

.. math::
  f_{t} = \sigma(X_{t}W_{xf} + h_{t-1}W_{hf} + b_{f})

.. math::
  o_{t} = \sigma(X_{t}W_{xo} + h_{t-1}W_{ho} + b_{o})

.. math::
  c_{t} = f_{t} \odot c_{t-1} + i_{t} \odot g_{t}

.. math::
  h_{t} = o_{t} \odot tanh(c_{t})

.. math::
  \sigma(X) = [\frac{1}{1+exp(-x_{1})},...,\frac{1}{1+exp(-x_{k})}]^{T}

.. math::
  tanh(X) = [\frac{1-exp(-2x_{1})}{1+exp(-2x_{1})},...,\frac{1-exp(-2x_{k})}{1+exp(-2x_{k})}]^{T}

GRU:

.. math::
  z_{t} = \sigma(X_{t}W_{xz} + h_{t-1}W_{hz} + b_{z})

.. math::
  r_{t} = \sigma(X_{t}W_{xr} + h_{t-1}W_{hr} + b_{r})

.. math::
  g_{t} = tanh(X_{t}W_{xh} + (r_{t} \odot h_{t-1})W_{hh} + b_{h})

.. math::
  h_{t} = z_{t} \odot h_{t-1} + (1-z_{t}) \odot g_{t}
