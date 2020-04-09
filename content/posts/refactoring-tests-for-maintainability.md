---
title: Refactoring Tests For Maintainability
date: 2015-01-28T09:03:54-04:00
draft: false
---
I just began working at Intent Media  and I've been spending some time getting to know the code base. I spent the other day pairing with another guy on the team to add a new feature. We needed to change one of the data models by adding a new field. So we wrote some tests specifying how we thought the new data field should work. Next we got them passing. After that we ran all the unit tests. Oh man were we in trouble. We had twenty-five test failures. I was a little surprised since we had made a pretty small change in the production code.


Small changes in production code should have small effects in test code. So to my mind this meant that we had some refactoring to do. The object we were changing had an internal builder pattern which looks something like this.

```java
package org.charlieaustin.blog;

public class DataModel {

    private Object field;
    private Object field1;
    private Object field2;

    private DataModel() {}

    public DataModelBuilder dataModelBuilder() {
        return new DataModelBuilder();
    }

    protected static class DataModelBuilder {


        private final org.charlieaustin.blog.DataModel dataModel;
        private Object field;
        private Object field1;
        private Object field2;

        public void setField(Object field) {
            //do some transformations
            this.field = field;
        }

        public void setField1(Object field1) {
            // do some transformations
            this.field1 = field1;
        }

        public void setField2(Object field2) {
            // do some transformations
            this.field2 = field2;
        }

        public DataModel build() {
            validate();
            dataModel.field = field;
            dataModel.field1 = field1;
            dataModel.field2 = field2;
            return dataModel;
        }

        private void validate(){
            // validate that fields are correct
        }


        private DataModelBuilder(){
            this.dataModel = new DataModel();
        }

    }
} 
```
 


While looking at the tests we realized they were all failing for the same reason. The new field we had created wasn't initialized in any way for the old tests. We quickly realized we were constructing the object in too many places as part of test code set up. We could fix this without refactoring, but we would have to make the same change in 25 places. Also some times the data used to construct the data model was important to the test, while other times it wasn't. It was difficult to see the difference between the two cases.


The builder was doing a good job of controlling when the fields got accessed and ensuring that we only had consistent data models in the world, but we needed another abstraction to ease the construction for tests. We really wanted to minimize the number of places that we constructed the builder without losing the flexibility that it provided. We decided to pull the construction of the data model builder into a central place on the data model test class. That way we could create a data model builder with sensible defaults which the consuming class could override with import data before building. We also added a second method to construct a default data model. It looked something like this.


```java
public class DataModelBuilderTest extends TestCase {
    // there are some tests in here somewhere ...
    //  ...

    public static DataModel.DataModelBuilder getDefaultDataModelBuilder() {
        DataModel.DataModelBuilder dataModelBuilder = DataModel.dataModelBuilder();
        dataModelBuilder.setField(new Object()); //add default object
        dataModelBuilder.setField1(new Object()); //add default object
        dataModelBuilder.setField2(new Object()); //add default object
        return dataModelBuilder;
    }
    
    public static DataModel getDefaultDataModel () {
        return getDefaultDataModelBuilder().build();
    }
} 
```

We then went from failing test to failing test. First fixing the test and then constructing the default object where possible. If the data was important to the test we would create the builder and make it clear that this particular field was important to this particular test. Later during the story when we needed to add another field we ended up having to change the code in only three places. First where we actually added the field, second where the builder created the default test object, and finally where we needed to add tests for the new field. The resulting test code was also easier to read. We could more easily see when the data was important and we got rid of a lot of noise in the test code.