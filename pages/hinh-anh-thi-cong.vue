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
            <link rel="alternate" href="https://www.centralcons.vn/hinh-anh-thi-cong/" hreflang="vi" />
            <link rel="alternate" href="https://www.centralcons.vn/en/gallery/" hreflang="en" />
            <link rel="alternate" href="https://www.centralcons.vn/ch/%e5%9b%be%e4%b9%a6%e9%a6%86/" hreflang="zh" />

            <!-- This site is optimized with the Yoast SEO plugin v22.1 - https://yoast.com/wordpress/plugins/seo/ -->
            <title>Hình ảnh thi công - CENTRAL</title>
            <link rel="canonical" href="https://www.centralcons.vn/hinh-anh-thi-cong/" />
            <meta property="og:locale" content="vi_VN" />
            <meta property="og:locale:alternate" content="en_US" />
            <meta property="og:locale:alternate" content="zh_CN" />
            <meta property="og:type" content="article" />
            <meta property="og:title" content="Hình ảnh thi công - CENTRAL" />
            <meta property="og:url" content="https://www.centralcons.vn/hinh-anh-thi-cong/" />
            <meta property="og:site_name" content="CENTRAL" />
            <meta property="article:modified_time" content="2022-03-02T05:09:43+00:00" />
            <meta property="og:image"
                content="https://www.centralcons.vn/wp-content/uploads/2022/04/3456DB59-8729-4F39-A00A-7AAD586A8085.png" />
            <meta property="og:image:width" content="1609" />
            <meta property="og:image:height" content="1998" />
            <meta property="og:image:type" content="image/png" />
            <meta name="twitter:card" content="summary_large_image" />
            <script type="application/ld+json"
                class="yoast-schema-graph">{"@context":"https://schema.org","@graph":[{"@type":"WebPage","@id":"https://www.centralcons.vn/hinh-anh-thi-cong/","url":"https://www.centralcons.vn/hinh-anh-thi-cong/","name":"Hình ảnh thi công - CENTRAL","isPartOf":{"@id":"https://www.centralcons.vn/#website"},"datePublished":"2021-11-21T10:26:11+00:00","dateModified":"2022-03-02T05:09:43+00:00","breadcrumb":{"@id":"https://www.centralcons.vn/hinh-anh-thi-cong/#breadcrumb"},"inLanguage":"vi","potentialAction":[{"@type":"ReadAction","target":["https://www.centralcons.vn/hinh-anh-thi-cong/"]}]},{"@type":"BreadcrumbList","@id":"https://www.centralcons.vn/hinh-anh-thi-cong/#breadcrumb","itemListElement":[{"@type":"ListItem","position":1,"name":"Home","item":"https://www.centralcons.vn/"},{"@type":"ListItem","position":2,"name":"Hình ảnh thi công"}]},{"@type":"WebSite","@id":"https://www.centralcons.vn/#website","url":"https://www.centralcons.vn/","name":"CENTRAL","description":"","potentialAction":[{"@type":"SearchAction","target":{"@type":"EntryPoint","urlTemplate":"https://www.centralcons.vn/?s={search_term_string}"},"query-input":"required name=search_term_string"}],"inLanguage":"vi"}]}</script>
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
                href="https://www.centralcons.vn/wp-json/wp/v2/pages/333" />
            <link rel="EditURI" type="application/rsd+xml" title="RSD"
                href="https://www.centralcons.vn/xmlrpc.php?rsd" />
            <meta name="generator" content="WordPress 6.7.2" />
            <link rel='shortlink' href='https://www.centralcons.vn/?p=333' />
            <link rel="alternate" title="oNhúng (JSON)" type="application/json+oembed"
                href="https://www.centralcons.vn/wp-json/oembed/1.0/embed?url=https%3A%2F%2Fwww.centralcons.vn%2Fhinh-anh-thi-cong%2F" />
            <link rel="alternate" title="oNhúng (XML)" type="text/xml+oembed"
                href="https://www.centralcons.vn/wp-json/oembed/1.0/embed?url=https%3A%2F%2Fwww.centralcons.vn%2Fhinh-anh-thi-cong%2F&#038;format=xml" />
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
            class="page-template page-template-page-gallery page-template-page-gallery-php page page-id-333 mega-menu-menu-1 unselectable">
            <!--[if lt IE 8]>
<p class="browserupgrade">You are using an <strong>outdated</strong> browser. Please <a href="http://browsehappy.com/">upgrade
    your browser</a> to improve your experience.</p>
