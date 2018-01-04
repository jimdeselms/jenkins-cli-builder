# A docker container to run the Jenkins CLI

The Jenkins CLI offers a ton of ways to automate Jenkins administration, and you can create a docker container to make it easy to run.

### Sexy use case ###
As we spin up new services and microsites, there's some overhead in creating Jenkins jobs for your new service. We can use the CLI, however, to download a job definition, make some tweaks to it, and re-upload it as a new job.

Here's a quick example that shows how to copy the CMS.SomeJenkinsJob job and then repost it under a new name.

```
docker run --rm jenkinscli get-job CMS.SomeJenkinsJob > job.xml
# Then, make some changes to the xml file, like changing the git repo that it watches
cat .\job.xml | docker run --rm -i jenkinscli create-job YourNewJob
```

### Building the image ###

Before you build this image, you'll have to pass in an authorization token which you can get from Jenkins **http://jenkins.storefront.vpsvc.com:8080/user/_yourName_/configure**

<pre>
docker build -t jenkinscli --build-arg AUTH=<i>yourName</i>:<i>yourApiToken</i> .
</pre>

For example:

<pre>
docker build -t jenkinscli --build-arg AUTH=fredwilkerson:4267b5d3451b372fd592613ee85e2e7fa</i> .
</pre>

### Running the container ###

The image exposes a shell command "jenkins" which you can either interactively, or by passing a command into the "docker run" command

#### Running interactively ####

```
docker run -it --rm jenkinscli
```

Then, when you get to the shell prompt, you can run the commands using the "jenkins" script:

```
root@4e0277d2636d:/usr/bin# jenkins help
root@4e0277d2636d:/usr/bin# jenkins who-am-i
```

#### Running as a one-line command ####

You can pass commands directly in "docker run" and they'll be passed directly into the "jenkins" script:

```
docker run --rm jenkinscli who-am-i
```

#   j e n k i n s - c l i - b u i l d e r  
 