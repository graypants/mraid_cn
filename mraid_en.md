
Interactive Advertising Bureau 

Mobile Rich-media Ad Interface Definitions (MRAID) v.2.0 

Released September 27, 2012, 

Revised With Clarifications April 16, 2013

#	Contributors
The IAB MRAID Working Group includes representatives from the following companies:
<pre>
24/7 Real Media, Inc. 					Medialets
AccuWeather.com 						MediaMind
AdMarvel								Microsoft Advertising
AdMeld									Mixpo
ADTECH									Mocean Mobile
Adobe Systems Inc. 						NBC Universal Digital Media
AOL										New York Times Co.
CBS Interactive							Nexage
Celtra									Pandora
Crisp Media								PointRoll
Dow Jones & Company						Rhythm NewMedia
ESPN									Spongecell
FreeWheel								Sprout
Goldspot Media							TargetSpot
Google									Time Inc.
Greystripe								Turner Broadcasting System, Inc.
IDG										Univision
inMobi									The Weather Channel
Innovid									Yahoo!, Inc.
Jumptap
</pre>

#	Acknowledgement
The IAB acknowledges the contributors to the ORMMA.org API project, which provided a starting point for this document. ORMMA.org is a group of industry thought leaders who have worked together since Spring 2010 to develop and test a complete and versatile mobile rich media ad API. Contributors to ORMMA at the point the IAB launched the MRAID project included:
<pre>
Adam Schuetz, AdMarvel					Philippe Laporte, Goldspot Media
Dennis Doughty, Jumptap					Robert Hedin, The Weather Channel
Jon Badenell, The Weather Channel		Todd Pasternack, Pointroll
Nathan Carver, Crisp Media				Wook Chung, Google
Neal Karasic, Jumptap					Xavier Facon, Crisp Media
</pre>

#	About MRAID
The Interactive Advertising Bureau (“IAB”), its members and other significant contributors joined together to create this document, a standard interface specification for mobile rich media ads. The goal of the Mobile Rich-media Ad Interface Definition (MRAID) project is to address known interoperability issues between publisher mobile applications, different ad servers and different rich media platforms.

##	IAB Contact Information

Joe Laszlo, Senior Director, IAB Mobile Marketing Center of Excellence, mobile@iab.net

#	Executive Summary
As rich media display advertising in mobile applications and on the mobile web has become more popular over the last several years, various innovative companies have accepted the challenge of creating an ecosystem for mobile ad serving. Innovation in mobile rich media ad serving has led to many exciting possibilities for content publishers and advertisers, but it has also created inefficiencies that often delay and inhibit the optimal monetization of content.

Simplifying the process for designers of mobile in-app ad creatives significantly increases the likeliness that agencies will leverage mobile into their media buys. Advertisers want to review compelling creative, approve it and decide to buy a specific inventory of mobile media, regardless of which device platform, application, or technology is used to display the media.

#	Definitions
The following terms are used throughout the MRAID specification.

**Ad View/Container:** The constrained area which displays the ad creative. The container provides the area on the screen, the MRAID controller, and the web-based view for the ad to display. Ad Containers are usually, though not necessarily, provided by SDKs. An app may contain multiple Ad Containers from a single SDK.

**Close Event Region:** The close event region is a tappable area on the ad creative that will cause the ad to return to its default state (in the case of an expandable/resizeable ad) or be removed from the screen (in the case of an interstitial).

**Close Indicator:** The close indicator is the visual cue to the user as to the location of the close event region.

**Controller:** The JavaScript code that provides ad designers access to MRAID methods and events. The ad creative uses the controller to perform advertising-related interactions with the Ad Container, and, indirectly, with the application and the device.

**Density-Independent Pixels:** All length values passed between the container and the creative through the MRAID API are in density-independent pixels.

Density-independent pixels are an abstraction from physical screen pixels meant to simplify application and content development across devices of different screen densities.

Using density-independent pixels means that, for example, retina display iPhones and older iPhones will return the same dimensions/measures, despite having different numbers of physical pixels. 1 density-independent pixel corresponds roughly 1/160 of an inch (1 device pixel on a device with roughly 160 DPI).

On iOS, these should map to “points”; on Android, to “density-independent pixels”.

Note: One density-independent pixel will match 1 CSS pixel only if the viewport scale is 1.0. To map between CSS pixels and density-independent pixels, the creative should use the following formula:

`css_pixels * viewport_scale = density_independent_pixels`

