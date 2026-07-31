# Technical Deep-Dive: FedPKDA (AAAI 2026)**Presenter:** [Your Name]

**Topic:** Personalized Federated Learning with Privacy-Preserving Knowledge Dynamic Alignment

---

## 📌 Executive SummaryFedPKDA solves the two biggest flaws in Personalized Federated Learning (PFL): **Privacy Leakage** (from gradient sharing) and **Local Representation Bias** (overfitting to local data). It achieves State-of-the-Art (SOTA) accuracy on non-IID datasets while maintaining rigorous differential privacy guarantees

---

## 🎯 Deep-Dive 1: The Core Tension (Privacy vs. Utility)

### The Problem with Existing PFL***Gradient Leakage:** Sharing model weights or gradients allows adversaries to run Deep Leakage from Gradients (DLG) attacks, completely recovering raw user images.* **The Noise Penalty:** Applying standard Differential Privacy (DP) noise to gradients causes severe model degradation

### The FedPKDA Solution: Noisy Prototypes***What is a Prototype?** A prototype ($p_i^k$) is simply the mean feature vector of a specific class $k$ computed by client $i$. It is compact and abstracts away individual data points.* **The Defense Mechanism:** Clients apply Local Clipping to bound sensitivity, then inject Laplacian Noise

  $ \tilde{p}_i^k = p_i^k + \xi, \quad \xi \sim \text{Lap}(0, b) $
  ***Key Takeaway:** By sharing *perturbed semantic averages* instead of instance-level gradients, FedPKDA drastically shrinks the attack surface.

---

## 🎯 Deep-Dive 2: Server-Side Geometry Filtering (Handling the Noise)

### The Problem: Naive Averaging Fails* If the server simply calculates a standard mean of these noisy prototypes, the residual noise from limited client sampling creates fluctuating errors, corrupting the global model

### The Mechanism: Mahalanobis Distance Aggregation1. **Clustering:** The server uses K-Means to identify the geometric center ($\mu_k$) of all uploaded client prototypes for class $k$.2. **Distance Calculation:** It computes the Mahalanobis Distance ($d_i^k$) for each client. This measures how many standard deviations away a noisy prototype is from the cluster center, accounting for latent space covariance.3. **Inverse Weighting:** Aggregation weights are inversely proportional to this distance

  $ w_i^k \propto \frac{1}{d_i^k} $

### Key Takeaway*Highly distorted or noisy prototypes receive a massive distance score, resulting in a near-zero weight.* The server mathematically filters out noise spikes and reconstructs a clean, robust global prototype ($p_G^k$) without seeing unperturbed data

---

## 🎯 Deep-Dive 3: Client-Side Evolutionary Learning (Dynamic Alignment)

### The Problem: Over-Personalization* Typical PFL models perform exceptionally well on local data but completely fail when exposed to unseen test samples because they isolate themselves from global knowledge

### The Mechanism: Dual-Alignment LossFedPKDA forces the local feature extractor to minimize distance to both local and global prototypes via a time-dependent activation function $\phi(t)$ (where $t$ progresses from 0 to 1 over communication rounds)

$ \mathcal{L}_{align} = \omega_{local} \cdot (1 - \phi(t)) \cdot \mathcal{L}_{LA} + \omega_{global} \cdot \phi(t) \cdot \mathcal{L}_{GA} $

### The Training Evolution* **Phase 1: Early Rounds ($\phi(t) \to 0$)**

* Focuses heavily on **Local Alignment Loss ($\mathcal{L}_{LA}$)**.
* *Goal:* Stabilize individual local feature spaces and smooth out discrepancies.* **Phase 2: Late Rounds ($\phi(t) \to 1$)**
* Focuses heavily on **Global Alignment Loss ($\mathcal{L}_{GA}$)**.
* *Goal:* Pull local models toward the clean, aggregated global prototype to absorb cross-client consensus.

### Key Takeaway* This dynamic shift acts as a bridge. It allows clients to first ground themselves in local context before safely graduating to a generalized global understanding, eliminating representation bias

## 📊 Proof of Concept: Empirical Highlights***Accuracy:** Beats 12 SOTA baselines. On CIFAR-100 (highly non-IID, $\beta=0.3$), FedPKDA scores **44.91%** accuracy—outperforming the runner-up FedAS by **4.23%**.* **Privacy:** Under DLG reconstruction attacks, FedPKDA holds a Peak Signal-to-Noise Ratio (PSNR) of ~6.9. Visually, the reconstructed images look like pure random static, proving the attack is thwarted

---

## 💬 Discussion & Q&A Openers*How does the calculation of the covariance matrix on the server handle non-IID data without leaking cross-client identity?* What is the computational overhead on the client side when computing the Fisher Information Matrix (FIM) for model aggregation?
