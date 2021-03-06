# LIPR

LIPR is an unofficial Objective-C framework for programmatically addressing BERG's [Little Printer](http://bergcloud.com/littleprinter/) [Direct Print API](http://remote.bergcloud.com/developers/littleprinter/direct_print_codes). LIPR has been developed for using it with OS X 10.9 Mavericks, but most classes should also work on iOS and with earlier versions of OS X, though I haven't tested this. 

BERG's Little Printer [ceased trading in 2014](http://littleprinterblog.tumblr.com/post/97047976103/the-future-of-little-printer), but this code could still be of some use.

## Usage

Just include the framework in your Xcode project and create an instance of the LIPrinter class with the following line of code:

	LIPrinter *printer = [[LIPrinter alloc] initWithPrinterAccessCode:@"XXXXXXXXXXXX"];
	
With "XXXXXXXXXXXX" being your printer's access code, as to be retrieved from [BERG'S developer website](http://remote.bergcloud.com/developers/littleprinter/direct_print_codes). 

You can then address your Little Printer by using one of the following public methods of the LIPrinter instance visible via the header file, for example:

	[printer printTextMessage:@"Hello World!"];

If you want to be notified whether your call to the LIPrinter instance was successful or not, you have to set your application delegate (or any other of your application's objects) as an delegate to the LIPrinter instance:

	[printer setDelegate:self];
	
You will then be notified via the LIPrinterProtocol of your outcome of any call to the LIPR framework.

The LIPrinter object so far possesses public methods for printing strings, images and formatted html:

	-(NSString*) printTextMessage:(NSString*)text;
	-(NSString*) printImageMessage:(NSImage*)image;
	-(NSString*) printMessageWithHeading:(NSString*)heading andText:(NSString*)text;
	-(NSString*) printHTML:(NSString*)html;

These classes should be easy enough to use, the all will return a unique message ID string, comparable to a hash string, for identifying the concrete message; this can be useful if you listen to LIPrinter's delegate notifications. 

For more complex use cases, especially the printHTML: method, I'll publish a sample project using the LIPR framework for printing a Twitter timeline on GitHub separately.

For a more detailed description of the LIPR framework, please see the [documentation](DOCUMENTATION.md). 

## Known Issues and Caveats

* BERG seems to have a size limit for any content sent to a Little Printer via the Direct Print API in the region of 200 KBytes; this is at the moment not monitored by the LIPR framework as the API doesn't return an error code in this case. If the content is too large, the Little Printer will print only a short empty sheet of paper.
* Little Printers only accept specific HTML formattings, so you will have to experiment what is allowed and what is not.
* Image rendering (dithering) is already implemented in the LIPR framework but could possibly be improved.
* Image loading, if necessary for rendering, is done via a synchronous loading at the moment, which works quite well, but should be replaced with asynchronous loading for better performance.
* BERG stresses that their Direct Print API is a temporary API and will be replaced by a permanent solution. It is very likely that the LIPR framework will be affected by such a change. 

## License

The (MIT-style) license for this source code is contained in the [license.md](LICENSE.md) file. In a nutshell, as long as you give appropriate attribution, you're free to use the source code for any purpose.