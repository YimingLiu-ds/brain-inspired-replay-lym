# Research Project:
## Exploring the application of brain-inspired feature exaggeration to a state-of-the-art continual learning model
Recent developments in neuroscience have uncovered the benifical role of memory exaggeration in the brain. The it has been shown that the brain will tend to exaggerate the differences between similar, competing memeories in order to better differentiate between them. This project explores possible adaptions to the current state-of-the-art generative continual learning model proposed by Van de Ven et al, 2020 in an attempt to applying such an exaggeration/repulsion to the generative mode's replayed 'memories' of competing (similar and often confused between) classes. Ultimately, the aim of this exaggeration is to reduce the extent of catastrophic forgeting of earlier-learnt classes by encouraging a better differentiation between them. This project builds upon the work of Van de Ven et al, 2020, adding extensive modifications to their PyTorch implementation. Most alternations were made within the 'models/VAE.py', 'main_cl.py' and .... Entirely new additions have been highlighted through the use of a quadruple hashtag: '####'. 


## Installation & requirements
The current version of the code has been tested with `Python 3.5.2` on both Linux and Windows operating systems with the following versions of PyTorch and Torchvision:
* `pytorch 1.9.0`
* `torchvision 0.2.2`
* `kornia`
* `scikit-learn`
* `matplotlib`
* `visdom`
* `torchmetrics`
* `scipy`


To use the code, download the repository and change into it:
```bash
git clone https://github.com/jackmillichamp/brain-inspired-replay.git
cd brain-inspired-replay
```
(If downloading the zip-file, extract the files and change into the extracted folder.)
 
Assuming Python3 is set up, the Python-packages used by this code can be installed into a fresh docker image via the included dockerfile.

The required datasets do not need to be explicitly downloaded, this will be done automatically the first time the code is run.


## Demos

#### Demo 1: Brain-inspired replay on split MNIST
```bash
./main_cl.py --experiment=splitMNIST --scenario=class --replay=generative --brain-inspired --pdf
```
This runs a single continual learning experiment: brain-inspired replay on the class-incremental learning scenario of split MNIST.
Information about the data, the model, the training progress and the produced outputs (e.g., a pdf with results) is printed to the screen.
Expected run-time on a standard laptop is ~12 minutes, with a GPU it should take ~4 minutes.

#### Demo 2: Comparison of continual learning methods
```bash
./compare_MNIST.py --scenario=class
```
This runs a series of continual learning experiments to compare the performance of various methods.
Information about the different experiments, their progress and the produced outputs (e.g., a summary pdf) is printed to the screen.
Expected run-time on a standard laptop is ~50 minutes, with a GPU it should take ~18 minutes.


These two demos can also be run with on-the-fly plots using the flag `--visdom`.
For this visdom must be activated first, see instructions below.


## Running comparisons from the paper
The script `create_figures.sh` provides step-by-step instructions for re-running the experiments and re-creating the 
figures reported in the paper.

Although it is possible to run this script as it is, it will take very long and it is probably sensible to parallellize 
the experiments.


## Running custom experiments
Using `main_cl.py`, it is possible to run custom individual experiments. The main options for this script are:
- `--experiment`: which task protocol? (`splitMNIST`|`permMNIST`|`CIFAR100`)
- `--scenario`: according to which scenario? (`task`|`domain`|`class`)
- `--tasks`: how many tasks?

To run specific methods, use the following:
- Synaptic Intelligenc (SI): `./main_cl.py --si --c=0.1`
- Brain-Inspired Replay (BI-R): `./main_cl.py --replay=generative --brain-inspired`

For information on further options: `./main_cl.py -h`.


## On-the-fly plots during training
With this code it is possible to track progress during training with on-the-fly plots. This feature requires `visdom`.
Before running the experiments, the visdom server should be started from the command line:
```bash
python -m visdom.server
```
The visdom server is now alive and can be accessed at `http://localhost:8097` in your browser (the plots will appear
there). The flag `--visdom` should then be added when calling `./main_cl.py` to run the experiments with on-the-fly plots.
