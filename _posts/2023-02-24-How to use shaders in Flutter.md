---
title: How to use fragment shaders in Flutter?
date: 2023-02-24 09:12:54 +0100
categories: [Mobile]
tags: [flutter,shaders,loading indicator,progress indicator,painter]
image:
  path: /assets/img/hummingbird.png
  alt: fragment shaders
author: kamil_powalowski
toc: true
---

# How to use fragment shaders in Flutter?

Flutter's rendering engine(engines) supports GLSL fragment shaders. Suppose this sentence is unclear to you; afraid not, it wasn't clear to me a few weeks ago. In this article, you will learn what a GLSL fragment shader is, how to look for ready-to-use shaders, and how to add them to your project.

**What is a shader?** - Shader is a program created to work on a user's GPU, and it uses graphic card construction to optimize parallel processing.

**What is a fragment shader?** - Fragment shader is a program that operates on colors, and it results in information on which color (and opacity) the given point should have.

**What is a GLSL fragment shader?** - GLSL comes from OpenGL Shading Language, a language similar to C used to write shaders.

**How does it work in Flutter?** - Flutter renders its UI using Skia (and Impeller). And Skia has its own Shader Language (SkSL). When you build the app, your GLSL shader complies into the correct format used by the backend runtime.

## Hot to use the GLSL fragment shader in Flutter?

