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
            <link rel="alternate" href="https://www.centralcons.vn/" hreflang="vi" />
            <link rel="alternate" href="https://www.centralcons.vn/en/" hreflang="en" />
            <link rel="alternate" href="https://www.centralcons.vn/ch/" hreflang="zh" />

            <!-- This site is optimized with the Yoast SEO plugin v22.1 - https://yoast.com/wordpress/plugins/seo/ -->
            <title>CÔNG TY CỔ PHẦN XÂY DỰNG CENTRAL</title>
            <meta name="description"
                content="CENTRAL được hình thành và phát triển với khát khao tạo dựng nên những công trình kiến trúc kỳ vĩ có độ bền đến hàng thế kỷ." />
            <link rel="canonical" href="https://www.centralcons.vn/" />
            <meta property="og:locale" content="vi_VN" />
            <meta property="og:locale:alternate" content="en_US" />
            <meta property="og:locale:alternate" content="zh_CN" />
            <meta property="og:type" content="website" />
            <meta property="og:title" content="CÔNG TY CỔ PHẦN XÂY DỰNG CENTRAL" />
            <meta property="og:description"
                content="CENTRAL được hình thành và phát triển với khát khao tạo dựng nên những công trình kiến trúc kỳ vĩ có độ bền đến hàng thế kỷ." />
            <meta property="og:url" content="https://www.centralcons.vn/" />
            <meta property="og:site_name" content="CENTRAL" />
            <meta property="article:modified_time" content="2025-04-05T09:34:11+00:00" />
            <meta property="og:image"
                content="https://www.centralcons.vn/wp-content/uploads/2022/04/3456DB59-8729-4F39-A00A-7AAD586A8085.png" />
            <meta property="og:image:width" content="1609" />
            <meta property="og:image:height" content="1998" />
            <meta property="og:image:type" content="image/png" />
            <meta name="twitter:card" content="summary_large_image" />
            <script type="application/ld+json"
                class="yoast-schema-graph">{"@context":"https://schema.org","@graph":[{"@type":"WebPage","@id":"https://www.centralcons.vn/","url":"https://www.centralcons.vn/","name":"CÔNG TY CỔ PHẦN XÂY DỰNG CENTRAL","isPartOf":{"@id":"https://www.centralcons.vn/#website"},"datePublished":"2021-11-16T01:50:27+00:00","dateModified":"2025-04-05T09:34:11+00:00","description":"CENTRAL được hình thành và phát triển với khát khao tạo dựng nên những công trình kiến trúc kỳ vĩ có độ bền đến hàng thế kỷ.","breadcrumb":{"@id":"https://www.centralcons.vn/#breadcrumb"},"inLanguage":"vi","potentialAction":[{"@type":"ReadAction","target":["https://www.centralcons.vn/"]}]},{"@type":"BreadcrumbList","@id":"https://www.centralcons.vn/#breadcrumb","itemListElement":[{"@type":"ListItem","position":1,"name":"Home"}]},{"@type":"WebSite","@id":"https://www.centralcons.vn/#website","url":"https://www.centralcons.vn/","name":"CENTRAL","description":"","potentialAction":[{"@type":"SearchAction","target":{"@type":"EntryPoint","urlTemplate":"https://www.centralcons.vn/?s={search_term_string}"},"query-input":"required name=search_term_string"}],"inLanguage":"vi"}]}</script>
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
                href="https://www.centralcons.vn/wp-json/wp/v2/pages/8" />
            <link rel="EditURI" type="application/rsd+xml" title="RSD"
                href="https://www.centralcons.vn/xmlrpc.php?rsd" />
            <meta name="generator" content="WordPress 6.7.2" />
            <link rel='shortlink' href='https://www.centralcons.vn/' />
            <link rel="alternate" title="oNhúng (JSON)" type="application/json+oembed"
                href="https://www.centralcons.vn/wp-json/oembed/1.0/embed?url=https%3A%2F%2Fwww.centralcons.vn%2F" />
            <link rel="alternate" title="oNhúng (XML)" type="text/xml+oembed"
                href="https://www.centralcons.vn/wp-json/oembed/1.0/embed?url=https%3A%2F%2Fwww.centralcons.vn%2F&#038;format=xml" />
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
            class="home page-template page-template-front-page page-template-front-page-php page page-id-8 mega-menu-menu-1 unselectable">
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
                <div id="banner_site" class="newslide mb-4" data-aos="fade-up">
                    <div class="item">
                        <img src="https://www.centralcons.vn/wp-content/uploads/2025/02/TAIPEI-WEB-2-scaled.jpg"
                            class="img-fluid" alt="">
                        <div class="meta_info">
                            <div class="container">
                                <div class="meta_inner">
                                    <div class="bg_inner">
                                        <div class="title fs-24 fw-bold">
                                            TAIPEI TWIN TOWERS (ĐÀI LOAN) </div>
                                    </div>
                                </div>
                            </div>
                        </div>
                    </div>
                    <div class="item">
                        <img src="https://www.centralcons.vn/wp-content/uploads/2025/02/Zhongya-Anju-Social-Housing-WWEb-scaled.jpg"
                            class="img-fluid" alt="">
                        <div class="meta_info">
                            <div class="container">
                                <div class="meta_inner">
                                    <div class="bg_inner">
                                        <div class="title fs-24 fw-bold">
                                            ZHONGYA ANJU HOUSING (ĐÀI LOAN) </div>
                                    </div>
                                </div>
                            </div>
                        </div>
                    </div>
                    <div class="item">
                        <img src="https://www.centralcons.vn/wp-content/uploads/2025/02/SUMO-WEB-2-scaled.jpg"
                            class="img-fluid" alt="">
                        <div class="meta_info">
                            <div class="container">
                                <div class="meta_inner">
                                    <div class="bg_inner">
                                        <div class="title fs-24 fw-bold">
                                            SUMOU TOWERS (Ả RẬP XÊ-ÚT) </div>
                                    </div>
                                </div>
                            </div>
                        </div>
                    </div>
                    <div class="item">
                        <img src="https://www.centralcons.vn/wp-content/uploads/2025/02/BANGTONG-Web-1-scaled.jpg"
                            class="img-fluid" alt="">
                        <div class="meta_info">
                            <div class="container">
                                <div class="meta_inner">
                                    <div class="bg_inner">
                                        <div class="title fs-24 fw-bold">
                                            BANGTONG APARTMENT (CAMPUCHIA) </div>
                                    </div>
                                </div>
                            </div>
                        </div>
                    </div>
                    <div class="item">
                        <img src="https://www.centralcons.vn/wp-content/uploads/2025/02/HH1-CAT-BA-WEB-scaled.jpg"
                            class="img-fluid" alt="">
                        <div class="meta_info">
                            <div class="container">
                                <div class="meta_inner">
                                    <div class="bg_inner">
                                        <div class="title fs-24 fw-bold">
                                            TÒA THÁP ĐÔI HH1, THUỘC KHU DU LỊCH – DỊCH VỤ THƯƠNG MẠI VỊNH TRUNG TÂM CÁT
                                            BÀ </div>
                                    </div>
                                </div>
                            </div>
                        </div>
                    </div>
                    <div class="item">
                        <img src="https://www.centralcons.vn/wp-content/uploads/2025/02/BANNER-WEB-2-scaled.jpg"
                            class="img-fluid" alt="">
                        <div class="meta_info">
                            <div class="container">
                                <div class="meta_inner">
                                    <div class="bg_inner">
                                        <div class="title fs-24 fw-bold">
                                            MIDORI THE NEST </div>
                                    </div>
                                </div>
                            </div>
                        </div>
                    </div>
                    <div class="item">
                        <img src="https://www.centralcons.vn/wp-content/uploads/2025/02/Banner-Web-scaled.jpg"
                            class="img-fluid" alt="">
                        <div class="meta_info">
                            <div class="container">
                                <div class="meta_inner">
                                    <div class="bg_inner">
                                        <div class="title fs-24 fw-bold">
                                            “SIÊU THƯỢNG LƯU THẦM LẶNG” MANDARIN ORIENTAL ĐÀ NẴNG </div>
                                    </div>
                                </div>
                            </div>
                        </div>
                    </div>
                    <div class="item">
                        <img src="https://www.centralcons.vn/wp-content/uploads/2025/02/WEB-2-scaled.jpg"
                            class="img-fluid" alt="">
                        <div class="meta_info">
                            <div class="container">
                                <div class="meta_inner">
                                    <div class="bg_inner">
                                        <div class="title fs-24 fw-bold">
                                            THE LEGEND CITY ĐÀ NẴNG </div>
                                    </div>
                                </div>
                            </div>
                        </div>
                    </div>
                    <div class="item">
                        <img src="https://www.centralcons.vn/wp-content/uploads/2024/11/ANPHABE-2024-BANNER-WEB-2-03-scaled.jpg"
                            class="img-fluid" alt="">
                        <div class="meta_info">
                            <div class="container">
                                <div class="meta_inner">
                                    <div class="bg_inner">
                                        <div class="title fs-24 fw-bold">
                                            TOP 3 CÔNG TY CÓ MÔI TRƯỜNG LÀM VIỆC TỐT NHẤT NGÀNH XÂY DỰNG NĂM 2024 </div>
                                    </div>
                                </div>
                            </div>
                        </div>
                    </div>
                    <div class="item">
                        <img src="https://www.centralcons.vn/wp-content/uploads/2024/11/APEA-2024-VN-02-02-scaled.jpg"
                            class="img-fluid" alt="">
                        <div class="meta_info">
                            <div class="container">
                                <div class="meta_inner">
                                    <div class="bg_inner">
                                        <div class="title fs-24 fw-bold">
                                            DOANH NGHIỆP XUẤT SẮC CHÂU Á APEA 2024 </div>
                                    </div>
                                </div>
                            </div>
                        </div>
                    </div>
                    <div class="item">
                        <img src="https://www.centralcons.vn/wp-content/uploads/2024/10/BANNER-WEB-scaled.jpg"
                            class="img-fluid" alt="">
                        <div class="meta_info">
                            <div class="container">
                                <div class="meta_inner">
                                    <div class="bg_inner">
                                        <div class="title fs-24 fw-bold">
                                            VAQUARIUS VĂN GIANG </div>
                                    </div>
                                </div>
                            </div>
                        </div>
                    </div>
                    <div class="item">
                        <img src="https://www.centralcons.vn/wp-content/uploads/2024/10/BANNER-WEB-2-scaled.jpg"
                            class="img-fluid" alt="">
                        <div class="meta_info">
                            <div class="container">
                                <div class="meta_inner">
                                    <div class="bg_inner">
                                        <div class="title fs-24 fw-bold">
                                            CĂN HỘ XANH – SỨC KHỎE ESSENSIA SKY </div>
                                    </div>
                                </div>
                            </div>
                        </div>
                    </div>
                    <div class="item">
                        <img src="https://www.centralcons.vn/wp-content/uploads/2024/06/BANNERR-GIAI-THUONG-WEB-2-scaled.jpg"
                            class="img-fluid" alt="">
                        <div class="meta_info">
                            <div class="container">
                                <div class="meta_inner">
                                    <div class="bg_inner">
                                        <div class="title fs-24 fw-bold">
                                            TOP #10 NHÀ THẦU XÂY DỰNG HÀNG ĐẦU VIỆT NAM 2024 </div>
                                    </div>
                                </div>
                            </div>
                        </div>
                    </div>
                    <div class="item">
                        <img src="https://www.centralcons.vn/wp-content/uploads/2023/09/BANNER-WEBSITE-01-scaled.jpg"
                            class="img-fluid" alt="">
                        <div class="meta_info">
                            <div class="container">
                                <div class="meta_inner">
                                    <div class="bg_inner">
                                        <div class="title fs-24 fw-bold">
                                            TOP #1 GIẢI THƯỞNG XÂY DỰNG THẾ GIỚI BỀN VỮNG </div>
                                    </div>
                                </div>
                            </div>
                        </div>
                    </div>
                    <div class="item">
                        <img src="https://www.centralcons.vn/wp-content/uploads/2023/09/Metropole-Banner-2-scaled.jpg"
                            class="img-fluid" alt="">
                        <div class="meta_info">
                            <div class="container">
                                <div class="meta_inner">
                                    <div class="bg_inner">
                                        <div class="title fs-24 fw-bold">
                                            THE METROPOLE - THE CREST RESIDENCE </div>
                                    </div>
                                </div>
                            </div>
                        </div>
                    </div>
                    <div class="item">
                        <img src="https://www.centralcons.vn/wp-content/uploads/2023/09/Sol-Forest-Banner-1-scaled.jpg"
                            class="img-fluid" alt="">
                        <div class="meta_info">
                            <div class="container">
                                <div class="meta_inner">
                                    <div class="bg_inner">
                                        <div class="title fs-24 fw-bold">
                                            SOL FOREST ECOPARK </div>
                                    </div>
                                </div>
                            </div>
                        </div>
                    </div>
                    <div class="item">
                        <img src="https://www.centralcons.vn/wp-content/uploads/2022/10/edit-BUN02330-BANNER-up-WEB-F-scaled.jpg"
                            class="img-fluid" alt="">
                        <div class="meta_info">
                            <div class="container">
                                <div class="meta_inner">
                                    <div class="bg_inner">
                                        <div class="title fs-24 fw-bold">
                                            TRUNG TÂM PHỨC HỢP ĐIỀU HÀNH AN NINH MẠNG VÀ CÔNG NGHỆ CAO </div>
                                    </div>
                                </div>
                            </div>
                        </div>
                    </div>
                    <div class="item">
                        <img src="https://www.centralcons.vn/wp-content/uploads/2022/12/GLOBAL-banner-web-2022.png"
                            class="img-fluid" alt="">
                        <div class="meta_info">
                            <div class="container">
                                <div class="meta_inner">
                                    <div class="bg_inner">
                                        <div class="title fs-24 fw-bold">
                                            THE GLOBAL CITY </div>
                                    </div>
                                </div>
                            </div>
                        </div>
                    </div>
                    <div class="item">
                        <img src="https://www.centralcons.vn/wp-content/uploads/2022/04/DJI_0664-scaled.jpg"
                            class="img-fluid" alt="">
                        <div class="meta_info">
                            <div class="container">
                                <div class="meta_inner">
                                    <div class="bg_inner">
                                        <div class="title fs-24 fw-bold">
                                            EMPIRE CITY </div>
                                        <div class="description">
                                            Tổng thầu Xây Dựng & MEPF hai khối tháp Cove Residences<br />
                                            Quy mô: 2 tầng hầm, 16 tầng cao<br />
                                            CFA: 48.654,1m2 </div>
                                    </div>
                                </div>
                            </div>
                        </div>
                    </div>
                    <div class="item">
                        <img src="https://www.centralcons.vn/wp-content/uploads/2022/04/DJI_0661-scaled.jpg"
                            class="img-fluid" alt="">
                        <div class="meta_info">
                            <div class="container">
                                <div class="meta_inner">
                                    <div class="bg_inner">
                                        <div class="title fs-24 fw-bold">
                                            THỦY CUNG VINPEARL PHÚ QUỐC </div>
                                        <div class="description">
                                            Nhà thầu chính<br />
                                            Thủy Cung VAP lớn nhất Đông Nam Á </div>
                                    </div>
                                </div>
                            </div>
                        </div>
                    </div>
                    <div class="item">
                        <img src="https://www.centralcons.vn/wp-content/uploads/2021/11/121101678_672984973348701_1338295634941350714_o-scaled.jpeg"
                            class="img-fluid" alt="">
                        <div class="meta_info">
                            <div class="container">
                                <div class="meta_inner">
                                    <div class="bg_inner">
                                        <div class="title fs-24 fw-bold">
                                            GRAND WORLD PHÚ QUỐC </div>
                                        <div class="description">
                                            Nhà thầu chính<br />
                                            Quy mô: Sông Venice Italy<br />
                                            231 căn shophouse, Vinholidays Fiesta Hotel 687 phòng, Vinholidays Infinity
                                            Hotel 971 phòng, Boutique Hotel 198 phòng </div>
                                    </div>
                                </div>
                            </div>
                        </div>
                    </div>
                    <div class="item">
                        <img src="https://www.centralcons.vn/wp-content/uploads/2021/11/191550905_1136453956759999_2538584156741076789_n-e1637983926569-scaled.jpeg"
                            class="img-fluid" alt="">
                        <div class="meta_info">
                            <div class="container">
                                <div class="meta_inner">
                                    <div class="bg_inner">
                                        <div class="title fs-24 fw-bold">
                                            VINPEARL NAM HỘI AN </div>
                                        <div class="description">
                                            Nhà thầu chính<br />
                                            Quy mô: Phố đi bộ & Cổng Bến Cảng 12 Tàu </div>
                                    </div>
                                </div>
                            </div>
                        </div>
                    </div>
                </div>
                <div class="section_key_number py-5">
                    <div class="container-fluid">
                        <h3 class="text-center fs-30 fw-bold mb-3 cl-blue" data-aos="fade-left">TỔNG THẦU XÂY DỰNG &
                            MEPF HÀNG ĐẦU VIỆT NAM</h3>
                        <h4 class="text-center fs-40 mb-md-5 mb-3 fw-bold cl-orange" data-aos="fade-right"><img
                                src="https://www.centralcons.vn/wp-content/themes/central-cons/assets/images/Central_icon-04.png"
                                style="width: 25px; margin-bottom: 5px" alt=""> DOANH NGHIỆP XUẤT SẮC CHÂU Á APEA 3 NĂM
                            LIÊN TIẾP <img
                                src="https://www.centralcons.vn/wp-content/themes/central-cons/assets/images/Central_icon-06.png"
                                style="width: 25px; margin-bottom: 5px" alt=""></h4>
                    </div>
                    <div class="container">
                        <div class="row justify-content-center">
                            <div class="col-md-3">
                                <div class="text-center key_item" data-aos="fade-up" data-aos-delay="0">
                                    <div class="fs-24 fw-bold text-uppercase">
                                        Dự Án thi công </div>
                                    <div class="number_count">
                                        <span class="fs-40 fw-bold cl-orange">130</span> dự án
                                    </div>
                                </div>
                            </div>
                            <div class="col-md-3">
                                <div class="text-center key_item" data-aos="fade-up" data-aos-delay="100">
                                    <div class="fs-24 fw-bold text-uppercase">
                                        Doanh số Backlog </div>
                                    <div class="number_count">
                                        <span class="fs-40 fw-bold cl-orange">16.955</span> tỷ
                                    </div>
                                </div>
                            </div>
                            <div class="col-md-3">
                                <div class="text-center key_item" data-aos="fade-up" data-aos-delay="200">
                                    <div class="fs-24 fw-bold text-uppercase">
                                        Tổng số nhân viên </div>
                                    <div class="number_count">
                                        <span class="fs-40 fw-bold cl-orange">900</span> nhân viên
                                    </div>
                                </div>
                            </div>
                            <div class="col-md-3">
                                <div class="text-center key_item" data-aos="fade-up" data-aos-delay="300">
                                    <div class="fs-24 fw-bold text-uppercase">
                                        Lực lượng thi công </div>
                                    <div class="number_count">
                                        <span class="fs-40 fw-bold cl-orange">10.000</span> nhân công
                                    </div>
                                </div>
                            </div>
                        </div>
                    </div>
                </div>
                <div class="project_section pt-3">
                    <div class="container">
                        <div class="title_project mb-4" data-aos="fade-up">
                            <div class="row align-items-center justify-content-between">
                                <div class="col-auto">
                                    <h3 class="fs-40 fw-bold">Dự án</h3>
                                </div>
                                <div class="col">
                                    <div class="line_img">

                                    </div>
                                </div>
                            </div>
                        </div>
                    </div>
                    <div class="slider_projects">
                        <div class="item">
                            <div class="wrapper">
                                <a href="https://www.centralcons.vn/project/sol-forest-ecopark/"></a>
                                <figure>
                                    <img src="https://www.centralcons.vn/wp-content/uploads/2022/06/c3-scaled.jpg"
                                        class="img-fluid" alt="">
                                </figure>
                                <div class="meta_info">
                                    <h4 class="fs-20 fw-bold text-uppercase">Sol Forest Ecopark</h4>
                                    <div class="detail_info">
                                        <div class="table-responsive">
                                            <table class="table">
                                                <tbody>
                                                    <tr>
                                                        <td>Khách hàng</td>
                                                        <td>Tập Đoàn Ecopark</td>
                                                    </tr>
                                                    <tr>
                                                        <td>Vị trí</td>
                                                        <td>Ecopark, Hưng Yên. Hà Nội</td>
                                                    </tr>
                                                    <tr>
                                                        <td>Phạm vi công việc</td>
                                                        <td>
                                                            Tổng Thầu D&B<br />
                                                            02 tầng hầm + 02 tháp x 41 tầng cao<br />
                                                            CFA: 143.000m2 </td>
                                                    </tr>
                                                </tbody>
                                            </table>
                                        </div>
                                        <a href="http://central.commawebdesign.com/project/the-metropole/"
                                            class="read_more">Đọc tiếp</a>
                                    </div>
                                </div>
                            </div>
                        </div>
                        <div class="item">
                            <div class="wrapper">
                                <a href="https://www.centralcons.vn/project/alma-resort/"></a>
                                <figure>
                                    <img src="https://www.centralcons.vn/wp-content/uploads/2022/07/ALMA-RESORT-CAM-RANH-tam-retouch-dọc.jpg"
                                        class="img-fluid" alt="">
                                </figure>
                                <div class="meta_info">
                                    <h4 class="fs-20 fw-bold text-uppercase">Alma Resort</h4>
                                    <div class="detail_info">
                                        <div class="table-responsive">
                                            <table class="table">
                                                <tbody>
                                                    <tr>
                                                        <td>Khách hàng</td>
                                                        <td> Công ty TNHH Khu Du Lịch Vịnh Thiên Đường</td>
                                                    </tr>
                                                    <tr>
                                                        <td>Vị trí</td>
                                                        <td>Cam Ranh, Khánh Hòa</td>
                                                    </tr>
                                                    <tr>
                                                        <td>Phạm vi công việc</td>
                                                        <td>
                                                            Tổng Thầu<br />
                                                            200 căn biệt thự<br />
                                                            Diện tích đất 30ha </td>
                                                    </tr>
                                                </tbody>
                                            </table>
                                        </div>
                                        <a href="http://central.commawebdesign.com/project/the-metropole/"
                                            class="read_more">Đọc tiếp</a>
                                    </div>
                                </div>
                            </div>
                        </div>
                        <div class="item">
                            <div class="wrapper">
                                <a href="https://www.centralcons.vn/project/red-ruby/"></a>
                                <figure>
                                    <img src="https://www.centralcons.vn/wp-content/uploads/2022/04/prj4.jpg"
                                        class="img-fluid" alt="">
                                </figure>
                                <div class="meta_info">
                                    <h4 class="fs-20 fw-bold text-uppercase">Happy One Central</h4>
                                    <div class="detail_info">
                                        <div class="table-responsive">
                                            <table class="table">
                                                <tbody>
                                                    <tr>
                                                        <td>Khách hàng</td>
                                                        <td>CTCP Tập đoàn địa ốc Vạn Xuân</td>
                                                    </tr>
                                                    <tr>
                                                        <td>Vị trí</td>
                                                        <td>Đại lộ Bình Dương, Thành phố Thủ Dầu Một, Tỉnh Bình Dương
                                                        </td>
                                                    </tr>
                                                    <tr>
                                                        <td>Phạm vi công việc</td>
                                                        <td>
                                                            Tổng Thầu<br />
                                                            03 tầng hầm, 02 tầng mái + 02 tháp x 40 tầng cao<br />
                                                            CFA: 159.982m2 </td>
                                                    </tr>
                                                </tbody>
                                            </table>
                                        </div>
                                        <a href="http://central.commawebdesign.com/project/the-metropole/"
                                            class="read_more">Đọc tiếp</a>
                                    </div>
                                </div>
                            </div>
                        </div>
                        <div class="item">
                            <div class="wrapper">
                                <a href="https://www.centralcons.vn/project/opal-tower/"></a>
                                <figure>
                                    <img src="https://www.centralcons.vn/wp-content/uploads/2022/04/opal-scaled.jpg"
                                        class="img-fluid" alt="">
                                </figure>
                                <div class="meta_info">
                                    <h4 class="fs-20 fw-bold text-uppercase">Opal Tower</h4>
                                    <div class="detail_info">
                                        <div class="table-responsive">
                                            <table class="table">
                                                <tbody>
                                                    <tr>
                                                        <td>Khách hàng</td>
                                                        <td>Vietnam Land SSG</td>
                                                    </tr>
                                                    <tr>
                                                        <td>Vị trí</td>
                                                        <td>92 Nguyễn Hữu Cảnh, Bình Thạnh, TP.HCM</td>
                                                    </tr>
                                                    <tr>
                                                        <td>Phạm vi công việc</td>
                                                        <td>
                                                            Nhà Thầu Chính<br />
                                                            04 tầng hầm + 41 tầng cao<br />
                                                            CFA: 91.922m2 </td>
                                                    </tr>
                                                </tbody>
                                            </table>
                                        </div>
                                        <a href="http://central.commawebdesign.com/project/the-metropole/"
                                            class="read_more">Đọc tiếp</a>
                                    </div>
                                </div>
                            </div>
                        </div>
                        <div class="item">
                            <div class="wrapper">
                                <a href="https://www.centralcons.vn/project/uoa-tower/"></a>
                                <figure>
                                    <img src="https://www.centralcons.vn/wp-content/uploads/2022/04/UOA.jpg"
                                        class="img-fluid" alt="">
                                </figure>
                                <div class="meta_info">
                                    <h4 class="fs-20 fw-bold text-uppercase">UOA Tower</h4>
                                    <div class="detail_info">
                                        <div class="table-responsive">
                                            <table class="table">
                                                <tbody>
                                                    <tr>
                                                        <td>Khách hàng</td>
                                                        <td>Công Ty TNHH UOA Tower<br />
                                                            UOA Group Malaysia</td>
                                                    </tr>
                                                    <tr>
                                                        <td>Vị trí</td>
                                                        <td>Quận 7, TP.HCM</td>
                                                    </tr>
                                                    <tr>
                                                        <td>Phạm vi công việc</td>
                                                        <td>
                                                            Nhà Thầu Chính<br />
                                                            04 tầng hầm + 25 tầng cao<br />
                                                            CFA: 48.645m2 </td>
                                                    </tr>
                                                </tbody>
                                            </table>
                                        </div>
                                        <a href="http://central.commawebdesign.com/project/the-metropole/"
                                            class="read_more">Đọc tiếp</a>
                                    </div>
                                </div>
                            </div>
                        </div>
                        <div class="item">
                            <div class="wrapper">
                                <a href="https://www.centralcons.vn/project/vinhomes-grand-park-the-origami/"></a>
                                <figure>
                                    <img src="https://www.centralcons.vn/wp-content/uploads/2022/04/the-origami-scaled.jpg"
                                        class="img-fluid" alt="">
                                </figure>
                                <div class="meta_info">
                                    <h4 class="fs-20 fw-bold text-uppercase">Vinhomes Grand Park &#8211; The Origami
                                    </h4>
                                    <div class="detail_info">
                                        <div class="table-responsive">
                                            <table class="table">
                                                <tbody>
                                                    <tr>
                                                        <td>Khách hàng</td>
                                                        <td>Vingroup – Mitsubishi – Nomura <br />
                                                        </td>
                                                    </tr>
                                                    <tr>
                                                        <td>Vị trí</td>
                                                        <td>Đường Nguyễn Xiển, Quận 9, TP.HCM</td>
                                                    </tr>
                                                    <tr>
                                                        <td>Phạm vi công việc</td>
                                                        <td>
                                                            Nhà Thầu Chính<br />
                                                            02 tầng hầm + 04 tháp x 21 - 31 tầng cao<br />
                                                            CFA: 221.070m2 </td>
                                                    </tr>
                                                </tbody>
                                            </table>
                                        </div>
                                        <a href="http://central.commawebdesign.com/project/the-metropole/"
                                            class="read_more">Đọc tiếp</a>
                                    </div>
                                </div>
                            </div>
                        </div>
                        <div class="item">
                            <div class="wrapper">
                                <a href="https://www.centralcons.vn/project/grand-world-phu-quoc/"></a>
                                <figure>
                                    <img src="https://www.centralcons.vn/wp-content/uploads/2022/04/ks-pq.png"
                                        class="img-fluid" alt="">
                                </figure>
                                <div class="meta_info">
                                    <h4 class="fs-20 fw-bold text-uppercase">Grand World Phú Quốc</h4>
                                    <div class="detail_info">
                                        <div class="table-responsive">
                                            <table class="table">
                                                <tbody>
                                                    <tr>
                                                        <td>Khách hàng</td>
                                                        <td>Vingroup</td>
                                                    </tr>
                                                    <tr>
                                                        <td>Vị trí</td>
                                                        <td>Bãi Dài, Gành Dầu, Phú Quốc</td>
                                                    </tr>
                                                    <tr>
                                                        <td>Phạm vi công việc</td>
                                                        <td>
                                                            Nhà thầu chính<br />
                                                            231 căn shophouse, Vinholidays Fiesta Hotel 687 phòng,
                                                            Vinholidays Infinity Hotel 971 phòng, Boutique Hotel 198
                                                            phòng </td>
                                                    </tr>
                                                </tbody>
                                            </table>
                                        </div>
                                        <a href="http://central.commawebdesign.com/project/the-metropole/"
                                            class="read_more">Đọc tiếp</a>
                                    </div>
                                </div>
                            </div>
                        </div>
                        <div class="item">
                            <div class="wrapper">
                                <a href="https://www.centralcons.vn/project/tuye%cc%82n-so%cc%9bn/"></a>
                                <figure>
                                    <img src="https://www.centralcons.vn/wp-content/uploads/2022/04/TAKASHI-680X855.jpg"
                                        class="img-fluid" alt="">
                                </figure>
                                <div class="meta_info">
                                    <h4 class="fs-20 fw-bold text-uppercase">Takashi Kỳ Co &#8211; Cổng Torii</h4>
                                    <div class="detail_info">
                                        <div class="table-responsive">
                                            <table class="table">
                                                <tbody>
                                                    <tr>
                                                        <td>Khách hàng</td>
                                                        <td>Tập Đoàn Danh Khôi</td>
                                                    </tr>
                                                    <tr>
                                                        <td>Vị trí</td>
                                                        <td>Khu Đô Thị Sinh Thái Nhơn Hội, TP. Quy Nhơn, Tỉnh Bình Định
                                                        </td>
                                                    </tr>
                                                    <tr>
                                                        <td>Phạm vi công việc</td>
                                                        <td>
                                                            Tổng Thầu D&B<br />
                                                            03 Tòa tháp HH-04, HH-05, HH-06<br />
                                                            Tổng Thầu<br />
                                                            03 Tòa tháp HH-01, HH-02, HH-03<br />
                                                            Cổng Torii và Thư viện trung tâm </td>
                                                    </tr>
                                                </tbody>
                                            </table>
                                        </div>
                                        <a href="http://central.commawebdesign.com/project/the-metropole/"
                                            class="read_more">Đọc tiếp</a>
                                    </div>
                                </div>
                            </div>
                        </div>
                        <div class="item">
                            <div class="wrapper">
                                <a href="https://www.centralcons.vn/project/the-metropole/"></a>
                                <figure>
                                    <img src="https://www.centralcons.vn/wp-content/uploads/2022/04/METROPOLE-680-X-855-scaled.jpg"
                                        class="img-fluid" alt="">
                                </figure>
                                <div class="meta_info">
                                    <h4 class="fs-20 fw-bold text-uppercase">The Metropole &#8211; The Crest Residence
                                    </h4>
                                    <div class="detail_info">
                                        <div class="table-responsive">
                                            <table class="table">
                                                <tbody>
                                                    <tr>
                                                        <td>Khách hàng</td>
                                                        <td>SonKim Land</td>
                                                    </tr>
                                                    <tr>
                                                        <td>Vị trí</td>
                                                        <td>Khu đô thị Thủ Thiêm, Quận 2, TP.HCM</td>
                                                    </tr>
                                                    <tr>
                                                        <td>Phạm vi công việc</td>
                                                        <td>
                                                            Tổng Thầu<br />
                                                            1 Tháp - Căn hộ cao cấp 24 tầng<br />
                                                            2 Tháp - Tòa nhà văn phòng 28 tầng<br />
                                                            CFA: 194.000 m2 </td>
                                                    </tr>
                                                </tbody>
                                            </table>
                                        </div>
                                        <a href="http://central.commawebdesign.com/project/the-metropole/"
                                            class="read_more">Đọc tiếp</a>
                                    </div>
                                </div>
                            </div>
                        </div>
                        <div class="item">
                            <div class="wrapper">
                                <a href="https://www.centralcons.vn/project/masteri-centre-point/"></a>
                                <figure>
                                    <img src="https://www.centralcons.vn/wp-content/uploads/2022/04/MASTERI-680X855.jpg"
                                        class="img-fluid" alt="">
                                </figure>
                                <div class="meta_info">
                                    <h4 class="fs-20 fw-bold text-uppercase">Masteri Centre Point</h4>
                                    <div class="detail_info">
                                        <div class="table-responsive">
                                            <table class="table">
                                                <tbody>
                                                    <tr>
                                                        <td>Khách hàng</td>
                                                        <td>Masterise Homes</td>
                                                    </tr>
                                                    <tr>
                                                        <td>Vị trí</td>
                                                        <td>Vinhomes Grand Park, Quận 9, TP.HCM</td>
                                                    </tr>
                                                    <tr>
                                                        <td>Phạm vi công việc</td>
                                                        <td>
                                                            Tổng Thầu<br />
                                                            05 tháp x 3.000 căn hộ cao 32- 39 tầng<br />
                                                            CFA: 320.000 m2 </td>
                                                    </tr>
                                                </tbody>
                                            </table>
                                        </div>
                                        <a href="http://central.commawebdesign.com/project/the-metropole/"
                                            class="read_more">Đọc tiếp</a>
                                    </div>
                                </div>
                            </div>
                        </div>
                        <div class="item">
                            <div class="wrapper">
                                <a href="https://www.centralcons.vn/project/astral-city-phat-dat/"></a>
                                <figure>
                                    <img src="https://www.centralcons.vn/wp-content/uploads/2022/04/ASTRAL-680X855.jpg"
                                        class="img-fluid" alt="">
                                </figure>
                                <div class="meta_info">
                                    <h4 class="fs-20 fw-bold text-uppercase">Astral City</h4>
                                    <div class="detail_info">
                                        <div class="table-responsive">
                                            <table class="table">
                                                <tbody>
                                                    <tr>
                                                        <td>Khách hàng</td>
                                                        <td>CTCP Phát triển Bất động sản Phát Đạt</td>
                                                    </tr>
                                                    <tr>
                                                        <td>Vị trí</td>
                                                        <td>Đại lộ Bình Dương, Thành phố Thuận An, Tỉnh Bình Dương</td>
                                                    </tr>
                                                    <tr>
                                                        <td>Phạm vi công việc</td>
                                                        <td>
                                                            Tổng Thầu D&B<br />
                                                            03 tầng hầm + 08 tháp x 40 tầng cao<br />
                                                            CFA: 536.000m2 </td>
                                                    </tr>
                                                </tbody>
                                            </table>
                                        </div>
                                        <a href="http://central.commawebdesign.com/project/the-metropole/"
                                            class="read_more">Đọc tiếp</a>
                                    </div>
                                </div>
                            </div>
                        </div>
                        <div class="item">
                            <div class="wrapper">
                                <a href="https://www.centralcons.vn/project/vinhomes-smart-city/"></a>
                                <figure>
                                    <img src="https://www.centralcons.vn/wp-content/uploads/2022/04/Vinhomes-Smart-City.jpg"
                                        class="img-fluid" alt="">
                                </figure>
                                <div class="meta_info">
                                    <h4 class="fs-20 fw-bold text-uppercase">Vinhomes Smart City</h4>
                                    <div class="detail_info">
                                        <div class="table-responsive">
                                            <table class="table">
                                                <tbody>
                                                    <tr>
                                                        <td>Khách hàng</td>
                                                        <td>Vingroup</td>
                                                    </tr>
                                                    <tr>
                                                        <td>Vị trí</td>
                                                        <td>Tây Mỗ, Hà Nội</td>
                                                    </tr>
                                                    <tr>
                                                        <td>Phạm vi công việc</td>
                                                        <td>
                                                            Nhà Thầu Chính<br />
                                                            1 tầng hầm + 3 tháp x 34 tầng cao<br />
                                                            CFA: 152.600m2 </td>
                                                    </tr>
                                                </tbody>
                                            </table>
                                        </div>
                                        <a href="http://central.commawebdesign.com/project/the-metropole/"
                                            class="read_more">Đọc tiếp</a>
                                    </div>
                                </div>
                            </div>
                        </div>
                        <div class="item">
                            <div class="wrapper">
                                <a href="https://www.centralcons.vn/project/nha-may-dien-gio-phu-lac-2/"></a>
                                <figure>
                                    <img src="https://www.centralcons.vn/wp-content/uploads/2022/04/Cot-dien-gio-tai-nha-may-Phu-Lac.jpg"
                                        class="img-fluid" alt="">
                                </figure>
                                <div class="meta_info">
                                    <h4 class="fs-20 fw-bold text-uppercase">Nhà Máy Điện Gió Phú Lạc 2</h4>
                                    <div class="detail_info">
                                        <div class="table-responsive">
                                            <table class="table">
                                                <tbody>
                                                    <tr>
                                                        <td>Khách hàng</td>
                                                        <td>REE Corporation</td>
                                                    </tr>
                                                    <tr>
                                                        <td>Vị trí</td>
                                                        <td>Phú Lạc, Tuy Phong, Bình Thuận</td>
                                                    </tr>
                                                    <tr>
                                                        <td>Phạm vi công việc</td>
                                                        <td>
                                                            Tổng Thầu BOP<br />
                                                            26 MW/Tổng chiều dài cáp 2,8 km </td>
                                                    </tr>
                                                </tbody>
                                            </table>
                                        </div>
                                        <a href="http://central.commawebdesign.com/project/the-metropole/"
                                            class="read_more">Đọc tiếp</a>
                                    </div>
                                </div>
                            </div>
                        </div>
                        <div class="item">
                            <div class="wrapper">
                                <a
                                    href="https://www.centralcons.vn/project/movenpick-the-wonderland-novaworld-ho-tram/"></a>
                                <figure>
                                    <img src="https://www.centralcons.vn/wp-content/uploads/2022/04/khach-san-Wonderland-Novaworld-Ho-Tram-fix.jpg"
                                        class="img-fluid" alt="">
                                </figure>
                                <div class="meta_info">
                                    <h4 class="fs-20 fw-bold text-uppercase">Mövenpick The Wonderland &#8211; Novaworld
                                        Hồ Tràm</h4>
                                    <div class="detail_info">
                                        <div class="table-responsive">
                                            <table class="table">
                                                <tbody>
                                                    <tr>
                                                        <td>Khách hàng</td>
                                                        <td>Novaland</td>
                                                    </tr>
                                                    <tr>
                                                        <td>Vị trí</td>
                                                        <td>Bình Châu, Xuyên Mộc, Bà Rịa - Vũng Tàu</td>
                                                    </tr>
                                                    <tr>
                                                        <td>Phạm vi công việc</td>
                                                        <td>
                                                            Tổng Thầu </td>
                                                    </tr>
                                                </tbody>
                                            </table>
                                        </div>
                                        <a href="http://central.commawebdesign.com/project/the-metropole/"
                                            class="read_more">Đọc tiếp</a>
                                    </div>
                                </div>
                            </div>
                        </div>
                        <div class="item">
                            <div class="wrapper">
                                <a href="https://www.centralcons.vn/project/selavia/"></a>
                                <figure>
                                    <img src="https://www.centralcons.vn/wp-content/uploads/2022/04/selavia.jpg"
                                        class="img-fluid" alt="">
                                </figure>
                                <div class="meta_info">
                                    <h4 class="fs-20 fw-bold text-uppercase">Selavia</h4>
                                    <div class="detail_info">
                                        <div class="table-responsive">
                                            <table class="table">
                                                <tbody>
                                                    <tr>
                                                        <td>Khách hàng</td>
                                                        <td>TTC Group</td>
                                                    </tr>
                                                    <tr>
                                                        <td>Vị trí</td>
                                                        <td>Ấp Suối Lớn, xã Dương Tơ, Phú Quốc</td>
                                                    </tr>
                                                    <tr>
                                                        <td>Phạm vi công việc</td>
                                                        <td>
                                                            Tổng Thầu Design & Build <br />
                                                            99 Shophouse 5 Tầng<br />
                                                            Tổng Thầu<br />
                                                            Thi công cụm Khách sạn </td>
                                                    </tr>
                                                </tbody>
                                            </table>
                                        </div>
                                        <a href="http://central.commawebdesign.com/project/the-metropole/"
                                            class="read_more">Đọc tiếp</a>
                                    </div>
                                </div>
                            </div>
                        </div>
                        <div class="item">
                            <div class="wrapper">
                                <a href="https://www.centralcons.vn/project/venezia-beach-ho-tram/"></a>
                                <figure>
                                    <img src="https://www.centralcons.vn/wp-content/uploads/2022/04/venezia-beach-binh-chau-binh-thuan.jpg"
                                        class="img-fluid" alt="">
                                </figure>
                                <div class="meta_info">
                                    <h4 class="fs-20 fw-bold text-uppercase">Venezia Beach Hồ Tràm</h4>
                                    <div class="detail_info">
                                        <div class="table-responsive">
                                            <table class="table">
                                                <tbody>
                                                    <tr>
                                                        <td>Khách hàng</td>
                                                        <td>Nhà phát triển BĐS Công ty Cổ Phần Hưng Vượng</td>
                                                    </tr>
                                                    <tr>
                                                        <td>Vị trí</td>
                                                        <td>Hồ Tràm, Bình Châu, Bà Rịa - Vũng Tàu</td>
                                                    </tr>
                                                    <tr>
                                                        <td>Phạm vi công việc</td>
                                                        <td>
                                                            Tổng Thầu Design & Build<br />
                                                            3979 căn </td>
                                                    </tr>
                                                </tbody>
                                            </table>
                                        </div>
                                        <a href="http://central.commawebdesign.com/project/the-metropole/"
                                            class="read_more">Đọc tiếp</a>
                                    </div>
                                </div>
                            </div>
                        </div>
                        <div class="item">
                            <div class="wrapper">
                                <a href="https://www.centralcons.vn/project/van-phong-the-gioi-di-dong/"></a>
                                <figure>
                                    <img src="https://www.centralcons.vn/wp-content/uploads/2022/04/prj14.jpg"
                                        class="img-fluid" alt="">
                                </figure>
                                <div class="meta_info">
                                    <h4 class="fs-20 fw-bold text-uppercase">Văn phòng Thế Giới Di Động MWG</h4>
                                    <div class="detail_info">
                                        <div class="table-responsive">
                                            <table class="table">
                                                <tbody>
                                                    <tr>
                                                        <td>Khách hàng</td>
                                                        <td>Công ty TNHH Thế giới Di động (MWG)</td>
                                                    </tr>
                                                    <tr>
                                                        <td>Vị trí</td>
                                                        <td>Khu Công nghệ cao, Quận 9, TP.HCM</td>
                                                    </tr>
                                                    <tr>
                                                        <td>Phạm vi công việc</td>
                                                        <td>
                                                            Nhà Thầu Chính<br />
                                                            02 tầng hầm + 12 tầng cao </td>
                                                    </tr>
                                                </tbody>
                                            </table>
                                        </div>
                                        <a href="http://central.commawebdesign.com/project/the-metropole/"
                                            class="read_more">Đọc tiếp</a>
                                    </div>
                                </div>
                            </div>
                        </div>
                        <div class="item">
                            <div class="wrapper">
                                <a href="https://www.centralcons.vn/project/thao-dien-green/"></a>
                                <figure>
                                    <img src="https://www.centralcons.vn/wp-content/uploads/2022/04/thao-dien-green.jpg"
                                        class="img-fluid" alt="">
                                </figure>
                                <div class="meta_info">
                                    <h4 class="fs-20 fw-bold text-uppercase">Thảo Điền Green</h4>
                                    <div class="detail_info">
                                        <div class="table-responsive">
                                            <table class="table">
                                                <tbody>
                                                    <tr>
                                                        <td>Khách hàng</td>
                                                        <td>SIC Real Estate Investment JSC<br />
                                                            <br />
                                                        </td>
                                                    </tr>
                                                    <tr>
                                                        <td>Vị trí</td>
                                                        <td>192 Nguyễn Văn Hưởng, Phường Thảo Điền, Quận 2, TP.HCM</td>
                                                    </tr>
                                                    <tr>
                                                        <td>Phạm vi công việc</td>
                                                        <td>
                                                            Tổng Thầu<br />
                                                            02 tầng hầm + 25 tầng cao<br />
                                                            CFA: 40.000m2 </td>
                                                    </tr>
                                                </tbody>
                                            </table>
                                        </div>
                                        <a href="http://central.commawebdesign.com/project/the-metropole/"
                                            class="read_more">Đọc tiếp</a>
                                    </div>
                                </div>
                            </div>
                        </div>
                    </div>
                </div>
                <div class="latest_news_section pt-md-5 pt-2">
                    <div class="container">
                        <div class="title_section" data-aos="fade-up">
                            <div class="row align-items-center justify-content-between">
                                <div class="col-md-6 col-auto">
                                    <h3 class="fs-40 mb-0 fw-bold"><span>Tin mới nhất</span></h3>
                                </div>
                                <div class="col-md-6 col-auto">
                                    <div class="text-end">

                                    </div>
                                </div>
                            </div>
                        </div>
                        <div class="post_cff mt-4" data-aos="fade-up">
                            <div class="row">
                                <div class="col-md-12">
                                    <div class="post_item mb-4">
                                        <div class="row gx-2 gx-md-3">
                                            <div class="col-md-2 col-6">
                                                <figure>
                                                    <a
                                                        href="https://www.centralcons.vn/cap-nhat-tien-do-du-an-sieu-thuong-luu-tham-lang-mandarin-oriental-da-nang/">
                                                        <img width="720" height="450"
                                                            src="https://www.centralcons.vn/wp-content/uploads/2025/04/Thumnail.jpg"
                                                            class="img-fluid wp-post-image" alt="" decoding="async"
                                                            fetchpriority="high" /> </a>
                                                </figure>
                                            </div>
                                            <div class="col-md-10 col-6">
                                                <div class="post_info">
                                                    <h4 class="fs-16 fw-bold">
                                                        <a href="https://www.centralcons.vn/cap-nhat-tien-do-du-an-sieu-thuong-luu-tham-lang-mandarin-oriental-da-nang/"
                                                            class="cl-blue">
                                                            CENTRAL CẬP NHẬT TIẾN ĐỘ DỰ ÁN SIÊU THƯỢNG LƯU THẦM LẶNG
                                                            MANDARIN ORIENTAL ĐÀ NẴNG </a>
                                                    </h4>
                                                    <div class="d-none d-md-block">
                                                        <div class="post_cate fw-bold fs-13">
                                                            Tin dự án | 05/04/2025 </div>
                                                        Nép mình bên một trong những bãi biển đẹp nhất hành tinh, khu
                                                        biệt thự biển Mandarin Oriental Đà Nẵng không chỉ là nơi sở hữu
                                                        đất ở vô thời hạn, mà còn là nơi tâm hồn tìm thấy sự...
                                                    </div>
                                                </div>
                                            </div>
                                        </div>
                                    </div>
                                </div>
                                <div class="col-md-12">
                                    <div class="post_item mb-4">
                                        <div class="row gx-2 gx-md-3">
                                            <div class="col-md-2 col-6">
                                                <figure>
                                                    <a
                                                        href="https://www.centralcons.vn/central-ghi-danh-top-10-nha-thau-xay-dung-uy-tin-2025/">
                                                        <img width="720" height="450"
                                                            src="https://www.centralcons.vn/wp-content/uploads/2025/03/THUMNAIL-5.jpg"
                                                            class="img-fluid wp-post-image" alt="" decoding="async" />
                                                    </a>
                                                </figure>
                                            </div>
                                            <div class="col-md-10 col-6">
                                                <div class="post_info">
                                                    <h4 class="fs-16 fw-bold">
                                                        <a href="https://www.centralcons.vn/central-ghi-danh-top-10-nha-thau-xay-dung-uy-tin-2025/"
                                                            class="cl-blue">
                                                            CENTRAL GHI DANH TOP 10 NHÀ THẦU XÂY DỰNG UY TÍN 2025 </a>
                                                    </h4>
                                                    <div class="d-none d-md-block">
                                                        <div class="post_cate fw-bold fs-13">
                                                            Hoạt động nội bộ | 31/03/2025 </div>
                                                        Ngày 31/03/2025, Vietnam Report công bố danh sách Top 10 Nhà
                                                        thầu Xây dựng Uy tín năm 2025, vinh danh Công ty Cổ phần Xây
                                                        dựng Central. Đây không chỉ là một cột mốc đáng tự hào mà còn
                                                        là...
                                                    </div>
                                                </div>
                                            </div>
                                        </div>
                                    </div>
                                </div>
                                <div class="col-md-12">
                                    <div class="post_item mb-4">
                                        <div class="row gx-2 gx-md-3">
                                            <div class="col-md-2 col-6">
                                                <figure>
                                                    <a
                                                        href="https://www.centralcons.vn/central-tiep-tuc-trung-thau-thi-cong-he-thong-co-dien-du-an-sun-symphony-residence-da-nang/">
                                                        <img width="720" height="450"
                                                            src="https://www.centralcons.vn/wp-content/uploads/2025/03/Thumnail-MEP.jpg"
                                                            class="img-fluid wp-post-image" alt="" decoding="async" />
                                                    </a>
                                                </figure>
                                            </div>
                                            <div class="col-md-10 col-6">
                                                <div class="post_info">
                                                    <h4 class="fs-16 fw-bold">
                                                        <a href="https://www.centralcons.vn/central-tiep-tuc-trung-thau-thi-cong-he-thong-co-dien-du-an-sun-symphony-residence-da-nang/"
                                                            class="cl-blue">
                                                            CENTRAL TIẾP TỤC TRÚNG THẦU THI CÔNG HỆ THỐNG CƠ ĐIỆN DỰ ÁN
                                                            SUN SYMPHONY RESIDENCE ĐÀ NẴNG </a>
                                                    </h4>
                                                    <div class="d-none d-md-block">
                                                        <div class="post_cate fw-bold fs-13">
                                                            Tin dự án | 27/03/2025 </div>
                                                        Sau khi hoàn thành xuất sắc công tác thi công tầng hầm vượt tiến
                                                        độ và chính thức khởi công phần thân tòa tháp B1.1 của dự án Sun
                                                        Symphony Residence vào tháng 10/2024, Tổng thầu Central tiếp tục
                                                        khẳng...
                                                    </div>
                                                </div>
                                            </div>
                                        </div>
                                    </div>
                                </div>
                                <div class="col-md-12">
                                    <div class="post_item mb-4">
                                        <div class="row gx-2 gx-md-3">
                                            <div class="col-md-2 col-6">
                                                <figure>
                                                    <a
                                                        href="https://www.centralcons.vn/central-trien-khai-hoat-dong-danh-gia-giam-sat-he-thong-iso-nam-2025/">
                                                        <img width="720" height="450"
                                                            src="https://www.centralcons.vn/wp-content/uploads/2025/03/Thumnail-4.jpg"
                                                            class="img-fluid wp-post-image" alt="" decoding="async" />
                                                    </a>
                                                </figure>
                                            </div>
                                            <div class="col-md-10 col-6">
                                                <div class="post_info">
                                                    <h4 class="fs-16 fw-bold">
                                                        <a href="https://www.centralcons.vn/central-trien-khai-hoat-dong-danh-gia-giam-sat-he-thong-iso-nam-2025/"
                                                            class="cl-blue">
                                                            CENTRAL TRIỂN KHAI HOẠT ĐỘNG ĐÁNH GIÁ GIÁM SÁT HỆ THỐNG ISO
                                                            NĂM 2025 </a>
                                                    </h4>
                                                    <div class="d-none d-md-block">
                                                        <div class="post_cate fw-bold fs-13">
                                                            Hoạt động nội bộ | 13/03/2025 </div>
                                                        Ngày 11/03 và 12/03/2025, Central triển khai đánh giá giám sát
                                                        hệ thống ISO năm 2025 được thực hiện bởi Tổ chức chứng nhận QMS
                                                        Việt Nam. Hoạt động này thể hiện rõ tầm quan trọng trong công
                                                        tác vận...
                                                    </div>
                                                </div>
                                            </div>
                                        </div>
                                    </div>
                                </div>
                                <div class="col-md-12">
                                    <div class="post_item mb-4">
                                        <div class="row gx-2 gx-md-3">
                                            <div class="col-md-2 col-6">
                                                <figure>
                                                    <a
                                                        href="https://www.centralcons.vn/chinh-thuc-khanh-thanh-truong-noi-tru-song-ngu-quoc-te-emasi-plus-waterpoint/">
                                                        <img width="720" height="450"
                                                            src="https://www.centralcons.vn/wp-content/uploads/2025/03/Thumnail-3.jpg"
                                                            class="img-fluid wp-post-image" alt="" decoding="async" />
                                                    </a>
                                                </figure>
                                            </div>
                                            <div class="col-md-10 col-6">
                                                <div class="post_info">
                                                    <h4 class="fs-16 fw-bold">
                                                        <a href="https://www.centralcons.vn/chinh-thuc-khanh-thanh-truong-noi-tru-song-ngu-quoc-te-emasi-plus-waterpoint/"
                                                            class="cl-blue">
                                                            CHÍNH THỨC KHÁNH THÀNH TRƯỜNG NỘI TRÚ SONG NGỮ QUỐC TẾ EMASI
                                                            PLUS WATERPOINT </a>
                                                    </h4>
                                                    <div class="d-none d-md-block">
                                                        <div class="post_cate fw-bold fs-13">
                                                            Tin dự án | 11/03/2025 </div>
                                                        Sáng ngày 07/03/2025, một sự kiện trọng đại đã diễn ra tại đại
                                                        đô thị Waterpoint, Long An: Lễ khánh thành trường Nội trú Song
                                                        ngữ Quốc tế EMASI Plus – Waterpoint Campus. Sự kiện này không
                                                        chỉ đánh dấu...
                                                    </div>
                                                </div>
                                            </div>
                                        </div>
                                    </div>
                                </div>
                                <div class="col-md-12">
                                    <div class="post_item mb-4">
                                        <div class="row gx-2 gx-md-3">
                                            <div class="col-md-2 col-6">
                                                <figure>
                                                    <a
                                                        href="https://www.centralcons.vn/central-to-chuc-workshop-tri-an-ngay-quoc-te-phu-nu-8-3/">
                                                        <img width="720" height="450"
                                                            src="https://www.centralcons.vn/wp-content/uploads/2025/03/THUMNAIL-2.jpg"
                                                            class="img-fluid wp-post-image" alt="" decoding="async" />
                                                    </a>
                                                </figure>
                                            </div>
                                            <div class="col-md-10 col-6">
                                                <div class="post_info">
                                                    <h4 class="fs-16 fw-bold">
                                                        <a href="https://www.centralcons.vn/central-to-chuc-workshop-tri-an-ngay-quoc-te-phu-nu-8-3/"
                                                            class="cl-blue">
                                                            CENTRAL TỔ CHỨC WORKSHOP CHÀO MỪNG NGÀY QUỐC TẾ PHỤ NỮ 8/3
                                                        </a>
                                                    </h4>
                                                    <div class="d-none d-md-block">
                                                        <div class="post_cate fw-bold fs-13">
                                                            Hoạt động nội bộ | 07/03/2025 </div>
                                                        Hòa trong không khí chào mừng ngày Quốc tế Phụ nữ 8/3, Công ty
                                                        Cổ phần Xây dựng Central đã tổ chức buổi lễ dành riêng cho Chị
                                                        Em nhằm tri ân những cống hiến của phái đẹp. Buổi lễ...
                                                    </div>
                                                </div>
                                            </div>
                                        </div>
                                    </div>
                                </div>
                                <div class="col-md-12">
                                    <div class="post_item mb-4">
                                        <div class="row gx-2 gx-md-3">
                                            <div class="col-md-2 col-6">
                                                <figure>
                                                    <a
                                                        href="https://www.centralcons.vn/le-khoi-cong-du-an-midori-park-the-nest/">
                                                        <img width="720" height="450"
                                                            src="https://www.centralcons.vn/wp-content/uploads/2025/03/Thumnail.jpg"
                                                            class="img-fluid wp-post-image" alt="" decoding="async" />
                                                    </a>
                                                </figure>
                                            </div>
                                            <div class="col-md-10 col-6">
                                                <div class="post_info">
                                                    <h4 class="fs-16 fw-bold">
                                                        <a href="https://www.centralcons.vn/le-khoi-cong-du-an-midori-park-the-nest/"
                                                            class="cl-blue">
                                                            LỄ KHỞI CÔNG DỰ ÁN MIDORI PARK The NEST </a>
                                                    </h4>
                                                    <div class="d-none d-md-block">
                                                        <div class="post_cate fw-bold fs-13">
                                                            Tin dự án | 05/03/2025 </div>
                                                        Ngày 05/03/2025, Công ty Cổ phần Xây dựng Central phối hợp cùng
                                                        với Chủ đầu tư Công ty TNHH BECAMEX TOKYU đã tổ chức long trọng
                                                        lễ khởi công dự án Căn hộ cao cấp MIDORI PARK The NEST tại...
                                                    </div>
                                                </div>
                                            </div>
                                        </div>
                                    </div>
                                </div>
                                <div class="col-md-12">
                                    <div class="post_item mb-4">
                                        <div class="row gx-2 gx-md-3">
                                            <div class="col-md-2 col-6">
                                                <figure>
                                                    <a
                                                        href="https://www.centralcons.vn/le-khoi-cong-du-an-benh-vien-mat-troi-phu-quoc/">
                                                        <img width="720" height="450"
                                                            src="https://www.centralcons.vn/wp-content/uploads/2025/03/R1-720x450.jpg"
                                                            class="img-fluid wp-post-image" alt="" decoding="async" />
                                                    </a>
                                                </figure>
                                            </div>
                                            <div class="col-md-10 col-6">
                                                <div class="post_info">
                                                    <h4 class="fs-16 fw-bold">
                                                        <a href="https://www.centralcons.vn/le-khoi-cong-du-an-benh-vien-mat-troi-phu-quoc/"
                                                            class="cl-blue">
                                                            LỄ KHỞI CÔNG DỰ ÁN BỆNH VIỆN MẶT TRỜI PHÚ QUỐC </a>
                                                    </h4>
                                                    <div class="d-none d-md-block">
                                                        <div class="post_cate fw-bold fs-13">
                                                            Tin dự án | 03/03/2025 </div>
                                                        Ngày 03/03/2025, Công ty Cổ phần Xây dựng Central tổ chức lễ
                                                        khởi công dự án Bệnh Viện Mặt Trời Phú Quốc tại Đảo Ngọc, Phú
                                                        Quốc. Buổi lễ có sự tham dự của Ban quản lý dự án cùng...
                                                    </div>
                                                </div>
                                            </div>
                                        </div>
                                    </div>
                                </div>
                            </div>
                        </div>
                        <div class="news_more">
                            <a href="http://www.centralcons.vn/tin-du-an/">Xem tất cả <i
                                    class="fa fa-angle-double-right"></i></a>
                        </div>
                    </div>
                </div>
                <div class="career_section pt-3 mb-5">
                    <div class="container">
                        <div class="row gx-0">
                            <div class="col-md-6">
                                <div class="content_career" data-aos="fade-left">
                                    <h3 class="fs-60 fw-bold">Tuyển dụng</h3>
                                    <div class="content_inner">
                                        <div class="border-left">
                                            Chúng tôi luôn tìm kiếm ứng viên tiềm năng cho sự hợp tác và phát triển bền
                                            vững </div>
                                        <a href="http://www.centralcons.vn/co-hoi-nghe-nghiep/">Ứng tuyển ngay</a>
                                    </div>
                                </div>
                            </div>
                            <div class="col-md-6">
                                <div class="accordion" id="accordioncareer" data-aos="fade-right">
                                    <div class="accordion-item">
                                        <div class="accordion-header" id="heading12777">
                                            <a class="accordion-button " href="javascript:;" data-bs-toggle="collapse"
                                                data-bs-target="#collapse12777" aria-expanded="true"
                                                aria-controls="collapse12777">
                                                <img width="300" height="300"
                                                    src="https://www.centralcons.vn/wp-content/uploads/2021/11/DJI_0682-2-scaled-300x300.jpg"
                                                    class="img-fluid wp-post-image" alt="" decoding="async" /> <span
                                                    class="head_info">
                                                    NHÂN VIÊN HÀNH CHÍNH <span class="job_description">
                                                        <i class="fa fa-star-o"></i> Mô tả công việc </span>
                                                </span>
                                            </a>
                                        </div>
                                        <div id="collapse12777" class="accordion-collapse collapse show"
                                            aria-labelledby="heading12777" data-bs-parent="#accordioncareer">
                                            <div class="accordion-body">
                                                <p>1./ Mô Tả Công Việc: 2./ Tiêu Chuẩn Tuyển Dụng: 3./ Cách Thức Nộp Hồ
                                                    Sơ: <a
                                                        href="https://www.centralcons.vn/career/nhan-vien-hanh-chinh/">Xem
                                                        thêm <i class="fa fa-angle-double-right"></i></a></p>
                                            </div>
                                        </div>
                                    </div>
                                    <div class="accordion-item">
                                        <div class="accordion-header" id="heading12704">
                                            <a class="accordion-button collapsed" href="javascript:;"
                                                data-bs-toggle="collapse" data-bs-target="#collapse12704"
                                                aria-expanded="false" aria-controls="collapse12704">
                                                <img width="300" height="300"
                                                    src="https://www.centralcons.vn/wp-content/uploads/2021/11/DJI_0682-2-scaled-300x300.jpg"
                                                    class="img-fluid wp-post-image" alt="" decoding="async" /> <span
                                                    class="head_info">
                                                    GIÁM SÁT XÂY DỰNG (QA/QC) <span class="job_description">
                                                        <i class="fa fa-star-o"></i> Mô tả công việc </span>
                                                </span>
                                            </a>
                                        </div>
                                        <div id="collapse12704" class="accordion-collapse collapse "
                                            aria-labelledby="heading12704" data-bs-parent="#accordioncareer">
                                            <div class="accordion-body">
                                                <p>Địa điểm làm việc: Đà Nẵng/Hà Nội/Hưng Yên 1. Mô Tả Công Việc: 2.
                                                    Tiêu Chuẩn Tuyển Dụng: 3. Ứng tuyển: <a
                                                        href="https://www.centralcons.vn/career/giam-sat-xay-dung-qa-qc/">Xem
                                                        thêm <i class="fa fa-angle-double-right"></i></a></p>
                                            </div>
                                        </div>
                                    </div>
                                    <div class="accordion-item">
                                        <div class="accordion-header" id="heading12683">
                                            <a class="accordion-button collapsed" href="javascript:;"
                                                data-bs-toggle="collapse" data-bs-target="#collapse12683"
                                                aria-expanded="false" aria-controls="collapse12683">
                                                <img width="300" height="300"
                                                    src="https://www.centralcons.vn/wp-content/uploads/2021/11/DJI_0682-2-scaled-300x300.jpg"
                                                    class="img-fluid wp-post-image" alt="" decoding="async" /> <span
                                                    class="head_info">
                                                    GIÁM SÁT AN TOÀN <span class="job_description">
                                                        <i class="fa fa-star-o"></i> Mô tả công việc </span>
                                                </span>
                                            </a>
                                        </div>
                                        <div id="collapse12683" class="accordion-collapse collapse "
                                            aria-labelledby="heading12683" data-bs-parent="#accordioncareer">
                                            <div class="accordion-body">
                                                <p>Địa điểm làm việc: Bình Dương 1. Mô Tả Công Việc: 2. Tiêu Chuẩn Tuyển
                                                    Dụng: 3. Cách Thức Nộp Hồ Sơ: <a
                                                        href="https://www.centralcons.vn/career/giam-sat-an-toan/">Xem
                                                        thêm <i class="fa fa-angle-double-right"></i></a></p>
                                            </div>
                                        </div>
                                    </div>
                                </div>
                            </div>
                        </div>
                    </div>
                </div>
                <div class="cultural_section">
                    <div class="container">
                        <div class="title_section" data-aos="fade-up">
                            <div class="row align-items-center">
                                <div class="col-md-6 col">
                                    <h3 class="fs-40 mb-0 fw-bold"><span>Văn hóa Central</span></h3>
                                </div>
                                <div class="col-md-6 col-auto">
                                    <div class="text-end">

                                    </div>
                                </div>
                            </div>
                        </div>
                        <div class="video_slider mt-4">
                            <div class="container">
                                <div class="row">
                                    <div class="col-md-4">
                                        <div class="item" data-aos="fade-up" data-aos-delay="0">
                                            <a href="https://www.centralcons.vn/cultural/bai-viet-demo-4/">
                                                <img width="720" height="450"
                                                    src="https://www.centralcons.vn/wp-content/uploads/2022/04/5-NGUYEN-TAC-PHAT-TRIEN-BEN-VUNG-2-041-720x450.jpg"
                                                    class="img-fluid wp-post-image" alt="" decoding="async" /> </a>
                                            <div class="title_video">
                                                5 NGUYÊN TẮC PHÁT TRIỂN BỀN VỮNG CENTRAL </div>
                                        </div>
                                    </div>
                                    <div class="col-md-4">
                                        <div class="item" data-aos="fade-up" data-aos-delay="0">
                                            <a
                                                href="https://www.centralcons.vn/cultural/huong-dan-qui-cach-su-dung-logo/">
                                                <img width="720" height="450"
                                                    src="https://www.centralcons.vn/wp-content/uploads/2023/05/Thumnail-CCVH-27-01-720x450.jpg"
                                                    class="img-fluid wp-post-image" alt="" decoding="async"
                                                    srcset="https://www.centralcons.vn/wp-content/uploads/2023/05/Thumnail-CCVH-27-01-720x450.jpg 720w, https://www.centralcons.vn/wp-content/uploads/2023/05/Thumnail-CCVH-27-01.jpg 1500w"
                                                    sizes="(max-width: 720px) 100vw, 720px" /> </a>
                                            <div class="title_video">
                                                HƯỚNG DẪN QUI CÁCH SỬ DỤNG LOGO </div>
                                        </div>
                                    </div>
                                    <div class="col-md-4">
                                        <div class="item" data-aos="fade-up" data-aos-delay="0">
                                            <a
                                                href="https://www.centralcons.vn/cultural/tinh-than-ho%cc%a3c-ta%cc%a3p-cu%cc%89a-nguoi-central/">
                                                <img width="720" height="450"
                                                    src="https://www.centralcons.vn/wp-content/uploads/2022/06/KY12-TINH-THAN-TU-HOC-CUA-NGUOI-CENTRAL-720x450.gif"
                                                    class="img-fluid wp-post-image" alt="" decoding="async"
                                                    srcset="https://www.centralcons.vn/wp-content/uploads/2022/06/KY12-TINH-THAN-TU-HOC-CUA-NGUOI-CENTRAL-720x450.gif 720w, https://www.centralcons.vn/wp-content/uploads/2022/06/KY12-TINH-THAN-TU-HOC-CUA-NGUOI-CENTRAL-1536x960.gif 1536w"
                                                    sizes="(max-width: 720px) 100vw, 720px" /> </a>
                                            <div class="title_video">
                                                TINH THẦN HỌC TẬP CỦA NGƯỜI CENTRAL </div>
                                        </div>
                                    </div>
                                </div>
                            </div>
                        </div>
                    </div>
                </div>
                <div class="video_section pt-4">
                    <div class="container">
                        <div class="title_section mb-4" data-aos="fade-up">
                            <div class="row align-items-center">
                                <div class="col-md-6 col">
                                    <h3 class="fs-40 mb-0 fw-bold"><span>Videos</span></h3>
                                </div>
                                <div class="col-md-6 col-auto">
                                    <div class="text-end">
                                    </div>
                                </div>
                            </div>
                        </div>
                    </div>
                    <div class="video_slider">
                        <div class="container">
                            <div class="row">
                                <div class="col-md-4">
                                    <div class="item" data-aos="fade-up" data-aos-delay="0">
                                        <a href="https://www.youtube.com/watch?v=Mdqmnv8CyQc" data-fancybox>
                                            <img src="https://www.centralcons.vn/wp-content/uploads/2022/04/video1-720x450.jpg"
                                                class="img-fluid" alt="">
                                            <span class="play_btn">
                                                <i class="fa fa-play"></i>
                                            </span>
                                        </a>
                                        <div class="title_video">
                                            CENTRALNEWS #49 </div>
                                    </div>
                                </div>
                                <div class="col-md-4">
                                    <div class="item" data-aos="fade-up" data-aos-delay="0">
                                        <a href="https://www.youtube.com/watch?v=7kip7Jd8Wn8" data-fancybox>
                                            <img src="https://www.centralcons.vn/wp-content/uploads/2022/04/video2-720x450.jpg"
                                                class="img-fluid" alt="">
                                            <span class="play_btn">
                                                <i class="fa fa-play"></i>
                                            </span>
                                        </a>
                                        <div class="title_video">
                                            DOANH NGHIỆP XUẤT SẮC CHÂU Á TẠI APEA VN 2021 </div>
                                    </div>
                                </div>
                                <div class="col-md-4">
                                    <div class="item" data-aos="fade-up" data-aos-delay="0">
                                        <a href="https://www.youtube.com/watch?v=htANcHr6744" data-fancybox>
                                            <img src="https://www.centralcons.vn/wp-content/uploads/2022/04/video3-720x450.jpg"
                                                class="img-fluid" alt="">
                                            <span class="play_btn">
                                                <i class="fa fa-play"></i>
                                            </span>
                                        </a>
                                        <div class="title_video">
                                            CỦNG CỐ NỘI LỰC, NẮM BẮT THỜI CƠ </div>
                                    </div>
                                </div>
                            </div>
                        </div>
                    </div>
                </div>
                <div class="partner_section mt-5 py-5">
                    <div class="container">
                        <div class="title_partner" data-aos="fade-up">
                            <div class="row align-items-center">
                                <div class="col-auto">
                                    <h3 class="fs-40 mb-0 fw-bold"><span>Khách hàng và đối tác</span></h3>
                                </div>
                                <div class="col">
                                    <div class="line_img">
                                    </div>
                                </div>
                            </div>
                        </div>
                        <div class="partner_slider mt-4" data-aos="fade-up">
                            <div class="item">
                                <img src="https://www.centralcons.vn/wp-content/uploads/2021/11/1200px-Vingroup_logo.svg.png"
                                    class="img-fluid" alt="">
                            </div>
                            <div class="item">
                                <img src="https://www.centralcons.vn/wp-content/uploads/2021/11/1200px-Tokyu_logo.svg.png"
                                    class="img-fluid" alt="">
                            </div>
                            <div class="item">
                                <img src="https://www.centralcons.vn/wp-content/uploads/2021/11/ssg-group-logo-1.png"
                                    class="img-fluid" alt="">
                            </div>
                            <div class="item">
                                <img src="https://www.centralcons.vn/wp-content/uploads/2021/11/empire_city_logo2.png"
                                    class="img-fluid" alt="">
                            </div>
                            <div class="item">
                                <img src="https://www.centralcons.vn/wp-content/uploads/2021/11/logo-chu-dau-tu-van-xuan-land-group-1608694908.7669.png"
                                    class="img-fluid" alt="">
                            </div>
                            <div class="item">
                                <img src="https://www.centralcons.vn/wp-content/uploads/2021/11/logo-masterise-group-1.png"
                                    class="img-fluid" alt="">
                            </div>
                            <div class="item">
                                <img src="https://www.centralcons.vn/wp-content/uploads/2021/11/hoa-lam.png"
                                    class="img-fluid" alt="">
                            </div>
                            <div class="item">
                                <img src="https://www.centralcons.vn/wp-content/uploads/2021/11/ttc-logo-1.png"
                                    class="img-fluid" alt="">
                            </div>
                            <div class="item">
                                <img src="https://www.centralcons.vn/wp-content/uploads/2021/11/son-kim-1.png"
                                    class="img-fluid" alt="">
                            </div>
                            <div class="item">
                                <img src="https://www.centralcons.vn/wp-content/uploads/2022/04/shimz.png"
                                    class="img-fluid" alt="">
                            </div>
                            <div class="item">
                                <img src="https://www.centralcons.vn/wp-content/uploads/2022/04/LOGO-HUNG-VUONG-2.png"
                                    class="img-fluid" alt="">
                            </div>
                            <div class="item">
                                <img src="https://www.centralcons.vn/wp-content/uploads/2022/04/tuner.png"
                                    class="img-fluid" alt="">
                            </div>
                            <div class="item">
                                <img src="https://www.centralcons.vn/wp-content/uploads/2021/11/Phat-Dat_Logo-ngang_new-01.png"
                                    class="img-fluid" alt="">
                            </div>
                            <div class="item">
                                <img src="https://www.centralcons.vn/wp-content/uploads/2021/11/logo-saigon-pearl-1-1.png"
                                    class="img-fluid" alt="">
                            </div>
                            <div class="item">
                                <img src="https://www.centralcons.vn/wp-content/uploads/2022/04/27587.png"
                                    class="img-fluid" alt="">
                            </div>
                            <div class="item">
                                <img src="https://www.centralcons.vn/wp-content/uploads/2021/11/logo-bim-group-2.png"
                                    class="img-fluid" alt="">
                            </div>
                            <div class="item">
                                <img src="https://www.centralcons.vn/wp-content/uploads/2021/11/1584004197668-1583807295821-TTG-Logo-Hires_BW_01.png"
                                    class="img-fluid" alt="">
                            </div>
                            <div class="item">
                                <img src="https://www.centralcons.vn/wp-content/uploads/2021/11/logo-bw-1.png"
                                    class="img-fluid" alt="">
                            </div>
                            <div class="item">
                                <img src="https://www.centralcons.vn/wp-content/uploads/2021/11/Logo_MIK-1.png"
                                    class="img-fluid" alt="">
                            </div>
                            <div class="item">
                                <img src="https://www.centralcons.vn/wp-content/uploads/2021/11/EMASI-logo-6.png"
                                    class="img-fluid" alt="">
                            </div>
                            <div class="item">
                                <img src="https://www.centralcons.vn/wp-content/uploads/2021/11/logo_en-1.png"
                                    class="img-fluid" alt="">
                            </div>
                        </div>
                        <div class="partner_more">
                            <div class="row align-items-center">
                                <div class="col-auto">
                                    <a href="http://www.centralcons.vn/about-central/doi-tac-va-khach-hang/">Xem tất cả
                                        <i class="fa fa-angle-double-right"></i></a>
                                </div>
                                <div class="col">
                                    <div class="line_img"></div>
                                </div>
                            </div>
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
var ajax_object = {"ajaxurl":"https:\/\/www.centralcons.vn\/wp-admin\/admin-ajax.php","link":"https:\/\/www.centralcons.vn\/"};
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
