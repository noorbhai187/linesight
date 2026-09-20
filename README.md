# LineSight

Visual inspection & defect root-cause assistant. NeuraX Hackathon 3.0, Domain 2 (AI in Industry & Automation).

**Live page: https://noorbhai187.github.io/linesight/**

Not another image classifier. The system ties a defective unit to the process condition that produced it, and to what that costs per hour. Software only, simulation based: no live camera, PLC or machine control anywhere, and every process and profitability output is advisory.

## Measured on held-out data

| what | result |
| --- | --- |
| accept / reject | 99.71% at AUC 0.9995 |
| false ACCEPT | 0.00% |
| false reject | 1.46% |
| defect family | 99.04% over five classes |
| localization | mask IoU 0.726, box IoU 0.802 |
| unseen defect types | 79.3% by distance, against 48.9% by classifier confidence |
| rotation, brightness | +0.0000 accuracy change |
| sensor noise | -0.1937, the real weakness, reported rather than buried |
| line constraint | Cell 1 at 86.7% while all four presses idle at 43.5% |
| root cause | 3 of 4 injected drivers recovered at rank 1, mean rank 1.25 of 29 |

## How

Gaussian background subtraction leaves a residual; twelve physical measurements of that residual feed a Mahalanobis distance against a model of normal only, then a calibrated random forest for the defect family. Anomaly first, classification second, which is what lets it say "I don't know" instead of guessing. No pretrained weights and no GPU: numpy, scipy and scikit-learn.

The whole pipeline also runs in the browser. index.html is one self-contained file carrying the reported model itself, checked against Python fixtures so the page cannot quote one accuracy and run a different model. Drop in your own unit image or production CSV and it analyses that file, offline, with no server anywhere.
