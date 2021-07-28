---
title: "Options collateral analysis via the Robinhood API"
---

A Robinhood API applet to analyze your options collateral. Login is persistent via local storage.




<div style="margin-top:30px">
	<div id="profileData" style="text-align:center; display:none; padding-top:20px; border-bottom:solid; margin-bottom:40px">
		<div style="text-align:center; padding-bottom:20px">
		<button onclick="getData();">Account Value</button>
		<button id="optionButton" onclick="getData(); getOptions();">Options Collateral</button>
		<button id="optionButton" onclick="logout();">Log Out</button>
		</div>		
		<div style="font-weight:bold; padding-top:20px">Total value: <span id="accountValue">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;</span> &nbsp;&nbsp;&nbsp;&nbsp;&nbsp; Cash available: <span id="cash"></span></div>		
		<div style="padding-top:20px">Incomplete spreads:
		<ul style="overflow:auto; max-height:50vh; list-style-type:none; padding-left:0px;margin-bottom:20px;" id="singles"></ul>
		</div>		
		<div style="padding-top:20px">Incomplete condors:
		<ul style="overflow:auto; max-height:50vh; list-style-type:none; padding-left:0px;margin-bottom:20px;" id="unpaired"></ul>
		</div>		
		<div style="padding-top:20px">Spread buybacks:
		<ul style="overflow:auto; max-height:50vh; list-style-type:none; padding-left:0px;margin-bottom:20px;" id="release"></ul>
		</div>		
		<div style="padding-top:20px">Condor buybacks:
		<ul style="overflow:auto; max-height:50vh; list-style-type:none; padding-left:0px;margin-bottom:20px;" id="condorRelease"></ul>
		</div>				
		<div style="padding-top:20px">Incomplete condor buybacks:
		<ul style="overflow:auto; max-height:50vh; list-style-type:none; padding-left:0px;margin-bottom:20px;" id="ncRelease"></ul>
		</div>		
		<div style="padding-top:20px; padding-bottom:20px;">Near or in the money:
		<ul style="overflow:auto; max-height:50vh; list-style-type:none; padding-left:0px;margin-bottom:20px;" id="itm"></ul>
		</div>		
	</div>
	
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
	
	<form id="MFAForm" onsubmit="submitMFAForm(); return false;" style="padding-top:20px; display:none">	
		MFA:<br>
		<input id="loginMFA" type="password" autocomplete="off">		
		<br><br>
		<div style="text-align:left; padding-left:10px">
			<button id="smallloginbut" type="submit">Submit</button>
			<button type="button" style="margin-right:10px; margin-bottom:10px;" onclick="D('MFAForm').reset()">Cancel</button>
    		</div>
	</form> 
</div>

<script>/////////////////////////////////////////////////////////

var currentID;
var form = {};
var authData = {};
var authHeader = {};
	
var optionsBySymbol = {};
	

	
if (typeof(Storage) !== "undefined") {
	authHeader = {'Authorization': localStorage.getItem("authString") };
	if (localStorage.getItem("authString")) D('profileData').style.display = "block";
} 	
	
function D(str) { return document.getElementById(str); }
	
function logout() {
	D('cash').textContent = "";
	D('accountValue').textContent = "\xa0\xa0\xa0\xa0\xa0";
	wipeDOM();
	
	currentID = "";
	form = {};
	authData = {};
	authHeader = {};	
	optionsBySymbol = {};
	
	hide('profileData');
	
	// if (typeof(Storage) !== "undefined") localStorage.setItem("authString", "");
}
	
function clearDOM(myNode) {
	while (myNode.firstChild) {
		myNode.removeChild(myNode.lastChild);
	}
}
	
function wipeDOM() {	
	clearDOM(D('unpaired'));
	clearDOM(D('release'));
	clearDOM(D('condorRelease'));
	clearDOM(D('ncRelease'));
	clearDOM(D('itm'));
}
	
function addCol(text, width, li) {								       
	let col = make('span');
	col.style.width = width;
	col.style.display = "inline-block";
	col.style.textAlign = "center";
	col.textContent = text;
	li.appendChild(col);  
	return col;
}		
	
