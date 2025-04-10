<template>
    <div>


        <head>

            <meta charset="UTF-8">
            <meta name="viewport" content="width=device-width, initial-scale=1">
            <link rel="profile" href="https://gmpg.org/xfn/11">
            <link rel="apple-touch-icon"
                href="https://www.centralcons.vn/wp-content/themes/central-cons/favicon-80x80.png.png">
            <link rel="apple-touch-icon" sizes="152x152"
                href="https://www.centralcons.vn/wp-content/themes/central-cons/favicon-152x152.png">
            <link rel="apple-touch-icon" sizes="180x180"
                href="https://www.centralcons.vn/wp-content/themes/central-cons/favicon-180x180.png">
            <link rel="apple-touch-icon" sizes="167x167"
                href="https://www.centralcons.vn/wp-content/themes/central-cons/favicon-167x167.png">
            <style>
                .pro-wccp:before {
                    content: "\f160";
                    top: 3px;
                }

                .pro-wccp:before {
                    color: #02CA03 !important
                }

                .pro-wccp {
                    transform: rotate(45deg);
                }
            </style>
            <script id="wccp_pro_disable_hot_keys">
/*****************For contenteditable tags***************/
var wccp_pro_iscontenteditable_flag = false;

function wccp_pro_iscontenteditable(e)
{
	var e = e || window.event; // also there is no e.target property in IE. instead IE uses window.event.srcElement
  	
	var target = e.target || e.srcElement;
	
	var iscontenteditable = "false";
		
	if(typeof target.getAttribute!="undefined" )
	{
		iscontenteditable = target.getAttribute("contenteditable"); // Return true or false as string
		
		if(typeof target.hasAttribute!="undefined")
		{
			if(target.hasAttribute("contenteditable"))
				iscontenteditable = true;
		}
	}
	
	console.log("iscontenteditable:" + iscontenteditable);
	
	var iscontenteditable2 = false;
	
	if(typeof target.isContentEditable!="undefined" ) iscontenteditable2 = target.isContentEditable; // Return true or false as boolean

	if(target.parentElement !=null) iscontenteditable2 = target.parentElement.isContentEditable;
	
	if (iscontenteditable == "true" || iscontenteditable == true || iscontenteditable2 == true)
	{
		if(typeof target.style!="undefined" ) target.style.cursor = "text";
		
		//wccp_pro_log_to_console_if_allowed("", iscontenteditable + " " + iscontenteditable2);
		
		wccp_pro_iscontenteditable_flag = true;
		
		wccp_pro_log_to_console_if_allowed("function", "wccp_pro_iscontenteditable: true");
		
		return true;
	}
	wccp_pro_log_to_console_if_allowed("function", "wccp_pro_iscontenteditable: false");
	
	//wccp_pro_iscontenteditable_flag = false;
}
/******************************************************/
function wccp_pro_clear_any_selection()
{
	if(window.wccp_pro_iscontenteditable_flag == true) return;
	
	wccp_pro_log_to_console_if_allowed("function", "wccp_pro_clear_any_selection");
	
	var myName = wccp_pro_clear_any_selection.caller.toString();
	
	myName = myName.substr('function '.length);
	
	myName = myName.substr(0, myName.indexOf('('));

	console.log("called_by: " + myName);
	
	if (window.getSelection)
	{
		if (window.getSelection().empty)
		{  // Chrome
			window.getSelection().empty();
		} else if (window.getSelection().removeAllRanges) 
		{  // Firefox
			window.getSelection().removeAllRanges();
		}
	} else if (document.selection)
	{  // IE?
		document.selection.empty();
	}
	
	//show_wccp_pro_message("You are not allowed to make this operation");
}


/*Is content_editable element*/
function is_content_editable_element(element_name = "")
{
	if (element_name == "TEXT" || element_name == "#TEXT" || element_name == "TEXTAREA" || element_name == "INPUT" || element_name == "PASSWORD" || element_name == "SELECT" || element_name == "OPTION" || element_name == "EMBED" || element_name == "CODE" || element_name == "CODEBLOCK")
	{
		wccp_pro_log_to_console_if_allowed("function", "is_content_editable_element: true >>" + element_name);
		
		return true;
	}
	wccp_pro_log_to_console_if_allowed("function", "is_content_editable_element: false >>" + element_name);
	
	return false;
}
/*Is selection enabled element*/
/*
function is_selection_enabled_element(element_name = "")
{
	if (is_content_editable_element == true)
	{
		wccp_pro_log_to_console_if_allowed("function", "is_selection_enabled_element: true >>" + element_name);
		
		return true;
	}
	wccp_pro_log_to_console_if_allowed("function", "is_selection_enabled_element: false >>" + element_name);
	
	return false;
}
*/
/*Hot keys function  */
function disable_hot_keys(e)
{
	wccp_pro_log_to_console_if_allowed("function", "disable_hot_keys");
	
	e = e || window.event;
	
	//console.log(e);
	
	if (!e) return;
	
	var key;

		if(window.event)
			  key = window.event.keyCode;     /*IE*/
		else if (e.hasOwnProperty("which")) key = e.which;     /*firefox (97)*/

	wccp_pro_log_to_console_if_allowed("Data:", key);
	
			
		if (key == 123 || (e.ctrlKey && e.shiftKey && e.keyCode == 'J'.charCodeAt(0)) )//F12 chrome developer key disable
		{
			show_wccp_pro_message('You are not allowed to do this action on the current page!!');
			
			return false;
		}
		
	var elemtype = e.target.tagName;
	
	elemtype = elemtype.toUpperCase();
	
	var sel = getSelectionTextAndContainerElement();
	
	if(elemtype == "BODY" && sel.text != "") elemtype = sel.containerElement.tagName; /* no need for it when tag name is BODY, so we get the selected text tag name */

	/*elemtype must be merged by elemtype checker on function disable_copy & disable_copy_ie*/
	if (is_content_editable_element(elemtype) == true)
	{
		elemtype = 'TEXT';
	}
	
	if(wccp_pro_iscontenteditable(e) == true) elemtype = 'TEXT';
	
		if (key == 44)/*For any emement type, text elemtype is not excluded here, (prntscr (44)*/
		{
			copyTextToClipboard("");
			show_wccp_pro_message('You are not allowed to do this action on the current page!!');
			return false;
		}	
	if (e.ctrlKey || e.metaKey)
	{
		if (elemtype!= 'TEXT' && (key == 97 || key == 99 || key == 120 || key == 26 || key == 43))
		{
			 show_wccp_pro_message('<b>Alert:</b> You are not allowed to copy content or view source');
			 return false;
		}
		if (elemtype!= 'TEXT')
		{
						
			if (key == 65)
			{
				show_wccp_pro_message('You are not allowed to do this action on the current page!!');
				return false;
			}			
						
			if (key == 67)
			{
				show_wccp_pro_message('You are not allowed to do this action on the current page!!');
				return false;
			}			
						
			if (key == 88)
			{
				show_wccp_pro_message('You are not allowed to do this action on the current page!!');
				return false;
			}			
						
			if (key == 86)
			{
				show_wccp_pro_message('You are not allowed to do this action on the current page!!');
				return false;
			}		}
				
		if (key == 85)
		{
			show_wccp_pro_message('You are not allowed to do this action on the current page!!');
			return false;
		}		
				if (key == 80)
		{
			show_wccp_pro_message('You are not allowed to do this action on the current page!!');
			return false;
		}		
				if (key == 44)
		{
			copyTextToClipboard("no");
			show_wccp_pro_message('You are not allowed to do this action on the current page!!');
			return false;
		}		
		
					if (key == 73)//F12 chrome developer key disable
			{
				show_wccp_pro_message('You are not allowed to do this action on the current page!!');
				return false;
			}
				
				
		if (key == 83)
		{
			show_wccp_pro_message('You are not allowed to do this action on the current page!!');
			return false;
		}    }
return true;
}


window.addEventListener('load', function (){
	if(window.Zepto || !window.jQuery) jQuery =  $;
	jQuery(document).ready(function() {
	  jQuery(document).bind("keyup keydown", disable_hot_keys);
	});
});

