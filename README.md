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

- finding the single pixel, which is the "center" of a solid ellipse on a noisy background: [notebook](ellipse/Ellipses%20Experiments.ipynb#Detecting-the-position-of-a-single-ellipse-on-a-noisy-background);
- finding the single pixel, which is the "center" of a hollow ellipse ("doughnut") on a noisy background: [notebook](ellipse/Ellipses%20Experiments.ipynb#Hollow-Ellipse);
- finding the single pixel on each frame tracking the position of the "center" of a hollow "continious" (present in every frame) ellipse on a noisy background including other random (appearing and disappearing on each frame) hollow-ellipses: [notebook](ellipse/Ellipses%20Experiments.ipynb#Moving-ellipses).

# Small data-sets

Synthetic data used in the experiments was intentially very limited:

- `300` frames with solid ellipse;
- `300` frames with hollow ellipse;
- `30` movies with `250` frames each.

This presented an interesting challenge of finding models with very low capacity.

The best results were achieved using transfer learning

# Transfer learning

Steps:

1. Trained `207`-parameters model through `100` epochs for detecting solid ellipses, and achieved mean distance from the label of `2.6` pixels. Verified that model achieves similar performance from different initializations.
2. Trained a model with `516`-parameters through `100` epochs to detect hollow ellipse, and achieved mean distance from the label of `13.5` pixels. This `516`-parameters model incorporates the `207`-parameters model from the first step, which was frozen initially.  Verified that model achieves similar performance from different initializations of the trainable part. Model was then fine-tuned with all parameters unfrozen to achieve `11.8`-pixels mean distance.
3. Trained a model with `2126` parameters (of these, `516` were carried over from step 2 and were initially frozen) through `100` epochs to detect moving hollow ellipses among the noise, which also consisted of hollow ellipses. Achieved mean distance of `16.2`. Fine-tuning didn't improve results.
4. Trained the same model as in step 3 (`2126`) from scratch: with all parameters unfrozen and without using any pre-trained parameters. After `1000` epochs this model demonstrated to be unstable in training. Its best result was `24.4`-pixels mean distance.

Conclusion: transfer learning allowed to achieve useful results with extremely small data-set and very small training time.
