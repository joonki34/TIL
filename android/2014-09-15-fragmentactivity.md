# FragmentActivity

1)  
A FragmentActivity is a subclass of Activity that was built for the Android Support Package.

The FragmentActivity class adds a couple new methods to ensure compatibility with older versions of Android, but other than that, there really isn't much of a difference between the two. Just make sure you change all calls to getLoaderManager() and getFragmentManager() to getSupportLoaderManager() and getSupportFragmentManager() respectively.

2)  
FragmentActivity gives you all of the functionality of Activity plus the ability to use Fragments which are very useful in many cases, particularly when working with the ActionBar, which is the best way to use Tabs in Android.

If you are only targeting Honeycomb (v11) or greater devices, then you can use Activity and use the native Fragments introduced in v11 without issue. FragmentActivity was built specifically as part of theSupport Library to back port some of those useful features (such as Fragments) back to older devices.