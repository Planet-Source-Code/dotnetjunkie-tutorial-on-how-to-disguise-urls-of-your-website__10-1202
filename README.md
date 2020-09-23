<div align="center">

## Tutorial on How To Disguise URLs of your Website


</div>

### Description

The purpose of this article is to show you how to disguise the url of your webpage. This is great for masking your webpage extension to not give away your server and development platform to potential hackers. You can also mask any parameters passed through the query string.<br>Please Vote!
 
### More Info
 


<span>             |<span>
---                |---
**Submitted On**   |
**By**             |[dotNETJunkie](https://github.com/Planet-Source-Code/PSCIndex/blob/master/ByAuthor/dotnetjunkie.md)
**Level**          |Intermediate
**User Rating**    |4.7 (70 globes from 15 users)
**Compatibility**  |VB\.NET, ASP\.NET
**Category**       |[Internet/ Browsers/ HTML](https://github.com/Planet-Source-Code/PSCIndex/blob/master/ByCategory/internet-browsers-html__10-9.md)
**World**          |[\.Net \(C\#, VB\.net\)](https://github.com/Planet-Source-Code/PSCIndex/blob/master/ByWorld/net-c-vb-net.md)
**Archive File**   |[](https://github.com/Planet-Source-Code/dotnetjunkie-tutorial-on-how-to-disguise-urls-of-your-website__10-1202/archive/master.zip)





### Source Code

I uploaded the source code for this, and it is at http://planetsourcecode.com/vb/scripts/ShowCode.asp?txtCodeId=1204&lngWId=10<hr>The method used to change the url is the RewritePath method. Let's begin with the tutorial.
<br>
Create a new webproject.<br>
Add a class to the project.<br>Name it Rewriter<br>Add Imports statements for System and System.Web.<br>
We will write 2 methods(procedures/functions)<br>
The first will be a Private Function returning a String. This function will act as a translator for our URL.<br><br>
Public Function GetSubstitution(ByVal zPath As String) As String<br>
<font color=green>'This first string check is to see if the word test is in the URL<br>'If So, return show.aspx<br>'show.aspx is the actual page<br>and zPath is what is displayed in the browser and passed to the server through links</font><br>
 If InStr(zPath, "test") > 0 Then<br>
  Return "show.aspx"<br>
 End If<br>
<br><font color=green>'This second string validation checks the URL<br>for the extension htm<br>'If it is, replace the extension with aspx</font><br>
 If InStr(zPath, ".htm") > 0 Then<br>
  Dim intMarker As Integer<br>
  Dim strTemp As String<br>
  intMarker = Len(zPath) - 4<br>
  strTemp = Mid(zPath, 1, intMarker)<br>
  Return strTemp & ".aspx"<br>
 End If<br>
<br><font color=green>'The third check will show us how we can pass a query string<br>'</font><br><br>
 If IsNumeric(Left(zPath,Len(zPath)-4)) = True<br> Then<br>
  Dim strValue() As String<br>
  strValue = Left(zPath,Len(zPath)-4)<br>
  Return "show.aspx?ID=" & strValue<br>
 End If<br>
<font color=green>'After all string checks were perforemed<br>'and none of the criteria didn't match<br>'Return the original string</font>
 Return zPath<br>
 End Function<br>
<br>The second will be a Public Sub Procedure and include the Shared statement (this will be called from our web project). <br><br>
Public Shared Sub ReplaceURL()<br>
<font color=green>Create an instance of the class</font><br>
 Dim objRewrite As ReWriter = New urlReWriter()<br>
<font color=green>Create a string variable for our URL substitution</font><br>
 Dim strSubst As String<br>
<font color=green>Set the substitution string to our first function GetSubstitution<br>'We will pass the URL being sent to the browser<br>'through the function to get it's replacement</font><br>
 strSubst = objRewrite.GetSubstitution(HttpContext.Current.Request.Path)<br>
<font color=green>'If the length of our new URL is greater than zero<br>'Rewrite the URL path</font><br>
 If strSubst.Length > 0 Then<br>
  HttpContext.Current.RewritePath(strSubst)<br>
 End If<br>
 End Sub<br>
<br>Now in order to implement this, we need to call our class and put it to work.<br>In the Application_BeginRequest of the Global.asax file, call the ReplaceURL function.<br><br>Sub Application_BeginRequest(ByVal sender As Object, ByVal e As EventArgs)<br>
 ReWriter.ReplaceURL()<br>
 End Sub<br><br>This won't work yet. There's one more thing we have to do. That is tell the webserver how to handle our extensions.<br><br>open the IIS management console.<br>Open the properties of our webproject.<br>Click configuration button on the directory tab.<br>Add new extension<br>In the executable page, enter <b>C:\WINDOWS\Microsoft.NET\Framework\v1.1.4322\aspnet_isapi.dll</b><br>For extensions, enter the extensions you will be using, for this example, lets use <b>.*</b><br>Under Verbs, select Limit to, and enter <b>GET,HEAD,POST</b><br>Uncheck <b><br>Check that file exists<b><br>Click Ok, and apply settings. Now let's test it out.<br>Add a web form to the project. Name it <b>show.aspx</b>, and another named <b>5.aspx<br>In both pages in the page load add the line <b>Response.Write("Page= " & Request.Url.ToString)</b><br><br>Now add an HTML page. Call it what you wish. Add links to <b>5.tst</b> and <b>text.htm</b> and <b>test.zzz</b><br><br>The ReWrite function will be called on request of the pages, and will tell the server which page to load.

