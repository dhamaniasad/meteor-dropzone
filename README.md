# Meteor-Dropzone [![Build Status](https://travis-ci.org/devonbarrett/meteor-dropzone.png?branch=master)](https://travis-ci.org/devonbarrett/meteor-dropzone)

## Dropzone.js
[DropzoneJS](http://www.dropzonejs.com/) is an open source library that provides drag'n'drop file uploads with image previews.

## Compatibility
Intended for versions of Meteor 1.0

## Installation
```
    meteor add dbarrett:dropzonejs
```

## Usage
In your handlebar templates you can just include the template Dropzonejs:

```
    {{> dropzone url='http://somewebsite.com/upload' id='dropzoneDiv'}}
```
Which will post any uploaded files to the url of your choice.


id is optional, if you do not set one, a random string will be used.
*url is not optional* you must pass a url to the template.

### Options
If you would like more control over your Dropzone. You can pass the full range of options found in the Dropzone [documentation](http://www.dropzonejs.com/) to the template e.g.

```
    {{> dropzone url='http://somewebsite.com/upload' id='dropzoneDiv' maxFiles=4}}
```

Or you can set the default options that every dropzone instance is instantiated with globally:

```
	Meteor.Dropzone.options.maxFiles = 4;
```

These will be overridden by any parameters set in the template helper.

A full list of options can be found in the Dropzone [documentation](http://www.dropzonejs.com/)

## Example for upload on server.
Iron router is quite tricky for handling post data. Flow router is mostly for clietn side. Either you implement your own handler on a specific route or simply use: [metor-upload-server](https://github.com/tomitrescak/meteor-tomi-upload-server)

You can see an example-app using this method inside of the
[example-app](example-app) directory.

First add the server to your packages.
```
meteor add tomi:upload-server
```
Thanks to [tomi](https://github.com/tomitrescak) for his wonderful package.

Then use the following simple code in one of your server files:

```
UploadServer.init({
  tmpDir: '/tmp/',
  uploadDir: '/var/www/upload/',
  checkCreateDirectories: true,
  uploadUrl: '/upload/',
  // *** For renaming files on server
  getFileName: function(file, formData) {
  	return new Date().getTime() + '-' + Math.floor((Math.random() * 10000) + 1) + '-' + file.name; 
  	// we get this value in the ajax response
  }
});
```

Files can be renamed on the server as mentioned above. They can be used in the client as follows:
```
Template.tDropzone.onRendered(function () {
  var options = _.extend( {}, Meteor.Dropzone.options, this.data );
  
  // if your dropzone has an id, you can pinpoint it exactly and do various client side operations on it.
  if ( this.data.id ) {
    this.dropzone = new Dropzone( '#' + this.data.id + '.dropzone', options );

    var self = this;

    // this is how you get the response from the ajax call.
    this.dropzone.on('success', function(file, response) {
      var res = JSON.parse(response);
      file.fileNameOnServer = res.files[0].name;
    });
  } else {
    this.$('.dropzone').dropzone( options );
  }
});
```

**Pull requests are very welcome**

## Contributors
- [aramk](https://github.com/aramk)
- [Sudhanshu](https://github.com/s7dhansh)
- [bySabi](https://github.com/bySabi)
