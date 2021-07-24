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
			<button type="button" style="margin-right:10px; margin-bottom:10px;" onclick="wipeForms()">Cancel</button>
    		</div>
    
	</form> 

</div>

<script>
	
	
function generate_device_token() {
    let rands = [];
    for (let i = 0; i < 16; i++) {
        r = Math.random();
        rand = 4294967296.0 * r;
        rands.push((Math.round(rand) >> ((3 & i) << 3)) & 255);
    }

    let hexa = [];
    for (let i = 0; i < 256; i++) {	
	let myhex = (i).toString(16);
	while (myhex.length < 4) {
		console.log(myhex);
		myhex.unshift("0");
	}
        hexa.push((i).toString(16));
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
	fetch("https://sandboxansyble.herokuapp.com/cors/", 
		{
    	method: 'POST', 
      headers: {
    'Target-URL': "https://api.robinhood.com/oauth2/token/",    
     'whole-header': JSON.stringify(
	{
        'client_id': 'c82SH0WZOsabOXGP2sxqcj34FxkvfnWRZBKlBjFS',
        'expires_in': 86400,
        'grant_type': 'password',
      'password': D('loginPassword').value,
      'username': D('loginUsername').value,
        'scope': 'internal',
        'challenge_type': "sms",
        'device_token': mytoken
	}
	
	),
        }}).then(function(response) {
		return response.text();
    }).then(function(data){console.log(data);});
  
  }
  
</script>
