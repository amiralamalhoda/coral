# Setting up Raspberry Pi Compute Module 4 with Coral USB Edge TPU
This is a tutorial for setting up your Raspberry Pi CM4 module and for using a Coral USB Edge TPU. The Coral USB Accelerator adds a Coral Edge TPU to your CM4 so you can accelerate your machine learning models.

Here I assume that you have installed Raspberry OS on your device. If you don't have Raspberry OS click [here](https://www.jeffgeerling.com/blog/2020/how-flash-raspberry-pi-os-compute-module-4-emmc-usbboot) for a tutorial on how to do that. 
After you have Raspberry OS 64bit installed, we're set to install requiermenst for Coral USB Accelerator. 

## Install the Edge TPU runtime
Run the following command in your Raspberry OS terminal.

```bash
echo "deb https://packages.cloud.google.com/apt coral-edgetpu-stable main" | sudo tee /etc/apt/sources.list.d/coral-edgetpu.list
curl https://packages.cloud.google.com/apt/doc/apt-key.gpg | sudo apt-key add -
sudo apt-get update
sudo apt-get install libedgetpu1-std
  ```


## Install the Pycoral library
Next thing to install is the Pycoral library. Pycoral currently supports python 3.6 through 3.9. Run the following command to see what python version is installed on your device.
```bash
python -V
```
If you have any python version older than 3.6 or newer than 3.9, you will need to set up a virtual environment and install a suitable version of python. 

### Setting up a virtual environment using Pyenv
To build a virtual environment with some specific python alongside the system's default python, we need a python version management tool. To install Pyenv, first you need to build its dependencies. Run to following command.
```bash
sudo apt-get install -y make build-essential libssl-dev zlib1g-dev \
libbz2-dev libreadline-dev libsqlite3-dev wget curl llvm libncurses5-dev \
libncursesw5-dev xz-utils tk-dev libffi-dev liblzma-dev openssl
```
Now you can install Pyenv. Run this command:
```bash
set -e
curl -s -S -L https://raw.githubusercontent.com/pyenv/pyenv-installer/master/bin/pyenv-installer ❘ bash
```
Now you need to add Pyenv to your load path. Run the following command
```bash
nano ~/.bashrc
```
and the add these lines at the bottom of the file
```
export PATH="$HOME/.pyenv/bin:$PATH"
eval "$(pyenv init -)"
eval "$(pyenv virtualenv-init -)"
```
and save the file. Then restart your terminal and Pyenv will be ready to use. To make sure Pyenv is set up successfully, run the following command:
```bash
pyenv install --list
```
You must see a very long list of python versions and implementations that you can install. You need python 3.6 through 3.9. I recommend you install python 3.9 and the rest of this tutorial will assume you have installed python 3.9. 
```bash
pyenv install 3.9
```
This might take a while. After installation is done, run the following command to make sure the installation was successful.
```bash
pyenv versions
```
You must see python 3.9 alongside your system default version. Now set python 3.9 to be the global python version on your device. 
```bash
pyenv global 3.9
```
Now you are ready to install tflite runtime and pycoral lirary. If you have installed python 3.9, simply run the following commands.
```bash
wget https://github.com/google-coral/pycoral/releases/download/v2.0.0/tflite_runtime-2.5.0.post1-cp39-cp39-linux_aarch64.whl
wget https://github.com/google-coral/pycoral/releases/download/v2.0.0/pycoral-2.0.0-cp39-cp39-linux_aarch64.whl
```
If you have installed any other python version, download the tflite-runtime and pycoral wheel files for your version from [here](https://github.com/google-coral/pycoral/releases/).
Now install tflite-runtime and pycoral using pip.
```bash
pip install --upgrade pip
pip install tflite_runtime-2.5.0.post1-cp39-cp39-linux_aarch64.whl
pip install pycoral-2.0.0-cp39-cp39-linux_aarch64.whl
```
You're all set. Now you can use your USB accelerator to speed up your AI models. Make sure to re-plug your accelerator to the CM4 and run your model.
Trained models for the Edge TPU are available [here](https://coral.ai/models/). If you want to run other models on the accelerator, be sure to convert to a Tensorflow lite model and compile it for the Edge TPU. For more help read [here](https://coral.ai/docs/edgetpu/models-intro/).
For a test run, you can clone into this repository and run `detect.py`. To run `detect.py` you need to install opencv as well as other dependencies. 
```bash 
pip install opencv-python
``` 
