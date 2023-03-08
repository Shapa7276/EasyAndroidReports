One liner to find exported content provider from muliple apktool output 

```
grep -r -o -E '<provider.*android:name="[^"]+".*android:exported="true".*>' . | grep -o -E 'android:name="[^"]+"' | cut -d '"' -f 2 | sed 's/^/Exported Content Provider: /'
```