function fetchData(url, method, data) {
	if (data) {
	return fetch("https://sandboxansyble.herokuapp.com/cors/", 
		{cache:'no-cache', headers: {method: method, url: url, 'json-data': JSON.stringify(data) }}).then(function(response) {
		return response.json();
	});
	} else {
	return fetch("https://sandboxansyble.herokuapp.com/cors/", 
		{cache:'no-cache', headers: {method: method, url: url }}).then(function(response) {
		return response.json();
	});						  
	}
}
	
function generate_device_token() {
    let rands = [];
    for (let i = 0; i < 16; i++) {
        rands.push((Math.round(4294967296.0 * Math.random()) >> ((3 & i) << 3)) & 255);
    }

    let hexa = [];
    for (let i = 0; i < 256; i++) {	
	let myhex = (i + 256).toString(16).substring(1);
        hexa.push(myhex);
    }

    let id = "";
    for (let i = 0; i < 16; i++) {
        id += hexa[rands[i]];
        if ((i == 3) || (i == 5) || (i == 7) || (i == 9)) id += "-";
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
	
			if (typeof(Storage) !== "undefined") localStorage.setItem("authString", data.token_type + " " + data.access_token);
		});
	});
}

function getData() {	
//	fetchData("https://api.robinhood.com/accounts/", 'GET', {headers:authHeader}).then(function(data) {
//		//D('cash').textContent = "Cash available: $" + parseFloat(data.results[0].cash_available_for_withdrawal).toFixed(2);
//		console.log("Accounts");
//		console.log(data);
//	});
	fetchData("https://api.robinhood.com/portfolios/", 'GET', {headers:authHeader}).then(function(data) {
		let growth = data.results[0].equity - data.results[0].equity_previous_close;
		
		if (growth > 0) D("accountValue").style.color = "#1a8";
		else if (growth < 0) D("accountValue").style.color = "#F00";
		D('accountValue').textContent = "$" + parseFloat(data.results[0].equity).toFixed(2) + " ($" 
			+ parseFloat(growth).toFixed(2) +")";
		D('cash').textContent = "$" + parseFloat(data.results[0].withdrawable_amount).toFixed(2);
		console.log("Portfolios");
		console.log(data);
	});
}	
	
