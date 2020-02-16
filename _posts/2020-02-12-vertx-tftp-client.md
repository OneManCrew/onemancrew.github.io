---
layout: post
title: Vert.x TFTP Client
subtitle: 
image: /img/tftp.jpg
tags: [Java,Vert.x,TFTP]
---

# Vert.x TFTP Client
Trivial File Transfer Protocol (TFTP) is a simple lockstep File Transfer Protocol which allows a client to get a file from or put a file onto a remote host. One of its primary uses is in the early stages of nodes booting from a local area network. TFTP has been used for this application because it is very simple to implement. 
TFTP was first standardized in 1981 and the current specification for the protocol can be found in RFC 1350. 

vertx-tftp-client is a simple  async tftp client which works with vert.x

The Following Java project implements the TFTP protocol and builds a TFTP client. Scope of the client is to upload and download files from a remote TFTP server.
I have used Vert.x to implement this project as asynchronously.

To use this component, add the following dependency to the dependencies section of your build descriptor:
[ ![Download](https://api.bintray.com/packages/onemancrew/vertx/vertx-tftp-client/images/download.svg?version=1.0) ](https://bintray.com/onemancrew/vertx/vertx-tftp-client/1.0/link)
Maven (in your pom.xml):
````
<dependency>
  <groupId>io.github.onemancrew</groupId>
  <artifactId>vertx-tftp-client</artifactId>
  <version>1.0</version>
</dependency>
````
Gradle:
````
implementation 'io.github.onemancrew:vertx-tftp-client:1.0'
````
ivy:
````
<dependency org="io.github.onemancrew" name="vertx-tftp-client" rev="1.0">
	<artifact name="vertx-tftp-client" ext="pom"></artifact>
</dependency>
````
#### Creating TFTP Client
````
Vertx vertx = Vertx.vertx();
TftpClient client =new TftpClient(vertx,tftpServerIp,port);//default port 69
``````

#### Upload files
````
client.upload("filePath",(progress)->{
    //progress will update every change in the upload progress.
    },
    (result)->{
    if (result.succeeded()) {
        System.out.println("upload succeeded");
      } else {
        System.out.println("error upload file" + result.cause().getMessage());
      }
});
````

#### Download file
````
client.download(fileName,downloadFoler,(result)->{
    (result)->{
        if (result.succeeded()) {
            System.out.println("download succeeded");
          } else {
            System.out.println("error download file" + result.cause().getMessage());
          }
});
````
#### Error Code Description
In case of TttpError Exception this id the description for each error code:

| ErrorCode |            Description            |
|:---------:|:---------------------------------:|
|     1     | File not found.                   |
|     2     | Access violation.                 |
|     3     | Disk full or allocation exceeded. |
|     4     | Illegal TFTP operation.           |
|     5     | Unknown transfer ID.              |
|     6     | File already exists.              |
|     7     | No such user.                     |

#### TFTP Upload Protocol:
![Upload diagram](/img/upload.svg)

#### TFTP Download Protocol:
![Download diagram](/img/download.svg)
