AutoKarma
===

A lightweight notification service to allow motorists to register their vehicle and receive targeted notifications in case of an issue (eg failed tail light). 1 to many notifications can also be sent for vehicle recalls etc.

* register vehicle for notifications
* share a notification with the network

All API (REST, POST) driven to allow maximum integration/adoption opportunities. Lightweight server implementation utilizing a cloud hosted backend database and serverless API layer to manage updates and notifications. Individual interfaces - native mobile apps, SMS integration, Voice Assistant 'skills' etc - operate against these common APIs.

Vehicle profile
===
* Vehicle ID (longint sequential or a GUID. Not user visible)
* [M] Country [, State], Rego/Plate
* [O] Make, Model [, sub-model/trim], Year, Colour
* [O] VIN (used for bulk notifications)

([M]: Mandatory, [O] optional)

* Question: how to validate a vehicle? Do we need to? Places like VIC can do a [lookup](https://www.vicroads.vic.gov.au/registration/buy-sell-or-transfer-a-vehicle/buy-a-vehicle/check-vehicle-registration/vehicle-registration-enquiry) for VIN given rego, but complex to automate

User profile
===
* userid
* email
* password
* email validated [t/f] - required to send notifications
* SMS number (for notifications - once funded)
* Twitter handle (for notifications)
* other communications paths (for notifications)


Vehicle Interest
===

* Reference ID (needed as more than one user could have an interest in notifications for a vehicle, or one user could need notifications from many vehicles - eg in a fleet scenario)
* index on userid and vehicle ID

(only validated users can create/edit these records)

* Question: how to validate ownership of a vehicle? Do we need to (honesty system ... after all, it's called "karma"!)?

Notfications
====
* The system runs as an exposed API that allows messages to be sent from multiple sources (website, Voice Assistant, sms, twitter etc)
* Login/validation not required to send notifications (to encourage easy initial engagement), but more capabilities exposed if user is registered/logged in. Spam will hopefully be reduced as the messages are specific codes rather than freeform text (eg a URL can't be sent)
* Messages are received, optionally aknowledged, tracked, and sent to the relevant vehicle 'listeners'.
* Logged in, registered senders can track/review their sent messages.
* Vehicle notifications can be viewed historically

simple API, POST supporting URLEncoded or JSON payloads
* country (optional, use geoid to default if not supplied)
* state (optional, use geoid to default if not supplied)
* rego/plate (required, used with country/state to identify vehicle)
* sender (optional, userid. if not supplied but sender can be determined by phone number, twitter handle etc then automatically assign)
* notification:
  * tail light not working [L/R]
  * brake light not working [L/R/center]
  * indicator not working [L/R, F/R]
  * headlight not working [L/R]
  * lights not on (short term)
  * exhaust loose
  * smoke/oil burning
  * debris lodged under body
  * trailer lights not working
  * trailer sparks
  * item dropped (unsecured load)
  * others ??
* location (from gps/geoid/ip2geo etc), especially useful for item dropped

Responses from the service would indicate that the message has been received - an http 200 indicating success, 400 if the payload is malformed, 404 if the vehicle is not found/registered.

A notification then triggers an email (and/or text, tweet etc) to alert the owner to the notification (intelligent limiting to ensure if there are a dozen notifications of a broken tail light in a day the user only gets pinged once)

Upon receipt of a notification a user can acknowledge receipt, and later aknowledge as 'fixed'.  If the notification(s) were sent by registered user(s) they will see an update on their dashboard of the number of people they've helped.

Funding
---
Would be great to get users to subscribe, but can't see that happening. Corporate sponsorship is more viable.

Donations would be great ;)

Ad supported on the website - seems a good target for automotive suppliers of all sorts, so a good fit (especially if a tail light is out, you need somewhere to buy one!)