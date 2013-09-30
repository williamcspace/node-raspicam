# node-raspicam

A Node.js-based controller module for the Raspberry Pi camera.

_Note_: This should work well for photo and timelapse, video has yet to be tested but may work as well.

## To Install

	npm install raspicam

## To Use

Require raspicam in your node app, then used the exposed constructor to create a RaspiCam object that you can use to take photos, start a timelapse, or record video.

	var RaspiCam = require("raspicam");

	var camera = new RaspiCam({ opts });

	//to take a snapshot, start a timelapse or video recording
	camera.start( );

	//to stop a timelapse or video recording
	camera.stop( );

	//listen for the "started" event triggered when the start method has been successfully initiated
	camera.on("started", function(){ 
		//do stuff
	});

	//listen for the "read" event triggered when each new photo/video is saved
	camera.on("read", function(err, filename){ 
		//do stuff
	});

	//listen for the process to exit when the timeout has been reached
	camera.on("exited", function(){
		//do stuff
	});


### RaspiCam Constructor

The RaspiCam() constructor creates an object that can then be triggered to take a snapshot or start a timelapse or video recording. The constructor options mirror those offered by the raspistill and raspivideo commands provided by the Raspberry Pi Camera API.


### RaspiCam Constructor Options

#### Required

`mode` : String - "photo", "timelapse" or "video"
`output` : String - the path and filename where you want to store the photos or video (use sprintf-style variables, like %d, for incrementing timelapse photos)

#### Optional

##### Image parameter commands

###### Parameters with Values

`w`, `width` : Integer or String - Set image width <size>
`h`, `height` : Integer or String - Set image height <size>
`q`, `quality` : Integer or String - Set jpeg quality <0 to 100>
`t`, `timeout` : Integer or String - Time (in ms) before takes picture and shuts down (if not specified, set to 5s)
`th`, `thumb` : String - Set thumbnail parameters (x:y:quality)
`e`, `encoding` : String - Encoding to use for output file (jpg, bmp, gif, png)
`x`, `exif` : String - EXIF tag to apply to captures (format as 'key=value')
`tl`, `timelapse` : Integer or String - Timelapse mode. Takes a picture every <t>ms

###### Flags

`r`, `raw` : Add raw bayer data to jpeg metadata
`v`, `verbose` : Output verbose information during run
`d`, `demo` : Run a demo mode (cycle through range of camera options, no capture)


##### Preview parameter commands

###### Flags

Note: I've kept these in for completeness, but I'm not sure how they will be useful.

-p, --preview	: Preview window settings <'x,y,w,h'>
-f, --fullscreen	: Fullscreen preview mode
-op, --opacity	: Preview window opacity (0-255)
-n, --nopreview	: Do not display a preview window


###### Image parameter commands

###### Parameters with Values

`sh`, `sharpness` : Integer or String - Set image sharpness (-100 to 100)
`co`, `contrast` : Integer or String - Set image contrast (-100 to 100)
`br`, `brightness` : Integer or String - Set image brightness (0 to 100)
`sa`, `saturation` : Integer or String - Set image saturation (-100 to 100)
`ISO`, `ISO` : Integer or String - Set capture ISO
`ev`, `ev` : Integer or String - Set EV compensation
`ex`, `exposure` : String - Set exposure mode (off,auto,night,nightpreview,backlight,spotlight,sports,snow,beach,verylong,fixedfps,antishake,fireworks)
`awb`, `awb` : String - Set AWB mode (off,auto,sun,cloud,shade,tungsten,fluorescent,incandescent,flash,horizon)
`ifx`, `imxfx` : String - Set image effect (none,negative,solarise,sketch,denoise,emboss,oilpaint,hatch,gpen,pastel,watercolour,film,blur,saturation,colourswap,washedout,posterise,colourpoint,colourbalance,cartoon)
`cfx`, `colfx` : String - Set colour effect (U:V)
`mm`, `metering` : String - Set metering mode (average,spot,backlit,matrix)
`rot`, `rotation` : Integer or String - Set image rotation (0-359)

###### Flags

`vs`, `vstab` : Turn on video stablisation
`hf`, `hflip` : Set horizontal flip
`vf`, `vflip` : Set vertical flip



### RaspiCam Object Methods

#### RaspiCam.start( )

Depending on the `mode`, this will either take a snapshot ("photo"), start a timelapse ("timelapse") or start a video recording ("video"). You can then set up a listener for a `"read"` event that the RaspiCam process will emit when the snapshot or video is complete, and each time a timelapse photo is taken.

You can only call start() once on a RaspiCam object, as the same physical camera cannot do multiple captures at once.

Returns `false` if any errors, otherwise returns `true`.


#### RaspiCam.stop( )

This stops any ongoing camera process.

Returns `true` if it stopped a process, otherwise returns `false`.



#### RaspiCam.set( opt, value )

This is a setter - it sets any option you give it. Opt must be a string (eg. "width").


#### RaspiCam.get( opt )

This is a getter - it returns any option you give it. Opt must be a string (eg. "width").



