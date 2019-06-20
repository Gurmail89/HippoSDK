# Installing Hippochat SDK on Android

Install	Hippochat	to	see	and	talk	to	users	of	your	Android	app.
Hippochat	for	Android	supports	API	16	and	above.

## Pre	Requisites	:

````
1. Hippochat Library can be included in any Android application.
2. Hippochat Library supports Android 4.1.x	(JELLY_BEAN) and later.
3.	Hippochat SDK supports apps targeting Android version 5.0+.
The	SDK	itself is compatible all the way down to JELLY_BEAN(API	Level 16).
````
If	you	have	any	queries	during	the	integration,	please	reach	out	to	us	at
support@fuguchat.com

## Step	1:	Add	Hippo	SDK	to	your

## app

Add	the	following	dependency	to	your	app	module’s	build.gradle	file
project/app/build.gradle:

```
android{
				...
				compileOptions	{
```

```
								sourceCompatibility	JavaVersion.VERSION_1_
								targetCompatibility	JavaVersion.VERSION_1_
				}
				packagingOptions	{
								pickFirst('META-INF/LICENSE')
								pickFirst('META-INF/LICENSE.txt')
				}
}
dependencies	{
				implementation	'com.hippochat:hippo:1.0.3’
}
```
Add	Kotlin support	in	your	project(If	not	supported)
Add	the	following	code	into	your	project	build.gradle	file

```
buildscript	{
				ext.kotlin_version	=	'1.2.71'
			...
				dependencies	{
								...
								classpath	"org.jetbrains.kotlin:kotlin-gradle-plug
in:$kotlin_version"
				}
}
```
Do	not	forget	to	add	internet	permission	in	manifest	if	already	not
present


```
<uses-permission	android:name="android.permission.INTERNET
"	/>
```
## Step	2:	Add	provider	in

## AndroidManifest.xml

```
<provider
				android:name="android.support.v4.content.FileProvider"
				android:authorities="{applicationId}.provider"	//eg	:
com.example.project.provider
				android:exported="false"
				android:grantUriPermissions="true">
				<meta-data
							android:name="android.support.FILE_PROVIDER_PATHS"
							android:resource="@xml/provider_paths"	/>
</provider>
```
```
Create	a	folder	named	xml	in	app/src/main/res
Create	a	file	named	provider_paths.xml	in	xml	folder	and	a
dd	following	code	:
<paths	xmlns:android="http://schemas.android.com/apk/res/a
ndroid">
				<external-path	name="external_files"	path="."/>
</paths>
```

## Step	3:	Initializing	Hippo	SDK

Then	initialize	paper	db	in	onCreate()	Method	of	application	class	:

```
Paper.init(getApplicationContext());
```
Create	Hippo	instance	with	your	app	secret	key	before	invoking/
attempting	to	use	any	other	features	of	Fugu	SDK.

Note	:
We	highly	recommend	instance	creation	only	once	and	from	your	app’s
launcher/main	activity’s	onCreate() function.
Don’t	forget	to	replace	the	YOUR-APP-SECRET-KEY in	the	following	code
snippet	with	the	actual	app	secret	key.

Before	invoking/	attempting	to	use	any	other	features	you	need	to
initialised	Hippo	SDK.

```
CaptureUserData	userData	=	new	CaptureUserData.Builder()
							.userUniqueKey("your	unique	identifier	for	user")
							.fullName("Full	name	string")
							.email("Email	string")
							.phoneNumber("phone	number	string")
							...
							.build();
```

```
				HippoConfigAttributes	configAttributes	=	new	HippoConf
igAttributes.Builder()
								.setAppKey(YOUR-APP-SECRET-KEY)
								.setAppType(APP_TYPE)
								.setCaptureUserData(userData)
								.setProvider(Dependencies.getFileProvider())
								.setDeviceToken(Dependencies.getDeviceToken(this))
								.build();
```
```
				HippoConfig.initHippoConfig(this,	configAttributes);
```
#### -OR-

##### If	you	have	reseller	token	then	use	the	following

##### method	in	HippoConfigAttributes	builder

```
				HippoConfigAttributes	configAttributes	=	new	HippoConf
igAttributes.Builder()
								...
								.setResellerToken()
								.setReferenceId()
								...
				.build();
```
```
				HippoConfig.initHippoConfig(this,	configAttributes);
```

### Hippo	offers	some	other	amazing

### features

#### To	send	custom	attributes:

```
HashMap<String,	Object>	customAttributes	=	new	HashMap<>()
;
	customAttributes.put(KEY,		VALUE);
```
```
HippoConfigAttributes	configAttributes	=	new	HippoConfigAt
tributes.Builder()
...
.setCustomAttributes(customAttributes)
...
.build();
```
#### To	configure	test	environment	use:

```
HippoConfigAttributes	configAttributes	=	new	HippoConfigAt
tributes.Builder()
...
.setShowLog(true)
...
.build();
```
#### To	enable	payment	use:


```
HippoConfigAttributes	configAttributes	=	new	HippoConfigAt
tributes.Builder()
...
.setPaymentEnabled(true)
...
.build();
```
#### To	get	unread	count:

```
HippoConfigAttributes	configAttributes	=	new	HippoConfigAt
tributes.Builder()
...
.setUnreadCount(true)
...
.build();
```
```
//This	send	callback	in	following	interface:
FuguConfig.getInstance().setCallbackListener(new	UnreadCou
nt()	{
				@Override
				public	void	count(int	count)	{
				Log.v("Unread",	"Unread	Count	=	"	+	count);
				}
});
```
### Changing	the	colors	of	Hippo	Chat


### screens	to	give	a	look	and	feel	of	your

### application

