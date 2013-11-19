npm-debug-root
==============

Trying to reproduce an npm install problem - this is not real code

If you `npm install` this project, you'll get the following error:

```
 $ npm i
npm WARN package.json npm-debug-root@0.3.0 No repository field.
npm http GET http://registry.npmjs.org/npm-debug-level-1
npm http 304 http://registry.npmjs.org/npm-debug-level-1
npm http GET http://registry.npmjs.org/npm-debug-level-2
npm http 200 http://registry.npmjs.org/npm-debug-level-2
npm http GET http://registry.npmjs.org/npm-debug-root
npm http 200 http://registry.npmjs.org/npm-debug-root
npm ERR! cycle Unresolvable cycle detected
npm ERR! cycle While installing: npm-debug-root@0.3.0
npm ERR! cycle Found a pathological dependency case that npm cannot solve.
npm ERR! cycle Please report this to the package author.

npm ERR! System Darwin 13.0.0
npm ERR! command "node" "/usr/local/bin/npm" "i"
npm ERR! cwd /Users/nick.heiner/public-projects/npm-debug-root
npm ERR! node -v v0.10.21
npm ERR! npm -v 1.3.13
npm ERR! code ECYCLE
npm ERR! 
npm ERR! Additional logging details can be found in:
npm ERR!     /Users/nick.heiner/public-projects/npm-debug-root/npm-debug.log
npm ERR! not ok code 0
```

This is erroneous. There is no cycle. The dependencies are:

```
npm-debug-root ====(dep)===> npm-debug-level-1 ===(dep)===> npm-debug-level-2 ===(dev dep)===> npm-debug-root
```

Because the link from `npm-debug-level-2` to `npm-debug-root` is a dev dep, this is not a cycle. 
If we were to install `npm-debug-root`, we would not install the dev deps of its deps.
npm is being overzealous in throwing `ECYCLE`.
