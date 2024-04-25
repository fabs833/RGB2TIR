This is from a RGB-2-TIR image-to-image conversion class project at the University of Utah. ECE6960 with Professor Rajesh Menon.
Group Members Dudley Irish, Jason Howard, Fabian Fulda

The above test results ARE from a dataset with 29,000 image pairs, captured on the camera RGB + TIR camera described here. 
As a baseline, we used the popular pix-to-pix architecture from https://github.com/junyanz/pytorch-CycleGAN-and-pix2pix. 
@inproceedings{CycleGAN2017,
  title={Unpaired Image-to-Image Translation using Cycle-Consistent Adversarial Networks},
  author={Zhu, Jun-Yan and Park, Taesung and Isola, Phillip and Efros, Alexei A},
  booktitle={Computer Vision (ICCV), 2017 IEEE International Conference on},
  year={2017}
}

One modification we tried was using dropout. When trained using dropout, some percentage of neurons is randomly deactivated on each forward pass. This forces the neuron to learn redundancies in its representation of the data, which can improve generalization to new, previously unseen inputs. It can also make the task more difficult for the network to learn.
In these experiments, the dropout rate was set to 50%.
Using dropout did not seem to have a significant impact on the results. However, we also tried a Resnet variant instead of the Unet on the same dataset and with the same test images. Results were distinctly different and in a number of cases better. So, this might be a direction worth pursuing going forward.
So far, we only get decent test data results if we have very similar training data. None of the models are very robust to any kind of significant change in environment. And, I am skeptical of others showing off results out there. We haven't seen anyone else out there clearly showing they used significantly different test images from their training images. 
Or, in some cases, maybe even just showed off training data. I'll be happy to stand corrected if someone posts indicative training and test data with their results.
Working with dropout didn't seem to have a significant impact. However, the results of the trained ResNet model look promising to me. I think this direction is definitely worth further investigation.
The supercomputer did not like any of the zip files made on my Windows machine. After some frustration, I finally copied the original uncompressed folders on a physical drive, compressed them on an old 2017 iMac I keep around for strange moments like this, and then uploaded them to the supercomputer from there. 

Runtime for 100 Epochs on the dataset with ≈29,000+ image pairs (of those 80%, so ≈23,200 image pairs were processed for training) was 10h 41min. on a single node with 4 Nvidia H100 GPU. So, ≈5,800 image pairs x 100 Epochs per GPU over 38,460sec ≈ 0.0663sec per image pair per Epoch on an H100. 

As to the image capture, with The Seek Mosaic sensor and the Arducam Lens Board OV5647 Sensor on the RPi4 recording straight to the Micro SD card, power consumption was ≈1.48Wh for capturing 2000 images over about 2500 seconds. So, even on a very small powerpack with only 10Wh or so, one could easily capture 10,000+ pictures without interruption. For working with a drone the maximum flight time would likely be the limiting factor.
Even without external ventilation or even passive cooling in a closed box, the RPi4 wouldn't overheat, Although I have never tried it on a really hot day or with the camera/case exposed to the sun for any prolonged period of time.

The current level of support on the Raspberry Pi 5 has led us to switch to using a Raspberry Pi 4 (RPi4). This also will yield a better battery life. 
The first step in setting up our modified camera is to get the Seek Thermal software running on a new Raspberry Pi 4.  Follow the following steps to install and configure the required software:
1. Download and install the latest 64-bit Raspberry OS.
2. Complete normal system setup.  (Created the user ece with the password "ece6960")
3. Enable SSH.
4. Install Emacs.
5. Follow the instructions in the Seek SDK Quick Start Guide.
6. Follow the instructions in the Seek Python binding Readme file.  https://github.com/seekthermal/seekcamera-python
7. Using apt install python3-opencv and python3-picamera2.
8. Create a virtual Python environment, "python3 -m venv --system-site-packages \~/.venv".  The system site-packages option is required to access the Seek, OpenCV, and Picamera modules.
9. Use git to clone https://github.com/apisdn/TRI2I.git.

With this software installed the image capture is performed by running the python script combined.py in TRI21/utils/data\_capture.  This script has been modified to use Picamera2 to capture images from the IMX219 camera via the built-in camera interface of the RPi4.  
