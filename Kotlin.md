# Kotlin



## Data Type

1. Int
2. Float
3. Char
4. String
5. Boolean
6. Array
7. Any

## Variable Type

1. var
2. val

## Comparator and Logical Operators

Supports all the comparators and logical operator like other language

### Comparators

> <, >, ==, >=, <=, !=

Logical Operators

> &&, ||

## Control Flows

- **If** statment

- **When** expression

  ```kotlin
  when(number) {
      in 1..2 -> println('Number is between 1 & 2');
      in 3..4 -> println('Number is between 3 & 4');
      else {
          println('Number is > 4')
      }
  }
  
  val toDo = when(dayOfWeek) {
      1,2,3,4,5 -> "Go to School"
      6,7 -> "Weekend"
      else -> "Invalid day"
  }
  ```

  

- **While**, **do-while** Loop

- **For** Loop

  ```kotlin
  for (i in 1..5){
      println("$i") // 1 2 3 4 5
  }
  for (i in 5 downTo 1){//** Reverse
      println("$i") //5 4 3 2 1
  }
  for (i in 5..20 step 2){//** Advances by 2
      println("$i") //5 7 9 ..19
  }
  ```

  

## Null-Safe operators

- Safe call operator `?.`

  ```kotlin
  val myString: String? = null
  println("Length: ${myString?.length}") //***
  ```

  

- Safe call with let `?.let`

  ```kotlin
  myString?.let {
  	println("Length: ${it.length}")//*** //Null value will not be printed
  }
  ```

  

- Elvis operator `?:`

  ```kotlin
  val length = myString?.length ?: -1 //***//Like ternary operator
  ```

  

- Not-null Assertion `!!` 

  ```kotlin
  val length = myString!!.length //*** //!! says it can't be null
  ```

  