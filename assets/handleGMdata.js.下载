var map;

function initialize() {
	controlDDL();

	var gps_nsb; 
	var myLatlng = new google.maps.LatLng(22.369761,114.142456);
	var myOptions = {
		zoom: 10,
		center: myLatlng,
		mapTypeId: google.maps.MapTypeId.ROADMAP
	}

        map = new google.maps.Map(document.getElementById("map_canvas"), myOptions);
	if (isNowNight)
		gps_nsb = matchNSBwithGPS(nsb15, gps);
	else
		gps_nsb = matchNSBwithGPS(nsb, gps);
		
        createMarkers(gps_nsb);
}

function clone2dArray(myarray){
	var clonedArray = new Array();
	var leng = myarray.length;
		
	for (var x in myarray){
		clonedArray[x] = new Array();
		clonedArray[x] = myarray[x].slice(0);
	}

	return clonedArray;
}

var marker = new Array();
function createMarkers(gps_nsb){

        var numOfMarkers = gps_nsb.length;
        var lat, lng, img, markerTitle;
        var myLatlng;
        var infoContent = "";
        var infoWindow;
        var siteFullName_en, siteFullName_ch, orgLink, viewDetailLink, viewDetail, nsb;

        marker = new Array(numOfMarkers);
        for(var x=0; x<numOfMarkers; x++){
                deviceCode = gps_nsb[x][0];
                lat = gps_nsb[x][1];
                lng = gps_nsb[x][2];
                markerTitle = gps_nsb[x][6] + "";
	
                img = gps_nsb[x][7];

		if (markerTitle==-1)	markerTitle = "Maintenance";
		if (markerTitle==0)	markerTitle = "No data";
		
		//info window content
                siteFullName_en = gps_nsb[x][4];
                siteFullName_ch = gps_nsb[x][3];
		orgLink 	= gps_nsb[x][5];
		viewDetail 	= "查看詳細資料 View details";
		viewDetailLink 	= location.hostname + "/about-nsn.php";

		if (gps_nsb[x][6]==-1 || gps_nsb[x][6]==0)	nsb = "N/A";
               	else 	nsb	= gps_nsb[x][6];

                img = img==""? null: img;

                myLatlng = new google.maps.LatLng(lat, lng);
                marker[x] = new google.maps.Marker({
                        position: myLatlng,
                        map: map,
//                        title: markerTitle,
                        icon: img
                });
		
		infoContent = formInfoContent(siteFullName_en, siteFullName_ch, orgLink, viewDetailLink, viewDetail, nsb, updatedOn, deviceCode);
                handleClickMarker(marker[x], infoContent);
        }
}

function formInfoContent(siteFullName_en, siteFullName_ch, orgLink, viewDetailLink, viewDetail, nsb, updatedOn, deviceCode){
	if(!isNaN(nsb)){
		nsb = (parseInt(Math.round(parseFloat(nsb) * 10 )))/10;
		if(parseInt(nsb,10)===nsb){
			nsb = nsb + '.0';
		}
	}
	var content;
	var linkhead = '';
	var linkend = '';
	if(orgLink != '#'){
		linkhead = '<a target="_blank" href="'+ orgLink +'">';
		linkend = '</a>';
	}
	content = "" +
'<div class="google-map-bubble">' +
        '<div class="bubble-top">' +
				'<div class="bubble-close" onClick="javascript:infoBox.setMap(null);">X</div>'+
                '<div class="bubble-name-ch">' + linkhead + siteFullName_ch + linkend + '</div>' +
                '<div class="bubble-name-en">' + linkhead + siteFullName_en +'</div>' +
                //'<div class="bubble-link"><a target="_blank" href="'+ orgLink +'">'+ orgLink +'</a></div>' +
                '<div class="bubble-more"><a target="_blank" href="http://'+ viewDetailLink +'#'+deviceCode.toLowerCase()+'">'+ viewDetail+'</a></div>' +
                '<div class="bubble-data">' +
                        '<table cellpadding="0" cellspacing="0" border="0">'+
                        '<tr><td class="title">夜空光度:</td><td>'+ nsb +'</td><td>等每平方角秒</td></tr>' +
                        '<tr><td class="title">NSB:</td><td>'+ nsb +'</td><td>mag/arcsec<sup>2</sup></td></tr>' +
                        '</table>' +
                '</div>' +
                '<div class="bubble-update-time"><strong>更新 Updated on: </strong>'+ updatedOn +'</div>' +
        '</div>' +
        '<div class="bubble-bottom"><!-- empty --></div>' +
'</div>';

	return content;
}

