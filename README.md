gosms
-----

Your own local SMS gateway
==========================

What's the use ?
----------------
Can be used to send SMS,
where you don't have access to internet or cannot use Web SMS gateways
or want to save some money per SMS,
or have minimal requirements for personal / internal use and such

- deploy in less than 1 minute
- supports Windows, GNU\Linux, Mac OS
- works with GSM modems
- provides API over HTTP to push messages to gateway, just like the internet based gateways do
- takes care of queuing, throttling and retrying

![gosms dashboard](https://raw.githubusercontent.com/haxpax/gosms/screenshot/screenshots/gosms.png)

deployment
----------
- Update conf.ini `[DEVICES]` section with your modem's COM port.
  for ex. `COM10` or `/dev/USBtty2`
- Run

API specification
------------------
- /api/sms/ [*POST*]
    - param **mobile**
        - mobile number to send message to
        - number should have contry code prefix
        - for ex. +919890098900
    - param **message**
        - message text
        - max length is limited to 160 characters
    - response
```json
{
  "status": 200,
  "message": "ok"
}
```
- /api/logs/ [*GET*]
    - response
```json
{
  "status": 200,
  "message": "ok",
  "summary": [ 10, 50, 2 ],
  "daycount": { "2015-01-22": 10, "2015-01-23": 25 },
  "messages": [
    {
      "uuid": "d04f17c4-a32c-11e4-827f-00ffcf62442b",
      "mobile": "+1858111222",
      "body": "Hey! Just playing around with gosms.",
      "status": 1
    },
  ]
}
```
    - message status codes
      - 0 : Pending
      - 1 : Processed
      - 2 : Error

planned features
-------
- Allowing multiple mobile numbers with a single message in `/api/sms/`
- CRUD support for messages, possibly support cancellation of message
- Authentication support for API
- Adding authentication for Dashboard
- Send an email to admin on high failure rate

building on windows
-------------------
- go get `github.com/haxpax/gosms`
- Setup GCC for go-sqlite3 package
	- Download MinGW from http://sourceforge.net/projects/mingw/
	- Add `C:\MinGW\bin` to PATH
	- run `mingw-get install gcc` from command line
- go build