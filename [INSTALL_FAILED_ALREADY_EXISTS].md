# via adb install, Failure [INSTALL_FAILED_ALREADY_EXISTS] when I tried to update my application

[Failure [INSTALL_FAILED_ALREADY_EXISTS]](https://stackoverflow.com/questions/4449540/failure-install-failed-already-exists-when-i-tried-to-update-my-application)

when I tried to update my applcation with new version that has same signature as previous one, shows above error.
What I am missing?

If you install the application on your device via adb install you should look for the reinstall option which should 
be -r. So if you do adb install -r you should be able to install without uninstalling before.
```
I came here because I wanted to know whether adb install -r would remove first and then install or upgrade my app. 
Although adb's description is not very clear (-r: replace existing application), adb install -r does indeed upgrade 
your app and does not remove your app data. Therefore suitable to test upgrading your app (which is the information 
I was looking for).
```
