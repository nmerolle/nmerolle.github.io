---
layout: post
title:      "Authentication in Sinatra Using Bcrypt"
date:       2021-04-28 16:59:22 +0000
permalink:  authentication_in_sinatra_using_bcrypt
---

Bcrypt is a password-hashing function that can be implemented in several languages including C, C++, Java, Javascript, Python, Perl and in our case Ruby as well as other languages. Password hashing takes a plaintext password a user types in and creates a hash of the password. Hashing is a one way mathematical function unlike encryption which is two way, a two way mathematical function like encryption can be decrypted if a hacker weer able to get their hands on a decryption key a hash cannot be decrypted and has no key to worry about securing.

A hash is an alphanumeric 31 character string like IjZAgcfl7p92ldGxad68LJZdL17lhWy. It is important to note that particular hashes correspond to plaintext passwords. So if two users have the same password they would also use the same hashes. This makes hashes susceptible to what are called “Rainbow Table” attacks. Rainbow tables are just tables of hashes that correspond to popularly used passwords. If a hacker gets access to a database they can simply iterate through the hashes stored using the table and possibly find some users passwords.

To further secure users passwords from Rainbow Table attacks bcrypt also adds a “salt” to the hashed password. Salts are 16 byte (64 bit) randomly generated strings that ar unique to individual users that are concatenated with the hashes to protect against rainbow tables. The total sum of all characters also includes a hash algorithm identifier and cost factor as below:
$2a$10$N9qo8uLOickgx2ZMRZoMyeIjZAgcfl7p92ldGxad68LJZdL17lhWy
\__/\/ \____________________/\_____________________________/
Alg Cost Salt Hash

the hash algorithm identifer is a bcrypt hash in modular crypt format (I will include a link below for any interested in what that is) and the cost factor is an input to the bcrypt algorithm and represents 2 raised to that power.

Authentication in Sinatra is done by adding the bcrypt gem to your application. Once the gem is installed you can add a simple line to your ActiveRecord model, has_secure_password. The has_secure_password method is not actually a method rather it is a macro that allows your application to do several things related to password security including ensuring that a password is used.

Bcrypt does not store any passwords, it stores the salted-hash as a password_digest. It is critical in your ActiveRecord model to create a column with a string of password_digest and not password.
The macro also adds 5 methods to your application, among these methods is the “authenticate” method. The authenticate method can be used on a user as such, user.authenticate(params[:password]), if the password does not match the users password it returns false. If the password does match the users password authenticate returns the user.
Resources used and for further reading:

https://en.wikipedia.org/wiki/Bcrypt

https://emmanuelhayford.com/understanding-the-bcrypt-hashing-function-and-its-role-in-rails/

https://passlib.readthedocs.io/en/stable/modular_crypt_format.html


