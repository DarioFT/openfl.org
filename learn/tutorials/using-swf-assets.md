---
title: Using SWF Assets
---

## Introduction

OpenFL includes support for decoding many features of SWF assets generated from editors such as Flash Professional and Adobe Animate.

At compile time, OpenFL is able to convert SWF assets into a faster, more optimal format compatible with HTML5 and native platforms, while using the original SWF content if you target Flash Player.

## Creating SWF Content

Using any version of Flash Professional or Adobe Animate, you can publish SWF assets to be used by OpenFL. Supported features include Shapes, SimpleButtons, MovieClips, many drawing operations, and where possible, masks, scrollRects, filters and blend modes.

Certain features that rely on ActionScript, such as frame scripts, the Text Layout Framework or inverse kinematics are not supported. The current releases of Flash Professional include an "Export as Bitmap" feature, which is useful for certain types of rendering.

Sounds, shape tweens or new motion tweens (not "classic tweens") are not currently supported, but some of these features could be supported with contributions. Fonts are supported if they are embedded separately.

## Embedding a SWF Library

If you do not already have a project created, you can use a command like the following:

{% highlight bash %}
openfl create project UsingSWFAssets
{% endhighlight %}

You can find many of the same concepts in the "SimpleSWFLayout" sample, also, if you prefer:

{% highlight bash %}
openfl create SimpleSWFLayout
{% endhighlight %}

Once the SWF library is included, you can add assets using the following tag:

{% highlight bash %}
<library path="to/my.swf" />
{% endhighlight %}

This will use a _default_ ID value, matching the file name (in this case "my").

You can set this yourself using the `id` attribute if you prefer:

{% highlight bash %}
<library path="to/my.swf" id="swf-library" />
{% endhighlight %}

There are additional features as well. Adding a `preload` attribute can help you automatically load a SWF library as part of the preload process (more on that later), and `generate` will attempt to create classes for every "Export for ActionScript" class in the SWF, so you can `new MySymbolClass ()` in your code.

{% highlight bash %}
<library path="to/my.swf" preload="true" generate="true" />
{% endhighlight %}

## Loading a SWF Library

Unless you have used `preload="true"` in your `<library />` tag, before you are able to use content from a SWF library, you must load the library first:

{% highlight haxe %}
Assets.loadLibrary ("swf-library", function (_) {
	
	trace ("SWF library loaded");
	
});
{% endhighlight %}

## Instantiating a SWF Symbol

Once a library is loaded, you can access named symbols from the library using the following code:

{% highlight haxe %}
var clip = Assets.getMovieClip ("swf-library:MySymbolClass");
{% endhighlight %}

If the `generate` attribute was used, each symbol class should be available to instantiate normally:

{% highlight haxe %}
var clip = new MySymbolClass ();
{% endhighlight %}

Code completion should be available for all generated classes.

If you want to access symbols from multiple SWF libraries, change the prefix before the colon (":") in the `Assets.getMovieClip` call. If you would prefer to load the entire SWF timeline, and not a child clip, use an empty value for the symbol name, like this:

{% highlight haxe %}
var clip = Assets.getMovieClip ("swf-library:");
{% endhighlight %}

## Standard Example

_project.xml_

{% highlight bash %}
<?xml version="1.0" encoding="utf-8"?>
<project>
	
	<meta title="Using SWF Assets" package="org.openfl.tutorials.usingswfassets" version="1.0.0" company="OpenFL" />
	<app main="Main" path="Export" file="UsingSWFAssets" />
	
	<source path="Source" />
	
	<haxelib name="openfl" />
	
	<library path="Assets/library.swf" />
	
	<assets path="Assets" rename="assets" exclude="*.swf" />
	
</project>
{% endhighlight %}

_Main.hx_

{% highlight haxe %}
package;


import openfl.display.Sprite;
import openfl.Assets;


class Main extends Sprite {
	
	
	public function new () {
		
		super ();
		
		Assets.loadLibrary ("library", function (_) {
			
			var clip = Assets.getMovieClip ("library:MySymbolClass");
			addChild (clip);
			
		});
		
	}
	
	
}
{% endhighlight %}

## Using "preload" and "generate"

_project.xml_

{% highlight bash %}
<?xml version="1.0" encoding="utf-8"?>
<project>
	
	<meta title="Using SWF Assets" package="org.openfl.tutorials.usingswfassets" version="1.0.0" company="OpenFL" />
	<app main="Main" path="Export" file="UsingSWFAssets" />
	
	<source path="Source" />
	
	<haxelib name="openfl" />
	
	<library path="Assets/library.swf" preload="true" generate="true" />
	
	<assets path="Assets" rename="assets" exclude="*.swf" />
	
</project>
{% endhighlight %}

_Main.hx_

{% highlight haxe %}
package;


import openfl.display.Sprite;
import openfl.Assets;


class Main extends Sprite {
	
	
	public function new () {
		
		super ();
		
		var clip = new MySymbolClass ();
		addChild (clip);
		
	}
	
	
}
{% endhighlight %}
