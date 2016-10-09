# WebGL Setup and Installation
# WebGL�������밲װ

Techincally you don't need anything other than a web browser to do WebGL development. Go to [jsfiddle.net][1] or [jsbin.com][2] or [codepen.io][3] and just start applying the lessons here.

�Ӽ����Ͻ����������Ҫһ����ҳ������� WebGL ���������Ե� [jsfiddle.net][1]�� [jsbin.com][2] ���� [codepen.io][3] Ӧ������Ľ̳����ݡ�

On all of them you can reference external scripts by adding a <script src="..."></script> tag pair if you want to use external scripts.

�������ʹ���ⲿ�ű������е������㶼����ͨ�������ⲿ�ű���ǩ <script src="..."> ��ʹ�á�

Still, there are limits. WebGL has stronger restrictions than Canvas2D for loading images which means you can't easily access images from around the web for your WebGL work. On top of that it's just faster to work with everything local.

���������������ơ�WebGL ����� Canvas2D �ڼ���ͼƬ�����и�ǿ��Լ��������ζ������ WebGL �����в������׵ķ������������ͼƬ������Ҫ���ǣ����뱾����Դ��������졣

Let's assume you want to run and edit the samples on this site. The first thing you should do is download the site. [You can download it here][4].

�����Ǽ��������������վ���кͱ༭ʾ�����롣������Ӧ�����������վ��[���������������][4]��

![download][5]

Unzip the files into some folder.
���ļ����н�ѹ�ļ���

## Using a small simple easy Web Server
## ʹ��һ���򵥵� Web ������

Next up you should install a small web server. I know "web server" sounds scary but the truth is [web servers are actually extremely simple][6].

��һ����Ӧ�ð�װһ��С�� web ����������֪����web �������������������ˣ�����ʵ�� [web ������ʵ���Ϸǳ���][6]��

If you're on Chrome here's a simple solution. [Here's a small chrome extension that's a web server][7].

������� Chrome����һ���򵥵Ľ��������[������һ��С�� Web ������ chrome ��չ][7]��

![chrome extension][8]

Just point it at the folder where you unzipped the files and click one of the web server URLs.

ֻ������ѹ�˵��ļ����е��ļ�����������һ�� web �������� URL��

If you're not on chrome or if you don't want to use the extension another way is to use [node.js][9]. Download it, install it, then open a command prompt / console / terminal window. If you're on Windows the installer will add a special "Node Command Prompt" so use that.

����㲻��ʹ�� chrome �����㲻��ʹ����չ����һ�ַ�����ʹ�� [node.js][9]�����أ���װ��Ȼ���������/����̨/�ն˴����д� ��������� Windows �а�װ���������һ������� ��Node Command Prompt�� �Թ�ʹ�á�

Then install the http-server by typing��

Ȼ��ͨ�����������밲װ http-server��

```
npm -g install http-server
```

If you're on OSX use
������� OSX ��ʹ��

```
sudo npm -g install http-server
```

Once you've done that type

��װ���֮������

```
http-server path/to/folder/where/you/unzipped/files
```

It should print something like

�����Ӧ�ô�ӡ�����ƵĶ���

![http-server][10]

Then in your browser go to http://localhost:8080.

Ȼ������������������ http://localhost:8080��

If you don't specify a path then http-server will server the current folder.

�����û��ָ��·����http-server ������ָ��ǰ���ļ��С�

## Using your Browsers Developer Tools
## ʹ���������������߹���

Most browser have extensive developer tools built in.
�������������������˿����߹��ߡ�

![developer tools][11]

[Docs for Chrome's are here][12], [Firefox's are here][13].
Chrome �Ĳο��ĵ������Firefox �������

Learn how to use them. If nothing else always check the JavaScript console. If there is an issue it will often have an error message. Read the error message closely and you should get a clue where the issue is.

ѧϰ���ʹ�����ǡ����û����������һֱ��� JavaScript ����̨����������⣬������ʾ������Ϣ����ϸ�Ķ�������Ϣ�����õ������������

![chrome console][14]

## WebGL Helpers
## WebGL ����

There are various WebGL Inspectors / Helpers. [Here's one for Chrome][15].

�����и��ָ����� WebGL ����Ա/���֡�[����� Chrome ��][15]��

![WebGL Inspectors / Helper][16]

They may or may not be helpful. Most of them are designed for animated samples and will capture a frame and let you see all the WebGL calls that made that frame. That's great if you already have something working or if you had something working and it broke. But it's not so great if your issue is during initialization which they don't catch or if you're not using animation, as in drawing something every frame. Still they can be very useful. I'll often click on a draw call, and check the uniforms. If I see a bunch of NaN (NaN = Not a Number) then I can usually track down the code that set that uniform and find the bug.

���ǿ��ܻ�Ҳ���ܲ����а����������еĴ������רΪ����ʾ���ģ����ǲ���չʾһ������ WebGL �Ŀ�������Ѿ�����һЩʾ����Ч�˻�����һЩʾ����Ч����ʧ���ˣ���ܺá��������������ڳ�ʼ�����������ֲ��ܲ����ܻ��������ڻ��ƿ��ʱû��ʹ�ö�������Ͳ��Ǻܺ��ˡ���������Ȼ���Էǳ����õġ��Ҿ�������һ�����ã��Լ�� uniforms������ҿ���һ�� NaN��NaN = Not a Number���ҾͿ��Ը��ٴ��룬���� uniform ���ҳ�����

## Inspect the Code
## ������

Also always remember you can inspect the code. You can usually just pick view source

��Զ��ס����Լ����롣�����ѡ��ֻ�鿴Դ�롣

![helper][17]

Even if you can't right click a page or if the source is in a separate file you can always view the source in the devtools

��ʹ�㲻���һ�ҳ�����Դ������һ���������ļ��У�����Ȼ�����ڿ����߹����в鿴Դ�롣

![devtools][18]

## Get Started
## ��ʼ

Hopefully that helps you get started. [Now back to the lessons][19].

ϣ�����������㿪ʼ��[���ڻص��̳�][19]��

[1]: https://jsfiddle.net/
[2]: http://jsbin.com/
[3]: http://codepen.io/
[4]: https://github.com/greggman/webgl-fundamentals/
[5]: http://webglfundamentals.org/webgl/lessons/resources/download-webglfundamentals.gif
[6]: http://games.greggman.com/game/saving-and-loading-files-in-a-web-page/
[7]: https://chrome.google.com/webstore/detail/web-server-for-chrome/ofhbbkphhbklhfoeikjpcbhemlocgigb?hl=en
[8]: http://webglfundamentals.org/webgl/lessons/resources/chrome-webserver.png
[9]: https://nodejs.org/en/
[10]: http://webglfundamentals.org/webgl/lessons/resources/http-server-response.png
[11]: http://webglfundamentals.org/webgl/lessons/resources/chrome-devtools.png
[12]: https://developers.google.com/web/tools/chrome-devtools/
[13]: https://developer.mozilla.org/en-US/docs/Tools
[14]: http://webglfundamentals.org/webgl/lessons/resources/javascript-console.gif
[15]: https://benvanik.github.io/WebGL-Inspector/
[16]: https://benvanik.github.io/WebGL-Inspector/images/screenshots/1-Trace.gif
[17]: http://webglfundamentals.org/webgl/lessons/resources/view-source.gif
[18]: http://webglfundamentals.org/webgl/lessons/resources/devtools-source.gif
[19]: http://webglfundamentals.org/