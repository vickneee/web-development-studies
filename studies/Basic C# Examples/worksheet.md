# C#

## Examples:

### 1. Variables
- **Types of Variables**:
  ```csharp
  int integerNumber = 5;
  string text = "Hello";
  char letter = 'A';
  bool isCSharpFun = true;
  ```

- **Variable Conversion**:
  ```csharp
  // From string to int
  string numberAsString = "123";
  int convertedNumber = int.Parse(numberAsString);
  
  // From int to string
  int number = 123;
  string numberText = number.ToString();
  
  // From string to char
  string characterString = "B";
  char character = characterString[0];
  ```

### 2. Lists and Arrays
- **Adding to List**:
  ```csharp
  List<string> fruits = new List<string>();
  fruits.Add("Apple");
  ```

- **Removing from List**:
  ```csharp
  fruits.Remove("Apple");
  ```

- **Searching in List**:
  ```csharp
  if (fruits.Contains("Banana")) {
      Console.WriteLine("Banana is in the list");
  }
  ```

### 3. If, Else If, Else
  ```csharp
  if (convertedNumber < 10) {
      Console.WriteLine("Number is less than 10");
  } else if (convertedNumber == 10) {
      Console.WriteLine("Number is 10");
  } else {
      Console.WriteLine("Number is greater than 10");
  }
  ```

### 4. Method Definition
- **Parameters**:
  ```csharp
  void Greet(string name) {
      Console.WriteLine($"Hello, {name}!");
  }
  ```

- **Return Types**:
  ```csharp
  int Add(int a, int b) {
      return a + b;
  }
  ```

- **Overloading**:
  ```csharp
  void Display(string message) {
      Console.WriteLine(message);
  }
  void Display(int number) {
      Console.WriteLine(number);
  }
  ```

- **Tuples**:
  ```csharp
  (int, string) GetPerson() {
      return (1, "Alice");
  }
  ```

### 5. Switch Case
  ```csharp
  switch (letter) {
      case 'A':
          Console.WriteLine("Letter is A");
          break;
      case 'B':
          Console.WriteLine("Letter is B");
          break;
      default:
          Console.WriteLine("Other letter");
          break;
  }
  ```

### 6. Loops
- **Nested Loops**:
  ```csharp
  for (int i = 0; i < 3; i++) {
      for (int j = 0; j < 2; j++) {
          Console.WriteLine($"i = {i}, j = {j}");
      }
  }
  ```

- **Breaking a Loop**:
  ```csharp
  for (int i = 0; i < 10; i++) {
      if (i == 5) break;
      Console.WriteLine(i);
  }
  ```

### 7. Creating Objects
- **Object Lists**:
  ```csharp
  List<Person> people = new List<Person>();
  Person alice = new Person("Alice");
  people.Add(alice);
  ```

### 8. Enums
  ```csharp
  enum Days { Monday, Tuesday, Wednesday, Thursday, Friday };
  Days today = Days.Monday;
  ```

### 9. Classes
- **Creating Classes**:
  ```csharp
  public class Person {
      public string Name { get; set; }
      public Person(string name) {
          Name = name;
      }
  }
  ```

- **Constructors and Fields**:
  ```csharp
  // Inside the Person class
  private int age;
  public Person(string name, int age) {
      Name = name;
      this.age = age;
  }
  ```

- **Public and Private**:
  ```csharp
  // Public method
  public void SayHello() {
      Console.WriteLine("Hello!");
  }
  
  // Private method
  private void WhisperSecret() {
      Console.WriteLine("I love C#");
  }
  ```

## More Examples:

### 1. Variables and Conversions
Using a custom type conversion:

```csharp
class Temperature {
    public float Celsius { get; }

    public Temperature(float celsius) {
        Celsius = celsius;
    }

    public static explicit operator Temperature(float fahrenheit) {
        return new Temperature((fahrenheit - 32) * 5 / 9);
    }

    public override string ToString() {
        return $"{Celsius}°C";
    }
}

float fahrenheit = 86f;
Temperature temp = (Temperature)fahrenheit;
Console.WriteLine(temp);  // Outputs: "30°C"
```

