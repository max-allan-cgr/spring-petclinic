# Using petclinic to demo CraC in Docker

## With the public CraC image
Build and run the image
```
docker build -t petclinic-public -f Dockerfile.public .
docker run --cap-add CHECKPOINT_RESTORE --cap-add SYS_PTRACE --name petclinic-crac petclinic-public
```
The image will run and create a checkpoint at the end.

When it's exited, we can create a new image from the exited container with `docker commit` - and change the entrypoint to restore from the checkpoint while we're doing that.

And, remove the exited image
```
docker commit --change='ENTRYPOINT ["java", "-XX:CRaCRestoreFrom=/checkpoint"]' petclinic-crac petclinic-advanced-crac-restore
docker rm petclinic-crac
```
Now start a new instance of the petclinic from the image with the checkpoint in:
```
docker run -it --rm -p 8080:8080 --cap-add CHECKPOINT_RESTORE --cap-add SYS_PTRACE --name petclinic-crac petclinic-advanced-crac-restore
```

You should see the container start up very quickly!

## With the Chainguard CraC image

First lets remove the old images:
```
docker rmi petclinic-advanced-crac-restore petclinic-public
```

Build and run the image
```
docker build -t petclinic-cgr -f Dockerfile.cgr .
docker run --cap-add CHECKPOINT_RESTORE --cap-add SYS_PTRACE --name petclinic-crac petclinic-cgr
```
The image will run and create a checkpoint at the end.

When it's exited, we can create a new image from the exited container with `docker commit` - and change the entrypoint to restore from the checkpoint while we're doing that.

And, remove the exited image
```
docker commit --change='ENTRYPOINT ["java", "-XX:CRaCRestoreFrom=/checkpoint"]' petclinic-crac petclinic-advanced-crac-restore
docker rm petclinic-crac
```
Now start a new instance of the petclinic from the image with the checkpoint in:
```
docker run -it --rm -p 8080:8080 --cap-add CHECKPOINT_RESTORE --cap-add SYS_PTRACE --name petclinic-crac petclinic-advanced-crac-restore
```

You should see the container start up very quickly!