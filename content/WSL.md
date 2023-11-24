


## Use Docker

docker run -it --gpus all -p PORT:PORT -V LOCAL_PATH:MOUNT_TO IMAGE:TAG
>>> docker run -it --ip 127.0.0.1 --gpus all -p 5000:8888 -v ~/:/peizhi nvidia-docker:latest

## Tensorflow, Pytorch, Juputer
docker run -it --gpus all -p 8888:8888 -v ~/:/tf nvidia-docker:latest

## Pytorch3D 
docker run -it --gpus all --ip 0.0.0.0 -p 8888:8888 -v /:/ubuntu_root pytorch3d


docker image ls

docker container ls


## remove docker image
docker image rm [OPTIONS] IMAGE [IMAGE...]

## save container to image
# docker commit [CONTAINER_ID] [new_image_name]
>>> docker commit 7b855f37f296 nvidia-docker





----------------------------------------------------
Windows Powershell Commands:
----------------------------------------------------

Give WSL a static IP: https://github.com/MicrosoftDocs/WSL/issues/418

Open SSH for WSL (see https://www.hanselman.com/blog/how-to-ssh-into-wsl2-on-windows-10-from-an-external-machine):


# WSL
netsh interface portproxy add v4tov4 listenaddress=0.0.0.0 listenport=22 connectaddress=192.168.50.16 connectport=22

netsh advfirewall firewall add rule name="Open Port 22 for WSL2" dir=in action=allow protocol=TCP localport=22



# Docker Jupyter
netsh interface portproxy add v4tov4 listenaddress=0.0.0.0 listenport=8888 connectaddress=192.168.50.16  connectport=8888

netsh advfirewall firewall add rule name="Open Port 8888 for Jupyter" dir=in action=allow protocol=TCP localport=8888


# Check
netsh interface portproxy show v4tov4


# To remove all
#netsh int portproxy reset all


