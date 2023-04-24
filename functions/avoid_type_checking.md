### Avoid type checking

TypeScript is a strict syntactical superset of JavaScript and adds optional static type checking to the language.  
Always prefer to specify types of variables, parameters and return values to leverage the full power of TypeScript features.  
It makes refactoring more easier.

**Bad:**

```js
function travelToTexas(vehicle: Bicycle | Car) {
  if (vehicle instanceof Bicycle) {
    vehicle.pedal(currentLocation, new Location('texas'));
  } else if (vehicle instanceof Car) {
    vehicle.drive(currentLocation, new Location('texas'));
  }
}
```

**Good:**

```js
type Vehicle = Bicycle | Car;

function travelToTexas(vehicle: Vehicle) {
  vehicle.move(currentLocation, new Location('texas'));
}
```
