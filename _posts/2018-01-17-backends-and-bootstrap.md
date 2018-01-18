---
layout: post
title: Backends and Bootstrap
---

I have once again discovered that open source software is not known for the quality of the documentation. Simple things always turn out to be more difficult than they seem, and the examples are only sometimes helpful.

On Bootstrap
---
I don't particularly love CSS frameworks, but I hate them less than I hate the idea of learning CSS. So I use stuff like Bootstrap because I intend for the whole thing to become someone else's problem. Until I learn from that problem, I use lazy approximations.

The [meteor-site](https://github.com/redfin/react-server/tree/master/packages/react-server-examples/meteor-site) example from `react-server` shows how to thread Bootstrap into things. It uses version 3.3.6, while the docs at the time of this writing already moved on to 4.0.0-beta3. I guess the layout grid stuff I wanted to use is all new.

On POSTing data in react-server
---
It turns out that `react-server` is not preconfigured to read request data that's been POSTed with `ReactServerAgent`. Since the API endpoints handling those requests are implemented as Express middleware, those ended up being the docs to look at. Apparently the `body` part of `req.body` doesn't show up unless you register some middleware to add it for you. I'm not sure why this level of modularity is so important to everyone in JS-land, but [the recommended incantation](https://expressjs.com/en/4x/api.html#req.body) worked:

```javascript
import bodyParser from 'body-parser';
server.use('/api', bodyParser.json());
```

After a bit of investigation, it turns out that this is actually part of the normal behavior. But by [adding custom middleware](https://github.com/redfin/react-server/blob/b8277064b1e66b51269b0fdfbc77575fcc02f939/docs/guides/react-server-cli.md#use-custom-express-middleware) I've blown away the defaults. That's what you get for following the examples but missing some mundane detail in the docs. It is also unclear why the full default middleware isn't exposed vs just the rendering piece.

On react-server and WTF
---
Being a curious person, I wanted to make sure my new project works when the server isn't running locally. Naturally I turned to [ngrok](https://ngrok.com/). Naturally `react-server` didn't play along. The `ngrok` approach works fine when accessed from my dev machine, but breaks when views on a separate laptop. The issue appears to be some requests to `0.0.0.0:3001` for some JS bundles. The server logs indicate that this is for hot-loading JS edits:

```
info: [react-server-cli.src.commands.start] Started hot reload JavaScript server over HTTP on 0.0.0.0:3001
```

Fine, I'll turn off dev mode and run as if it's production. I mean it's very nice to call `ngrok` production, but it's not unreasonable to have a different setup. Booting up in production mode shows no signs of the auxiliary port 3001, so one would assume it went away. It did not go away. The remote machine has continued to ask for JS bundles on port 3001, it's just asking for minified versions now.

The docs point out a [--js-port](https://github.com/redfin/react-server/blob/b8277064b1e66b51269b0fdfbc77575fcc02f939/docs/guides/react-server-cli.md#--js-port) option, so this separation is a deliberate design decision. I can't say that I agree with it. This might have simplified the hot reloading implementation, but it probably broke server side rendering completely. If nothing else it's an unusual choice and it's made my developer experience worse. Also the hot reloading doesn't even work that well anyway, so there.

After this latest annoyance I took some time to reflect. I'm already get parallel data querying because of Redux and my own tendencies. I'm probably not going to build something big enough to benefit from fancy partial rendering tricks vs rendering all at once. And I'm getting tired of this developer experience. I had hoped that a nice framework would save me from learning a bunch of arcane nonsense. Instead I've just been learning arcane nonsense that won't apply outside of the framework and that very few others have already attempted to document.

So it's time for `react-server` and I to take a break. I'm starting to believe that maybe [Razzle](https://github.com/jaredpalmer/razzle) had it right. I've been let down by boilerplate and frameworks alike, and this third way is at least guaranteed to be different if not better.
