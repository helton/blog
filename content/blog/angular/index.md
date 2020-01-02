---
title: Angular
date: "2019-12-31T19:08:00.000Z"
description: "The Road to Angular"
tags: ["angular", "javascript", "typescript", "framework", "frontend", "guide"]
---

Angular is the shiny web framework developed by Google. Over a couple of years, it evolved a lot since its first version, called **AngularJS**, released in 2010. Angular 2+ is a complete rewrite of its first version.

## General structure

**Modules** group components and services.

**Components** are the building block of any Angular app. They control the view.

**Services** provide the way to abstract business logic (usually fetch and process data).

### Components

A simple [component](https://angular.io/guide/architecture-components) looks like this in Angular:

```typescript
import { Component } from '@angular/core';

@Component({
  selector: 'app-server',
  templateUrl: './server.component.html',
  styleUrls: ['./server.component.css'],
})
export class ServerComponent {

}
```

A component is identified by the decorator `@Component` which has three properties:

- **selector**: it defines how the components should be referenced on the app. Common choices are:
  - **directive**: references the component like `<app-cool></app-cool>`
  - **attribute**: references the component like `<div app-cool></div>`
  - **class**: references the component like `<div class="app-cool"></div>`
- **template**: the HTML code for the component. You can add an external file or embedded the HTML in the `@Component` declaration.
- **style**: a list of styles (CSS) that is scoped to the component. You can add external files or embedded the list of CSS in the `@Component` declaration.

## Typescript

[Typescript](https://www.typescriptlang.org/) is a superset of Javascript, and it's widely used to develop Angular apps since it was officially adopted by Google on Angular 2.

## Modules

Components and services are grouped in [modules](https://angular.io/guide/ngmodules). By default Angular doesn't scan your entire app to find all the components and services available to use. Angular look at the *module* you registered. All your custom components and services should be registered on `app.module.ts` on the `declaration` property.

```typescript
@NgModule({
  declarations: [
    AppComponent,
    MyCustomComponent,
  ],
  ...
})
export class AppModule { }
```

Tips:

- If you plan to use `ngModel` to have two-way data binding in forms remember to import [`FormsModule`](https://angular.io/api/forms/FormsModule) from `@angular/forms` and add it to your `app.module.ts`
- If you plan to fetch data on your services, remember to import [`HttpClientModule`](https://angular.io/api/common/http/HttpClientModule) from `@angular/common/http` and add it to your `app.module.ts`

## Bootstrapping

Every Angular app has an `index.html` file where you can find the root component(s) of your app. This component will be used to [boostrap](https://angular.io/guide/bootstrapping) the app and it should be registered in `app.module.ts` under `@NgModule` via `bootstrap` property:

```typescript
@NgModule({
  ...
  bootstrap: [AppComponent]
})
export class AppModule { }

```

On the `main.ts` file, you can see where the Angular is called to create and patch the DOM:

```typescript
import { enableProdMode } from '@angular/core';
import { platformBrowserDynamic } from '@angular/platform-browser-dynamic';

import { AppModule } from './app/app.module';
import { environment } from './environments/environment';

if (environment.production) {
  enableProdMode();
}

platformBrowserDynamic().bootstrapModule(AppModule)
  .catch(err => console.error(err));
```

## Routing

Angular supports routing via `app.routing.module.ts` where you can map the paths to their components:

```typescript
import { NgModule } from '@angular/core';
import { Routes, RouterModule } from '@angular/router';
import { HomeComponent } from './home/home.component';
import { ListComponent } from './list/list.component';

const routes: Routes = [
  {
    path: '',
    component: HomeComponent,
  },
  {
    path: 'list',
    component: ListComponent,
  }
];

@NgModule({
  imports: [RouterModule.forRoot(routes)],
  exports: [RouterModule]
})
export class AppRoutingModule { }
```

You may not have this file if you chose to not support routing when using the CLI to create your project.

## CLI

Angular offers a CLI (Command Line Interface) to help create the projects, components and services you will use to build your app.

You can install it globally via `npm install -g @angular/cli` (or via `yarn global add @angular/cli`)

Some useful commands are:

- `ng new project-name`: Creates a project. You can choose to have routing and how you'll style your app (via CSS, SCSS, etc)
- `ng generate component component-name`: Create a component with the html, css, typescript and the tests. You can use `ng g c component-name` for short.

## `angular.json`

This file stores the general configuration of an Angular app. You usually touch this file to add new global CSS (like when using [Bootstrap](https://getbootstrap.com/), [Foundation](https://foundation.zurb.com/) or other frameworks)

## Versions

Angular has a major version released every 6 months.
All the major releases are supported for 18 months: 6 months of *active support* + 12 months of *LTS* support.
The Angular 3 was skipped and today we have the current releases:

| VERSION   | STATUS | RELEASED     |
|---------- |--------|--------------|
| Angular 8 | Active | May 28, 2019 |
| Angular 7 | LTS    | Oct 18, 2018 |
| Angular 6 | LTS    | May 4, 2018  |
| Angular 5 | -      | Nov 1, 2017  |
| Angular 4 | -      | Mar 23, 2017 |
| Angular 2 | -      | Sep 14, 2016 |
| AngularJS | -      | Oct 20, 2010 |

## Alternatives

Angular is not new to the JS libraries and frameworks for the web. There are some major contenders:

- [React](https://reactjs.org/): `A JavaScript library for building user interfaces` developed by Facebook.
- [VueJS](https://vuejs.org/): `The Progressive JavaScript Framework` developed by Evan You and a huge community
- [Ember](https://emberjs.com/): `A framework for ambitious web developers`.

### Unorthodox alternatives

- [Svelte](https://svelte.dev/): `Cybernetically enhanced web apps`
- [Elm](https://elm-lang.org/): `A delightful language for reliable webapps.`
