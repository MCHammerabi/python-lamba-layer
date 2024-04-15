# python-lamba-layer
The best and easiest way to make compatible lambda layers on python

## Start with Cloud9

### create environment 
platform: Amazon Linux 2023

### install miniconda (most recent python version) - in cloud9 terminal
```
mkdir -p ~/miniconda3
wget https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh -O ~/miniconda3/miniconda.sh
bash ~/miniconda3/miniconda.sh -b -u -p ~/miniconda3
rm -rf ~/miniconda3/miniconda.sh
~/miniconda3/bin/conda init bash
```

## Now build layers

### create directory
```
mkdir layer
cd layer
```

### requirements.txt
add python libraries in layer -- careful! there is a 50MB limit on layers!

### create lambda layer directory and install libraries
Note: match up python version with python version in lambda
```
mkdir -p aws-layer/python/lib/python3.12/site-packages
pip install -r requirements.txt --target aws-layer/python/lib/python3.12/site-packages
or
pip install -r requirements.txt --target aws-layer/python/lib/python3.12/site-packages --no-deps
```
Let it do its thing :)

### cd to aws-layer dir and zip
Note: name zip file (layer.zip)
```
cd aws-layer
zip -r layer.zip python/lib/python3.12/site-packages/*
```

### now publish your layer!
```
aws lambda publish-layer-version --layer-name cdsapi_layer --zip-file fileb://layer.zip --compatible-runtimes python3.12 --compatible-architectures x86_64
```

## conda installs
```
mkdir cfgrib
cd cfgrib
python -m venv cfgrib
source ./cfgrib/bin/activate
conda install -c conda-forge cfgrib
deactivate
```
### then create zip file
```
mkdir python
cd python
cp -r ../cfgrib/lib64/python3.12/site-packages/* .
cd ..
zip -r cfgrib_lambda_layer.zip python
```

### now publish your layer!
```
aws lambda publish-layer-version --layer-name cfgrib_layer_2 --zip-file fileb://cfgrib_lambda_layer.zip --compatible-runtimes python3.12 --compatible-architectures x86_64
```






