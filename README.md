# UAV Aerial Traffic Small-Object Dataset

无人机航拍交通小目标检测数据集，面向道路交通场景中的车辆与非机动车目标检测研究。数据集由无人机视频抽帧、图像裁剪和人工标注构建，包含训练、验证和测试划分，并提供 YOLO 格式的边界框标签。

This dataset supports research on small-object detection in UAV aerial traffic imagery. It contains cropped and annotated road-scene images with predefined training, validation, and test splits, using YOLO-format bounding-box labels.

## Dataset at a Glance

| Split | Images | Intended use |
| --- | ---: | --- |
| Train | 1,125 | Model training |
| Val | 321 | Validation and model selection |
| Test | 162 | Final evaluation |
| **Total** | **1,608** | |

The image files occupy approximately 1.02 GiB in the current dataset directory. Images are JPEG crops at 1024 x 1024 pixels. The labels use three classes, in the order recorded in `train/labels/classes.txt` (also present in the validation and test splits):

| Class ID | Class name | Description |
| ---: | --- | --- |
| 0 | `small-vehicle` | Small vehicle |
| 1 | `large-vehicle` | Large vehicle |
| 2 | `non-motor-vehicle` | Non-motor vehicle |

## Collection and Preparation

The accompanying thesis describes footage collected with a DJI Flip over roads in Shenyang, Liaoning, China, including urban arterial roads, suburban roads, and intersections. Flights were conducted at approximately 120–450 m. Video was sampled at one frame per 60 frames; 3840 x 2160 source frames were cropped into 1024 x 1024 image patches using a sliding window with 15% overlap and a stride of 870 pixels. Objects were annotated in three traffic categories. The thesis reports an approximately 7:2:1 train/validation/test split; the image counts in this repository match its reported split counts.

## Directory Layout

```text
train/
  images/   # Training JPEG images
  labels/   # Matching YOLO label files and classes.txt
val/
  images/
  labels/
test/
  images/
  labels/
```

Each image has a label file with the same basename and `.txt` extension. Each non-empty annotation line follows the YOLO format:

```text
class_id x_center y_center width height
```

Box coordinates are normalized to the image width and height, with values in the range 0–1. Images without annotated objects may have an empty label file.

## Use with Ultralytics YOLO

Create a `data.yaml` file in the repository root:

```yaml
path: .
train: train/images
val: val/images
test: test/images

names:
  0: small-vehicle
  1: large-vehicle
  2: non-motor-vehicle
```

For example, with Ultralytics installed:

```bash
yolo detect train model=yolo11n.pt data=data.yaml
```

Check the installed Ultralytics version and supported model names before training; model identifiers may vary by release.

## Related Research

This dataset is described in the thesis *Research on Small Target Detection Method for UAV Aerial Traffic Scenarios* (无人机航拍交通场景下小目标检测方法研究). The thesis reports a DEF-YOLO mAP@0.5 of 88.3% on its self-built dataset. This is a result reported for that study and is not a guarantee of performance for other implementations or evaluation setups.

## Downloading

The JPEG files are stored with Git LFS. Install Git LFS before cloning so Git can fetch the image content:

```bash
git lfs install
git clone <REPOSITORY-URL>
cd <REPOSITORY-DIRECTORY>
git lfs pull
```

## License and Responsible Use

No dataset license has been specified. Public visibility on GitHub does not by itself grant permission to reuse, redistribute, or adapt the images or annotations. Confirm the rights to publish the footage and annotations, check for identifying or sensitive content such as readable license plates, and add an explicit license only after those checks. Until then, all rights remain with their respective rights holders.

## Citation

If you use this dataset, cite the accompanying thesis and include the repository URL and version or commit hash. Complete the bibliographic details below before release:

```text
Author. Research on Small Target Detection Method for UAV Aerial Traffic Scenarios.
Master's thesis, institution, year. (Fill in the verified details.)
Dataset repository: <REPOSITORY-URL>, version/commit: <VERSION-OR-COMMIT>.
```
