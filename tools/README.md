# Data Preparation Steps

All data processing tools assume by default a symlink to the folder `data`, linking each dataset: MegaDepth, Cambridge Landmarks and 7 scenes. Here's an example on how that looks normally

```
$ tree data
data
├── 7scenes -> /storage/blind_pnp/7scenes
├── cambridge -> /storage/blind_pnp/cambridge
├── MegaDepth_undistort -> /storage/blind_pnp/megadepth
```

You also need to make use of a third-party dependency called `immatch`, that is available [here](https://github.com/GrumpyZhou/image-matching-toolbox) to generate descriptor caches.

## MegaDepth

### Getting the Dataset

Download and preprocess the dataset following the [instructions](https://github.com/mihaidusmanu/d2-net#downloading-and-preprocessing-the-megadepth-dataset) from the D2-Net project repo.

### Generating a Scene File

tool location: [`tools/prep_megadepth_2D3D_data.py`](tools/prep_megadepth_2D3D_data.py)

By default the tool assumes there is a folder with the undistorted SfM reconstruction of MegaDepth symlinked to the `data` folder, as explained in the previous section. If the folder is symlinked, theoretically no arguments need to passed. Just call the tool with
```
$ python tools/prep_megadepth_2D3D_data.py
```

After completion, the tool will generate a number of files in `data/MegaDepth_undistort/data_processed/v2`.


### Generating a Descriptor Cache

Run the tool `tools/extract_features_immatch.py`.

```
python -m tools.extract_features_immatch --immatch_config 'immatch/sift'   --dataset 'megadepth'
```
This will generate the descriptor cache for sift.

## Cambridge Landmarks
### Getting the Dataset

The dataset is composed of multiple landmarks. These can be downloaded from the following locations:
- [Great Court](https://www.repository.cam.ac.uk/handle/1810/251291)
- [King's College](https://www.repository.cam.ac.uk/handle/1810/251342)
- [Old Hospital](https://www.repository.cam.ac.uk/handle/1810/251340)
- [Shop Facade](https://www.repository.cam.ac.uk/handle/1810/251336)
- [St. Mary's Church](https://www.repository.cam.ac.uk/handle/1810/251294)

Download the reconstructions and retrieval pairs released by [PixLoc](https://github.com/cvg/pixloc) at this [url](https://cvg-data.inf.ethz.ch/pixloc_CVPR2021/Cambridge-Landmarks/). You can download everything recursively using the following command
```
wget -r -np -R "index.html*" -nH --cut-dirs=2 https://cvg-data.inf.ethz.ch/pixloc_CVPR2021/Cambridge-Landmarks/
```
If you call this from the folder where your extracted your dataset is, the reconstructions will downloaded to each landmark folder automatically.

### Generating a Scene File

tool location: [`tools/prep_cambridge_2D3D_data.py`](tools/prep_cambridge_2D3D_data.py)

By default the tool assumes there is a folder with the undistorted SfM reconstruction of MegaDepth symlinked to the `data` folder, as explained in the previous section. If the folder is symlinked, theoretically no arguments need to passed. Just call the tool with
```
$ python tools/prep_cambridge_2D3D_data.py
```

After completion, the tool will generate a number of files in `data/cambridge/data_processed/query-netvlad10`.

### Generating a Descriptor Cache

Run the tool `tools/extract_features_immatch.py`.

```
python -m tools.extract_features_immatch --immatch_config 'immatch/sift'   --dataset 'cambridge'
```
This will generate the descriptor cache for sift.

