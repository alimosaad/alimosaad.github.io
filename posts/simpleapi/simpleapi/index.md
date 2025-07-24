# Simple Api Using Spring FrameWork

 
# How to Create API Using Spring FrameWork

## Introduction
We need to establish communication between a client(applications) and a
server or establish communication between two backend services.
This is REST APIs come into play.

## What is REST?
REST stands for REpresentational State Transfer. REST was an architectural approach designed to make the optimum use of the HTTP protocol.
it's a set of rules that simplifies the communication over the internet using HTTP Methods.
- Get  -->  Used to fetch data from dataBase. 
- Post -->  Used to send data to dataBase.
- Put  -->  Used to update data on the dataBase. 
- Delete --> Used to delete data from the dataBase.

REST is not a standard but a style whose purpose is to constrain our architecture to a client-server architecture and is designed to use stateless communication protocols like HTTP.  Meaning each request from a client contains all the necessary information for the server to process it, without relying on previous requests. 

# HTTP Standard Status Codes
The status codes defined in HTTP are the following:
- 200: Success
- 201: Created
- 401: Unauthorized
- 404: Resource Not Found
- 500: Server Error

## What is Spring Boot REST?
Spring boot REST is a part of the spring frameWork that simplifies the creation of REST APIs.
It provides built-in support for handling HTTP requests and responses, data conversion,
and error handling. This allows developers to focus on writing business logic instead of managing low-level configurations.
# Benefits of spring boot REST 
- **Easy to use**: Reduces boilerplate code and simplifies development.
- **Scalable**: Can handle multiple requests efficiently.
- **Flexible**: Supports JSON, XML, and other formats.
- **Integration**-friendly: Works well with databases, security tools, and third-party services.

## Steps to create RESTful API in spring boot
### Step 1: Setting up the Spring Boot Project
1. New -> project ->springboot project 
2. Configure your project 
3. Add dependancies: 
    - Spring web starters (for REST APi dev)
    - Spring Boot DevTools (for automatic reloads during development)
    - spring Data JPA (for DataBase connectivity) --> In this blog i don't use DataBase 
    - H2 DataBase (for in-memory DataBase)
### Step 2: Create Data Model Class
**Data Model Class represent the data API will handle create**
**Book.java**
```java
package com.example.springbootapi.restApiModel;

public class Book {
    private int id;
    private String title;
    private String author;
    public Book(){}
    public Book(int id, String title, String author){
        this.id=id;
        this.title=title;
        this.author=author;
    }
    public int getId(){
        return id;
    }

    public void setId(int id) {
        this.id = id;
    }

    public String getTitle() {
        return title;
    }

    public void setTitle(String title) {
        this.title = title;
    }

    public String getAuthor() {
        return author;
    }

    public void setAuthor(String author) {
        this.author = author;
    }
}

```
### Step 3: Create Service interface and Service class
`Service interface` contain all service methods app use and `service implementation` implement the service interface.
`This is Service layer` That handle data access.
`Service Class` encapsulate the business logic and interact with DataBase.
```java
// This is Service Interface
package com.example.springbootapi.restApiService;

import com.example.springbootapi.restApiModel.Book;
import org.springframework.stereotype.Repository;

import java.util.List;
@Repository 
public interface BookService {
    List<Book> findAllBooks();
    Book findBookByID(int id);
    void deleteBook(int id);
    void addBook(Book book);
    void updateBook(Book book,int id);
}

```
```java
// This is Service Class implementation
package com.example.springbootapi.restApiService;

import com.example.springbootapi.restApiModel.Book;
import org.springframework.stereotype.Service;

import java.util.ArrayList;
import java.util.Arrays;
import java.util.List;
@Service
public class BookServiceImplementation implements BookService{
    private List<Book> books=new ArrayList<>(Arrays.asList(
            new Book(1,"War and Peace","Leo Tolstoy"),
            new Book(2,"Ulysses","James Joyce"),
            new Book(3,"The Great Gatsby","F. Scott Fitzgerald")
    ));

    @Override
    public List<Book> findAllBooks() {
        return books;
    }

    @Override
    public Book findBookByID(int id) {
        return books.stream().filter(t -> t.getId() == (id)).findFirst().orElse(null);
    }

    @Override
    public void deleteBook(int id) {
        books.removeIf(t->t.getId()==id);
    }
    @Override
    public void addBook(Book book) {
         books.add(book);
    }

    @Override
    public void updateBook(Book book,int id){
        for (int i=0;i<books.size();i++){
            Book book1=books.get(i);
            if(book1.getId()==id){
                books.set(i,book);
                return;
            }
        }
    }
}

```
### Step 4: Create The Controller
Create controller class and annotate it as @RestController to tell spring that is a controller.
`Controller` is a java class that maps URI to a method when method is executed it return response. `@RestController` automatically handles conversion to json and autowire servise object.
```java
package com.example.springbootapi.restApiController;

import com.example.springbootapi.restApiModel.Book;
import com.example.springbootapi.restApiService.BookServiceImplementation;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.web.bind.annotation.*;

import java.util.List;

@RestController
@RequestMapping("/api")
public class BookController {
    @Autowired
    private BookServiceImplementation bookServiceImplementation;

    @GetMapping("/books")
    public List<Book> getAllBooks(){
        return bookServiceImplementation.findAllBooks();
    }
    @PostMapping( "/books")
    public void addBook(@RequestBody Book book){
        bookServiceImplementation.addBook(book);
    }
    @GetMapping("/books/{id}")
    public Book getBook(@PathVariable int id){
        return bookServiceImplementation.findBookByID(id);
    }
    @PutMapping("books/{id}")
    public void updateBook(@RequestBody Book book,@PathVariable int id){
        bookServiceImplementation.updateBook(book,id);
    }

    @DeleteMapping("/books/{id}")
    public void deleteBook(@PathVariable int id){

        bookServiceImplementation.deleteBook(id);

    }

}

```
### step 5: Run application
- Run application
### Step 6 :
Test your Api using `browser` or `Postman` GET http://localhost:8080/api/books --> to get all books 

## Additional Features
1. `connecting DataBase`
2. `change server port` --> Using apllication.properties file 
    ```java
    // default port is 8080
    server.port=3000 // change port to 3000
    // if you use 0 the application use new port each run (random port)

    ```
3. Securing the API
4. Deploying the API

## Thanks 
Thank you for taking the time to read this Blog! If you found it helpful and enjoyed the content, please cplease don’t forget to keep me in your prayers. Your support and encouragement mean a lot and will motivate me to continue writing and sharing more valuable insights in the future. Thank you!
