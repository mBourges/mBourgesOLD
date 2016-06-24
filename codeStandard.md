# Introduction
The purpose of this document is to define some common rules in order to create a clean and easy to read project. Because the standards are agreed by everyone, they should be strictly respected to maintain a high level of code quality.

The most important principle to keep in mind is that **we are @author**. We create code. We are responsible and accountable for this code. We should be proud of this code.

Another important thing to remember is that **we are a team**. Your code is shared with everyone. Your code should be readable and understandable by everyone, even by the future you, trying to fix a bug 6 months later.

Finally, a last rule to follow is the **Boy Scout rule**: “Always leave the campground cleaner than you found it”. Every time you read some code, if any line seems to be unclear, abstract, dirty or anything else not respecting this document, it is also **your responsibility** to clean it and to improve this code (you are, of course, allowed to blame and slap the author). Afraid to break anything doing this? That’s why unit tests are made.

# Common rules to any language

## Naming
**A method says what it does and does what it says.** Use long name variables, no abbreviations, even for temp variables. The only one exception is for for variables, where i,j or k are allowed.
Use CamelCase, all classes starting with a upper case, methods and fields with lower case

```java
  BAD
  if(app.ts2__Offers__r.size() > 0){
    Boolean tmp = true;
    cdd.offDate = app.ts2__Offers__r[0].Offer_Date__c;
    cdd.stage = 'Offer';
    myClass Local_class = new myClass();
    Local_class.DoSomething(); 
    return Local_class.MyField;
  }
```
