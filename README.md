# Angular2Webpack

Angular 2 with Webpack

In this Angular 2  Webpack tutorial, we will take you through the process of creating the Angular 2 Application from scratch using Webpack as our module loader.

Prerequisites

You need to install following before you proceed further with this tutorial

Visual Studio Code (or any other editor of your choice)
NPM Package Manager

Create an Application Folder

Open a command prompt and create the folder AngularWebpack.

md Angular2Webpack
cd Angular2Webpack

package.json Configuration file

A package.json file contains the metadata about our application. It includes the list of Javascript libraries that are used by our application. The npm package manager reads this file to install/update the libraries.

You can manually create the package.json file and run the command npm install to install the dependencies. In this tutorial, we will show it how to do it from the command prompt.

Run the following command to create the package.json file

npm init -f

npm install @angular/common @angular/compiler @angular/core @angular/forms 
@angular/http @angular/platform-browser @angular/platform-browser-dynamic @angular/router --save

The –save option ensures that these libraries are saved to package.json file

Installing third party libraries

The Angular 2 requires you to install the following dependencies

npm install core-js rxjs zone.js --save

Rxjs or Reactive Extensions (Rx) is a library for composing asynchronous and event-based programs using observable sequences and LINQ-style query operators
Zone is used for change detection
Core-js is a ES6 polyfill for typescript.

Installing Development Dependencies

The development dependencies are those libraries, which required only to develop the application. For Example javascript libraries for unit tests, minification, module bundlers are required only at the time of development of the application.

Our Angular 2 application needs Typescript. Webpack module, loaders & plugins etc.

Typescript

Typescript is a superset of Javascript. It extends the Javascript and brings options like static typing, classes, interfaces. The Code written in typescript cannot be used directly in the web browser. It must be compiled to javascript before running in the web browser. This process is known as Transpiling

npm install typescript --save-dev

Typings

Typescript forces you to define the types before using them. This has great advantages as any errors are detected at the compile time rather than at the run time

npm install typings --save-dev

Webpack

The dynamic web applications usually have lots of javascript files. These files are either created by you or it could be third party libraries like jquery/bootstrap etc. We include these in our index.html file using <script> tag. When a user sends requests to our application, the browser requests and loads these files one at a time. If you have lots of these files, then it will make your application slow. The solution to this problem is to merge all these files into a one or two files so that the browser can download the entire file in one request. This is where Webpack is used.

Webpack is a bundler, which scans your web application looking for javascript files and merges them into one ( or more) big file. Webpack has the ability to bundle any kind of file like JavaScript, CSS, SASS, LESS, images, HTML, & fonts etc. Webpack also comes with Development Server that supports hot module reloading.

Webpack along with Webpack dev server can be installed using the following command.

npm install webpack webpack-dev-server --save-dev

The –save-dev option ensures that these are installed as development dependencies

Webpack loaders and plugins

Webpack supports custom loaders and plugins. A loader is a program that allows you to preprocess files as you “load” them. They extract the content of the file, transform them and then return the transformed content to Webpack for bundling. With the help of loaders, the Webpack can handle any type of files

Webpack loaders

npm install angular2-template-loader awesome-typescript-loader css-loader file-loader 
html-loader null-loader raw-loader style-loader to-string-loader --save-dev

Webpack plugins

A plugin is a program that changes the behavior of webpack

npm install html-webpack-plugin webpack-merge extract-text-webpack-plugin --save-dev

Others dependencies
npm install rimraf --save-dev

Creating the Component

So far we have installed all the required dependencies. The next step is to create our application. Under the root folder of our application create folder call src. Under src create a folder called app.

Component class

First, let us create an Angular 2 Component. Create app.component.ts under the src/app directory 

Root Module

The Angular 2 follows the modular approach, the application development. Every Angular 2 application must have one module known as root Module. We will name it as app.module.ts. Create the file with the name app.module.ts under the folder app

Bootstrapping our root module

We have so far created AppComponent which is bound to the HTML template app.component.html. We have added the AppComponent to AppModule. In AppModule we indicated that the AppComponent is to be loaded when AppModule is loaded

The Next step is to ask the Angular to load the AppModule when the application is loaded. To do need to create main.ts file

Create main.ts in the src folder

Index page

We need a root page for our application. Create index.html under src folder

Assets

We have imported styles.css and used “angular.png” image in our AppComponent.

Create the folder assets/css under src

imilarly, create the folder assets/images under src. 
Configuring Our Application

We have successfully built our application. The next step is to run the application. But before that, we need to configure Typescript, Typings and Webpack libraries

Typescript

Create the file tsconfig.json in the root folder our project

Webpack Bundle

The next step is to configure the Webpack. Webpack allows us to bundle all our javascript files into a one or more files. Let us create three bundles in our application

In the first bundle, we add all our application code like components, service, modules etc. We call it as an app. We do not have to create a separate file to that. Our main.ts file will be the starting point for this bundle.

We put all the external libraries like Rxjs, Zone etc into a separate bundle. This includes Angular 2 libraries also. Let us call it as the  vendor. To do that we need to create the vendor.ts and import required libraries. Create the file called the vendor.ts under root folder

In the third bundle, we include the polyfills we require to run Angular applications in most modern browsers. Create a file called polyfills.ts

Webpack configuration

The next step is to configure the Webpack.

The Webpack by convention uses the webpack.config.js file to read the configuration information. Create the webpack.config.js in the root folder of our project. 

module.exports = require('./config/webpack.dev.js');

The above code tells the Webpack to read the configuration file webpack.dev.js from the config folder.

The Webpack can be setup so that you can have a separate configuration option for testing , development, and production. What you need to do is to create separate config files for development . testing and production and then switch between these config file in the main configuration file (webpack.config.js)

Create the folder “config” in the root of our project. This is where we are going to put all over Webpack related configuration option

Create the file webpack.common.js under the folder config 

First, we let Webpack know our entry points. Remember that we have decided to create three bundles of our application. Our three entry points are polyfills.ts , vendor.ts, and main.ts all located in the src folder.

The Webpack starts from these files and traverses through it to find dependencies and merges all of them one bundle per each entry.

Webpack then uses loaders to transform our files. For example, the Typescript files (ts extension) are passed through “angular2-template-loade” and then to “awesome-typescript-loader” (Right to left)

The CommonsChunkPlugin removes all the multiple used chunks of code and uses it only once.

The HtmlWebpackPlugin adds a script tag to our index.html for the each of the bundle created.
Webpack.dev.js

Create webpack.dev.js under the config folder 

The webpack.dev.js file imports the webpack.common.js and uses additional configuration options required only for the development.

The devtool defines how the source map is created. The source maps help in debugging our applications in the browser.

Output configuration has options that affect the output of the Webpack compilation. You can configure location on disk where the compiled files are written to (path), the name of the bundle (filename), the name of the chunk file (chunkfilename) and public URL path (publicPath) etc.

You call any development environment specific plugin here. The extract-text-webpack-plugin removes the compiled CSS from the bundle and emits is as a separate file.

