# DCLM-Morph

This repository contains the data preprocess code for paper [Scaling Inference-Efficient Language Models (ICML'25)](https://arxiv.org/pdf/2501.18107). Our code is based on [DCLM](https://github.com/mlfoundations/dclm).

## Getting Started

To get started with DCLM-Morph, follow these steps:

1. **Clone the repository**:
    ```bash
    git clone https://github.com/Waterpine/dclm-morph.git
    cd dclm-morph
    ```

2. **Install dependencies**:
    ```bash
    pip install -r requirements.txt
    ```
    Before installing the dependencies, make sure cmake, build-essential, and g++ are installed, e.g., by installing:
    ```bash
    apt install cmake build-essential
    apt install g++-9
    update-alternatives --install /usr/bin/g++ g++ /usr/bin/g++-9 90
    ```

We recommend the use of Python 3.10 with DCLM.


## Example
We show an example here to explain how to preprocess DCLM dataset.

1. **Download Dataset**: 
   1. use AWS CLI to download dataset from S3. 
   2. Please refer to the link below to download the dataset from Hugging Face: https://huggingface.co/datasets/mlfoundations/dclm-baseline-1.0/tree/main
2. **Start Ray Cluster**: use the following command:
    ```bash
    ray start --head --port 6379
    ```
3. **Run the tokenize and shuffle script**:
    ```bash
    mkdir dclm_output
    export PYTHONPATH=/users/xxx/dclm-shape:$PYTHONPATH
    python ray_processing/tokenize_shuffle.py --input /users/xxx/dclm-data-sample/ --readable_name dclm_shard --output dclm_output --content_key text
    ```
    where /users/xxx/dclm-data-sample/ contains the data downloaded from AWS S3 or Hugging Face. Run all commands under "dclm-shape" directory.
4. **Tear down**: Tear down the Ray cluster as in the processing step.
5. The `tokenize_shuffle.py` script creates a dataset in `webdataset` format, along with a `manifest.jsonl` file. This file is required by the training script, and it contains information on the number of sequences inside each shard of the dataset. If needed, this manifest file can also be created manually, via the following command:

```bash
python -m open_lm.utils.make_wds_manifest --data-dir dclm_output
```


## How to Cite Us

If you use DCLM dataset or models in your research, please cite as follows:

```bibtex
@article{li2024datacomplm,
  title={DataComp-LM: In search of the next generation of training sets for language models}, 
  author={Jeffrey Li and Alex Fang and Georgios Smyrnis and Maor Ivgi and Matt Jordan and Samir Gadre and Hritik Bansal and Etash Guha and Sedrick Keh and Kushal Arora and Saurabh Garg and Rui Xin and Niklas Muennighoff and Reinhard Heckel and Jean Mercat and Mayee Chen and Suchin Gururangan and Mitchell Wortsman and Alon Albalak and Yonatan Bitton and Marianna Nezhurina and Amro Abbas and Cheng-Yu Hsieh and Dhruba Ghosh and Josh Gardner and Maciej Kilian and Hanlin Zhang and Rulin Shao and Sarah Pratt and Sunny Sanyal and Gabriel Ilharco and Giannis Daras and Kalyani Marathe and Aaron Gokaslan and Jieyu Zhang and Khyathi Chandu and Thao Nguyen and Igor Vasiljevic and Sham Kakade and Shuran Song and Sujay Sanghavi and Fartash Faghri and Sewoong Oh and Luke Zettlemoyer and Kyle Lo and Alaaeldin El-Nouby and Hadi Pouransari and Alexander Toshev and Stephanie Wang and Dirk Groeneveld and Luca Soldaini and Pang Wei Koh and Jenia Jitsev and Thomas Kollar and Alexandros G. Dimakis and Yair Carmon and Achal Dave and Ludwig Schmidt and Vaishaal Shankar},
  year={2024},
  journal={arXiv preprint arXiv:2406.11794}
}
```

If you use the DCLM dataset to study scaling laws, we kindly ask that you cite the following work:
```bibtex
@article{bian2025scaling,
  title={Scaling Inference-Efficient Language Models},
  author={Bian, Song and Yan, Minghao and Venkataraman, Shivaram},
  journal={arXiv preprint arXiv:2501.18107},
  year={2025}
}
```

## License
This project is licensed under the MIT License. See the [license](LICENSE.txt) file for details.

