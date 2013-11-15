
Interactive Advertising Bureau 

Mobile Rich-media Ad Interface Definitions (MRAID) v.2.0 

Released September 27, 2012, 

Revised With Clarifications April 16, 2013

#	Contributors
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

**IAB Contact Information**

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

Using density-independent pixels means that, for example, retina display iPhones and older iPhones will return the same dimensions/measures, despite having different numbers of physical pixels. 1 density-independent pixel corresponds rougly 1/160 of an inch (1 device pixel on a device with roughly 160 DPI).

On iOS, these should map to “points”; on Android, to “density-independent pixels”.

Note: One density-independent pixel will match 1 CSS pixel only if the viewport scale is 1.0. To map between CSS pixels and density-independent pixels, the creative should use the following formula:

`css_pixels * viewport_scale = density_independent_pixels`

**Inline Ad:** An ad that appears onscreen accompanied by other kinds of content, e.g., a banner on a web page or in an app.

**Interstitial Ad:** A full page modal ad that displays on top of content -- a "roadblock" or "overlay." The ad must be dismissed for the user to return to the publisher content. Such ads can appear between levels of a game, or before or after a video clip or other dynamic content. (An ad that is in-between pages and swipes into view like in many magazine apps, is considered an inline ad under MRAID.)

Physical Pixels: The actual pixels on a device screen. For example, a retina-display iPhone measures 960x640 physical pixels. MRAID API length values are always calculated in density-independent pixels (defined above) NOT physical pixels.

SDK: Sofware Development Kit. The reusable piece of code (library) integrated into publisher apps to enable advertisements/Ad Containers. An SDK, by itself, is not a visual component.

Web View: The HTML-based viewer that displays the ad creative. The web view is used to perform rendering of HTML- and Javascript-enabled ads.