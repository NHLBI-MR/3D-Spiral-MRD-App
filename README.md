# 3D-Spiral-MRD-App
This repository provides a reference implementation of a binning reconstruction for 3D stack-of-spiral imaging using the Gadgetron's opensource framework. This implementation is presented in Self-gated 3D stack-of-spirals UTE pulmonary imaging at 0.55T. 

## Reference Data 
A reference dataset is provided within the docker image and contains acquisitions, gradient waveforms, and header stored in the MRD format. 

## Running the reconstruction
The MRD app can be downloaded from Docker Hub at https://hub.docker.com/r/gadgetronnhlbi/stack-of-spiral-mrd-app. In a command prompt on a system with Docker installed, download the Docker image:

``` 
docker pull gadgetronnhlbi/stack-of-spiral-mrd-app 
```

Start the docker container on a system with gpus [ This implementation of binned stack of spiral reconstruction requires cuda ] 

```
docker run --gpus all --name=mrd-app-sos -ti -p 9002:9002  --volume=/home/$USER/mrdata/:/opt/data --restart unless-stopped  --detach gadgetronnhlbi/stack-of-spiral-mrd-app

```
Run the reconstruction using the provided test data within the container. First connect to the running container: 

```
docker exec -ti mrd-app-sos bash
```
Then run the reconstruction with in the container
```
gadgetron_ismrmrd_client -p 9002 -f test_data/test_data_stack_of_spiral.h5 -c spiral_inline_binning.xml -o /opt/data/reconstructed_images.h5
```
You can monitor the logs in a seperate terminal by running
```
docker logs -f mrd-app-sos
```

The reconstructed images are written in /home/$USER/mrdata/. These can be viewed using a h5 viewer. There are two series of reconstructed images 1) fully sampled images 2) 40% stable binned data.
