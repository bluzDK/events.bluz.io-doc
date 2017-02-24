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

###POST
Post a device ID to the API so that data can be logged for it. Data is not stored until the device is posted, so you must do this first (and once) before you try to collect data. It can take up to 30 seconds after this call before data will be available.

###GET
Get a list of events based on the device ID. List is returned in order by date, newest events first

###DELETE
Delete a device ID from logging. This will stop all future logging of data. 

####Arguments

The following arguments must be passed in the URL:
- device ID: Device ID of the events to retreieve
- access token: Access token of your account, used to verify ownership of device

The following are optional parameters for GET that can be provided in the URL
- limit: The number of events to return, default is 50
- name: Name of the event to filter on, is a wildcard match so passing in 'foo' will match 'foo' and 'foobar'

####Response:
Code | Description 
--- | --- 
200| Success
400| Bad Request
403| Forbidden
500| Server Error

Get returns a list of event's in the following format:
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
# first POST the device ID, only need to do this once per device
curl https://events.bluz.io/devices/b1e2b1e2b1e2b1e2b1e2b1e2 -d access_token=12345

# next, we can get data!
curl https://events.bluz.io/devices/b1e2b1e2b1e2b1e2b1e2b1e2\?limit\=10\&access_token\=12345

# if we ever want to delete it from collecting more data...
curl https://events.bluz.io/devices/b1e2b1e2b1e2b1e2b1e2b1e2\?access_token\=12345 -X DELETE
```
