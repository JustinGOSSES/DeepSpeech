
## Purpose of this document: 

#### I had trouble installing <a href="https://github.com/mozilla/DeepSpeech">Mozilla's Deepspeech</a> because my MacOS operating system was not the expected one. This required a few differences in installation, which I've tried to document here. 

##### It should be noted that these instructions also worked on the docker image I tried, <a herf="https://hub.docker.com/r/manujbhatia/deepspeech/">here</a>. This docker image loads the various things you need on ubuntu, but it does not do the DeepSpeech installation steps.  

The basic problem that caused the issues was that <a href="https://pypi.python.org/pypi/deepspeech/0.1.0">pypi files</a> for MacOSX referenced during the command *pip install deepspeech* were only for v10.12 of MacOSX and my computer was on v10.11 of MacOSX.

This instructions are for the python bindings only, not native and not node.js. Also, this is focused on cpu not gpu code. Please refer to the <a href="https://github.com/mozilla/DeepSpeech">original</a> readme for more detailed directions. 

A debugging discussion occured on the Discourse discussion board informed these steps <a href="https://discourse.mozilla.org/t/pip-install-deepspeech-doesnt-find-deepspeech/22788/11">here</a>. https://discourse.mozilla.org/t/pip-install-deepspeech-doesnt-find-deepspeech/22788

## DeepSpeech Installation Steps

### 1. Install Github Repo
git clone https://github.com/mozilla/DeepSpeech

### 2.  Install Python bindings 
#### [NORMAL METHODS]
```
pip install deepspeech
```

If you want to use GPU, quicker inference (The realtime factor on a GeForce GTX 1070 is about 0.44.) can be performed using a supported NVIDIA GPU on Linux. (See the release notes to find which GPU's are supported.) This is done by instead installing the GPU specific package:

```bash
pip install deepspeech-gpu
```
#### [ALTERNATIVE]
The reason this didn't work for me was that the python bindings in pypi that are accessed via pip where only made for MacOSX v10.12. I was using MacOSX v10.11. The result was that when I tried to install the python bindings via 
```
pip install deepspeech
```
I saw an output that suggested there was no such pip module. I knew this wasn't the case. I was able to deal with this required MacOSX version issue by:

1. Downloading the <a href="https://pypi.python.org/pypi/deepspeech/0.1.0">wheel</a> directly & manually from pypi website. 
2. Changing the name of the wheel file such that it was called v10.11 instead of v10.12
3. Manually running pip via:

```
pip install "name-of-the-wheel-here"
```

### 3. Download and unpack the pre-built model that Mozzila has made and open-sourced.
#### [ALTERNATIVE ONLY AS THIS MAY HAVE BEEN DONE IN STEP 1] 
NOTE: This might not be necessary. Check to see if there is a *models* folder with output_model.pb inside. If you already have it, you can likely skip this step.

If you don't have the language models folder and file, follow these instructions. 
1. Download the language model from the <a href="https://github.com/mozilla/DeepSpeech/releases/download/v0.1.0/deepspeech-0.1.0-models.tar.gz">'releases' part of the github project page<a/>. 
2. Put in in the top-level of the deepspeech folder. 
3. Unpack the tar.gz file. It should unpack into a "models" folder. 

### 4. Run the inference using python bindings against the pre-trained model. 
#### [NORMAL]
According to the readme.md on the github repo, this command should work with the python bindings. 
```
deepspeech output_model.pb my_audio_file.wav alphabet.txt
```

#### [ALTERNATIVE] 

If you downloaded the language models folder as described instep 3, the file 'output_model.pb' mentioned in deepspeech's readme.md file is actually now called 'output_graph.pb'. 

You can either change the name of the file or change the command to match the name. Depending which you pick, you can then run.
```
deepspeech output_model.pb my_audio_file.wav alphabet.txt
```
or 
```
deepspeech output_graph.pb my_audio_file.wav alphabet.txt
``` 
NOTE: You might need to run this command inside of the models folder. 

## Additional Information About My Setup:
platform : osx-64
conda version : 4.3.30
conda is private : False
conda-env version : 4.3.30
conda-build version : 1.21.3
python version : 3.5.2.final.0
requests version : 2.18.4
root environment : /Users/jgossess/anaconda  (writable)
default environment : /Users/jgossess/anaconda/envs/deepspeech-venv-35
envs directories : /Users/jgossess/anaconda/envs
                      /Users/jgossess/.conda/envs
package cache : /Users/jgossess/anaconda/pkgs
                      /Users/jgossess/.conda/pkgs
channel URLs : https://repo.continuum.io/pkgs/main/osx-64
                      https://repo.continuum.io/pkgs/main/noarch
                      https://repo.continuum.io/pkgs/free/osx-64
                      https://repo.continuum.io/pkgs/free/noarch
                      https://repo.continuum.io/pkgs/r/osx-64
                      https://repo.continuum.io/pkgs/r/noarch
                      https://repo.continuum.io/pkgs/pro/osx-64
                      https://repo.continuum.io/pkgs/pro/noarch
config file : None
netrc file : None
offline mode : False
user-agent : conda/4.3.30 requests/2.18.4 CPython/3.5.2 Darwin/15.6.0 OSX/10.11.6    
UID:GID : 1396618864:1286109195
