# JSXMLUtil
Javascript XML utility to convert String into XML DOM object
I have just mentioned utility functions which will later be converted into a full scale library and added to repository
Will Start with Core/Main function

convert:function(xmlstr)
		{
			//xmlstr = xmlstr.replace(/&amp;/g,'&');
			//xmlstr = xmlstr.replace(/&/g,'&amp;');
			
			//xmlstr = SEG.utils.ApplicationUtil.htmlUnescape(xmlstr);
			//xmlstr = SEG.utils.ApplicationUtil.htmlEscape(xmlstr);
			
			xmlstr = SEG.utils.ApplicationUtil.unEscapeXMLAttributes(xmlstr);
			xmlstr = SEG.utils.ApplicationUtil.escapeXMLAttributes(xmlstr);
			
			var doc;
			//if (window.ActiveXObject) {         //IE
			if(SEG.utils.ApplicationUtil.isIE()){
			    var doc = new ActiveXObject("Microsoft.XMLDOM");
			    //doc.async = "false";
			    doc.loadXML(xmlstr);
			} else {                             //Mozilla
			    var doc = new DOMParser().parseFromString(xmlstr, "text/xml");
			}
			
			return doc;

		}
		
		
		
		
		
		
		
		
		///Utility functions
		
		//This will look for attribute values with escabale characters and escape
		escapeXMLAttributes:function(xml)
		{
			var strArray = xml.match(/\'([^']*?)\'|"([^"]*?)\"/g);  //Regex to find string between " or ' //this needs modification
			for(var i=0;i<strArray.length;i++)
			{
				var st =  strArray[i].substring(1, strArray[i].length - 1);
				xml = xml.replace(st,SEG.utils.ApplicationUtil.xmlEscape(st));
			}
			return xml;
			
		},
		
		///reverse of escapeXMLAttributes
		unEscapeXMLAttributes:function(xml)
		{
			var strArray = xml.match(/\'([^']*?)\'|"([^"]*?)\"/g);  //this has ISSUE and needs fix
			for(var i=0;i<strArray.length;i++)
			{
				var st =  strArray[i].substring(1, strArray[i].length - 1);
				xml = xml.replace(st,SEG.utils.ApplicationUtil.xmlUnescape(st));
			}
			return xml;
		},
		
		
		xmlEscape:function(str){
			str = SEG.utils.ApplicationUtil.xmlUnescape(str);
		    return String(str)
		            .replace(/&/g, '&amp;')
		            .replace(/"/g, '&quot;')
		            .replace(/'/g, '&#39;')
		            .replace(/</g, '&lt;')
		            .replace(/>/g, '&gt;');
		},

		// I needed the opposite function today, so adding here too:
		xmlUnescape:function(value){
		    return String(value)
		        .replace(/&quot;/g, '"')
		        .replace(/&#39;/g, "'")
		        .replace(/&lt;/g, '<')
		        .replace(/&gt;/g, '>')
		        .replace(/&amp;/g, '&');
		},
