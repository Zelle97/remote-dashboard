# Remote-dashboard

This is the dashboard frontend for the home-remote-app.
Its a simple vue application that communicates with a backend.

## Util

This application uses has 3 different utilities. It can 
 - Show if a host is Online or Offline
 - Shutdown a Host
 - Start a Host
 
Above utils are only calls to a server-backend which executes the necessary logic.
 
## Disclaimer

This application is for use in your secure home network only!
To use this you will need to be able to communicate with the server-backend.

## Prequesites

Make sure the communication between the dashboard and the specified backend is possible.

### Stand-Alone

If you want to use the remote-dashboard backend without the home-remote-compose file you can do this in three ways.

 1) Run the Docker Container without the compose from home-remote-app
 2) Clone this repo and execute ``` npm run dev ```.
 3) Run ``` npm run build ``` and copy the content of the dist directory into a webserver


### Docker and Architectures

This application was mainly developed to be used to start and shutdown other computers from a smaller device ( like a raspberry ).
The Docker images for this app are automaticly build from DockerHub.
But if you want to use this app on a raspberry you have to build the image yourself 
because DockerHub doesen't support the automated building of multiple architectures ( at this moment ).

To do so clone this repo and execute ``` docker build . -t zellesdocker/remote-dashboard ```.
This will build the image with the architecture of the system you are executing the command on.
 

##### Author & Licence

If you have any questions or improvement Ideas feel free to open an Issue or a feature request.

**Projects** © [Zelle97](https://github.com/Zelle97), All Projects are released under the MIT License.

> Authored and maintained by Zelle97 · GitHub [@Zelle97](https://github.com/Zelle97)
