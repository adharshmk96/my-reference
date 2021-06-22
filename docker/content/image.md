Docker Image


# Image
- Image is a single file with all dependancies and configuration associated with a software program.
- Consider image like ready to run file that can be run by docker.
- Contains a Filesystem snapshot and a startup command.
- <user>/<name>:<version>

## Dockerfile
- Set of instruction to build an image.
- Can be inherited from other images.

## Building image
`docker build /path/to/Dockerfile -t "imagename"`


# Container
- Set of a process and reserved resources which is seperated from eachother.
- Exclusive segments with namespacing combined with process isolation with control group