</script>
            <style>
                .wccp_pro_copy_code_button {
                    line-height: 6px;
                    width: auto;
                    font-size: 8pt;
                    font-family: tahoma;
                    margin-top: 1px;
                    margin-right: 2px;
                    position: absolute;
                    top: 0;
                    right: 0;
                    border-radius: 4px;
                    opacity: 100%;
                    margin-top: -30px;
                }

                .wccp_pro_copy_code_button:hover {
                    opacity: 100%;
                }

                .wccp_pro_copy_code_button[disabled] {
                    opacity: 40%;
                    border-color: red;
                }

                code,
                pre {
                    overflow: visible;
                    white-space: pre-line;
                }
            </style>
            <script id="wccp_pro_disable_Right_Click">
		function nocontext(e) {

			wccp_pro_log_to_console_if_allowed("function", "nocontext");
			
			e = e || window.event; // also there is no e.target property in IE. instead IE uses window.event.srcElement
			
			if (apply_class_exclusion(e) == 'Yes') return true;
			
	    	var exception_tags = 'NOTAG,';
			
	        var clickedTag = (e==null) ? event.srcElement.tagName : e.target.tagName;
			
			console.log("clickedTag: " + clickedTag);
			
			var target = e.target || e.srcElement;
			
			var parent_tag = ""; var parent_of_parent_tag = "";
			
			if(target.parentElement != null)
			{
				parent_tag = target.parentElement.tagName;
				
				if(target.parentElement.parentElement != null) parent_of_parent_tag = target.parentElement.parentElement.tagName;
			}
			
	        var checker = 'checked';
	        if ((clickedTag == "IMG" || clickedTag == "FIGURE" || clickedTag == "SVG" || clickedTag == "PROTECTEDIMGDIV") && checker == 'checked') {
	            if (alertMsg_IMG != "")show_wccp_pro_message(alertMsg_IMG);
	            return false;
	        }else {exception_tags = exception_tags + 'IMG,';}
			
			checker = '';
			if ((clickedTag == "VIDEO" || clickedTag == "PROTECTEDWCCPVIDEO" || clickedTag == "EMBED") && checker == 'checked') {
	            if (alertMsg_VIDEO != "")show_wccp_pro_message(alertMsg_VIDEO);
	            return false;
	        }else {exception_tags = exception_tags + 'VIDEO,PROTECTEDWCCPVIDEO,EMBED,';}
	        
	        checker = 'checked';
	        if ((clickedTag == "A" || clickedTag == "TIME" || parent_tag == "A" || parent_of_parent_tag == "A") && checker == 'checked') {
	            if (alertMsg_A != "")show_wccp_pro_message(alertMsg_A);
	            return false;
	        }else {exception_tags = exception_tags + 'A,';if(parent_tag == "A" || parent_of_parent_tag == "A") clickedTag = "A";}

	        checker = 'checked';
	        if ((clickedTag == "P" || clickedTag == "B" || clickedTag == "FONT" ||  clickedTag == "LI" || clickedTag == "UL" || clickedTag == "STRONG" || clickedTag == "OL" || clickedTag == "BLOCKQUOTE" || clickedTag == "TH" || clickedTag == "TR" || clickedTag == "TD" || clickedTag == "SPAN" || clickedTag == "EM" || clickedTag == "SMALL" || clickedTag == "I" || clickedTag == "BUTTON") && checker == 'checked') {
	            if (alertMsg_PB != "")show_wccp_pro_message(alertMsg_PB);
	            return false;
	        }else {exception_tags = exception_tags + 'P,B,FONT,LI,UL,STRONG,OL,BLOCKQUOTE,TD,SPAN,EM,SMALL,I,BUTTON,';}
	        
	        checker = 'checked';
	        if ((clickedTag == "INPUT" || clickedTag == "PASSWORD") && checker == 'checked') {
	            if (alertMsg_INPUT != "")show_wccp_pro_message(alertMsg_INPUT);
	            return false;
	        }else {exception_tags = exception_tags + 'INPUT,PASSWORD,';}
	        
	        checker = 'checked';
	        if ((clickedTag == "H1" || clickedTag == "H2" || clickedTag == "H3" || clickedTag == "H4" || clickedTag == "H5" || clickedTag == "H6" || clickedTag == "ASIDE" || clickedTag == "NAV") && checker == 'checked') {
	            if (alertMsg_H != "")show_wccp_pro_message(alertMsg_H);
	            return false;
	        }else {exception_tags = exception_tags + 'H1,H2,H3,H4,H5,H6,';}
	        
	        checker = 'checked';
	        if (clickedTag == "TEXTAREA" && checker == 'checked') {
	            if (alertMsg_TEXTAREA != "")show_wccp_pro_message(alertMsg_TEXTAREA);
	            return false;
	        }else {exception_tags = exception_tags + 'TEXTAREA,';}
	        
	        checker = 'checked';
	        if ((clickedTag == "DIV" || clickedTag == "BODY" || clickedTag == "HTML" || clickedTag == "ARTICLE" || clickedTag == "SECTION" || clickedTag == "NAV" || clickedTag == "HEADER" || clickedTag == "FOOTER") && checker == 'checked') {
	            if (alertMsg_EmptySpaces != "")show_wccp_pro_message(alertMsg_EmptySpaces);
	            return false;
	        }
	        else
	        {
	        	if (exception_tags.indexOf(clickedTag)!=-1)
	        	{
		        	return true;
		        }
	        	else
	        	return false;
	        }
	    }
		
		function disable_drag_images(e)
		{
			wccp_pro_log_to_console_if_allowed("function", "disable_drag_images");
			
			var e = e || window.event; // also there is no e.target property in IE. instead IE uses window.event.srcElement
			
			var target = e.target || e.srcElement;
			
			//For contenteditable tags
			if (apply_class_exclusion(e) == "Yes") return true;

			var elemtype = e.target.nodeName;
			
			if (elemtype != "IMG") {return;}
			
			elemtype = elemtype.toUpperCase();
			
			var disable_drag_drop_images = 'checked';
			
			if (disable_drag_drop_images != "checked")  return true;
			
			if (window.location.href.indexOf("/user/") > -1) {
			  return true; //To allow users to drag & drop images when editing thier profiles
			}
			
			show_wccp_pro_message(alertMsg_IMG);
			
			return false;
		}
		
	    var alertMsg_IMG = " <b>Alert:</b> Image is protected";
	    var alertMsg_A = "<b>Alert:</b> This link is protected";
	    var alertMsg_PB = "<b>Alert:</b> Right click on text is disabled";
	    var alertMsg_INPUT = "<b>Alert:</b> Right click is disabled";
	    var alertMsg_H = "<b>Alert:</b> Right click on headlines is disabled";
	    var alertMsg_TEXTAREA = "<b>Alert:</b> Right click is disabled";
	    var alertMsg_EmptySpaces = "<b>Alert:</b> Right click on empty spaces is disabled";
		var alertMsg_VIDEO = "<b>Alert:</b> Right click on videos is disabled";
	    document.oncontextmenu=null;
		document.oncontextmenu = nocontext;
		document.addEventListener("contextmenu",nocontext);
		window.addEventListener("contextmenu",nocontext);
</script>

            <script id="wccp_pro_disable_drag_images">
	document.ondragstart = disable_drag_images;
		window.addEventListener('load', function (){
			if(window.Zepto || !window.jQuery) jQuery =  $;
			jQuery(document).ready(function(){
				jQuery('img').each(function() {
					jQuery(this).attr('draggable', false);
				});
			});
		});
	</script>
            <style id="wccp_pro_style1">
                img {
                    -moz-user-select: none;
                    -webkit-user-select: none;
                    -ms-user-select: none;
                    -khtml-user-select: none;
                    user-select: none;
                    -webkit-user-drag: none;
                }
            </style>
            <style>
                /* Start your code after this line */

                /* End your code before this line */
            </style>
            <script id="wccp_pro_css_disable_selection">
	function wccp_pro_msieversion() 
		{
			var ua = window.navigator.userAgent;
			var msie = ua.indexOf("MSIE");
			var msie2 = ua.indexOf("Edge");
			var msie3 = ua.indexOf("Trident");

		if (msie > -1 || msie2 > -1 || msie3 > -1) // If Internet Explorer, return version number
		{
			return "IE";
		}
		else  // If another browser, return 0
		{
			return "otherbrowser";
		}
	}
    
	var e = document.getElementsByTagName('H1')[0];
	if(e && wccp_pro_msieversion() == "IE")
	{
		e.setAttribute('unselectable',"on");
	}
	</script>
            <script id="wccp_pro_class_exclusion">
function copyToClipboard(elem) {
	  // create hidden text element, if it doesn't already exist
    var targetId = "_wccp_pro_hiddenCopyText_";
    {
        // must use a temporary form element for the selection and copy
        target = document.getElementById(targetId);
        if (!target) {
            var target = document.createElement("textarea");
            target.style.position = "absolute";
            target.style.left = "-9999px";
            target.style.top = "0";
            target.id = targetId;
            document.body.appendChild(target);
        }
        target.textContent = elem.textContent;
    }
    // select the content
    var currentFocus = document.activeElement;
    target.focus();
    target.setSelectionRange(0, target.value.length);
    
    // copy the selection
    var succeed;
    try {
    	  succeed = document.execCommand("copy");
    } catch(e) {
        succeed = false;
    }

    // restore original focus
    if (currentFocus && typeof currentFocus.focus === "function") {
        currentFocus.focus();
    }
    
    
	// clear temporary content
	target.textContent = "";
	document.getElementsByTagName('span')[0].innerHTML = " ";
    return succeed;
}
/**************************************************/
function wccp_pro_log_to_console_if_allowed(title = "title", data = "")
{
	var myName = "";
	
	if(wccp_pro_log_to_console_if_allowed.caller != null) myName = wccp_pro_log_to_console_if_allowed.caller.toString();
	
	myName = myName.substr('function '.length);
	
	myName = myName.substr(0, myName.indexOf('('));

	//console.log("function_name: " + myName);
	
	}
/**************************************************/
function fallbackCopyTextToClipboard(text) {
  var textArea = document.createElement("textarea");
  textArea.value = text;
  document.body.appendChild(textArea);
  textArea.focus();
  textArea.select();

  try {
    var successful = document.execCommand("copy");
    var msg = successful ? "successful" : "unsuccessful";
    console.log("Fallback: Copying text command was " + msg);
  } catch (err) {
    console.error("Fallback: Oops, unable to copy", err);
  }

  document.body.removeChild(textArea);
}
/*****************************************/
function copyTextToClipboard(text) {
  if (!navigator.clipboard) {
    fallbackCopyTextToClipboard(text);
    return;
  }
  navigator.clipboard.writeText(text).then(
    function() {
      console.log("Async: Copying to clipboard was successful!");
    },
    function(err) {
      console.error("Async: Could not copy text: ", err);
    }
  );
}
/*****************************************/
/*getSelectionTextAndContainerElement*/
function getSelectionTextAndContainerElement()
{
    var text = "", containerElement = null;
    if (typeof window.getSelection != "undefined") {
        var sel = window.getSelection();
        if (sel.rangeCount) {
            var node = sel.getRangeAt(0).commonAncestorContainer;
            containerElement = node.nodeType == 1 ? node : node.parentNode;
			if (typeof(containerElement.parentElement) != 'undefined') current_clicked_object = containerElement.parentElement;
            text = sel.toString();
        }
    } else if (typeof document.selection != "undefined" && document.selection.type != "Control")
	{
        var textRange = document.selection.createRange();
        containerElement = textRange.parentElement();
        text = textRange.text;
    }
    
	return {
        text: text,
        containerElement: containerElement
    };
}

function getSelectionParentElement() {
    var parentEl = null, sel;
	
    if (window.getSelection) {
        sel = window.getSelection();
        if (sel.rangeCount) {
            parentEl = sel.getRangeAt(0).commonAncestorContainer;
			//sel.getRangeAt(0).startContainer.parentNode;
            if (parentEl.nodeType != 1) {
                parentEl = parentEl.parentNode;
            }
        }
    } else if ( (sel = document.selection) && sel.type != "Control") {
        parentEl = sel.createRange().parentElement();
    }
	
	let arr = new Array();
	
	arr["nodeName"] = "cant_find_parent_element";
	
	if(parentEl != null)
		return parentEl;
	else
		return arr;
}
/*****************************************/
function sleep(ms) {
    return new Promise(resolve => setTimeout(resolve, ms));
}
/*****************************************/
</script>

            <script id="apply_class_exclusion">
