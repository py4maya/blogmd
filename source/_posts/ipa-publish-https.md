---
title: 通过https 站点分发ipa 安装包
date: 2018-01-12 10:01:07
tags: [https,ipa]
categories: 开发
---

* 配置下载页:
```html
 <a href="itms-services://?action=download-manifest&url=https://......./test.plist">下载测试版</a>
```

* 编写plist文件:
```shell
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
					<string>https://.....................ipa</string>
				</dict>
				<dict>
					<key>kind</key>
					<string>full-size-image</string>
					<key>needs-shine</key>
					<false/>
					<key>url</key>
					<string>https://............................icon.png</string>
				</dict>
				<dict>
					<key>kind</key>
					<string>display-image</string>
					<key>needs-shine</key>
					<false/>
					<key>url</key>
					<string>https://...........................icon@2x.png</string>
				</dict>
			</array>
			<key>metadata</key>
			<dict>
				<key>bundle-identifier</key>
				<string>com.package.yours</string>
				<key>bundle-version</key>
				<string>2.0.2</string>
				<key>kind</key>
				<string>software</string>
				<key>subtitle</key>
				<string>测试ipa</string>
				<key>title</key>
				<string>测试ipa安装</string>
			</dict>
		</dict>
	</array>
</dict>
</plist>
```

* 最后放好你的ipa，用ios 手机 safri 浏览器打开你的 发布页，点击 下载测试版 就可以安装了.
