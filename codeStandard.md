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

GOOD
if (application.ts2__Offers__r.size() > 0) {
    Boolean applicationHasManyOffers = true;
    candidate.offerDate = application.ts2__Offers__r.get(0).Offer_Date__c;
    candidate.stage = 'Offer';

    MyClass localClass = new MyClass();
    localClass.doSomething(); 
    return localClass.myField;
}

for (int i = 0 ; i < maxSize ; ++i) { ... }
```
## Comments
Comments should be only used in last solution. Use explicit methods instead. Avoid commented code. Version control tools or Eclipse local history are there if you need to get back your old commented code.
```java
BAD
// code before
//checks if Retainer-Type Job Order only has 1 active placement and loads job order fees
if(jobOrder.RecordType.DeveloperName == 'Retainer') isRetainer = true;
if(isRetainer) this.loadRetainerFees(jobOrder); 
// code after

GOOD
// code before
this.loadRetainerFeesIfNeeded(jobOrder);
// code after

private void loadRetainerFeesIfNeeded (ts2__Job__c jobOrder) {
    if (jobOrder.RecordType.DeveloperName == 'Retainer') {
        this.isRetainer = true;
        this.loadRetainerFees(jobOrder);
    } 
} 

BAD
$scope.openLink = function(link){
    $scope.selectedLinkId = link;				
    $scope.showForm=true;
    //document.getElementById('pplIframe').contentWindow.location.reload();
};
```
## Structure
Use a 4 spaces for each level. Open bracket on same line. Close bracket on the same level. Use only one return;
Example:
```java
public with sharing class Ats3PipelineBuilder {
    private Id jobOrderId;
    
    public void build(Id jobOrderId) {
        this.atsPipelineDto = new Ats3PipelineDto();
        if (jobOrder.ts2__Applications__r.size() == 0) {
            this.messages.add(new Ats3MessageDto('error', 'noApplication'));
        } else {
            this.jobOrderName = jobOrder.Name;
        }
    }
}

BAD
private List<ts2__Offer__c> myFunction(){
    if (this.Condition()) {
       return new List<ts2__Offer__c>();
    } else {
        return null;
    }
}

GOOD
private List<ts2__Offer__c> myFunction(){
    List<ts2__Offer__c> objectToReturn;
    if (this.Condition()) {
       objectToReturn = new List<ts2__Offer__c>();
    }
    return objectToReturn;
}
```
## Space
Put space between parenthesis and brackets, in for loops and in logical conditions.
Example:
```Java
BAD
private void processOffers(List<ts2__Offer__c> offers){
    for(ts2__Offer__c offer:offers){
        if(offer.ts2__Candidate__c==null){

GOOD
private void processOffers (List<ts2__Offer__c> offers) {
    for (ts2__Offer__c offer : offers) {
        if (offer.ts2__Candidate__c == null) {
```
## If conditions
If you have two or more logical conditions in an if, use an explicit private method. If your if condition returns a Boolean, inline it. Don’t chain many useless levels of if conditions. One space after if condition and before opening bracket.
Example:
```Java
BAD
if (offer.ts2__Candidate__c == null && this.number > 10 && (this.toto != null || !tata)) {
    // DO STUFF
}

GOOD
if (shouldDoStuff()) {
    // DO STUFF
}

private boolean shouldDoStuff () {
    return offer.ts2__Candidate__c == null 
           && this.number > 10 
           && (this.toto != null 
               || ! tata);
}        
```
```Java
BAD
if(pars.stages && pars.stages.length > 0 && pars.stages.indexOf(item.stage) != -1) { 
    return true;
}
return false;

GOOD
return (pars.stages && pars.stages.length > 0 && pars.stages.indexOf(item.stage) != -1);
```
```java
BAD
if(items[i].cddOwnerId == pars.selectedUserId){
    if(stageCheck(pars, items[i])) arrayToReturn.push(items[i]);
}

GOOD
if (items[i].cddOwnerId == pars.selectedUserId && stageCheck(pars, items[i])) {
    arrayToReturn.push(items[i]);
}

BETTER
if (this.shouldPushData(parser, items[i])) {
    arrayToReturn.push(items[i]);
}

this.shouldPushData = function(parser, item) {
    return item.cddOwnerId == parser.selectedUserId 
           && stageCheck(parser, item);
}
```
## Brackets
Use brackets every time, even for one line if or for loop. One line if or for loop are bugs magnets.
Example:
```java
DO NOT DO THIS! 
for(ts2__Application__c app : j.ts2__Applications__r)
    ids.add(app.ts2__Candidate_Contact__c);
for(ts2__Submittal__c sub : j.ts2__Presents__r)
    ids.add(sub.ts2__Candidate__c);
for(ts2__Interview__c ccm : j.ts2__Interviews__r)
    ids.add(ccm.ts2__Candidate__c);
for(ts2__Offer__c off : j.ts2__Offers__r)
    ids.add(off.ts2__Candidate__c);
for(ts2__Placement__c plc : j.ts2__Placements__r)
    ids.add(plc.ts2__Employee__c);

I WILL KILL YOUR DOG IF YOU DO THIS 
if(jobOrder.RecordType.DeveloperName == 'Retainer') isRetainer = true;
if(isRetainer) this.loadRetainerFees(jobOrder);

OR THIS
for(var i=0;i<items.length; i++)if(stageCheck(pars,items[i]))arrayToReturn.push(items[i]);
```
