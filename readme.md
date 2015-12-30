# Postman #
Create rich Html emails using Razor Views.

---
Use the Razor Engine that powers MVC views.  Can also be used in non MVC projects simply by adding a reference to ```System.Web.Razor```

**Dependences:**  
1. PreMailer.Net (and CsQuery)  
2. System.Web.Razor

  
**Supported:**   
1. @functions { }   
1. @section <Html|Text> { } 

**Not Supported:**   
1. Layout   
2. @helpers

**Simple Email Template.**  
```html
@{
	From = new System.Net.Mail.MailAddress(Model.From);
	To.Add(Model.To);
	Subject = "Welcome to mysite.com";
}
    
@functions {	// we support functions :)
	static int _counter = 1;
	int GetId() { return _counter++; }
}
    
@section Text {		// This will be the text/plain alternative 
	Dear @Model.Name,
    
	An account has been created for you.
	Your account is FREE and allows you to perform bla bla features.
	To login and complete your profile, please go to:
    
	@Model.LogOnUrl
    
	Remember, we'll never ask for your password.
	Counter: @GetId()
}
    
@section Html {
	<body>
		Dear @Model.Name,<br>
		<p>An account has been created for you.<br />
		Your account is FREE and allows you to perform bla bla features.<br />
		To login and complete your profile, please go to:<br />
		<br />
		<a href="@Model.LogOnUrl">@Model.LogOnUrl</a>
		<br />
		<img src="@EmbedResource("image.jpg")"  /><br />
		<b>Remember, we'll never ask for your password.</b>
		</p>
	</body>
}
```

**Usage:**  
```CSharp
    
var service = new Postman.PostmanService();

var model = new { From = "admin@localhost.com", To = "customer@localhost.com", Name = "Goober Shoes", LogOnUrl = "http://www.website.com/logon/" };
var msg = service.Render("WelcomeMail.cshtml", model);
using (var smtp = new System.Net.Mail.SmtpClient())
{
	smtp.Send(msg);
}
```