function apply_class_exclusion(e)
{
	wccp_pro_log_to_console_if_allowed("function", "apply_class_exclusion" + e);
	
	var my_return = 'No';
	
	var e = e || window.event; // also there is no e.target property in IE. instead IE uses window.event.srcElement
  	
	var target = e.target || e.srcElement || 'nothing';
	
	//if(target.parentElement != null) console.log (target.parentElement.className);
	
	var excluded_classes = '' + '';
	
	var class_to_exclude = "";
	
	if(target.parentElement != null)
	{
		class_to_exclude = target.className + ' ' + target.parentElement.className || '';
	}else{
		class_to_exclude = target.className;
	}
	
	var class_to_exclude_array = Array();
	
	//console.log(class_to_exclude);
	
	if (typeof(class_to_exclude) != 'undefined') class_to_exclude_array = class_to_exclude.split(" ");
	
	//console.log (class_to_exclude_array);
	
	class_to_exclude_array.forEach(function(item)
	{
		if(item != '' && excluded_classes.indexOf(item)>=0)
		{
			//target.style.cursor = "text";
			
			//console.log ('Yes');
			
			my_return = 'Yes';
		}
	});

	try {
		class_to_exclude = target.parentElement.getAttribute('class') || target.parentElement.className || '';
		}
	catch(err) 
		{
		class_to_exclude = '';
		}
	
	if(class_to_exclude != '' && excluded_classes.indexOf(class_to_exclude)>=0)
	{
		//target.style.cursor = "text";
		my_return = 'Yes';
	}

	return my_return;
}
</script>
            <style id="wccp_pro_style2" data-asas-style="">
                *[contenteditable],
                [contenteditable] *,
                *[contenteditable="true"],
                [contenteditable="true"] * {
                    /* for contenteditable tags*/
                    ,
                    /* for tags inside contenteditable tags*/
                    -webkit-user-select: auto !important;
                    cursor: text !important;
                    user-select: text !important;
                    pointer-events: auto !important;
                }

                /*
	*[contenteditable]::selection, [contenteditable] *::selection, [contenteditable="true"]::selection, [contenteditable="true"] *::selection { background: Highlight !important; color: HighlightText !important;}
	*[contenteditable]::-moz-selection, [contenteditable="true"] *::-moz-selection { background: Highlight !important; color: HighlightText !important;}
	input::selection,textarea::selection, code::selection, code > *::selection { background: Highlight !important; color: HighlightText !important;}
	input::-moz-selection,textarea::-moz-selection, code::-moz-selection, code > *::-moz-selection { background: Highlight !important; color: HighlightText !important;}
	*/
                a {
                    cursor: pointer;
                    pointer-events: auto !important;
                }
            </style>
            <style>
                TEXT,
                TEXTAREA,
                input[type="text"] {
                    cursor: text !important;
                    user-select: text !important;
                }
            </style>
            <meta name='robots'
                content='index, follow, max-image-preview:large, max-snippet:-1, max-video-preview:-1' />
            <style>
                img:is([sizes="auto" i], [sizes^="auto," i]) {
                    contain-intrinsic-size: 3000px 1500px
                }
            </style>
            <link rel="alternate" href="https://www.centralcons.vn/du-an/" hreflang="vi" />
            <link rel="alternate" href="https://www.centralcons.vn/en/central-project/" hreflang="en" />
            <link rel="alternate" href="https://www.centralcons.vn/ch/%e9%a1%b9%e7%9b%ae/" hreflang="zh" />

            <!-- This site is optimized with the Yoast SEO plugin v22.1 - https://yoast.com/wordpress/plugins/seo/ -->
            <title>Dự án - CENTRAL</title>
            <link rel="canonical" href="https://www.centralcons.vn/du-an/" />
            <meta property="og:locale" content="vi_VN" />
            <meta property="og:locale:alternate" content="en_US" />
            <meta property="og:locale:alternate" content="zh_CN" />
            <meta property="og:type" content="article" />
            <meta property="og:title" content="Dự án - CENTRAL" />
            <meta property="og:url" content="https://www.centralcons.vn/du-an/" />
            <meta property="og:site_name" content="CENTRAL" />
            <meta property="article:modified_time" content="2022-05-12T07:00:42+00:00" />
            <meta property="og:image"
                content="https://www.centralcons.vn/wp-content/uploads/2022/04/3456DB59-8729-4F39-A00A-7AAD586A8085.png" />
            <meta property="og:image:width" content="1609" />
            <meta property="og:image:height" content="1998" />
            <meta property="og:image:type" content="image/png" />
            <meta name="twitter:card" content="summary_large_image" />
            <script type="application/ld+json"
                class="yoast-schema-graph">{"@context":"https://schema.org","@graph":[{"@type":"WebPage","@id":"https://www.centralcons.vn/du-an/","url":"https://www.centralcons.vn/du-an/","name":"Dự án - CENTRAL","isPartOf":{"@id":"https://www.centralcons.vn/#website"},"datePublished":"2021-11-29T08:11:28+00:00","dateModified":"2022-05-12T07:00:42+00:00","breadcrumb":{"@id":"https://www.centralcons.vn/du-an/#breadcrumb"},"inLanguage":"vi","potentialAction":[{"@type":"ReadAction","target":["https://www.centralcons.vn/du-an/"]}]},{"@type":"BreadcrumbList","@id":"https://www.centralcons.vn/du-an/#breadcrumb","itemListElement":[{"@type":"ListItem","position":1,"name":"Home","item":"https://www.centralcons.vn/"},{"@type":"ListItem","position":2,"name":"Dự án"}]},{"@type":"WebSite","@id":"https://www.centralcons.vn/#website","url":"https://www.centralcons.vn/","name":"CENTRAL","description":"","potentialAction":[{"@type":"SearchAction","target":{"@type":"EntryPoint","urlTemplate":"https://www.centralcons.vn/?s={search_term_string}"},"query-input":"required name=search_term_string"}],"inLanguage":"vi"}]}</script>
            <!-- / Yoast SEO plugin. -->


            <link rel="alternate" type="application/rss+xml" title="Dòng thông tin CENTRAL &raquo;"
                href="https://www.centralcons.vn/feed/" />
            <link rel="alternate" type="application/rss+xml" title="CENTRAL &raquo; Dòng bình luận"
                href="https://www.centralcons.vn/comments/feed/" />
            <link rel='stylesheet' id='wp-block-library-css'
                href='https://www.centralcons.vn/wp-includes/css/dist/block-library/style.min.css?ver=6.7.2'
                media='all' />
            <style id='classic-theme-styles-inline-css'>
                /*! This file is auto-generated */
                .wp-block-button__link {
                    color: #fff;
                    background-color: #32373c;
                    border-radius: 9999px;
                    box-shadow: none;
                    text-decoration: none;
                    padding: calc(.667em + 2px) calc(1.333em + 2px);
                    font-size: 1.125em
                }

                .wp-block-file__button {
                    background: #32373c;
                    color: #fff;
                    text-decoration: none
                }
            </style>
            <style id='global-styles-inline-css'>
                :root {
                    --wp--preset--aspect-ratio--square: 1;
                    --wp--preset--aspect-ratio--4-3: 4/3;
                    --wp--preset--aspect-ratio--3-4: 3/4;
                    --wp--preset--aspect-ratio--3-2: 3/2;
                    --wp--preset--aspect-ratio--2-3: 2/3;
                    --wp--preset--aspect-ratio--16-9: 16/9;
                    --wp--preset--aspect-ratio--9-16: 9/16;
                    --wp--preset--color--black: #000000;
                    --wp--preset--color--cyan-bluish-gray: #abb8c3;
                    --wp--preset--color--white: #ffffff;
                    --wp--preset--color--pale-pink: #f78da7;
                    --wp--preset--color--vivid-red: #cf2e2e;
                    --wp--preset--color--luminous-vivid-orange: #ff6900;
                    --wp--preset--color--luminous-vivid-amber: #fcb900;
                    --wp--preset--color--light-green-cyan: #7bdcb5;
                    --wp--preset--color--vivid-green-cyan: #00d084;
                    --wp--preset--color--pale-cyan-blue: #8ed1fc;
                    --wp--preset--color--vivid-cyan-blue: #0693e3;
                    --wp--preset--color--vivid-purple: #9b51e0;
                    --wp--preset--gradient--vivid-cyan-blue-to-vivid-purple: linear-gradient(135deg, rgba(6, 147, 227, 1) 0%, rgb(155, 81, 224) 100%);
                    --wp--preset--gradient--light-green-cyan-to-vivid-green-cyan: linear-gradient(135deg, rgb(122, 220, 180) 0%, rgb(0, 208, 130) 100%);
                    --wp--preset--gradient--luminous-vivid-amber-to-luminous-vivid-orange: linear-gradient(135deg, rgba(252, 185, 0, 1) 0%, rgba(255, 105, 0, 1) 100%);
                    --wp--preset--gradient--luminous-vivid-orange-to-vivid-red: linear-gradient(135deg, rgba(255, 105, 0, 1) 0%, rgb(207, 46, 46) 100%);
                    --wp--preset--gradient--very-light-gray-to-cyan-bluish-gray: linear-gradient(135deg, rgb(238, 238, 238) 0%, rgb(169, 184, 195) 100%);
                    --wp--preset--gradient--cool-to-warm-spectrum: linear-gradient(135deg, rgb(74, 234, 220) 0%, rgb(151, 120, 209) 20%, rgb(207, 42, 186) 40%, rgb(238, 44, 130) 60%, rgb(251, 105, 98) 80%, rgb(254, 248, 76) 100%);
                    --wp--preset--gradient--blush-light-purple: linear-gradient(135deg, rgb(255, 206, 236) 0%, rgb(152, 150, 240) 100%);
                    --wp--preset--gradient--blush-bordeaux: linear-gradient(135deg, rgb(254, 205, 165) 0%, rgb(254, 45, 45) 50%, rgb(107, 0, 62) 100%);
                    --wp--preset--gradient--luminous-dusk: linear-gradient(135deg, rgb(255, 203, 112) 0%, rgb(199, 81, 192) 50%, rgb(65, 88, 208) 100%);
                    --wp--preset--gradient--pale-ocean: linear-gradient(135deg, rgb(255, 245, 203) 0%, rgb(182, 227, 212) 50%, rgb(51, 167, 181) 100%);
                    --wp--preset--gradient--electric-grass: linear-gradient(135deg, rgb(202, 248, 128) 0%, rgb(113, 206, 126) 100%);
                    --wp--preset--gradient--midnight: linear-gradient(135deg, rgb(2, 3, 129) 0%, rgb(40, 116, 252) 100%);
                    --wp--preset--font-size--small: 13px;
                    --wp--preset--font-size--medium: 20px;
                    --wp--preset--font-size--large: 36px;
                    --wp--preset--font-size--x-large: 42px;
                    --wp--preset--spacing--20: 0.44rem;
                    --wp--preset--spacing--30: 0.67rem;
                    --wp--preset--spacing--40: 1rem;
                    --wp--preset--spacing--50: 1.5rem;
                    --wp--preset--spacing--60: 2.25rem;
                    --wp--preset--spacing--70: 3.38rem;
                    --wp--preset--spacing--80: 5.06rem;
                    --wp--preset--shadow--natural: 6px 6px 9px rgba(0, 0, 0, 0.2);
                    --wp--preset--shadow--deep: 12px 12px 50px rgba(0, 0, 0, 0.4);
                    --wp--preset--shadow--sharp: 6px 6px 0px rgba(0, 0, 0, 0.2);
                    --wp--preset--shadow--outlined: 6px 6px 0px -3px rgba(255, 255, 255, 1), 6px 6px rgba(0, 0, 0, 1);
                    --wp--preset--shadow--crisp: 6px 6px 0px rgba(0, 0, 0, 1);
                }

                :where(.is-layout-flex) {
                    gap: 0.5em;
                }

                :where(.is-layout-grid) {
                    gap: 0.5em;
                }

                body .is-layout-flex {
                    display: flex;
                }

                .is-layout-flex {
                    flex-wrap: wrap;
                    align-items: center;
                }

                .is-layout-flex> :is(*, div) {
                    margin: 0;
                }

                body .is-layout-grid {
                    display: grid;
                }

                .is-layout-grid> :is(*, div) {
                    margin: 0;
                }

                :where(.wp-block-columns.is-layout-flex) {
                    gap: 2em;
                }

                :where(.wp-block-columns.is-layout-grid) {
                    gap: 2em;
                }

                :where(.wp-block-post-template.is-layout-flex) {
                    gap: 1.25em;
                }

                :where(.wp-block-post-template.is-layout-grid) {
                    gap: 1.25em;
                }

                .has-black-color {
                    color: var(--wp--preset--color--black) !important;
                }

                .has-cyan-bluish-gray-color {
                    color: var(--wp--preset--color--cyan-bluish-gray) !important;
                }

                .has-white-color {
                    color: var(--wp--preset--color--white) !important;
                }

                .has-pale-pink-color {
                    color: var(--wp--preset--color--pale-pink) !important;
                }

                .has-vivid-red-color {
                    color: var(--wp--preset--color--vivid-red) !important;
                }

                .has-luminous-vivid-orange-color {
                    color: var(--wp--preset--color--luminous-vivid-orange) !important;
                }

                .has-luminous-vivid-amber-color {
                    color: var(--wp--preset--color--luminous-vivid-amber) !important;
                }

                .has-light-green-cyan-color {
                    color: var(--wp--preset--color--light-green-cyan) !important;
                }

                .has-vivid-green-cyan-color {
                    color: var(--wp--preset--color--vivid-green-cyan) !important;
                }

                .has-pale-cyan-blue-color {
                    color: var(--wp--preset--color--pale-cyan-blue) !important;
                }

                .has-vivid-cyan-blue-color {
                    color: var(--wp--preset--color--vivid-cyan-blue) !important;
                }

                .has-vivid-purple-color {
                    color: var(--wp--preset--color--vivid-purple) !important;
                }

                .has-black-background-color {
                    background-color: var(--wp--preset--color--black) !important;
                }

                .has-cyan-bluish-gray-background-color {
                    background-color: var(--wp--preset--color--cyan-bluish-gray) !important;
                }

                .has-white-background-color {
                    background-color: var(--wp--preset--color--white) !important;
                }

                .has-pale-pink-background-color {
                    background-color: var(--wp--preset--color--pale-pink) !important;
                }

                .has-vivid-red-background-color {
                    background-color: var(--wp--preset--color--vivid-red) !important;
                }

                .has-luminous-vivid-orange-background-color {
                    background-color: var(--wp--preset--color--luminous-vivid-orange) !important;
                }

                .has-luminous-vivid-amber-background-color {
                    background-color: var(--wp--preset--color--luminous-vivid-amber) !important;
                }

                .has-light-green-cyan-background-color {
                    background-color: var(--wp--preset--color--light-green-cyan) !important;
                }

                .has-vivid-green-cyan-background-color {
                    background-color: var(--wp--preset--color--vivid-green-cyan) !important;
                }

                .has-pale-cyan-blue-background-color {
                    background-color: var(--wp--preset--color--pale-cyan-blue) !important;
                }

                .has-vivid-cyan-blue-background-color {
                    background-color: var(--wp--preset--color--vivid-cyan-blue) !important;
                }

                .has-vivid-purple-background-color {
                    background-color: var(--wp--preset--color--vivid-purple) !important;
                }

                .has-black-border-color {
                    border-color: var(--wp--preset--color--black) !important;
                }

                .has-cyan-bluish-gray-border-color {
                    border-color: var(--wp--preset--color--cyan-bluish-gray) !important;
                }

                .has-white-border-color {
                    border-color: var(--wp--preset--color--white) !important;
                }

                .has-pale-pink-border-color {
                    border-color: var(--wp--preset--color--pale-pink) !important;
                }

                .has-vivid-red-border-color {
                    border-color: var(--wp--preset--color--vivid-red) !important;
                }

                .has-luminous-vivid-orange-border-color {
                    border-color: var(--wp--preset--color--luminous-vivid-orange) !important;
                }

                .has-luminous-vivid-amber-border-color {
                    border-color: var(--wp--preset--color--luminous-vivid-amber) !important;
                }

                .has-light-green-cyan-border-color {
                    border-color: var(--wp--preset--color--light-green-cyan) !important;
                }

                .has-vivid-green-cyan-border-color {
                    border-color: var(--wp--preset--color--vivid-green-cyan) !important;
                }

                .has-pale-cyan-blue-border-color {
                    border-color: var(--wp--preset--color--pale-cyan-blue) !important;
                }

                .has-vivid-cyan-blue-border-color {
                    border-color: var(--wp--preset--color--vivid-cyan-blue) !important;
                }

                .has-vivid-purple-border-color {
                    border-color: var(--wp--preset--color--vivid-purple) !important;
                }

                .has-vivid-cyan-blue-to-vivid-purple-gradient-background {
                    background: var(--wp--preset--gradient--vivid-cyan-blue-to-vivid-purple) !important;
                }

                .has-light-green-cyan-to-vivid-green-cyan-gradient-background {
                    background: var(--wp--preset--gradient--light-green-cyan-to-vivid-green-cyan) !important;
                }

                .has-luminous-vivid-amber-to-luminous-vivid-orange-gradient-background {
                    background: var(--wp--preset--gradient--luminous-vivid-amber-to-luminous-vivid-orange) !important;
                }

                .has-luminous-vivid-orange-to-vivid-red-gradient-background {
                    background: var(--wp--preset--gradient--luminous-vivid-orange-to-vivid-red) !important;
                }

                .has-very-light-gray-to-cyan-bluish-gray-gradient-background {
                    background: var(--wp--preset--gradient--very-light-gray-to-cyan-bluish-gray) !important;
                }

                .has-cool-to-warm-spectrum-gradient-background {
                    background: var(--wp--preset--gradient--cool-to-warm-spectrum) !important;
                }

                .has-blush-light-purple-gradient-background {
                    background: var(--wp--preset--gradient--blush-light-purple) !important;
                }

                .has-blush-bordeaux-gradient-background {
                    background: var(--wp--preset--gradient--blush-bordeaux) !important;
                }

                .has-luminous-dusk-gradient-background {
                    background: var(--wp--preset--gradient--luminous-dusk) !important;
                }

                .has-pale-ocean-gradient-background {
                    background: var(--wp--preset--gradient--pale-ocean) !important;
                }

                .has-electric-grass-gradient-background {
                    background: var(--wp--preset--gradient--electric-grass) !important;
                }

                .has-midnight-gradient-background {
                    background: var(--wp--preset--gradient--midnight) !important;
                }

                .has-small-font-size {
                    font-size: var(--wp--preset--font-size--small) !important;
                }

                .has-medium-font-size {
                    font-size: var(--wp--preset--font-size--medium) !important;
                }

                .has-large-font-size {
                    font-size: var(--wp--preset--font-size--large) !important;
                }

                .has-x-large-font-size {
                    font-size: var(--wp--preset--font-size--x-large) !important;
                }

                :where(.wp-block-post-template.is-layout-flex) {
                    gap: 1.25em;
                }

                :where(.wp-block-post-template.is-layout-grid) {
                    gap: 1.25em;
                }

                :where(.wp-block-columns.is-layout-flex) {
                    gap: 2em;
                }

                :where(.wp-block-columns.is-layout-grid) {
                    gap: 2em;
                }

                :root :where(.wp-block-pullquote) {
                    font-size: 1.5em;
                    line-height: 1.6;
                }
            </style>
            <link rel='stylesheet' id='contact-form-7-css'
                href='https://www.centralcons.vn/wp-content/plugins/contact-form-7/includes/css/styles.css?ver=5.9.8'
                media='all' />
            <link rel='stylesheet' id='megamenu-css'
                href='https://www.centralcons.vn/wp-content/uploads/maxmegamenu/style_vi.css?ver=27b881' media='all' />
            <link rel='stylesheet' id='dashicons-css'
                href='https://www.centralcons.vn/wp-includes/css/dashicons.min.css?ver=6.7.2' media='all' />
            <link rel='stylesheet' id='central-cons-style-css'
                href='https://www.centralcons.vn/wp-content/themes/central-cons/style.css?ver=1.0.3' media='all' />
            <link rel='stylesheet' id='bootstrap-css'
                href='https://www.centralcons.vn/wp-content/themes/central-cons/assets/css/bootstrap.min.css?ver=1.1'
                media='all' />
            <link rel='stylesheet' id='slick-css'
                href='https://www.centralcons.vn/wp-content/themes/central-cons/assets/css/slick.css?ver=1.1'
                media='all' />
            <link rel='stylesheet' id='slick-theme-css'
                href='https://www.centralcons.vn/wp-content/themes/central-cons/assets/css/slick-theme.css?ver=1.1'
                media='all' />
            <link rel='stylesheet' id='awesome-css'
                href='https://www.centralcons.vn/wp-content/themes/central-cons/assets/css/font-awesome.min.css?ver=1.1'
                media='all' />
            <link rel='stylesheet' id='aos-css'
                href='https://www.centralcons.vn/wp-content/themes/central-cons/assets/css/aos.css?ver=1.1'
                media='all' />
            <link rel='stylesheet' id='fancybox-css'
                href='https://www.centralcons.vn/wp-content/themes/central-cons/assets/css/jquery.fancybox.min.css?ver=1.1'
                media='all' />
            <link rel='stylesheet' id='swipper-css'
                href='https://www.centralcons.vn/wp-content/themes/central-cons/assets/css/swiper-bundle.min.css?ver=1.1'
                media='all' />
            <link rel='stylesheet' id='main-css'
                href='https://www.centralcons.vn/wp-content/themes/central-cons/assets/css/main.css?ver=4.3'
                media='all' />
            <link rel='stylesheet' id='wp-pagenavi-css'
                href='https://www.centralcons.vn/wp-content/plugins/wp-pagenavi/pagenavi-css.css?ver=2.70'
                media='all' />
            <link rel='stylesheet' id='css-protect.css-css'
                href='https://www.centralcons.vn/wp-content/plugins/wccp-pro/css-protect.css?wccp_ver_num=10&#038;ver=10.9.2'
                media='all' />
            <script
                src="https://www.centralcons.vn/wp-content/themes/central-cons/assets/js/jquery-3.1.1.min.js?ver=3.1.1"
                id="jquery-core-js"></script>
            <script
                src="https://www.centralcons.vn/wp-content/themes/central-cons/assets/js/jquery-migrate-3.0.0.min.js?ver=3.0.0"
                id="jquery-migrate-js"></script>
            <link rel="https://api.w.org/" href="https://www.centralcons.vn/wp-json/" />
            <link rel="alternate" title="JSON" type="application/json"
                href="https://www.centralcons.vn/wp-json/wp/v2/pages/1161" />
            <link rel="EditURI" type="application/rsd+xml" title="RSD"
                href="https://www.centralcons.vn/xmlrpc.php?rsd" />
            <meta name="generator" content="WordPress 6.7.2" />
            <link rel='shortlink' href='https://www.centralcons.vn/?p=1161' />
            <link rel="alternate" title="oNhúng (JSON)" type="application/json+oembed"
                href="https://www.centralcons.vn/wp-json/oembed/1.0/embed?url=https%3A%2F%2Fwww.centralcons.vn%2Fdu-an%2F" />
            <link rel="alternate" title="oNhúng (XML)" type="text/xml+oembed"
                href="https://www.centralcons.vn/wp-json/oembed/1.0/embed?url=https%3A%2F%2Fwww.centralcons.vn%2Fdu-an%2F&#038;format=xml" />
            <link rel="icon" href="https://www.centralcons.vn/wp-content/uploads/2021/11/cropped-logo_mark-300x300.png"
                sizes="32x32" />
            <link rel="icon" href="https://www.centralcons.vn/wp-content/uploads/2021/11/cropped-logo_mark-300x300.png"
                sizes="192x192" />
            <link rel="apple-touch-icon"
                href="https://www.centralcons.vn/wp-content/uploads/2021/11/cropped-logo_mark-300x300.png" />
            <meta name="msapplication-TileImage"
                content="https://www.centralcons.vn/wp-content/uploads/2021/11/cropped-logo_mark-300x300.png" />
            <style type="text/css">
                /** Mega Menu CSS: fs **/
            </style>
            <!-- Google tag (gtag.js) -->
            <script async src="https://www.googletagmanager.com/gtag/js?id=UA-140682733-1"></script>
            <script>
  window.dataLayer = window.dataLayer || [];
  function gtag(){dataLayer.push(arguments);}
  gtag('js', new Date());

  gtag('config', 'UA-140682733-1');
