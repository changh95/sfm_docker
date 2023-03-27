# Environment

## Hardware

- CPU: Intel i9-11900K @3.50GHz x 16
- RAM: 32.0GB
- GPU: NVIDIA GeForce RTX 3080

## Software

- OS: Ubuntu Linux 22.04.2 LTS 64bit
- GNOME Version: 42.5
- Windowing System: X11
- NVIDIA Driver: 510.108.03
- CUDA Version: 11.6

---

# How to build & run

## COLMAP

### Build 
```
docker build . --file colmap.dockerfile --tag colmap:latest
```

### Run

> Data is stored in the `/home/data` folder, which the volume is shared with `/data` inside the docker image

```
docker run \
    -e QT_XCB_GL_INTEGRATION=xcb_egl \
    -e DISPLAY=$DISPLAY \
    -v /tmp/.X11-unix:/tmp/.X11-unix \
    -v /home/data:/data \
    --env="QT_X11_NO_MITSHM=1" \
    --env="XAUTHORITY=$XAUTH" \
    --volume="$XAUTH=$XAUTH" \
    --network host \
    --gpus all \
    --privileged \
    --runtime=nvidia \
    -it colmap:latest colmap gui
```

## OpenSFM

### Build
```
docker build . --file opensfm.dockerfile --tag opensfm:latest
```

### Run

> Data is stored in the `/home/data` folder, which the volume is shared with `/source/OpenSfM/data` inside the docker image

```
docker run -it -p 8080:8080 -v /home/data:/source/OpenSfM/data opensfm:latest
```

Running sfm on the 'berlin' sample dataset
```
bin/opensfm_run_all_data/berlin
```

Running web-based viewer for 'berlin' sample dataset, after running sfm. Access `http://localhost:8080/` to use the GUI viewer.
```
python3 viewer/server.py -d data/berlin
```

## OpenMVG

### Build

```
docker build . --file openmvg.dockerfile --tag openmvg:latest
```

### Run

```
docker run -it -v /home/data:/data openmvg:latest
```

Run sfm on the '[SceauxCastle](https://github.com/openMVG/ImageDataset_SceauxCastle)' sample dataset
```
cd /opt/openMVG_Build/software/SfM
```

```
python3 SfM_GlobalPipeline.py /data/SceauxCastle/images/ /data/SceauxCastle
```
