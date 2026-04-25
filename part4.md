# Can a client-side developer prevent IDOR?

No. They can only prevent accidental access by honest users. Malicious users ignore the UI.

# Why is the backend the only place for security?

Because the backend is the only "trusted" environment. The client is an "untrusted" environment that can be fully manipulated by the end-user.

# What is the "IDOR" pattern?

It is when an application uses an internal identifier (like a database ID) to access an object without verifying if the user has permission to that specific instance of the object.

# CardInterface.kt

```java
@PUT("/api/use/{card_number}")
fun useCard(@Path("card_number") card_number: Int?, @Header("Authorization") authHeader: String): Call<Card?>
```

# usecard.kt

```java
client.useCard(card?.id, token)?.enqueue(...)
```

## Keytakeaways

- Client-Side Invisibility: Even if you write code in UseCard.kt to "hide" the use button for cards the user doesn't own, it doesn't matter. An attacker can use curl to send a PUT request directly to https://appsec.moyix.net/api/use/104 with their own valid login token. The server will accept the request because it only checks "Is this user logged in?" and not "Does this user own this card?".

- The "Proper" Backend Fix: The server-side code must query the database to ensure the user_id associated with the provided token matches the owner_id of the card matching the card_number in the database.

- Trusting Input: The golden rule of secure development is "Never trust user input." In this scenario, the card_number in the URL is user input, and the server is blindly trusting it to point to a resource the user is allowed to modify.