**Inline Ad:** An ad that appears onscreen accompanied by other kinds of content, e.g., a banner on a web page or in an app.

**Interstitial Ad:** A full page modal ad that displays on top of content -- a "roadblock" or "overlay." The ad must be dismissed for the user to return to the publisher content. Such ads can appear between levels of a game, or before or after a video clip or other dynamic content. (An ad that is in-between pages and swipes into view like in many magazine apps, is considered an inline ad under MRAID.)

**Physical Pixels:** The actual pixels on a device screen. For example, a retina-display iPhone measures 960x640 physical pixels. MRAID API length values are always calculated in density-independent pixels (defined above) NOT physical pixels.

**SDK:** Sofware Development Kit. The reusable piece of code (library) integrated into publisher apps to enable advertisements/Ad Containers. An SDK, by itself, is not a visual component.

**Web View:** The HTML-based viewer that displays the ad creative. The web view is used to perform rendering of HTML- and Javascript-enabled ads.

![Definitions](./images/a.jpg)

#	General Requirements for Supporting MRAID
This section details the requirements of an in-app ad-serving SDK that is MRAID compliant.

It is expected that an implementation would be in two parts. The first part defines a native container for rich media ads to display in apps and the second part defines a JavaScript controller for ad creatives to interact with. The native container encapsulates an HTML and JavaScript enabled web view, such as iOS’s UIWebView, and the controller serves as a bridge that can integrate HTML-based ads with the native capabilities. Actual implementations may vary.

When planning, key design considerations are:

* Access to the device’s native features (orientation, location, acceleration, etc.) in a consistent manner, where allowed by the app publisher/ad seller.
* Industry standard Ad development (HTML and JavaScript)
* Progressive complexity (simple things are simple, complex things are possible but harder)

##	_Technical Audience_
The specifications are technical by nature, but are not intended to limit innovation. This document is intended for Publishers or SDK vendors and addresses the needs of the Ad Designers.
>### 	Native Application Developer
There are no requirements in this specification for app developers. They should follow the instructions provided by their SDK developer for integrating ads into their application.
>###	SDK Developer
SDK builders have a number of responsibilities outside this recommendation. (See “out-of-scope.”) As mentioned, it is expected that the SDK developer will provide two interfaces to implement these recommendations: a container for the native developer to integrate via the SDK and a controller for the ad designer to use directly.
>This document outlines the requirements of the controller needed by the ad designer. It is the intention of the writers that these concepts can be managed with a facade layer for existing SDKs.
>###	Ad Designer
There are no creative requirements in this document for ad designers besides the use of web standards. Ad designers who use the methods in this specification can provide consumers with a rich media experience across platforms and publishers.				
		
It is important for ad designers to recognize that calls to the native device must be asynchronous by design. For most web developers, this is analogous to AJAX programming.

##	_Viewport and Default Container Set-Up_
Creative designers should be aware of the need to understand and potentially override the default settings of the web view in which they are running. They should do this by querying an MRAID container just as they would query a web page to understand the environment there. These settings include height and width of the container, scale, and whether the user can change the scale of the container.

While MRAID does not establish any new parameters or controls over the web view, it is recommended that the creative should check and adjust the parameters, since the author of said creative might wish to set them differently from the default container settings..

##	_Out of Scope_
Each MRAID implementation provides unique features sets to developers. This document outlines a minimum set of features for interoperability and does not define features that may also be part of an SDK such as

* Retrieving the ad from Ad Server, Ad Network, or local resources
* Reporting
* IDE integrations
* Security / Privacy
* Internationalization
* Error reporting
* Logging
* Billing and payments
* Ad dimensions and ad behavior
* Downloading of assets to the local file system for caching or off-line use

Of course, the SDK developer must implement the ability to render web content in the area intended for the ad unit. For most environments, this capability is already available as a web view component although the developer may have to develop additional functions to support these specifications.

It is the intent of the writers that vendors are not limited to delivering only the features outlined in the API. They should continue to innovate and present features that differentiate them in the marketplace. These other features must be implemented outside the MRAID namespace.

An ad that uses SDK-specific features in addition to MRAID features would not necessarily be an MRAID ad anymore in the sense of working across all MRAID-compliant SDKs.

##	_Standard Web Technologies_
For interoperability, only web compatible languages should be used for markup and scripting languages. This document assumes HTML/JavaScript/CSS. The ad designer should be able to develop and test the ad unit in a web browser. If designers use tags, styles and functions which are compatible with only one browser (such as CSS3 on WebKit), then the ad should be targeted to compatible devices.

