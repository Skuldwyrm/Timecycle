# Timecycle

## About

Timecycle is a kiosk loader and comes with a clock slideshow (hence the name). It fetches the slideshow list from a JSON file, load the images and show them. That's it.

Mount a old monitor on the wall (IPS or better adviced for good viewing angles), put a Raspberry PI or another tiny linux or Windows PC behind it, run a browser in some form of kiosk mode, point it to Timecycle. And you have a amazing digital wall clock.

* Responsive Design: Adjusts to different screen sizes using viewport units for a consistent display.
* Error Handling: Displays a clear error message if the image list cannot be fetched.
* Fade Transitions: Each slide fades in and out for a smooth visual effect.
* Regular Image Updates: Periodically checks if the image list has changed to keep the slideshow updated.
* Loading Placeholder: Shows a placeholder with an emoji while waiting for images to load.
* Slide Validity Check: Displays only valid images and hides the placeholder when a valid image is loaded.

### Background

This is a simplified concept based on another project of mine, Heraldex which was more complex than this. Heraldex was designed to be a wall clock but tried to be a lot more, and there was issues if network connection was lost, so it was far from a perfect kiosk display system. I wanted something even simpler and much more resilient if the WiFi was unstable for longer periods.

Timecycle is back to basics, it has only one purpose. But the code is flexible enough that one can fork this code and easily adapt it to other needs and has design choices to make that easier.

It runs permanently 24/7/365 as my wall clock. If the WiFi is having issues, it will be totaly unaffected as it essentially runs cached/locally on the kiosk PC browser.

### Timecycle and Timecycle Slideshow

Timecycle is the index.html file, Timecycle Slideshow is the clock.html file and is a "bundled" demonstration and is a lso how to practically use it. If you want to use something else than the slideshow clock them just change the iframe src url, and that's it.

The images are shown at one minute intervals, they are cached, so normally no more network activity is done. However the slideshow will check the json file at regular intervals if it has changed, if the json file has changed it is refetched and if the images has changed the current slideshow is replaced.

The index.html is a wrapper for clock.html which holds the clock and slideshow. The reason for this is to allow a stable "kiosk" wrapper, the kiosk index will check the last modified date of itself using a HTTP HEAD request, if changed it will refresh itself (which also refresh the clock slideshow file). The kiosk index shold normally never be changed, because if the code in that breaks then the refresh check could break. It will never get new functionality, only bug fixes if these are found or compatibility fixes are needed.

If the clock.html is edited or changed then the kiosk index.html file should be "touched" this will trigger the refresh and everything is reloaded. Why not have a single file with the refresh in it? Because the clock and slideshow code could break, in which case you would need to manually trigger a reload on your kiosk PC, which could be inconvenient if you have it tucked behind a screen with no mouse or keyboard and don't want to remote into it etc. It's just so more convenient to re-save the index.html so the filedate timestamp is updated. And if you have multiple of these around the place, they will all "auto update".

Any normal imnage should work, so .png .jpg. .webp .svg and whatever future standard formats browsers will support.

## Installation

1. Clone the repository to your local machine.
2. Due to CORS restrictions, Timecycle needs to be run on a web server. You can set up a simple local web server using Python or Node.js or PHP.

## Usage

Edit the images.json to add more images or change them.

To use the Timecycle Slideshow in a webpage (as seen in clock.html), create a new instance of the Timecycle class and pass the id of an iframe element.

```javascript
new Timecycle_Slideshow({
            slideContainerId: 'slides',
            errorMessageContainerId: 'error-message',
            placeholderContainerId: 'placeholder',
            slideDuration: 1, // Customize slide duration (1 minute)
            refetchInterval: 15 // Customize refetch interval (15 minutes)
        });
```

And as seen in index.html, to use the Timecycle kiosk "loader".

```javascript
        Timecycle.start(15);  // Refresh interval (15 minutes)
```

## License

Timecycle is licensed under [MIT](license.md).

## Copyrights
Digital-7 font ©️ 2008 Sizenko Alexander, sadly his website seems to no longer exist but you can find the font on many font websites, he is a programmer at Style-7 in Ukraine. I'm using the Monospace version and if I recall correctly I had to fix it because of some alignment issues, I also converted it to woff2 in the process. There are more variants of this as well other similar cool ones made by that font designer. I like Digital-7 as it has that Style-7 very retro LCD clock look, the font is also very tiny in filesize as it basically has just A-Z and 0-9 characters.
