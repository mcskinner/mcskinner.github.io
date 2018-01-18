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
It turns out that `react-server` does not come with batteries installed for actually reading data that's been POSTed with `ReactServerAgent`. The API endpoints are implemented as Express middleware, and indeed those were the docs to look at. Apparently the `body` part of `req.body` doesn't show up unless you register another more different middleware to add it for you. I'm not sure how it works, but [the recommended incantation](https://expressjs.com/en/4x/api.html#req.body) worked:

```javascript
import bodyParser from 'body-parser';
server.use('/api', bodyParser.json());
```
