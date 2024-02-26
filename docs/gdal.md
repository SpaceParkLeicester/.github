# GDAL Installation

## Instructions (Ubuntu 20.04)

1. Update, upgrade and add `ubuntugis` and instal `gdal-bin` and `libgdal-dev`

```
sudo add-apt-repository ppa:ubuntugis/ppa && sudo apt-get update
sudo apt-get install gdal-bin
sudo apt-get install libgdal-dev
```

2. Check `ogr` info

```
ogrinfo --version
```

3. In the virtual environment, install `gdal`

```
pip install GDAL==<ogrinfo --version>
```