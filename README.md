[![Deployment status from DeployBot](https://bluz.deploybot.com/badge/66802254036135/99029.svg)](http://deploybot.com)

<p align="center" >
<img src="http://bluz.io/static/img/logo.png" alt="" title="">
</p>

Particle Event API
==========
This API will allow anyone to read back publicly published information from the Particle cloud for a given device ID.

**WARNING**: This API is considered experimental. Data integrity and access are not guaranteed, nor are there any guarantees on latency to fetch it. This API is merely in the test phase and should not be used for mission critical apps. 

##Endpoint
All functions use the common endpoint https://events.bluz.io

##Events
/devices/{device ID}

###GET
Get a list of events based on the device ID. List is returned in order by date, newest events first

####Arguments

The following arguments must be passed in the URL:
- device ID: Device ID of the events to retreieve
- access token: Access token of your account, used to verify ownership of device

The following are optional parameters that can be provided in the URL
- limit: The number of events to return, default is 50
- name: Name of the event to filter on, is a wildcard match so passing in 'foo' will match 'foo' and 'foobar'

####Response:
Code | Description
--- | ---
200| Success
400| Bad Request
403| Forbidden
500| Server Error

Returns a list of event's in the following format:
```
[
  {
    "deviceID": <device ID>,
    "name": <name of the event>,
    "data": <data of the event>,
    "dateTime": <date time the event was published>
  }
]
```

####Example:
```
curl https://events.bluz.io/devices/b1e2b1e2b1e2b1e2b1e2b1e2\?limit\=10\&access_token\=12345
```

FAQ
==========

**What is it?** It is a simple REST API to get access to your data, as easily as we could come up with. Any data you have sent with a Particle.publish (in non-PRIVATE mode) will be available to you. Nothing else you need to do, just call Particle.publish and you can access it.

**How does it work?** Simple, just call the API like this (well, put in your real device ID and access token first):
```
curl https://events.bluz.io/devices/b1e2b1e2b1e2b1e2b1e2b1e2\?limit\=10\&access_token\=12345
```
You will see the last 10 Particle.publish events for your device!

**What can I use it for?** All sorts of things! This would allow you to really quickly pull data into a website or app, for example, for graphing or reporting. Or you could easily write a script to dump the data to CSV for analyzing it in excel. The point is to let you not have to worry about the back-end storing of data.

**Aren't there others that do this, like Udibots and Grovestreams?** There sure are, and we aren't trying to replace them. We are just laying out another alternative to accessing your data.

**Do I have to register first?** Nope, just call the API and you will see your published data.

**Can other people access my data?** Nope. You provide your access token and we verify the events belong to you before you can get access to them. Your access token is never stored, only used to verify ownership.

**What about Particle.publish events that are PRIVATE?** Sorry, there is only access to public events for now

**Is it really free?** Yup, go ahead and try it, we'll wait...

**Is this a Particle built feature?** No, it is built by us over at bluz.  

**Ok, what's the deal?** Well, to be honest, we just wanted to see if people will use this type of feature. It has been talked about on the Particle forums for some time, and the feature may be available form Particle soon as well. But we figured we could provide a stop-gap solution, so why not! Plus, we had some AWS credits lying around and wanted to put them to good use, think of this as a public service brought to you by bluz!

**So what's the catch?** Nothing really, though we are calling this experimental (think pre-beta even) for now. Based on the volumes of data it requires some careful tuning and we can't be sure we got it right the first time around. So you probably shouldn't build a product on top of it, but for home projects or even prototypes we hope it can make your life easier. As the system becomes more stable, we may one day remove the experimental tag altogether.

**How long do you store data?** Not sure yet. We haven't set a limit, but we almost certainly will. Current thinking is 3-7 days sounds about right, but if we can go longer without significant performance implications, we will. 

**What if I want access to my PRIVATE events, or want to set this up for an organization?** We would be happy to talk to you about using the system for your own purposes. Shoot us an email at hello@bluz.io
