# ssr works differently when running node build 
Repro for sveltekit build bug [#1921](https://github.com/sveltejs/kit/issues/1921)

For sake of time, I've set it up to do a 302 redirect to signin page if no session cookie is present when going to `/todos`.
The potential bug is presented in option 3 of the below 3 ways of running the app

1. Dev test
```
npm i
npm run dev
```
 All works as expected. You can manually add the "session" cookie in dev tools to see the /todos page loads fine once this is present.

 2. Preview production build
```
npm i
npm run build
npm run preview
```
 All works as expected. You can manually add the "session" cookie in dev tools to see the /todos page loads fine once this is present.

 3. Run production build using node build
 ```
npm i
npm run build
node build
```
Once hydrated, works as expected, if "session" cookie is present, /todos page loads fine. If you refresh, SSR loads what was defined when `npm run build` is executed which should be something like this in your logs:
```
> Using @sveltejs/adapter-node
  302 /todos -> /signin
  ✔ done
```
So thus it goes to /signin instead of /todos regardless of session cookie now being present. (see your logs for "session cookie at handle func: ")