Use	.setColorConfig(hippoColorConfig) function	in	builder	to	easily
replicate	your	application’s	look	and	feel	in	Hippo	Activities/Screens,
reference	code	snippet	is	as	follows	:

###### 	/**		

```
				*	Set	color	configurations,	by	default	Hippo
				*	default	colors	would	be	used
				*/
				HippoColorConfig	hippoColorConfig	=	new	HippoColorConf
ig.Builder()
				...
				.hippoActionBarBg(YOUR_ACTION_BAR_BG_COLOR_STRING)
				...
.build();
```
```
				HippoConfigAttributes	configAttributes	=	new	HippoConf
igAttributes.Builder()
								...
								setColorConfig(hippoColorConfig)
								...
				.build();
```

# Showing	conversations	inside

# your	application

In	response	to	a	specific	UI	events	like	a	menu	selection	or	button
onClick	event,	invoke	the	showConversations()	function	to	launch	the
Conversation	flow.	If	the	app	has	multiple	channels	configured,	the	user
will	see	the	channel	list.	Channel	list	is	ordered	as	specified	in	the
Dashboard	when	there	are	no	messages.

```
//	Launching	Conversation	List	from	click	of	a	button	in	y
our	app	screen
myConversationsButton.setOnClickListener(new	OnClickListen
er()	{
				@Override
				public	void	onClick(View	view)	{
								FuguConfig.getInstance().showConversations(<Pass	c
alling	activity	here>,	TITLE_TEXT);
				}
});
```
# Handling	Push	Notifications

## Prerequisites:

```
1 .	FCM	Implemented	in	you	app
2 .	Device	running	Play	Services
```

```
3 .	FCM	Server	Key
```
### Steps	to	get	push	notifications	working

### with	Hippo

```
1 .	Sending	the	device	registration	token	to	Hippochat	SDK
2 .	Customizing	Notification	appearance	and	passing	notification
data	to	Hippochat	SDK
3 .	Handle	push	notification	click
```
### 1:	Send	Device	Registration	Token

In	the	app’s	implementation	of	FirebaseInstanceIdService,	send	the
token	to	Hippo	as	follows

```
HippoConfigAttributes	configAttributes	=	new	HippoConfigAt
tributes.Builder()
...
.setDeviceToken(token)
...
.build();
```
### 2:	Customizing	Notification	appearance

### and	passing	notification	data	to	Hippo

In	the	app’s	implementation	of	FirebaseMessagingService,	pass	the


RemoteMessage	object	to	Hippo	if	it	is	a	Hippo	notification.

```
@Override
public	void	onMessageReceived(RemoteMessage	remoteMessage)
	{
HippoNotificationConfig	hippoNotificationConfig	=	new	Hipp
oNotificationConfig();
if	(hippoNotificationConfig.isHippoNotification(remoteMess
age.getData()));	{
			if(hippoNotificationConfig.isHippoCallNotification(getA
pplicationContext(),	remoteMessage.getData()))	{
							JSONObject	messageJson	=	new	JSONObject(remoteMessa
ge.getData().get("message"));
HippoCallConfig.getInstance().onNotificationReceived(getAp
plicationContext(),	messageJson);
			}	else	{
							hippoNotificationConfig.setSmallIcon(R.mipmap.ic_la
uncher);
							hippoNotificationConfig.setNotificationSoundEnabled
(true);
hippoNotificationConfig.setNotificationSoundEnabled(true/f
alse);
			hippoNotificationConfig.setPriority(NotificationCompat.
PRIORITY_HIGH);
							hippoNotificationConfig.showNotification(getApplica
tionContext(),	remoteMessage.getData())
			}
}	else	{
```

```
			//	Your	logic	goes	here
}
}
```
### 3:	Handle	push	notification	click

When	a	user	taps	on	a	push	notification	we	hold	onto	data	such	as	the
URI	in	your	message	or	the	conversation	to	open.	When	you	want	Fugu
to	act	on	that	data,	just	perform	following	steps:

Pass	the	intent	that	you	get	in	your	Splash	activity	to	Home	Activity

```
Intent	mainIntent	=	new	Intent(this,	<Your	home	activity>)
;
mainIntent.putExtra("bundle",	getIntent().getExtras());
```
And	in	onCreate() method	of	HomeActivity use	following	line	to
perform	relevant	push	action:

```
new	HippoNotificationConfig()
.handleHippoPushNotification(HomeActivity.this,	getIntent(
).getBundleExtra("bundle"));
```
### Clear	User	Data	on	Logout

Clear	user	data	at	logout	or	when	deemed	appropriate	based	on	user


action	in	the	app	by	invoking	the	clearFuguData	function:

```
HippoConfig.clearHippoData(<Pass	calling	activity	here>);;
```
# Advanced	features

## Open	chat	via	specific	transaction/Peer

## to	Peer	chat

In	response	to	a	specific	UI	events	like	a	menu	selection	or	button	on
click	event,	invoke	the	openChat() function	to	launch	the	specified
channel	by	passing	channel	id.

```
//	Launching	Conversation	List	from	click	of	a	button	in	y
our	app's	screen
				myChannelButton.setOnClickListener(new	OnClickListener
()	{
				@Override
				public	void	onClick(View	view)	{
							ChatByUniqueIdAttributes	chatAttr	=	new	ChatByUniqu
eIdAttributes.Builder()
																.setTransactionId(<ID>)
																.setUserUniqueKey(<UNIQUE_KEY>)
																.setChannelName(<CHANNEL_NAME>
																...
																.build()
							HippoConfig.getInstance().openChatByUniqueId(chatAt
```

tr);
				}
});	

```
Note:	For	Peer	to	Peer	chat	you	have	to	send	other	user	unique
keys	in	ChatByUniqueIdAttributes	builder
```


This is a offline tool, your data stays locally and is not send to any server!
Feedback & Bug Reports
