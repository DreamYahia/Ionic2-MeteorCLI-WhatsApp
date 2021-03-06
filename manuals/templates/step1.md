> If you got directly into here, please read the whole [intro section](/tutorials/whatsapp2-tutorial) explaining to goals for this tutorial and project.

Both [Meteor](meteor.com) and [Ionic](ionicframework.com) took their platform to the next level in tooling.
Both provide CLI interface instead of bringing bunch of dependencies and configure build tools.
There are also differences between those tools. in this tutorial we will focus on the `Meteor` CLI.

> If you are interested in the Angular CLI, the steps needed to use it with Meteor are almost indentical to the steps required by the Ionic CLI

Start by installing `Meteor` if you haven't already (See [reference](https://www.meteor.com/install)).

Now let's create our app - write this in the command line:

    $ meteor create whatsapp

And let's see what we've got. Go into the new folder:

    $ cd whatsapp

A `Meteor` project will contain the following dirs by default:

  - client - A dir containing all client scripts.
  - server - A dir containing all server scripts.

These scripts should be loaded automatically by their alphabetic order on their belonging platform, e.g. a script defined under the client dir should be loaded by `Meteor` only on the client. A script defined in neither of these folders should be loaded on both.

`Meteor` apps use [Blaze](http://blazejs.org/) as its default template engine and [Babel](https://babeljs.io/) as its default script pre-processor. In this tutorial, we're interested in using [Angular 2](https://angular.io/)'s template engine and [Typescript](https://www.typescriptlang.org/) as our script pre-processor; Therefore will remove the `blaze-html-templates` package:

    $ meteor remove blaze-html-templates

And we will replace it with a package called `angular2-compilers`:

    $ meteor add angular2-compilers

The `angular2-compilers` package not only replace the `Blaze` template engine, but it also applies `Typescript` to our `Meteor` project, as it's the recommended scripting language recommended by the `Angular` team. In addition, all `CSS` files will be compiled with a pre-processor called [SASS](http://sass-lang.com/), something which will definitely ease the styling process.

The `Typescript` compiler operates based on a user defined config, and without it, it's not going to behave expectedly; Therefore, we will define the following `Typescript` config file:

{{{diff_step 1.3}}}

> More information regards `Typescript` configuration file can be found [here](https://www.typescriptlang.org/docs/handbook/tsconfig-json.html).

Not all third-party libraries have any support for `Typescript` whatsoever, something that we should be careful with when building a `Typescript` app. To allow non-supported third-party libraries, we will add the following declaration file:

{{{diff_step 1.4}}}

Like most libraries, `Angular 2` is relied on peer dependencies, which we will have to make sure exist in our app by installing the following packages:

    $ meteor npm install --save @angular/common
    $ meteor npm install --save @angular/compiler
    $ meteor npm install --save @angular/compiler-cli
    $ meteor npm install --save @angular/core
    $ meteor npm install --save @angular/forms
    $ meteor npm install --save @angular/http
    $ meteor npm install --save @angular/platform-browser
    $ meteor npm install --save @angular/platform-browser-dynamic
    $ meteor npm install --save @angular/platform-server
    $ meteor npm install --save meteor-rxjs
    $ meteor npm install --save reflect-metadata
    $ meteor npm install --save rxjs
    $ meteor npm install --save zone.js
    $ meteor npm install --save-dev @types/meteor
    $ meteor npm install --save-dev @types/meteor-accounts-phone
    $ meteor npm install --save-dev @types/underscore
    $ meteor npm install --save-dev meteor-typings

Now that we have all the compilers and their dependencies ready, we will have to transform our project files into their supported extension. `.js` files should be renamed to `.ts` files, and `.css` files should be renamed to `.scss` files.

    $ mv server/main.js server/main.ts
    $ mv client/main.js client/main.ts
    $ mv client/main.css client/main.scss

Last but not least, we will add some basic [Cordova](https://cordova.apache.org/) plugins which will take care of mobile specific features:

    $ meteor add cordova:cordova-plugin-whitelist@1.3.1
    $ meteor add cordova:cordova-plugin-console@1.0.5
    $ meteor add cordova:cordova-plugin-statusbar@2.2.1
    $ meteor add cordova:cordova-plugin-device@1.1.4
    $ meteor add cordova:ionic-plugin-keyboard@1.1.4
    $ meteor add cordova:cordova-plugin-splashscreen@4.0.1

> The least above was determined based on Ionic 2's [app base](https://github.com/driftyco/ionic2-app-base).

Everything is set, and we can start using the `Angular 2` framework. We will start by setting the basis for our app:

{{{diff_step 1.8}}}

In the step above, we simply created the entry module and component for our application, so as we go further in this tutorial and add more features, we can simply easily extend our module and app main component.

## Ionic 2

[Ionic](http://ionicframework.com/docs/) is a free and open source mobile SDK for developing native and progressive web apps with ease. When using `Ionic`CLI, it comes with a boilerplate, just like `Meteor` does, but since we're not using `Meteor` CLI and not `Ionic`'s, we will have to set it up manually.

The first thing we're going to do in order to integrate `Ionic 2` in our app would be installing its `NPM` dependencies:

    $ meteor npm install --save @ionic/storage
    $ meteor npm install --save ionic-angular
    $ meteor npm install --save ionic-native
    $ meteor npm install --save ionicons

`Ionic` build system comes with a built-in theming system which helps its users design their app. It's a powerful tool which we wanna take advantage of. In-order to do that, we will define the following `SCSS` file, and we will import it:

{{{diff_step 1.10}}}

> The `variables.scss` file is hooked to different `Ionic` packages located in the `node_modules` dir. It means that the logic already existed, we only bound all the necessary modules together in order to emulate `Ionic`'s theming system.

`Ionic` looks for fonts in directory we can't access. To fix it, we will use the `mys:font` package to teach `Meteor` how to put them there.

    $ meteor add mys:fonts

That plugin needs to know which font we want to use and where it should be available.

Configuration is pretty easy, you will catch it by just looking on an example:

{{{diff_step 1.12}}}

`Ionic` is set. We will have to make few adjustments in our app in order to use `Ionic`, mostly importing its modules and using its components:

{{{diff_step 1.13}}}

## Running on Mobile

To add mobile support, select the platform(s) you want and run the following command:

    $ meteor add-platform ios
    # OR / AND
    $ meteor add-platform android

To run an app in the emulator use:

    $ meteor run ios
    # OR
    $ meteor run android

To learn more about **Mobile** in `Meteor` read the [*"Mobile"* chapter](https://guide.meteor.com/mobile.html) of the Meteor Guide.

`Meteor` projects come with a package called `mobile-experience` by default, which is a bundle of `fastclick`, `mobile-status-bar` and `launch-screen`. The `fastclick` package might cause some conflicts between `Meteor` and `Ionic`'s functionality, something which will probably cause some unexpected behavior. To fix it, we're going to remove `mobile-experience` and install the rest of its packages explicitly:

    $ meteor remove mobile-experience
    $ meteor add mobile-status-bar
    $ meteor add launch-screen

### Web

`Ionic` apps are still usable in the browser. You can run them using the command:

    $ meteor start
    # OR
    $ npm start

The app should be running on port `3000`, and can be changed by specifying a `--port` option.