</script>
        </head>

        <body
            class="page-template page-template-page-filter page-template-page-filter-php page page-id-1161 mega-menu-menu-1 unselectable"
            data-aos-easing="ease" data-aos-duration="800" data-aos-delay="0">
            <!--[if lt IE 8]>
<p class="browserupgrade">You are using an <strong>outdated</strong> browser. Please <a href="http://browsehappy.com/">upgrade
    your browser</a> to improve your experience.</p>
<![endif]-->
            <div id="fb-root" class=" fb_reset">
                <div style="position: absolute; top: -10000px; width: 0px; height: 0px;">
                    <div></div>
                </div>
            </div>
            <script async="" defer="" crossorigin="anonymous"
                src="https://connect.facebook.net/en_US/sdk.js#xfbml=1&amp;version=v12.0&amp;appId=495836187288100&amp;autoLogAppEvents=1"
                nonce="iyvSn4dU"></script>
            <Header />
            <div class="wrapper_content">
                <div class="page_cover">
                    <div class="bg_cover">
                        <img src="https://www.centralcons.vn/wp-content/uploads/2022/02/MAIN-IMAGE-2000-x-1080-scaled-e1645354391937.jpg"
                            class="img-fluid" alt="" draggable="false">
                    </div>
                    <div class="meta_info">
                        <h2 class="fs-40 fw-bold">Dự án</h2>
                        <div class="breadcrumb_site"><span><span><a href="https://www.centralcons.vn/">Home</a></span> »
                                <span class="breadcrumb_last" aria-current="page">Dự án</span></span></div>
                    </div>
                    <div class="stripe">
                        <div class="container">
                            <div class="stripe_banner">
                            </div>
                        </div>
                    </div>
                </div>
                <div class="filter_project py-md-5 py-3">
                    <div class="container">
                        <div class="row align-items-center">
                            <div class="col-md">
                                <div class="d-md-flex justify-content-between align-items-center">
                                    <ul>
                                        <li class=""><a href="https://www.centralcons.vn/du-an/">TẤT CẢ</a></li>
                                        <li class="text-uppercase active"><a
                                                href="https://www.centralcons.vn/du-an/?catename=10">Nổi bật</a></li>
                                        <li class="text-uppercase "><a
                                                href="https://www.centralcons.vn/du-an/?catename=11">Khu dân cư</a></li>
                                        <li class="text-uppercase "><a
                                                href="https://www.centralcons.vn/du-an/?catename=12">Nghỉ dưỡng - Khách
                                                sạn - TTTM</a></li>
                                        <li class="text-uppercase "><a
                                                href="https://www.centralcons.vn/du-an/?catename=13">Cao ốc văn
                                                phòng</a></li>
                                        <li class="text-uppercase "><a
                                                href="https://www.centralcons.vn/du-an/?catename=14">Sức khỏe &amp; Giáo
                                                dục</a></li>
                                        <li class="text-uppercase "><a
                                                href="https://www.centralcons.vn/du-an/?catename=15">Công nghiệp</a>
                                        </li>
                                        <li class="text-uppercase "><a
                                                href="https://www.centralcons.vn/du-an/?catename=283">Hạ tầng</a></li>
                                        <li class="text-uppercase "><a
                                                href="https://www.centralcons.vn/du-an/?catename=630">Dự án quốc tế</a>
                                        </li>
                                    </ul>
                                    <div class="filter">
                                        <a href="javascript:;" class="text-nowrap">
                                            <i class="fa fa-sliders" aria-hidden="true"></i> Lọc </a>
                                    </div>
                                </div>

                            </div>
                            <div class="col-md-auto">
                                <div class="d-md-flex">
                                    <div class="d-none view__option">
                                        <a href="javascript:;" class="me-3 gridview active"><i class="fa fa-th"
                                                aria-hidden="true"></i></a>
                                        <a href="javascript:;" class="me-3 listview"><i class="fa fa-list"
                                                aria-hidden="true"></i></a>
                                    </div>
                                    <a href="https://www.google.com/maps/d/u/0/viewer?mid=1vlXU4KhERW0P9IxlfkD5Ltnug1iy3lwL&amp;ll=15.484992582890763%2C106.48091650496168&amp;z=6"
                                        target="_blank"><i class="fa fa-map-o" aria-hidden="true"></i> Map dự án</a>
                                </div>
                            </div>
                        </div>
                    </div>
                </div>
                <div class="filter_advance" style="display: block">
                    <div class="container">
                        <form action="https://www.centralcons.vn/du-an/" method="get" class="py-4">
                            <div class="row align-items-center">
                                <div class="col-md">
                                    <div class="row align-items-center">
                                        <div class="col-md-2">
                                            <h4 class="fs-20 fw-bold">Năm</h4>
                                        </div>
                                        <div class="col-md-10">
                                            <ul>
                                                <li><input type="checkbox" class="all_item_year" name="years[]"
                                                        value="-1" id="py-all"> <label for="py-all">All</label></li>
                                                <li><input type="checkbox" class="item_year" name="years[]" value="629"
                                                        id="py-629"> <label for="py-629">2025</label></li>
                                                <li><input type="checkbox" class="item_year" name="years[]" value="576"
                                                        id="py-576"> <label for="py-576">2024</label></li>
                                                <li><input type="checkbox" class="item_year" name="years[]" value="523"
                                                        id="py-523"> <label for="py-523">2023</label></li>
                                                <li><input type="checkbox" class="item_year" name="years[]" value="517"
                                                        id="py-517"> <label for="py-517">2017</label></li>
                                                <li><input type="checkbox" class="item_year" name="years[]" value="138"
                                                        id="py-138"> <label for="py-138">2018</label></li>
                                                <li><input type="checkbox" class="item_year" name="years[]" value="135"
                                                        id="py-135"> <label for="py-135">2019</label></li>
                                                <li><input type="checkbox" class="item_year" name="years[]" value="136"
                                                        id="py-136"> <label for="py-136">2020</label></li>
                                                <li><input type="checkbox" class="item_year" name="years[]" value="137"
                                                        id="py-137"> <label for="py-137">2021</label></li>
                                                <li><input type="checkbox" class="item_year" name="years[]" value="273"
                                                        id="py-273"> <label for="py-273">2022</label></li>
                                            </ul>
                                        </div>
                                        <div class="col-md-2">
                                            <h4 class="fs-20 fw-bold mt-3 mt-md-0">Khu vực</h4>
                                        </div>
                                        <div class="col-md-10">
                                            <ul>
                                                <li><input type="checkbox" class="all_item_regions" name="regions[]"
                                                        value="-1" id="pr-all"> <label for="pr-all">All</label></li>
                                                <li><input type="checkbox" class="item_regions" name="regions[]"
                                                        value="132" id="pr-132"> <label for="pr-132">Miền Bắc</label>
                                                </li>
                                                <li><input type="checkbox" class="item_regions" name="regions[]"
                                                        value="133" id="pr-133"> <label for="pr-133">Miền Trung</label>
                                                </li>
                                                <li><input type="checkbox" class="item_regions" name="regions[]"
                                                        value="134" id="pr-134"> <label for="pr-134">Miền Nam</label>
                                                </li>
                                            </ul>
                                        </div>
                                    </div>
                                </div>
                                <div class="col-md-auto">
                                    <div class="mt-3 mt-md-0">
                                        <input type="hidden" value="10" name="catename">
                                        <input type="reset" class="btn btn-orange" value="Đặt lại">
                                        <input type="submit" class="btn btn-orange" value="Xác nhận">
                                    </div>
                                </div>
                            </div>
                        </form>
                    </div>
                </div>
                <div class="page_project py-5">
                    <div class="container">
                        <div class="row justify-content-center justify-content-md-start">
                            <div class="col-md-12 col-12">
                                <div class="project_item project_list">
                                    <figure>
                                        <a href="https://www.centralcons.vn/project/khach-sa%cc%a3n-5-sao-marriott/">
                                            <img width="800" height="450"
                                                src="https://www.centralcons.vn/wp-content/uploads/2023/01/thumbnail-SKYLINE-800x450.png"
                                                class="img-fluid wp-post-image" alt="" decoding="async"
                                                fetchpriority="high" draggable="false"> </a>
                                    </figure>
                                    <div class="project_meta">
                                        <h3 class="fs-26 fw-bold"><a
                                                href="https://www.centralcons.vn/project/khach-sa%cc%a3n-5-sao-marriott/">Khách
                                                Sạn 5 Sao Marriott</a></h3>
                                        <div class="excerpt">
                                            <p>Tổ hợp văn phòng, khách sạn, căn hộ cao cấp, tọa lạc vị trí trung tâm Thủ
                                                đô Hà Nội, được quản lý và vận hành theo tiêu chuẩn chất lượng quốc tế
                                                bởi Tập đoàn Marriott.</p>
                                        </div>
                                        <a href="https://www.centralcons.vn/project/khach-sa%cc%a3n-5-sao-marriott/"
                                            class="read_more">Xem chi tiết</a>
                                    </div>
                                </div>
                            </div>
                            <div class="col-md-12 col-12">
                                <div class="project_item project_list">
                                    <figure>
                                        <a href="https://www.centralcons.vn/project/vinhomes-dream-city/">
                                            <img width="800" height="450"
                                                src="https://www.centralcons.vn/wp-content/uploads/2022/07/Biet-thu-dao-dua-800x450.jpg"
                                                class="img-fluid wp-post-image" alt="" decoding="async"
                                                draggable="false"> </a>
                                    </figure>
                                    <div class="project_meta">
                                        <h3 class="fs-26 fw-bold"><a
                                                href="https://www.centralcons.vn/project/vinhomes-dream-city/">Vinhomes
                                                Dream City</a></h3>
                                        <div class="excerpt">
                                            <p>Vinhomes Dream City là đại đô thị kiểu mẫu, văn minh, hiện đại; đa dạng
                                                về sản phẩm nhà ở, thương mại, dịch vụ được kiến tạo để trở thành một
                                                “Khu đô thị sinh thái trong mơ” với nhiều…</p>
                                        </div>
                                        <a href="https://www.centralcons.vn/project/vinhomes-dream-city/"
                                            class="read_more">Xem chi tiết</a>
                                    </div>
                                </div>
                            </div>
                            <div class="col-md-12 col-12">
                                <div class="project_item project_list">
                                    <figure>
                                        <a href="https://www.centralcons.vn/project/the-beverly-solari/">
                                            <img width="800" height="450"
                                                src="https://www.centralcons.vn/wp-content/uploads/2023/11/the-beverly-soalri-edit-800x450.jpg"
                                                class="img-fluid wp-post-image" alt="" decoding="async"
                                                srcset="https://www.centralcons.vn/wp-content/uploads/2023/11/the-beverly-soalri-edit-800x450.jpg 800w, https://www.centralcons.vn/wp-content/uploads/2023/11/the-beverly-soalri-edit-1536x864.jpg 1536w, https://www.centralcons.vn/wp-content/uploads/2023/11/the-beverly-soalri-edit-2048x1152.jpg 2048w"
                                                sizes="(max-width: 800px) 100vw, 800px" draggable="false"> </a>
                                    </figure>
                                    <div class="project_meta">
                                        <h3 class="fs-26 fw-bold"><a
                                                href="https://www.centralcons.vn/project/the-beverly-solari/">The
                                                Tropical</a></h3>
                                        <div class="excerpt">
                                            <p>The Tropical nằm ngay trung tâm toàn khu, hưởng trọn vẹn hệ sinh thái đại
                                                công viên Grand Forest 36 hecta. Đặc biệt, phân khu The Beverly Solari
                                                còn nằm kế tuyến Metro số 1, view trực diện Sông Tắc,…</p>
                                        </div>
                                        <a href="https://www.centralcons.vn/project/the-beverly-solari/"
                                            class="read_more">Xem chi tiết</a>
                                    </div>
                                </div>
                            </div>
                            <div class="col-md-12 col-12">
                                <div class="project_item project_list">
                                    <figure>
                                        <a href="https://www.centralcons.vn/project/fairmont-hanoi/">
                                            <img width="800" height="450"
                                                src="https://www.centralcons.vn/wp-content/uploads/2022/07/fairmont-hanoi-2-1172x710-1-800x450.jpg"
                                                class="img-fluid wp-post-image" alt="" decoding="async"
                                                draggable="false"> </a>
                                    </figure>
                                    <div class="project_meta">
                                        <h3 class="fs-26 fw-bold"><a
                                                href="https://www.centralcons.vn/project/fairmont-hanoi/">Fairmont
                                                Hanoi</a></h3>
                                        <div class="excerpt">
                                            <p>Dự án được thiết kế bởi hai công ty nổi tiếng là Perkins Eastman và Aston
                                                Design,&nbsp;kiến trúc và nội thất của Khách sạn Fairmont Hanoi là sản
                                                phẩm của sự dày công nghiên cứu về văn hóa, lịch sử,…</p>
                                        </div>
                                        <a href="https://www.centralcons.vn/project/fairmont-hanoi/"
                                            class="read_more">Xem chi tiết</a>
                                    </div>
                                </div>
                            </div>
                            <div class="col-md-12 col-12">
                                <div class="project_item project_list">
                                    <figure>
                                        <a href="https://www.centralcons.vn/project/vinhomes-ocean-park/">
                                            <img width="800" height="450"
                                                src="https://www.centralcons.vn/wp-content/uploads/2022/06/1947269_154833_1G-800x450.jpg"
                                                class="img-fluid wp-post-image" alt="" decoding="async"
                                                draggable="false"> </a>
                                    </figure>
                                    <div class="project_meta">
                                        <h3 class="fs-26 fw-bold"><a
                                                href="https://www.centralcons.vn/project/vinhomes-ocean-park/">The
                                                Pavilion – Vinhomes Ocean Park</a></h3>
                                        <div class="excerpt">
                                            <p>Vinhomes Ocean Park là đại đô thị đẳng cấp quốc tế mang phong cách
                                                Singapore, sở hữu không gian xanh rộng lớn với những bãi cát trắng và
                                                hàng dừa nhiệt đới trải dài, mang đến một chất sống “đại…</p>
                                        </div>
                                        <a href="https://www.centralcons.vn/project/vinhomes-ocean-park/"
                                            class="read_more">Xem chi tiết</a>
                                    </div>
                                </div>
                            </div>
                            <div class="col-md-12 col-12">
                                <div class="project_item project_list">
                                    <figure>
                                        <a href="https://www.centralcons.vn/project/thuy-cung-vinpearl-phu-quoc/">
                                            <img width="800" height="389"
                                                src="https://www.centralcons.vn/wp-content/uploads/2022/04/thumnail.gif"
                                                class="img-fluid wp-post-image" alt="" decoding="async"
                                                draggable="false"> </a>
                                    </figure>
                                    <div class="project_meta">
                                        <h3 class="fs-26 fw-bold"><a
                                                href="https://www.centralcons.vn/project/thuy-cung-vinpearl-phu-quoc/">Thủy
                                                Cung Vinpearl Phú Quốc</a></h3>
                                        <div class="excerpt">
                                            <p>Thuỷ cung Kim Quy siêu lớn và hiện đại bậc nhất Đông Nam Á với không gian
                                                rộng 15.000m2 – là nơi sinh sống của 300 loài với gần 255 nghìn cá thể
                                                được sắp xếp và phân chia thành…</p>
                                        </div>
                                        <a href="https://www.centralcons.vn/project/thuy-cung-vinpearl-phu-quoc/"
                                            class="read_more">Xem chi tiết</a>
                                    </div>
                                </div>
                            </div>
                        </div>
                    </div>
                    <div class="paging_nav mt-4 mb-md-5">
                        <nav aria-label="Page navigation example">
                            <div class="wp-pagenavi" role="navigation">
                                <span aria-current="page" class="current">1</span><a class="page larger" title="Page 2"
                                    href="https://www.centralcons.vn/du-an/page/2/?catename=10">2</a><a
                                    class="page larger" title="Page 3"
                                    href="https://www.centralcons.vn/du-an/page/3/?catename=10">3</a><a
                                    class="page larger" title="Page 4"
                                    href="https://www.centralcons.vn/du-an/page/4/?catename=10">4</a><a
                                    class="nextpostslink" rel="next" aria-label="Trang sau"
                                    href="https://www.centralcons.vn/du-an/page/2/?catename=10">»</a>
                            </div>
                        </nav>
                    </div>
                </div>
            </div>


            <footer id="footer_site" class="">
                <div class="container">
                    <div class="pt-5 pb-3">
                        <div class="row">
                            <div class="col-md-3">
                                <div class="logo_box mb-3">
                                    <a href="" class="logo_ft">
                                        <img src="https://www.centralcons.vn/wp-content/uploads/2021/11/logo_ft.png"
                                            class="img-fluid" alt="" draggable="false">
                                    </a>
                                    <img src="https://www.centralcons.vn/wp-content/themes/central-cons/assets/images/mapft_bg.png"
                                        alt="" draggable="false">
                                </div>
                            </div>
                            <div class="col-md-9">
                                <div class="row">
                                    <div class="col-md-5 col-8 order-md-0">
                                        <div class="pe-md-3">
                                            <h3 class="fs-16 text-uppercase fw-bold mb-3">
                                                Liên Hệ </h3>
                                            <ul>
                                                <li><img src="https://www.centralcons.vn/wp-content/themes/central-cons/assets/images/marker.svg"
                                                        alt="" draggable="false"> 204/9 Nguyễn Văn Hưởng, P. Thảo Điền,
                                                    TP. Thủ Đức, TP. Hồ Chí Minh, Việt Nam</li>
                                                <li><img src="https://www.centralcons.vn/wp-content/themes/central-cons/assets/images/phone.svg"
                                                        alt="" draggable="false"> (+84) 28 3620 5151</li>
                                                <li><img src="https://www.centralcons.vn/wp-content/themes/central-cons/assets/images/email.svg"
                                                        alt="" draggable="false"> contact@centralcons.vn</li>
                                            </ul>
                                            <h3 class="fs-16 text-uppercase fw-bold mb-3">Văn phòng Hà Nội</h3>
                                            <ul>
                                                <li><img src="https://www.centralcons.vn/wp-content/themes/central-cons/assets/images/marker.svg"
                                                        alt="" draggable="false"> 6A, No1B, khu đô thị Sài Đồng, Phúc
                                                    Đồng, Long Biên, Hà Nội</li>
                                            </ul>
                                        </div>
                                    </div>
                                    <div class="col-md-3 col-4 order-md-1">
                                        <h3 class="fs-16 text-uppercase fw-bold mb-3">
                                            SITEMAP </h3>
                                        <ul id="menu-menu-footer" class="">
                                            <li id="menu-item-908"
                                                class="menu-item menu-item-type-post_type menu-item-object-about-central menu-item-908">
                                                <a href="https://www.centralcons.vn/about-central/tong-quan/">Giới
                                                    thiệu</a></li>
                                            <li id="menu-item-909"
                                                class="menu-item menu-item-type-post_type menu-item-object-what-we-do menu-item-909">
                                                <a href="https://www.centralcons.vn/what-we-do/linh-vuc-hoat-dong/">Lĩnh
                                                    vực hoạt động</a></li>
                                            <li id="menu-item-2353"
                                                class="menu-item menu-item-type-post_type menu-item-object-page current-menu-item page_item page-item-1161 current_page_item menu-item-2353">
                                                <a href="https://www.centralcons.vn/du-an/" aria-current="page">Dự
                                                    án</a></li>
                                            <li id="menu-item-911"
                                                class="menu-item menu-item-type-post_type menu-item-object-page menu-item-911">
                                                <a href="https://www.centralcons.vn/tin-du-an/">Tin dự án</a></li>
                                            <li id="menu-item-913"
                                                class="menu-item menu-item-type-post_type menu-item-object-page menu-item-913">
                                                <a href="https://www.centralcons.vn/co-hoi-nghe-nghiep/">Tuyển dụng</a>
                                            </li>
                                            <li id="menu-item-912"
                                                class="menu-item menu-item-type-post_type menu-item-object-page menu-item-912">
                                                <a href="https://www.centralcons.vn/lien-he/">Liên hệ</a></li>
                                        </ul>
                                    </div>
                                    <div class="col-md-3 col-6 order-md-2">
                                        <h3 class="fs-16 text-uppercase fw-bold mb-3">
                                            Theo dõi </h3>
                                        <ul class="social">
                                            <li><a href="https://www.facebook.com/Centralcons/" target="_blank"><i
                                                        class="fa fa-facebook"></i></a></li>
                                            <li><a href="https://www.youtube.com/channel/UCOeL6At05nMNbq4vyvCfiJQ"
                                                    target="_blank"><i class="fa fa-youtube-play"></i></a></li>
                                            <li><a href="https://erp.centralcons.vn" target="_blank"
                                                    class="fw-bold">ERP</a></li>
                                        </ul>
                                    </div>

                                </div>
                            </div>
                        </div>
                    </div>
                    <div class="copyright py-3 fs-13">
                        <div class="row justify-content-between">
                            <div class="col-auto">
                                © 2021 Central. All Rights Reserved.
                            </div>
                            <div class="col-auto">
                                <div class="d-flex">
                                    <div class="ps-3">
                                        <a href="#">Terms &amp; Conditions</a>
                                    </div>
                                    <div class="ps-3">
                                        <a href="#">Privacy policy</a>
                                    </div>
                                </div>
                            </div>
                        </div>
                    </div>
                </div>
            </footer>
            <!-- Modal -->
            <div class="modal fade" id="LeaderInfo" tabindex="-1" aria-labelledby="exampleModalLabel"
                aria-hidden="true">
                <div class="modal-dialog modal-lg modal-dialog-centered">
                    <div class="modal-content">
                        <div class="modal-body">
                            <a href="javascript:;" class="btn-close" data-bs-dismiss="modal" aria-label="Close"></a>
                            <div class="leader_content py-4">
                            </div>
                        </div>
                    </div>
                </div>
            </div>
            <script id="wccp_pro_alert_message">
	window.addEventListener('DOMContentLoaded', function() {}); //This line to stop JS deffer function in wp-rockt pluign
	
	window.addEventListener('load', function (){
		// Create the first div element with the "oncontextmenu" attribute
		const wccp_pro_mask = document.createElement('div');
		wccp_pro_mask.setAttribute('oncontextmenu', 'return false;');
		wccp_pro_mask.setAttribute('id', 'wccp_pro_mask');

		// Create the second div element with the "msgmsg-box-wpcp hideme" classes
		const wpcp_error_message = document.createElement('div');
		wpcp_error_message.setAttribute('id', 'wpcp-error-message');
		wpcp_error_message.setAttribute('class', 'msgmsg-box-wpcp hideme');

		// Add a span element with the "error: " text inside the second div
		const error_span = document.createElement('span');
		error_span.innerText = 'error: ';
		wpcp_error_message.appendChild(error_span);

		// Add the error message text inside the second div
		const error_text = document.createTextNode('<b>Alert: </b>Content selection is disabled!!');
		wpcp_error_message.appendChild(error_text);

		// Add the div elements to the document body
		document.body.appendChild(wccp_pro_mask);
		document.body.appendChild(wpcp_error_message);
	});

	var timeout_result;
	function show_wccp_pro_message(smessage="", style="")
	{
		wccp_pro_log_to_console_if_allowed("function", "show_wccp_pro_message" + smessage);
				
		timeout = 0;
		
		if(style == "") style = "warning-wpcp";
		
		if (smessage !== "" && timeout!=0)
		{
			var smessage_text = smessage;
			jquery_fadeTo();
			document.getElementById("wpcp-error-message").innerHTML = smessage_text;
			document.getElementById("wpcp-error-message").className = "msgmsg-box-wpcp showme " + style;
			clearTimeout(timeout_result);
			timeout_result = setTimeout(hide_message, timeout);
		}
		else
		{
			clearTimeout(timeout_result);
			timeout_result = setTimeout(hide_message, timeout);
		}
	}
	function hide_message()
	{
		jquery_fadeOut();
		document.getElementById("wpcp-error-message").className = "msgmsg-box-wpcp warning-wpcp hideme";
	}
	function jquery_fadeTo()
	{
		try {
			jQuery("#wccp_pro_mask").fadeTo("slow", 0.3);
		}
		catch(err) {
			//alert(err.message);
			}
	}
	function jquery_fadeOut()
	{
		try {
			jQuery("#wccp_pro_mask").fadeOut( "slow" );
		}
		catch(err) {}
	}
	</script>
            <style>
                #wccp_pro_mask {
                    position: absolute;
                    bottom: 0;
                    left: 0;
                    position: fixed;
                    right: 0;
                    top: 0;
                    background-color: #000;
                    pointer-events: none;
                    display: none;
                    z-index: 10000;
                    animation: 0.5s ease 0s normal none 1 running ngdialog-fadein;
                    background: rgba(0, 0, 0, 0.4) none repeat scroll 0 0;
                }

                #wpcp-error-message {
                    direction: ltr;
                    text-align: center;
                    transition: opacity 900ms ease 0s;
                    pointer-events: none;
                    z-index: 99999999;
                }

                .hideme {
                    opacity: 0;
                    visibility: hidden;
                }

                .showme {
                    opacity: 1;
                    visibility: visible;
                }

                .msgmsg-box-wpcp {
                    border-radius: 10px;
                    color: #555555;
                    font-family: Tahoma;
                    font-size: 12px;
                    margin: 10px;
                    padding: 10px 36px;
                    position: fixed;
                    width: 255px;
                    top: 50%;
                    left: 50%;
                    margin-top: -10px;
                    margin-left: -130px;
                }

                .msgmsg-box-wpcp b {
                    font-weight: bold;
                    text-transform: uppercase;
                }

                .warning-wpcp {
                    background: #ffecec url('https://www.centralcons.vn/wp-content/plugins/wccp-pro/images/warning.png') no-repeat 10px 50%;
                    border: 1px solid #f2bfbf;
                    -webkit-box-shadow: 0px 0px 34px 2px #f2bfbf;
                    -moz-box-shadow: 0px 0px 34px 2px #f2bfbf;
                    box-shadow: 0px 0px 34px 2px #f2bfbf;
                }

                .success-wpcp {
                    background: #fafafa url('https://www.centralcons.vn/wp-content/plugins/wccp-pro/images/success.png') no-repeat 10px 50%;
                    border: 1px solid #00b38f;
                    box-shadow: 0px 0px 34px 2px #adc;
                }
            </style>
            <script src="https://www.centralcons.vn/wp-includes/js/dist/hooks.min.js?ver=4d63a3d491d11ffd8ac6"
                id="wp-hooks-js"></script>
            <script src="https://www.centralcons.vn/wp-includes/js/dist/i18n.min.js?ver=5e580eb46a90c2b997e6"
                id="wp-i18n-js"></script>
            <script id="wp-i18n-js-after">
