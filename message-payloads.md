# Message payloads

## Standard message components

CSML message components all have a matching message format for client use in regular `JSON`. They can be extended by adding additional properties to the `content` wrapper.

### Text\(\)

```javascript
> Text()
{
  "content": {
    "text": "message"
  },
  "content_type": "text"
}
```

### Typing\(\)

```javascript
> Typing()
{
  "content": {
    "duration": 1000
  },
  "content_type": "typing"
}
```

### Wait\(\)

```javascript
> Wait()
{
  "content": {
    "duration": 1000
  },
  "content_type": "wait"
}
```

### Url\(\)

```javascript
> Url()
{
  "content": {
    "url": "https://example.com",
    "title": "title",
    "text": "text"
  },
  "content_type": "url"
}
```

### Image\(\)

```javascript
> Image()
{
  "content": {
    "url": "https://example.com/image.jpg",
  },
  "content_type": "image"
}
```

### Audio\(\)

```javascript
> Audio()
{
  "content": {
    "url": "https://example.com/audio.mp3",
  },
  "content_type": "audio"
}
```

### Video\(\)

```javascript
> Video()
{
  "content": {
    "url": "https://example.com/video.mp4",
  },
  "content_type": "video"
}
```

### File\(\)

```javascript
> File()
{
  "content": {
    "url": "https://example.com/video.mp4",
  },
  "content_type": "file"
}
```

### Button\(\)

```javascript
> Button()
{
  "content": {
    "title": "Button title",
    "payload": "Button payload"
  },
  "content_type": "button"
}
```

### Payload\(\)

```javascript
> Payload()
{
  "content": {
    "payload": "Custom payload"
  },
  "content_type": "payload"
}
```

### Question\(\)

```javascript
> Question()
{
  "content": {
    "title": "Question",
    "buttons": [Button]
  },
  "content_type": "question"
}
```

## Child components

Component payloads can be included into one another seamlessly. For example: 

```cpp
// CSML
say Question(
    "Where is Brian",
    buttons = [
        Button("In the kitchen"),
        Button("Somewhere else"),
    ]
)

// JSON output
{
    "content": {
    "title": "Question",
    "buttons": [
      {
        "content_type": "button",
        "content": {
          "title": "In the kitchen",
        }
      },
      {
        "content_type": "button",
        "content": {
          "title": "Somewhere else",
        }
      },
    ],
  },
  "content_type": "video"
}
```

