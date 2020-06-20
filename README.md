# Sinusoidal Representation Networks (SIREN)

Unofficial PyTorch implementation of Sinusodial Representation networks (SIREN) from the paper [Implicit Neural Representations with Periodic Activation Functions](https://arxiv.org/abs/2006.09661). This repository is a PyTorch port of [this](https://github.com/titu1994/tf_SIREN) excellent TF 2.0 implementation of the same.

The training is ongoing. I will update the repo with the updated checkpoint and final output image soon.

# Setup
- Install dependencies using `requirements.txt`
```
$ pip install -r requirements.txt
```
- Add this folder to your PYTHONPATH
```
$ export PYTHONPATH=/path/to/siren:$PYTHONPATH
```

# Usage

## Sine activation
You can use the `Sine` activation as any other activation
```python
from siren import Sine

x = torch.rand(10)
y = Sine(w0=1)(x)
```

## Initialization
The authors in the paper propose a principled way of intializing the layers for the SIREN model. The initialization function
can be used as any other initialization present in `torch.nn.init`.

```python
from siren.init import siren_uniform_

w = torch.empty(3, 5)
siren_uniform_(w, mode='fan_in', c=6)
```

## SIREN model
The SIREN model used in the paper, with sine activation and custom initialization, can directly be created as follows.

```python
from siren import SIREN

# defining the model
layers = [256, 256, 256, 256, 256]
in_features = 2
out_features = 3
initializer = 'siren'
w0 = 1.0
w0_initial = 30.0
c = 6
model = SIREN(
    layers, in_features, out_features, w0, w0_initial,
    initializer=initializer, c=c)

# defining the input
x = torch.rand(10, 2)

# forward pass
y = model(x)
```

# Results on Image Inpainting task
A partial implementation of the image inpainting task is available as the `train_inpainting_siren.py` and `eval_inpainting_siren.py` scripts.

To run training:
```bash
$ python train_inpainting_siren.py
```

To run evaluation:
```bash
$ python eval_inpainting_siren.py
```

Weight files are made available in the repository under the `checkpoints` directory. It generates the following output after 5000 epochs of training with batch size 8192 while using only 10% of the available pixels in the image during training phase.

<img src="https://github.com/dalmia/siren/blob/master/images/celtic_spiral_knot.jpg?raw=true" height=100% width=100%>

# Tests
Tests are written using `unittest`. You can run any script under the `tests` folder.

# Contributing
As mentioned at the beginning, this codebase is a PyTorch port of [this](https://github.com/titu1994/tf_SIREN). So, I might have missed a few details mentioned in the original paper. Assuming that the implemention in the linked repo is correct, one can safely trust this implementation as well. The only major difference from the reference repo is that it has `w0` as part of the initialization as well. I did not see that in the paper and hence, didn't include it here. I have not deeply read the paper and this is simply to serve as a starting point for anyone looking for the implementation. Please feel free to make a PR or create an issue if you find a bug or you want to contribute to improve any other aspect of the codebase.

