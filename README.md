# HEAT WAVE 🎮

<img src="super2.png" width="400" />

The Ionic Super Starter is a batteries-included starter project for Ionic 2.x apps complete with pre-built pages, providers, , and best practices for Ionic development.

The goal of the Super Starter is to get you from zero to app store faster than before, with a set of opinions from the Ionic team around page layout, data/user management, and project structure.

The way to use this starter is to pick and choose the various page types you want use, and remove the ones you don't. If you want a blank slate, this starter isn't for you (use the `blank` type instead).

One of the big advances in Ionic 2 was moving from a rigid route-based navigation system to a flexible push/pop navigation system modeled off common native SDKs. We've embraced this pattern to provide a set of reusable pages that can be navigated to anywhere in the app. Take a look at the [Settings page](https://github.com/driftyco/ionic-starter-super/blob/master/src/pages/settings/settings.html#L38) for a cool example of a page navigating to itself to provide a different UI without duplicating code.

_Note: the Ionic Super Starter requires Ionic CLI 2.1.18 or greater._

## PouchDB & CryptoPouch

This project includes [PouchDB](https://pouchdb.com) and [CryptoPouch]().
After installing these and adding the test code for crypto-pouch usage, there was this error:
```
webpackMissingModule — index.js:12Error: Cannot find module "node-uuid"
```

Installing node-uuid solved the problem::
```
$ npm i node-uuid --save
```

The sample crypto-pouch usage can be found in the src/pages/tutorial/tutorial directory.
The basic usage is this:
```
import * as PouchDB from 'pouchdb';
import * as CryptoPouch from 'crypto-pouch';

export class TutorialPage {
  constructor(public navCtrl: NavController, public menu: MenuController, translate: TranslateService) {
    
    PouchDB.plugin(CryptoPouch);
    var db = new PouchDB('kittens');
    var password = "password";

    db.crypto(password);

    // db.crypto(password).then(function () {
    //   return db.put({_id: 'foo', bar: 'baz'});
    // }).then(function () {
    //     return db.get('foo');
    // }).then(function (doc) {
    //     console.log('decrypted', doc);
    //     return db.removeCrypto();
    // }).then(function () {
    //     return db.get('foo');
    // }).then(function (doc) {
    //     console.log('encrypted', doc);
    // })
```

This code will run, but just doing ```db.crypto(password)``` apparently will do nothing.
According to [this StackOverflow answer](http://stackoverflow.com/questions/35758553/how-to-encrypt-a-pouchdb-database),
This is needed:
```
db.crypto(password).then(function () {
   // <-- encryption set up
})
```


However, trying this, for example by uncommenting the above commented out code will cause this runtime error:
```
EXCEPTION: Error in ./MyApp class MyApp - inline template:16:2 caused by: undefined is not an object (evaluating 'db.crypto(password).then')
```

Not sure why the first ```db.crypto(password)``` will work but the ```db.crypto(password).then``` will not.


## Table of Contents

1. [Pages](#pages)
2. [i18n](#i18n) (adding languages)

## Pages

The Super Starter comes with a variety of ready-made pages. These pages help you assemble common building blocks for your app so you can focus on your unique features and branding.

The app loads with the `FirstRunPage` set to `TutorialPage` as the default. If the user has already gone through this page once, it will be skipped the next time they load the app.

If the tutorial is skipped but the user hasn't logged in yet, the Welcome page will be displayed which is a "splash" prompting the user to log in or create an account.

Once the user is authenticated, the app will load with the `MainPage` which is set to be the `TabsPage` as the default.

The entry and main pages can be configured easily by updating the corresponding variables in [src/pages/pages.ts](https://github.com/driftyco/ionic-starter-super/blob/master/src/pages/pages.ts).

Please read the [Pages](https://github.com/driftyco/ionic-starter-super/tree/master/src/pages) readme, and the readme for each page in the source for more documentation on each.

## Providers

The Super Starter comes with some basic implementations of common providers.

### User

The `User` provider is used to authenticate users through its `login(accountInfo)` and `signup(accountInfo)` methods, which perform `POST` requests to an API endpoint that you will need to configure.

### Api

The `Api` provider is a simple CRUD frontend to an API. Simply put the root of your API url in the Api class and call get/post/put/patch/delete 

## i18n

Ionic Super Starter comes with internationalization (i18n) out of the box with [ng2-translate](https://github.com/ocombe/ng2-translate). This makes it easy to change the text used in the app by modifying only one file. 

By default, the only language strings provided are American English.

### Adding Languages

To add new languages, add new files to the `src/assets/i18n` directory, following the pattern of LANGCODE.json where LANGCODE is the language/locale code (ex: en/gb/de/es/etc.).

### Changing the Language

To change the language of the app, edit `src/app/app.component.ts` and modify `translate.use('en')` to use the LANGCODE from `src/assets/i18n/`