# Setting up Raspberry Pi Compute Module 4 with Coral USB Edge TPU


This tutorial will guide you through the process of setting up your Raspberry Pi CM4 module and using a Coral USB Edge TPU. The Coral USB Accelerator adds a Coral Edge TPU to your CM4, enabling you to accelerate your machine learning models.


This guide assumes that you have already installed Raspberry OS on your device. If you haven't, you can follow this [tutorial](https://www.jeffgeerling.com/blog/2020/how-flash-raspberry-pi-os-compute-module-4-emmc-usbboot) to do so. Once you have Raspberry OS 64bit installed, we can proceed with the installation of the necessary components for the Coral USB Accelerator.


## Installing the Edge TPU runtime


To install the Edge TPU runtime, execute the following commands in your Raspberry OS terminal:


```bash


echo "deb https://packages.cloud.google.com/apt coral-edgetpu-stable main" | sudo tee /etc/apt/sources.list.d/coral-edgetpu.list


curl https://packages.cloud.google.com/apt/doc/apt-key.gpg | sudo apt-key add -


sudo apt-get update


sudo apt-get install libedgetpu1-std


```


## Installing the Pycoral library


The next step is to install the Pycoral library. Pycoral currently supports Python 3.6 through 3.9. To check the Python version installed on your device, run the following command:


```bash


python -V


```


If your Python version is older than 3.6 or newer than 3.9, you will need to set up a virtual environment and install a compatible version of Python.


### Setting up a virtual environment using Pyenv


To create a virtual environment with a specific Python version alongside the system's default Python, we need a Python version management tool. Pyenv is a great choice for this. To install Pyenv, you first need to install its dependencies. Execute the following command:


```bash


sudo apt-get install -y make build-essential libssl-dev zlib1g-dev \


libbz2-dev libreadline-dev libsqlite3-dev wget curl llvm libncurses5-dev \


libncursesw5-dev xz-utils tk-dev libffi-dev liblzma-dev openssl


```


Now, you can install Pyenv by running this command:


```bash


set -e


curl -s -S -L https://raw.githubusercontent.com/pyenv/pyenv-installer/master/bin/pyenv-installer | bash


```


Next, add Pyenv to your load path by running:


```bash


nano ~/.bashrc


```


Then, add these lines at the bottom of the file:


```


export PATH="$HOME/.pyenv/bin:$PATH"


eval "$(pyenv init -)"


eval "$(pyenv virtualenv-init -)"


```


Save the file, restart your terminal, and Pyenv should be ready to use. To verify that Pyenv is set up successfully, run:


```bash


pyenv install --list


```


You should see a long list of Python versions and implementations that you can install. For this tutorial, we recommend installing Python 3.9:


```bash


pyenv install 3.9


```


This might take a while. After the installation is complete, verify the installation by running:


```bash


pyenv versions


```


You should see Python 3.9 alongside your system default version. Now, set Python 3.9 as the global Python version on your device:


```bash


pyenv global 3.9


```


Now, you are ready to install the tflite runtime and Pycoral library. If you have installed Python 3.9, simply run the following commands:


```bash


wget https://github.com/google-coral/pycoral/releases/download/v2.0.0/tflite_runtime-2.5.0.post1-cp39-cp39-linux_aarch64.whl


wget https://github.com/google-coral/pycoral/releases/download/v2.0.0/pycoral-2.0.0-cp39-cp39-linux_aarch64.whl


```


If you have installed a different Python version, download the tflite-runtime and Pycoral wheel files for your version from [here](https://github.com/google-coral/pycoral/releases/). Now, install tflite-runtime and Pycoral using pip:


```bash


pip install --upgrade pip


pip install tflite_runtime-2.5.0.post1-cp39-cp39-linux_aarch64.whl


pip install pycoral-2.0.0-cp39-cp39-linux_aarch64.whl


```


You're all set! Now you can use your USB accelerator to speed up your AI models. Remember to re-plug your accelerator into the CM4 and run your model. Trained models for the Edge TPU are available [here](https://coral.ai/models/). If you want to run other models on the accelerator, ensure they are converted to a TensorFlow Lite model and compiled for the Edge TPU. For more information, read [here](https://coral.ai/docs/edgetpu/models-intro/).


For a test run, you can clone this repository and run `detect.py`. To run `detect.py`, you need to install OpenCV:


```bash


pip install opencv-python


```
