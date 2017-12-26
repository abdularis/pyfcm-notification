## fcm.py
This is a simple python module to send a firebase notification to registration id or list of registration id using firebase notification API.

> **Note:** Not all firebase notification features/keys are implemented.
> I'am using this for my project [UNMA-backend](https://github.com/abdularis/UNMA-backend)

#### References
- [About Firebase Cloud Messaging Server](https://firebase.google.com/docs/cloud-messaging/server)
- [Firebase Cloud Messaging HTTP Protocol](https://firebase.google.com/docs/cloud-messaging/http-server-ref)

#### Example

- **Send to list of registration id**
~~~python
import fcm

...

f = fcm.FcmNotification(SERVER_KEY)

# use data parameter to send custom dictionary data like
# f.send(registration_ids, data={'tag': 'Important!'})

try:
	responses = f.send(registration_ids, notification={'title': 'Hello World!'})
	for status_code, resp_msg in responses:
		if status_code == 200 and len(resp_msg.results) > 0:
			for result in resp_msg.results:
				# get an error from firebase response result if there is one
				error = result[1].get('error')
				# check if the error is related to registration id
				if error in fcm.reg_id_errors:
					remove_reg_id_from_db(result[0])
except requests.exceptions.ConnectionError as msg:
	print(msg)
~~~

- **Send to one registration id**
~~~python
import fcm

...

f = fcm.FcmNotification(SERVER_KEY)

# use data parameter to send custom dictionary data like
# f.send_to(regid, data={'tag': 'Important!'})
try:
	status_code, result = f.send_to(reg_id, notification={'title': 'Hello there'))
except requests.exceptions.ConnectionError as msg:
	print(msg)
~~~