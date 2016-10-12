## WebGL Resizing the Canvas.
## WebGL ������С
  
Here's what you need to know to change the size of the canvas.
��������Ҫ֪���ı仭���Ĵ�С�ķ�����

Every canvas has 2 sizes. The size of its drawingbuffer. This is how many pixels are in the canvas. The second size is the size the canvas is displayed. CSS determines the size the canvas is displayed.

ÿһ�� canvas ����2����С����һ����С�� drawingbuffer�����ڻ������ж��ٸ����ء��ڶ�����С�� canvas ��ʾ�Ĵ�С��CSS �����˻�����ʾ�Ĵ�С��

You can set the size of the canvas's drawingbuffer in 2 ways. One using HTML

�������ַ�ʽ�������� canvas �� drawingbuffer �Ĵ�С������һ����ʹ�� HTML

```
<canvas id="c" width="400" height="300"></canvas>
```

The other using JavaScript

��һ����ʹ�� JavaScript

```
<canvas id="c" ></canvas>
```

JavaScript

JavaScript

```
var canvas = document.getElementById("c");
canvas.width = 400;
canvas.height = 300;
```

As for setting a canvas's display size if you don't have any CSS that affects the canvas's display size the display size will be the same size as its drawingbuffer. So in the 2 examples above the canvas's drawingbuffer is 400x300 and its display size is also 400x300.

�������û�������ʾ��С�������û��ʹ���κ� CSS �ı� canvas ����ʾ��С��������ʾ��С����� drawingbuffer �Ĵ�Сһ�����������������ʾ���� canvas �� drawingbuffer ��С�� 400x300����ʾ��СҲ�� 400x300��

Here's an example of a canvas whose drawingbuffer is 10x15 pixels that is displayed 400x300 pixels on the page

���ʾ���е� canvas �� drawingbuffer �� 10x15 ���أ���ҳ���ϵ���ʾ��С�� 400x300 ���ء�

```
<canvas id="c" width="10" height="15" style="width: 400px; height: 300px;"></canvas>
```

or for example like this

���������ʾ��

```
<style>
#c {
  width: 400px;
  height: 300px;
}
</style>
<canvas id="c" width="10" height="15"></canvas>
```

If we draw a single pixel wide rotating line into that canvas we'll see something like this

��������ڻ����ϻ�һ�������ؿ����ת�ߣ����ǻῴ������Ч��

<iframe class="webgl_example" style=" " src="/webgl/resources/editor.html?url=/webgl/lessons/../webgl-10x15-canvas-400x300-css.html"></iframe>

[click here to open in a separate window][1]

[����������´����д�][1]

Why is it so blurry? Because the browser takes our 10x15 pixel canvas and stretches it to 400x300 pixels and generally it filters it when it stretches it.

Ϊʲô�����ģ������Ϊ����������� 10x15 ���صĻ������쵽 400x300 ���أ�ͨ������������컭��ʱʹͼ��ʧ�档

So, what do we do if, for example, we want the canvas to fill the window? Well, first we can get the browser to stretch the canvas to fill the window with CSS. Example

���ԣ����������Щʲô�����磬����ϣ�������ܹ��������ڣ��ţ��������ǿ���ͨ�� CSS ����������컭�����������ڡ�ʾ��

```
<html>
  <head>
    <style>
      /* remove the border */
      /* �Ƴ��߿� */
      body {
        border: 0;
        background-color: white;
      }
      /* make the canvas the size of the viewport */
      /* ���� canvas ��СΪ�ӿڴ�С */
      canvas {
        width: 100vw;
        height: 100vh;
        display: block;
      }
    <style>
  </head>
  <body>
    <canvas id="c"></canvas>
  </body>
</html>
```

Now we just need to make the drawingbuffer match whatever size the browser has stretched the canvas. We can do that using `clientWidth` and `clientHeigh`t which are properties every element in HTML has that let JavaScript check what size that element is being displayed.

��������ֻ��Ҫ�� drawingbuffer ƥ������ߴ�����컭��������������ǿ���ʹ��ÿһ�� HTML Ԫ�ض��е����� `clientWidth` �� `clientHeight`���� JavaScript ���Ԫ�ص���ʾ��С��

