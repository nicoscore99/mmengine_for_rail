# mmengine_for_rail

MMEngine is a PyTorch-based foundational library for the OpenMMLab ecosystem. This fork implements various changes to the original MMEngine verision. The main changes are listed in following:

### Shuffling for ConcatDataset
[ConcatDataset](mmengine/dataset/dataset_wrapper.py) is a dataset wrapper to combine multiple datastes into a single one. Per default, every dataset gets batched separately durng training. The 'shuffle' init parameter in the allows to shuffle the concatenated datasets randomely before batching, such that training happens in a balanced manner between the datasets.

### Model Weight Freezing
Added model weight freezing to the [BaseModule](mmengine/model/base_module.py). Allows to set parameter 'freeze_cfg' in the configuration file for any model that inherits from the BaseModule class. Configured the [Runner](mmengine/runner/runner.py) to call parameter freezing for all models that contain the corresponding parameter.

### Return Validation Loss
MMEngine per default does not return validation losses on the defined loss functions after validation epochs. Configured the [EpochBasedTrainLoop](mmengine/runner/loops.py) and the [ValLoop](mmengine/runner/loops.py) to add the validation losses as a per-iteration average to the 'after_val_epoch' hooks.

### Spawning Multiprocessing
Point cloud classification training in MMDetection3D relies on the [Open3D BallPivoting](https://www.open3d.org/) surface completion algorithm for up-sampling small point clouds to the minimal input size of the classification network. The combination of MMEngine and Open3D seems to create a deadlock that halts the training process. Changed the [multi-processing starting procedure](https://docs.python.org/3/library/multiprocessing.html) to 'spawn' to resolve the issue.

### Upsampler Registry
Created a new [registry](mmengine/registry/root.py) to register different point cloud up-sampling algorithms in MMDetection3D.

## Installation
Installation with setup.py:

```bash
sudo python3 setup.py install
```
