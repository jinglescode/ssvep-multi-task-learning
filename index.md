---
layout: default
title: Deep Multi-Task Learning for SSVEP Detection and Visual Response Mapping
---

Using multi-task learning to efficiently captures signals simultaneously from the fovea and the neighboring targets in the peripheral vision, generating a visual response map. A calibration-free user-independent solution, desirable for clinical diagnostics. A stepping stone for an objective assessment of glaucoma patients' visual field.

![figures-main-idea-big](https://user-images.githubusercontent.com/1694368/91680983-9caa4580-eb7f-11ea-8bfd-50e0705c141d.jpg)

## Abstract

Glaucoma is an eye disease that occurs without the onset of symptoms at initial, and late diagnosis results in irreversible degeneration of retinal ganglion cells. Standard automated perimetry is the gold standard for assessing glaucoma; however, the examination is subjective, where responses can fluctuate each time the test is performed, significantly confounding the test's interpretation. In this study, we present our approach that aims to provide a rapid point-of-care diagnostics for glaucoma patients by eliminating the cognitive aspect in existing visual field assessment. Unlike existing methods that mostly report the foveal target detection's accuracy, we employed a multi-task learning architecture that efficiently captures signals simultaneously from the fovea and the neighboring targets in the peripheral vision, generating a visual response map. Furthermore, we designed a multi-task learning module that learns multiple tasks in parallel efficiently. We evaluated our model classification on a 40-classes dataset, with yields 92% and 95% in accuracy and F1 score, respectively. Our model is able to perform on a calibration-free user-independent scenario, which is desirable for clinical diagnostics. Our proposed approach could be a stepping stone for an objective assessment of glaucoma patients' visual field.

## Results

Compare our model’s classification accuracy against against CCA-based methods. At 1-second data length, CCA, [FBCCA](https://iopscience.iop.org/article/10.1088/1741-2560/12/4/046008/meta), and [TRCA](https://www.sciencedirect.com/science/article/pii/S105381191200852X) yielded approximately 59%, 72%, and 88%, respectively. Using our proposed method, we can identify the foveal’s target frequency effectively. Our model’s average cross-validation accuracy for each subject is compared against other methods are as shown.

![figures-acc-by-subjects](https://user-images.githubusercontent.com/1694368/92562604-d29aa880-f2a8-11ea-9d4d-9d99236e88d7.png)

We used the MTL approach to learn all 40-tasks, thus enabling a unified system to predict all target frequencies simultaneously. As such, this enables us to visualize what the user has seen with a visual response map. We selected 6-targets that are located around the center of the screen as shown. This is the region of interest for visual field assessment in our future work. We explored further on the models’ ability to diagnose users who are not experienced in SSVEP-based BCIs. Specifically, we trained our model with the first eight participants from the dataset and validated the remaining 27 subjects. Each map is the average response of the 27 subjects for a target frequency: a darker shade denotes a stronger signal being detected. As the subjects recruited in the HS-SSVEP dataset have healthy eyesight and considering that all the stimuli are flickering simultaneously in a visual spelling task, expected results will be the intense signal concentration at the target frequency.

![figures-test_visuals](https://user-images.githubusercontent.com/1694368/91679148-10495400-eb7a-11ea-8f77-27a133203dd0.png)

## Model

The network is composed of four convolution blocks and one convolution layer. Each convolution block consists of a convolution layer, a batch normalization, and an exponential linear unit. The fifth and final convolution layer is a multi-task learning block, where it learns to differentiate multiple SSVEP target frequencies.

![figures-model](https://user-images.githubusercontent.com/1694368/91679200-2ce58c00-eb7a-11ea-82a3-1df6ef6aee24.png)

We designed an MTL block that performs convolutions by groups, By defining t groups, we are essentially performing separate convolutions within a single convolution layer. This allows us to use the same model architecture and scale to any number of tasks effectively. We expanded the output from previous convolution block by concatenation and employ a group-wise convolution to split the input into different groups of weights, each responsible for learning each target frequency separately. This MTL block allowed us to train multiple tasks in parallel efficiently on a single GPU. We can dynamically tweak our MTL model for any number of tasks, which is potentially suitable for other MTL applications.

## Conclusion

Traditionally, studies on steady-state visual-evoked potential (SSVEP) were focused on detecting the target stimulating frequency in the foveal while the other flicking stimuli are regarded as interference. Our study presented an end-to-end multi-task learning approach to detect the responses from the peripheral vision. Furthermore, we designed multi-task learning (MTL) block that learns multiple tasks in parallel efficiently, and we can dynamically tweak our MTL model to the number of tasks.

Our results show that our MTL model can perform in single target SSVEP classification, which could be potentially useful in other SSVEP applications and studies. We observed that the MTL approach is able to learn each target frequency separately, which allowed us to yield a map of the patient’s visible field of vision.

In view of recent events during disease outbreak and pandemics where non-essential hospital appointments are recommended to be kept to a minimum, this assessment method can reduce the number of tests needed, thus minimizing any unnecessary or additional tests. In essence, this study enables our future work to potentially assess glaucoma patients’ visual field to detect peripheral vision loss. To improve the reliability of the assessment results, utilizing SSVEP could eliminate a patient’s ability to carry out the procedure and variability of the patient’s mental state. Assessment time could be cut down by detecting multiple SSVEP targets at once and generating a visual response map. Our approach could be potentially suitable for providing a rapid point-of-care diagnostics for glaucoma patients.

## Citation

If you use the model in your research and found it helpful, please cite the following paper:

```
pending...
```

## Codes

#### Model

<script src="https://gist.github.com/jinglescode/b99c0c3c31af5c217c6305cacb7e16b0.js"></script>

#### Experiment

<script src="https://gist.github.com/jinglescode/3d74fe165dee44e2f794e56ce0a2a958.js"></script>