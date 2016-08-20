## development-meteor - Docker Runtime for non packaged Meteor Apps

This image is made to run Meteor apps as you would run them locally with `meteor`. It is basically a Node Image with Meteor preinstalled.

This would allow you to deploy a debug version of your app on your server and share it with your team/testers.

To use you would need to add the following to a `Dockerfile` in your Meteor app's root directory:
```
FROM tzapu/development-meteor:1.4.1
WORKDIR /opt/app/
ADD . /opt/app/
RUN meteor npm install
CMD meteor;
```

What this does is:
- get the development-meteor image with Meteor version 1.4.1 (will try to provide up to date Meteor versions. you can also use `latest`)
- sets the working directory
- copies all your source files to the working directory
- installs all your `package.json` dependencies
- finally runs Meteor

It is recommended you have a `.dockerignore` file at the same level as your Dockerfile, containing:
```
node_modules
.meteor/dev_bundle
.meteor/local
```

To build you would then issue a command like
```
sudo docker build -t user/image-name .
```

When you are running this image, do not forget to map a port to internal port 3000 on which Meter's built in webserver will run. Sample run command:
```
sudo docker run -d
  -e MAIL_URL=smtp://postmaster@domain.com:rnd@smtp.mailgun.org:587
  -p 80:3000
  --name container-name user/image-name'
```

I hope you find it useful.
