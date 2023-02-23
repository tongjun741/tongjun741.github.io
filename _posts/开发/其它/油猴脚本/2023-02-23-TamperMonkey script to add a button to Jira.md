---
tags:
    - 其它
    - 油猴脚本
---

The below script detects whether we are in Jira, and if it is run in Jira it will add a button. Clicking that button will use the REST API of Jira to create an issue, and navigate to the recently created issue.

```js
// ==UserScript==
// @name         Tell Jira I did a thing
// @namespace    http://demuyt.net/
// @version      0.1
// @description  try to take over the world!
// @author       Tom J Demuyt
// @match        https://jira*
// @match        https://jira.it.aenetworks.com/*
// @grant        none
// ==/UserScript==

//Documentation
//https://docs.atlassian.com/software/jira/docs/api/REST/7.12.0/
//https://developer.atlassian.com/server/jira/platform/jira-rest-api-examples/

(function() {
    'use strict';

    let userName = "";
    let projectCode = "CF";
    let fixVersion = "15484";

    //The id of the create button is `create_link`, dont show up if that button is not there
    let createButton = document.getElementById('create_link');
    //Leave quietly if we cannot find that button
    if(!createButton){
        return;
    }

    //Get the parent (<ul>) of the <li> that holds the <a>
    let createButtonParent = createButton.parentNode.parentNode;

    //Add a button for the user
    var node = document.createElement("LI");
    node.innerHTML = '<li style="padding-left: 20px; text-decoration: underline; cursor: pointer;"><div id="idat">Log activity</div></li>';
    createButtonParent.appendChild(node);

    //Get a reference to the button
    let idat = document.getElementById("idat");

    //Make it listen to clicks;
    idat.addEventListener("click", recordTheThing);
    //Try and build a ticket
    function recordTheThing(){
        //Prep a call to Jira
        let xhr = new XMLHttpRequest(),
            url = '/rest/api/2/issue/';
        //Ask what the user did
        var thing = window.prompt("Describe the activity performed?","");
        //Get out if the user got cold feet
        if(!thing){
            return;
        }
        //Start prepping the http request
        xhr.open('POST', url, true);
        //Use JSON since we will not fill in a form
        xhr.setRequestHeader("Content-Type", "application/json");
        //Prep the navigation to the new issue
        xhr.onreadystatechange = function () {
            debugger;
            //readyState:3
            //response: {"id":"125529","key":"CF-10282","self":"https://jira.it.aenetworks.com/rest/api/2/issue/125529"}
            //HTTP/REST return code 201 means created
            if (xhr.readyState === 4 && xhr.status === 201) {
                var json = JSON.parse(xhr.responseText);
                window.location.href = 'https://jira.it.aenetworks.com/browse/' + json.key;
            }
        };
        //The heart of the thing
        var data = JSON.stringify({
            "fields": {
                "project":
                {
                    "key": projectCode
                },
                "summary": thing,
                "description": thing,
                "issuetype": {
                   "name": "Task"
                },
                "assignee": {
                   "name": userName
                },
                //15484 -> Busines As Usual
                "fixVersions": [{ "id": fixVersion}],
            }
        });
        //Finalize the REST call
        xhr.send(data);

    }//Record the thing

    //Find from the load/start the user id
    let xhr = new XMLHttpRequest(),
    url = '/rest/api/2/mypermissions/';
    //Prep to extract the username from the call header
    //200 -> OK
    xhr.onreadystatechange = function () {
      if(xhr.readyState == 4 && xhr.status == 200){
        userName = xhr.getResponseHeader("X-AUSERNAME");
      }
    }
    //Shoot and pray
    xhr.open('GET', url, true);
    xhr.send();
})();
```

Please review for cleanliness, readability, standards, the usual..

