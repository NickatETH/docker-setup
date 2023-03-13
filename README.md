# docker-setup

Dockerfiles for containers with ROS2 Humble for the main raptor application and ROS1 Noetic with SVO Pro. 

## Get Running

Pull the noVNC container: 

```bash
docker pull theasp/novnc:latest
```

Clone the repository and switch into the directory containing the ROS distribution of your choice (in this example, Humble): 

```bash
git clone https://github.com/raptor-ethz/docker-setup.git
cd humble
```

Build the docker image (of your choice):

```bash
docker build -f Dockerfile.source .
```

Then, you may want to tag the image using the image ID: 

```bash
docker image ls
docker tag <image_id> raptor:latest
```

You may exchange the image name for something of your choice.

Then, start the noVNC container and a container of the image we just built:

```bash
docker network create ros

docker run -d --rm --net=ros \
   --env="DISPLAY_WIDTH=3000" --env="DISPLAY_HEIGHT=1800" --env="RUN_XTERM=no" \
   --name=novnc -p=8080:8080 \
   theasp/novnc:latest

docker run -it --net=ros --env="DISPLAY=novnc:0.0" raptor:source bash
```

You will be able to access the GUI at [http://localhost:8080/vnc.html](http://localhost:8080/vnc.html).

For running a VIO container, build the image in `noetic` and follow the instructions at [https://github.com/uzh-rpg/rpg_svo_pro_open](https://github.com/uzh-rpg/rpg_svo_pro_open).
