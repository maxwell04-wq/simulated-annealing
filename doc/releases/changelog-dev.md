:orphan:

# Release 0.25.0-dev (development release)

<h3>New features since last release</h3>

* Added the new optimizer, `qml.SPSAOptimizer` that implements the simultaneous
  perturbation stochastic approximation method based on
  [An Overview of the Simultaneous Perturbation Method for Efficient Optimization](https://www.jhuapl.edu/SPSA/PDF-SPSA/Spall_An_Overview.PDF).
  [(#2661)](https://github.com/PennyLaneAI/pennylane/pull/2661)

  It is a suitable optimizer for cost functions whose evaluation may involve
  noise, as optimization with SPSA may significantly decrease the number of
  quantum executions for the entire optimization.

  ```pycon
  >>> dev = qml.device("default.qubit", wires=1)
  >>> def circuit(params):
  ...     qml.RX(params[0], wires=0)
  ...     qml.RY(params[1], wires=0)
  >>> coeffs = [1, 1]
  >>> obs = [qml.PauliX(0), qml.PauliZ(0)]
  >>> H = qml.Hamiltonian(coeffs, obs)
  >>> @qml.qnode(dev)
  ... def cost(params):
  ...     circuit(params)
  ...     return qml.expval(H)
  >>> params = np.random.normal(0, np.pi, (2), requires_grad=True)
  >>> print(params)
  [-5.92774911 -4.26420843]
  >>> print(cost(params))
  0.43866366253270167
  >>> max_iterations = 50
  >>> opt = qml.SPSAOptimizer(maxiter=max_iterations)
  >>> for _ in range(max_iterations):
  ...     params, energy = opt.step_and_cost(cost, params)
  >>> print(params)
  [-6.21193761 -2.99360548]
  >>> print(energy)
  -1.1258709813834058
  ```

<h3>Improvements</h3>

* Adds a new function to compare operators. `qml.equal` can be used to compare equality of parametric operators taking into account their interfaces and trainability.
  [(#2651)](https://github.com/PennyLaneAI/pennylane/pull/2651)

<h3>Breaking changes</h3>

<h3>Deprecations</h3>

<h3>Documentation</h3>

<h3>Contributors</h3>

This release contains contributions from (in alphabetical order):

Ankit Khandelwal, Ixchel Meza Chavez, Moritz Willmann
