# ionic-5-PHP-Push-Notifications

Sending Push Notifications to app users plays a very import role in marketing the product, notifying the user related info etc.User get the instant notification about new launches and instant information of the app. In e-commerce, automated user specific Push Notifications plays a vital role to market new products arrival, offers, delivery time etc.

The above link explains, how to create a basic Push Notification from scratch in Ionic 5 using PHP .

This tutorial mainly focuses, how to setup your own Push Notification Server using PHP and how to automate the Push Notification, instead of using the Firebase console to send the Push Notification manually. And this tutorial requires the knowledge of PHP and MySQL. Please check, whether you have installed PHP and MySQL on your system.

Before starting this automation work, please make simple working Ionic 5 blank application and proceed further.
Once the basic Starter app is created. Install the required plugin from the below command.

    ionic cordova plugin add phonegap-plugin-push
    npm install @ionic-native/push
   
 And import the necessary dependency in app.module.ts and the home.page.ts
  
 import { Push, PushObject, PushOptions } from '@ionic-native/push/ngx';
 constructor(private push: Push) { }

Now you have created a basic working Push Notification Ionic 5 project using the above method. To automate Push Notification, you need three things. They are
1. Device registration id
2. Firebase Cloud Messaging Server API Key
3. Firebase API URL to send Push Notification

Device Registration ID

Whenever you call the pushObject.on() function, it will generate the Unique registration id. The registration id will look like below.

s_4fdfj6o9:HgS3Hudsd3KwNxzqdSd6ePcHhD3SS91IpKv_DhF3EtPc0nfdfdfwb7owKf2KfdwjQXXqCYoig6fX7s-Zd2bRH3zfdoKqRJi2owJ4R9-HgS3HucuHd-wfPedAdK4VkhkD7LV

You have to save the Device registration id. So import the Http class to the home.page.ts.

import { Http, Response, Headers, RequestOptions } from '@angular/http';
import 'rxjs/add/operator/map';

constructor(public http:Http)

saveDeviceToken(t)
{

this.http.get('http://192.168.0.100/ionic/saveToken.php?token='+t)
.map(res => res.json())
.subscribe(
data => {
alert(JSON.stringify(data));
},
err => {
console.log("Oops!");
}
);
}

The above code calls the Http service(ajax request). Please add the HttpModule to the app.module.ts, if you face the No provider for ConnectionBackend error message.

import { HttpModule } from '@angular/http';
imports: [
BrowserModule,
HttpModule,
IonicModule.forRoot(MyApp)
]

<?php
header('Access-Control-Allow-Origin: *');

if(!empty($_GET))
{
$t=$_GET["token"];
//write your php code to save the registration id to the table.
}
else {
echo "oooooooooooopppppppppppppsssssssssss";
}
?>

![Alt text](https://ampersandacademy.com/fileman/Uploads/tut/cloud_messaging.png "Optional title")

Firebase Cloud Messaging Server API Key

You can get the Firebase Cloud messaging API key under your project settings.


Firebase API URL to send Push Notification

Use the below URL to send Push Notification.

https://fcm.googleapis.com/fcm/send

The above URL will accept only the POST data. So you need to write a PHP code to execute the above URL in POST method. And the URL had many parameters to send Push Notification. They are 

1. message
2. title
3. subtitle
4. tickerText
5. vibrate
6. sound
7. smallIcon 
8. largeIcon

<?php
// API access key from Google API's Console
define( 'API_ACCESS_KEY', 'YOUR-API-ACCESS-KEY-GOES-HERE' );
$registrationIds = array( $_GET['id'] );
// prep the bundle
$msg = array
(
'message'=> 'here is a message. message',
'title'=> 'This is a title. title',
'subtitle'=> 'This is a subtitle. subtitle',
'tickerText'=> 'Ticker text here...Ticker text here...Ticker text here',
'vibrate'=>1,
'sound'=>1,
'largeIcon'=>'large_icon',
'smallIcon'=>'small_icon'
);
$fields = array
('registration_ids'=>$registrationIds,
'data'=>$msg
);

$headers = array
('Authorization: key=' . API_ACCESS_KEY,
'Content-Type: application/json'
);

$ch = curl_init();
curl_setopt( $ch,CURLOPT_URL, 'https://fcm.googleapis.com/fcm/send' );
curl_setopt( $ch,CURLOPT_POST, true );
curl_setopt( $ch,CURLOPT_HTTPHEADER, $headers );
curl_setopt( $ch,CURLOPT_RETURNTRANSFER, true );
curl_setopt( $ch,CURLOPT_SSL_VERIFYPEER, false );
curl_setopt( $ch,CURLOPT_POSTFIELDS, json_encode( $fields ) );
$result = curl_exec($ch );
curl_close( $ch );
echo $result;
?>

Replace the YOUR-API-ACCESS-KEY-GOES-HERE with your Firebase Cloud Messaging Server API Key. I am running this example on my localhost. The URL to send data to the registered device looks like

htttp://localhost/ionic/sendPushNotification.php?id=YOUR_DEVICE_REGISTRATION_ID


That's all. You are done. If you want to send a custom message in the automated manner, then pass the message and device registration id as a parameter to the sendPushNotification.php file.

This Tutorial is been taken reference from the following link for ionic 2. I have made the necessary changes for the ionic 5 version.

https://ampersandacademy.com/tutorials/ionic-framework-version-2/push-notification-automate-using-php

Please feel free to comment or suggest changes.

As it's my first github post.

