# Image Registry

- the image is stored in a central image repository known as image registry the default public registry where all is known as docker hub
- you may need to have your own internal Docker registry that can be hosted within your organization in that case you can deploy your own docker registry within your organization using the Docker Trusted registry service
- there are 3 types of images you find in docker hub
    - the official images that are maintained by docker
    - vitrified images that are maintained by third party publisher
    - user images pushed by individual users

# image tag

- if we created a container specifying the image name without a tag , it is assumed to use the latest tag , so latest is a tag associated to the latest version of that software

![Untitled](https://github.com/alinedam/Sitech-Internship/assets/108859223/f61a9f73-de02-4704-8203-1f8df79f4728)


- we can specify other versions of images as required using any of the supported tags note that each version can have multiple tag names

# Image list : list Local Available Images

- to search for images for our cli is to use the docker cli using the docker search option
- by default docker search displays 25 lines results,

# Image pull: Download latest Image

- what if we want to download the image and keep it so that later point when run the command we don’t have to wait for it to download
- for that we use the docker image pull command we the docker image pull command

```bash
$ docker image pull httpd 
```

in this case it pulls the httpd image but does not actually create a container from it once that is done we can list the images  

```bash
$ docker image list 
```

# Image addressing Convention

# Authenticating to Registries

- to pull an image form a public repo you don’t need to authenticate
- but pulling form a privet repo on either privet or privet registries then without authentication you would receive an access dined error
- also if you want to push your own custom image that you created to a registry either public or privet then you wold need to authenticate as well .

# how to auth a docker registry

- for this we run the docker login command and specify then username and password for the registry so if pushing to Docker Hub u must create an account on docker hub and use the credential here

![2nd](https://github.com/alinedam/Sitech-Internship/assets/108859223/092c4daf-0ecc-4656-b3df-fd1e19b9fcc7)


- Once logged in into a privet registry every push you perform will push that local image to remote registry

 

# image tag: Retagging an image locally

- how we create our own copy of an image say i want to create a copy of the httpd Alpine version of the image and push it to my internal registry
- we can’t rename an image but we can retag an image witch is nothing but creating a soft link to existing image
- we use the docker image tag command and specify the old image name followed by the new image name witch will create a link to an image ID with a new name

```bash
$ docker image tag httpd:alpine httpd:customv1 
```

![3d](https://github.com/alinedam/Sitech-Internship/assets/108859223/c8734c29-c383-4b2e-9bde-6e6a60d02dec)


- if you look at the image ID section it will show u the same ID as that of the old image  this indicates that these are just two tags assigned to the same image so there is no actual copy created for the image
- if this image was pulled for docker hub then we can re-tag this image to be pushed to our privet registry at [gcr.io](http://gcr.io)  like this

```bash
docker image tag httpd:alpin gcr.io/company/httpd:customv1
```

- the docker system df command will display the actual size of the different objects that are managed by docker such as images and containers

 

```bash
$ docker system df 
```

# Remove Images

- to remove an image run the docker image rm command  but u must ensure that there no containers running off of that image , so you must stop and delete all dependent containers to be able to delete an image when you try to remove an image, it does two things
    - it removes the soft link witch is the tag that is associated to that image
    - then removes the image , witch is the image layer itself freeing up space in this example , we have an image with two image tags

```bash
$ docker image rm httpd:customv1 
```

to delete a number of images here the easier way to do it 

- the command docker image prune -a

```bash
$ docker image prune -a 
```

- will delete all of the unused images and reclaim space on the host so you must be extremely careful with this that will only delete images that do not have any containers associated with it

# Inspect a running image

- if we wish to see all the image layers, witch are used to put together a practical image then we can run the image history command

```bash
$ docker image history ubuntu 
```

- this will list all the layers along with all the commands used to create that layer
- to see you’d like to see an additional details about a specific image , use the

```bash
$ docker image inspect httpd 
```

it returns all details of an image in a JOSON format 

![4th](https://github.com/alinedam/Sitech-Internship/assets/108859223/b72c9d5d-09b8-435b-97bb-d4f37f108902)


# save and load

- say for example in a restricted environment when you don’t have any access to the internet to download images retrieving images is going to be a challenge that is where you can convert you image into a tar file and copy it to the restricted environment and extract it there
- first pull the required image to the Docker using the Docker pull command
- and then we create a tar file from that image using the Docker image save command this command creates a tar bundle of the image

```bash
docker image save alpine:latest -o alpine.tar 
```

- then transfer this tar file to a shared location or to the restricted environment once their ,we use the docker image load command to tar file to extract and load the image

```bash
$ docker image load -i alpine.tar 
```

# Build Context

![build context](https://github.com/alinedam/Sitech-Internship/assets/108859223/f6567848-a1ce-44f4-ad0b-ea87e2555c47)


```jsx
$ docker build . -t my-custom-app
```

```bash
$ docker build /opt/my-custom-app
```

```bash
$ docker build https://gitube.com/myacocunt/mayapp
```

```bash
$ docker build https://github.com/myaccount/myapp#<brach>
```

```bash
$ docker build https://github.com/myaccount/myapp#<folder>
```

```bash
$ docker build -f Dockerfile.dev https://github.com/myaccount/myapp
```