<![endif]-->
            <div id="fb-root"></div>
            <script async defer crossorigin="anonymous"
                src="https://connect.facebook.net/en_US/sdk.js#xfbml=1&version=v12.0&appId=495836187288100&autoLogAppEvents=1"
                nonce="iyvSn4dU"></script>
            <Header />
            <div class="wrapper_content">
                <div class="page_cover">
                    <div class="bg_cover">
                        <img src="https://www.centralcons.vn/wp-content/themes/central-cons/assets/images/cover_about.jpg"
                            class="img-fluid" alt="">
                    </div>
                    <div class="meta_info">
                        <h2 class="fs-40 fw-bold">Hình ảnh thi công</h2>
                        <div class="breadcrumb_site"><span><span><a href="https://www.centralcons.vn/">Home</a></span> »
                                <span class="breadcrumb_last" aria-current="page">Hình ảnh thi công</span></span></div>
                    </div>
                    <div class="stripe">
                        <div class="container">
                            <div class="stripe_banner">
                            </div>
                        </div>
                    </div>
                </div>
                <div class="news__page py-5">
                    <div class="container">
                        <div class="menu_news">
                            <div class="row align-items-center">
                                <div class="col-md-6">
                                    <h3 class="fs-40 fw-bold text-uppercase">Hình ảnh thi công</h3>
                                </div>
                                <div class="col-md-6">
                                    <ul class="menu-news">
                                        <li><a href="https://www.centralcons.vn/gallery/hse/">HSE</a></li>
                                        <li><a href="https://www.centralcons.vn/gallery/logistic-3/">Logistic</a></li>
                                        <li><a
                                                href="https://www.centralcons.vn/gallery/structure-finishing/">Structure</a>
                                        </li>
                                        <li><a href="https://www.centralcons.vn/gallery/mepf/">MEPF</a></li>
                                        <li><a href="https://www.centralcons.vn/gallery/ban-giao/">Equipment</a></li>
                                        <li><a href="https://www.centralcons.vn/gallery/interior/">Interior &#038;
                                                Landscape</a></li>
                                    </ul>
                                </div>
                            </div>
                        </div>
                        <div class="other_post">

                            <div class="row">
                                <div class="col-md-4">
                                    <div class="item_post" data-aos="fade-up">
                                        <figure>
                                            <a href="https://www.centralcons.vn/wp-content/uploads/2021/12/z2709239218870_261b271b666313748368e5c0ae47071b.jpg"
                                                data-fancybox="galerry-all"
                                                data-options='{"caption" : "Huấn luyện đầu giờ triển khai công việc tại Alma Resort với hơn 3000 công nhân"}'>
                                                <img src="https://www.centralcons.vn/wp-content/uploads/2021/12/z2709239218870_261b271b666313748368e5c0ae47071b-720x450.jpg"
                                                    class="img-fluid" alt="">
                                            </a>
                                        </figure>
                                    </div>
                                </div>
                                <div class="col-md-4">
                                    <div class="item_post" data-aos="fade-up">
                                        <figure>
                                            <a href="https://www.centralcons.vn/wp-content/uploads/2021/12/DJI_0250-111.jpg"
                                                data-fancybox="galerry-all" data-options='{"caption" : ""}'>
                                                <img src="https://www.centralcons.vn/wp-content/uploads/2021/12/DJI_0250-111-720x450.jpg"
                                                    class="img-fluid" alt="">
                                            </a>
                                        </figure>
                                    </div>
                                </div>
                                <div class="col-md-4">
                                    <div class="item_post" data-aos="fade-up">
                                        <figure>
                                            <a href="https://www.centralcons.vn/wp-content/uploads/2021/12/DJI_0276-111.jpg"
                                                data-fancybox="galerry-all" data-options='{"caption" : ""}'>
                                                <img src="https://www.centralcons.vn/wp-content/uploads/2021/12/DJI_0276-111-720x450.jpg"
                                                    class="img-fluid" alt="">
                                            </a>
                                        </figure>
                                    </div>
                                </div>
                                <div class="col-md-4">
                                    <div class="item_post" data-aos="fade-up">
                                        <figure>
                                            <a href="https://www.centralcons.vn/wp-content/uploads/2022/03/z3570247492370_262fc9dae88e25eb7bf110c71a19c784.jpg"
                                                data-fancybox="galerry-all"
                                                data-options='{"caption" : "Huấn luyện An Toàn đầu giờ tại Dự Án Astral City"}'>
                                                <img src="https://www.centralcons.vn/wp-content/uploads/2022/03/z3570247492370_262fc9dae88e25eb7bf110c71a19c784-720x450.jpg"
                                                    class="img-fluid" alt="">
                                            </a>
                                        </figure>
                                    </div>
                                </div>
                                <div class="col-md-4">
                                    <div class="item_post" data-aos="fade-up">
                                        <figure>
                                            <a href="https://www.centralcons.vn/wp-content/uploads/2022/03/z3570247488882_97bf04555aef28654068c8b438a5cea0.jpg"
                                                data-fancybox="galerry-all"
                                                data-options='{"caption" : "Huấn luyện An Toàn đầu giờ tại Dự Án Astral City"}'>
                                                <img src="https://www.centralcons.vn/wp-content/uploads/2022/03/z3570247488882_97bf04555aef28654068c8b438a5cea0-720x450.jpg"
                                                    class="img-fluid" alt="">
                                            </a>
                                        </figure>
                                    </div>
                                </div>
                                <div class="col-md-4">
                                    <div class="item_post" data-aos="fade-up">
                                        <figure>
                                            <a href="https://www.centralcons.vn/wp-content/uploads/2021/12/DJI_0272-Retouch.jpg"
                                                data-fancybox="galerry-all" data-options='{"caption" : ""}'>
                                                <img src="https://www.centralcons.vn/wp-content/uploads/2021/12/DJI_0272-Retouch-720x450.jpg"
                                                    class="img-fluid" alt="">
                                            </a>
                                        </figure>
                                    </div>
                                </div>
                                <div class="col-md-4">
                                    <div class="item_post" data-aos="fade-up">
                                        <figure>
                                            <a href="https://www.centralcons.vn/wp-content/uploads/2021/12/AN-TOAN-4.jpg"
                                                data-fancybox="galerry-all"
                                                data-options='{"caption" : "Huấn luyện An Toàn đầu giờ"}'>
                                                <img src="https://www.centralcons.vn/wp-content/uploads/2021/12/AN-TOAN-4-720x450.jpg"
                                                    class="img-fluid" alt="">
                                            </a>
                                        </figure>
                                    </div>
                                </div>
                                <div class="col-md-4">
                                    <div class="item_post" data-aos="fade-up">
                                        <figure>
                                            <a href="https://www.centralcons.vn/wp-content/uploads/2021/12/z2709278180342_a89831d8e3e93d3030df2c532bf5f1d6.jpg"
                                                data-fancybox="galerry-all"
                                                data-options='{"caption" : "Huấn luyện An Toàn đầu giờ"}'>
                                                <img src="https://www.centralcons.vn/wp-content/uploads/2021/12/z2709278180342_a89831d8e3e93d3030df2c532bf5f1d6-720x450.jpg"
                                                    class="img-fluid" alt="">
                                            </a>
                                        </figure>
                                    </div>
                                </div>
                                <div class="col-md-4">
                                    <div class="item_post" data-aos="fade-up">
                                        <figure>
                                            <a href="https://www.centralcons.vn/wp-content/uploads/2021/12/cdc29c78ccfe3ea067ef-1.jpg"
                                                data-fancybox="galerry-all"
                                                data-options='{"caption" : "Diễn tập PCCC tại Dự Án Chadwick"}'>
                                                <img src="https://www.centralcons.vn/wp-content/uploads/2021/12/cdc29c78ccfe3ea067ef-1-720x450.jpg"
                                                    class="img-fluid" alt="">
                                            </a>
                                        </figure>
                                    </div>
                                </div>
                                <div class="col-md-4">
                                    <div class="item_post" data-aos="fade-up">
                                        <figure>
                                            <a href="https://www.centralcons.vn/wp-content/uploads/2021/12/z2709239206409_ee1407b8db8bd8210daabeb738fbe157.jpg"
                                                data-fancybox="galerry-all"
                                                data-options='{"caption" : "Huấn luyện An Toàn đầu giờ"}'>
                                                <img src="https://www.centralcons.vn/wp-content/uploads/2021/12/z2709239206409_ee1407b8db8bd8210daabeb738fbe157-720x450.jpg"
                                                    class="img-fluid" alt="">
                                            </a>
                                        </figure>
                                    </div>
                                </div>
                                <div class="col-md-4">
                                    <div class="item_post" data-aos="fade-up">
                                        <figure>
                                            <a href="https://www.centralcons.vn/wp-content/uploads/2021/12/4bca486ef15838066149-scaled.jpg"
                                                data-fancybox="galerry-all"
                                                data-options='{"caption" : "Tập thể dục sáng sớm"}'>
                                                <img src="https://www.centralcons.vn/wp-content/uploads/2021/12/4bca486ef15838066149-scaled-720x450.jpg"
                                                    class="img-fluid" alt="">
                                            </a>
                                        </figure>
                                    </div>
                                </div>
                                <div class="col-md-4">
                                    <div class="item_post" data-aos="fade-up">
                                        <figure>
                                            <a href="https://www.centralcons.vn/wp-content/uploads/2021/12/dao-tao.jpg"
                                                data-fancybox="galerry-all"
                                                data-options='{"caption" : "Huấn luyện An Toàn đầu giờ"}'>
                                                <img src="https://www.centralcons.vn/wp-content/uploads/2021/12/dao-tao-720x450.jpg"
                                                    class="img-fluid" alt="">
                                            </a>
                                        </figure>
                                    </div>
                                </div>
                                <div class="col-md-4">
                                    <div class="item_post" data-aos="fade-up">
                                        <figure>
                                            <a href="https://www.centralcons.vn/wp-content/uploads/2021/12/9127213429aecff096bf.jpg"
                                                data-fancybox="galerry-all"
                                                data-options='{"caption" : "Đào tạo kiến thức An Toàn Lao Động"}'>
                                                <img src="https://www.centralcons.vn/wp-content/uploads/2021/12/9127213429aecff096bf-720x450.jpg"
                                                    class="img-fluid" alt="">
                                            </a>
                                        </figure>
                                    </div>
                                </div>
                                <div class="col-md-4">
                                    <div class="item_post" data-aos="fade-up">
                                        <figure>
                                            <a href="https://www.centralcons.vn/wp-content/uploads/2021/12/z2414750475342_75da9ebb25dabde7d306e8a7697010f5.jpg"
                                                data-fancybox="galerry-all"
                                                data-options='{"caption" : "Hoạt động Green hour"}'>
                                                <img src="https://www.centralcons.vn/wp-content/uploads/2021/12/z2414750475342_75da9ebb25dabde7d306e8a7697010f5-720x450.jpg"
                                                    class="img-fluid" alt="">
                                            </a>
                                        </figure>
                                    </div>
                                </div>
                                <div class="col-md-4">
                                    <div class="item_post" data-aos="fade-up">
                                        <figure>
                                            <a href="https://www.centralcons.vn/wp-content/uploads/2021/12/z2414738523263_b78f6e7fe36fb746b69f77655afee87c.jpg"
                                                data-fancybox="galerry-all"
                                                data-options='{"caption" : "Hoạt động Green hour"}'>
                                                <img src="https://www.centralcons.vn/wp-content/uploads/2021/12/z2414738523263_b78f6e7fe36fb746b69f77655afee87c-720x450.jpg"
                                                    class="img-fluid" alt="">
                                            </a>
                                        </figure>
                                    </div>
                                </div>
                                <div class="col-md-4">
                                    <div class="item_post" data-aos="fade-up">
                                        <figure>
                                            <a href="https://www.centralcons.vn/wp-content/uploads/2021/12/z2414738522231_28309040767c2a1e81bd26d67f6a98df.jpg"
                                                data-fancybox="galerry-all"
                                                data-options='{"caption" : "Hoạt động Green hour"}'>
                                                <img src="https://www.centralcons.vn/wp-content/uploads/2021/12/z2414738522231_28309040767c2a1e81bd26d67f6a98df-720x450.jpg"
                                                    class="img-fluid" alt="">
                                            </a>
                                        </figure>
                                    </div>
                                </div>
                                <div class="col-md-4">
                                    <div class="item_post" data-aos="fade-up">
                                        <figure>
                                            <a href="https://www.centralcons.vn/wp-content/uploads/2021/12/b61c3fe6c40e3050691f.jpg"
                                                data-fancybox="galerry-all"
                                                data-options='{"caption" : "Hoạt động Green hour"}'>
                                                <img src="https://www.centralcons.vn/wp-content/uploads/2021/12/b61c3fe6c40e3050691f-720x450.jpg"
                                                    class="img-fluid" alt="">
                                            </a>
                                        </figure>
                                    </div>
                                </div>
                                <div class="col-md-4">
                                    <div class="item_post" data-aos="fade-up">
                                        <figure>
                                            <a href="https://www.centralcons.vn/wp-content/uploads/2021/12/24af23587fff99a1c0ee.jpg"
                                                data-fancybox="galerry-all" data-options='{"caption" : ""}'>
                                                <img src="https://www.centralcons.vn/wp-content/uploads/2021/12/24af23587fff99a1c0ee-720x450.jpg"
                                                    class="img-fluid" alt="">
                                            </a>
                                        </figure>
                                    </div>
                                </div>
                                <div class="col-md-4">
                                    <div class="item_post" data-aos="fade-up">
                                        <figure>
                                            <a href="https://www.centralcons.vn/wp-content/uploads/2021/12/131022119_812850282616010_5547004836758080739_n.jpg"
                                                data-fancybox="galerry-all"
                                                data-options='{"caption" : "Huấn luyện PCCC"}'>
                                                <img src="https://www.centralcons.vn/wp-content/uploads/2021/12/131022119_812850282616010_5547004836758080739_n-720x450.jpg"
                                                    class="img-fluid" alt="">
                                            </a>
                                        </figure>
                                    </div>
                                </div>
                                <div class="col-md-4">
                                    <div class="item_post" data-aos="fade-up">
                                        <figure>
                                            <a href="https://www.centralcons.vn/wp-content/uploads/2021/12/130906274_812849202616118_6811789101673347936_n.jpg"
                                                data-fancybox="galerry-all" data-options='{"caption" : ""}'>
                                                <img src="https://www.centralcons.vn/wp-content/uploads/2021/12/130906274_812849202616118_6811789101673347936_n-720x450.jpg"
                                                    class="img-fluid" alt="">
                                            </a>
                                        </figure>
                                    </div>
                                </div>
                                <div class="col-md-4">
                                    <div class="item_post" data-aos="fade-up">
                                        <figure>
                                            <a href="https://www.centralcons.vn/wp-content/uploads/2021/12/131398232_812850335949338_7613413773769744135_n.jpg"
                                                data-fancybox="galerry-all"
                                                data-options='{"caption" : "Huấn luyện PCCC"}'>
                                                <img src="https://www.centralcons.vn/wp-content/uploads/2021/12/131398232_812850335949338_7613413773769744135_n-720x450.jpg"
                                                    class="img-fluid" alt="">
                                            </a>
                                        </figure>
                                    </div>
                                </div>
                                <div class="col-md-4">
                                    <div class="item_post" data-aos="fade-up">
                                        <figure>
                                            <a href="https://www.centralcons.vn/wp-content/uploads/2021/12/DJI_0144-scaled.jpg"
                                                data-fancybox="galerry-all"
                                                data-options='{"caption" : "Văn phòng BCH The Beverly Solari"}'>
                                                <img src="https://www.centralcons.vn/wp-content/uploads/2021/12/DJI_0144-scaled-720x450.jpg"
                                                    class="img-fluid" alt="">
                                            </a>
                                        </figure>
                                    </div>
                                </div>
                                <div class="col-md-4">
                                    <div class="item_post" data-aos="fade-up">
                                        <figure>
                                            <a href="https://www.centralcons.vn/wp-content/uploads/2021/12/DJI_0143-scaled.jpg"
                                                data-fancybox="galerry-all"
                                                data-options='{"caption" : "Văn phòng BCH The Beverly Solari"}'>
                                                <img src="https://www.centralcons.vn/wp-content/uploads/2021/12/DJI_0143-scaled-720x450.jpg"
                                                    class="img-fluid" alt="">
                                            </a>
                                        </figure>
                                    </div>
                                </div>
                                <div class="col-md-4">
                                    <div class="item_post" data-aos="fade-up">
                                        <figure>
                                            <a href="https://www.centralcons.vn/wp-content/uploads/2021/12/DJI_0140-scaled.jpg"
                                                data-fancybox="galerry-all"
                                                data-options='{"caption" : "Bảng biển cổng trước Dự Án The Beverly Solari"}'>
                                                <img src="https://www.centralcons.vn/wp-content/uploads/2021/12/DJI_0140-scaled-720x450.jpg"
                                                    class="img-fluid" alt="">
                                            </a>
                                        </figure>
                                    </div>
                                </div>
                                <div class="col-md-4">
                                    <div class="item_post" data-aos="fade-up">
                                        <figure>
                                            <a href="https://www.centralcons.vn/wp-content/uploads/2021/12/2c12e6094aa1b7ffeeb0-scaled.jpg"
                                                data-fancybox="galerry-all"
                                                data-options='{"caption" : "Lối đi bộ hành lang VP BCH"}'>
                                                <img src="https://www.centralcons.vn/wp-content/uploads/2021/12/2c12e6094aa1b7ffeeb0-scaled-720x450.jpg"
                                                    class="img-fluid" alt="">
                                            </a>
                                        </figure>
                                    </div>
                                </div>
                                <div class="col-md-4">
                                    <div class="item_post" data-aos="fade-up">
                                        <figure>
                                            <a href="https://www.centralcons.vn/wp-content/uploads/2021/12/DJI_0116-scaled.jpg"
                                                data-fancybox="galerry-all"
                                                data-options='{"caption" : "Bảng biển cổng trước Dự Án The Beverly Solari"}'>
                                                <img src="https://www.centralcons.vn/wp-content/uploads/2021/12/DJI_0116-scaled-720x450.jpg"
                                                    class="img-fluid" alt="">
                                            </a>
                                        </figure>
                                    </div>
                                </div>
                                <div class="col-md-4">
                                    <div class="item_post" data-aos="fade-up">
                                        <figure>
                                            <a href="https://www.centralcons.vn/wp-content/uploads/2021/12/a3e5db55d4261b784237.jpg"
                                                data-fancybox="galerry-all" data-options='{"caption" : ""}'>
                                                <img src="https://www.centralcons.vn/wp-content/uploads/2021/12/a3e5db55d4261b784237-720x450.jpg"
                                                    class="img-fluid" alt="">
                                            </a>
                                        </figure>
                                    </div>
                                </div>
                                <div class="col-md-4">
                                    <div class="item_post" data-aos="fade-up">
                                        <figure>
                                            <a href="https://www.centralcons.vn/wp-content/uploads/2021/12/6b1b75465835976bce24.jpg"
                                                data-fancybox="galerry-all" data-options='{"caption" : ""}'>
                                                <img src="https://www.centralcons.vn/wp-content/uploads/2021/12/6b1b75465835976bce24-720x450.jpg"
                                                    class="img-fluid" alt="">
                                            </a>
                                        </figure>
                                    </div>
                                </div>
                                <div class="col-md-4">
                                    <div class="item_post" data-aos="fade-up">
                                        <figure>
                                            <a href="https://www.centralcons.vn/wp-content/uploads/2021/12/DJI_0221-scaled.jpg"
                                                data-fancybox="galerry-all" data-options='{"caption" : ""}'>
                                                <img src="https://www.centralcons.vn/wp-content/uploads/2021/12/DJI_0221-scaled-720x450.jpg"
                                                    class="img-fluid" alt="">
                                            </a>
                                        </figure>
                                    </div>
                                </div>
                                <div class="col-md-4">
                                    <div class="item_post" data-aos="fade-up">
                                        <figure>
                                            <a href="https://www.centralcons.vn/wp-content/uploads/2021/12/3-1-2.jpg"
                                                data-fancybox="galerry-all" data-options='{"caption" : ""}'>
                                                <img src="https://www.centralcons.vn/wp-content/uploads/2021/12/3-1-2-720x450.jpg"
                                                    class="img-fluid" alt="">
                                            </a>
                                        </figure>
                                    </div>
                                </div>
                                <div class="col-md-4">
                                    <div class="item_post" data-aos="fade-up">
                                        <figure>
                                            <a href="https://www.centralcons.vn/wp-content/uploads/2021/12/60f65ec498b757e90ea6.jpg"
                                                data-fancybox="galerry-all"
                                                data-options='{"caption" : "Bảng biển cổng trước Dự Án"}'>
                                                <img src="https://www.centralcons.vn/wp-content/uploads/2021/12/60f65ec498b757e90ea6-720x450.jpg"
                                                    class="img-fluid" alt="">
                                            </a>
                                        </figure>
                                    </div>
                                </div>
                                <div class="col-md-4">
                                    <div class="item_post" data-aos="fade-up">
                                        <figure>
                                            <a href="https://www.centralcons.vn/wp-content/uploads/2021/12/286f3e4b92e36fbd36f2-scaled.jpg"
                                                data-fancybox="galerry-all"
                                                data-options='{"caption" : "Mặt tiền nhà điều hành BCH Dự Án"}'>
                                                <img src="https://www.centralcons.vn/wp-content/uploads/2021/12/286f3e4b92e36fbd36f2-scaled-720x450.jpg"
                                                    class="img-fluid" alt="">
                                            </a>
                                        </figure>
                                    </div>
                                </div>
                                <div class="col-md-4">
                                    <div class="item_post" data-aos="fade-up">
                                        <figure>
                                            <a href="https://www.centralcons.vn/wp-content/uploads/2021/12/f4a8e107d8df2d8174ce28-scaled.jpg"
                                                data-fancybox="galerry-all" data-options='{"caption" : ""}'>
                                                <img src="https://www.centralcons.vn/wp-content/uploads/2021/12/f4a8e107d8df2d8174ce28-scaled-720x450.jpg"
                                                    class="img-fluid" alt="">
                                            </a>
                                        </figure>
                                    </div>
                                </div>
                                <div class="col-md-4">
                                    <div class="item_post" data-aos="fade-up">
                                        <figure>
                                            <a href="https://www.centralcons.vn/wp-content/uploads/2021/12/bch-masteri-scaled.jpg"
                                                data-fancybox="galerry-all"
                                                data-options='{"caption" : "Văn phòng BCH Masteri"}'>
                                                <img src="https://www.centralcons.vn/wp-content/uploads/2021/12/bch-masteri-720x450.jpg"
                                                    class="img-fluid" alt="">
                                            </a>
                                        </figure>
                                    </div>
                                </div>
                                <div class="col-md-4">
                                    <div class="item_post" data-aos="fade-up">
                                        <figure>
                                            <a href="https://www.centralcons.vn/wp-content/uploads/2021/12/bch-masteri-2.jpg"
                                                data-fancybox="galerry-all"
                                                data-options='{"caption" : "Văn phòng BCH Masteri"}'>
                                                <img src="https://www.centralcons.vn/wp-content/uploads/2021/12/bch-masteri-2-720x450.jpg"
                                                    class="img-fluid" alt="">
                                            </a>
                                        </figure>
                                    </div>
                                </div>
                                <div class="col-md-4">
                                    <div class="item_post" data-aos="fade-up">
                                        <figure>
                                            <a href="https://www.centralcons.vn/wp-content/uploads/2021/12/751c972f2ba0edfeb4b16-scaled.jpg"
                                                data-fancybox="galerry-all" data-options='{"caption" : ""}'>
                                                <img src="https://www.centralcons.vn/wp-content/uploads/2021/12/751c972f2ba0edfeb4b16-scaled-720x450.jpg"
                                                    class="img-fluid" alt="">
                                            </a>
                                        </figure>
                                    </div>
                                </div>
                                <div class="col-md-4">
                                    <div class="item_post" data-aos="fade-up">
                                        <figure>
                                            <a href="https://www.centralcons.vn/wp-content/uploads/2021/12/DJI_0045.jpg"
                                                data-fancybox="galerry-all"
                                                data-options='{"caption" : "Dự Án Haruka 1DEF Becamex Tokyu"}'>
                                                <img src="https://www.centralcons.vn/wp-content/uploads/2021/12/DJI_0045-720x450.jpg"
                                                    class="img-fluid" alt="">
                                            </a>
                                        </figure>
                                    </div>
                                </div>
                                <div class="col-md-4">
                                    <div class="item_post" data-aos="fade-up">
                                        <figure>
                                            <a href="https://www.centralcons.vn/wp-content/uploads/2021/12/DJI_0049-1.jpg"
                                                data-fancybox="galerry-all"
                                                data-options='{"caption" : "Dự Án Haruka 1DEF Becamex Tokyu"}'>
                                                <img src="https://www.centralcons.vn/wp-content/uploads/2021/12/DJI_0049-1-720x450.jpg"
                                                    class="img-fluid" alt="">
                                            </a>
                                        </figure>
                                    </div>
                                </div>
                                <div class="col-md-4">
                                    <div class="item_post" data-aos="fade-up">
                                        <figure>
                                            <a href="https://www.centralcons.vn/wp-content/uploads/2021/12/DJI_0063-scaled.jpg"
                                                data-fancybox="galerry-all"
                                                data-options='{"caption" : "Dự Án Haruka 1DEF Becamex Tokyu"}'>
                                                <img src="https://www.centralcons.vn/wp-content/uploads/2021/12/DJI_0063-scaled-720x450.jpg"
                                                    class="img-fluid" alt="">
                                            </a>
                                        </figure>
                                    </div>
                                </div>
                                <div class="col-md-4">
                                    <div class="item_post" data-aos="fade-up">
                                        <figure>
                                            <a href="https://www.centralcons.vn/wp-content/uploads/2021/12/DJI_0017-scaled.jpg"
                                                data-fancybox="galerry-all"
                                                data-options='{"caption" : "Dự Án The Metropole"}'>
                                                <img src="https://www.centralcons.vn/wp-content/uploads/2021/12/DJI_0017-scaled-720x450.jpg"
                                                    class="img-fluid" alt="">
                                            </a>
                                        </figure>
                                    </div>
                                </div>
                                <div class="col-md-4">
                                    <div class="item_post" data-aos="fade-up">
                                        <figure>
                                            <a href="https://www.centralcons.vn/wp-content/uploads/2021/12/c06cac140b21c57f9c30-1.jpg"
                                                data-fancybox="galerry-all"
                                                data-options='{"caption" : "Dự Án Charm Resort & Spa"}'>
                                                <img src="https://www.centralcons.vn/wp-content/uploads/2021/12/c06cac140b21c57f9c30-1-720x450.jpg"
                                                    class="img-fluid" alt="">
                                            </a>
                                        </figure>
                                    </div>
                                </div>
                                <div class="col-md-4">
                                    <div class="item_post" data-aos="fade-up">
                                        <figure>
                                            <a href="https://www.centralcons.vn/wp-content/uploads/2021/12/DJI_06273-scaled.jpg"
                                                data-fancybox="galerry-all"
                                                data-options='{"caption" : "Dự Án Charm Resort & Spa"}'>
                                                <img src="https://www.centralcons.vn/wp-content/uploads/2021/12/DJI_06273-scaled-720x450.jpg"
                                                    class="img-fluid" alt="">
                                            </a>
                                        </figure>
                                    </div>
                                </div>
                                <div class="col-md-4">
                                    <div class="item_post" data-aos="fade-up">
                                        <figure>
                                            <a href="https://www.centralcons.vn/wp-content/uploads/2021/12/IMG_0536-scaled.jpg"
                                                data-fancybox="galerry-all"
                                                data-options='{"caption" : "Thực thi phần kết cấu Thủy Cung VAP Phú Quốc"}'>
                                                <img src="https://www.centralcons.vn/wp-content/uploads/2021/12/IMG_0536-scaled-720x450.jpg"
                                                    class="img-fluid" alt="">
                                            </a>
                                        </figure>
                                    </div>
                                </div>
                                <div class="col-md-4">
                                    <div class="item_post" data-aos="fade-up">
                                        <figure>
                                            <a href="https://www.centralcons.vn/wp-content/uploads/2021/12/DJI_0638-scaled.jpg"
                                                data-fancybox="galerry-all"
                                                data-options='{"caption" : "Dự Án Thuỷ Cung Vinpearl Land Phú Quốc"}'>
                                                <img src="https://www.centralcons.vn/wp-content/uploads/2021/12/DJI_0638-scaled-720x450.jpg"
                                                    class="img-fluid" alt="">
                                            </a>
                                        </figure>
                                    </div>
                                </div>
                                <div class="col-md-4">
                                    <div class="item_post" data-aos="fade-up">
                                        <figure>
                                            <a href="https://www.centralcons.vn/wp-content/uploads/2021/12/IMG_0526-scaled.jpg"
                                                data-fancybox="galerry-all"
                                                data-options='{"caption" : "Thực thi phần kết cấu Thủy Cung VAP Phú Quốc"}'>
                                                <img src="https://www.centralcons.vn/wp-content/uploads/2021/12/IMG_0526-scaled-720x450.jpg"
                                                    class="img-fluid" alt="">
                                            </a>
                                        </figure>
                                    </div>
                                </div>
                                <div class="col-md-4">
                                    <div class="item_post" data-aos="fade-up">
                                        <figure>
                                            <a href="https://www.centralcons.vn/wp-content/uploads/2021/12/IMG_0488-scaled.jpg"
                                                data-fancybox="galerry-all"
                                                data-options='{"caption" : "Thực thi phần kết cấu Thủy Cung VAP Phú Quốc"}'>
                                                <img src="https://www.centralcons.vn/wp-content/uploads/2021/12/IMG_0488-scaled-720x450.jpg"
                                                    class="img-fluid" alt="">
                                            </a>
                                        </figure>
                                    </div>
                                </div>
                                <div class="col-md-4">
                                    <div class="item_post" data-aos="fade-up">
                                        <figure>
                                            <a href="https://www.centralcons.vn/wp-content/uploads/2021/12/471195579de869b630f9-1.jpg"
                                                data-fancybox="galerry-all"
                                                data-options='{"caption" : "Dự Án Nhà Máy Điện Gió Lợi Hải 2"}'>
                                                <img src="https://www.centralcons.vn/wp-content/uploads/2021/12/471195579de869b630f9-1-720x450.jpg"
                                                    class="img-fluid" alt="">
                                            </a>
                                        </figure>
                                    </div>
                                </div>
                                <div class="col-md-4">
                                    <div class="item_post" data-aos="fade-up">
                                        <figure>
                                            <a href="https://www.centralcons.vn/wp-content/uploads/2021/12/DJI_0789_1-scaled.jpg"
                                                data-fancybox="galerry-all"
                                                data-options='{"caption" : "Đổ móng trụ điện gió Phú Lạc 2"}'>
                                                <img src="https://www.centralcons.vn/wp-content/uploads/2021/12/DJI_0789_1-scaled-720x450.jpg"
                                                    class="img-fluid" alt="">
                                            </a>
                                        </figure>
                                    </div>
                                </div>
                                <div class="col-md-4">
                                    <div class="item_post" data-aos="fade-up">
                                        <figure>
                                            <a href="https://www.centralcons.vn/wp-content/uploads/2021/12/Banner-Tuyển-dụng-Chính-sách-nhân-sự-scaled.jpg"
                                                data-fancybox="galerry-all"
                                                data-options='{"caption" : "Dự Án Vinhomes Grand Park"}'>
                                                <img src="https://www.centralcons.vn/wp-content/uploads/2021/12/Banner-Tuyển-dụng-Chính-sách-nhân-sự-scaled-720x450.jpg"
                                                    class="img-fluid" alt="">
                                            </a>
                                        </figure>
                                    </div>
                                </div>
                                <div class="col-md-4">
                                    <div class="item_post" data-aos="fade-up">
                                        <figure>
                                            <a href="https://www.centralcons.vn/wp-content/uploads/2021/12/4b7536c8a9e163bf3af04.jpg"
                                                data-fancybox="galerry-all"
                                                data-options='{"caption" : "Dự án Vega City"}'>
                                                <img src="https://www.centralcons.vn/wp-content/uploads/2021/12/4b7536c8a9e163bf3af04-720x450.jpg"
                                                    class="img-fluid" alt="">
                                            </a>
                                        </figure>
                                    </div>
                                </div>
                                <div class="col-md-4">
                                    <div class="item_post" data-aos="fade-up">
                                        <figure>
                                            <a href="https://www.centralcons.vn/wp-content/uploads/2021/12/DJI_0616-2-scaled.jpg"
                                                data-fancybox="galerry-all"
                                                data-options='{"caption" : "Dự Án Vinhomes Smart City"}'>
                                                <img src="https://www.centralcons.vn/wp-content/uploads/2021/12/DJI_0616-2-scaled-720x450.jpg"
                                                    class="img-fluid" alt="">
                                            </a>
                                        </figure>
                                    </div>
                                </div>
                                <div class="col-md-4">
                                    <div class="item_post" data-aos="fade-up">
                                        <figure>
                                            <a href="https://www.centralcons.vn/wp-content/uploads/2021/12/central-vinhomes-grand-park-catnoc-h3-2.jpg"
                                                data-fancybox="galerry-all"
                                                data-options='{"caption" : "Dự Án Vinhomes Grand Park"}'>
                                                <img src="https://www.centralcons.vn/wp-content/uploads/2021/12/central-vinhomes-grand-park-catnoc-h3-2-720x450.jpg"
                                                    class="img-fluid" alt="">
                                            </a>
                                        </figure>
                                    </div>
                                </div>
                                <div class="col-md-4">
                                    <div class="item_post" data-aos="fade-up">
                                        <figure>
                                            <a href="https://www.centralcons.vn/wp-content/uploads/2021/12/DJI_0179-scaled.jpg"
                                                data-fancybox="galerry-all"
                                                data-options='{"caption" : "Thực thi phần thân Dự Án Hope Residences"}'>
                                                <img src="https://www.centralcons.vn/wp-content/uploads/2021/12/DJI_0179-scaled-720x450.jpg"
                                                    class="img-fluid" alt="">
                                            </a>
                                        </figure>
                                    </div>
                                </div>
                                <div class="col-md-4">
                                    <div class="item_post" data-aos="fade-up">
                                        <figure>
                                            <a href="https://www.centralcons.vn/wp-content/uploads/2021/12/DJI_0882-retouch-scaled.jpg"
                                                data-fancybox="galerry-all"
                                                data-options='{"caption" : "Thực thi phần thân Dự Án The Metropole Thủ Thiêm"}'>
                                                <img src="https://www.centralcons.vn/wp-content/uploads/2021/12/DJI_0882-retouch-scaled-720x450.jpg"
                                                    class="img-fluid" alt="">
                                            </a>
                                        </figure>
                                    </div>
                                </div>
                                <div class="col-md-4">
                                    <div class="item_post" data-aos="fade-up">
                                        <figure>
                                            <a href="https://www.centralcons.vn/wp-content/uploads/2021/12/DJI_0048-scaled.jpg"
                                                data-fancybox="galerry-all"
                                                data-options='{"caption" : "Dự Án Vinhomes River Park"}'>
                                                <img src="https://www.centralcons.vn/wp-content/uploads/2021/12/DJI_0048-scaled-720x450.jpg"
                                                    class="img-fluid" alt="">
                                            </a>
                                        </figure>
                                    </div>
                                </div>
                                <div class="col-md-4">
                                    <div class="item_post" data-aos="fade-up">
                                        <figure>
                                            <a href="https://www.centralcons.vn/wp-content/uploads/2021/12/DJI_0921-retouch-scaled.jpg"
                                                data-fancybox="galerry-all"
                                                data-options='{"caption" : "Dự Án Masterise Centre Point"}'>
                                                <img src="https://www.centralcons.vn/wp-content/uploads/2021/12/DJI_0921-retouch-scaled-720x450.jpg"
                                                    class="img-fluid" alt="">
                                            </a>
                                        </figure>
                                    </div>
                                </div>
                                <div class="col-md-4">
                                    <div class="item_post" data-aos="fade-up">
                                        <figure>
                                            <a href="https://www.centralcons.vn/wp-content/uploads/2021/12/z2689972641407_3604aa748d54d33f4fea52a2e4f48a85.jpg"
                                                data-fancybox="galerry-all" data-options='{"caption" : ""}'>
                                                <img src="https://www.centralcons.vn/wp-content/uploads/2021/12/z2689972641407_3604aa748d54d33f4fea52a2e4f48a85-720x450.jpg"
                                                    class="img-fluid" alt="">
                                            </a>
                                        </figure>
                                    </div>
                                </div>
                                <div class="col-md-4">
                                    <div class="item_post" data-aos="fade-up">
                                        <figure>
                                            <a href="https://www.centralcons.vn/wp-content/uploads/2021/12/z2709346380547_3c471c8272f173b9d45c376af1d8276e.jpg"
                                                data-fancybox="galerry-all" data-options='{"caption" : ""}'>
                                                <img src="https://www.centralcons.vn/wp-content/uploads/2021/12/z2709346380547_3c471c8272f173b9d45c376af1d8276e-720x450.jpg"
                                                    class="img-fluid" alt="">
                                            </a>
                                        </figure>
                                    </div>
                                </div>
                                <div class="col-md-4">
                                    <div class="item_post" data-aos="fade-up">
                                        <figure>
                                            <a href="https://www.centralcons.vn/wp-content/uploads/2021/12/z2711481786400_6a6bb0d2bd278ced0edf00d3c2cbd995.jpg"
                                                data-fancybox="galerry-all" data-options='{"caption" : ""}'>
                                                <img src="https://www.centralcons.vn/wp-content/uploads/2021/12/z2711481786400_6a6bb0d2bd278ced0edf00d3c2cbd995-720x450.jpg"
                                                    class="img-fluid" alt="">
                                            </a>
                                        </figure>
                                    </div>
                                </div>
                                <div class="col-md-4">
                                    <div class="item_post" data-aos="fade-up">
                                        <figure>
                                            <a href="https://www.centralcons.vn/wp-content/uploads/2021/12/z2709346325160_a0625a98bb36c8191fab47afc8fe52c6.jpg"
                                                data-fancybox="galerry-all" data-options='{"caption" : ""}'>
                                                <img src="https://www.centralcons.vn/wp-content/uploads/2021/12/z2709346325160_a0625a98bb36c8191fab47afc8fe52c6-720x450.jpg"
                                                    class="img-fluid" alt="">
                                            </a>
                                        </figure>
                                    </div>
                                </div>
                                <div class="col-md-4">
                                    <div class="item_post" data-aos="fade-up">
                                        <figure>
                                            <a href="https://www.centralcons.vn/wp-content/uploads/2021/12/01fdfb9713a1eeffb7b0.jpg"
                                                data-fancybox="galerry-all"
                                                data-options='{"caption" : "Hệ thống MEPF tầng mái"}'>
                                                <img src="https://www.centralcons.vn/wp-content/uploads/2021/12/01fdfb9713a1eeffb7b0-720x450.jpg"
                                                    class="img-fluid" alt="">
                                            </a>
                                        </figure>
                                    </div>
                                </div>
                                <div class="col-md-4">
                                    <div class="item_post" data-aos="fade-up">
                                        <figure>
                                            <a href="https://www.centralcons.vn/wp-content/uploads/2021/12/6ee72d8ac5bc38e261ad.jpg"
                                                data-fancybox="galerry-all"
                                                data-options='{"caption" : "Hệ thống MEPF tầng mái"}'>
                                                <img src="https://www.centralcons.vn/wp-content/uploads/2021/12/6ee72d8ac5bc38e261ad-720x450.jpg"
                                                    class="img-fluid" alt="">
                                            </a>
                                        </figure>
                                    </div>
                                </div>
                                <div class="col-md-4">
                                    <div class="item_post" data-aos="fade-up">
                                        <figure>
                                            <a href="https://www.centralcons.vn/wp-content/uploads/2021/12/29f46e9886ae7bf022bf.jpg"
                                                data-fancybox="galerry-all"
                                                data-options='{"caption" : "Hệ thống thông gió và tạo áp tầng mái"}'>
                                                <img src="https://www.centralcons.vn/wp-content/uploads/2021/12/29f46e9886ae7bf022bf-720x450.jpg"
                                                    class="img-fluid" alt="">
                                            </a>
                                        </figure>
                                    </div>
                                </div>
                                <div class="col-md-4">
                                    <div class="item_post" data-aos="fade-up">
                                        <figure>
                                            <a href="https://www.centralcons.vn/wp-content/uploads/2021/12/0-19bd4f68e3fb1ea547ea.jpg"
                                                data-fancybox="galerry-all"
                                                data-options='{"caption" : "Hệ thống Cooling Tower tầng mái"}'>
                                                <img src="https://www.centralcons.vn/wp-content/uploads/2021/12/0-19bd4f68e3fb1ea547ea-720x450.jpg"
                                                    class="img-fluid" alt="">
                                            </a>
                                        </figure>
                                    </div>
                                </div>
                                <div class="col-md-4">
                                    <div class="item_post" data-aos="fade-up">
                                        <figure>
                                            <a href="https://www.centralcons.vn/wp-content/uploads/2021/12/779afff217c4ea9ab3d5.jpg"
                                                data-fancybox="galerry-all"
                                                data-options='{"caption" : "Hệ thống HVAC tầng mái"}'>
                                                <img src="https://www.centralcons.vn/wp-content/uploads/2021/12/779afff217c4ea9ab3d5-720x450.jpg"
                                                    class="img-fluid" alt="">
                                            </a>
                                        </figure>
                                    </div>
                                </div>
                                <div class="col-md-4">
                                    <div class="item_post" data-aos="fade-up">
                                        <figure>
                                            <a href="https://www.centralcons.vn/wp-content/uploads/2021/12/z2788765831169_c0f07089669efd3ce2414fc992990bcf.jpg"
                                                data-fancybox="galerry-all"
                                                data-options='{"caption" : "Hệ thống MEPF tầng mái"}'>
                                                <img src="https://www.centralcons.vn/wp-content/uploads/2021/12/z2788765831169_c0f07089669efd3ce2414fc992990bcf-720x450.jpg"
                                                    class="img-fluid" alt="">
                                            </a>
                                        </figure>
                                    </div>
                                </div>
                                <div class="col-md-4">
                                    <div class="item_post" data-aos="fade-up">
                                        <figure>
                                            <a href="https://www.centralcons.vn/wp-content/uploads/2021/12/z2788766247277_9dd80cb4dd969a5baa69837b059ab0d6.jpg"
                                                data-fancybox="galerry-all"
                                                data-options='{"caption" : "Hệ thống MEPF tầng mái"}'>
                                                <img src="https://www.centralcons.vn/wp-content/uploads/2021/12/z2788766247277_9dd80cb4dd969a5baa69837b059ab0d6-720x450.jpg"
                                                    class="img-fluid" alt="">
                                            </a>
                                        </figure>
                                    </div>
                                </div>
                                <div class="col-md-4">
                                    <div class="item_post" data-aos="fade-up">
                                        <figure>
                                            <a href="https://www.centralcons.vn/wp-content/uploads/2021/12/a5fcfc9914afe9f1b0be.jpg"
                                                data-fancybox="galerry-all"
                                                data-options='{"caption" : "Hệ thống cấp nước tầng mái"}'>
                                                <img src="https://www.centralcons.vn/wp-content/uploads/2021/12/a5fcfc9914afe9f1b0be-720x450.jpg"
                                                    class="img-fluid" alt="">
                                            </a>
                                        </figure>
                                    </div>
                                </div>
                                <div class="col-md-4">
                                    <div class="item_post" data-aos="fade-up">
                                        <figure>
                                            <a href="https://www.centralcons.vn/wp-content/uploads/2021/12/0-35ca31976f04925acb15.jpg"
                                                data-fancybox="galerry-all"
                                                data-options='{"caption" : "Hệ thống cấp nước tầng mái"}'>
                                                <img src="https://www.centralcons.vn/wp-content/uploads/2021/12/0-35ca31976f04925acb15-720x450.jpg"
                                                    class="img-fluid" alt="">
                                            </a>
                                        </figure>
                                    </div>
                                </div>
                                <div class="col-md-4">
                                    <div class="item_post" data-aos="fade-up">
                                        <figure>
                                            <a href="https://www.centralcons.vn/wp-content/uploads/2021/12/2-2.jpg"
                                                data-fancybox="galerry-all"
                                                data-options='{"caption" : "Hoàn thiện hệ thống MEPF sảnh thang máy"}'>
                                                <img src="https://www.centralcons.vn/wp-content/uploads/2021/12/2-2-720x450.jpg"
                                                    class="img-fluid" alt="">
                                            </a>
                                        </figure>
                                    </div>
                                </div>
                                <div class="col-md-4">
                                    <div class="item_post" data-aos="fade-up">
                                        <figure>
                                            <a href="https://www.centralcons.vn/wp-content/uploads/2021/12/50eeeada4649bb17e258.jpg"
                                                data-fancybox="galerry-all"
                                                data-options='{"caption" : "Hoàn thiện hệ thống MEPF sảnh hành lang thang máy"}'>
                                                <img src="https://www.centralcons.vn/wp-content/uploads/2021/12/50eeeada4649bb17e258-720x450.jpg"
                                                    class="img-fluid" alt="">
                                            </a>
                                        </figure>
                                    </div>
                                </div>
                                <div class="col-md-4">
                                    <div class="item_post" data-aos="fade-up">
                                        <figure>
                                            <a href="https://www.centralcons.vn/wp-content/uploads/2021/12/bd63fc065095adcbf484.jpg"
                                                data-fancybox="galerry-all"
                                                data-options='{"caption" : "Hoàn thiện hệ thống MEPF sảnh thang máy"}'>
                                                <img src="https://www.centralcons.vn/wp-content/uploads/2021/12/bd63fc065095adcbf484-720x450.jpg"
                                                    class="img-fluid" alt="">
                                            </a>
                                        </figure>
                                    </div>
                                </div>
                                <div class="col-md-4">
                                    <div class="item_post" data-aos="fade-up">
                                        <figure>
                                            <a href="https://www.centralcons.vn/wp-content/uploads/2021/12/5ae5cab1c871022f5b6082.jpg"
                                                data-fancybox="galerry-all"
                                                data-options='{"caption" : "Hệ thống Chiller phòng kỹ thuật"}'>
                                                <img src="https://www.centralcons.vn/wp-content/uploads/2021/12/5ae5cab1c871022f5b6082-720x450.jpg"
                                                    class="img-fluid" alt="">
                                            </a>
                                        </figure>
                                    </div>
                                </div>
                                <div class="col-md-4">
                                    <div class="item_post" data-aos="fade-up">
                                        <figure>
                                            <a href="https://www.centralcons.vn/wp-content/uploads/2021/12/25d025e22422ee7cb73341.jpg"
                                                data-fancybox="galerry-all"
                                                data-options='{"caption" : "Hệ thống Chiller phòng kỹ thuật"}'>
                                                <img src="https://www.centralcons.vn/wp-content/uploads/2021/12/25d025e22422ee7cb73341-720x450.jpg"
                                                    class="img-fluid" alt="">
                                            </a>
                                        </figure>
                                    </div>
                                </div>
                                <div class="col-md-4">
                                    <div class="item_post" data-aos="fade-up">
                                        <figure>
                                            <a href="https://www.centralcons.vn/wp-content/uploads/2021/12/4240920a363ccb62922d.jpg"
                                                data-fancybox="galerry-all"
                                                data-options='{"caption" : "Hệ thống Chiller phòng kỹ thuật"}'>
                                                <img src="https://www.centralcons.vn/wp-content/uploads/2021/12/4240920a363ccb62922d-720x450.jpg"
                                                    class="img-fluid" alt="">
                                            </a>
                                        </figure>
                                    </div>
                                </div>
                                <div class="col-md-4">
                                    <div class="item_post" data-aos="fade-up">
                                        <figure>
                                            <a href="https://www.centralcons.vn/wp-content/uploads/2021/12/4546030ca73a5a64032b.jpg"
                                                data-fancybox="galerry-all"
                                                data-options='{"caption" : "Hệ thống Chiller phòng kỹ thuật"}'>
                                                <img src="https://www.centralcons.vn/wp-content/uploads/2021/12/4546030ca73a5a64032b-720x450.jpg"
                                                    class="img-fluid" alt="">
                                            </a>
                                        </figure>
                                    </div>
                                </div>
                                <div class="col-md-4">
                                    <div class="item_post" data-aos="fade-up">
                                        <figure>
                                            <a href="https://www.centralcons.vn/wp-content/uploads/2021/12/IMG_6971-2-scaled.jpg"
                                                data-fancybox="galerry-all"
                                                data-options='{"caption" : "Hệ thống MEPF tầng hầm"}'>
                                                <img src="https://www.centralcons.vn/wp-content/uploads/2021/12/IMG_6971-2-720x450.jpg"
                                                    class="img-fluid" alt="">
                                            </a>
                                        </figure>
                                    </div>
                                </div>
                                <div class="col-md-4">
                                    <div class="item_post" data-aos="fade-up">
                                        <figure>
                                            <a href="https://www.centralcons.vn/wp-content/uploads/2021/12/0-818835896b1a9644cf0b.jpg"
                                                data-fancybox="galerry-all"
                                                data-options='{"caption" : "Hệ thống MEPF tầng hầm"}'>
                                                <img src="https://www.centralcons.vn/wp-content/uploads/2021/12/0-818835896b1a9644cf0b-720x450.jpg"
                                                    class="img-fluid" alt="">
                                            </a>
                                        </figure>
                                    </div>
                                </div>
                                <div class="col-md-4">
                                    <div class="item_post" data-aos="fade-up">
                                        <figure>
                                            <a href="https://www.centralcons.vn/wp-content/uploads/2021/12/1253a28e4bb8b6e6efa9.jpg"
                                                data-fancybox="galerry-all"
                                                data-options='{"caption" : "Bơm trung chuyển nước tầng hầm"}'>
                                                <img src="https://www.centralcons.vn/wp-content/uploads/2021/12/1253a28e4bb8b6e6efa9-720x450.jpg"
                                                    class="img-fluid" alt="">
                                            </a>
                                        </figure>
                                    </div>
                                </div>
                                <div class="col-md-4">
                                    <div class="item_post" data-aos="fade-up">
                                        <figure>
                                            <a href="https://www.centralcons.vn/wp-content/uploads/2021/12/27686fb486827bdc2293.jpg"
                                                data-fancybox="galerry-all"
                                                data-options='{"caption" : "Bơm trung chuyển nước tầng hầm"}'>
                                                <img src="https://www.centralcons.vn/wp-content/uploads/2021/12/27686fb486827bdc2293-720x450.jpg"
                                                    class="img-fluid" alt="">
                                            </a>
                                        </figure>
                                    </div>
                                </div>
                                <div class="col-md-4">
                                    <div class="item_post" data-aos="fade-up">
                                        <figure>
                                            <a href="https://www.centralcons.vn/wp-content/uploads/2021/12/5825c7a1c361093f5070-1.jpg"
                                                data-fancybox="galerry-all"
                                                data-options='{"caption" : "Phòng kỹ thuật điện (Tủ MSB)"}'>
                                                <img src="https://www.centralcons.vn/wp-content/uploads/2021/12/5825c7a1c361093f5070-1-720x450.jpg"
                                                    class="img-fluid" alt="">
                                            </a>
                                        </figure>
                                    </div>
                                </div>
                                <div class="col-md-4">
                                    <div class="item_post" data-aos="fade-up">
                                        <figure>
                                            <a href="https://www.centralcons.vn/wp-content/uploads/2021/12/z2788642107209_80db19ce9f0daa24f1db263fa092e6bb.jpg"
                                                data-fancybox="galerry-all"
                                                data-options='{"caption" : "Hệ thống đường ống và cáp điện phòng kỹ thuật điện"}'>
                                                <img src="https://www.centralcons.vn/wp-content/uploads/2021/12/z2788642107209_80db19ce9f0daa24f1db263fa092e6bb-720x450.jpg"
                                                    class="img-fluid" alt="">
                                            </a>
                                        </figure>
                                    </div>
                                </div>
                                <div class="col-md-4">
                                    <div class="item_post" data-aos="fade-up">
                                        <figure>
                                            <a href="https://www.centralcons.vn/wp-content/uploads/2021/12/z1890729142009_ac86f7de6aac8059f33fdcd720c1a797.jpg"
                                                data-fancybox="galerry-all"
                                                data-options='{"caption" : "Hệ thống máy phát điện"}'>
                                                <img src="https://www.centralcons.vn/wp-content/uploads/2021/12/z1890729142009_ac86f7de6aac8059f33fdcd720c1a797-720x450.jpg"
                                                    class="img-fluid" alt="">
                                            </a>
                                        </figure>
                                    </div>
                                </div>
                                <div class="col-md-4">
                                    <div class="item_post" data-aos="fade-up">
                                        <figure>
                                            <a href="https://www.centralcons.vn/wp-content/uploads/2021/12/IMG_1801-scaled.jpg"
                                                data-fancybox="galerry-all"
                                                data-options='{"caption" : "Hệ thống MEPF khu vực hành lang"}'>
                                                <img src="https://www.centralcons.vn/wp-content/uploads/2021/12/IMG_1801-scaled-720x450.jpg"
                                                    class="img-fluid" alt="">
                                            </a>
                                        </figure>
                                    </div>
                                </div>
                                <div class="col-md-4">
                                    <div class="item_post" data-aos="fade-up">
                                        <figure>
                                            <a href="https://www.centralcons.vn/wp-content/uploads/2021/12/IMG_6091-scaled.jpg"
                                                data-fancybox="galerry-all"
                                                data-options='{"caption" : "Kỹ Sư MEP tại Dự Án Empire City"}'>
                                                <img src="https://www.centralcons.vn/wp-content/uploads/2021/12/IMG_6091-scaled-720x450.jpg"
                                                    class="img-fluid" alt="">
                                            </a>
                                        </figure>
                                    </div>
                                </div>
                                <div class="col-md-4">
                                    <div class="item_post" data-aos="fade-up">
                                        <figure>
                                            <a href="https://www.centralcons.vn/wp-content/uploads/2021/12/IMG_6141-scaled.jpg"
                                                data-fancybox="galerry-all"
                                                data-options='{"caption" : "Kỹ Sư MEPF tại Dự Án Empire City"}'>
                                                <img src="https://www.centralcons.vn/wp-content/uploads/2021/12/IMG_6141-scaled-720x450.jpg"
                                                    class="img-fluid" alt="">
                                            </a>
                                        </figure>
                                    </div>
                                </div>
                                <div class="col-md-4">
                                    <div class="item_post" data-aos="fade-up">
                                        <figure>
                                            <a href="https://www.centralcons.vn/wp-content/uploads/2021/12/IMG_6881-scaled.jpg"
                                                data-fancybox="galerry-all"
                                                data-options='{"caption" : "Kỹ Sư MEPF tại Dự Án Empire City"}'>
                                                <img src="https://www.centralcons.vn/wp-content/uploads/2021/12/IMG_6881-scaled-720x450.jpg"
                                                    class="img-fluid" alt="">
                                            </a>
                                        </figure>
                                    </div>
                                </div>
                                <div class="col-md-4">
                                    <div class="item_post" data-aos="fade-up">
                                        <figure>
                                            <a href="https://www.centralcons.vn/wp-content/uploads/2021/12/DSC09733-Retouch-Retouch-1-scaled.jpg"
                                                data-fancybox="galerry-all" data-options='{"caption" : ""}'>
                                                <img src="https://www.centralcons.vn/wp-content/uploads/2021/12/DSC09733-Retouch-Retouch-1-scaled-720x450.jpg"
                                                    class="img-fluid" alt="">
                                            </a>
                                        </figure>
                                    </div>
                                </div>
                                <div class="col-md-4">
                                    <div class="item_post" data-aos="fade-up">
                                        <figure>
                                            <a href="https://www.centralcons.vn/wp-content/uploads/2021/12/DSC09751-Retouch-Retouch-1-scaled.jpg"
                                                data-fancybox="galerry-all" data-options='{"caption" : ""}'>
                                                <img src="https://www.centralcons.vn/wp-content/uploads/2021/12/DSC09751-Retouch-Retouch-1-scaled-720x450.jpg"
                                                    class="img-fluid" alt="">
                                            </a>
                                        </figure>
                                    </div>
                                </div>
                                <div class="col-md-4">
                                    <div class="item_post" data-aos="fade-up">
                                        <figure>
                                            <a href="https://www.centralcons.vn/wp-content/uploads/2021/12/DSC09757-Retouch-Retouch-1-scaled.jpg"
                                                data-fancybox="galerry-all" data-options='{"caption" : ""}'>
                                                <img src="https://www.centralcons.vn/wp-content/uploads/2021/12/DSC09757-Retouch-Retouch-1-scaled-720x450.jpg"
                                                    class="img-fluid" alt="">
                                            </a>
                                        </figure>
                                    </div>
                                </div>
                                <div class="col-md-4">
                                    <div class="item_post" data-aos="fade-up">
                                        <figure>
                                            <a href="https://www.centralcons.vn/wp-content/uploads/2021/12/DSC09768-Retouch-Retouch-1-scaled.jpg"
                                                data-fancybox="galerry-all" data-options='{"caption" : ""}'>
                                                <img src="https://www.centralcons.vn/wp-content/uploads/2021/12/DSC09768-Retouch-Retouch-1-scaled-720x450.jpg"
                                                    class="img-fluid" alt="">
                                            </a>
                                        </figure>
                                    </div>
                                </div>
                                <div class="col-md-4">
                                    <div class="item_post" data-aos="fade-up">
                                        <figure>
                                            <a href="https://www.centralcons.vn/wp-content/uploads/2021/12/DSC09780-Retouch-Retouch-1-scaled.jpg"
                                                data-fancybox="galerry-all" data-options='{"caption" : ""}'>
                                                <img src="https://www.centralcons.vn/wp-content/uploads/2021/12/DSC09780-Retouch-Retouch-1-scaled-720x450.jpg"
                                                    class="img-fluid" alt="">
                                            </a>
                                        </figure>
                                    </div>
                                </div>
                                <div class="col-md-4">
                                    <div class="item_post" data-aos="fade-up">
                                        <figure>
                                            <a href="https://www.centralcons.vn/wp-content/uploads/2021/12/DSC09784-Retouch-Retouch-2-scaled.jpg"
                                                data-fancybox="galerry-all" data-options='{"caption" : ""}'>
                                                <img src="https://www.centralcons.vn/wp-content/uploads/2021/12/DSC09784-Retouch-Retouch-2-scaled-720x450.jpg"
                                                    class="img-fluid" alt="">
                                            </a>
                                        </figure>
                                    </div>
                                </div>
                                <div class="col-md-4">
                                    <div class="item_post" data-aos="fade-up">
                                        <figure>
                                            <a href="https://www.centralcons.vn/wp-content/uploads/2021/12/DSC09787-Retouch-Retouch-1-scaled.jpg"
                                                data-fancybox="galerry-all" data-options='{"caption" : ""}'>
                                                <img src="https://www.centralcons.vn/wp-content/uploads/2021/12/DSC09787-Retouch-Retouch-1-scaled-720x450.jpg"
                                                    class="img-fluid" alt="">
                                            </a>
                                        </figure>
                                    </div>
                                </div>
                                <div class="col-md-4">
                                    <div class="item_post" data-aos="fade-up">
                                        <figure>
                                            <a href="https://www.centralcons.vn/wp-content/uploads/2021/12/DSC09795-Retouch-Retouch-1-scaled.jpg"
                                                data-fancybox="galerry-all" data-options='{"caption" : ""}'>
                                                <img src="https://www.centralcons.vn/wp-content/uploads/2021/12/DSC09795-Retouch-Retouch-1-scaled-720x450.jpg"
                                                    class="img-fluid" alt="">
                                            </a>
                                        </figure>
                                    </div>
                                </div>
                                <div class="col-md-4">
                                    <div class="item_post" data-aos="fade-up">
                                        <figure>
                                            <a href="https://www.centralcons.vn/wp-content/uploads/2021/12/DSC09807-Retouch-Retouch-1-scaled.jpg"
                                                data-fancybox="galerry-all" data-options='{"caption" : ""}'>
                                                <img src="https://www.centralcons.vn/wp-content/uploads/2021/12/DSC09807-Retouch-Retouch-1-scaled-720x450.jpg"
                                                    class="img-fluid" alt="">
                                            </a>
                                        </figure>
                                    </div>
                                </div>
                                <div class="col-md-4">
                                    <div class="item_post" data-aos="fade-up">
                                        <figure>
                                            <a href="https://www.centralcons.vn/wp-content/uploads/2021/12/DSC09824-Retouch-Retouch-1-scaled.jpg"
                                                data-fancybox="galerry-all" data-options='{"caption" : ""}'>
                                                <img src="https://www.centralcons.vn/wp-content/uploads/2021/12/DSC09824-Retouch-Retouch-1-scaled-720x450.jpg"
                                                    class="img-fluid" alt="">
                                            </a>
                                        </figure>
                                    </div>
                                </div>
                                <div class="col-md-4">
                                    <div class="item_post" data-aos="fade-up">
                                        <figure>
                                            <a href="https://www.centralcons.vn/wp-content/uploads/2021/12/DSC09835-Retouch-Retouch-1-scaled.jpg"
                                                data-fancybox="galerry-all" data-options='{"caption" : ""}'>
                                                <img src="https://www.centralcons.vn/wp-content/uploads/2021/12/DSC09835-Retouch-Retouch-1-scaled-720x450.jpg"
                                                    class="img-fluid" alt="">
                                            </a>
                                        </figure>
                                    </div>
                                </div>
                                <div class="col-md-4">
                                    <div class="item_post" data-aos="fade-up">
                                        <figure>
                                            <a href="https://www.centralcons.vn/wp-content/uploads/2021/12/DSC09836-Retouch-Retouch-1-scaled.jpg"
                                                data-fancybox="galerry-all" data-options='{"caption" : ""}'>
                                                <img src="https://www.centralcons.vn/wp-content/uploads/2021/12/DSC09836-Retouch-Retouch-1-scaled-720x450.jpg"
                                                    class="img-fluid" alt="">
                                            </a>
                                        </figure>
                                    </div>
                                </div>
                                <div class="col-md-4">
                                    <div class="item_post" data-aos="fade-up">
                                        <figure>
                                            <a href="https://www.centralcons.vn/wp-content/uploads/2021/12/DSC09846-Retouch-Retouch-1-scaled.jpg"
                                                data-fancybox="galerry-all" data-options='{"caption" : ""}'>
                                                <img src="https://www.centralcons.vn/wp-content/uploads/2021/12/DSC09846-Retouch-Retouch-1-scaled-720x450.jpg"
                                                    class="img-fluid" alt="">
                                            </a>
                                        </figure>
                                    </div>
                                </div>
                                <div class="col-md-4">
                                    <div class="item_post" data-aos="fade-up">
                                        <figure>
                                            <a href="https://www.centralcons.vn/wp-content/uploads/2021/12/DSC09850-Retouch-Retouch-1-scaled.jpg"
                                                data-fancybox="galerry-all" data-options='{"caption" : ""}'>
                                                <img src="https://www.centralcons.vn/wp-content/uploads/2021/12/DSC09850-Retouch-Retouch-1-scaled-720x450.jpg"
                                                    class="img-fluid" alt="">
                                            </a>
                                        </figure>
                                    </div>
                                </div>
                                <div class="col-md-4">
                                    <div class="item_post" data-aos="fade-up">
                                        <figure>
                                            <a href="https://www.centralcons.vn/wp-content/uploads/2021/12/DSC09744-Retouch-Retouch-1-scaled.jpg"
                                                data-fancybox="galerry-all" data-options='{"caption" : ""}'>
                                                <img src="https://www.centralcons.vn/wp-content/uploads/2021/12/DSC09744-Retouch-Retouch-1-scaled-720x450.jpg"
                                                    class="img-fluid" alt="">
                                            </a>
                                        </figure>
                                    </div>
                                </div>
                                <div class="col-md-4">
                                    <div class="item_post" data-aos="fade-up">
                                        <figure>
                                            <a href="https://www.centralcons.vn/wp-content/uploads/2021/12/DSC09845-Retouch-Retouch-1-scaled.jpg"
                                                data-fancybox="galerry-all" data-options='{"caption" : ""}'>
                                                <img src="https://www.centralcons.vn/wp-content/uploads/2021/12/DSC09845-Retouch-Retouch-1-scaled-720x450.jpg"
                                                    class="img-fluid" alt="">
                                            </a>
                                        </figure>
                                    </div>
                                </div>
                                <div class="col-md-4">
                                    <div class="item_post" data-aos="fade-up">
                                        <figure>
                                            <a href="https://www.centralcons.vn/wp-content/uploads/2021/12/DSC09857-Retouch-Retouch-1-scaled.jpg"
                                                data-fancybox="galerry-all" data-options='{"caption" : ""}'>
                                                <img src="https://www.centralcons.vn/wp-content/uploads/2021/12/DSC09857-Retouch-Retouch-1-scaled-720x450.jpg"
                                                    class="img-fluid" alt="">
                                            </a>
                                        </figure>
                                    </div>
                                </div>
                                <div class="col-md-4">
                                    <div class="item_post" data-aos="fade-up">
                                        <figure>
                                            <a href="https://www.centralcons.vn/wp-content/uploads/2021/12/DSC09866-Retouch-Retouch-1-scaled.jpg"
                                                data-fancybox="galerry-all" data-options='{"caption" : ""}'>
                                                <img src="https://www.centralcons.vn/wp-content/uploads/2021/12/DSC09866-Retouch-Retouch-1-scaled-720x450.jpg"
                                                    class="img-fluid" alt="">
                                            </a>
                                        </figure>
                                    </div>
                                </div>
                                <div class="col-md-4">
                                    <div class="item_post" data-aos="fade-up">
                                        <figure>
                                            <a href="https://www.centralcons.vn/wp-content/uploads/2021/12/DSC09874-Retouch-Retouch-1-scaled.jpg"
                                                data-fancybox="galerry-all" data-options='{"caption" : ""}'>
                                                <img src="https://www.centralcons.vn/wp-content/uploads/2021/12/DSC09874-Retouch-Retouch-1-scaled-720x450.jpg"
                                                    class="img-fluid" alt="">
                                            </a>
                                        </figure>
                                    </div>
                                </div>
                                <div class="col-md-4">
                                    <div class="item_post" data-aos="fade-up">
                                        <figure>
                                            <a href="https://www.centralcons.vn/wp-content/uploads/2021/12/DSC09877-Retouch-Retouch-1-scaled.jpg"
                                                data-fancybox="galerry-all" data-options='{"caption" : ""}'>
                                                <img src="https://www.centralcons.vn/wp-content/uploads/2021/12/DSC09877-Retouch-Retouch-1-scaled-720x450.jpg"
                                                    class="img-fluid" alt="">
                                            </a>
                                        </figure>
                                    </div>
                                </div>
                                <div class="col-md-4">
                                    <div class="item_post" data-aos="fade-up">
                                        <figure>
                                            <a href="https://www.centralcons.vn/wp-content/uploads/2021/12/DSC09885-Retouch-Retouch-1-scaled.jpg"
                                                data-fancybox="galerry-all" data-options='{"caption" : ""}'>
                                                <img src="https://www.centralcons.vn/wp-content/uploads/2021/12/DSC09885-Retouch-Retouch-1-scaled-720x450.jpg"
                                                    class="img-fluid" alt="">
                                            </a>
                                        </figure>
                                    </div>
                                </div>
                                <div class="col-md-4">
                                    <div class="item_post" data-aos="fade-up">
                                        <figure>
                                            <a href="https://www.centralcons.vn/wp-content/uploads/2021/12/DSC09891-Retouch-Retouch-1-scaled.jpg"
                                                data-fancybox="galerry-all" data-options='{"caption" : ""}'>
                                                <img src="https://www.centralcons.vn/wp-content/uploads/2021/12/DSC09891-Retouch-Retouch-1-scaled-720x450.jpg"
                                                    class="img-fluid" alt="">
                                            </a>
                                        </figure>
                                    </div>
                                </div>
                                <div class="col-md-4">
                                    <div class="item_post" data-aos="fade-up">
                                        <figure>
                                            <a href="https://www.centralcons.vn/wp-content/uploads/2021/12/DSC09898-Retouch-Retouch-2-scaled.jpg"
                                                data-fancybox="galerry-all" data-options='{"caption" : ""}'>
                                                <img src="https://www.centralcons.vn/wp-content/uploads/2021/12/DSC09898-Retouch-Retouch-2-scaled-720x450.jpg"
                                                    class="img-fluid" alt="">
                                            </a>
                                        </figure>
                                    </div>
                                </div>
                                <div class="col-md-4">
                                    <div class="item_post" data-aos="fade-up">
                                        <figure>
                                            <a href="https://www.centralcons.vn/wp-content/uploads/2021/12/DSC09908-Retouch-Retouch-1-scaled.jpg"
                                                data-fancybox="galerry-all" data-options='{"caption" : ""}'>
                                                <img src="https://www.centralcons.vn/wp-content/uploads/2021/12/DSC09908-Retouch-Retouch-1-scaled-720x450.jpg"
                                                    class="img-fluid" alt="">
                                            </a>
                                        </figure>
                                    </div>
                                </div>
                                <div class="col-md-4">
                                    <div class="item_post" data-aos="fade-up">
                                        <figure>
                                            <a href="https://www.centralcons.vn/wp-content/uploads/2022/03/z2689972641407_3604aa748d54d33f4fea52a2e4f48a85.jpg"
                                                data-fancybox="galerry-all" data-options='{"caption" : ""}'>
                                                <img src="https://www.centralcons.vn/wp-content/uploads/2022/03/z2689972641407_3604aa748d54d33f4fea52a2e4f48a85-720x450.jpg"
                                                    class="img-fluid" alt="">
                                            </a>
                                        </figure>
                                    </div>
                                </div>
                                <div class="col-md-4">
                                    <div class="item_post" data-aos="fade-up">
                                        <figure>
                                            <a href="https://www.centralcons.vn/wp-content/uploads/2022/03/z2709346325160_a0625a98bb36c8191fab47afc8fe52c6.jpg"
                                                data-fancybox="galerry-all" data-options='{"caption" : ""}'>
                                                <img src="https://www.centralcons.vn/wp-content/uploads/2022/03/z2709346325160_a0625a98bb36c8191fab47afc8fe52c6-720x450.jpg"
                                                    class="img-fluid" alt="">
                                            </a>
                                        </figure>
                                    </div>
                                </div>
                                <div class="col-md-4">
                                    <div class="item_post" data-aos="fade-up">
                                        <figure>
                                            <a href="https://www.centralcons.vn/wp-content/uploads/2022/03/z2709346380547_3c471c8272f173b9d45c376af1d8276e.jpg"
                                                data-fancybox="galerry-all" data-options='{"caption" : ""}'>
                                                <img src="https://www.centralcons.vn/wp-content/uploads/2022/03/z2709346380547_3c471c8272f173b9d45c376af1d8276e-720x450.jpg"
                                                    class="img-fluid" alt="">
                                            </a>
                                        </figure>
                                    </div>
                                </div>
                                <div class="col-md-4">
                                    <div class="item_post" data-aos="fade-up">
                                        <figure>
                                            <a href="https://www.centralcons.vn/wp-content/uploads/2022/03/z2711481786400_6a6bb0d2bd278ced0edf00d3c2cbd995.jpg"
                                                data-fancybox="galerry-all" data-options='{"caption" : ""}'>
                                                <img src="https://www.centralcons.vn/wp-content/uploads/2022/03/z2711481786400_6a6bb0d2bd278ced0edf00d3c2cbd995-720x450.jpg"
                                                    class="img-fluid" alt="">
                                            </a>
                                        </figure>
                                    </div>
                                </div>
                                <div class="col-md-4">
                                    <div class="item_post" data-aos="fade-up">
                                        <figure>
                                            <a href="https://www.centralcons.vn/wp-content/uploads/2022/03/Cop-pha-nhom-gangform.jpg"
                                                data-fancybox="galerry-all" data-options='{"caption" : ""}'>
                                                <img src="https://www.centralcons.vn/wp-content/uploads/2022/03/Cop-pha-nhom-gangform-720x450.jpg"
                                                    class="img-fluid" alt="">
                                            </a>
                                        </figure>
                                    </div>
                                </div>
                                <div class="col-md-4">
                                    <div class="item_post" data-aos="fade-up">
                                        <figure>
                                            <a href="https://www.centralcons.vn/wp-content/uploads/2022/03/Cop-pha-nhom-cau-thang-bo.jpg"
                                                data-fancybox="galerry-all" data-options='{"caption" : ""}'>
                                                <img src="https://www.centralcons.vn/wp-content/uploads/2022/03/Cop-pha-nhom-cau-thang-bo-640x450.jpg"
                                                    class="img-fluid" alt="">
                                            </a>
                                        </figure>
                                    </div>
                                </div>
                            </div>
                        </div>
                        <div class="paging_nav mt-4 mb-md-5">
                            <nav aria-label="Page navigation example">
                                <div class='wp-pagenavi' role='navigation'>
                                    <span aria-current='page' class='current'>1</span><a class="page larger"
                                        title="Page 2"
                                        href="https://www.centralcons.vn/hinh-anh-thi-cong/page/2/">2</a><a
                                        class="nextpostslink" rel="next" aria-label="Trang sau"
                                        href="https://www.centralcons.vn/hinh-anh-thi-cong/page/2/">»</a>
                                </div>
                            </nav>
                        </div>
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
                                            class="img-fluid" alt="">
                                    </a>
                                    <img src="https://www.centralcons.vn/wp-content/themes/central-cons/assets/images/mapft_bg.png"
                                        alt="">
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
                                                        alt=""> 204/9 Nguyễn Văn Hưởng, P. Thảo Điền, TP. Thủ Đức, TP.
                                                    Hồ Chí Minh, Việt Nam</li>
                                                <li><img src="https://www.centralcons.vn/wp-content/themes/central-cons/assets/images/phone.svg"
                                                        alt=""> (+84) 28 3620 5151</li>
                                                <li><img src="https://www.centralcons.vn/wp-content/themes/central-cons/assets/images/email.svg"
                                                        alt=""> contact@centralcons.vn</li>
                                            </ul>
                                            <h3 class="fs-16 text-uppercase fw-bold mb-3">Văn phòng Hà Nội</h3>
                                            <ul>
                                                <li><img src="https://www.centralcons.vn/wp-content/themes/central-cons/assets/images/marker.svg"
                                                        alt=""> 6A, No1B, khu đô thị Sài Đồng, Phúc Đồng, Long Biên, Hà
                                                    Nội</li>
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
                                                class="menu-item menu-item-type-post_type menu-item-object-page menu-item-2353">
                                                <a href="https://www.centralcons.vn/du-an/">Dự án</a></li>
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
                                        <a href="#">Terms & Conditions</a>
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
var ajax_object = {"ajaxurl":"https:\/\/www.centralcons.vn\/wp-admin\/admin-ajax.php","link":"https:\/\/www.centralcons.vn\/hinh-anh-thi-cong\/"};
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