wp.i18n.setLocaleData( { 'text direction\u0004ltr': [ 'ltr' ] } );
</script>
            <script
                src="https://www.centralcons.vn/wp-content/plugins/contact-form-7/includes/swv/js/index.js?ver=5.9.8"
                id="swv-js"></script>
            <script id="contact-form-7-js-extra">
var wpcf7 = {"api":{"root":"https:\/\/www.centralcons.vn\/wp-json\/","namespace":"contact-form-7\/v1"}};
</script>
            <script id="contact-form-7-js-translations">
( function( domain, translations ) {
	var localeData = translations.locale_data[ domain ] || translations.locale_data.messages;
	localeData[""].domain = domain;
	wp.i18n.setLocaleData( localeData, domain );
} )( "contact-form-7", {"translation-revision-date":"2024-08-11 13:44:17+0000","generator":"GlotPress\/4.0.1","domain":"messages","locale_data":{"messages":{"":{"domain":"messages","plural-forms":"nplurals=1; plural=0;","lang":"vi_VN"},"This contact form is placed in the wrong place.":["Bi\u1ec3u m\u1eabu li\u00ean h\u1ec7 n\u00e0y \u0111\u01b0\u1ee3c \u0111\u1eb7t sai v\u1ecb tr\u00ed."],"Error:":["L\u1ed7i:"]}},"comment":{"reference":"includes\/js\/index.js"}} );
</script>
            <script src="https://www.centralcons.vn/wp-content/plugins/contact-form-7/includes/js/index.js?ver=5.9.8"
                id="contact-form-7-js"></script>
            <script src="https://www.centralcons.vn/wp-content/themes/central-cons/js/navigation.js?ver=1.0.3"
                id="central-cons-navigation-js"></script>
            <script
                src="https://www.centralcons.vn/wp-content/themes/central-cons/assets/js/bootstrap.bundle.min.js?ver=1.1"
                id="bootstrap-js"></script>
            <script src="https://www.centralcons.vn/wp-content/themes/central-cons/assets/js/slick.min.js?ver=1.1"
                id="slick-js"></script>
            <script
                src="https://www.centralcons.vn/wp-content/themes/central-cons/assets/js/jquery.fancybox.min.js?ver=2.6"
                id="fancybox-js"></script>
            <script src="https://www.centralcons.vn/wp-content/themes/central-cons/assets/js/aos.js?ver=2.6"
                id="aos-js"></script>
            <script
                src="https://www.centralcons.vn/wp-content/themes/central-cons/assets/js/swiper-bundle.min.js?ver=2.6"
                id="swipper-js"></script>
            <script src="https://www.centralcons.vn/wp-content/themes/central-cons/assets/js/waypoints.min.js?ver=2.6"
                id="waypoint-js"></script>
            <script
                src="https://www.centralcons.vn/wp-content/themes/central-cons/assets/js/jquery.counterup.min.js?ver=2.6"
                id="counter-js"></script>
            <script
                src="https://www.centralcons.vn/wp-content/themes/central-cons/assets/js/gsap-latest-beta.min.js?ver=1.3"
                id="gsap-js"></script>
            <script src="https://www.centralcons.vn/wp-content/themes/central-cons/assets/js/main.js?ver=1.7"
                id="main-js"></script>
            <script id="wccp_pro_admin_bar_ajax-js-extra">
