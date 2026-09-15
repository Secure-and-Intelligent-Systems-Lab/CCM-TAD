<h1 align="center">Cluster-Aware Causal Mixer for Online Anomaly Detection in Multivariate Time Series</h1>

<div align="center">

<p><a href="https://arxiv.org/pdf/2506.00188"><strong>Full Paper (ARXIV)</strong></a></p>
<hr style="border: 1px solid  #256ae2 ;">
</div>

Please cite our work if it sparks an idea, supports your research, or finds its way into your implementation.

```bibtex
@article{murad2025cluster,
  title={Cluster-Aware Causal Mixer for Online Anomaly Detection in Multivariate Time Series},
  author={Murad, Md Mahmuddun Nabi and Yilmaz, Yasin},
  journal={arXiv preprint arXiv:2506.00188},
  year={2025}
}
```
## 🔄 Updates
- **[Sep 2026]** To expedite the reseach, uploaded all datasets in the <a href="https://usf.box.com/s/rov7qj1234o6yxzgnj41mpj0560bvtqe"><strong>USF-BOX</strong></a>
- **[June 2026]** 🔥🔥🔥 Accepted (Poster) <a href="https://openreview.net/forum?id=R6JV2WOftz"><strong>(ICML-2026)</strong></a>

## Get started
Follow these steps to get started:
### 1. Install Requirements
Install Python 3.10 and the necessary dependencies.

```bash
pip install -r requirements.txt
```
### 2. Datasets
For convenience, all datasets used in this project are available in the
[USF Box folder](https://usf.box.com/s/rov7qj1234o6yxzgnj41mpj0560bvtqe).
Please cite the original creators of the datasets. You can also download them from the following links:

<b>SWAT</b>
- Download the files ```train.npy```, ```test.npz``` from the following link https://github.com/yuesuoqingqiu/SensitiveHUE/tree/master/data/SWaT
- Keep the downloaded files inside the directory: ```./data/SWAT/```

<b>PSM</b>
- Download the files ```test.csv```, ```test_label.csv```, and ```train.csv``` from the following link https://github.com/eBay/RANSynCoders/tree/main/data
- Keep the downloaded files inside the directory: ```./data/PSM/```

<b>WADI</b>
- Submit a data request form in https://docs.google.com/forms/d/1GOLYXa7TX0KlayqugUOOPMvbcwSQiGNMOjHuNqKcieA/viewform?edit_requested=true to download the dataset.
- Download the following files of 2017 ```WADI_14days.csv```, ```WADI_attackdata.csv``` from the dataset provider's link and keep them inside the directory ```./data/WADI/```
- In the same directory (```./data/WADI/```), you already have ```WADI_attacklabels.csv```. Now, run ```make_label.py``` to create the test label, named ```Attack_label.csv```, for the test dataset.
- Next, run the script ```make_pk.py``` from the same directory to create the dataset ```WADI.pk```

<b>SMAP and MSL</b>
- Download ```test``` and ```train``` sub-folders, along with ```labeled_anomalies.csv```, from the following link https://github.com/imperial-qore/TranAD/tree/main/data/SMAP_MSL
- Keep the downloaded csv file ad sub-folders inside the directory ```./data/SMAP_MSL/```.
- Next, run the script ```make_pk.py``` from the same directory to create the datasets ```SMAP.pk``` and ```MSL.pk```.

<b>SMD</b>
- Download the following sub-folders ```test```, ```train```, ```test_label``` from the following link https://github.com/NetManAIOps/OmniAnomaly/tree/master/ServerMachineDataset, and keep them in the directory ```./data/SMD```.
- Next, run the script ```make_pk.py``` from the same directory to create the dataset ```SMD.pk```.

### 2.1. If you want to train all entities combinedly, then use the following:

<b>SMAP_Combined</b>
- download the data from https://github.com/DAMO-DI-ML/KDD2023-DCdetector
- Keep the files in ```./data/SMAP_Combined```

<b>MSL_Combined</b>
- download the data from https://github.com/DAMO-DI-ML/KDD2023-DCdetector
- Keep the files in ```./data/MSL_Combined```

<b>SMD_Combined</b>
- download the data from https://github.com/DAMO-DI-ML/KDD2023-DCdetector
- Keep the files in ```./data/SMD_Combined```



### 3. Reproducing the main results
Note: To make evaluation faster, we can comment out some metrics in ```selected_keys``` within the ```anomaly_evaluation``` function in ```./utils/anomaly_evaluation.py```, especially ('Aff_F1', 'Aff_P', 'Aff_R', 'Range_F1', 'Range_P', 'Range_R').

Run the following scripts to reproduce the main results for single entity datasets,
```
bash ./scripts/main_result/SWAT.sh
bash ./scripts/main_result/PSM.sh
bash ./scripts/main_result/WADI.sh
bash ./scripts/main_result/SMAP.sh
bash ./scripts/main_result/MSL.sh
bash ./scripts/main_result/SMD.sh
```

Run the following scripts to reproduce the main results for multi-entity datasets using protocol-1 (training a single model across all entities),
```
bash ./scripts/main_result/SMAP_Combined.sh
bash ./scripts/main_result/MSL_Combined.sh
bash ./scripts/main_result/SMD_Combined.sh
```
Run the following scripts to reproduce the main results for multi-entity datasets using protocol-2 and protocol-3 (training separate models per entity),
```
bash ./scripts/main_result/SMAP.sh
bash ./scripts/main_result/MSL.sh
bash ./scripts/main_result/SMD.sh
```

Generated results will be found in the directory: ```./outputs/Results/```

### Implementation Details
- Our proposed model is implemented in ```./models/CCM_TAD.py```
- Our proposed anomaly detection framework is implemented in ```./utils/anomaly_evaluation.py```, specifically the function ```anomaly_evaluation_sequential```.
- Config for the datasets: ```./scripts/config/config.xlsx```

## Script Overview
### Two-Stage Script Workflow
This script is designed to speed up experiments by separating training and sequence-level evaluation into two runs.
1. First run: train with point-based anomaly detection using target_optimization = pf1.
2. Second run: load the trained checkpoint and evaluate with sequence-based anomaly detection using target_optimization = sf1.

Post processing using Sequence-based evaluation is expensive if done during every training iteration, so this two-stage setup is much faster.

### Checkpoint Setting Logic
Use the checkpoint flag based on whether a checkpoint already exists:

If checkpoint exists:
1. pf1 run: checkpoint_load = 1
2. sf1 run: checkpoint_load = 1

If checkpoint does not exist:
1. pf1 run: checkpoint_load = 0
2. sf1 run: checkpoint_load = 1

In our current scripts, both are set to 1 because we assume a checkpoint is already available.

<table>
  <tr>
    <td bgcolor="#4da5f3">
      <strong>Need help implementing your model or have questions about this framework? Feel free to contact the author (mmurad@usf.edu). No strings attached.</strong>
    </td>
  </tr>
</table>
