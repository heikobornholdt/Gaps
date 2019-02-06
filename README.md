# Gaps
Searches through your Plex Server for all movies, then queries for known movies in the same collection. If those movies don't exist in your library, Gaps will recommend getting those movies, legally of course.

## Usage
### Local Requirements

It's a Spring Boot App and you need Java to run this. You'll need to make sure Java is accessible in your command window or terminal. 

#### Windows 
This is a good write up on getting Java in the path variable on [windows](https://javatutorial.net/set-java-home-windows-10). 

#### Mac and Linux
You'll just need to add the Java classpath to your bash_profile.

### Properties
You need to find your plex movie all URL. Plex has a great write up on to get the URL. But first you'll need to get your token. The write ups are below.

https://support.plex.tv/articles/204059436-finding-an-authentication-token-x-plex-token/
https://support.plex.tv/articles/201638786-plex-media-server-url-commands/

It will look something like this.

http://127.0.0.1:32400/library/sections/1/all/?X-Plex-Token={My-Plex-Token}

Gaps supports a single movie URL or multiple. You can adjust to your needs. Then you need to go make an account on https://www.themoviedb.org and generate an API Key. Add that key to the application.yaml file under the movieDbApiKey property.
 ```yaml
  #Single
  #movieUrls:
  #  - http://127.0.0.1:32400/library/sections/1/all/?X-Plex-Token={My-Plex-Token}
  
  #Mulitple
  #movieUrls:
  #  - http://127.0.0.1:32400/library/sections/1/all/?X-Plex-Token={My-Plex-Token}
  #  - http://127.0.0.1:32400/library/sections/2/all/?X-Plex-Token={My-Plex-Token}
  #  - http://127.0.0.1:32400/library/sections/3/all/?X-Plex-Token={My-Plex-Token}
  movieUrls:
    - 

  #Go to https://www.themoviedb.org and make an API Key, place that key here
  #movieDbApiKey: {key}
  movieDbApiKey:

  #Should Gaps write out to a file as well as console
  writeToFile: true
  
  #Optional
  #Plex connection timeouts when querying for all movies in the Plex section. Time is in seconds. Default is 180 seconds
  plex:
    connectTimeout: 180
    writeTimeout: 180
    readTimeout: 180

```


### Running

You can run it with the run.bat or run.sh scripts. If you want to run command line you can run with. 
```bash
java -jar Gaps.{version-number}.jar
```
The output will come either to a file or the console. By default the output comes to a file. Gaps will print out information about duplicate movies and movies not found in the terminal. The program takes time but in the end you will get a list of movies missing from collections you have. If you have any questions, hit me up. Feel free to put up PRs to improve this as well.

### Running with Docker

First, the container must be built:

```bash
docker build -t gaps .
```

Next, run the container with the environment variables:

```bash
docker run -t -e DBAPIKEY= -e PLEXADDRESS= -e WRITETOFILE= gaps  
```

In example:

```bash
docker run -t -e DBAPIKEY=myapikey -e PLEXADDRESS=http://192.168.0.10:32400/library/sections/1/all/?X-Plex-Token=plextoken -e WRITETOFILE=true gaps

Multiple URL:
docker run -t -e DBAPIKEY=myapikey -e PLEXADDRESS=http://192.168.0.10:32400/library/sections/1/all/?X-Plex-Token=plextoken,http://192.168.0.10:32400/library/sections/2/all/?X-Plex-Token=plextoken  -e WRITETOFILE=true gaps
```

###Option Properties in Docker

CONNECT_TIMEOUT

WRITE_TIMEOUT

READ_TIMEOUT 

These are optional properties to help with Plex Sections that are very large. Timeouts can be set longer to help when parsing the big XML returned by Plex. They are not required and will default to 180 seconds.

```bash
docker run -t -e DBAPIKEY=myapikey -e PLEXADDRESS=http://192.168.0.10:32400/library/sections/1/all/?X-Plex-Token=plextoken -e WRITETOFILE=true -e CONNECT_TIMEOUT=180 -e WRITE_TIMEOUT=180 -e READ_TIMEOUT=180 gaps
```


## License
Copyright 2019 Jason H House

Permission is hereby granted, free of charge, to any person obtaining a copy of this software and associated documentation files (the "Software"), to deal in the Software without restriction, including without limitation the rights to use, copy, modify, merge, publish, distribute, sublicense, and/or sell copies of the Software, and to permit persons to whom the Software is furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.

