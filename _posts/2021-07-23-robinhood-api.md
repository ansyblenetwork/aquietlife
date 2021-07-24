---
Title: "Playing with the Robinhood API"
---

A Robinhood login via API.


<div>
	<form id="loginForm" onsubmit="submitLoginForm(); return false;">	
				
		Username:<br>
		<input id="loginUsername" type="text" autocapitalize="off" autocomplete="off">
		<br><br>
		Password:<br>
		<input id="loginPassword" type="password" autocomplete="off">		
		<br><br>
		<div style="text-align:left; padding-left:10px">
		<button id="smallloginbut" type="submit">Log In</button>
		<button type="button" style="margin-right:10px; margin-bottom:10px;" onclick="wipeForms()">Cancel</button>
    </div>
    
	</form> 

</div>

<script>
	
function D(string) { return document.getElementById(string);}
	
  function submitLoginForm() {
	
	console.log(D('loginUsername'));
	fetch("https://sandboxansyble.herokuapp.com/cors/", 
		{
    	method: 'POST', 
      headers: {
    'Target-URL': "https://api.robinhood.com/oauth2/token/",    
     'whole-header': JSON.stringify({
      grant_type: 'password',
      scope: 'internal',
      client_id: 'c82SH0WZOsabOXGP2sxqcj34FxkvfnWRZBKlBjFS',
      expires_in: 86400,
      password: D('loginPassword'),
      username: D('loginUsername'),
      device_token: null
    }),
        }}).then(function(response) {
		return response.text();
    }).then(function(data){console.log(data);});
  
  }
  
</script>
