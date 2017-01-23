# React 事件

[doc](https://facebook.github.io/react/docs/events.html)

## 1. Clipboard Events

Event Names: 

`onCopy` `onCut` `onPaste`

Properties: 

`DOMDataTransfer clipboardData`

## 2. Composition Events

Event Names: 

`onCompositionEnd ` `onCompositionStart ` `onCompositionUpdate `

Properties: 

`string data`

## 3. Keyboard Events

Event Names: 

`onKeyDown ` `onKeyPress ` `onKeyUp `

Properties: 

```
boolean altKey
number charCode
boolean ctrlKey
boolean getModifierState(key)
string key
number keyCode
string locale
number location
boolean metaKey
boolean repeat
boolean shiftKey
number which
```

## 4. Focus Events

Event Names: 

`onFocus ` `onBlur ` 

Properties: 

`DOMEventTarget relatedTarget`

## 5. Form Events

[form](https://facebook.github.io/react/docs/forms.html)

Event Names: 

`onChange ` `onInput ` `onSubmit `

Properties: 



## 6. Mouse Events

Event Names: 

```
onClick onContextMenu onDoubleClick onDrag onDragEnd onDragEnter onDragExit
onDragLeave onDragOver onDragStart onDrop onMouseDown onMouseEnter onMouseLeave
onMouseMove onMouseOut onMouseOver onMouseUp
```

Properties: 

```
boolean altKey
number button
number buttons
number clientX
number clientY
boolean ctrlKey
boolean getModifierState(key)
boolean metaKey
number pageX
number pageY
DOMEventTarget relatedTarget
number screenX
number screenY
boolean shiftKey
```

## 7. Selection Events

Event Names: 

`onSelect ` 

Properties: 

## 8. Touch Events

Event Names: 

`onTouchCancel onTouchEnd onTouchMove onTouchStart `

Properties: 

```
boolean altKey
DOMTouchList changedTouches
boolean ctrlKey
boolean getModifierState(key)
boolean metaKey
boolean shiftKey
DOMTouchList targetTouches
DOMTouchList touches
```

## 9. UI Events

Event Names: 

`onScroll `

Properties: 

```
number detail
DOMAbstractView view
```

## 10. Wheel Events

Event Names: 

`onWheel ` 

Properties: 

```
number deltaMode
number deltaX
number deltaY
number deltaZ
```

## 11. Media Events

Event Names: 

```
onAbort onCanPlay onCanPlayThrough onDurationChange onEmptied onEncrypted 
onEnded onError onLoadedData onLoadedMetadata onLoadStart onPause onPlay 
onPlaying onProgress onRateChange onSeeked onSeeking onStalled onSuspend 
onTimeUpdate onVolumeChange onWaiting
```

Properties: 

## 12. Image Events

Event Names: 

`onLoad ` `onError `

Properties: 

## 13. Animation Events

Event Names: 

`onAnimationStart ` `onAnimationEnd ` `onAnimationIteration `

Properties: 

```
string animationName
string pseudoElement
float elapsedTime
```

## 14. Transition Events

Event Names: 

`onTransitionEnd `

Properties: 

```
string propertyName
string pseudoElement
float elapsedTime
```



