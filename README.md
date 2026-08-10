# dogleg-moved

The old domain's only page. Serves **dogleg.cameronbristol.xyz**, which used to
be DogLeg itself; the game now lives at **[playdogleg.com](https://playdogleg.com)**.

`localStorage` is per-origin, so a player's clubhouse — their name, streak,
round archive, and the player id the daily dice are salted from — does not
follow them to the new domain on its own. Losing that id is the part that
actually breaks the game rather than merely annoying someone: it deals you
different dice for a day you have already posted.

So this page packs every `dogleg:` (and legacy `bp:`) key on this origin into a
URL fragment and bounces the browser to playdogleg.com, where the app unpacks
it, merges it, and strips the fragment. An old bookmark migrates its owner with
zero clicks, and keeps doing so for as long as this stays up.

## Do not edit index.html here

It is a **copy**. The source of truth is `handoff/index.html` in
[bulldogslucky13/dogleg](https://github.com/bulldogslucky13/dogleg), where it is
under test: `src/lib/handoff.test.ts` pulls that real script out of that real
file and runs it against the real unpacker, precisely because the two halves
live on different domains and can never share a bundle.

Edit it here and nothing fails — the tests still pass over there, against the
file you didn't change. The wire format drifts, and every player still holding
an old bookmark silently arrives as a stranger.

**To change it:** edit `handoff/index.html` in the main repo, let its tests
prove both halves still agree, then copy the file here verbatim.

## Deploying

1. Settings → Pages → Source: **Deploy from a branch**, `main` / root
2. Settings → Pages → Custom domain: `dogleg.cameronbristol.xyz`
   - The main repo must have **released** that domain first — GitHub refuses to
     let two repositories claim the same hostname.
   - The existing DNS record (a CNAME to `bulldogslucky13.github.io`) needs no
     change. GitHub routes by which repo claims the hostname, not by DNS target.
3. Wait for the HTTPS certificate, then load the domain and confirm it forwards.

## When can this be retired?

Not on a schedule — whenever old bookmarks, old share links and old search
results stop sending real people here. It is one static file and costs nothing
to leave up; the failure mode of removing it too early is a player losing years
of rounds with no way to get them back.