[javascript](https://codereview.stackexchange.com/questions/tagged/javascript)[ecmascript-6](https://codereview.stackexchange.com/questions/tagged/ecmascript-6)[userscript](https://codereview.stackexchange.com/questions/tagged/userscript)

[share](https://codereview.stackexchange.com/q/204768) [improve this question](https://codereview.stackexchange.com/posts/204768/edit) follow 

[edited Oct 3 '18 at 15:45](https://codereview.stackexchange.com/posts/204768/revisions)

[![img](/img-post/开发/其它/油猴脚本/TamperMonkey script to add a button to Jira.assets/QKxjQ.png)](https://codereview.stackexchange.com/users/120114/sᴀᴍ-onᴇᴌᴀ)

[Sᴀᴍ Onᴇᴌᴀ](https://codereview.stackexchange.com/users/120114/sᴀᴍ-onᴇᴌᴀ)

**20.2k**1010 gold badges3030 silver badges127127 bronze badges

asked Oct 2 '18 at 12:19

[![img](/img-post/开发/其它/油猴脚本/TamperMonkey script to add a button to Jira.assets/f9bf2c569b2bd023184b8ea9b6b1183c.png)](https://codereview.stackexchange.com/users/14625/konijn)

[konijn](https://codereview.stackexchange.com/users/14625/konijn)

**30.8k**44 gold badges6262 silver badges245245 bronze badges

- To begin with... you're separating your `let` statements with comments, even though comments don't break anything. – [FreezePhoenix](https://codereview.stackexchange.com/users/168361/freezephoenix) [Oct 2 '18 at 13:26](https://codereview.stackexchange.com/questions/204768/tampermonkey-script-to-add-a-button-to-jira#comment394938_204768)

[add a comment](https://codereview.stackexchange.com/questions/204768/tampermonkey-script-to-add-a-button-to-jira#)



## 1 Answer

[Active](https://codereview.stackexchange.com/questions/204768/tampermonkey-script-to-add-a-button-to-jira?answertab=active#tab-top)[Oldest](https://codereview.stackexchange.com/questions/204768/tampermonkey-script-to-add-a-button-to-jira?answertab=oldest#tab-top)[Votes](https://codereview.stackexchange.com/questions/204768/tampermonkey-script-to-add-a-button-to-jira?answertab=votes#tab-top)





2







The code is well commented. The indentation in most places seems to be four spaces but the last few lines have indentation of two spaces so that could be made more consistent. The last comment is a bit humorous...

It seems that if the button isn't found by the id attribute, it just bails. There could be alternative ways to find that button. For example, my company recently upgraded to version 7.11.2 and it appears that the create button still has an id attribute of `create_link` but the list item above it also has an id attribute of `create-menu` so that element could be found instead. And the unordered list also has a class name of `aui-nav` so if the create button can't be found the list could be found instead. I also see in the latest online version 1001.0.0 (e.g. *.atlassian.net) that the navigation menu has been moved to the left side of the screen and no longer uses list items so if your Jira gets updated you'll need to update your approach to finding that element.

I know you are familiar with `const` given your answers [like this one](https://codereview.stackexchange.com/a/149644/120114) so why not use it for things like `projectCode`, `idat`?

Also, a list item element is created and then the innerHTML contains another list item element:

> ```js
> var node = document.createElement("LI");
> node.innerHTML = '<li style="padding-left: 20px; text-decoration: underline; cursor: pointer;"><div id="idat">Log activity</div></li>';
> ```

This yields HTML like below:

> ```js
> <li><li style="padding-left: 20px; text-decoration: underline; cursor: pointer;"><div id="idat">Log activity</div></li></li>  
> ```

[MDN's documentation for ``](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/li) claims "*It must be contained in a parent element: an ordered list ([``](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/ol)), an unordered list ([``](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/ul)), or a menu ([``](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/menu))*"[1](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/li) and that the permitted content is [Flow Content](https://developer.mozilla.org/en-US/docs/Web/Guide/HTML/Content_categories#Flow_content), which does not include list item elements. But obviously our browsers render it as expected. Ideally that list item would not contain a child list item...

You could consider using the [fetch API](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API) instead of classical XHR requests unless [browser compatibilty](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API#Browser_compatibility) is an issue.

And since you are using ecmascript-2015 features like `let`, you could consider using more arrow functions...

1https://developer.mozilla.org/en-US/docs/Web/HTML/Element/li



[share](https://codereview.stackexchange.com/a/204857) [improve this answer](https://codereview.stackexchange.com/posts/204857/edit) follow 

[edited Oct 3 '18 at 16:17](https://codereview.stackexchange.com/posts/204857/revisions)

answered Oct 3 '18 at 15:54

[![img](/img-post/开发/其它/油猴脚本/TamperMonkey script to add a button to Jira.assets/QKxjQ.png)](https://codereview.stackexchange.com/users/120114/sᴀᴍ-onᴇᴌᴀ)

[Sᴀᴍ Onᴇᴌᴀ](https://codereview.stackexchange.com/users/120114/sᴀᴍ-onᴇᴌᴀ)

**20.2k**1010 gold badges3030 silver badges127127 bronze badges

- Good call on const, very good call on the list item, will read about fetch. I am horrified by the abuse of anonymous fat arrow functions. – [konijn](https://codereview.stackexchange.com/users/14625/konijn) [Oct 3 '18 at 16:26](https://codereview.stackexchange.com/questions/204768/tampermonkey-script-to-add-a-button-to-jira#comment395115_204857)