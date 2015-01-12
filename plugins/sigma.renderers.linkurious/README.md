sigma.renderers.linkurious
==================

Plugin developed by [Sébastien Heymann](https://github.com/sheymann) for [Linkurious](https://github.com/Linkurious) and published under the licence [GNU GPLv3](LICENSE) unless otherwise noticed by Linkurious.

Special thanks to [Julian Bilcke](https://github.com/jbilcke).

Contact: seb@linkurio.us

---
## General
This plugin overrides default renderers to provide more powerful renderers.Nodes take the following attributes:
- `active`: *boolean* (see Compatibility below)
- `active_color`: *string* (see Compatibility below)
- `type`: *string*
- `icon`: *object*
- `image`: *object*

Edges take the following attributes:
- `type`: *string*
- `active`: *boolean* (see Compatibility below)
- `active_color`: *string* (see Compatibility below)

See the following [example code](../../examples/renderers-linkurious.html) and [settings](settings.js) for full usage.

To use, include all .js files under this folder. The renderers will be used automatically.

## Shapes

You can assigning shapes to nodes by using the `type` attribute:

```javascript
mySigma.graph.addNode({
  type: 'star'
});
```

### List of shapes

#### type: 'circle'

Draw a disc shaped node (you can also use 'disc', which is an alias).

```javascript
{
  type: 'circle'
}
```

#### type: 'cross'

Draw a `+` shaped cross.

```javascript
{
  type: 'cross',
  star: {
    lineWeight: 0.08, // default: 0.08
    rotate: Math.PI / 180 // must be in radians. Default: 0
  }
}
```

#### type: 'diamond'

Draw a diamond. This is basically a square with a 45° rotation.

```javascript
{
  type: 'diamond'
}
```

#### type: 'equilateral'

Draw an equilateral triangle.

```javascript
{
  type: 'equilateral',
  triangle: {
    rotate: Math.PI / 180 // must be in radians. Default: 0
  }
}
```

#### type: 'square'

Draw a square. You can rotate it if needed.

```javascript
{
  type: 'square',
  square: {
    rotate: Math.PI / 180 // must be in radians. Default: 0
  }
}
```

#### type: 'star'

Draw a star. Example:

```javascript
{
  type: 'star',
  star: {
    numPoints: 6, // default: 5
    rotate: Math.PI / 180 // must be in radians. Default: 0
  }
}
```

Note: defining an inner star radius is not yet supported in WebGL mode.


## Images

You can add an image to any node, simply by adding `node.image` property, with the following content:
````javascript
node.image = {
  url: /* mandatory image URL */,
  clip: /* Ratio of image clipping disk compared to node size (def 1.0) - see example to how we adapt this to differenmt shapes */,
  scale: /* Ratio of how to scale the image, compared to node size, default 1.0 */,
  w: /* numeric width - important for correct scaling if w/h ratio is not 1.0 */,
  h: /* numeric height - important for correct scaling if w/h ratio is not 1.0 */
}
````

Notes:

- Images must come from trusted URLs: eg. same host, or permissive CORS policy (see below).
- The shape type will then be applied to crop the image.
- The `w` and `h` are not supported in WebGL mode.

## Font-icons (Canvas only)

Custom webfonts are supported!

You just need to make sure your custom font is actually loaded before actually using it in a graph, using the W3C CSS Font Loading API. You can use a shim like [zachleat/fontfaceonload](https://github.com/zachleat/fontfaceonload) or [bramstein/fontloader](https://github.com/bramstein/fontloader) to support non-evergreen browsers.

The syntax to use a webfont is simple:

```javascript
{
  icon: {
    font: 'Arial', // or 'FontAwesome' etc..
    content: '☺', // or custom fontawesome code eg. "\uF129"
    scale: 0.7, // 70% of node size
    color: '#ffffff' // foreground color (white)
  }
}
```

## Settings

 * **defaultLabelActiveColor**
   * type: *string*
   * default value: `#000`

 * **activeFont**
   * The active node's label font. If not specified, will heritate the `font` value.
   * type: *string*
   * default value: ``

 * **activeFontStyle**
   * Example: `bold`
   * type: *string*
   * default value: ``

 * **labelActiveColor**
   * Indicates how to choose the labels color of active nodes.
   * type: *string*
   * default value: `default`
   * available values: `node`, `default`

 * **nodeActiveColor**
   * Indicates how to choose the active nodes color.
   * type: *string*
   * default value: `node`
   * available values: `node`, `default`

 * **defaultNodeActiveColor**
   * type: *string*
   * default value: `rgb(236, 81, 72)`

 * **edgeActiveColor**
   * Indicates how to choose the active nodes color.
   * type: *string*
   * default value: `edge`
   * available values: `edge`, `default`

 * **defaultEdgeActiveColor**
   * type: *string*
   * default value: `rgb(236, 81, 72)`

 * **nodeBorderColor** (Canvas only)
   * Indicates how to choose the nodes border color.
   * type: *string*
   * default value: `node`
   * available values: `node`, `default`

 * **nodeOuterBorderColor** (Canvas only)
   * Indicates how to choose the nodes outer border color.
   * type: *string*
   * default value: `node`
   * available values: `node`, `default`

 * **outerBorderSize** (Canvas only)
   * The size of the outer border of hovered and active nodes.
   * type: *number*
   * default value: `0`

 * **defaultNodeOuterBorderColor** (Canvas only)
   * The default hovered and active node outer border's color.
   * type: *string*
   * default value: `#000`

 * **iconThreshold**
   * The minimum size a node must have to see its icon displayed (no effect on WebGL renderer).
   * type: *number*
   * default value: `8`

 * **imageThreshold**
   * The minimum size a node must have to see its image displayed (no effect on WebGL renderer).
   * type: *number*
   * default value: `8`

 * **imgCrossOrigin**
   * Controls the security policy of the image loading, from the browser's side. Please read [this page](https://developer.mozilla.org/en-US/docs/Web/HTML/CORS_enabled_image) for more information.
   * type: *string*
   * default value: `anonymous`

 * **shortLabelsOnHover** (WebGL only)
   * Indicate if you want to enable the custom label display mode when hovering nodes using the WebGL renderers. Setting this to true is more than recommended, unless you do not plan to use the custom shapes (but only images or icons, for instance). Default to false, to not interfer with normal Sigma.js behavior. Possible improvement: in the future, the plugin might force the default value to true when initialized, so that apps not using the plugin would not be affected.
   * type: *boolean*
   * default value: `false`

 * **spriteSheetResolution** (WebGL only)
   * This is the resolution of a side of the sprite sheet texture (which is a square). Must be power of two, as this is directly linked to the size of the final WebGL texture. Don't use a value too large. Values like 2048 or 4096 should be ok.
   * type: *number*
   * default value: `2048`

 * **spriteSheetMaxSprites** (WebGL only)
   * Defines how many sprites can fit inside the sprite sheet texture. If you have less than 128 images then you can enter the number of max different images you will even encounter, so that each sprite can use as much pixels as possible (this gives increased resolution).
   * type: *number*
   * default value: `128`

You may override the default values when instantiating Sigma, e.g.:

````javascript
var sigInst = new sigma({
  graph: g,
  container: 'graph-container',
  settings: {
    activeFontStyle: 'bold'
  }
});
````

#### Notes on spritesheets

Spritesheets are only used by the WebGL renderers. In this rendering mode, a large texture is created in memory, and is used to store the sprites. This large texture must have a fixed size, and for now cannot be resized dynamically.

The final resolution of each sprites thus depends on the resolution of this large texture, but also on the number of sprites you want to draw: the more sprite you need to draw, the less pixels are available, so they will have a smaller resolution, so you can either increase the resolution of the sprite sheet texture, or reduce the number of images.

### Compatibility

The plugin is compatible with [sigma.plugins.activeState](../sigma.plugins.activeState), which provides an API to manage the active state of nodes and edges.

The plugin is **NOT** compatible with [sigma.plugins.customShapes](../sigma.plugins.customShapes).

### Limitations

- WebGL node renderers ignore borders.
- Icons are not supported by the WebGL renderers.

#### Additional information about image loading

Dealing with images hosted on other servers is not a trivial task and must be done carefully.

Modern browsers impose restrictions regarding what kind of (image) data can be trusted or not, before "eating it" eg. load it in graphic card memory, or reading the buffer.

WebGL applications may use images and videos that come from other domains, with the cooperation of the server hosting the media, using Cross-Origin Resource Sharing [CORS].
In order to use such media, the application needs to explicitly request permission to do so,  and the server needs to explicitly grant permission.

If an image comes from an untrusted source, it might be labeled as such (or "tainted") and its use (reading pixels values from JS etc..) forbidden.

Please read this page for more information:
<https://developer.mozilla.org/en-US/docs/Web/HTML/CORS_enabled_image>

Some information about how WebGL deals with texture:
http://www.khronos.org/registry/webgl/specs/latest/1.0/#4.1