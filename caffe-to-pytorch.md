# Converting models from Caffe to PyTorch

1. Start with a Caffe model (i.e. `foo.prototxt` and `bar.caffemodel`).
2. Install [Microsoft's MMdnn tool](https://github.com/microsoft/MMdnn) using `pip`.
3. [Convert models from Caffe to an intermediate representation](https://github.com/Microsoft/MMdnn/blob/master/mmdnn/conversion/caffe/README.md#convert-models-from-caffe-to-ir-caffe---ir) using the following bash command:

    ```bash
    mmtoir -f caffe -n foo.prototxt -w bar.caffemodel -o placeholder1
    ```

    If you don't have PyCaffe installed (I didn't; that seems like a painful installation), this will throw a warning but it still ran for me anyway.

4. [Convert models from the intermediate representation to a PyTorch code snippet and weights](https://github.com/Microsoft/MMdnn/blob/master/mmdnn/conversion/pytorch/README.md#convert-models-from-ir-to-pytorch-code-snippet-and-weights) using the following bash command:

    ```bash
    mmtocode -f pytorch -n placeholder1.pb --IRWeightPath placeholder1.npy --dstModelPath placeholder2.py -dw placeholder2.npy
    ```

5. [Generate PyTorch model from code snippet file and weight file](https://github.com/Microsoft/MMdnn/blob/master/mmdnn/conversion/pytorch/README.md#generate-pytorch-model-from-code-snippet-file-and-weight-file) using the following bash command:

    ```bash
    mmtomodel -f pytorch -in placeholder2.py -iw placeholder2.npy -o placeholder3.pth
    ```

    If this step throws an error regarding shape incompatibility, you may have to modify `placeholder2.py` so that the weights in `placeholder2.npy` are modified to the shape expected before use. As an example, adding the following lines after `_weights_dict` was initialized in `placeholder2.py` fixed the issue for me:

    ```python
    for key in _weights_dict.keys():
        _weights_dict[key]['bias'] = np.squeeze(_weights_dict[key]['bias'])
    ```

6. According to MMdnn, to use the PyTorch model in Python:

    ```python
    import numpy as np
    import torch
    import imp
    MainModel = imp.load_source('MainModel', 'placeholder2.py')
    model = torch.load('placeholder3.pth')
    ```

Instead of Steps 5 and 6, I recommend manually converting the weights file (`placeholder2.npy`) into a `state_dict` and rewriting your model class from scratch because the automated conversion produces a model that wastes GPU memory by storing intermediate layer activations during the forward pass.

## Warnings

- Some sanity checks on the converted model might be useful to make sure nothing went horribly wrong with the conversion. More detailed comparisons would also be good.
- Caffe models use BGR input instead of RGB input, so you will need to account for that when preprocessing your input images.
- More complicated models whose layer-to-layer correspondence between Caffe models and PyTorch models are not clear may not work.
