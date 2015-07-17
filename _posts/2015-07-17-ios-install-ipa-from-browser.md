---
layout: post
tagline: "install ipa from web server(local publish)"
category : ios
tags : [ios]
---
{% include JB/setup %}

#### steps

1,create install plist install.xml (put it to a https, if don't have one, use github)
2,create install.html
3,visit install.html to install

#### install.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
<dict>
	<key>items</key>
	<array>
		<dict>
			<key>assets</key>
			<array>
				<dict>
					<key>kind</key>
					<string>software-package</string>
					<key>url</key>
					<string>$url_ipa</string>
				</dict>
				<dict>
					<key>kind</key>
					<string>full-size-image</string>
					<key>needs-shine</key>
					<true/>
					<key>url</key>
					<string>$url_icon_large</string>
				</dict>
				<dict>
					<key>kind</key>
					<string>display-image</string>
					<key>needs-shine</key>
					<true/>
					<key>url</key>
					<string>$url_icon_small</string>
				</dict>
			</array>
			<key>metadata</key>
			<dict>
				<key>bundle-identifier</key>
				<string>$appid</string>
				<key>bundle-version</key>
				<string>$appver</string>
				<key>kind</key>
				<string>software</string>
				<key>subtitle</key>
				<string>$appsubtitle</string>
				<key>title</key>
				<string>$apptitle</string>
			</dict>
		</dict>
	</array>
</dict>
</plist>
```

#### install.html

```javascript
<script>
var url="https://url_to_install.xml";
window.location="itms-services://?action=download-manifest&url=" + encodeURIComponent(url);
</script>
```
