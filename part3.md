# File-by-File updates

In each of the files you listed, you will find a line that defines the Retrofit base URL. You need to change the protocol from http to https.

Example:

Locate: .baseUrl("http://appsec.moyix.net")

Change to: .baseUrl("https://appsec.moyix.net")

# AndroidManifest

Remove: android:usesCleartextTraffic="true"
