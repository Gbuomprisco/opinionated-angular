![Logo](https://github.com/Gbuomprisco/opinionated-angular/blob/master/Group%203.png)

## HTML Wrapping and Order

 - do not exceed 80 characters per column** for all files: it’s simply much easier to read vertically than horizontally.

This is an element written without any convention:

```html
<input (input)="onInputChanged($event)" *ngIf="canEdit" class="form-control" [attr.placeholder]="placeholder" @fadeIn type="text" />
```


### Rules For Writing HTML Tags

* When an element has two or more attributes, write **one attribute** per line

* Attributes have to be written **in a specific order**

* Unless using a single attribute, the closing tag has to be written **on the next line**


```html
<input 
 (input)="onInputChanged($event)"
 *ngIf="canEdit"
 class="form-control"
 [attr.placeholder]="placeholder" 
 @fadeIn 
 type="text"
/>
```

Attributes order:

* Structural directives

* Animations

* Static properties

* Dynamic properties

* Events

Let’s see an example of how I would personally write the previous example:

```html
<input 
 *ngIf="canEdit"
 @fadeIn 
 type="text"
 class="form-control"
 [attr.placeholder]="placeholder"
 (input)="onInputChanged($event)"
/>
```

Add structural directives only to **ng-container** elements:


```html
<ng-container *ngIf="canEdit"> 
 <input 
  @fadeIn 
  type="text"
  class="form-control"
  [attr.placeholder]="placeholder"
  (input)="onInputChanged($event)"
 />
</ng-container>
```

A structural directive tells me (before I need to know anything else it does):

* Is this field being shown? And why?

* Is this field being repeated?

This can facilitate reading through and understanding the structure of your templates.

## Pipes

Wrap pipes expressions within parenthesis. The feeling of division provided by the parenthesis gives me a clue that the value is being transformed and generally is easier on the eye:

```html
<ng-container *ngIf="(canEdit$ | async)"> 
 <input 
  @fadeIn 
  type="text"
  class="form-control"
  [attr.placeholder]="(placeholder$ | async)"
  (input)="onInputChanged($event)"
 />
</ng-container>
```

When using multiple pipes, it may even be more important:

```html
<input 
 [value]="(value$ | async | uppercase | trim)"
/>
```

## Lifecycle Hooks

### Interfaces

Adding lifecycle hooks interfaces is not mandatory but a suggested practice, which I strongly recommend to follow.

### Order

When I look for lifecycle hooks, I usually head to the constructor and expect them to see all of them together and not mixed up with other class methods. Ideally, they should be defined in the same order they execute.

It is recommended to:

* always add interfaces

* add public and private properties above the constructor

* add methods right below the constructor and above the component’s methods

* add them all close to each other

* add them in the order they execute. Admittedly though, this is a bit harder to follow consistently, so I guess it’s the least important one

```typescript
class MyComponent implements OnChanges, OnInit, OnDestroy {
  // props
  constructor() {}
  
  ngOnChanges() {}
  ngOnInit() {}
  ngOnDestroy() {}
}
```

### Logic

Avoid directly writing any logic within the lifecycle hooks: encapsulate logic within private methods and call them within the lifecycle hooks:

```typescript
export class MyComponent implements OnInit {
  ngOnInit() {
    this.initialize();
  }
  
  private initialize() {
   // ...
  }
}
```

## Component Properties and Methods

Angular uses decorators for the component’s methods and properties in order to augment its functionality.

There are so many of them that defining a specific order to follow would be overwhelming, but the important thing I try to follow is to locate the properties and methods with the same decorator close to each other.

The following is what I would consider a bad example:

```typescript
export class MyComponent {
  @Input() value: string;
  @Output() valueChanged = new EventEmitter<string>();
  @Input() label: string;
}
```

And below is how I would write it; also, notice there’s an empty line in between groups of properties with the same decorator — I think it helps with readability:

```typescript
export class MyComponent {
  @Input() value: string;
  @Input() label: string;
  
  @Output() valueChanged = new EventEmitter<string>();
}
```

I don’t have a strong opinion on this, but try to locate private and public component properties not marked with any decorator separately from the decorated properties.

In my experience, mixing them up only leads to confusion and a feeling of chaos.

## Returning Values

Always leave one empty line when you return a value from a method or a function:

```typescript
 export class MyComponent {
   getUsersInAdministration () {
     const requestUrl = 'xxx';

     return this.http.get(requestUrl);
   }
 }
```

## Naming

When it comes to naming, I always have to think it twice or more to come up with a name that is understandable, unambiguous and easy-to-search:

* *understandable*: what does this do, at a glance?

* *unambiguous*: for example, if we have multiple click events on a single component, which one does this event refer to? So yeah, naming it *onClick* is **not** the way to go

* *easy-to-search*: I see naming code a bit like SEO: how will my users (teammates or me) search for this particular thing — and how can I write it to make sure they can search for it more easily?

### File Names

I like to use hyphen-case for all file names. I would think by now it’s a standard for Typescript projects, but I’ve seen quite a few variations, even in Angular projects, so I feel I have to mention this.

Examples:

* sign-up.component.ts

* profile-form.component.html

### Route Components

I tend to name route components with a suffix page.

For example, the authentication page would normally be called auth-page.component.ts — which tells me it’s a routed component, and I normally use it to wrap and display other components via router-outlet.

### Components

Some rules I tend to follow when naming components are:

* Try to use **no more than 3 words** (excluding the prefix). No specific reason why — they just don’t look very pretty. Of course, sometimes it’s simply not very easy to respect this rule

* Try to **avoid repeating words or contexts already used** with other components, as it would slow down while using the search function of my IDE, and also lead to mistakenly open other files, which is ultimately a waste of time and source of frustration

* At the same time, **also try not to be too generic**. For example: if we call a component settings — settings of what!? Help out a little here and give some further context (example: application-settings, profile-settings, organization-settings, etc.). 
No biggie for small applications, but at scale, it does make a difference

### Events Names

It seems simple and yet it’s not, especially with larger components with many events.

Here are a set of rules I try to follow:

* Do not prefix events/outputs names with on. The handler, instead, could be written with such prefix

* Don’t make me think: always specify the entity whose action referred, not only the action itself. 
If we are describing an event on a component whose value changed, the event change could be valueChange.
In my opinion, this is unambiguous and tells me what changed without having me question if this was the value, the status, or anything else

* Use past-sense or not (valueChange vs valueChanged)? This is controversial and I heard valid reasons on opposite sides, so it may be up for discussion for your and your team. 
As long as you agree on one single-way, I don’t think it’s *that* important. What do you think?

## ES Imports

Keeping your file imports ordered and neat is challenging, especially when using an IDE to auto-add them as you type. As your files grow, they tend to get quite messy.

This how I order my imports:

* 1. Angular imports always go at the top

* 2. Rx imports

* 3. Third parties (non-core)

* 4. Local/Project imports at the end

It’s also a good practice to leave a comment above each group
