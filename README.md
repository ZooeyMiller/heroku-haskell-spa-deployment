# How to deploy a haskell server with an SPA front end to heroku

This took me a while to go through when i was working it out and there was no straight up tutorial so I thought I'd write a quick one.

This is based on a haskell application / server built with stack and an SPA that uses npm to get the compiler / transpiler and stuff (I think that's all of them?).

Assumptions this makes: 
- you know how to use npm and the build tools for whatever SPA you're using
- you know how to use stack
- you know how to use heroku

If the above are not true for you then it's ok because they are well-documented and google-able.

1. make your haskell app using `stack new` maybe with a template or something.
2. go into your new haskell project and initialise it as a node project (in the root) using `npm init`. 
3. now you have a package.json in the root of your haskell project - feels weird right? 
3. make it into a git repo - `git init`
4. add any deps for the SPA to the `package.json` - maybe it's an elm front end so you add `elm` to it. Or maybe it's purescript so you add `purescript` and `spago` to the dependencies. I trust you to know what you need.
5. create your front end stuff in a sub directory, we'll be calling it `client`. 
6. get writing some code and set up your server to serve the `client/build` directory (or whatever it's called in your case), and get it to use the `PORT` environment variable for the port it runs on. 
7. create a heroku app and add the [heroku buildpack for stack](https://github.com/mfine/heroku-buildpack-stack) and the [node js buildpack](https://github.com/heroku/heroku-buildpack-nodejs#locking-to-a-buildpack-version) to the project.
```bash
heroku create --buildpack https://github.com/mfine/heroku-buildpack-stack.git

heroku buildpacks:set https://github.com/heroku/heroku-buildpack-nodejs
```
8. add a `postinstall` script to the `package.json` in the root that does all of the compilation jazz for your SPA. So it could be something like
```
{
  "postinstall": "cd client && elm make Main.elm --output="./build/index.html"
}
```
9. push your stuff up to heroku. 
```
git push heroku master
```
10. marvel at your deployed app. 

If there's something I could / should change send a PR or and issue
