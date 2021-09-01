# PNPM + Expo 42

Monorepo reproducing weird asset errors between expo / metro / pnpm

versions of stack:

```txt
pnpm: latest
expo: 42
expo-cli: 4.8.1
metro: 0.58 (0.59 has same issues, later versions beyon didn't seem to work)
```

First install PNPM:

```sh
npm i -g pnpm
yarn add --global pnpm
```

Then install the dependencies in this workspace

```sh
pnpm install
```

Finally start the application:

```sh
pnpm start --filter mobile-app
```

The application can successfully boot, but Expo/Metro can't seem to serve our custom fonts...

A few things to note:

- We get a MD5 mismatch on the fonts.
- the URL includes `/assets/assets/..` which looks incorrect?
- Font URL seems malformed (two `?` in the query params):
  - http://10.103.48.232:19000/assets/assets/fonts/babysweet.ttf?platform=ios&hash=5751ffa9c86291f684a5d30b67814476?platform=ios&dev=true&hot=false&minify=false
- What's also strange is manually manipulating the URL quickly seems to work
  - manually modify the URL to the following: http://10.103.48.232:19000/assets/fonts/babysweet.ttf
  - remove the `f` from `.ttf` causing an legitament 404 in the metro server
  - quickly restore the `f`, hit enter and you'll be served the font

```sh
[Unhandled promise rejection: Error: Downloaded file for asset 'babysweet.ttf' Located at http://10.103.48.232:19000/assets/assets/fonts/babysweet.ttf?platform=ios&hash=5751ffa9c86291f684a5d30b67814476?platform=ios&dev=true&hot=false&minify=false failed MD5 integrity check]
at node_modules/expo-asset/build/PlatformUtils.js:53:18 in _downloadAsyncManagedEnv
at [native code]:null in flushedQueue
at [native code]:null in invokeCallbackAndReturnFlushedQueue
```

![preview](https://github.com/hanford/pnpm-expo-42/blob/master/weird.gif)

Maybe related: https://github.com/pnpm/pnpm/issues/3321
