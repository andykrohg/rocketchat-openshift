Rocket.Chat: OpenShift Template
-------------------------------

This repository hosts a template to provision Rocket Chat in OpenShift.

### Deploy

```bash
oc new-project rocket-chat
oc process -f https://raw.githubusercontent.com/andykrohg/rocketchat-openshift/main/rocket-chat-persistent.yaml | oc create -f -
```
> This is _not_ idempotent, if you want to make a change to the template for some reason, you'll need to delete everything and recreate as opposed to using `oc apply`

### Configure
* Once the server comes up, login and create a user. **The first user you create will be the system administrator.** Note that organization information is optional.
* In order for *attachment uploads* (including screenshots) to work, you'll need to correct the **Site URL** in **Administration -> General**, as it defaults to `http://localhost:3000`. Set this to the address of your OpenShift `Route`.

* Rocket.Chat uses a domain check code to verify the validity of the e-mail address. To disable it, run the following commands:

```bash
# oc port-forward <mongodb_pod> 27017
# mongo localhost:27017

> use rocketchat
> db.auth('rocketchat-admin','rocketchat')
> db.rocketchat_settings.update({_id:'Accounts_UseDNSDomainCheck'},{$set:{value:false}})
```
