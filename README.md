# Mayo Clinic: STRIP AI

---

**Objective:**

Predict a probability for each of the two blood clot etiology classes in acute stroke. 2 types:
* Cardioembolic (CE)
* Large Artery Atherosclerosis (LAA)

---

**Methodology:**

**1. Pre-processing:**
  * Imbalanced classes (CE and LAA): -> downsampling or upsampling or data augmentation, synthetic oversampling such as re-sampling from the data for a minority class
  * Whole Slide Images (WSIs): large (multiple GB in size) -> break up into tiles/patches to further analysis. (preprocessing) 
      * WSI are often labeled at the slide level, not at the tile level -> annotate tile level, derive the ground truth labels of individual tiles from slide-wide label data

  -> Weakly supervised, borrowed from multiple instance learning: assume that a slide-level label implies the presence of at least k instances (less robust) or strongly supervised? Do the tiles have representative features of the slide-level label
  
  * Staining & imaging protocols: affect generalization across institutions. -> data augmentation
  * Artifacts due to slide preparation, large portions of background: need to be removed to speed up the training process and potentially improve performance-> deep tissue detector (artifacts, background, tissue). Tissue, artifact, and background filtering using triangle algorithm, Otsu’s method, and PathML’s deep tissue detector
  * Overfitting: 1% public test set, occurrences of pathological features may be ‘rare’ -> data augmentation, post-processing

**2. Neural network architectures:**

**Classification:**
* Strongly supervised: ResNet, DenseNet, SqueezeNet -> contain large number of layers and millions of parameters. Without a sufficient large dataset, such large architectures are susceptible to overfitting to the training data
* Weakly supervised: CHOWDER-aim to make more efficient use of the data? Multiple instance learning?

**3. Transfer learning**: reduce the risk of overfitting, useful when training dataset is small

**4. Training, inference and validation:**

**Training + Validation:**
  * 70-15-15 split (training, validation, and testing)
  * WSIs: important that the partition between training, validation, and test sets is at the patient-level 
      * Multiple WSIs for a single patient, all of them should be partitioned into the same set. Likewise, all tiles from the same slide should be part of the same set-> partition patients between the three sets is a good first step
  * Inference:
      * Metric: LogLoss
      * Post-processing
      * Visualize inference results








