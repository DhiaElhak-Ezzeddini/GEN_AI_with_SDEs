
# GEN_AI_with_SDEs

A concise, hands-on collection of labs and notebooks that explore ordinary differential equations (ODEs), stochastic differential equations (SDEs), and their applications to generative modeling. The materials build from numerical simulation of ODEs/SDEs to flow matching, score matching, and a conditional image generative model for MNIST using classifier-free guidance and a U-Net.

This repository contains three self-contained labs (Jupyter notebooks):

- `lab_one.ipynb` — Introduction to ODEs and SDEs, numerical integrators (Euler and Euler–Maruyama), and visualizations (Brownian motion, Ornstein–Uhlenbeck, Langevin dynamics). Useful utilities for simulation and plotting are included.
- `lab_two.ipynb` — Flow matching and score matching for 2D toy distributions. Implements Gaussian and linear conditional probability paths, conditional/vector-field trainers, and experiments showing how learned vector fields and scores transform distributions.
- `lab_three.ipynb` — Conditional generative modeling on MNIST. Extends the previous labs to images, implements classifier-free guidance (CFG), builds a U-Net-based conditional vector field, and demonstrates conditional generation of digits.

Table of contents
-----------------

- Project highlights
- Files and structure
- Quick start (install & run)
- Notebooks (detailed summaries)
- Implementation notes & reproducibility
- Tips, caveats and expected outputs
- Contributing, license & contact

Project highlights
------------------

- Hands-on implementations of numerical ODE/SDE simulators, including Euler and Euler–Maruyama schemes.
- Pedagogical examples: Brownian motion, Ornstein–Uhlenbeck process, Langevin dynamics.
- Probabilistic constructions: Gaussian conditional probability paths and linear conditional paths.
- Learning objectives: conditional flow matching and conditional score matching.
- A conditional image generator for MNIST using classifier-free guidance and a U-Net architecture.

Files and structure
-------------------

Top-level files:

- `lab_one.ipynb` — Lab 1 notebook (ODEs & SDEs, simulators, toy visualizations).
- `lab_two.ipynb` — Lab 2 notebook (flow matching, conditional paths, training code for 2D experiments).
- `lab_three.ipynb` — Lab 3 notebook (MNIST conditional generation, U-Net, CFG training & sampling).
- `data/` — expected place for datasets (the notebooks use a small MNIST fetcher and assume `./data` is available); a local copy of MNIST raw files may be present.
- `dynamics_animation.mp4` — optional animation output produced by lab one.

Quick start
-----------

1) Recommended Python environment

- Python 3.10+ (the included virtual environment shows Python 3.11 in `myenv/`).
- Install dependencies into a virtual environment. The notebooks rely on common scientific Python packages: numpy, matplotlib, torch (PyTorch), torchvision, seaborn, tqdm, scikit-learn, and celluloid (optional for animations).

Suggested minimal pip install (run inside a virtualenv):

	pip install -r requirements.txt


Notes:
- GPU support is optional but recommended for the MNIST U-Net training (PyTorch will use CUDA if available).
- For the animation cells, `ffmpeg` is required (install via your system package manager or conda-forge).

2) Open and run the notebooks

- Launch Jupyter in the repository root:

	jupyter lab

- Open `lab_one.ipynb`, `lab_two.ipynb`, or `lab_three.ipynb` and run cells in order. Each notebook is mostly self-contained and includes helper functions and examples.

Notebook summaries
------------------

lab_one.ipynb — "Simulating ODEs and SDEs"
- Introduces mathematical objects (vector fields, drift/diffusion).
- Implements abstract base classes for ODE/SDE, and Simulator with Euler and Euler–Maruyama integrators.
- Examples and exercises: Brownian motion, Ornstein–Uhlenbeck (OU) process, rescaling experiments.
- Langevin dynamics for transforming distributions; helper classes `Density` and `Sampleable`, plus Gaussian and GaussianMixture toy distributions.
- Visualization utilities and (optional) animation support (requires ffmpeg).

lab_two.ipynb — "Flow Matching and Score Matching"
- Extends the toy-problem setup to conditional probability paths.
- Implements Gaussian conditional paths (alpha, beta schedules) and linear conditional paths.
- Derives conditional vector fields and conditional scores for Gaussian paths.
- Implements Monte-Carlo training objectives for conditional flow matching (CFM) and conditional score matching (CSM) and trains small MLPs for the vector field and score.
- Experiments compare learned marginal flows and scores to analytical ground-truth behavior.

lab_three.ipynb — "A Conditional Generative Model for Images"
- Adapts the conditional probability path framework to image data (MNIST, resized to 32×32).
- Introduces classifier-free guidance (CFG) and a training recipe for CFG via label dropout (probability eta).
- Builds a U-Net based ConditionalVectorField (with Fourier time embeddings and label embeddings) tailored to 32×32 MNIST images.
- Demonstrates conditional sampling with different guidance scales and shows how guidance affects perceptual quality.

Implementation notes & reproducibility
-----------------------------------

- The notebooks include the full code for experiments. Most functions/classes are defined inline in the notebooks, so running cells top-to-bottom reproduces the experiments.
- Data: `lab_three.ipynb` uses torchvision.datasets.MNIST and downloads into `./data` by default; make sure your environment can access the internet or pre-populate `data/` if offline.
- GPU: when available, the notebooks set device = torch.device('cuda' if available else 'cpu'). Use an up-to-date PyTorch installation matching your CUDA version.
- Long-running training: some training loops (flow matching, score matching, U-Net) may take several minutes to hours depending on compute. The notebooks include sensible default epochs and batch sizes that you can reduce for faster, smaller experiments.

Tips, caveats and expected outputs
---------------------------------

- Numerical stability: Langevin-style SDE terms may explode near boundary times (e.g., beta(t) → 0). The notebooks note common fixes (e.g., choose sigma_t proportional to beta_t) and warn about large guidance or noise values.
- Discretization: The Euler and Euler–Maruyama integrators used in the labs are pedagogical and not intended for production-level training; reduce step size or use more advanced solvers for better stability.
- Classifier-Free Guidance: increasing guidance scale often improves visual clarity but can reduce diversity and introduce artifacts. Try multiple w (e.g., 1.0, 3.0, 5.0).

Contributing, license & contact
-------------------------------

- This repository is organized as teaching labs. If you want to propose changes, open an issue or submit a pull request describing the suggested improvement.
- If the authors/maintainers listed in the notebooks are relevant, the notebooks include the contact emails `erives@mit.edu` and `phold@mit.edu` used in the original material.
- License: No license file is included in the repository. If you plan to reuse or redistribute code, please add a suitable license (e.g., MIT, Apache 2.0) or ask the repository owner for guidance.

Acknowledgements
----------------

These labs appear to be derived from course/lecture material (authors referenced inside the notebooks). They provide an excellent guided path from basic ODE/SDE simulation to modern conditional generative modeling techniques.

Requirements coverage
---------------------

- Read all three notebooks and extract their purpose: Done.
- Produce a professional, clean README describing the repository, notebooks, setup, and usage: Done (this file).

If you'd like, I can also:

- Generate a pinned `requirements.txt` with versions inferred from the notebooks' imports.
- Add short runnable examples or a small Makefile/task that launches the three notebooks.
- Add a LICENSE and contributor guide.

If you want one of those, tell me which and I'll add it next.
