
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

###	IAB Contact Information

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