function getOptions() {	
D('optionButton').disabled = true;
fetchData("https://api.robinhood.com/options/positions/?nonzero=True", 'GET', { headers:authHeader }).then(function(data) {
	console.log(data);
	let accountOptions = data.results;

	let promises = [];
	optionsBySymbol = {};
	taskBySymbol = {};
	for (let i = 0; i < accountOptions.length; i++) {			
		if (!optionsBySymbol[accountOptions[i].chain_symbol]) {
			optionsBySymbol[accountOptions[i].chain_symbol] = {shortCall:[], shortPut:[], defCall:[], defPut:[]};
			taskBySymbol[accountOptions[i].chain_symbol] = [];
		}
		
		let task = fetchData(accountOptions[i].option, 'GET').then(function(optionData) {
			let quantity = parseInt(accountOptions[i].quantity);
			for(let j = 0; j < quantity; j++) {
				let dat = {expire:optionData.expiration_date, strike:Math.round(parseFloat(optionData.strike_price)*100)};
				if (accountOptions[i].type == "short") {
					if (optionData.type == "call") optionsBySymbol[optionData.chain_symbol].shortCall.push(dat);
					else optionsBySymbol[optionData.chain_symbol].shortPut.push(dat);
				}
				if (accountOptions[i].type == "long") {
					if (optionData.type == "call") optionsBySymbol[optionData.chain_symbol].defCall.push(dat);
					else optionsBySymbol[optionData.chain_symbol].defPut.push(dat);
				}
			}
		});
		promises.push(task);
		taskBySymbol[accountOptions[i].chain_symbol].push(task);
	}
	
	
	let queryString = "";
	wipeDOM();
	for (let symbol in taskBySymbol) {
		queryString += symbol + ",";
		Promise.all(taskBySymbol[symbol]).then(function() {
			optionsBySymbol[symbol].shortPut.sort(function(b, a) {
				if (a.expire > b.expire) return 1;
				else if (a.expire < b.expire) return -1;
				else if (a.strike > b.strike) return 1;
				else if (a.strike < b.strike) return -1;
				else return 0;					
			});
			optionsBySymbol[symbol].defPut.sort(function(b, a) { 
				// not really needed: the job applicants can come in any order, and find the best job
				// for them. If there is a better job applicant before them, they won't get this job. If the applicant
				// comes later, they will get displaced at that time.
				if (a.expire < b.expire) return 1;
				else if (a.expire > b.expire) return -1;
				else if (a.strike > b.strike) return 1;
				else if (a.strike < b.strike) return -1;
				else return 0;					
			});

			let append = optionsBySymbol[symbol].shortCall.length - optionsBySymbol[symbol].defCall;
			for (let i = 0; i < append; i++) {
				 optionsBySymbol[symbol].defCall.push({expire:"never", strike:0});
			}

			optionsBySymbol[symbol].shortCall.sort(function(b, a) { // needed due to the break in the loop!
				if (a.expire > b.expire) return 1;
				else if (a.expire < b.expire) return -1;
				else if (a.strike < b.strike) return 1;
				else if (a.strike > b.strike) return -1;
				else return 0;					
			});
			optionsBySymbol[symbol].defCall.sort(function(b, a) {
				if (a.expire < b.expire) return 1;
				else if (a.expire > b.expire) return -1;
				else if (a.strike < b.strike) return 1;
				else if (a.strike > b.strike) return -1;
				else return 0;					
			});

			pair(symbol);
			let totalCollateral = condor(symbol, true);
			let totalPutCollateral = noCondorPuts(symbol);
			let totalCallCollateral = noCondorCalls(symbol);

			for (let i = 0; i < optionsBySymbol[symbol].shortPut.length; i++) {
				let me = optionsBySymbol[symbol].shortPut[i];
				let myStrike = me.strike;
				let friend = me.friend;
				if (friend) {
					clearOption(friend);
					clearOption(me);
					me.strike = 0;

					findPutPair(symbol, friend);
					me.closeRelease = totalCollateral - condor(symbol);
					me.ncRelease = totalPutCollateral - noCondorPuts(symbol);

					me.strike = myStrike;
					findPutPair(symbol, null, me);
				} else me.closeRelease = myStrike;
			}

			for (let i = 0; i < optionsBySymbol[symbol].shortCall.length; i++) {
				let me = optionsBySymbol[symbol].shortCall[i];
				let myExpire = me.expire;
				let friend = me.friend;

				clearOption(friend);
				clearOption(me);
				me.expire = "neverplus";
				me.collateral = 0;

				findCallPair(symbol, friend);			
				me.closeRelease = totalCollateral - condor(symbol);
				me.ncRelease = totalCallCollateral - noCondorCalls(symbol);

				me.condorPair = [];
				for (let j = 0; j < optionsBySymbol[symbol].shortPut.length; j++) {
					let mypair = optionsBySymbol[symbol].shortPut[j];
					let myPairStrike = mypair.strike;
					let pairFriend = mypair.friend;
					if (pairFriend) {
						clearOption(pairFriend);
						clearOption(mypair);
						mypair.strike = 0;

						findPutPair(symbol, pairFriend);
						me.condorPair.push({condorPair:mypair, condorRelease:totalCollateral - condor(symbol)});

						mypair.strike = myPairStrike;
						findPutPair(symbol, null, mypair);
					}
				}

				me.expire = myExpire;
				delete me.collateral;
				findCallPair(symbol, null, me);
			}
			
			let d = new Date(Date.now() + 1000*60*60*24*21).toISOString();	
			fetch("https://api.tdameritrade.com/v1/marketdata/chains?apikey=T1V8GYUYK3GKC7HG3L23O9XBJ5OH1C4F&symbol=" 
				+ symbol + "&range=OTM&toDate=" + d).then(function(response) {
				return response.json();
			}).then(function(data) {
			console.log(data);
			if (data.status == "SUCCESS") {
				let myPuts = optionsBySymbol[symbol].shortPut;
				let myCalls = optionsBySymbol[symbol].shortCall;
					
				let myPutBuys = optionsBySymbol[symbol].defPut;
				let myCallBuys = optionsBySymbol[symbol].defCall;
					
				
				myPutBuys.forEach(function(contract) {
					if (!contract.friend) {
						let li = make('li');	
						li.onmouseenter = function() { this.style.fontWeight = "bold"; }
						li.onmouseleave = function() { this.style.fontWeight = ""; }
						li.style.textAlign = "center";	
						addCol(symbol, "100px", li);	
						addCol("Put", "100px", li);
						addCol(contract.expire, "150px", li);
						addCol("$" + contract.strike/100, "100px", li);
						D('singles').appendChild(li);
					}
				});

				myCallBuys.forEach(function(contract) {
					if (!contract.friend) {
						let li = make('li');	
						li.onmouseenter = function() { this.style.fontWeight = "bold"; }
						li.onmouseleave = function() { this.style.fontWeight = ""; }
						li.style.textAlign = "center";	
						addCol(symbol, "100px", li);	
						addCol("Call", "100px", li);
						addCol(contract.expire, "150px", li);
						addCol("$" + contract.strike/100, "100px", li);
						D('singles').appendChild(li);
					}
				});

				for (let contractDate in data.putExpDateMap) {
					let expiration = contractDate.substring(0, 10);

					let bestPut = null;
					let ncPut = null;
					myPuts.forEach(function(contract) {
					if (contract.expire == expiration && data.putExpDateMap[contractDate][(contract.strike/100).toFixed(1).toString()]) {
						contract.premium = data.putExpDateMap[contractDate][(contract.strike/100).toFixed(1).toString()][0].ask;
						let expirationM = data.putExpDateMap[contractDate][(contract.strike/100).toFixed(1).toString()][0].expirationDate;
						contract.rate = 100*100*contract.premium*365*1000*60*60*24/(contract.closeRelease*(1000*60*60*66 + expirationM-Date.now()));
						if ((!bestPut && contract.closeRelease > 0) || (bestPut && contract.rate < bestPut.rate)) bestPut = contract;
						
						contract.ncRate = 100*100*contract.premium*365*1000*60*60*24/(contract.ncRelease*(1000*60*60*66 + expirationM-Date.now()));
						if ((!ncPut && contract.ncRelease > 0) || (ncPut && contract.ncRate < ncPut.ncRate)) ncPut = contract;
					}
					});

					let bestCondor = {call:null, put:null, rate:Infinity};
					let bestCall = null;
					let ncCall = null;
					myCalls.forEach(function(contract) {
					if (contract.expire == expiration && data.callExpDateMap[contractDate][(contract.strike/100).toFixed(1).toString()]) {
						contract.premium = data.callExpDateMap[contractDate][(contract.strike/100).toFixed(1).toString()][0].ask;
						let expirationM = data.callExpDateMap[contractDate][(contract.strike/100).toFixed(1).toString()][0].expirationDate;
						contract.rate = 100*100*contract.premium*365*1000*60*60*24/(contract.closeRelease*(1000*60*60*66 + expirationM-Date.now()));
						if ((!bestCall && contract.closeRelease > 0) || (bestCall && contract.rate < bestCall.rate)) bestCall = contract;
						
						contract.ncRate = 100*100*contract.premium*365*1000*60*60*24/(contract.ncRelease*(1000*60*60*66 + expirationM-Date.now()));
						if ((!ncCall && contract.ncRelease > 0) || (ncCall && contract.ncRate < ncCall.ncRate)) ncCall = contract;

						contract.condorPair.forEach(function(pair) {
							let contract2 = pair.condorPair;
						if (contract2.expire == expiration && data.putExpDateMap[contractDate][(contract2.strike/100).toFixed(1).toString()]) {
							contract2.premium = data.putExpDateMap[contractDate][(contract2.strike/100).toFixed(1).toString()][0].ask;
							let minExp = data.putExpDateMap[contractDate][(contract2.strike/100).toFixed(1).toString()][0].expirationDate;
							if (expirationM < minExp) minExp = expirationM
							let condorRate = 100*100*(contract2.premium + contract.premium)*365*1000*60*60*24/(pair.condorRelease*(1000*60*60*66 + minExp-Date.now()));
							if (condorRate < bestCondor.rate) {
								bestCondor = {call:contract, put:contract2, premium:(contract2.premium + contract.premium), rate:condorRate, release:pair.condorRelease};
							}
						}
						});

					}
					});
					
					function addColStock(li, symbol, type, obj) {
						li.onmouseenter = function() { this.style.fontWeight = "bold"; }
						li.onmouseleave = function() { this.style.fontWeight = ""; }
						li.style.textAlign = "center";	
						addCol(symbol, "75px", li);	
						addCol(type, "75px", li);
						addCol(obj.expire, "125px", li);
						addCol("$" + obj.strike/100, "75px", li);
					}
					
					function addOrdered(li, element) {
						let toAdd = true;
						for (let j = 0; j < element.children.length; j++) {
							if (parseFloat(li.title) < parseFloat(element.children[j].title)) {
								element.insertBefore(li, element.children[j]);
								toAdd = false;
								break;
							}
						}
						if (toAdd) element.appendChild(li);
					}
					
					function addColCost(li, elt, rate, collateral) {
						li.title = rate;
						let col = addCol(elt.premium.toFixed(2), "75px", li);
						col.style.backgroundColor = "#ddf";
						col = addCol("$" + collateral, "75px", li);
						col.style.backgroundColor = "#ddf";
						col = addCol(Math.round(rate) + "%", "75px", li);
						col.style.backgroundColor = "#ddf";
					}
					
					if (ncPut) {						
						let li = make('li');	
						addColStock(li, symbol, "Put", ncPut);
						addColCost(li, ncPut, ncPut.ncRate, ncPut.ncRelease);
						addOrdered(li, D('ncRelease'));
					}
					if (bestPut) {						
						let li = make('li');	
						addColStock(li, symbol, "Put", bestPut);
						addColCost(li, bestPut, bestPut.rate, bestPut.closeRelease);
						addOrdered(li, D('release'));
					}
					if (ncCall) {					
						let li = make('li');
						addColStock(li, symbol, "Call", ncCall);
						addColCost(li, ncCall, ncCall.ncRate, ncCall.ncRelease);
						addOrdered(li, D('ncRelease'));
					}
					if (bestCall) {					
						let li = make('li');	
						addColStock(li, symbol, "Call", bestCall);
						addColCost(li, bestCall, bestCall.rate, bestCall.closeRelease);
						addOrdered(li, D('release'));
					}
					if (bestCondor.release) {
						let li = make('li');
						addColStock(li, symbol, "Call", bestCondor.call);

						addCol("Put", "75px", li);
						addCol(bestCondor.put.expire, "125px", li);
						addCol("$" + bestCondor.put.strike/100, "75px", li);

						addColCost(li, bestCondor, bestCondor.rate, bestCondor.release);					
						addOrdered(li, D('condorRelease'));
					}
				}
			} 			       	       
			});
			
		});
	}
	
	Promise.all(promises).then(function() {
		D('optionButton').disabled = false;
		fetchData('https://query1.finance.yahoo.com/v7/finance/quote?symbols=' + queryString, 'GET').then(function(data) {
		for (let i = 0; i < data.quoteResponse.result.length; i++) {
			let myPuts = optionsBySymbol[data.quoteResponse.result[i].symbol].shortPut;
			let myCalls = optionsBySymbol[data.quoteResponse.result[i].symbol].shortCall;

			myPuts.forEach(function(contract) {
				if (contract.strike > 100*0.97*data.quoteResponse.result[i].regularMarketPrice) {
					let li = make('li');	
					li.onmouseenter = function() { this.style.fontWeight = "bold"; }
					li.onmouseleave = function() { this.style.fontWeight = ""; }
					li.style.textAlign = "center";	
					addCol(data.quoteResponse.result[i].symbol, "100px", li);	
					addCol("Put", "100px", li);
					addCol(contract.expire, "150px", li);
					addCol("$" + contract.strike/100, "100px", li);
					D('itm').appendChild(li);
				}
			});

			myCalls.forEach(function(contract) {
				if (contract.strike < 100*1.03*data.quoteResponse.result[i].regularMarketPrice) {
					let li = make('li');	
					li.onmouseenter = function() { this.style.fontWeight = "bold"; }
					li.onmouseleave = function() { this.style.fontWeight = ""; }
					li.style.textAlign = "center";	
					addCol(data.quoteResponse.result[i].symbol, "100px", li);	
					addCol("Call", "100px", li);
					addCol(contract.expire, "150px", li);
					addCol("$" + contract.strike/100, "100px", li);
					D('itm').appendChild(li);
				}
			});
		}
		});
	});
});
}	
	