Most WebGL apps are animated so let's call this function just before we render so it will always adjust the canvas to our desired size just before drawing.

����� WebGL Ӧ�����ж����ģ��������������Ⱦ֮ǰ��������������������������ڻ滭֮ǰ������������������ĳߴ硣

```
function drawScene() {
   resize(gl.canvas);
 
   ...
```

And here's that

Ч������

<iframe class="webgl_example" style=" " src="/webgl/resources/editor.html?url=/webgl/lessons/../webgl-resize-canvas.html"></iframe>

[click here to open in a separate window][2]

[����������µĴ����д�][2]

Hey, something is wrong? Why is the line not covering the entire area?

�٣���ʲô���ˣ�Ϊʲôû�и�����������

The reason is when we resize the canvas we also need to call `gl.viewport` to set the viewport. `gl.viewport` tells WebGL how to convert from clipspace (-1 to +1) back to pixels and where to do it within the canvas. When you first create the WebGL context WebGL will set the viewport to match the size of the canvas but after that it's up to you to set it. If you change the size of the canvas you need to tell WebGL a new viewport setting.

������Ϊ���ǵ���������ͬʱ��Ҫ���� `gl.viewport` �������ӿڡ�`gl.viewport` ���� WebGL �ڻ�������δӲü��ռ䣨-1 �� +1��ת�������ؿռ䡣�����״δ��� WebGL �����ģ�WebGL ������ƥ�仭���ߴ���ӿڣ���֮�����ĳߴ��ȡ����������á������ı��˻�����С������Ҫ���� WebGL �µ��ӿ����á�

Let's change the code to handle this. On top of that, since the WebGL context has a reference to the canvas let's pass that into resize.

������һ��ı����������������ɡ�����Ҫ���ǣ���Ϊ WebGL �������жԻ��������ã����ǿ���ͨ����һ���õ����ߴ硣

```
function drawScene() {
   resize(gl.canvas);
 
   gl.viewport(0, 0, gl.canvas.width, gl.canvas.height);
   ...
```

Now it's working.

�������������ˡ�

<iframe class="webgl_example" style=" " src="/webgl/resources/editor.html?url=/webgl/lessons/../webgl-resize-canvas-viewport.html"></iframe>

[click here to open in a separate window][3]

[����������µĴ����д�][3]

Open that in a separate window, size the window, notice it always fills the window.

���µĴ����д򿪣��������ڣ�ע���������������ڡ�

I can hear you asking, why doesn't WebGL set the viewport for us automatically when we change the size of the canvas? The reason is it doesn't know how or why you are using the viewport. You could be rendering to a framebuffer or doing something else that requires a different viewport size. WebGL has no way of knowing your intent so it can't automatically set the viewport for you.

���ܹ�����������ʣ������Ǹı仭����СʱΪʲô WebGL ���Զ�Ϊ���������ӿڣ�������Ϊ����֪������λ�Ϊʲôʹ���ӿڡ��������Ⱦ��֡��������������������Ҫ��ͬ���ӿڴ�С�����顣WebGL �޴ӵ�֪�����ͼ������������Ϊ���Զ��������ӿڴ�С��

If you look at many WebGL programs they handle resizing or setting the size of the canvas in many different ways. If you're curious [here are some of the reasons I think the way described above is preferable][4].

���Ƿ񿴵��ܶ� WebGL ��������಻ͬ�ķ�ʽ������С�������û�����С������������棬[����Ϊ����Щԭ������������������][4]

**����֪ʶ**

----------

## How do I handle Retina or HD-DPI displays?

## ����δ��� Retina ���� HD-DPI ����ʾ����

When you specify a size in CSS or Canvas by pixels those are called CSS pixels which may or may not be actual pixels. Most current smartphones have what's called a high-definition DPI display (HD-DPI) or as Apple calls it a "Retina Display". For text and most CSS styling the browser can automatically render HD-DPI graphics but for WebGL, since you're drawing the graphics it's up to you to render at a higher resolution if you want your graphics to be "HD-DPI" quality. 
To do that we can look at the window.devicePixelRatio value. This value tells us how many real pixels equals 1 CSS pixel. We can change our resize function to handle that like this.