When newer web standards can provide consistency, ad designers are encouraged to use them. This may include protocols like sms: and tel:, as well as some widely implemented portions of the as-yet unfinished HTML5 specification. Designers need to be aware that in these cases, the expected protocols and implementations may not be truly interoperable across all devices and platforms.

##	_Ad Server Requirements_
The ad server used to traffic rich media ads should support HTML ads with JavaScript.

##	_Requirements for Ad Rendering_
###	Display of HTML Ads – Ad View Container
An MRAID-compatible container must display any HTML ad. The container should invoke an HTML with JavaScript rendering engine for rendering ads. In this document, that engine will be called the "web view". Whenever possible, the web view should incorporate the capabilities of the device web browser. For example, iOS developers may use UIWebView. A given App view can have one or more ad view containers that will all act independently of one another.

##	_Requirements for Ad Designers_
###	Display Control for Rich Media Ads – Ad Controller
An ad designer that expects his/her ad to make use of MRAID must indicate that by invoking the mraid.js script as soon as possible as the ad loads. This signals the SDK to inject the MRAID javascript into the creative.

MRAID remains in the background, leaving the ad designer in control of the ad display, but is available so that the creative can use the MRAID API when/if the ad needs to access MRAID features and functionalities. The internal interaction between the creative and the rich media SDK is hidden from both the Ad designer and the App developer.

An ad that does not utilize any device features does not need to use the MRAID API at all. However if the ad does not invoke MRAID, it will get the SDK’s default solution. Some of the things an ad uses MRAID’s API for are:

* Opening an embedded web browser
* Detecting whether the ad is viewable or not
* Expanding an ad that grows from a banner to a larger size
* Clicking within an ad triggering an action

Ad designers are encouraged to rely on MRAID’s capabilities to achieve the above effects.

##	_Lifecycle Examples_
###	Simple Ad Lifecycle Example
Non-rich-media ads (e.g., basic banners) can optionally invoke MRAID. If the ad does not invoke MRAID via the mraid.js script tag, then it will behave however the application/SDK normally handles such ads.

Ad designers may wish for such simple HTML ads to invoke mraid.js, if they want to use the MRAID-standard container rather than the SDK’s default container. In that case, the ad designer should make sure to use mraid.open() for any links to ensure consistent behavior.

###	Lifecycle of an MRAID Expandable Ad Example
In a rich media ad lifecycle example, the Ad Designer uses the JavaScript API to communicate with the native layer and interact with features of the device and OS.
![Lifecycle](./images/b.jpg)
As an example, when the user touches the ad, the ad uses the MRAID API to request that the ad can expand. The SDK should (though this is not part of the MRAID specification) notify the app that the ad is expanding so that it can stop anything that the user will not be able to interact with. The SDK then resizes the web view to take up the entire screen of the device or the full size of the expanded ad. The container reserves a space at the top right corner of the expanded ad container for an MRAID-enforced close event region, and will either supply the close indicator or, if the ad specifies, will allow the ad to supply the indicator in creative.

When the user is done with the expanded ad, they click a close button that causes the ad to resize to its original size, display the ad’s banner state, and notify the app that it can resume.

###	Lifecycle of an MRAID Interstitial Ad
The case of an interstitial ad is very similar. The ad can use the MRAID API to query the container as to whether it is visible onscreen or not, waiting until it is on before it takes other actions. As with an expandable, the container reserves a space at the top right corner of the expanded ad for an MRAID-enforced close event region, and will either supply the close indicator or, if the ad specifies, will allow the ad to supply the indicator in creative. When the user is done with the interstitial, they can tap the close button, which in this case changes the ad’s state to “hidden,” unregisters any event listeners, and notifies the app to resume.

##	_MRAID Versions_
The adoption of MRAID throughout the ad community is a high priority and essential for the success of mobile rich media advertising across platforms. For this reason, IAB is releasing the full feature set of MRAID in versions. This will allow SDK vendors to meet the compliance standards of the MRAID API in a consistent way and prevent possible fragmentation inherit in implementing only a portion of the standard.

Maintaining full backwards compatibility in MRAID is a key goal of this project. An MRAID 2.0-compliant SDK should be able to run an MRAID 1.0 ad with no problems whatsoever, and an MRAID 1.0-compliant SDK should be able to handle the MRAID 1.0-compliant features of an MRAID 2.0 ad.

