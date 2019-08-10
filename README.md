# NodeJS Instagram private API client

![logo](https://cloud.githubusercontent.com/assets/1809268/15931032/2792427e-2e56-11e6-831e-ffab238cc4a2.png)

[![Telegram Chat](https://img.shields.io/badge/telegram-join%20chat-informational.svg)](https://t.me/igpapi)
[![npm](https://img.shields.io/npm/dm/instagram-private-api.svg?maxAge=600)](https://www.npmjs.com/package/instagram-private-api)
[![npm](https://img.shields.io/npm/l/instagram-private-api.svg?maxAge=600)](https://github.com/huttarichard/instagram-private-api/blob/master/LICENSE)
[![Join the chat at https://gitter.im/instagram-private-api/Lobby](https://badges.gitter.im/Join%20Chat.svg)](https://gitter.im/instagram-private-api/Lobby)

---

# Install

From npm

```
npm install instagram-private-api
```

From github

```
npm install github:dilame/instagram-private-api
```

# Support us

If you find this library useful for you, you can support it by donating any amount

BTC: 1Dqnz9QuswAvD3t7Jsw7LhwprR6HAWprW6

# Examples

You can find usage examples [here](examples)

```typescript
import { IgApiClient } from './src';
import { sample } from 'lodash';
const ig = new IgApiClient();
// You must generate device id's before login.
// Id's generated based on seed
// So if you pass the same value as first argument - the same id's are generated every time
ig.state.generateDevice(process.env.IG_USERNAME);
// Optionally you can setup proxy url
ig.state.proxyUrl = process.env.IG_PROXY;
(async () => {
  // Execute all requests prior to authorization in the real Android application
  // Not required but recommended
  await ig.simulate.preLoginFlow();
  const loggedInUser = await ig.account.login(process.env.IG_USERNAME, process.env.IG_PASSWORD);
  // The same as preLoginFlow()
  // Optionally wrap it to process.nextTick so we dont need to wait ending of this bunch of requests
  process.nextTick(async () => await ig.simulate.postLoginFlow());
  // Create UserFeed instance to get loggedInUser's posts
  const userFeed = ig.feed.user(loggedInUser.pk);
  const myPostsFirstPage = await userFeed.items();
  // All the feeds are auto-paginated, so you just need to call .items() sequentially to get next page
  const myPostsSecondPage = await userFeed.items();
  await ig.media.like({
    // Like our first post from first page or first post from second page randomly
    mediaId: sample([myPostsFirstPage[0].id, myPostsSecondPage[0].id]),
    moduleInfo: {
      module_name: 'profile',
      user_id: loggedInUser.pk,
      username: loggedInUser.username,
    },
    d: sample([0, 1]),
  });
})();
```

# Basic concepts

## Feeds

Feed allows you to get data. Every feed is accessible via `ig.feed.key`. See [nice example](https://github.com/dilame/instagram-private-api/blob/01eb39940e79a0d3fc7a528586bdb0fe37bea11d/examples/account-followers.feed.example.ts) and learn how to work with library feeds.

Available feed keys list:

* `accountFollowers`
* `accountFollowing`
* `news`
* `discover`
* `pendingFriendships`
* `blockedUsers`
* `directInbox`
* `directPending`
* `directThread`
* `user`
* `tag`
* `location`
* `mediaComments`
* `reelsMedia`
* `reelsTray`
* `timeline`
* `musicTrending`
* `musicSearch`
* `musicGenre`
* `musicMood`
* `usertags`
* `saved`

## Repositories

Repositories implements low-level atomic operations. Any repository method must make at most one api-request. There is repository listing below, so you can get information about methods of each repository from our autogenerated docs.

Keys is a little hints, with it you will be able to get access to repository via `ig.key`.

| Key | Repository class documentation |
| --- | --- |
| `account` | [AccountRepository](https://github.com/dilame/instagram-private-api/blob/master/docs/classes/_repositories_account_repository_.accountrepository.md) |
| `attribution` | [AttributionRepository](https://github.com/dilame/instagram-private-api/blob/master/docs/classes/_repositories_attribution_repository_.attributionrepository.md) |
| `challenge` | [ChallengeRepository](https://github.com/dilame/instagram-private-api/blob/master/docs/classes/_repositories_challenge_repository_.challengerepository.md) |
| `consent` | [ConsentRepository](https://github.com/dilame/instagram-private-api/blob/master/docs/classes/_repositories_consent_repository_.consentrepository.md) |
| `creatives` | [CreativesRepository](https://github.com/dilame/instagram-private-api/blob/master/docs/classes/_repositories_creatives_repository_.creativesrepository.md) |
| `direct` | [DirectRepository](https://github.com/dilame/instagram-private-api/blob/master/docs/classes/_repositories_creatives_repository_.creativesrepository.md) |
| `directThread` | [DirectThreadRepository](https://github.com/dilame/instagram-private-api/blob/master/docs/classes/_repositories_creatives_repository_.creativesrepository.md) |
| `discover` | [DiscoverRepository](https://github.com/dilame/instagram-private-api/blob/master/docs/classes/_repositories_creatives_repository_.creativesrepository.md) |
| `fbsearch` | [FbsearchRepository](https://github.com/dilame/instagram-private-api/blob/master/docs/classes/_repositories_creatives_repository_.creativesrepository.md) |
| `friendship` | [FriendshipRepository](https://github.com/dilame/instagram-private-api/blob/master/docs/classes/_repositories_creatives_repository_.creativesrepository.md) |
| `launcher` | [LauncherRepository](https://github.com/dilame/instagram-private-api/blob/master/docs/classes/_repositories_creatives_repository_.creativesrepository.md) |
| `linkedAccount` | [LinkedAccountRepository](https://github.com/dilame/instagram-private-api/blob/master/docs/classes/_repositories_creatives_repository_.creativesrepository.md) |
| `live` | [LiveRepository](https://github.com/dilame/instagram-private-api/blob/master/docs/classes/_repositories_creatives_repository_.creativesrepository.md) |
| `location` | [LocationRepository](https://github.com/dilame/instagram-private-api/blob/master/docs/classes/_repositories_creatives_repository_.creativesrepository.md) |
| `locationSearch` | [LocationSearch](https://github.com/dilame/instagram-private-api/blob/master/docs/classes/_repositories_creatives_repository_.creativesrepository.md) |
| `loom` | [LoomRepository](https://github.com/dilame/instagram-private-api/blob/master/docs/classes/_repositories_creatives_repository_.creativesrepository.md) |
| `media` | [MediaRepository](https://github.com/dilame/instagram-private-api/blob/master/docs/classes/_repositories_creatives_repository_.creativesrepository.md) |
| `music` | [MusicRepository](https://github.com/dilame/instagram-private-api/blob/master/docs/classes/_repositories_creatives_repository_.creativesrepository.md) |
| `news` | [NewsRepository](https://github.com/dilame/instagram-private-api/blob/master/docs/classes/_repositories_creatives_repository_.creativesrepository.md) |
| `qe` | [QeRepository](https://github.com/dilame/instagram-private-api/blob/master/docs/classes/_repositories_creatives_repository_.creativesrepository.md) |
| `qp` | [QpRepository](https://github.com/dilame/instagram-private-api/blob/master/docs/classes/_repositories_creatives_repository_.creativesrepository.md) |
| `tag` | [TagRepository](https://github.com/dilame/instagram-private-api/blob/master/docs/classes/_repositories_creatives_repository_.creativesrepository.md) |
| `upload` | [UploadRepository](https://github.com/dilame/instagram-private-api/blob/master/docs/classes/_repositories_creatives_repository_.creativesrepository.md) |
| `user` | [UserRepository](https://github.com/dilame/instagram-private-api/blob/master/docs/classes/_repositories_creatives_repository_.creativesrepository.md) |
| `zr` | [ZrRepository](https://github.com/dilame/instagram-private-api/blob/master/docs/classes/_repositories_creatives_repository_.creativesrepository.md) |

# Contribution

If you need features that is not implemented - feel free to implement and create PRs!

Plus we need some documentation, so if you are good in it - you are welcome.

# Useful links

[instagram-id-to-url-segment](https://www.npmjs.com/package/instagram-id-to-url-segment) - convert the image url fragment to the media ID

## Special thanks

- [Richard Hutta](https://github.com/huttarichard), original author of this library. Thanks to him for starting it.

## Thanks to contributors

- [Nerixyz](https://github.com/Nerixyz), for writing a huge amount of code for this library.

## End User License Agreement (EULA)

1. _You will not use_ this repository for sending mass spam or any other malicious activity
2. _We / You will not support_ anyone who is violating this EULA conditions
3. _Repository is just for learning / personal purposes_ thus should not be part of any
   service available on the Internet that is trying to do any malicious activity (mass bulk request, spam etc.)
