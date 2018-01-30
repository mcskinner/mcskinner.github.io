---
layout: post
title: Back to create-react-app
---

Today I relearned that you should always use the simple option until you need something more. Razzle was dazzling, but fell short. I went back to `create-react-app` with hat in hand, and found myself amazed at how quickly everything came together.

On Razzle
---
For all the fuss Razzle put into criticizing other options, I can't say that they were all that easy to use. As with more or less everything else I've tried, it's really hard to work with API-provided data. Some interaction of Redux, server-side rendering, and a data-backed page kept adding up to an crashing page. As I found myself writing bizarre state management code to get around this, I decided that server-side rendering might not be worth the hassle. When I've talked to other folks about it there doesn't seem to be a huge value add.

On create-react-app
---
It turns out that it only takes a single trick to make `create-react-app` work out of the box. After running the `npx` incantation, you just need to proxy non-static requests to another server. You can do that with the [proxy npm-config param](https://docs.npmjs.com/misc/config#proxy). Since I wanted to use port 5000, my `package.json` looks like so:

```json
{
  "name": "react-app",
  ...
  "proxy": "http://localhost:5000"
}
```

And that's it. Now I can finally get back to working on the parts of this that I care about.
