
[Source](https://hackernoon.com/docker-compose-gpu-tensorflow-️-a0e2011d36 "Permalink to Docker Compose + GPU + TensorFlow = ❤️")

# Docker Compose + GPU + TensorFlow = ❤️

Docker is awesome — more and more people are leveraging it for development and distribution. Instant environment setup, platform independent apps, ready-to-go solutions, better version control, simplified maintenance: Docker has a lot of benefits.

But when it comes to data science and deep learning, there is a certain hitch. You have to memorize all those docker flags to share ports and files between host and container, create unnecessary `run.sh` scripts and deal with CUDA versions and GPU sharing. If you have ever seen this error, you know the pain:


    $ nvidia-smi   
    Failed to initialize NVML: Driver/library version mismatch

![][1]

### Our goal

The purpose of this small post is to introduce you a sufficient set of Docker utilities and GPU-ready boilerplate we often use in our company.

So, instead of this:


    docker run   
    --rm   
    --device /dev/nvidia0:/dev/nvidia0   
    --device /dev/nvidiactl:/dev/nvidiactl   
    --device /dev/nvidia-uvm:/dev/nvidia-uvm   
    -p 8888:8888   
    -v `pwd`:/home/user   
    gcr.io/tensorflow/tensorflow:latest-gpu

You will end up with this:


    doc up

Cool, right?

What do we actually want to achieve:

* Manage our application state (run, stop, remove) using one command
* Save all those run flags to a single configuration file we can commit to a git repo
* Forget about GPU driver version mismatch and sharing
* Use GPU-ready containers in production tools like Kubernetes or Rancher

So here is the list of tools we highly recommend for every deep learner:

### 1\. CUDA

First, you will need [CUDA toolkit][2]. It's an absolute must-have, if you plan to train models yourself. We recommend to use `runfile` installer type instead of `deb`, because it won't mess your dependencies in future updates.

_(Optional) How to check if it works:_


    cd /usr/local/cuda/samples/1_Utilities/deviceQuery  
    make  
    ./deviceQuery # Should print "Result = PASS"

### 2\. Docker

You don't want to pollute your computer with tons of libraries and be afraid of broken versions hell. Also, you won't have to build and install stuff yourself — usually, software is already built for you and packed in image! [Installing Docker][3] is simple:


    curl -sSL  | sh

### 3\. Nvidia Docker

A must have [utility][4] from NVIDIA if you use Docker — it really simplifies using GPU inside Docker containers.

Installation is really simple:


    wget -P /tmp https://github.com/NVIDIA/nvidia-docker/releases/download/v1.0.1/nvidia-docker_1.0.1-1_amd64.deb  
    sudo dpkg -i /tmp/nvidia-docker*.deb

Now, instead of sharing nvidia devices every time like this:


    docker run --rm --device /dev/nvidia0:/dev/nvidia0 --device /dev/nvidiactl:/dev/nvidiactl --device /dev/nvidia-uvm:/dev/nvidia-uvm nvidia/cuda nvidia-smi

you can use a `nvidia-docker` command:


    nvidia-docker run --rm nvidia/cuda nvidia-smi

Also, you can stop worrying about driver version mismatch: docker plugin from Nvidia will solve your problems.

### 4\. Docker Compose

Super useful utility that allows you to store `docker run` configuration in a file and manage application state more easily. Though it was designed to "compose" multiple docker containers together, docker compose is still very useful when you only have one service. Pick the stable version [here][5]:


    curl -L https://github.com/docker/compose/releases/download/1.15.0/docker-compose-`uname -s`-`uname -m` > /usr/local/bin/docker-compose  
    chmod +x /usr/local/bin/docker-compose

### 5\. Nvidia Docker Compose

Unfortunately, Docker Compose doesn't know that Nvidia Docker exists. Lucky, there is a solution: a tiny [Python script][6] that generates configuration with `nvidia-docker` driver. Install it using pip:


    pip install nvidia-docker-compose

Now you can use `nvidia-docker-compose` command instead of `docker-compose`.

#### Alternative

If you don't want to use `nvidia-docker-compose`, you can [pass volume-driver manually][7]. Just add those options to your `docker-compose.yml`:


    # Your nvidia driver version here  
    volumes:  
    nvidia_driver_375.26:  
    external: true  
    ...  
    volumes:  
    - nvidia_driver_375.26:/usr/local/nvidia:ro

### 6\. Bash aliases

But `nvidia-docker-compose` is 21 characters to type! That's too much.

![][8]

Lucky we can use bash aliases. Open `~/.bashrc` (sometimes `~/.bash_profile`) in your favorite editor and type those lines:


    alias doc='nvidia-docker-compose'  
    alias docl='doc logs -f --tail=100'

Update your settings by running `source ~/.bashrc`.

### Start a TensorFlow service

Now we are ready to use benefits from all those stuff above. For example, let's run a Tensorflow GPU-enable Docker container.

In a project directory create file `docker-compose.yml` with the following content:


    version: '3'


    services:  
    tf:  
    image: gcr.io/tensorflow/tensorflow:latest-gpu  
    ports:  
    - 8888:8888  
    volumes:  
    - .:/notebooks

Now we can start TensorFlow Jupiter with a single command:


    doc up

[1]: https://hackernoon.com/hn-images/1*nbtUgG4ZD9tayuliqwAkWA.png
[2]: https://developer.nvidia.com/cuda-downloads
[3]: https://www.docker.com/get-docker
[4]: https://github.com/NVIDIA/nvidia-docker
[5]: https://github.com/docker/compose/releases
[6]: https://github.com/eywalker/nvidia-docker-compose
[7]: https://github.com/NVIDIA/nvidia-docker/wiki/NVIDIA-driver#alternatives
[8]: https://hackernoon.com/hn-images/1*ze6lqlMM2Lesvq1Ywm5K4g.jpeg

  
