# Uvod u JavaScript prototipove

Objekti u JavaScriptu imaju interni property poznat kao prototip. To je jednostavna referenca na drugi objekat i sadrži zajedničke atribute/properties u svim instancama objekta. Atribut prototipa objekta specificira objekt od kojeg nasleđuje svojstva. npr.

```
let numArray = [14, 52, 5, 10];
```

Objekat Array ima prototip **Array.prototype** i instanca objekta, **num**, nasljeđuje properties objekta Array.

<img width="849" alt="Screenshot 2022-12-05 at 18 40 36" src="https://user-images.githubusercontent.com/21141150/205705521-fffd20c0-f0c6-4207-840d-d90c1729bd84.png">

Zbog toga možete koristiti metodu kao što je **sort()** na instanci polja.

```
console.log(numArray.sort()); // -> [5, 10, 14, 52]
```

Ista stvar vredi i za druge objekte kao što su **Date()**, **String()**, **Function()** itd.

Kada se izgradi funkcija konstruktora (poznata i kao pseudo classical inheritance), novostvoreni objekti nasleđuju svojstva prototipa funkcije konstruktora. One (konstruktorske funkcije) su izgrađene za inicijalizaciju novostvorenih objekata. Konstruktori se pozivaju korištenjem ***new*** ključne reči.

```
// Ovde funkcija ima ulogu funkcijskog konstruktora.
const Car = function(color, model, dateManufactured) {
  this.color = color;
  this.model = model;
  this.dateManufactured = dateManufactured;
};

// Ovim dodajemo metodu getColor Car objektu
Car.prototype.getColor = function() {
return this.color;
};

// Ovim dodajemo metodu getModel Car objektu
Car.prototype.getModel = function() {
return this.model;
};

// Ovim dodajemo metodu carDate Car objektu
Car.prototype.carDate = function() {
return `This ${this.model} was manufactured in the year ${this.dateManufactured}`
}

// Inicijalizacija func. konstruktora
let firstCar = new Car('red', 'Ferrari', '1985');

// Ispis
console.log(firstCar);
console.log(firstCar.carDate()); // à This Ferrari was manufactured in the year 1985.
```
<img width="529" alt="Screenshot 2022-12-05 at 17 04 31" src="https://user-images.githubusercontent.com/21141150/205684848-e4af7d2b-3c6d-4c5a-9f3b-4ae860429de8.png">

Iz gornjeg primera, funkcije **carDate**, **getColor** i **getModel** svojstva su objekta **Car** i instance objekta Car kao što je firstCar mogu naslijediti sva njegova svojstva.

## Lanac prototipa

Kada objekat dobije zahtjev za svojstvo koje nema, njegov prototip će se tražiti u drugom prototipu, zatim prototipu prototipa, i tako dalje. Dakle, ko je prototip objekta? To je predak prototip, entitet koji stoji iza gotovo svih objekata, **Object.prototype**. Mnogi objekti nemaju izravno **Object.prototype** kao svoj prototip, već umesto toga imaju drugi objekat koji pruža drugačiji skup zadanih svojstava. Funkcije proizlaze iz **Function.prototype**, a arrays proizlaze iz **Array.prototype** i tako dalje.

```
let protoCar = {
  color: 'Red',
  startEngine(model) {
    console.log(`Engine of ${this.brand} ${model} is started`);
  }
};

let sportCar = Object.create(protoCar);
sportCar.brand = "Ferrari";
sportCar.startEngine("458");
// → Engine of Ferrari 458 is started
```

**ProtoCar** deluje kao storage za svojstva koju dele svi auti. Pojedinačni objekat car, poput **sportCar**, sadrži svojstva koja se odnose samo na njega samog. U ovom slučaju njegov tip i izvodi zajednička svojstva iz svog prototipa.

Pogledajmo ovaj kod:

```
let mainObject = {
  x: 2
};

let myObject = Object.create( mainObject );

for (let k in myObject) {
  console.log("found: " + k);
}

// found: x
("x" in myObject);  // true
```

Ali, ako **x** nije pronađen na myObjectu, njegov lanac prototipa, ako nije prazan, on se proverava. Ovaj proces se nastavlja sve dok se ne pronađe odgovarajući naziv svojstva ili dok se lanac prototipa ne završi. Ako se do kraja lanca ne pronađe odgovarajuće svojstvo, konačni rezultat operacije je **undefined**.

## Napomena 

```
let protoCar = {
  color: 'Red',
  startEngine(model) {
    console.log(`Engine of ${this.brand} ${model} is started`);
  }
};

let sportCar = Object.create(protoCar);
sportCar.brand = "Ferrari";
sportCar.startEngine("458");
// → Engine of Ferrari 458 is started
```

Gore naveden kod se može takođe napisati kao:

```
let protoCar = function(brend, model, color) {
  this.brend = brend;
  this.model = model;
  this.color = color;
};

protoCar.prototype.getColor = function() {
  return this.color;
}
protoCar.prototype.startEngine = function() {
  console.log(`Engine of ${this.brand} ${this.model} is started`);
}

let sportCar = new protoCar('Ferrari', '458!', 'Red');
sportCar.startEngine();
```

