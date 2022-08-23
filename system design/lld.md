## SOLID Principles

Advantages of these principles:
- Avoid Duplicate code
- Easy to maintain and understand
- Flexible software

### Single Responsibility

> A class should have only 1 reason to change.

```ts
// suppose we have a class called marker
class Marker {
    constructor(name: string, price: number) {
        this.name  = name;
        this.price = price;
    }
}

// a class called invoice which generate invoice for marker
class Invoice {
    constructor(marker: Marker, quantity: number) {
        this.marker   = marker;
        this.quantity = quantity;
    }

    public calculateTotal() {
        return (this.marker.price * this.quantity);
    }

    public printInvoice() {
        // prints invoice
    }

    public saveToDb() {
        // saves data to DB
    }
}

/** 
 * In above example Invoice class is doing these tasks
 * calculate total
 * printInvoice
 * saving to db
 * 
 * We can hold Single Responsibility priciple by having
 * InvoiceDao and InvoicePrinter
*/

class Invoice {
    constructor(marker: Marker, quantity: number) {
        this.marker   = marker;
        this.quantity = quantity;
    }

    public calculateTotal() {
        return (this.marker.price * this.quantity);
    }
}


class InvoiceDao {
    constructor(invoice) {
        this.invoice = invoice;
    }

    public saveToDB() {
        // save into db
    }
}


class InvoicePrinter {
    constructor(invoice) {
        this.invoice = invoice;
    }

    public print() {
        // print the invoice
    }
}
```

### Open/Close Principle
> Open for extension but closed for modification

```ts
class InvoiceDao {
    constructor(invoice) {
        this.invoice = invoice;
    }

    public saveToDB() {
        // save into db
    }

    // for adding a functionality we are modifying tested file (prone to bugs)
    public saveToFile() {
        // save to file
    }
}

interface InvoiceDao {
    public save(invoice);
}

class DatabaseInvoiceDao implements InvoiceDao {
    save(invoice) {
        // save to db
    }
}

class FileInvoiceDao implements InvoiceDao {
    save(invoice) {
        // save to file
    }
}
```

### Liskov Subsitution Principle
> If class B is subtype of class A, then we should be able to replace object of A with B without breaking the behaviour of the program

> Subclass should extend the capability of parent class not narrow it down

### Interface Segmented principle
> Interfaces should be such that client should not implement unnecessary functions they don't need.

```ts
interface RestaurantEmployee {
    washDishes();
    serveCustomers();
    cookFood();
}

class Waiter implements RestaurantEmployee {
    washDishes() {
        // i don't wash
    }

    serveCustomers() {
        // I serve
    }

    cookFood() {
        // i don't cook
    }
}

// New 
interface WaiterInterface {
    takeOrder();
    serveCustomers();
}

class Waiter implements WaiterInterface {

}
```

### Dependency Inversion
> Class should depend on interfaces rather than concrete class

```ts
class Macbook {
    constructor() {
        this.keyboard = new WiredKeyboard();
        this.mouse    = new WiredMouse();
    }
}

// New
class Macbook {
    constructor(keyboard: Keyboard, mouse: Mouse) {
        this.keyboard = keyboard;
        this.mouse    = mouse;
    }
}
```