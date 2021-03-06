jQuery DFP - A jQuery implementation for Google DFP
======================================================

This script is a drop in solution for getting Double Click for Publishers (DFP) by Google working on your page. By including this script on your page and then initiating it in the ways described below you should find it very easy to get DFP working.

Do not include any of the generated DFP script tags from the DFP admin on your page, this script replaces them.

This script also works with [Zepto.js](http://zeptojs.com/) and [Tire.js](http://tirejs.com/)

Setup
-----

You can add containers to your page in any location that you would like to display an ad.

By default this script will look for ad units with a class of `adunit` but you can of course use jQuery selectors as well.

You can specify a global ad unit name for all positions on the page using a document meta value:

	<meta name="dfp-adunit" content="10TV_ROS/10TV_Homepage" />

You can optionally specify the adunit name per position in the following way:

    <div class="adunit" data-adunit="Ad_unit_id" data-dimensions="393x176"></div>

This method can be useful for including multiple ad units of the same name which when part of a DFP placement will then pull in as many different creatives as possible.

You can also specify multiple dimensions sets:

    <div class="adunit" data-adunit="Ad_unit_id" data-dimensions="393x176,450x500"></div>

Also you can optionally specify custom targeting on a per ad unit basis in the following way:

    <div class="adunit" data-adunit="Ad_unit_id" data-dimensions="393x176" data-targeting='{"city_id":"1"}'></div>

To create an out of page ad unit set the data-outofpage property on the ad unit, dimension.

    <div class="adunit" data-adunit="Ad_unit_id" data-outofpage="true"></div>

Usage
-----

Calling the script:

    <html>
    <head>
        <title>DFP TEST</title>
        <script src="//ajax.googleapis.com/ajax/libs/jquery/1.9.0/jquery.min.js"></script>
        <script src="jquery.dfp.min.js"></script>
        
        <!-- DoubleClick Targeting Information -->
		<meta name="dfp-id" content="8919" />
		<meta name="dfp-adunit" content="10TV_ROS/10TV_Homepage" />
		<meta name="dfp-targeting" content='{"gender":"male"}' />
        
    </head>
    <body>

        <div class="adunit"
        	data-dimensions="728x90"
        	data-targeting='{"city_id":"1"}'>
        </div>

    </body>
    </html>



Available Options
-----------------

<table>
    <tr>
        <th>Option</th>
        <th>Description</th>
    </tr>
    <tr>
        <td>enableSingleRequest</td>
        <td>This boolean sets whether the page ads are fetched with a single request or not, you will need to set this to false it you want to call $.dfp() more than once, typically you would do this if you are loading ad units into the page after the initial load.</td>
    </tr>
    <tr>
        <td>collapseEmptyDivs</td>
        <td>This can be set to true, false or 'original'. If its set to true the divs will be set to display:none if no line item is found. False means that the ad unit div will stay visible no matter what. Setting this to 'original' (the default option) means that the ad unit div will be hidden if no line items are found UNLESS there is some existing content inside the ad unit div tags. This allows you to have fall back content in the ad unit in the event that no ads are found.</td>
    </tr>
    <tr>
        <td>refreshExisting</td>
        <td>This boolean controls what happens when dfp is called multiple times on ad units. By default it is set to true which means that if an already initialised ad is initialised again it will instead be refreshed.</td>
    </tr>
</table>

Callbacks
---------

This script exposes jQuery events that can bound bound to to handle ad loads:

	<!-- Samples of how to interact with Ads events -->
	<script type="text/javascript">
	
		// Get single ad loaded
		$(".adunit").bind("adLoaded", function() {
			//alert($(this).width() + " x " + $(this).height());
		});
		
		// Get all ads loaded
		$("body").bind("allAdsLoaded", function() {
			//alert("All Ads Loaded");	
		});
		
		// Before Ads Load set some properties
		$("body").bind("beforeAdsLoad", function(event, options) {
			options.enableSingleRequest = true;
            options.collapseEmptyDivs = 'original';
            options.targetPlatform = 'web';
		});
		
	</script>

Please see the example-bootstrap.js file for an example of how to use these.

Default Targeting
-----------------

The following targeting options are built into this script and should be setup in your DFP account ([within Inventory/Custom Targeting](https://support.google.com/dfp_sb/bin/answer.py?hl=en&answer=2983838)) to make full use of them:

<table>
    <tr>
        <th>Key</th>
        <th>Description</th>
    </tr>
    <tr>
        <td>Domain</td>
        <td>This allows you to target different domains, for example you could test your ads on your staging environment before pushing them live by specifying a domain of staging.yourdomain.com within DFP, this script will take care of the rest.</td>
    </tr>
    <tr>
        <td>inURL</td>
        <td>This allows you to target URLs containing the segment you specify in DFP, for example you could set inURL to '/page1' on the targeting options of the DFP line item and it would then allow ads to show on any page that contains /page1 in its URL. e.g. http://www.yourdomain.com/page1 or http://www.yourdomain.com/page1/segment2 or http://www.yourdomain.com/section/page1</td>
    </tr>
    <tr>
        <td>URLIs</td>
        <td>This allows you to target the exact URL of the users browser, for example if you set URLIs to '/page1' on the targeting options of the DFP line item it would match http://www.yourdomain.com/page1 only and not http://www.yourdomain.com/page1/segment2</td>
    </tr>
    <tr>
        <td>Query</td>
        <td>This allows you to target the query parameters of a page. For example if the URL was http://www.yourdomain.com/page1?param1=value1 you could target it with a DFP ad by specifying a Query targeting string of param1:value1</td>
    </tr>
</table>

Contributors
------------

Thanks to:

<ul>
    <li>@crccheck - https://github.com/crccheck</li>
    <li>@MikeSilvis - https://github.com/MikeSilvis</li>
</ul>
