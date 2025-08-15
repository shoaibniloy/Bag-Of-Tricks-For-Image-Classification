
## Bag of Tricks for Image Classification with Convolutional Neural Networks

The pipeline is based on [pytorch-lightning framework](https://github.com/PyTorchLightning/pytorch-lightning).

### Installation

1. Clone the repository;
2. Set up the virtual environment;
3. If you are going to run experiments with mixed-precision:

   - Check you CUDA version with `nvcc --version`;
   - Install pytorch 1.5.1 compiled with CUDA version which you have installed. In our case it is CUDA 10.1 so the
     pytorch should be installed with
     `pip install torch==1.5.1+cu101 -f https://download.pytorch.org/whl/torch_stable.html`;
   - Install Nvidia Apex by following [the official instructions](https://github.com/NVIDIA/apex#quick-start);
     Otherwise:
   - Run `pip install torch==1.5.1` to install PyTorch

4. Run `pip install -r requirements.txt`

### Dataset

Please, follow the instruction to prepare the dataset:

- Download zip-archive from kaggle ([link](https://www.kaggle.com/dansbecker/food-101/));
- Unzip the data;
- Use `split_food-101.py` to split Food-101 into train/test folders. This script will parse `train.txt` and `test.txt`
  and copy images into corresponding sub-folders. Note that we hard-coded the classes which we are going to use.

### Training

To train model launch

```
python main.py --gpus [gpus_number] --max_epochs [epoch_number] --data-root [path_to_dataset] --amp_level [optimization_level] (only for mixed-precision with apex)
```

You can turn on any trick by adding the corresponding key:

```
--use-smoothing for label smoothing;
--use-mixup for mixup augmentation;
--use-cosine-scheduler for Cosine LR Scheduler;
--use-knowledge-distillation for Knowledge Distillation;
```

_Note:_ If you want to train the model on GPU you should always use `--gpus` key, without it the pytorch lightning console log will show you that the GPU is used but the training will be performed on CPU.

For Knowledge Distillation, please, download the teacher weights from [Dropbox](https://www.dropbox.com/s/za5eeyhhy6pmpd2/bag_of_tricks_resnet50_teacher.ckpt?dl=1).

Run `python main.py --help` to see all possible arguments. This command will also show you the arguments for pytorch lightning Trainer. Please, see the [official documentation](https://pytorch-lightning.readthedocs.io/en/0.8.5/trainer.html#trainer-flags) for details about them.

### Testing

To evaluate a trained model launch

```
python main.py --gpus [gpus_number] --data-root [path_to_dataset] -e --checkpoint [path_to_checkpoint]
```

