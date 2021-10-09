# Bookstore App

This project will be a series of experiments with different software development technologies using a simple bookstore application as the basis for implementation of these technologies.

## Structure of the Project

The project will include frontend and backend projects separately, and each of these will have their respective implementations of the bookstore app design.

Each technology stack will have its own branch.

## Design

### User personas
* User: a book lover who wants to place some orders.
* Admin: a person who maintains the system.

### User Stories – User
* U1: As a user, I want to create an account so that I can order books.
* U2: As a user, I want to log in to my account so that I can start a session for ordering books.
* U3: As a user, I want to renew my forgotten or lost password, so that I can continue ordering books with my existing account even when I have lost or forgotten my password.
* U4: As a user, I want to view a list of most recently published books that fit my interests so that I can easily find out what titles there are to be bought in the fields of my interest.
* U5: As a user, I want to search books by title, author, category, price or description content, so that I can quickly find books that interest me.
* U6: As a user, I want to view a single book with its cover, description, technical details, preview and reviews so that I can know as much as possible about the book before I make my decision to buy it.
* U7: As a user, I want collect the books into a shopping cart before I place an order, so that I can freely shop around and make a batch of purchases before I proceed to the "bureaucracy" of placing the order.
* U8: As a user, I want to place the order by giving the specifications about how the order should be shipped, where, and which payment method should be used so that I can pay for the books I want to buy have them delivered to me.
* U9: As a user, I want to be able to give one review per books that I have demonstrably purchased so that others could learn from my own experiences about the book and have more information to decide by whether they are interested in the book or not.
* U10: As a user, I want to have a contact form so that I can send messages with questions, suggestions, feedback and whatever I need to inform the administrators.

### User Stories – Admin
* A1: As an admin, I want to log in and change my password, so that I can authenticate as an admin to administer this service.
* A2: As an admin, I want to create a book, so that it can be found and bought in the service.
* A3: As an admin, I want to update a book, so that I can fix and update any information that needs to be changed.
* A4: As an admin, I want to delete a book, so that I can manually remove an item in the store whenever it is needed.
* A5 As an admin, I want to view the list of users sorted by last names, so that I can browse the user information to find users.
* A6: As an admin, I want to search for users by first and last names and user ids, so that I can quickly find a user whose information I need at the moment.
* A7: As an admin, I want to delete a user so that I can block an unwanted user from using the service.
* A8: As an admin, I want to view error logs that have been generated by the service so that I know whenever there is something to be fixed in the service.
* A9: As an admin, I want to have an inbox where I receive messages from users, so that I can receive feedback from customers and reply to their messages.

### Data Model

#### Correspondence with U User Stories
* U1: User
* U2
    * User.email
    * User.password
* U3
    * User.email
    * User.password
* U4
    * Book
    * User.interests
* U5
    * Book.title
    * Book.author
    * Book.category
    * Book.price
    * Book.description
* U6
    * Book.coverImg
    * Book.description
    * (what exactly are the "technical details" that should be visible?)
    * Book.preview
    * Book.reviews
* U7: Order
* U8
    * Order.Book[]
    * Order.shippingOption
    * Order.User.shippingAddress
    * Order.paymentMethod (?)
* U9
    * Review.ratings
    * Review.text
    * Review.User
* U10
    * InboxMessage.User
    * InboxMessage.text
    * InboxMessage.category

#### Correspondence with the A User Stories
* A1
    * AdminUser.id
    * How to create a secure way to authorize a user with admin privileges?
* A2: Book
* A3: Book
* A4: Book
* A5 User.lastName
* A6
    * User.id
    * User.firstName
    * User.lastName
* A7: User
* A8
    * InboxMessage
    * Might make more sense to use a different approach than saving the errors to the DB
* A9
    * InboxMessage.text
    * InboxMessage.User

### API Functions

#### Correspondence with the U User Stories
* U1: registerUser
* U2: login
* U3: renewPassword
* U4
    * Book.getMany()
        * default: sort by time, descending
        * default: if logged in, filter by User.interests
* U5
    * Book.getMany({
        query: {
            title
            author
            category
            description
        },
        sort: {
            title
            author
            price
        }
    })
* U6
    * Book.getOne(id)
    * Book.getReviews(bookId)
* U7: N/A (frontend only)
* U8
    * Order.create(Order)
    * Order.update(orderId, PartialOrder)
* U9
    * Review.create(bookId, userId, Review)
* U10
    * InboxMessage.create(InboxMessage)

#### Correspondence with the A User Stories
* A1
    * User.create(User) (this is for admin users primarily)
    * User.login(username, password)
    * User.renewPassword(email)
* A2
    * Book.create(Book)
* A3: 
    * Book.update(bookId, PartialBook)
* A4
    * Book.delete(bookId)
* A5 
    * User.getMany()
        * default: sort by lastName, ascending
* A6
    * User.getMany({
        query: {
            id
            firstName
            lastName
        }
        sort {
            lastName
        }
    })
* A7
    * User.delete(userId)
* A8
    * If self-created
        * InboxMessage.create(InboxMessage)
        * InboxMessage.getOne()
        * InboxMessage.getMany()
    * If using an external package or service
        * Module for handling a third-party package or service
* A9
    * InboxMessage.create(InboxMessage)
    * InboxMessage.markAsRead(id)
    * InboxMessage.delete(id)