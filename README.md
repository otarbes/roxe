# Rexursive-Observer

This is a small utility to observe changes to complex objects.
As Javascript devs know, native `observe` and `watch` methods were suffering of poor performance and were deprecated. Proxies then were created as a partial solution. Also RxJS implements greatly the Observable Pattern.

So I took them and merged them: _**R**e**x**ursive-observer_
The result is a typescript package that let you `subscribe` to objects and nested objects in a very easy way.


### Install

```sh
$ npm install --save rexursive-observer
```

<br>
<hr>

### Usage example


In this usage example, I'm supposing you are using Typescript to show you the real advantage of this package.

You simply need to instantiate a new class for every external object (not nested ones) you want to observe. Easily pass the interface that defines your Object as generic parameter of `ObservableObject`. This will let you, in Typescript, to access to object's properties, like
`nestedObjects.firstProperty`. Else, Typescript will not recognize it.

```typescript
interface CustomObject {
	// here lays you object definition
	firstProperty: number,
	secondProperty: {
		third: number,
		fiftyfive: {
			anotherNestedProperty: number
		}
	}
}

const nestedObjects = new ObservableObject<CustomObject>({
	firstProperty: 5,
	secondProperty: {
		third: 7,
		fiftyfive: {
			anotherNestedProperty: 55
		}
	}
});

// Subscribe to the object changes.

const firstPropertyObserver = nestedObjects.observe("firstProperty");
const subscription = firstPropertyObserver.subscribe({
	next: (newValue => {
		console.log("first property changed!");
	});
});

const sub1 = nestedObjects.observe("secondProperty.fiftyfive.anotherNestedProperty")
	.subscribe( ... );


// Else where, edit this object.

nestedObjects.firstProperty = 1;

nestedObjects.secondProperty = {
	third: 3,
	fiftyfive: {
		anotherNestedProperty: 4;
	}
}
```

<br>
<br>

___

## Documentation
___

**constructor()**

```typescript
new ObservableObject<T>(from: T, optHandlers: ProxyHandler<any>): T & ObservableObject<T>;
new ObservableObject(from, optHandlers);
```

**Arguments**:

| Key | Type | Description | Optional | Default |
|-----|:----:|-------------|:--------:|:-------:|
| obj | T    | The object you want to observe | `false` | - |
| optHandlers | ProxyHandler<any> | Other handlers through which the object changes developers may them want to pass. A set handler will be executed after the current one | `true` | `{}` |

<br>

___

<br>

**.observe()**

```typescript
observableObject.observe<T = any>(prop: string): Subject<T>;
```

**Description**:

Use this method to register a Subject to subscribe to.
To avoid confusion between Subject `.unsubscribe` and Subscription `.unsubscribe`, the returned Subject will unsubscribe only the registered `Subscriptions` without closing/stopping the Subject, even if (I know...), Subject shouldn't be reusable.

This methods accepts a generic type `T` that will be passed to the creation of the Subject.

<br>

**Returns**:

`Subject<T>`

<br>

**Arguments**:

| Key  | Type | Description |
|------|:----:|-------------|
| prop |string| The key-path to identify the property, or object, to observe. |

<br>

___
<br>

**.unsubscribeAll()**

```typescript
observableObject.unsubscribeAll(subscriptions: Subscription[]): void;
```

**Description**:
Performs unbscription on all the passed `Subscription`;

<br>
<br>

___
### Credits ⭐
___
This small package is a fork of a package, created by me, while working at [IdeaSolutions S.r.l.](http://www.ideasolutions.it/), an Italian company based in Naples, Italy. A great company to work for.

<br>
<br>

___

Made with ❤ in Italy.
Every contribution is welcome.
