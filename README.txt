This Tutorial is for FLEX tracking for Adobe Analytics for Flex 4.X. It has been done with Flash Builder 4.7

1.Documentation for Adobe Analytics FLASH, FLEX, AIR tracking can be found here : http://microsite.omniture.com/t2/help/en_US/sc/appmeasurement/flash/oms_sc_appmeasure_flash.pdf

2.To download the SDK please go to Code Manager in Adobe Analytics Admin

3.ONce SDK downloaded, unzip and reference the library AppMeasurement.swc

4.IN the .mxml file put the following code : 

<fx:Script>
	<![CDATA[
	]]>
</fx:Script>

5.a : add import com.omniture.AppMeasurement;
5.b : create a variable s : public var s:AppMeasurement;
5.c : cretae a funtion that will initialize the s object and send a pageview image request :


private function configAppMeasurement():void {
	s = new AppMeasurement();
	s.debugTracking = true;
	s.trackLocal = true;
	s.account = "REPORT SUITE ID";
	s.pageName = "FLEX test"
	s.pageURL = "http://www.mywebsite.com";
	s.visitorNamespace = "VISITOR NAMESPACE";
	s.autoTrack = false;
	s.charSet = "UTF-8";
	s.trackingServer="TRACKING SERVER";
	//Send the pageview image request
	s.track();				
				
}

P.S: Basically reproduce what you would have in the file s_code.js or AppMeasurement.js if you are doing a normal website implementation

6. In <s:Application add initialize="configAppMeasurement();" to load the function when the page loads.

<s:Application xmlns:fx="http://ns.adobe.com/mxml/2009" 
			   xmlns:s="library://ns.adobe.com/flex/spark" 
			   xmlns:mx="library://ns.adobe.com/flex/mx" minWidth="955" minHeight="600"
			   initialize="configAppMeasurement();"
			   >



7. (Optional) For link/Button/any other action than pageview use same technic as below:
a.Cretae a function that will be called on i.e click :

private function clickButton(evt:MouseEvent):void {
	PopUpManager.addPopUp(ttlWndw, this, true);
	PopUpManager.centerPopUp(ttlWndw);
				
	//BEGINNING OMNITURE LINK TRACKING CODE
	//This code will send data when the button is clicked. It should a custom link tracking pe="lnk_o"
	s.prop4=s.pageName + "LinkFLEXtest";
	s.lightTrackVars = "prop4"
	//send the link tracking image request
	s.trackLink("http://mywebsite.com/linktest.html","o","Test Flex pop up window");
	//END OMNITURE LINK TRACKING CODE
				
}

The important part is 

//BEGINNING OMNITURE LINK TRACKING CODE
//This code will send data when the button is clicked. It should a custom link tracking pe="lnk_o"
s.prop4=s.pageName + "LinkFLEXtest";
s.lightTrackVars = "prop4"
//send the link tracking image request
s.trackLink("http://mywebsite.com/linktest.html","o","Test Flex pop up window");
//END OMNITURE LINK TRACKING CODE

b.Call the function when the button is clicked : 
<s:controlBarContent>
	<s:Button id="btn"
		label="Show TitleWindow"
		click="clickButton(event);" />
</s:controlBarContent>

HOW TO DEBUG :
USE any packet monitor and filter by /b/ss. If no image reuquest is sent then check above. If image request fails check s.account,s.visitorNamespace and s.trackingServer.

P.S : IF s.track and s.trackLink are not called no image request will be sent