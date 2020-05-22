# Typescript

Resources:
- [Typescript Todo tutorial](https://ts.chibicode.com/todo/)


## Basics

### Create a type

```ts
type Book = {
  id: number
  title: string
  read: boolean
}
```

### Assign a type to a variable

```ts
const foo: Book = {
  id: 1,
  title: 'Est-ce qu\'on ment aux gens qu\'on aime ?',
  read: true
}
```

### Specify argument and return types of a function

```ts
function toggleReadStatus(book: Book): Book {
  return {
    ...book,
    read: !book.read
  }
}
```

### Set an object type property as read only

```ts
type Book = {
  readonly id: number
  readonly title: string
  done: boolean
}
```

### Set all object type properties as read only

```ts
type Book = Readonly<{
  id: number
  title: string
  done: boolean
}>
```

or using mapped types to convert a type into another:

```ts
type Book = { 
  id: number 
}

type ReadonlyBook = Readonly<Book>
```

### Set an array passed as a function argument as readonly

This prevents accidentaly modifying the original array instead of creating a new one.

```ts
function markAllAsRead(books: readonly Book[]): Book[] {
   //...
}
```

### Use literal value to specify exactly what value is allowed for a property

```ts
type ReadBook = Readonly<{
  id: number
  title: string
  done: true
}>

function markAllAsRead(books: readonly Book[]): ReadBook[] {
   //...
}
```

### Use intersection types to combine two types and override values

```ts
type Book = Readonly<{
  id: number
  title: string
  done: boolean
}>

type ReadBook = Book & {
  readonly done: true;
}
```

### Use union types to allow several types for a property

```ts
type Genre = 'science-fiction' | 'fantasy' | { custom: string }

type Book = Readonly<{
  id: number
  title: string
  done: boolean
  genre: Genre
}>
```

### Make a property optional

```ts
type Book = Readonly<{
  id: number
  title: string
  done: boolean
  genre?: Genre
}>
```