var ajax_object = {"ajaxurl":"https:\/\/www.centralcons.vn\/wp-admin\/admin-ajax.php","link":"https:\/\/www.centralcons.vn\/du-an\/?catename=10"};
</script>
            <script src="https://www.centralcons.vn/wp-content/plugins/wccp-pro/js/admin_bar_ajax.js?ver=6.7.2"
                id="wccp_pro_admin_bar_ajax-js"></script>
            <script
                src="https://www.google.com/recaptcha/api.js?render=6LcnJQkhAAAAAJpdpCwVam7w3s7-8BfV4wZ3dXNN&amp;ver=3.0"
                id="google-recaptcha-js"></script>
            <script src="https://www.centralcons.vn/wp-includes/js/dist/vendor/wp-polyfill.min.js?ver=3.15.0"
                id="wp-polyfill-js"></script>
            <script id="wpcf7-recaptcha-js-extra">
var wpcf7_recaptcha = {"sitekey":"6LcnJQkhAAAAAJpdpCwVam7w3s7-8BfV4wZ3dXNN","actions":{"homepage":"homepage","contactform":"contactform"}};
</script>
            <script
                src="https://www.centralcons.vn/wp-content/plugins/contact-form-7/modules/recaptcha/index.js?ver=5.9.8"
                id="wpcf7-recaptcha-js"></script>
            <script src="https://www.centralcons.vn/wp-includes/js/hoverIntent.min.js?ver=1.10.2"
                id="hoverIntent-js"></script>
            <script src="https://www.centralcons.vn/wp-content/plugins/megamenu/js/maxmegamenu.js?ver=3.4.1"
                id="megamenu-js"></script>



            <div>
                <div class="grecaptcha-badge" data-style="bottomright"
                    style="width: 256px; height: 60px; display: block; transition: right 0.3s; position: fixed; bottom: 14px; right: -186px; box-shadow: gray 0px 0px 5px; border-radius: 2px; overflow: hidden;">
                    <div class="grecaptcha-logo"><iframe title="reCAPTCHA" width="256" height="60" role="presentation"
                            name="a-7b6qjevi7vk5" frameborder="0" scrolling="no"
                            sandbox="allow-forms allow-popups allow-same-origin allow-scripts allow-top-navigation allow-modals allow-popups-to-escape-sandbox allow-storage-access-by-user-activation"
                            src="https://www.google.com/recaptcha/api2/anchor?ar=1&amp;k=6LcnJQkhAAAAAJpdpCwVam7w3s7-8BfV4wZ3dXNN&amp;co=aHR0cHM6Ly93d3cuY2VudHJhbGNvbnMudm46NDQz&amp;hl=en&amp;v=hbAq-YhJxOnlU-7cpgBoAJHb&amp;size=invisible&amp;cb=o6y1zm5ghrs"></iframe>
                    </div>
                    <div class="grecaptcha-error"></div><textarea id="g-recaptcha-response-100000"
                        name="g-recaptcha-response" class="g-recaptcha-response"
                        style="width: 250px; height: 40px; border: 1px solid rgb(193, 193, 193); margin: 10px 25px; padding: 0px; resize: none; display: none;"></textarea>
                </div><iframe style="display: none;"></iframe>
            </div>
            <div oncontextmenu="return false;" id="wccp_pro_mask"></div>
            <div id="wpcp-error-message" class="msgmsg-box-wpcp hideme"><span>error: </span>&lt;b&gt;Alert:
                &lt;/b&gt;Content selection is disabled!!</div>
        </body>

    </div>