function pair(symbol) {
	for (let i = 0; i < optionsBySymbol[symbol].shortPut.length; i++) {
		findPutPair(symbol, null, optionsBySymbol[symbol].shortPut[i]);
	}

	for (let i = 0; i < optionsBySymbol[symbol].shortCall.length; i++) {
		findCallPair(symbol, null, optionsBySymbol[symbol].shortCall[i]);
	}
}
	
function findCallPair(symbol, defCall, shortCall) {	
	let data = optionsBySymbol[symbol];
	let maxGain = 0;
	let collateral = Infinity;
	let candidate = null;
	let seeker = defCall;
	if (!seeker) seeker = shortCall;
	if (defCall) {
		for (let j = 0; j < data.shortCall.length; j++) {
			if (evaluate(defCall, data.shortCall[j], data.shortCall[j])) break;
		}
	}
	if (shortCall) {
		for (let j = 0; j < data.defCall.length; j++) {
			if (evaluate(data.defCall[j], shortCall, data.defCall[j])) break;
		}
	}
	
	function evaluate(defCall, shortCall, subject) {
		if (defCall.expire >= shortCall.expire 
		    && (callGain(defCall, shortCall, subject) > maxGain || (maxGain == Infinity && callCollateral(defCall, shortCall) < collateral ))) {
			maxGain = callGain(defCall, shortCall, subject);
			collateral = callCollateral(defCall, shortCall);
			candidate = subject;
			if (callGain(defCall, shortCall, subject) == Infinity) return true;
			if (candidate.friend) return true;
		}
		return false;
	}
	
	if (candidate) {
		let orphan = candidate.friend;
		if (orphan) clearOption(orphan);
		
		candidate.friend = seeker;
		seeker.friend = candidate;
		candidate.collateral = collateral;
		seeker.collateral = collateral;

		if (orphan) {
			if (defCall) findCallPair(symbol, orphan);
			else if (shortCall) findCallPair(symbol, null, orphan);
		}
	}	

	function callCollateral(defCall, shortCall) {	
		if (defCall.strike > shortCall.strike) return defCall.strike - shortCall.strike;
		return 0;
	}

	function callGain(defCall, shortCall, subject) {
		if (!subject.friend) return Infinity;
		return subject.collateral - callCollateral(defCall, shortCall);
	}
}
	