function discardDec(myfloat)
{
       if (myfloat != null)
return parseInt(myfloat * 100,10)/100;
       else
return 0;
}

function handleClickMarker(marker, markerContent){

      google.maps.event.addListener(marker, 'click',function () {
          if (typeof infoBox == "object" && infoBox.map) {
                infoBox.setMap(null);
          }
          else {
                showInfoWindow(marker, markerContent);
          }
      });
}

function showInfoWindow(marker, markerContent){
        infoBox = new InfoBox({
                latlng: marker.getPosition(),
                map: map,
                html: markerContent
        });
}
/*
function dump($data) {
if(is_array($data)) { //If the given variable is an array, print using the print_r function.
 print "<pre>-----------------------\n";
 print_r($data);
 print "-----------------------</pre>";
} elseif (is_object($data)) {
 print "<pre>==========================\n";
 var_dump($data);
 print "===========================</pre>";
}
else {
 print "=========&gt; ";
 var_dump($data);
 print " <=========";
}
}
*/
// usage: document.write(var_dump(ANY-JS-VAR,'html'));
function var_dump(data, addwhitespace, safety, level)
{
    var rtrn = '';
    var dt, it, spaces = '';

    if (!level) {
        level = 1;
    }

    for (var i=0; i<level; i++) {
        spaces += '   ';
    } //end for i<level

    if (typeof(data) != 'object') {
        dt = data;

        if (typeof(data) == 'string') {
            if (addwhitespace == 'html') {
                dt = dt.replace(/&/g,'&amp;');
                dt = dt.replace(/>/g,'&gt;');
                dt = dt.replace(/</g,'&lt;');
            } //end if addwhitespace == html
            dt = dt.replace(/\"/g,'\"');
            dt = '"' + dt + '"';
        } //end if typeof == string

        if (typeof(data) == 'function' && addwhitespace) {
            dt = new String(dt).replace(/\n/g,"\n"+spaces);
            if (addwhitespace == 'html') {
                dt = dt.replace(/&/g,'&amp;');
                dt = dt.replace(/>/g,'&gt;');
                dt = dt.replace(/</g,'&lt;');
            } //end if addwhitespace == html
        } //end if typeof == function

        if (typeof(data) == 'undefined') {
            dt = 'undefined';
        } //end if typeof == undefined

        if (addwhitespace == 'html') {
            if (typeof(dt) != 'string') {
                dt = new String(dt);
            } //end typeof != string
            dt = dt.replace(/ /g,"&nbsp;").replace(/\n/g,"<br>");
        } //end if addwhitespace == html
        return dt;
    } //end if typeof != object && != array

    for (var x in data) {
        if (safety && (level > safety)) {
            dt = '*RECURSION*';
        } else {
            try {
                dt = var_dump(data[x],addwhitespace,safety,level+1);
            } catch (e) {
                continue;
            }
        } //end if-else level > safety
        it = var_dump(x,addwhitespace,safety,level+1);
        rtrn += it + ':' + dt + ',';
        if (addwhitespace) {
            rtrn += '\n'+spaces;
        } //end if addwhitespace
    } //end for...in

    if (addwhitespace) {
        rtrn = '{\n' + spaces + rtrn.substr(0,rtrn.length-(2+(level*3))) + '\n' + spaces.substr(0,spaces.length-3) + '}';
    } else {
        rtrn = '{' + rtrn.substr(0,rtrn.length-1) + '}';
    } //end if-else addwhitespace

    if (addwhitespace == 'html') {
        rtrn = rtrn.replace(/ /g,"&nbsp;").replace(/\n/g,"<br>");
    } //end if addwhitespace == html

    return rtrn;
} //end function var_dump


function matchNSBwithGPS(nsb, gpsArray){

	var gps = clone2dArray(gpsArray);
        var markerPath = "assets/images/map_front/";
		
        for(var i in gps){
			var match = false;
           for(var j in nsb)
           {
                if( !isNaN(parseFloat(nsb[j][1])) &&  nsb[j][0] == gps[i][0])
                {
			if (nsb[j][1]==-1){	//maintanence
				var image = markerPath + "maintenance.png";
			} else if (nsb[j][1]==0){	//no data
				var image = markerPath + "no-data.png";
			/*	
			} else if(parseFloat(nsb[j][1]) <= 13)
                        {
                                var image = markerPath + "red-dot.png";
                        }
                        else if(parseFloat(nsb[j][1]) <= 14)
                        {
                                var image = markerPath + "pink-dot.png";
                        }
                        else if(parseFloat(nsb[j][1]) <= 15)
                        {
                                var image = markerPath + "orange-dot.png";
                        }
                        else if(parseFloat(nsb[j][1]) <= 16)
                        {
                                var image = markerPath + "yellow-dot.png";
                        }
                        else if(parseFloat(nsb[j][1]) <= 17)
                        {
                                var image = markerPath + "green-dot.png";
                        }
                        else if(parseFloat(nsb[j][1]) <= 18)
                        {
                                var image = markerPath + "blue-dot.png";
                        }
                        else if(parseFloat(nsb[j][1]) <= 19)
                        {
                                var image = markerPath + "lightblue.png";
                        }
                        else
                        {
                                var image = markerPath + "grey-dot.png";
                        }
				*/
				} else if(parseFloat(nsb[j][1]) >= 19.5)
                        {		var image = markerPath + "grey-dot.png";
                                
                        }
                        else if(parseFloat(nsb[j][1]) >= 18.5)
                        {		var image = markerPath + "lightblue.png";
                                
                        }
                        else if(parseFloat(nsb[j][1]) >= 17.5)
                        {		var image = markerPath + "blue-dot.png";
                                
                        }
                        else if(parseFloat(nsb[j][1]) >= 16.5)
                        {		var image = markerPath + "green-dot.png";
                                
                        }
                        else if(parseFloat(nsb[j][1]) >= 15.5)
                        {		var image = markerPath + "yellow-dot.png";
                                
                        }
                        else if(parseFloat(nsb[j][1]) >= 14.5)
                        {		var image = markerPath + "orange-dot.png";
                                
                        }
                        else if(parseFloat(nsb[j][1]) >= 13.5)
                        {		var image = markerPath + "pink-dot.png";
                                
                        }
                        else
                        {		var image = markerPath + "red-dot.png";
                                
                        }
                        gps[i].push(nsb[j][1]);
						match = true;
                }
           }
		   if(match){
				gps[i].push(image);
		   }else{
				gps[i].push("-1");
				gps[i].push(markerPath + "maintenance.png");
		   }
        }
		//document.getElementById("tracer").innerHTML = var_dump(gps);
        return gps;
}

function clearMarkers(marker){
	for (var x in marker){
		marker[x].setMap(null);
	}
}

function realTimeRadioClicked(){
	controlDDL();	
	clearMarkers(marker);
	gps_nsb = matchNSBwithGPS(nsb15, gps);
	createMarkers(gps_nsb);
}

function lastNightRadioClicked(){
	controlDDL();	
	getDDLvalue();
}
//function to disable / enable dropdown list (above google map)
function controlDDL(){
	var radio = document.getElementById("last-night-map");
	var ddl = document.getElementById("night-time-choose-value-type");

	if (radio.checked){
		ddl.removeAttribute("disabled");
	} else 
		ddl.setAttribute("disabled", "disabled");

} 

function getDDLvalue(){
	var ddlValue = document.getElementById("night-time-choose-value-type").value;
	var gps_nsb;

	clearMarkers(marker);
	
	if (ddlValue=="overall"){	//ddl choose Overall Median Value
		gps_nsb = matchNSBwithGPS(nsb, gps);
		createMarkers(gps_nsb);
	} else {	//others
		$.get('/json_get_session_info/' + ddlValue, {}, function(data, status) {
			if ('success' == status) {
				var nsb = data;	
				gps_nsb = matchNSBwithGPS(nsb, gps);
				createMarkers(gps_nsb);
			}
		}, 'json');
	}

}
