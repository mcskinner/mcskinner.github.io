---
layout: post
title: React has a nice ecosystem
---

Today I (re)discovered the [brillout/awesome-react-components](https://github.com/brillout/awesome-react-components) repository. I had already discovered [moroshko/react-autosuggest](https://github.com/moroshko/react-autosuggest) for my own needs, but holy cow is there a lot of prebuilt stuff out there to choose from.

On react-autosuggest
---
At long last, I have tried something in frontend development that just worked. The docs for this component are great, and the API is well designed for the transition from local to async data. It was just a matter of swapping out the suggestion fetch and clear logic to use a normal Redux pattern instead of the local data.

It is worth noting that a major feature of this API is the clarity of naming. The fetch hook is aptly named `onSuggestionsFetchRequested`. The name alone makes it clear that the fetch can be async, since it's just a request. Try to save on typing and you'll end up with something like `onFetchSuggestions`, which is not nearly as clear.

Stars for you Mr. Moroshko.

On the React ecosystem
---
Holy crap there is a lot out there. The selection at [brillout/awesome-react-components](https://github.com/brillout/awesome-react-components) is only a curated start, and that covers an awful lot of what I imagine myself doing.

As it turns out, that's not even the best way to interface with this data. There is a nicely searchable interface available at [
https://devarchy.com/react](https://devarchy.com/react).

On ngrok and proxying
---
I really should have figured this out earlier. A few weeks ago I bailed on `react-server` because of multi-port issues that cropped up with [ngrok](https://ngrok.io). While I don't regret the decision to ditch the framework, it turns out that I never really got past that fundamental problem. So now that I'm happy with my frontend server setup, I need to actually figure it out.

A quick search reveals the `--host-header` flag. Per [the docs](https://ngrok.com/docs#host-header) vanilla `ngrok` doesn't do anything smart with the requests it gets, "they are copied to your server byte-for-byte as they are received." When rewrites are needed, flags like this are there to help. Since I'm running everything locally, `--host-header=localhost` did the trick.