As you probably concluded with preview paragraphs, I'm not an expert in shaders. But I don't have to because there is a page called [Shadertoy BETA](https://www.shadertoy.com). You can find many different shaders there. But please be aware of two things:

1. Not all of them will work for you (SkSL is limited compared to GLSL).
2. Before using something, check for a license or ask a creator.

For this experiment, I selected [Shadertoy](https://www.shadertoy.com/view/7tcSRS). This element looks fantastic and can be used as a loading/progress indicator. Additionally, its code is short, so it will be easy to present what's changed.

![Shaders](assets/video/shaders.mp4)

### 1. Add shader to the project

Copy the code visible on [Shadertoy](https://www.shadertoy.com/view/7tcSRS) and paste it to the new `loading.glsl` file in your project. Shader behaves similarly to assets, so I created a shaders directory and put that file there. Similarly to assets, you have to add them into `pubspec.yaml` under `flutter` section:
```yaml
// ...

flutter:
  uses-material-design: true
  assets:
// ...
    
  shaders:
    - shaders/loading.glsl
```

### 2. Updated shader code

Unfortunately, you can simply copy and paste the shader from the Shadertoy website. SkSL requires the declaration of inputs and outputs and the entry point.

At the top of the `loading.glsl` file, you have to declare what will are the inputs and outputs.
```c
uniform float iTime;
uniform vec2 iResolution;

out vec4 fragColor;
```
In this case, we will provide an elapsed time and the size of the shader. The output is always the same - `vec4` of `fragColor`.
At the end of the file, we must add an entry point - `main` function.
```dart
void main() {
  mainImage(FlutterFragCoord().xy);
}
```
It calls the `mainImage` function (with a modified list of parameters). To use it, you need to import `runtime_effect.glsl` provided by Flutter. Consult complete shader code for given changes.
```c
// Base on https://www.shadertoy.com/view/7tcSRS
#include <flutter/runtime_effect.glsl>

uniform float iTime;
uniform vec2 iResolution;

out vec4 fragColor;

#define hue(v) ( .6 + .6 * cos( 6.3 * (v) + vec4(0, 23, 21, 0) ) )

void mainImage( vec2 fragCoord )
{           
    vec2 uv = (fragCoord.xy - iResolution.xy * 0.5) / iResolution.x; 
          
    float t = iTime*5.;
    
    vec3 rd = vec3(0.);
    
    for(float i = 0.; i < 30.; i += .8){
    
        float tt = t + sqrt(100. - i) * 2.0;        
        vec2 m = vec2(cos(tt), sin(2. * tt) / 3.5)*.3;
        
        float ift = i*.0015;
        float d = smoothstep(.06 - ift, .00 - ift,  length(uv + m));
        
        rd = rd + d * hue(-tt * .33).rgb;       
    }
    
   
    fragColor = vec4(vec3(rd), 1.);
}

void main() {
  mainImage(FlutterFragCoord().xy);
}
```

### 3. Load shader

To load your shader to the application, you should use the [fromAsset](https://api.flutter.dev/flutter/dart-ui/FragmentProgram/fromAsset.html) static method on FragmentProgram class. When this method is called, it also checks the shader and gives you errors when something is wrong. Loading fragment programs can be demanding, so you should remember to store them and reuse them when necessary. I encapsulated it in `LoadingShader` class.
```dart
import 'dart:ui';

class LoadingShader {
  LoadingShader._();

  static Future<LoadingShader> compile() async {
    if (_shader != null) return LoadingShader._();
    final program = await FragmentProgram.fromAsset(
      'shaders/loading.glsl',
    );
    _shader = program.fragmentShader();

    return LoadingShader._();
  }

  static FragmentShader? _shader;

  Shader shader({
    required Size iResolution,
    required double iTime,
  }) {
    return _shader!
      ..setFloat(0, iTime)
      ..setFloat(1, iResolution.width)
      ..setFloat(2, iResolution.height);
  }
}

```
The shader method visible above sets `iTime` and `iResolution` needed by the shader (declared in the previous step). `setFloat` and `setImageSampler` are the only two methods that allow you to pass values between your Flutter code and the shader.

### 4. Create a painter

To display your shader, you will need `CustomPaint`; for that, a class that extends `CustomPainter` is required. What should it do? It should create paint with the provided shader and use provided canvas to draw the output. Since our shader is animated, it will also pass the current time and size of the drawable area to it.
```dart
import 'package:flutter/material.dart';

import 'loading_shader.dart';

class ShaderPainter extends CustomPainter {
  const ShaderPainter(this.shader, this.time);

  final LoadingShader shader;
  final double time;

  @override
  void paint(Canvas canvas, Size size) {
    final paint = Paint()
      ..shader = shader.shader(
        iResolution: size,
        iTime: time,
      );
    canvas.drawRect(
      Rect.fromLTWH(0, 0, size.width, size.height),
      paint,
    );
  }

  @override
  bool shouldRepaint(covariant CustomPainter oldDelegate) => true;
}

```

### Display shader

Now you are ready to build a widget that loads a shader and updates every tick. It can look like something presented here.
```dart
import 'package:flutter/material.dart';
import 'package:flutter/scheduler.dart';

import 'shader_painter.dart';
import 'loading_shader.dart';

class LoadingShaderWidget extends StatefulWidget {
  const LoadingShaderWidget({Key? key}) : super(key: key);

  @override
  State<LoadingShaderWidget> createState() => _LoadingShaderWidgetState();
}

class _LoadingShaderWidgetState extends State<LoadingShaderWidget>
    with SingleTickerProviderStateMixin {
  late final Ticker _ticker;
  LoadingShader? _shader;
  double _time = 0;

  @override
  void initState() {
    super.initState();
    _ticker = createTicker(_tick)..start();
    _loadShader();
  }

  @override
  void dispose() {
    _ticker.dispose();
    super.dispose();
  }

  void _loadShader() async {
    _shader = await LoadingShader.compile();
  }

  void _tick(Duration timestamp) {
    setState(
      () => _time = timestamp.inMicroseconds / Duration.microsecondsPerSecond,
    );
  }

  @override
  Widget build(BuildContext context) {
    final shader = _shader;

    if (shader == null) return const SizedBox();

    final size = MediaQuery.of(context).size;
    return CustomPaint(
      painter: ShaderPainter(shader, _time),
      size: size,
    );
  }
}

```
And that's it. Combine what's described above, and you can display this and many other shaders available online in your Flutter application.

## Summary

At first, looking at that code may be intimidating, but when you start playing with it, you will quickly discover that most of it is the same, no matter what shader you are using, so you can abstract it. I think shaders can be an excellent addition to the Flutter developer toolbelt. You can add stunning effects to your application without increasing the size of the app. If you want to know more, go to [Writing and using fragment shaders](https://docs.flutter.dev/development/ui/advanced/shaders).