# Service Worker

This exercise helps you in understanding the usage of Service Workers in Angular.

## Tasks

### Create Element

1. Create a new application with `ng n sw-app`.

2. Add the pwa package to your project: `ng add @angular/pwa --project PROJECT_NAME`

3. Build the application in production mode: `ng build --prod`

4. Install the http-server to start your dist folder in a http-server:

```
npm i -g http-server
http-server -p 8080 -c-1 dist/PROJECT_NAME
```

5. Change something in your application. Rebuild it with `--prod`. Rerun the http-server and check if the changes are visible. Take a look at the network tab of your browser. 

6. Write a log, if a newer version of your service-worker cache is available: 

```
@Injectable()
export class LogUpdateService {

  constructor(updates: SwUpdate) {
    updates.available.subscribe(event => {
      console.log('current version is', event.current);
      console.log('available version is', event.available);
    });
  }
}
```

7. Add a logic which checks every two seconds if a new version of your manifest is available: 

```
const appIsStable$ = appRef.isStable.pipe(first(isStable => isStable === true));
const everyTwoSeconds$ = interval(2 * 1000);

concat(appIsStable$, everyTwoSeconds$).subscribe(() => updates.checkForUpdate());
```

8. Reload your application with `document.location.reload()` if a new version of the service-worker is available.
