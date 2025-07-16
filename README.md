## Adversarial Attacks on Robust Vision-Language Models

This repository contains a single Google Colab notebook, `adversarial_attack_on_vlm_applied_to_google_lens.ipynb`, which demonstrates and analyzes adversarial attacks against the **FARE** (Fooling-Aware Robust-to-Ensembling) CLIP model. The FARE model, introduced in the ICML 2024 paper cited below, is specifically fine-tuned for adversarial robustness. Our notebook explores the practical limits of this robustness by implementing two distinct attack strategies and testing their real-world transferability against Google Lens.

The goal of the attacks is to generate a small, human-imperceptible perturbation for an image that causes the model to misclassify it according to a chosen text prompt (e.g., making a "photo of a dresser" be identified as a "photo of a dog").

### Key Concepts Demonstrated in the Notebook

The notebook implements two adversarial attack methods:

1.  **Global (Unmasked) PGD Attack**: A classic Projected Gradient Descent (PGD) attack where the adversarial noise is applied across the entire image. This implementation is particularly effective as it optimizes the full-resolution image and differentiates through the model's standard pre-processing pipeline (resizing and cropping).

2.  **Targeted (Masked) PGD Attack**: A more sophisticated and stealthy attack. It first uses the CLIPSeg model to generate a segmentation mask of the main foreground object. The PGD attack is then constrained to only perturb the **background**, leaving the primary subject of the image completely untouched and visually pristine.

### How to Use

The entire project is self-contained within the provided notebook.

1.  Open `adversarial_attack_on_vlm_applied_to_google_lens.ipynb` in Google Colab.
2.  Run the cells sequentially. The notebook will guide you through installing dependencies and loading the necessary models.
3.  You will be prompted to upload an image to attack.
4.  Modify the `original_text` and `adversary_text` variables in the code to define your specific attack scenario.
5.  The notebook will execute both the unmasked and masked attacks, display the results, and allow you to download the generated adversarial images.

### Key Findings & Future Work

Our experiments testing the generated images on Google Lens revealed critical insights into attacking real-world vision systems:

*   **Unmasked Attack Success**: The global attack successfully transferred to Google Lens, demonstrating that the adversarial features were general enough to fool a different, black-box model.
*   **Masked Attack's Pipeline Challenge**: The stealthy masked attack initially failed on Google Lens. We concluded this was due to Google's pre-processing pipeline, which likely performed object detection, cropped to the *clean* foreground object, and thus discarded the adversarial background.
*   **Pipeline-Aware Attacks**: We successfully bypassed this defense by "zooming out" of the image, forcing the object detector's bounding box to include the adversarial background. This highlights a key future direction: developing **pipeline-aware attacks** that have a dual objective of fooling the final classifier while also manipulating upstream components like object detection to ensure the adversarial payload is delivered.

### Acknowledgement & Reference

The FARE model and the foundational concepts of robustifying Vision-Language Models are from the following work. This notebook serves as a practical exploration of the attack surfaces that persist even in such robust models.

@article{schlarmann2024robustclip,
    title={Robust CLIP: Unsupervised Adversarial Fine-Tuning of Vision Embeddings for Robust Large Vision-Language Models}, 
    author={Christian Schlarmann and Naman Deep Singh and Francesco Croce and Matthias Hein},
    year={2024},
    journal={ICML}
}