### 2. Lists and Arrays
Complex list operations including finding an item with LINQ:

```csharp
List<string> cities = new List<string> { "New York", "London", "Mumbai", "Tokyo" };

// Find a city that starts with 'L'
string foundCity = cities.FirstOrDefault(city => city.StartsWith("L"));

if (foundCity != null) {
    Console.WriteLine(foundCity);  // Outputs: "London"
}
```

### 3. Control Flow with If-Else and Ternary Operator
Here's a more complex decision-making process using if-else:

```csharp
int score = 75;
string grade;

if (score >= 90) {
    grade = "A";
} else if (score >= 80) {
    grade = "B";
} else if (score >= 70) {
    grade = "C";
} else if (score >= 60) {
    grade = "D";
} else {
    grade = "F";
}

Console.WriteLine($"Score: {score}, Grade: {grade}");  // Outputs: "Score: 75, Grade: C"
```

### 4. Methods with Parameters and Overloading
Using a class with methods that can calculate area in different ways:

```csharp
public class Geometry {
    public static double Area(double radius) {
        // Area of a circle
        return Math.PI * radius * radius;
    }

    public static double Area(double width, double height) {
        // Area of a rectangle
        return width * height;
    }
}

double circleArea = Geometry.Area(5);
double rectArea = Geometry.Area(4.5, 6);

Console.WriteLine($"Circle Area: {circleArea}");
Console.WriteLine($"Rectangle Area: {rectArea}");
```

### 5. Switch Case with Pattern Matching
A switch statement that uses the new C# pattern matching features:

```csharp
object value = "Hello";

switch (value) {
    case int i:
        Console.WriteLine($"Integer: {i}");
        break;
    case string s when s.Length > 5:
        Console.WriteLine($"Long string: {s}");
        break;
    case string s:
        Console.WriteLine($"String: {s}");
        break;
    default:
        Console.WriteLine("Unknown type!");
        break;
}
```

### 6. Loops with Break and Continue
Iterating over a collection with specific conditions:

```csharp
for (int i = 1; i <= 10; i++) {
    if (i % 

```csharp
3 == 0) {
        Console.WriteLine($"{i} is divisible by 3. Skipping!");
        continue; // Skip the rest of this loop iteration
    }
    if (i == 8) {
        Console.WriteLine("Reached 8, breaking!");
        break; // Exit the loop
    }
    Console.WriteLine(i);
}
```

### 7. Object Creation with a List
Here's how you might maintain a list of objects:

```csharp
public class Book {
    public string Title { get; private set; }
    public string Author { get; private set; }

    public Book(string title, string author) {
        Title = title;
        Author = author;
    }
}

List<Book> library = new List<Book> {
    new Book("1984", "George Orwell"),
    new Book("The Great Gatsby", "F. Scott Fitzgerald")
};

foreach (var book in library) {
    Console.WriteLine($"{book.Title} by {book.Author}");
}
```

### 8. Enums with Switch Case
Using enums in a switch case for state management:

```csharp
enum TrafficLight {
    Red,
    Yellow,
    Green
}

TrafficLight light = TrafficLight.Yellow;

switch (light) {
    case TrafficLight.Red:
        Console.WriteLine("STOP");
        break;
    case TrafficLight.Yellow:
        Console.WriteLine("PREPARE");
        break;
    case TrafficLight.Green:
        Console.WriteLine("GO");
        break;
}
```

### 9. Classes with Properties and Access Modifiers
Implementing a class with public and private access modifiers:

```csharp
public class User {
    private string password; // Private field, not directly accessible outside the class

    public string Username { get; set; } // Public property, accessible from anywhere

    public User(string username) {
        Username = username;
    }

    public void SetPassword(string newPassword) {
        password = newPassword; // Can set private field within the class
    }

    public bool CheckPassword(string attempt) {
        return password == attempt; // Can access private field within the class
    }
}

User user = new User("alice");
user.SetPassword("secret");
bool isPasswordCorrect = user.CheckPassword("secret");
Console.WriteLine($"Password correct: {isPasswordCorrect}");
```