</template>

<script>
import Header from '@/components/Header.vue'
export default {
    components: {
        Header
    }
}
</script>

<style>
#wccp_pro_mask {
    position: absolute;
    bottom: 0;
    left: 0;
    position: fixed;
    right: 0;
    top: 0;
    background-color: #000;
    pointer-events: none;
    display: none;
    z-index: 10000;
    animation: 0.5s ease 0s normal none 1 running ngdialog-fadein;
    background: rgba(0, 0, 0, 0.4) none repeat scroll 0 0;
}

#wpcp-error-message {
    direction: ltr;
    text-align: center;
    transition: opacity 900ms ease 0s;
    pointer-events: none;
    z-index: 99999999;
}

.hideme {
    opacity: 0;
    visibility: hidden;
}

.showme {
    opacity: 1;
    visibility: visible;
}

.msgmsg-box-wpcp {
    border-radius: 10px;
    color: #555555;
    font-family: Tahoma;
    font-size: 12px;
    margin: 10px;
    padding: 10px 36px;
    position: fixed;
    width: 255px;
    top: 50%;
    left: 50%;
    margin-top: -10px;
    margin-left: -130px;
}

.msgmsg-box-wpcp b {
    font-weight: bold;
    text-transform: uppercase;
}

.warning-wpcp {
    background: #ffecec url('https://www.centralcons.vn/wp-content/plugins/wccp-pro/images/warning.png') no-repeat 10px 50%;
    border: 1px solid #f2bfbf;
    -webkit-box-shadow: 0px 0px 34px 2px #f2bfbf;
    -moz-box-shadow: 0px 0px 34px 2px #f2bfbf;
    box-shadow: 0px 0px 34px 2px #f2bfbf;
}

.success-wpcp {
    background: #fafafa url('https://www.centralcons.vn/wp-content/plugins/wccp-pro/images/success.png') no-repeat 10px 50%;
    border: 1px solid #00b38f;
    box-shadow: 0px 0px 34px 2px #adc;
}
</style>