class: center, middle

# Abstractions.

[live presentation](https://alonisser.github.io/abstractions-talk/) <br/>
[twitter](alonisser@twitter.com), [medium](https://medium.com/@alonisser/)

Works at [zencity.io](https://zencity.io/). We help cities leverage AI to understand their citizens needs.

#####Shameless promotion: you can also read my political blog: [דגל אדום](degeladom@wordpress.com)
---
# Castles of the mind

>*The programmer, like the poet, works only slightly removed from pure thought-stuff. He builds his castles in the air, from air, creating by exertion of the imagination.”* 
― Frederick P. Brooks Jr., The Mythical Man-Month: Essays on Software Engineering

---
# Abstractions

Software is about abstractions. It's the way we communicate. It's an important part of the way we reason about software.
We are building castles with abstractions.

--

And yes. It's abstractions all the way down

---

# The old tale about the DB connection

```python
db = MongoClient('mongodb://127.0.0.1:27017/mydb')
```

---

# The old tale about the DB connection

A little wrapping abstraction

```python
# Technical repo module
db = MongoClient('mongodb://127.0.0.1:27017/mydb') # <= a URI here

class DataStore(object):
    @classmethod
    get(query):
        return db.find(query)
        
# And in application module:

from datastores import DataStore
DataStore.get(somequery)         
```

---

# Intermezzo about URI

URI - Uniform Resource Identifier

```python

mongo_url="mongodb://127.0.0.1:27017/mydb"
         
```

> *Uniformity provides several benefits.  It allows different types
  of resource identifiers to be used in the same context, even when
  the mechanisms used to access those resources may differ.  It
  allows uniform semantic interpretation of common syntactic
  conventions across different types of resource identifiers.  It
  allows introduction of new types of resource identifiers without
  interfering with the way that existing identifiers are used.  It
  allows the identifiers to be reused in many different contexts,
  thus permitting new applications or protocols to leverage a pre-
  existing, large, and widely used set of resource identifiers.* From the URI current definition By the Internet Engineering task force (IETF) in rfc-3986

---

# Intermezzo about URI

URI - Uniform Resource Identifier

```python

mongo_url="mongodb://127.0.0.1:27017/mydb"
         
```

> *Uniformity provides several benefits.  It allows **different types**
  of resource identifiers to be used in the same context, even when
  **the mechanisms used to access those resources may differ**.  It
  allows uniform semantic interpretation of common syntactic
  conventions across **different types of resource** identifiers.  It
  allows introduction of new types of resource identifiers without
  interfering with the way that existing identifiers are used.  It
  **allows the identifiers to be reused in many different contexts**,
  thus permitting new applications or protocols to leverage a pre-
  existing, large, and widely used set of resource identifiers.* 
  From the URI current definition By the Internet Engineering task force (IETF) in rfc-3986

--

The URI is used to abstract differences between resources

It's a powerful tool: it's a concise abstraction which delivers lot's of data: a protocol - how do we communicate, identification, resource location and even configuration

---

# The old tale about the DB connection

We change from using concrete ip address to a localhost name.

```python

mongo_url="mongodb://localhost:27017/mydb"
# Technical code here .. 
# And in application module:

from datastores import DataStore
DataStore.get(somequery)  
         
```
--

A new abstraction: DNS. instead of providing hard coded addresses we provide a name

---

# The old tale about the DB connection
  
Names! Is there a more used (and litigated) software abstraction? 

 
---

# The old tale about the DB connection

As we are moving to deploy we opt to use a mongo as a service from a provider

```python

mongo_url="mongodb://myuser:mypass@ds666.mlabs.com:61055/mydb"

# Technical code here .. 
# And in application module:

from datastores import DataStore
DataStore.get(somequery)  

         
```
--

Note that from the application code, nothing changes. Our abstraction is fulfilling its destiny!

---
# The old tale about the DB connection

Finally we move to production, with a mongo cluster and a ssl connection


```python

mongo_url="mongodb://myuser:mypass@ds666-a0.mlabs.com,ds666-a1.mlabs.com:61055/mydb?replicaSet=ds666&ssl=true"

# Technical code here .. 
# And in application module:

from datastores import DataStore
DataStore.get(somequery)  

         
```
Note that from the application code, still nothing changes. 

---

# The old tale about the DB connection

Lets reflect at the way we went.

--

from the crude local mongo code in the application to an abstracted db api which actually uses a highly available cloud service.
This was done by small iterative changes, in good **refactoring** practice.
--

And from application code perspective nothing changes

--

OR DOES IT?

---

class: center, middle

# We are not in Kansas anymore
```python

mongo_url="mongodb://myuser:mypass@ds666-a0.mlabs.com,ds666-a1.mlabs.com:61055/mydb?replicaSet=ds666&ssl=true"
```
---

# Leaky abstractions

>*"All non-trivial abstractions, to some degree, are leaky"* Joel Spolsky from: [The Law of Leaky Abstractions
](https://www.joelonsoftware.com/2002/11/11/the-law-of-leaky-abstractions/)
---

# Leaky DB abstraction
So while from the application code perspective "nothing has changed" . LOTS HAS ACTUALLY CHANGED:

--

* local socket communication with local process has been replaced with over the unreliable internet network via tcp/ip 
* So we now need to reason about ssl, latency, consistency (It's a cluster!).
* Failure handling and connection pooling which are usually a small concern on a local install, are an order of magnitude more complex under those conditions
 
---

# Numbers

.img-container[![Numbers that every programmer should know](https://camo.githubusercontent.com/77f72259e1eb58596b564d1ad823af1853bc60a3/687474703a2f2f692e696d6775722e636f6d2f6b307431652e706e67)]

from https://gist.github.com/hellerbarde/2843375

---

# Open question

Is the fact that all this new complexity can be added while completely abstracted from our application code is good? 

---

# First Mystery: staging failure

One of the first things I was told

 
--
Yes I do know that **the only real staging environment is production**

---

# Read some more



---

class: center, middle

#Open source rocks!

---

class: center, middle

#Thanks for listening!

---