function clearOption(option) {
	delete option.friend;
	delete option.release;
	delete option.collateral;
}
	
function findPutPair(symbol, defPut, shortPut) {	
	let data = optionsBySymbol[symbol];
	let maxGain = 0;
	let candidate = null;
	let seeker = defPut;
	if (!seeker) seeker = shortPut;
	if (defPut) {
		for (let j = 0; j < data.shortPut.length; j++) {
			if (evaluate(defPut, data.shortPut[j], data.shortPut[j])) break;
		}
	}
	if (shortPut) {
		for (let j = 0; j < data.defPut.length; j++) {
			if (evaluate(data.defPut[j], shortPut, data.defPut[j])) break;
		}
	}

	if (candidate) {
		let orphan = candidate.friend;
		if (orphan) clearOption(orphan);
		
		candidate.friend = seeker;
		seeker.friend = candidate;
		if (!candidate.release) candidate.release = 0;
		candidate.release += maxGain;
		seeker.release = candidate.release;
		
		candidate.collateral = candidate.strike - candidate.release;
		seeker.collateral = seeker.strike - seeker.release;
		
		if (orphan) {
			if (defPut) findPutPair(symbol, orphan);
			else if (shortPut) findPutPair(symbol, null, orphan);
		}
	}	
	
	function evaluate(defPut, shortPut, subject) {
		if (defPut.expire >= shortPut.expire && putGain(defPut, shortPut, subject) > maxGain) {
			maxGain = putGain(defPut, shortPut, subject);
			candidate = subject;
			if (candidate.friend) return true;
		}
		return false;
	}

	function putGain(defPut, shortPut, subject) {
		let release = shortPut.strike;
		if (defPut.strike < shortPut.strike) release = defPut.strike;
		if (!subject.friend) return release;
		return release - subject.release;
	}
}
	
