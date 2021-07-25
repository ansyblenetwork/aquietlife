---
title: "Experimenting with the Robinhood API"
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
	
	<br>
	<form id="MFAForm" onsubmit="submitMFAForm(); return false;" style="display:none">	
		MFA:<br>
		<input id="loginMFA" type="password" autocomplete="off">		
		<br><br>
		<div style="text-align:left; padding-left:10px">
			<button id="smallloginbut" type="submit">Submit</button>
			<button type="button" style="margin-right:10px; margin-bottom:10px;" onclick="D('MFAForm').reset()">Cancel</button>
    		</div>
	</form> 
	
	<br>
	<div id="profileData" style="display:none">
	<button onclick="getData()">Get Data</button>
	<button onclick="getOptions()">Get Options</button>
	</div>

</div>

<script>/////////////////////////////////////////////////////////

var currentID;
var form = {};
var authData = {};
var authHeader = {};
	
var accountOptions = [];
var accountOptionsData = [];
var optionsBySymbol = {};
	
function D(str) {
	return document.getElementById(str);
}
if (typeof(Storage) !== "undefined") {
	// localStorage.setItem("authString", data.token_type + " " + data.access_token);
	authHeader = {'Authorization': localStorage.getItem("authString") };
	if (localStorage.getItem("authString")) D('profileData').style.display = "initial";
} 	
	
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
	let mytoken = generate_device_token();
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

	fetchData("https://api.robinhood.com/oauth2/token/", 'POST', {form:form}).then(function(data){
		show('MFAForm');
		if (data.challenge) currentID = data.challenge.id;
	});
}
  
function submitMFAForm() {
	fetchData('https://api.robinhood.com/challenge/' + currentID + '/respond/', 'POST', { form:{ 'response': D('loginMFA').value }}).then(function(data){
		fetchData("https://api.robinhood.com/oauth2/token/", 'POST', {form:form, headers:{'X-ROBINHOOD-CHALLENGE-RESPONSE-ID':currentID}}).then(function(data){
			hide('MFAForm');
			show('profileData');
			authData = data;
			authHeader = {'Authorization':data.token_type + " " + data.access_token};
	
			if (typeof(Storage) !== "undefined") {
				localStorage.setItem("authString", data.token_type + " " + data.access_token);
				// authHeader = {'Authorization': localStorage.getItem("authString") };
			} 	
		});
	});
}

function getData() {	
	fetchData("https://api.robinhood.com/accounts/", 'GET', {headers:authHeader}).then(function(data){
		console.log(data);
	});
}
	
	
function getOptions() {	
	fetchData("https://api.robinhood.com/options/positions/?nonzero=True", 'GET', { headers:authHeader }).then(function(data){
		console.log(data);
		accountOptions = data.results;
	
		let promises = [];
		optionsBySymbol = {};
		for (let i = 0; i < accountOptions.length; i++) {			
			optionsBySymbol[accountOptions[i].chain_symbol] = {shortPuts:[],shortCalls:[],longPuts:[],longCalls[]};	
			promises.push(
			fetchData(accountOptions[i].option, 'GET').then(function(optionData) {
				accountOptionsData[i] = optionData;  
			})
			 );
			Promise.all(promises).then(function() {
			  // something here
			  });
		}
	});
}	
	
function fetchData(url, method, data) {
	if (data) {
	return fetch("https://sandboxansyble.herokuapp.com/cors/", 
		{headers: {method: method, url: url, 'json-data': JSON.stringify(data) }}).then(function(response) {
		return response.json();
	});
	} else {
	return fetch("https://sandboxansyble.herokuapp.com/cors/", 
		{headers: {method: method, url: url }}).then(function(response) {
		return response.json();
	});						  
	}
}
	

</script>
