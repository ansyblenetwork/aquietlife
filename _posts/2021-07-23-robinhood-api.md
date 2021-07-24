---
title: "Playing with the Robinhood API"
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
			<button type="button" style="margin-right:10px; margin-bottom:10px;" onclick="D('loginForm').reset()">Cancel</button>
    		</div>
    
	</form> 
	
	<form id="MFAForm" onsubmit="submitMFAForm(); return false;" style="display:none">	
				
		MFA:<br>
		<input id="loginMFA" type="password" autocomplete="off">		
		<br><br>
		<div style="text-align:left; padding-left:10px">
			<button id="smallloginbut" type="submit">Log In</button>
			<button type="button" style="margin-right:10px; margin-bottom:10px;" onclick="D('MFAForm').reset()">Cancel</button>
    		</div>
    
	</form> 
	
	
	<button onclick="getData()">Get Data</button>

</div>

<script>/////////////////////////////////////////////////////////

var currentID;
var form = {};
var authData = {};
var authHeader = {};
	
function generate_device_token() {
    let rands = [];
    for (let i = 0; i < 16; i++) {
        r = Math.random();
        rand = 4294967296.0 * r;
        rands.push((Math.round(rand) >> ((3 & i) << 3)) & 255);
    }

    let hexa = [];
    for (let i = 0; i < 256; i++) {	
	let myhex = (i + 256).toString(16).substring(1);
  //let myhex = (i).toString(16);
        hexa.push(myhex);
    }

    let id = "";
    for (let i = 0; i < 16; i++) {
        id += hexa[rands[i]];

        if ((i == 3) || (i == 5) || (i == 7) || (i == 9))
            id += "-";
    }
   return id;
}

	
  function submitLoginForm() {
	
	console.log(D('loginUsername').value);
	let mytoken = generate_device_token();
	console.log(mytoken);
	
	form = {
        'client_id': 'c82SH0WZOsabOXGP2sxqcj34FxkvfnWRZBKlBjFS',
        'expires_in': 86400,
        'grant_type': 'password',
      'password': D('loginPassword').value,
      'username': D('loginUsername').value,
        'scope': 'internal',
        'challenge_type': "sms",
        'device_token': mytoken
	};
	
	fetch("https://sandboxansyble.herokuapp.com/cors/", 
		{
    	method: 'POST', 
      headers: {
    'Target-URL': "https://api.robinhood.com/oauth2/token/",    
     'json-data': JSON.stringify({form:form}),
        }}).then(function(response) {
		return response.json();
    }).then(function(data){
	show('MFAForm');
	currentID = data.challenge.id;
	console.log(data);
	
	});
  
  }
  
  function submitMFAForm() {
	fetch("https://sandboxansyble.herokuapp.com/cors/", 
		{
    	method: 'POST', 
      headers: {
    'Target-URL': 'https://api.robinhood.com/challenge/' + currentID + '/respond/',
     'json-data': JSON.stringify(
	{  form:{  'response': D('loginMFA').value }	}
	),
        }}).then(function(response) {
		return response.json();
    }).then(function(data){
	console.log(data);
	
	
	fetch("https://sandboxansyble.herokuapp.com/cors/", 
		{
    	method: 'POST', 
      headers: {
    'Target-URL': "https://api.robinhood.com/oauth2/token/",    
     'json-data': JSON.stringify({form:form, headers:{'X-ROBINHOOD-CHALLENGE-RESPONSE-ID':currentID}}),
        }}).then(function(response) {
		return response.json();
    }).then(function(data){
	hide('MFAForm');
	console.log(data);
	authData = data;
	authHeader = {'Authorization':data.token_type + " " + data.access_token};
	});
	
	});
  }

function getData() {
	
	
	fetch("https://sandboxansyble.herokuapp.com/cors/", 
		{
    	method: 'GET', 
      headers: {
    'Target-URL': "https://api.robinhood.com/accounts/",
     'json-data': JSON.stringify({headers:authHeader}),
        }}).then(function(response) {
		return response.json();
    }).then(function(data){
	console.log(data);
	
	});
	
	}
	
</script>
