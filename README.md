# How to track events within an iframe with GTM or Matomo ?

Tracking an iframe with a tag manager isn't easy. By following these few steps, you'll be able to track everything inside an iframe!

## Step 1: Add trigger code to your iframe

By adding this code to your `iframe` source code, you allow your `iframe` to send information to its parent web page.

```javascript
<script>
    function sendEventFromIframe(data)
    {
        window.parent.postMessage({dataLayer: data}, '*'); 
    }
</script>
```

## Step 2: Track whatever you want with `sendEventFromIframe()`

Use this function whenever you want to send an event to your parent page `dataLayer`.
<br>
Example:

```html
<button onclick="sendEventFromIframe({event: 'demo', foo: 'bar'})">
    Click me !
</button>
```

## Step 3: Listen for iframe events in your website

Add this script to your regular website to handle iframe events and send them to the `dataLayer` object.

```javascript
<script>
    // Initial dataLayer declaration
    window.dataLayer = window.dataLayer || [];

    // Listen for iframe events
    window.addEventListener('message', function(event) {

        // Keep only event from a trusted source (optional but recommended)
        if (event.origin !== 'http://the-iframe-host.com') {
            return;
        }

        // Keep only event containing the dataLayer key
        if(!event.data.dataLayer){
            return;
        }

        // Push event data to the dataLayer
        window.dataLayer.push(event.data.dataLayer)
    })
</script>
```

## Step 3: Configure your Tag Manager to listen for dataLayer events

If you look at the "dataLayer" object you will see that the iframe events are visible, you are now free to use your tag manager as usual to collect the event and send data to your favorite analytics tool.