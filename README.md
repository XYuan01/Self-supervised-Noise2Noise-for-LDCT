# Self-supervised Noise2Noise for LDCT denoising from corrupted images.

## Dependencies

* [PyTorch](https://pytorch.org/) (0.4.1)
* [Torchvision](https://pytorch.org/docs/stable/torchvision/index.html) (0.2.0)
* [NumPy](http://www.numpy.org/) (1.14.2)
* [Matplotlib](https://matplotlib.org/) (2.2.3)
* [Pillow](https://pillow.readthedocs.io/en/latest/index.html) (5.2.0)

To install the latest version of all packages, run
```
pip3 install --user -r requirements.txt
```


## Dataset

Dataset address: [Mayo](https://wiki.cancerimagingarchive.net/pages/viewpage.action?pageId=52758026)
## Training

See `python3 train.py --h` for list of optional arguments.

By default, the model train with noisy targets. To train with clean targets, use `--clean-targets`. To train and validate on smaller datasets, use the `--train-size` and `--valid-size` options. To plot stats as the model trains, use `--plot-stats`; these are saved alongside checkpoints. By default CUDA is not enabled: use the `--cuda` option if you have a GPU that supports it.

### Hybrid noise
The noise parameter is the maximum standard deviation σ.
```
python3 train.py \
  --train-dir ../data/train/ldct\
  --valid-dir ../data/valid/ldct\
  --ckpt-save-path ../ckpts \
  --nb-epochs 10 \
  --batch-size 4 \
  --loss l2 \
  --noise-type hybrid\
  --noise-param 15\
  --crop-size 64 \
  --plot-stats \
  --cuda
```



## Testing

Model checkpoints are automatically saved after every epoch. To test the denoiser, provide `test.py` with a PyTorch model (`.pt` file) via the argument `--load-ckpt` and a test image directory via `--data`. The `--show-output` option specifies the number of noisy/denoised/clean montages to display on screen. To disable this, simply remove `--show-output`.

```
python3 test.py \
  --data ../data \
  --load-ckpt ../ckpts/
  --noise-type hybrid\
  --noise-param 15 \
  --crop-size 256 \
  --show-output 3 \
  --cuda
```

See `python3 test.py --h` for list of optional arguments, or `examples/test.sh` for an example.
