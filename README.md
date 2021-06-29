# WU

// ==UserScript==
// @name        Wu Tang Clan
// @author      waltzd@
// @namespace   http://tampermonkey.net/
// @version     1.0
// @description Displays a WU member name next to the user's given name
// @include     https://phonetool.amazon.com/users/*
// @grant       GM.xmlHttpRequest
// ==/UserScript==

function hash(str) {
  var hash = 0, i, chr;
  if (str.length === 0) return hash;
  for (i = 0; i < str.length; i++) {
    chr   = str.charCodeAt(i);
    hash  = ((hash << 5) - hash) + chr;
    hash |= 0; // Convert to 32bit integer
  }
  return hash;
};

try {
    GM.xmlHttpRequest({
        url: 'https://akabab.github.io/superhero-api/api/all.json',
        method: "GET",
        headers: {"Accept": "application/json"},
        onload: function(response) {
            var responseList = eval("(" + response.responseText + ")");
            var randNum = Math.ceil(Math.random() * Math.floor(563));
            // uncomment following line if you want a superhero associated with a user and not change randomly
            // var randNum = hash(window.location.pathname.substring(window.location.pathname.lastIndexOf('/') + 1));
            if(randNum < 0){
                randNum *= -1;
            }
            randNum %= responseList.length;
            console.log(randNum);
            var item = responseList[randNum];
            var container = document.querySelector(".PersonalInfoName");
            var nameForGSearch = item.name.split(" ").join("+");
            var googleSearchLink = `https://www.google.com/search?q=${nameForGSearch}+superhero`;
            var imgLink =
            `<span style='margin-left: 23px;
            color:#88292F; font-size:23px'>${item.name}</style>
            <a href=${googleSearchLink} target="_blank"> <img style="border-radius: 50%; width:48px; height:48px;" src=${item.images.md}> </a>`;
            container.innerHTML += imgLink;
        }
    });
}
catch (e) {
    console.log(`'${GM.info.script.name}' Update the script! Contact paladurg@`, e);
