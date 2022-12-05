# Uvod u JavaScript prototipove

Objekti u JavaScriptu imaju interni property poznato kao prototip. To je jednostavna referenca na drugi objekat i sadrži zajedničke atribute/properties u svim instancama objekta. Atribut prototipa objekta specificira objekt od kojeg nasleđuje svojstva. npr.

```
let numArray = [14, 52, 5, 10];
```

Objekat Array ima prototip **Array.prototype** i instanca objekta, **num**, nasljeđuje properties objekta Array.

<img width="1068" alt="Screenshot 2022-12-05 at 14 48 54" src="https://user-images.githubusercontent.com/21141150/205653296-40b02181-e23d-44c6-9828-6301b216b028.png">

Zbog toga možete koristiti metodu kao što je **sort()** na instanci polja.

```
console.log(numArray.sort()); // -> [5, 10, 14, 52]
```

Ista stvar vrijedi i za druge objekte kao što su **Date()**, **String()**, **Function()** itd.

Kada se izgradi funkcija konstruktora (poznata i kao pseudo classical inheritance), novostvoreni objekti nasljeđuju svojstva prototipa funkcije konstruktora. One (konstruktorske funkcije) su izgrađene za inicijalizaciju novostvorenih objekata. Konstruktori se pozivaju korištenjem ***new*** ključne riječi.

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
