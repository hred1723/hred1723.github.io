---
layout: post
title:  "What is HTTPS and should I use it?"
date:   2021-11-16 09:56:16 -0700
categories: internet standards
---
What is HTTPS, how does it differ from HTTP and should we make use of it?

HTTPS is the sort of thing you might set up once and then no longer thing
about. Many programmers don't ever have to set it up themselves&mdash;you might
be coding front-end web applications most of the time and leave it to DevOps or
someone else to set up web server things...

However, we see HTTPS all over and now Google Chrome and other browsers are
pushing to encourage HTTPS by default. So, I think it is soemthing that may be
worth understanding.

## What is HTTPS?

**HTTP** is the **protocol** by which lots of web traffic happens. In
non-techno-babble terms, it is like the conventions used for sending 
data through websites. When you submit a form (for example, to create
a login for a website), HTTP is used to send data you enter to somebody else's
computer (server). When you visit a web page, your browser `GET`s data from a
server somewhere.

**HTTPS** is supposed to be a more "**secure**" version of HTTP. What does this
really mean?

HTTPS uses an **encryption protocol** to keep the data you are sending private
to you (client) and the server you're communicating with. HTTPS does the same
stuff as HTTP but it adds encryption via **assymetric public key
infrastructure**. If some cyber pervert is *sniffing* for data over a wireless
network, they will not be able to read your data if you encrypt it. Rather than
seeing stuff like,

```json
{
	username: "hred1723",
	password: "password123"
}

```

...they'd see character soup which is created via an encryption algorithm.

Here's a little Ruby script using some basic encryption. I referred to the
[official
documentation](https://ruby-doc.org/stdlib-2.4.0/libdoc/openssl/rdoc/OpenSSL/Cipher.html)
to write this:

```ruby
require 'openssl'
require 'digest/sha1'

data = "super secret message"
puts "Here is the unencrypted data: ", data, "\n"
puts "Now, let's encrypt it."

# Create a cipher for encrypting
cipher = OpenSSL::Cipher::AES.new(128, :CBC)
cipher.encrypt
key = cipher.random_key
iv = cipher.random_iv

encrypted = cipher.update(data) + cipher.final
puts "Here is the encrypted data: ", encrypted, "\n"
puts "You'll need the key and iv to decrypt it."

# Create a decipher for decrypting
decipher = OpenSSL::Cipher::AES.new(128, :CBC)
decipher.decrypt
decipher.key = key # Same key as above
decipher.iv = iv   # Same iv as above

# Confirm deciphered data is the same as our original data
plain = decipher.update(encrypted) + decipher.final
if data == plain
  puts "Data is same as decrypted data"
end
```

If you use HTTPS then a similar mechanism to that shown in the script above is
used to ensure only encrypted data is sent and received. HTTPS gives the
appropriate keys to you and the server of the website you're visiting so that 
you can encrypt/decrypt as appropriate.

## So what's the catch?

Just having more secure data sounds like a good thing, no? 

Unlike the example I gave in the ruby script here, using HTTPS requires
bringing a *third party* into the picture. Whereas using encryption
technologies through something like SSH to get into a Unix server somewhere may
just involve you and the server you're contacting communicating, the way HTTPS
works is through [SSL
certificates](https://www.cloudflare.com/learning/ssl/what-is-an-ssl-certificate/).
Certificates must be issued by some *authority* You have to interface with the
big boys in practice to get one of these:

> Technically, anyone can create their own SSL certificate by generating a
> public-private key pairing and including all the information mentioned above.
> Such certificates are called self-signed certificates because the digital
> signature used, instead of being from a CA, would be the website's own
> private key.

> But with self-signed certificates, there's no outside authority to verify
> that the origin server is who it claims to be. Browsers don't consider
> self-signed certificates trustworthy and may still mark sites with one as
> "not secure," despite the https:// URL. They may also terminate the
> connection altogether, blocking the website from loading.

Just as you practically have to through some service to register a domain name,
you have to go through some service to get an SSL certificate.

Now, in terms of costs (as in money), this isn't an issue. But if you wanted to
be a sneaky 1337 h4x0r anonymous hero or something, the way HTTPs is used in
practice doesn't serve you.

What HTTPS *can* do, however, is create a secure way for normal people
(including paying customers) to securely use your websites even using
McDonalds' wifi.

## Should I use HTTPS?

For interfacing with the mainstream establishment (including accepting payments
via credit cards and so on), then using HTTPS is probably a good idea. Not only
are very popular browsers like Google chrome starting to flag sites that
*aren't* using HTTPS, many big web services are working to make it easier to
set up HTTPS.

If you prefer to chat on IRC, use the gopher protocol and edit documents in
emacs and are positioned financially to sustain these habits, you probably
don't need to use HTTP. But then again, if this is you then you probably
wouldn't want the likes of Google to tell you what software to use anyways.

## Learn more

- [What is HTTP?](https://www.cloudflare.com/learning/ssl/what-is-https/) (cloudflare)
- [Simple Encryption in Ruby without external gems](https://stackoverflow.com/questions/4128939/simple-encryption-in-ruby-without-external-gems) (Stack overflow)