function noCondorCalls(symbol) {
	let data = optionsBySymbol[symbol];
	let totalCalls = 0;
	data.shortCall.forEach(function(contract) {
		if (contract.collateral !== undefined) totalCalls += contract.collateral;
		else console.log("CALL CONTRACT LACKS COLLATERAL");
	});	
	return totalCalls;
}
	
function noCondorPuts(symbol) {
	let data = optionsBySymbol[symbol];
	let totalPuts = 0;	
	data.shortPut.forEach(function(contract) {
		if (contract.collateral) totalPuts += contract.collateral;
		else totalPuts += contract.strike;
	});	
	return totalPuts;
}

function condor(symbol, build) {
	let data = optionsBySymbol[symbol];
	let callExpireTypes = {};
	let putExpireTypes = {};
	data.shortCall.forEach(function(contract) {
		if (!callExpireTypes[contract.expire]) {
					callExpireTypes[contract.expire] = {};
					putExpireTypes[contract.expire] = {};
		}
		if (contract.collateral !== undefined) {
			if (!callExpireTypes[contract.expire][contract.strike]) callExpireTypes[contract.expire][contract.strike] = contract.collateral;
			else callExpireTypes[contract.expire][contract.strike] += contract.collateral;
		} else console.log("CALL CONTRACT LACKS COLLATERAL");
	});
	
	data.shortPut.forEach(function(contract) {
		if (!putExpireTypes[contract.expire]) putExpireTypes[contract.expire] = {};
		if (contract.collateral) {
			if (!putExpireTypes[contract.expire][contract.strike]) putExpireTypes[contract.expire][contract.strike] = contract.collateral;
			else putExpireTypes[contract.expire][contract.strike] += contract.collateral;
		}
		else {
			if (!putExpireTypes[contract.expire][contract.strike]) putExpireTypes[contract.expire][contract.strike] = contract.strike;
			else putExpireTypes[contract.expire][contract.strike] += contract.strike;
		}
	});	
	
	let totalSaved = 0;
	for (let expiration in putExpireTypes) {
		for (let strike in putExpireTypes[expiration]) {
			for (let callStrike in callExpireTypes[expiration]) {
				if (parseInt(callStrike) > parseInt(strike)) {
					if (callExpireTypes[expiration][callStrike] <= putExpireTypes[expiration][strike]) {
						putExpireTypes[expiration][strike] -= callExpireTypes[expiration][callStrike];						
						totalSaved += callExpireTypes[expiration][callStrike];
						callExpireTypes[expiration][callStrike] = 0;
					}
					else {
						callExpireTypes[expiration][callStrike] -= putExpireTypes[expiration][strike];
						totalSaved += putExpireTypes[expiration][strike];
						putExpireTypes[expiration][strike] = 0;
						break;
					}
				}
			}
		}
	}
	
	let totalPuts = 0;
	let totalCalls = 0;
	for (let expiration in putExpireTypes) {
		let expirePuts = 0;
		for (let strike in putExpireTypes[expiration]) {
			expirePuts += putExpireTypes[expiration][strike];
		}
		
		if (build && expirePuts > 0) {
			let li = make('li');
			li.title = expirePuts;
			li.onmouseenter = function() { this.style.fontWeight = "bold"; }
			li.onmouseleave = function() { this.style.fontWeight = ""; }
			li.style.textAlign = "center";					
			addCol(symbol, "100px", li);	
			addCol("Call", "100px", li);
			addCol(expiration, "150px", li);
			addCol("$" + expirePuts, "100px", li);
			let toAdd = true;
			for (let j = 0; j < D('unpaired').children.length; j++) {
				if (parseFloat(li.title) < parseFloat(D('unpaired').children[j].title)) {
					D('unpaired').insertBefore(li, D('unpaired').children[j]);
					toAdd = false;
					break;
				}
			}
			if (toAdd) D('unpaired').appendChild(li);
		}
		totalPuts += expirePuts;
		
		let expireCalls = 0;
		for (let callStrike in callExpireTypes[expiration]) {
			expireCalls += callExpireTypes[expiration][callStrike];
		}
		
		if (build && expireCalls > 0) {
			let li = make('li');	
			li.title = expireCalls;
			li.onmouseenter = function() { this.style.fontWeight = "bold"; }
			li.onmouseleave = function() { this.style.fontWeight = ""; }
			li.style.textAlign = "center";	
			addCol(symbol, "100px", li);	
			addCol("Put", "100px", li);
			addCol(expiration, "150px", li);
			addCol("$" + expireCalls, "100px", li);
			let toAdd = true;
			for (let j = 0; j < D('unpaired').children.length; j++) {
				if (parseFloat(li.title) < parseFloat(D('unpaired').children[j].title)) {
					D('unpaired').insertBefore(li, D('unpaired').children[j]);
					toAdd = false;
					break;
				}
			}
			if (toAdd) D('unpaired').appendChild(li);
		}
		totalCalls += expireCalls;
	}
					
	return totalSaved + totalPuts + totalCalls;
}
	

</script>