������ CSS ���� Canvas ��������ָ����С����Щ����Ϊ CSS pixels��������Ҳ���ܲ���ʵ�ʵ����ء����ڴ���������ֻ����Ǹ���ֱ��ʣ�HD-DPI������ƻ����֮Ϊ��Retina Display��������������е��ı��ʹ���� CSS ��ʽ���ܹ��Զ����� HD-DPI ͼ����Ⱦ�������� WebGL�������ϣ��ͼ���ǡ�HD-DPI�������ģ���ȡ��������һ�����ߵķֱ����л���ͼ�Ρ�Ϊ�����ǿ��Բ鿴 window.devicePixelRatio ��ֵ�����ֵ�������Ƕ��� real pixels ���� 1 CSS pixel�����ǿ��Ըı� resize ���������������⡣

```
function resize(gl) {
  var realToCSSPixels = window.devicePixelRatio || 1;

  // Lookup the size the browser is displaying the canvas in CSS pixels
  // �����������չʾ�� canvas �� CSS pixels
  // and compute a size needed to make our drawingbuffer match it in
  // device pixels.
  // ���� drawingbuffer ƥ�� device pixels �ĳߴ硣
  var displayWidth  = Math.floor(gl.canvas.clientWidth  * realToCSSPixels);
  var displayHeight = Math.floor(gl.canvas.clientHeight * realToCSSPixels);

  // Check if the canvas is not the same size.
  // ��� canvas �ߴ��Ƿ�һ����
  if (gl.canvas.width  != displayWidth ||
      gl.canvas.height != displayHeight) {

    // Make the canvas the same size
    // �ǻ�����С��ͬ
    gl.canvas.width  = displayWidth;
    gl.canvas.height = displayHeight;
  }
}
```
If you have an HD-DPI display, for example if you view this page on your smartphone you should notice the line below is thinner than the one above which didn't adjust for HD-DPI displays

������� HD-DPI ��ʾ�������磬���������ֻ��ϲ鿴���ҳ�棬���ע�⵽������߱�����û�н��� HD-DPI ��ʾ�������߸�ϸ��

<iframe class="webgl_example" style=" " src="/webgl/resources/editor.html?url=/webgl/lessons/../webgl-resize-canvas-hd-dpi.html"></iframe>

[click here to open in a separate window][5]

[����������µĴ����д�][5]

Whether you really want to adjust for HD-DPI is up to you. On iPhone4 or iPhone5 window.devicePixelRatio is 2 which means you'll be drawing 4 times as many pixels. I believe on an iPhone6Plus that value is 3 which means you'd be drawing 9 times as many pixels. That can really slow down your program. In fact it's a common optimization in games to actually render less pixels than are displayed and let the GPU scale them up. It really depends on what your needs are. If you're drawing a graph for printing you might want to support HD-DPI. If you're making a game you might not or you might want to give the user the option to turn support on or off if their system is not fast enough to draw so many pixels.

�Ƿ���� HD-DPI ����ȡ�����㣬�� iPhone4 ���� iPhone5 �� window.devicePixelRatio Ϊ 2����ζ���㽫����4�������ء��������� iPhone6 Plus ��ֵ�� 3������ζ���㽫��9�������ء����ʹ��ĳ����óٶۡ�ʵ������Ⱦ����Ҫ��ʾ�ĸ��ٵ�����Ȼ���� GPU �Ŵ�������Ϸ�г������Ż��������ȡ����������������������ڻ�һ��ӡˢ��ͼ����ܻ�ϣ��֧�� HD-DPI���������������Ϸ�����û���ϵͳû���㹻����ٶ�ȥ���Ƹ��������ʱ�������ϣ�����û�ѡ���Ƿ���֧�� HD-DPI ��ѡ�

[1]: http://webglfundamentals.org/webgl/webgl-10x15-canvas-400x300-css.html
[2]: http://webglfundamentals.org/webgl/webgl-resize-canvas.html
[3]: http://webglfundamentals.org/webgl/webgl-resize-canvas-viewport.html
[4]: http://webglfundamentals.org/webgl/lessons/webgl-anti-patterns.html
[5]: http://webglfundamentals.org/webgl/webgl-resize-canvas-hd-dpi.html