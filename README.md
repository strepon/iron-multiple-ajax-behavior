# iron-multiple-ajax-behavior
It is useful to send multiple XHR requests asynchronously and have one final handler to be invoked after all the requests are executed.
It is a polymer behvaior and can be easily used with the api based applicaton.

#installation
```
bower install iron-multiple-ajax-behavior
```
#usage
Let's take a situation where user has to call multiple api call and when all the api calls are executed certain action should be taken.
This is where iron-multiple-ajax-behavior can be useful.

#Example
Getting data from multiple sources and when all of them are recieved it should show data has been loades successfully.
please refere https://github.com/ankitsorathiya/iron-multiple-ajax-behavior/blob/master/demo/index.html

First import following behavior
```html
<link rel="import" href="iron-multiple-ajax-behavior.html">
```
Then inject behavior in polymer element 
```html
Polymer({
            is: "iron-multiple-ajax-demo",
            behaviors: [window.extendedIronMultipleAjaxBehavior],
            properties: {
                items: {
                    type: Array,
                    value: function () {
                        return [];
                    }
                },
                dataLoaded: {
                    type: Boolean,
                    value: false
                }
            },
            ready: function () {
                this.initializeData();
            },
            initializeData: function () {
                this.dataLoaded = false;
                //prepare all requests to be initiated
                var requests = [];
                for (var index = 1; index <= 10; index++) {
                    requests.push(this.getRequestObject(index));
                }
               //once we have all requests, fire multiple requests
               //the finalResponseHandler will be executed when all the requests are served.
               this._sendMultipleRequests(requests, "finalResponseHandler");
            },
            getRequestObject: function (index) {
                var url = 'https://jsonplaceholder.typicode.com/posts/' + index;
                return this._getIronRequestObj(url, "individualDataRetrival");
            },
            individualDataRetrival: function (item) {
                this.push("items", item);
            },
            finalResponseHandler: function (responses) {
            
                //this will be invoked when all the requests are served either with success or failure.
                this.dataLoaded = true;
            }
        });
 ```
 
What you can see is, the progress bar get's hidden when all the calls have executed successfully. 
 
<!--
```
<custom-element-demo>
  <template>
    <link rel="import" href="demo/iron-multiple-ajax-demo.html">
    <iron-multiple-ajax-demo></iron-multiple-ajax-demo>
  </template>
</custom-element-demo>
```
```html
<iron-multiple-ajax-demo></iron-multiple-ajax-demo>
```
-->
 
 
