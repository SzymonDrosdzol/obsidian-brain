Here's the script for tracking calls of functions. Created when working on [[root.research.android.oneplus.auto-open-google-play]]

```javascript
Java.perform(function () {
    const jumpActivity = Java.use("com.oplus.games.ExternalJumpActivity");

    // Hook onCreate
    jumpActivity.onCreate.overload("android.os.Bundle").implementation = function (bundle) {
        console.log("[*] ExternalJumpActivity.onCreate called");

        const data = this.getIntent().getData();
        const paramNames = data.getQueryParameterNames();
        const it = paramNames.iterator();

        while (it.hasNext()) {
            const key = it.next();
            const val = data.getQueryParameter(key);
            console.log("    → " + key + ": " + val);
        }
        this.onCreate(bundle);
    };
});

Java.perform(function () {
    const Uri = Java.use("android.net.Uri");

    Uri.parse.overload('java.lang.String').implementation = function (uriStr) {
        console.log("[*] Uri.parse() called");
        console.log("    → Input: " + uriStr);

        // Optional: print a short Java stack
        const stack = Java.use("android.util.Log").getStackTraceString(Java.use("java.lang.Throwable").$new());
        console.log("    ↳ Stack:\n" + stack.split("\n").slice(0, 6).join("\n"));

        return this.parse(uriStr); // proceed as normal
    };
});
```