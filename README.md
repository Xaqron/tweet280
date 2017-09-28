آموزش توییت ۲۸۰ کاراکتری
===================
----------

> - به سایت [تمپرمانکی](http://tampermonkey.net) رفته و افزونه را بر ااساس مرورگر خود انتخاب و نصب کنید.
> - کد زیر را به عنوان یک اسکریپت جدید در افزونه اضافه و ذخیره نمایید:

```javascript
// ==UserScript==
// @name         Tweet 280
// @namespace    http://tampermonkey.net/
// @version      1.0
// @description  Tweet 280 characters
// @author       Xaqron
// @run-at       document-idle
// @match        https://tweetdeck.twitter.com/*
// @grant        *
// ==/UserScript==

var stopcondition = false;


interval = setInterval(function(){
    if (stopcondition==false) {
        if (/New Tweet/i.test (document.body.innerHTML) )        {
            TD.services.TwitterClient.prototype.makeTwitterCall = function(b, e, f, g, c, d, h) {
                c = c || function() {};
                d = d || function() {};
                b = this.request(b, {
                    method: f,
                    params: Object.assign(e, {
                        weighted_character_count: !0
                    }),
                    processor: g,
                    feedType: h
                });
                return b.addCallbacks(function(a) {
                    c(a.data);
                }, function(a) {
                    d(a.req, "", a.msg, a.req.errors);
                }), b;
            };


            twttrTxt = Object.assign({}, twttr.txt, {
                isInvalidTweet: function() {
                    return !1;
                },
                getTweetLength: function() {
                    return twttr.txt.getTweetLength.apply(this, arguments) - 140;
                }
            });
            stopcondition = true;
        }
    }

},500);
```
> - به سایت [توییت دک](https://tweetdeck.twitter.com) (متعلق به توییتر و زیر دامنه آن است) رفته و با فعال بودن اسکریپت اقدام به ارسال توییت نمایید.