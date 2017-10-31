# Ellipses Experiments

The structure of all experiments is very similar:

- generate a fully synthetic data-set, including:
  - ground truth (positions of the ellipses in pixel-space);
  - noisy observations (background with random pixels plus ellipses);
- split data-set into training and validation;
- define an **intuitive** "upstream" criterion (non-differentiable: argmax over predictions to determine the most likely position of the ellipse);
- define parameterized NN-based model to find the ellipses;
- perform hyperparameter search;
- visualize results.

# Experiments

- finding the single pixel, which is the "center" of a solid ellipse on a noisy background: `ellipse`;
- finding the single pixel, which is the "center" of a hollow ellipse ("doughnut") on a noisy background: `hollow-ellipse`;
- finding the single pixel on each frame tracking the position of the "center" of a hollow "continious" (present in every frame) ellipse on a noisy background including other random (appearing and disappearing on each frame) hollow-ellipses: `moving-hollow-ellipse`.
