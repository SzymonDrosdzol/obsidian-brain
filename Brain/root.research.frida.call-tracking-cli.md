As it turns out, one can track calls straight with a CLI command. The greatest part are wildcards:
```
frida-trace -U com.google.android.apps.messaging -i Java_com_google_chat_*
```
Taken from [[root.research.sources.reverse_how_to_find_fully_remote_bugs_with_reverse_engineering]]