In establishing both versions of MRAID, the IAB and its MRAID working group have focused on six key goals:

* __High interoperability__ – ads developed to run in one MRAID container can run on MRAID containers of multiple platforms and operating systems.
* __Graceful degradation__ – ads developed to take advantage of all the MRAID features also have the capacity to downgrade gracefully as needed. This will be especially important as gaining access to device functionalities becomes part of MRAID’s scope in the future.
* __Progressive complexity__ – ad design using the API should be simple, adding complexity only as necessary.
* __Consistent means for ads to change size and/or open new pages, preferably in an embedded browser__ –MRAID provides ads a consistent way to communicate with rich media SDKs regarding their need to expand, and open an app’s embedded browser (or in the native browser if an embedded browser does not exist).
* __Consistent means for a consumer to exit an ad__ – MRAID ads will always have a consistent control by which a user can indicate that they wish to exit out of the ad experience and return to the app/content they were in.
* __Flexibility for publishers__ – although MRAID-compliant SDKs must support all MRAID capabilities, app publishers/ad sellers are free to allow or disallow ads that make use of the features MRAID enables. That is, MRAID enables rich media ad features, but does not dictate that all sellers of rich media ads must support all those features.

###	Version 1
The methods and events included in MRAID Version 1 provide a minimum level of requirements for rich media ads, primarily to display HTML ads that can change size in a fixed container (e.g., expand from banner to larger/full screen size), and interstitial ads.

###	Version 2
MRAID Version 2 extends the capabilities of MRAID Version 1 to give ad designers more control over expandable ads, and provides a new method, resize() that permits more subtle and interesting size changes in ad creatives as well.

In addition, MRAID v.2 provides a standard way to query a device regarding certain capabilities, offers consistent handling of video creative, and addresses two native capabilities not well implemented by HTML5 at present: adding an entry to the device calendar and storing an image in the device photo roll.

For examples of ads that can be developed using the MRAID Version 2 API, please see the addendum.

#	Interface Requirements and Definitions
This list outlines all the methods and events that ad designers will have access to under MRAID v2.0. Methods and events new in MRAID v2.0 (e.g., that were not in MRAID v1) are indicated by an asterisk below.

**Methods**
<pre>
	addEventListener					getVersion
	createCalendarEvent*				isViewable
	close 								open
	expand 								playVideo*
	getCurrentPosition*					removeEventListener
	getDefaultPosition*					resize
	getExpandProperties					setExpandProperties
	getMaxSize*							setResizeProperties*
	getPlacementType					storePicture*
	getResizeProperties*				supports*
	getScreenSize*						useCustomClose
	getState
</pre>

**Events**
<pre>
	error 								stateChange
	ready 								viewableChange
	sizeChange*
</pre>

##	_Identification_
It is required that ads identify themselves as being MRAID compliant. This is done by adding an MRAID script reference as soon as possible the creative and well before any MRAID functions are referenced in the creative. In other words, the MRAID identification script reference must be identifiable as soon as possible by any MRAID-compliant container or SDK.

**MRAID** script reference

