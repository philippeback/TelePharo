# TelePharo
Complete toolset for remote development of Pharo images. It includes:

- remote playground
- remote browser
- remote debugger
- remote inspector

## Server installation
Server part of project should be installed on target image:
```Smalltalk
Metacello new
  baseline: 'TelePharo';
  repository: 'github://dionisiydk/TelePharo';
  load: 'Server'.
```
Then server should be started on port where client image can connect:
```Smalltalk
TlpRemoteUIManager registerOnPort: 40423
```
Image can be saved with running server. It will be automatically started when image restarts. Or you can use command line option for this:
```bash
./pharo PharoServer.image remotePharo --startServerOnPort=40423
```
## Client connection
On IDE image client side of project should be installed:
```Smalltalk
Metacello new
  baseline: 'TelePharo';
  repository: 'github://dionisiydk/TelePharo';
  load: 'Client'.
```
And then you can connect Pharo to remote image:
```Smalltalk
remotePharo := TlpRemoteIDE connectTo: (TCPAddress ip: #[127 0 0 1] port: 40423)
```
It registers local debugger and browser on remote image:

Any error on remote image will open debugger on client image with remote process stack
Any browser request on remote image will open browser on client image with remote packages and classes
User requests from server are redirected to client. Any confirm or inform requests from remote image will be shown on client. For example author name request will be shown on client image where user can type own name remotely.

With remotePharo instance you can open remote browser and playground:
```Smalltalk
remotePharo openPlayground.
remotePharo openBrowser
```
And you can evaluate remote scripts:
```Smalltalk
remotePharo evaluateAsync: [ [1/0] fork ].
remotePharo evaluate: [ 1 + 2 ] "==> 3".
remotePharo evaluate: [ 0@0 corner: 2@3 ] "==> aSeamlessProxy on remote rectangle".
```
Some operations are not working remotely. For example #debugIt command and refactorings lead to errors. But in future they will be supported.

TODO: Look at original project post for more details about scripting and inspecting.
