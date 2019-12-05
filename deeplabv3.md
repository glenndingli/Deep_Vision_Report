# DeepLabV3

## Experiment Setup

## Experiment Results
| Input Data | Fusion Method               | weighted loss | mIOU   | Background | Roof   | Facade | Roof Equipment | Car    | Ground Equipment |
|------------|-----------------------------|---------------|--------|------------|--------|--------|----------------|--------|------------------|
| RGB        | N/A                         | N             | 0.4549 | 0.8674     | 0.8535 | 0.6819 | 0.0282         | 0.3268 | 0.1348           |
| RGBT       | Input Fusion                | N             | 0.5130 | 0.8817     | 0.8761 | 0.7605 | 0.0302         | 0.3018 | 0.3113           |
| RGBT       | Input Fusion                | Y             | 0.5195 | 0.8977     | 0.9058 | 0.7890 | 0.0319         | 0.3449 | 0.2573           |
| RGBT       | Input Fusion + More Filters | N             | 0.5227 | 0.9056     | 0.9179 | 0.8155 | 0.0359         | 0.3394 | 0.2348           |
| RBGT       | Input Fusion + More Filters | Y             | 0.5238 | 0.8996     | 0.9212 | 0.8129 | 0.0384         | 0.3369 | 0.2158           |
| RGBT       | Feature Fusion              | N             | 0.5376 | 0.9065     | 0.9311 | 0.8149 | 0.0247         | 0.3378 | 0.2799           |
| RGBT       |                             |               |        |            |        |        |                |        |                  |
| RGBT       |                             |               |        |            |        |        |                |        |                  |