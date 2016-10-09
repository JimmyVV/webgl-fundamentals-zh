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

![rotating line][1]

[click here to open in a separate window][2]

[����������´����д�][2]

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

![resize][3]

[click here to open in a separate window][4]

[����������µĴ����д�][4]

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

![][5]

[click here to open in a separate window][6]

[����������µĴ����д�][6]

Open that in a separate window, size the window, notice it always fills the window.

���µĴ����д򿪣��������ڣ�ע���������������ڡ�

I can hear you asking, why doesn't WebGL set the viewport for us automatically when we change the size of the canvas? The reason is it doesn't know how or why you are using the viewport. You could be rendering to a framebuffer or doing something else that requires a different viewport size. WebGL has no way of knowing your intent so it can't automatically set the viewport for you.

���ܹ�����������ʣ������Ǹı仭����СʱΪʲô WebGL ���Զ�Ϊ���������ӿڣ�������Ϊ����֪������λ�Ϊʲôʹ���ӿڡ��������Ⱦ��֡��������������������Ҫ��ͬ���ӿڴ�С�����顣WebGL �޴ӵ�֪�����ͼ������������Ϊ���Զ��������ӿڴ�С��

If you look at many WebGL programs they handle resizing or setting the size of the canvas in many different ways. If you're curious [here are some of the reasons I think the way described above is preferable][7].

���Ƿ񿴵��ܶ� WebGL ��������಻ͬ�ķ�ʽ������С�������û�����С������������棬[����Ϊ����Щԭ������������������][7]

[2]: http://webglfundamentals.org/webgl/webgl-10x15-canvas-400x300-css.html
[4]: http://webglfundamentals.org/webgl/webgl-resize-canvas.html
[6]: http://webglfundamentals.org/webgl/webgl-resize-canvas-viewport.html
[7]: http://webglfundamentals.org/webgl/lessons/webgl-anti-patterns.html