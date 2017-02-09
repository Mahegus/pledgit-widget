(function () {
    var $pledgit = function (a) {
        this.w = a || [];
    }
    $pledgit.prototype.set = function (a, b) {
        this.w[a] = b || !1;
    }
    var pledgit = new $pledgit;

    function pl(key, value) {
        pledgit.set(key, value);
    }
    var W = window,
        postUrl = function () {
            return 'http://localhost:50629/api/campaign';
        };
    var request = function (url, payload, callback) {

        W.jQuery.ajax({
            url: url,
            type: "GET",
            success: function (result) {
                callback(result);
            },
            error: function (xhr, status) {

            }
        });
        return !0;
        /*
        var xhr = W.XMLHttpRequest;
        if (!xhr) return !1;
        var newXhr = new xhr;
        try {
            newXhr.open('GET', url, !0);
            newXhr.setRequestHeader('Content-Type', "text/plain");
            newXhr.onreadystatechange = function () {
               4 == newXhr.readyState && (callback(newXhr), newXhr = null)
            }
        
            newXhr.send();
        } catch (e) {
            pledgit.stop();
        }
        return !0;
        */
    }
    var is_undefined = function (object) {
        return 'undefined' === typeof object
    }
    pledgit.timeoutInterval = 30000,
    pledgit.timeout = null,
    pledgit.campaignId = '',
    pledgit.cid = function () {
        pledgit.campaignId = arguments[0]
    },
    pledgit.idElement = null,
    pledgit.id = function () {
        pledgit.idElement = document.querySelector('#' + arguments[0]);
    }
    pledgit.start = function () {
        //make sure that the id actually exists
        if (is_undefined(pledgit.idElement) || null === pledgit.idElement) return log('Element not found');

        //create the html to house the value
        pledgit.idElement.innerHTML = '<span>£0.00</span>';

        //make the first call to the server to get the value
        retrieve();
    },
    pledgit.refresh = function () {
        pledgit.timeout = setTimeout(function () {
            retrieve();
        }, pledgit.timeoutInterval);
    }
    pledgit.updateDonationFigure = function (response) {
        //update the value field
        log(response);
        pledgit.idElement.innerHTML = '<span>£' + response.response + '</span>';

        //refresh the figure in 30 seconds
        //pledgit.refresh();

    },
    pledgit.stop = function () {
        clearTimeout(pledgit.timeout)
    }
    var retrieve = function () {
        var url = postUrl() + '/' + pledgit.campaignId + '/donations';
        //add the campaign id to the url
        request(url, pledgit.w['cid'] || '', pledgit.updateDonationFigure);
    },
        log = function (text) {
            if (W.console) window.console.log(text);
        },
        createListeners = function (object, event, callback) {
            try {
                object.addEventListener ? object.addEventListener(event, callback) : object.attachEvent && object.attachEvent("on" + event, callback)
            } catch (e) {
                log(e)
            }
        },
        globalObject = function () {
            return 'undefined' !== typeof W['pl'] ? W['pl'] : []
        },
        getQueue = function () {
            return globalObject().q || []
        },
        clearQueue = function (index) {
            getQueue().length ? globalObject().q = [] : !0
        },
        runQueue = function () {
            var q = getQueue();
            for (var i = 0, j = q.length; i < j; i++) {
                try {
                    pledgit[q[i][0]](q[i][1]); 
                } catch (e) {
                    log('Function not found: ' + e);
                }
            }
            clearQueue()
        }
     
    
    createListeners(W, 'load', runQueue);
    
})(window);
