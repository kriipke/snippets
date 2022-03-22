# PowerShell Invoke-WebRequest - Parse and scrape a web page | 4sysops
PowerShell’s _Invoke-WebRequest_ is a powerful cmdlet that allows you to download, parse, and scrape web pages.

-   [Author](#abh_about)
-   [Recent Posts](#abh_posts)

[![](https://4sysops.com/wp-content/uploads/avatars/2/85cc6024c6505346362f16e3f5ab4db2-bpfull.png)
](https://4sysops.com/members/michael-pietroforte/ "Michael Pietroforte")

Michael Pietroforte is the founder and editor in chief of 4sysops. He has more than 35 years of experience in IT management and system administration.  
[](https://4sysops.com/archives/results-of-the-4sysops-member-and-author-competition-in-2018/)

[![](https://4sysops.com/wp-content/uploads/avatars/2/85cc6024c6505346362f16e3f5ab4db2-bpfull.png)
](https://4sysops.com/members/michael-pietroforte/ "Michael Pietroforte")

Latest posts by Michael Pietroforte ([see all](https://4sysops.com/members/michael-pietroforte/))

In a previous post, I outlined the options you have to [download files with different Internet protocols](https://4sysops.com/archives/use-powershell-to-download-a-file-with-http-https-and-ftp/). You use _Invoke-WebRequest_ to download files from the web via HTTP and HTTPS. However, the cmdlet enables you to do much more than just download files; you can use it to analyze to contents of web pages and use the information in your scripts.

## The HtmlWebResponseObject object [^](#Content-bal-title "Back to table of contents")

If you pass a URI to _Invoke-WebRequest_, it won’t just display the HTML code of the web page. Instead, it will show you formatted output of various properties of the corresponding web request. For example:

$WebResponse = Invoke-WebRequest  "[http://www.contoso.com](http://www.contoso.com)"

$WebResponse = Invoke-WebRequest "[http://www.contoso.com](http://www.contoso.com)" $WebResponse

    $WebResponse = Invoke-WebRequest "http://www.contoso.com"
    $WebResponse

[![](https://4sysops.com/wp-content/uploads/2015/06/Storing-HtmlWebResponseObject-in-a-variable_thumb.png)
](https://4sysops.com/wp-content/uploads/2015/06/Storing-HtmlWebResponseObject-in-a-variable.png)

_Storing HtmlWebResponseObject in a variable_

Like most cmdlets, _Invoke-WebRequest_ returns an object. If you execute the object’s _GetType_ method, you will learn that the object is of the type _HtmlWebResponseObject_.

    $WebResponse.GetType()

As usual, you can pipe the object to _Get-Member_ to get an overview of the object’s properties:

PS C:>  $WebResponse| Get-Member

TypeName: Microsoft.PowerShell.Commands.HtmlWebResponseObject

Name MemberType Definition

\---- ---------- ----------

Equals Method bool Equals(System.Object obj)

GetHashCode Method int GetHashCode()

GetType Method type GetType()

ToString Method string ToString()

AllElements Property Microsoft.PowerShell.Commands.WebCmdletElementCollection AllElements {get;}

BaseResponse Property System.Net.WebResponse BaseResponse {get;set;}

Content Property string Content {get;}

Forms Property Microsoft.PowerShell.Commands.FormObjectCollection Forms {get;}

Headers Property System.Collections.Generic.Dictionary\[string,string] Headers {get;}

Images Property Microsoft.PowerShell.Commands.WebCmdletElementCollection Images {get;}

InputFields Property Microsoft.PowerShell.Commands.WebCmdletElementCollection InputFields {get;}

Links Property Microsoft.PowerShell.Commands.WebCmdletElementCollection Links {get;}

ParsedHtml Property mshtml.IHTMLDocument2 ParsedHtml {get;}

RawContent Property string RawContent {get;}

RawContentLength Property long RawContentLength {get;}

RawContentStream Property System.IO.MemoryStream RawContentStream {get;}

Scripts Property Microsoft.PowerShell.Commands.WebCmdletElementCollection Scripts {get;}

StatusCode Property int StatusCode {get;}

StatusDescription Property string StatusDescription {get;}

PS C:\\> $WebResponse| Get-Member TypeName: Microsoft.PowerShell.Commands.HtmlWebResponseObject Name MemberType Definition ---- ---------- ---------- Equals Method bool Equals(System.Object obj) GetHashCode Method int GetHashCode() GetType Method type GetType() ToString Method string ToString() AllElements Property Microsoft.PowerShell.Commands.WebCmdletElementCollection AllElements {get;} BaseResponse Property System.Net.WebResponse BaseResponse {get;set;} Content Property string Content {get;} Forms Property Microsoft.PowerShell.Commands.FormObjectCollection Forms {get;} Headers Property System.Collections.Generic.Dictionary\[string,string] Headers {get;} Images Property Microsoft.PowerShell.Commands.WebCmdletElementCollection Images {get;} InputFields Property Microsoft.PowerShell.Commands.WebCmdletElementCollection InputFields {get;} Links Property Microsoft.PowerShell.Commands.WebCmdletElementCollection Links {get;} ParsedHtml Property mshtml.IHTMLDocument2 ParsedHtml {get;} RawContent Property string RawContent {get;} RawContentLength Property long RawContentLength {get;} RawContentStream Property System.IO.MemoryStream RawContentStream {get;} Scripts Property Microsoft.PowerShell.Commands.WebCmdletElementCollection Scripts {get;} StatusCode Property int StatusCode {get;} StatusDescription Property string StatusDescription {get;}

    PS C:\\> $WebResponse| Get-Member


       TypeName: Microsoft.PowerShell.Commands.HtmlWebResponseObject

    Name              MemberType Definition                                                                 
    \-\-\-\-              \-\-\-\-\-\-\-\-\-\- ---------\-                                                                 
    Equals            Method     bool Equals(System.Object obj)                                             
    GetHashCode       Method     int GetHashCode()                                                          
    GetType           Method     type GetType()                                                             
    ToString          Method     string ToString()                                                          
    AllElements       Property   Microsoft.PowerShell.Commands.WebCmdletElementCollection AllElements {get;}
    BaseResponse      Property   System.Net.WebResponse BaseResponse {get;set;}                             
    Content           Property   string Content {get;}                                                      
    Forms             Property   Microsoft.PowerShell.Commands.FormObjectCollection Forms {get;}            
    Headers           Property   System.Collections.Generic.Dictionary\[string,string\] Headers {get;}        
    Images            Property   Microsoft.PowerShell.Commands.WebCmdletElementCollection Images {get;}     
    InputFields       Property   Microsoft.PowerShell.Commands.WebCmdletElementCollection InputFields {get;}
    Links             Property   Microsoft.PowerShell.Commands.WebCmdletElementCollection Links {get;}      
    ParsedHtml        Property   mshtml.IHTMLDocument2 ParsedHtml {get;}                                    
    RawContent        Property   string RawContent {get;}                                                   
    RawContentLength  Property   long RawContentLength {get;}                                               
    RawContentStream  Property   System.IO.MemoryStream RawContentStream {get;}                             
    Scripts           Property   Microsoft.PowerShell.Commands.WebCmdletElementCollection Scripts {get;}    
    StatusCode        Property   int StatusCode {get;}                                                      
    StatusDescription Property   string StatusDescription {get;}

## Parse an HTML page [^](#Content-bal-title "Back to table of contents")

Properties such as _Links_ or _ParsedHtml_ indicate that the main purpose of the cmdlet is to parse web pages. If you just want to access the plain content of the downloaded page, you can do so through the _Content_ property:

    $WebResponse.Content

There also is a _RawContent_ property, which includes the HTTP header fields that the web server returned. Of course, you can also only read the HTTP header fields:

    $WebResponse.Headers

[![](https://4sysops.com/wp-content/uploads/2015/06/Headers-of-a-web-request_thumb.png)
](https://4sysops.com/wp-content/uploads/2015/06/Headers-of-a-web-request.png)

_Headers of a web request_

It may also be useful to have easy access to the HTTP response status codes and their descriptions:

    $WebResponse.StatusCode
    $WebResponse.StatusDescription

The _Links_ property is an array of objects that contain all the hyperlinks in the web page. The most interesting properties of a link object are _innerHTML_, _innerText_, _outerHTML_, and _href_.

The URL that the hyperlink points to is stored in _href_. To get a list of all links in the web page, you could use this command:

$WebResponse.Links | Select href

$WebResponse.Links | Select href

    $WebResponse.Links | Select href

[![](https://4sysops.com/wp-content/uploads/2015/06/Displaying-a-web-pages-links_thumb.png)
](https://4sysops.com/wp-content/uploads/2015/06/Displaying-a-web-pages-links.png)

_Displaying a web page’s links_

_outerHTML_ refers to the entire link as it appears together with the <a> tag: <a href="http://contoso.com">Contoso</a>. Of course, other elements can appear here, such as additional attributes of the <a> element or additional HTML elements after the start tag (<a>), such as image tags. In contrast, the _innerHTML_ property only stores the content between the start tag and the end tag (</a>) together with enclosed additional HTML elements.

The _innerText_ property strips all HTML code from the _innerHTML_ property. You can use this property to read the anchor text of a hyperlink. However, if the additional HTML elements exist inside the <a> element, you will get the text between those tags as well.

Note that the _Link_ object also has an _outerText_ property, but its contents will always be identical to the _innerText_ property if you read a web page. The difference between _outerText_ and _innerText_ only matters if you write HTML code, which we don’t do here.

The _Image_ property can be handled in a similar way as the _Link_ property. It, of course, does not contain the images. Instead, it stores objects with properties that contain HTML code that refers to the images. The most interesting properties are _width_, _height_, _alt_, and _src_. If you know a little HTML, you will know how to deal with these attributes.

The following example downloads all images from a web page:

$WebResponse= Invoke-WebRequest [https://mywebsite.com/page](https://mywebsite.com/page)

ForEach  ($Image  in  $WebResponse.Images)

$FileName = Split-Path  $Image.src -Leaf

Invoke-WebRequest  $Image.src -OutFile $FileName

$WebResponse= Invoke-WebRequest [https://mywebsite.com/page](https://mywebsite.com/page) ForEach ($Image in $WebResponse.Images) { $FileName = Split-Path $Image.src -Leaf Invoke-WebRequest $Image.src -OutFile $FileName }

    $WebResponse= Invoke-WebRequest https://mywebsite.com/page
    ForEach ($Image in $WebResponse.Images)
    {
        $FileName = Split-Path $Image.src -Leaf
        Invoke-WebRequest $Image.src -OutFile $FileName
    }

$WebResponse.Images stores an array of image objects from where we extract the _src_ attribute of the <img> element, which refers to the location of the image. With the help of the _Split-Path_ cmdlet, we get the file name from the URL, which we use to store the image in the current folder.

The properties that you see when you pipe an _HtmlWebResponseObject_ object to _Get-Member_ are those that you need most often when you have to parse an HTML page. If you are looking for other HTML elements, you can use the _AllElements_ and _ParsedHTML_ properties.

_AllElements_ (you guessed it already) contains all the HTML elements that the page contains:

    $WebResponse.AllElements

Of course, this also includes <a> and <img> elements, which means that you can also access them through the _AllElements_ property. For instance, the command below, which displays all the links in a web page, is a bit more longwinded alternative to $WebResponse.links:

$WebResponse.AllElements | Where {$\_.TagName -eq "a"}

$WebResponse.AllElements | Where {$\_.TagName -eq "a"}

    $WebResponse.AllElements | Where {$_.TagName -eq "a"}

_ParsedHTML_ gives you access to the Document Object Model (DOM) of the web page. One difference from _AllElements_ is that _ParsedHTML_ also includes empty attributes of HTML elements. More interesting is that you can easily retrieve additonal information about the web page. For example, the following command tells you when the page was last modified:

$WebResponse.ParsedHtml.IHTMLDocument2_lastModified

$WebResponse.ParsedHtml.IHTMLDocument2_lastModified

    $WebResponse.ParsedHtml.IHTMLDocument2_lastModified

[![](https://4sysops.com/wp-content/uploads/2015/06/Determining-when-a-web-page-was-last-modified_thumb.png)
](https://4sysops.com/wp-content/uploads/2015/06/Determining-when-a-web-page-was-last-modified.png)

_Determining when a web page was last modified_

## Submit an HTML form [^](#Content-bal-title "Back to table of contents")

_Invoke-WebRequest_ also allows you to fill out form fields. Many websites use the HTTP method GET for forms, in which case you simply have to submit a URL that contains the form field entries. If you use a web browser to submit a form, you usually see how the URL is constructed. For instance, the next command searches for PowerShell on 4sysops:

Invoke-WebRequest [https://4sysops.com/index.php?s=powershell](https://4sysops.com/index.php?s=powershell)

Invoke-WebRequest [https://4sysops.com/index.php?s=powershell](https://4sysops.com/index.php?s=powershell)

    Invoke-WebRequest https://4sysops.com/index.php?s=powershell

If the website uses the POST method, things get a bit more complicated. The first thing you have to do is find out which method is used by displaying the forms objects:

$WebResponse = Invoke-WebRequest [https://twitter.com](https://twitter.com)

$WebResponse = Invoke-WebRequest [https://twitter.com](https://twitter.com) $WebResponse.Forms

    $WebResponse = Invoke-WebRequest https://twitter.com
    $WebResponse.Forms

[![](https://4sysops.com/wp-content/uploads/2015/06/Displaying-the-forms-in-a-web-page_thumb.png)
](https://4sysops.com/wp-content/uploads/2015/06/Displaying-the-forms-in-a-web-page.png)

_Displaying the forms in a web page_

A web page sometimes has multiple forms using different methods. Usually you recognize the form you need by inspecting the Fields column. If the column is cut off, you can display all the form fields with this command:

$WebResponse.Forms.Fields

$WebResponse.Forms.Fields

    $WebResponse.Forms.Fields

Let’s have a look at a more concrete example. Our goal is to scrape the country code of a particular IP address from a Whois website. We first have to find out how the form field is structured. Because we are working on the PowerShell console, it is okay to use the alias of _Invoke-WebRequest_:

(wget [https://who.is](https://who.is)).forms

(wget [https://who.is](https://who.is)).forms

    (wget https://who.is).forms

[![](https://4sysops.com/wp-content/uploads/2015/06/Determining-the-form-field-of-a-Whois-website_thumb.png)
](https://4sysops.com/wp-content/uploads/2015/06/Determining-the-form-field-of-a-Whois-website.png)

_Determining the form field of a Whois website_

We see that the website uses the POST method, that the URL to be called to process the query is [https://who.is/domains/search](https://who.is/domains/search), and that two form fields are required. The default value of the Search_type field is “Whois” and the query field is most likely the field for the IP address. We are now ready to scrape the country code of the IP address from the result page:

$Fields = @{"search_type" = "Whois";"query" = "134.170.185.46"}

$WebResponse = Invoke-WebRequest -Uri "[https://who.is/domains/search](https://who.is/domains/search)" -Method Post -Body $Fields

$Pre = $WebResponse.AllElements | Where {$\_.TagName -eq "pre"}

If  ($Pre -match "country:\\s+(\\w{2})")

Write-Host  "Country code:"  $Matches\[1]

$Fields = @{"search_type"="Whois";"query"="134.170.185.46"} $WebResponse = Invoke-WebRequest -Uri"[https://who.is/domains/search](https://who.is/domains/search)" -Method Post -Body $Fields $Pre = $WebResponse.AllElements | Where {$\_.TagName -eq "pre"} If ($Pre -match "country:\\s+(\\w{2})") { Write-Host"Country code:" $Matches\[1] }

    $Fields = @{"search_type" = "Whois";"query" = "134.170.185.46"}
    $WebResponse = Invoke-WebRequest -Uri "https://who.is/domains/search" -Method Post -Body $Fields
    $Pre = $WebResponse.AllElements | Where {$_.TagName -eq "pre"}
    If ($Pre -match "country:\\s+(\\w{2})")
    {
        Write-Host "Country code:" $Matches\[1\]
    }

Update: The example no longer works because the web page uses a different form field now. You can use field variable now:

$Fields = @{"searchString" = "134.170.185.46"}

$Fields = @{"searchString" = "134.170.185.46"}

    $Fields = @{"searchString" = "134.170.185.46"}

In the first line, we define a hash table that contains the names of our two form fields and the values we want to submit. In line 2, we store the result of the request page of the query in a variable. The web page returns the result within a <pre> element, and we extract its content in the next line.

We then use the _-match_ operator with a regular expression to search for the country code. “\\s+" matches any white space character, and “\\w{2}” is supposed to match the country code, which consists of two characters. The parentheses group the country code, which allows us to access the result through the automatic variable _$Matches_.

\+3

[![](https://www.gravatar.com/avatar/a75a0155f4dd8442baf45504d554495a?s=32&r=g&d=mm)
](https://4sysops.com/members/nananarayanan/ "DarkPantheR") 
 [https://4sysops.com/archives/powershell-invoke-webrequest-parse-and-scrape-a-web-page/](https://4sysops.com/archives/powershell-invoke-webrequest-parse-and-scrape-a-web-page/) 
 [https://4sysops.com/archives/powershell-invoke-webrequest-parse-and-scrape-a-web-page/](https://4sysops.com/archives/powershell-invoke-webrequest-parse-and-scrape-a-web-page/)