The MRAID tag follows HTML Javascript syntax so that both fully formed web pages and HTML fragments can be identified as MRAID ads. mraid.js will be included as a script in the document either using an HTML tag or as javascript. MRAID sample ads ([www.iab.net/mraid](http://www.iab.net/mraid)) illustrate where the script tag should be positioned.

`<script src="mraid.js"></script>`

While MRAID ads need to identify themselves as such via the mraid.js script in a timely fashion so that the container can inject the MRAID libraries, ad designers should avoid using the string “mraid.js” for any other purpose in an ad creative, as doing so may lead containers/SDKs to mistakenly inject multiple copies of the MRAID libraries.

##	_Initialization_
MRAID governs interactions between the ad and the container and identifies the container as compatible with these specifications. Ad designers must include the JavaScript identification reference for MRAID, but the actual JavaScript libraries are supplied by the container, and it is the responsibility of the container to ensure they are available to the ad in a timely fashion after the script reference is made, and to signal as such by firing the ready event.

The following summarizes step-by-step the actions that the ad and MRAID container take in the initial loading of the ad and the injection of MRAID API libraries.

1. Ad identifies itself as MRAID as early as possible by invoking MRAID script tag.`<script src="mraid.js"></script>`
2. SDK/MRAID-compatible Container

	a. Optionally detects the script call

	b. Always provides the MRAID JavaScript bridge for MRAID ads

	c. Provides limited MRAID object with an MRAID State = “loading” and the ability to query the state

3. If the ad uses createElement, needs to wait for mraid.js to finish loading
4. If MRAID State=”loading” then ad listens for “ready” event with mraid.addEventListener('ready')
5. SDK/Container finishes initializing MRAID library into the webview

	a. Changes the MRAID state to “default” and the StateChange Event is triggered

	b. Fires the MRAID “ready” event

6. Ad's "ready" event listener is triggered and ad JavaScript can now use the MRAID APIs

**ready** event

The ready event triggers when the container is fully loaded, initialized, and ready for any calls from the ad creative.

It is the responsibility of the MRAID-compliant container to prepare the API methods before the ad creative is loaded. This prevents a condition where the ad cannot register to listen for the ready event because the API methods are unavailable. While the container may load all of MRAID at once, at a minimum the container must be prepared to support the getState and addEventListener capabilities as early as possible in the ad loading process; otherwise there will be no way for the ad to register for the ready event. In the event that the container may still need more time to initialize settings or prepare additional features, ready should only fire when the container is completely prepared for any MRAID request.

The ad should always attempt to wait for the ready event before executing any rich media operations. Because of timing issues, such as the ready event firing before the ad has registered to listen, ad designers should use the ready event in conjunction with the getState() method.

Example

```
function showMyAd() {
    ...
}
if (mraid.getState() === 'loading') {
    mraid.addEventListener('ready', showMyAd);
} else {
    showMyAd();
}
```

“ready”
<pre>
parameters:
· none
side effects:
· MRAID JavaScript library available to ad unit 
return values:
· none
event triggered:
· none
</pre>

**getVersion** method

The getVersion method allows the ad to confirm a basic feature set before display. This version number must correspond with the MRAID version specification (e.g., 1.0 or 2.0) and not the vendor’s SDK version.

getVersion() -> String
<pre>
parameters:
· none
return values:
· String – the MRAID version that this SDK is certified against by the IAB, or that this SDK is compliant with. For example, for the current version of MRAID, getVersion() will return “2.0.”
</pre>

##	_Initial Display_
It is up to the ad designer to provide simple HTML, such as an `<img>` tag, for the initial display of their ad while other assets are loaded in the background. This HTML will be displayed in the Container while JavaScript uses the Controller to request and invoke additional capabilities. Ultimately, the initial HTML display may be completely replaced by a rich media ad once all assets are ready, depending on the creative requirements.

##	_Event Handling_
Event handling is a key concept of MRAID. Communicating between the web layer and native layer is asynchronous by nature. Through event handling, the ad designer is able to listen for particular events and respond to those events on an as-needed basis. MRAID advocates broadcast-style events to support the broadest range of features/flexibility with the greatest consistency.

The controller exposes these methods.

**addEventListener** method

Use this method to subscribe a specific handler method to a specific event. In this way, multiple listeners can subscribe to a specific event, and a single listener can handle multiple events. An ad may register for more than one listener at a time, and it is required that ads be permitted to do so. The events supported by MRAID v.2 are:

<table border="1" cellpadding="3">
	<tr>
		<th>value</th>
		<th>description</th>
	</tr>
	<tr>
		<td>ready</td>
		<td>report initialize complete</td>
	</tr>
	<tr>
		<td>error</td>
		<td>report error has occurred</td>
	</tr>
	<tr>
		<td>stateChange</td>
		<td>report state changes</td>
	</tr>
	<tr>
		<td>viewableChange</td>
		<td>report viewable changes</td>
	</tr>
	<tr>
		<td>sizeChange</td>
		<td>report a change in size of the ad</td>
	</tr>
</table>

addEventListener(event, listener)
<pre>
parameters:
· event – string, name of event to listen for
· listener – function to execute
return values:
· none
side effects:
· none
</pre>

**removeEventListener** method

Use this method to unsubscribe a specific handler method from a specific event. Event listeners should always be removed when they are no longer useful to avoid errors. If no listener function is specified, then all functions listening to the event will be removed.

removeEventListener(event, listener)
<pre>
parameters:
· event – string, name of event
· listener – function to be removed
return values:
· none
events triggered:
· none
</pre>

##	_Error Handling_
When an error in the container occurs, the "error" event is triggered with diagnostic information about the event. Any number of listeners can monitor for errors of different types and respond as needed.

**error** event

This event is triggered whenever a container error occurs. The event contains a description of the error that occurred and, when appropriate, the name of the action that resulted in the error (in the absence of an associated action, the action parameter is null). JavaScript errors remain the full responsibility of the ad designer.

“error” -> function(message, action)
<pre>
parameters:
· message: String, description of the type of error
· action: String, name of action that caused error
triggered by:
· anything that goes wrong
</pre>
Ad designers should note that errors can be handled on either a synchronous or an asynchronous basis by the SDK/container.

While the “message” part of the error event is not defined by the MRAID specification and mainly intended for pre-flight debugging of creative, the “action” part of the error is always the name of the method that the ad tried to use that led to the error. In principle, any MRAID method may trigger an error, so ad designers using an error event listener should listen for the following as potential error actions:
<pre>
	addEventListener					getVersion
	createCalendarEvent 				isViewable
	close 								open
	expand 								playVideo 
	getCurrentPosition 					removeEventListener
	getDefaultPosition 					resize
	getExpandProperties					setExpandProperties
	getMaxSize 							setResizeProperties 
	getPlacementType					storePicture 
	getResizeProperties 				supports 
	getScreenSize 						useCustomClose
	getState
</pre>
While any MRAID method may lead to an error, in practice using resize, adding an image to a device’s photo album or adding an event to a calendar are the most likely MRAID methods to generate errors. Ad designers using those methods should be particularly diligent about adding an error listener to check whether an error occurs on a resize, storePhoto, or createCalendarEvent action, so that the ad creative can potentially take a different action.

##	_Controlling Ad Display_
Besides the initial display, the ad designer may have a number of reasons to control the display.

* An application may load views in the background to help with latency issues so that an ad is requested, but not visible to the user.
* The ad may expand beyond the default size over the application content.
* The ad may return to the default size once user interaction is complete.

**getState** method, **stateChange** event

Each ad container (or Webview) has a state that is one of the following:

<table border="1" cellpadding="3">
	<tr>
		<th>value</th>
		<th>description</th>
	</tr>
	<tr>
		<td>loading</td>
		<td>the container is not yet ready for interactions with the MRAID implementation</td>
	</tr>
	<tr>
		<td>default</td>
		<td>the initial position and size of the ad container as placed by the application and SDK</td>
	</tr>
	<tr>
		<td>expanded</td>
		<td>the ad container has expanded to cover the application content at the top of the view hierarchy</td>
	</tr>
	<tr>
		<td>resized</td>
		<td>the ad container has changed size via MRAID 2.0’s resize() method</td>
	</tr>
	<tr>
		<td>hidden</td>
		<td>the state an interstitial ad transitions to when closed. Where supported, the state a banner ad transitions to when closed</td>
	</tr>
</table>

The getState method returns the current state of the ad container, returning whether the ad container is in its default, fixed position or is in an expanded or resized, larger position, or hidden.

The stateChange event fires when the state is changed programmatically by the ad or by the environment. This event is triggered when the Ad View changes between default, expanded, resized, and hidden states as the result of an expand(), resize(), or a close(). The container or SDK may also close an ad as the result of a user or system action, such as resuming from background.

Any MRAID ad can have only one state at a time. In the case of two-part expandable ads, this requirement means there is only one state for both web views. For as long as the expanded view is onscreen, using getState() will return “expanded.”

The effect on state from calling expand(), resize(), and close() are defined in this table.

How to read: the initial state of the creative is in the left-most column; how the state changes when an MRAID method is used can be found by looking down the column for that method. So for example, if the ad’s state is “expanded” and the close() method is used, the ad’s state changes to “default.”

<table border="1" cellpadding="3">
	<tr>
		<th>Initial state</th>
		<th>expand()</th>
		<th>resize()</th>
		<th>close()</th>
	</tr>
	<tr>
		<td>loading</td>
		<td>no effect</td>
		<td>no effect</td>
		<td>no effect</td>
	</tr>
	<tr>
		<td>default</td>
		<td>For a banner, state changed to “expanded” For an interstitial, no effect</td>
		<td>For a banner, state changed to “resized” For an interstitial, no effect</td>
		<td>For a banner, state changed to “hidden” (if supported by SDK/container) For an interstitial, state changed to “hidden”</td>
	</tr>
	<tr>
		<td>expanded</td>
		<td>no effect (state remains “expanded”)</td>
		<td>triggers an error; state remains “expanded”</td>
		<td>state changed to “default”</td>
	</tr>
	<tr>
		<td>resized</td>
		<td>state changed to “expanded”</td>
		<td>state changed to “resized” (that is, an event listener will hear a new stateChange event, even though the state is still “resized” after the event fires)</td>
		<td>state changed to “default”</td>
	</tr>
	<tr>
		<td>hidden</td>
		<td>no effect</td>
		<td>no effect</td>
		<td>no effect</td>
	</tr>
</table>

In the case of a two-piece expandable, the new, expanded web view starts in the “loading” state briefly until MRAID is available, upon which the “ready” event is fired and the state of the ad then transitions to “expanded.” The banner (the first piece) of the two-piece ad also changes its state, from “default” to “expanded.”

For an interstitial ad, the web view goes from “loading” to “default,” and when the interstitial is closed, the state changes to “hidden.”

getState() -> String
<pre>
parameters:
· none
return values:
· String: "loading", "default", "expanded”, “resized,” or “hidden” 
related events:
· stateChange
</pre>

“stateChange” -> function(state)
<pre>
parameters:
· state - String, either "loading", "default", "expanded", “resized”, or “hidden” 
triggered by:
· expand, resize, close, or the app
</pre>

**getPlacementType()** method

For efficiency, ad designers sometimes flight a single piece of creative in both banner and interstitial placements. So that the creative can be aware of its placement, and therefore potentially behave differently, each ad container has a placement type determining whether the ad is being displayed inline with content (i.e. a banner) or as an interstitial overlaid content (e.g. during a content transition). The container returns the value of the placement to creative so that creative can behave differently as necessary. The container does not determine whether a banner is an expandable (the creative does) and thus does not return a separate type for expandable.

<table border="1" cellpadding="3">
	<tr>
		<th>value</th>
		<th>description</th>
	</tr>
	<tr>
		<td>inline</td>
		<td>the default ad placement is inline with content in the display (i.e. a banner)</td>
	</tr>
	<tr>
		<td>interstitial</td>
		<td>the ad placement is over laid on top of content</td>
	</tr>
</table>

getPlacementType should always return the placement that it initially displayed in.That is, in the case of two-part expandables, the second, expanded part should also see “inline” if it does a getPlacementType.

getPlacementType() -> String
<pre>
parameters:
· none
return values:
· String: "inline", "interstitial" 
related events:
· none
</pre>

**isViewable** method, **viewableChange** event

In addition to the state of the ad container, it is possible that the container is loaded off-screen as part of an application's buffer to help provide a smooth user experience. This is especially prevalent in apps that employ scrolling views or in interstitial ads, for example between levels of a game.

The isViewable method returns whether the ad container is currently on or off the screen. The viewableChange event fires when the ad moves from on-screen to off-screen and vice versa. For a two-piece expandable ad, when the ad state is expanded, isViewable will return an answer based on the viewability of the expanded piece of the ad.

In any situation where an ad may be loaded offscreen, it is a good practice for the ad to check on its viewable state and/or register for viewableChange before taking any action.

Note that MRAID does not define a minimum threshold percentage or number of pixels of the ad that must be onscreen to constitute “viewable.”(The IAB currently has a project underway to establish guidelines for in-app ad measurement, potentially including a viewability threshold, is currently underway within the IAB.)

isViewable() -> boolean
<pre>
parameters:
· none
return values:
· boolean - 
true: container is on-screen and viewable by the user; 
false: container is off-screen and not viewable 
related events:
· viewableChange
</pre>

“viewableChange” -> function(boolean)
<pre>
parameters:
· boolean - 
true: container is on-screen and viewable by the user; 
false: container is off-screen and not viewable 
triggered by:
· a change in the application view controller
</pre>

Below is an example of an ad that takes into account both “Ready” and “isViewable” before taking action.

```
// Wait for the SDK to become ready
if (mraid.getState() === 'loading') {
	mraid.addEventListener('ready', onSdkReady);
} else {
	onSdkReady();
}

function onSdkReady() {
	// Wait for the ad to become viewable for the first time
	if (mraid.isViewable()) {
		showMyAd();
	} else {
		mraid.addEventListener('viewableChange',function(viewable) {
			if (viewable) {
				mraid.removeEventListener('viewableChange', arguments.callee);
				showMyAd();
			}
		}); 
	}
}

function showMyAd() {
... 
}
```

##	_Changing the Size of an Ad_

MRAID v2 includes three distinct ways for an ad to change its size. By far the simplest is to use the open() method, which is intended for click-throughs where an entire web site is loaded in a new browser window (generally this will be a browser running within the app, but it may be the device’s native browser).

The expand() method is intended for ads that expand in a fairly simple, straightforward way to cover the content of the application.

In addition to these, the resize() method is intended for ads that grow or shrink in more subtle ways, in a dialogue with the app in which it is running. This method allows designers complete freedom and control – with the trade-off that additional methods and listeners are required for both the ad creative and the app/container to react appropriately in different placements.

###	Differences between resize(), expand(), and open()

Although these methods are related, they promote an approach of progressive complexity. That is, simple operations should be simple, but sophisticated efforts are still be possible. Understanding the distinction between resize(), expand() and open() helps ad designers choose the best method for their needs.

open()

* lowest common denominator
* used for advertiser landing pages or microsites
* opens a new URL, generally in a browser window within the app, however may open in the device’s native browser
* always full screen
* no additional properties

expand()

* simple interface
* maintains ad experience
* full screen
* few additional properties
* support for one-part or two-part creatives
* MRAID-enforced tap-to-close area in fixed (top right) location
* relative alignment for creatives

resize()

* flexible interface
* continuous, non-modal ad experience
* no default values, can change to larger or smaller sizes
* additional properties and methods required
* one-part creatives only
* MRAID-enforced tap-to-close area, but ad designer can change the close area’s position within the creative area.
* absolute positioning possible
* supports direction of resizing

This table summarizes the differences between these methods.

<table border="1" cellpadding="3">
	<tr>
		<th>property</th>
		<th>open()</th>
		<th>expand()</th>
		<th>resize()</th>
	</tr>
	<tr>
		<td>modal</td>
		<td>Y</td>
		<td>Y</td>
		<td>N</td>
	</tr>
	<tr>
		<td>MRAID-enforced close control</td>
		<td>N</td>
		<td>Y</td>
		<td>Y</td>
	</tr>
	<tr>
		<td>viewer stays within ad experience</td>
		<td>N</td>
		<td>Y</td>
		<td>Y</td>
	</tr>
	<tr>
		<td>two-part creatives</td>
		<td>n/a</td>
		<td>Y</td>
		<td>N</td>
	</tr>
	<tr>
		<td>one-part creatives</td>
		<td>n/a</td>
		<td>Y</td>
		<td>Y</td>
	</tr>
	<tr>
		<td>aligned to screen</td>
		<td>n/a</td>
		<td>Y</td>
		<td>N</td>
	</tr>
	<tr>
		<td>background provided for small creatives</td>
		<td>n/a</td>
		<td>Y</td>
		<td>N</td>
	</tr>
	<tr>
		<td>size up</td>
		<td>n/a</td>
		<td>Y</td>
		<td>Y</td>
	</tr>
	<tr>
		<td>size down</td>
		<td>n/a</td>
		<td>N</td>
		<td>Y</td>
	</tr>
	<tr>
		<td>app-defined max area</td>
		<td>n/a</td>
		<td>N</td>
		<td>Y</td>
	</tr>
	<tr>
		<td>callback required to complete</td>
		<td>n/a</td>
		<td>N</td>
		<td>Y</td>
	</tr>
	<tr>
		<td>supports directionality</td>
		<td>n/a</td>
		<td>N</td>
		<td>Y</td>
	</tr>
	<tr>
		<td>creative can control position of resized ad</td>
		<td>n/a</td>
		<td>N</td>
		<td>Y</td>
	</tr>
	<tr>
		<td>app can return to default state</td>
		<td>n/a</td>
		<td>N</td>
		<td>Y</td>
	</tr>
</table>

In MRAID 2.0 the creation of partial-screen non-modal expansions requires using the resize() method. The deprecation of the expand property “isModal” is reflected in this usage chart. Under MRAID 2.0, calling expand() using expanded creative that is smaller than the size of the full screen requires the container web view blank out, cover, or otherwise obscure the underlying app to make it very clear to the end user that the expanded ad is modal in nature. In that sense, modal, partial screen ads are not allowed in MRAID 2.0.

<table border="1" cellpadding="3">
	<tr>
		<th></th>
		<th>Modal</th>
		<th>Non-modal</th>
	</tr>
	<tr>
		<th>Full Screen</th>
		<td>OK - Use expand()</td>
		<td>Not possible</td>
	</tr>
	<tr>
		<th>Partial Screen</th>
		<td>Not possible</td>
		<td>OK - Use resize()</td>
	</tr>
</table>















