# Stream SIM into Web-browser   

- Install [xpra](https://github.com/Xpra-org/xpra) by following the instructions.
- *OPTIONAL* install [virtualgl](https://virtualgl.org/), this may be required depending on the simulator you want to stream.
- After succesfully installing all the dependencies just run the following command:
```
xpra start --bind-tcp=0.0.0.0:8081 --html=path_to_www --exec-wrapper="vglrun -d :0" --start="python demo.py"
```
Modify the options: html, start to your proper setup. The option exec-wrapper can be ommited if vglrun was not able to be installed. If you are able to launch the web client but cant see anything poping up, just open the log file that appear when running this command for debugging.

# Docker
Just follow the docker implementation is pretty straigth forward. In case you happened to face a problem with the installation, just remove the step for virtualgl from the dockerfile.
```
docker compose up
```
Inside the docker you are using the DEFAULT html client included when installing xpra. To use your modified version you would have to integrate it with the immage.

# HTML client

You have to focus mainly on the index.html, sw.js, client.css

In order to see the changes reflected you will have to generate everytime a brotli file (corresponding to the modified file) and also the gunzip file.
