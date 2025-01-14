# Segment Anything Models (SAM) API

![](https://github.com/ksugar/samapi/releases/download/assets/qupath-samapi.gif)

A web API for [SAM](https://github.com/facebookresearch/segment-anything) implemented with [FastAPI](https://fastapi.tiangolo.com).

This is a part of the following paper. Please [cite](#citation) it when you use this project. You will also cite [the original SAM paper](https://arxiv.org/abs/2304.02643).

- Sugawara, K. [*Training deep learning models for cell image segmentation with sparse annotations.*](https://biorxiv.org/cgi/content/short/2023.06.13.544786v1) bioRxiv 2023. doi:10.1101/2023.06.13.544786


## Install

Create a conda environment.

```bash
conda create -n samapi -y python=3.11
conda activate samapi
```

If you're using a computer with CUDA-compatible GPU, install `cudatoolkit`.

```bash
conda install -y cudatoolkit=11.8
```

If you're using a computer with CUDA-compatible GPU on Windows, install `torch` with GPU-support with the following command.

```bash
# Windows with CUDA-compatible GPU only
python -m pip install torch --index-url https://download.pytorch.org/whl/cu118
```

Install `samapi` and its dependencies.

```bash
python -m pip install git+https://github.com/ksugar/samapi.git
```

If you are using WSL2, `LD_LIBRARY_PATH` will need to be updated as follows.

```bash
export LD_LIBRARY_PATH=/usr/lib/wsl/lib:$LD_LIBRARY_PATH
```

## Usage

### Launch a server

```bash
uvicorn samapi.main:app
```

The command above will launch a server at http://localhost:8000.

```
INFO:     Started server process [21258]
INFO:     Waiting for application startup.
INFO:     Application startup complete.
INFO:     Uvicorn running on http://127.0.0.1:8000 (Press CTRL+C to quit)
```

For more information, see [uvicorn documentation](https://www.uvicorn.org/#command-line-options).

### Request body

```python
class SAMBody(BaseModel):
    type: Optional[ModelType] = ModelType.vit_h
    bbox: Tuple[int, int, int, int] = Field(example=(0, 0, 0, 0))
    b64img: str
```

| key    | value                                   |
| ------ | --------------------------------------- |
| type   | One of `vit_h`, `vit_l`, or `vit_b`     |
| bbox   | Coordinate of a bbox `(x1, y1, x2, y2)` |
| b64img | Base64-encoded image data               |

### Response body

The response body contains a list of [GeoJSON Feature objects](https://geojson.org).

Supporting other formats is a future work.

## Updates

- v0.2.0: Support for MPS backend (MacOS) by [@petebankhead](https://github.com/petebankhead)

## Citation

Please cite my paper on [bioRxiv](https://biorxiv.org/cgi/content/short/2023.06.13.544786v1).

```.bib
@article {Sugawara2023.06.13.544786,
	author = {Ko Sugawara},
	title = {Training deep learning models for cell image segmentation with sparse annotations},
	elocation-id = {2023.06.13.544786},
	year = {2023},
	doi = {10.1101/2023.06.13.544786},
	publisher = {Cold Spring Harbor Laboratory},
	abstract = {Deep learning is becoming more prominent in cell image analysis. However, collecting the annotated data required to train efficient deep-learning models remains a major obstacle. I demonstrate that functional performance can be achieved even with sparsely annotated data. Furthermore, I show that the selection of sparse cell annotations significantly impacts performance. I modified Cellpose and StarDist to enable training with sparsely annotated data and evaluated them in conjunction with ELEPHANT, a cell tracking algorithm that internally uses U-Net based cell segmentation. These results illustrate that sparse annotation is a generally effective strategy in deep learning-based cell image segmentation. Finally, I demonstrate that with the help of the Segment Anything Model (SAM), it is feasible to build an effective deep learning model of cell image segmentation from scratch just in a few minutes.Competing Interest StatementKS is employed part-time by LPIXEL Inc.},
	URL = {https://www.biorxiv.org/content/early/2023/06/13/2023.06.13.544786},
	eprint = {https://www.biorxiv.org/content/early/2023/06/13/2023.06.13.544786.full.pdf},
	journal = {bioRxiv}
}
```