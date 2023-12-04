# Object Oriented PHP

## Benefits
- Data and functions are grouped together
- Reduces complexity
- Allows for reuse
- Changes only need to happen in one place
- Code is better organised and more coherent
- Makes it easy to visualise the design of your application

## Objects
In OOP we combine related variables and functions into a cohesive unit which we call an object.
- properties: variables which belong to objects
- methods: functions which belong to objects

## Classes
In order to need to make an object, you need to make a class which is the blueprint of what the object will look like, what its attributes are going to be.

```
class Product 
{
// properties
    public $name = "soap";
    public $price = 40;

// constructor
    public function __construct($name = "Elias", $price = 500)
    {
        $this->name = $name;
        $this->price = $price;
    }

// methods
    public function priceAsCurrency($divisor = 1, $currencySymbol = "$")
    {
        // $this is a pseudo variable, refers to whatever instance you are using 
       $priceAsCurrency = $this->price / $divisor;
       
       return $currencySymbol . $priceAsCurrency;
    }
}
$product = new Product();
// named argument
print $product->priceAsCurrency(currencySymbol: "€");
```

## Constructors
You’ll use constructors to do whatever should always be done - and done first - when an object of this class is made. 

### Constructor promoted properties (PHP 8.0) 
```
class Product
{
    public function __construct(public $name = "Soap", public $price = 100)
    {
    }
}
```

## Type hinting
```
class Song 
{
    private int|float $rating;

// $name must be of type string
    public function __construct(public string $name, public int $numberOfPlays) 
    {
    }

    public function setRating(int|float $rating): void
    {
        $rating = max(0, $rating);

        $this->rating = min(5, $rating);
    }

    public function getRating(): int|float
    {
       return $this->rating;
    }
```

## Class and return type declarations
```
class Playlist 
{
    private $songs = [];

// $song must be of type Song
// doesn't return anything 
    public function addSong(Song $song): void
    {
        $this->songs[] = $song;
    }

// returns array
    public function getSongs(): array
    {
       return $this->songs;
    }
}
```

## Inheritance
Combats redundancy and duplication by allowing a child class to extend a parent class.
```
abstract class Book 
{
    public $title;
    public $author;
    public $price;

    public function __construct
    (
        string $title, 
        string $author, 
        int $price, 
    )
    {
        $this->title = $title;
        $this->author = $author;
        $this->price = $price;
    }

    public function getTitle(): string 
    {
        return $this->title;
    }

    public function getAuthor(): string 
    {
        return $this->author;
    }

    public function getPrice(): int 
    {
        return $this->price;
    }

    public function getPriceAsCurrency(): string
    {
        return "$" . $this->price / 100;
    }

// will override parent's method
    public function print():string
    {
        return "{$this->title}, {$this->author}";
    }

    abstract public function write(): string;

}

class PhysicalBook extends Book
{
    public $weight;
    
    public function __construct
    (
        string $title, 
        string $author, 
        int $price, 
        int $weight
    )
    {
        // :: is a scope resolution operator 
        // call parent's constructor
        parent::__construct($title, $author, $price);
        $this->weight = $weight;
    }

    public function getWeight(): int 
    {
        return $this->weight;
    }

    public function print():string
    {
        return "{$this->title}, {$this->author}, weight: {$this->weight}";
    }

    public function write(): string
    {
        return "{$this->title}, weight: {$this->weight}";
    }

}
```

## Visibility 
- **Public** properties and methods can be accessed anywhere in your code.
- **Protected** properties and methods can only be accessed within the declaring class or from a subclass.
- **Private** properties and methods can only be accessed within the declaring class. They are not visible anywhere else.


## Encapsulation
- The functionality is defined in one place and not in multiple places.
- It is defined in a logical place, e.g the same place as its data.
- The data inside our objects can’t unexpectedly or unwantedly modified by external code in a completely different part of our program.


## Static keyword
- Available anywhere in your program
- Easy to setup and access
- Same value is available to every object instance of that class
- Directly from class, not an object

```
class Connection 
{
    private static int $count = 0;

    public function __construct() {
        self::$count++;
    }

    public static function getCount(): int 
    {
        return self::$count;
    }
}
```

## Constants
```
class Http 
{
    public const NOT_FOUND = 404;
    public const OK = 200;
    public const CREATED = 201;
}
```

## Abstract keyword
```
// can't instantiate class
abstract class Book 
{
    public $title;
    public $author;
    public $price;

    public function __construct
    (
        string $title, 
        string $author, 
        int $price, 
    )
    {
        $this->title = $title;
        $this->author = $author;
        $this->price = $price;
    }

    public function getTitle(): string 
    {
        return $this->title;
    }

    public function getAuthor(): string 
    {
        return $this->author;
    }

    public function getPrice(): int 
    {
        return $this->price;
    }

     public function getPriceAsCurrency(): string
    {
        return "$" . $this->price / 100;
    }

      public function print():string
    {
        return "{$this->title}, {$this->author}";
    }

// this method needs to be included in every extension of this class
    abstract public function write(): string;

}
```
