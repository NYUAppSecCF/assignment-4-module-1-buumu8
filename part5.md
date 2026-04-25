1. Update AndroidManifest.xml
   You need to remove the location permissions and ensure no other unnecessary services are requested.

Remove these lines:

XML
<uses-permission android:name="android.permission.ACCESS_FINE_LOCATION"/>
<uses-permission android:name="android.permission.ACCESS_COARSE_LOCATION"/> 2. Update ProductScrollingActivity.kt and CardScrollingActivity.kt
These files are currently packed with location and sensor logic. You need to strip them down to their core functionality (browsing/listing cards).

Remove: \* The SensorEventListener and LocationListener interfaces.

The SensorManager and mAccel initialization and variable declarations.

The calls to locationManager.requestLocationUpdates.

All methods related to sensors and location: onSensorChanged, onAccuracyChanged, onLocationChanged.

The onResume and onPause overrides that manage the sensor listener.

Keep: Only the logic related to Retrofit (fetching products/cards) and the RecyclerView setup.

3. Cleanup: UserInfo.kt and Metrics Reporting
   Since you are no longer collecting sensor or location data, the UserInfo interface and its related model are likely obsolete.

Delete/Comment out: The postInfo function in your UserInfo interface and any code in your activities that creates a UserInfoContainer and attempts to send it to the server.
