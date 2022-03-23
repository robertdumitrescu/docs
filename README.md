# Docs - OBSOLETE - Moved to swp-scripts

## Contents

## Small observations

#### Why was null replaced with undefined in the models?
We have the case when we edit an object that had previously a value for a key and in order to update the key value with nothing we need to be able to pass null to "toEntityModel" method. 

Example: We have the content object, and the content object had the mainTermId property populated with number 84. We don't want an mainTerm anymore for that content. How should we specify that content doesn't need that association anymore? We can have all the properties as undefined and filter the undefined ones but keep the null ones.
