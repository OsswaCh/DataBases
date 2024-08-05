when you normalize a db table, you structure st it can't express redundant information.

example: in a date of birth table, one person can't have two dates of birth 

they are: 
* easier to understand 
* easier to enhance and extend 
* protected from 
	* insertion anomalies
	* update anomalies 
	* deletion anomalies

## How to know if a table is normalized or not? : Normal forms

5 levels of safety assessments : normal forms of DB theory 

### First Normal Form 
- same elements with different order are equivalent
#### violations
- using row order to convey information violates 1NF (entering data that needs to be in a ceratin order)
- mixing datatypes within a column
- designing a table without a primary key ![[Pasted image 20240805094839.png]] 
- repeating groups: putting multiple items in the same row  ![[Pasted image 20240805095052.png]]
=> better design is to do the following ![[Pasted image 20240805095214.png]] 
### Second Normal Form
it is about how a table non key columns relate to a key => each non-key attribute must depend on the entire primary key 
![[Pasted image 20240805095920.png]]
in this example the key is a combination of palyer_ID and item_type 
below we can see the function dependency each value on the left is associated with one element on the left  
however, player rating depends on the player only: not on the entire primary key ==> not in second normal formal 
how to fix? make a special table for the table for the player

### Third Normal Form
Aims to prevent the dependecy of a non key attribute on a non key attribute  ![[Pasted image 20240805100608.png]] 
 how to fix it? make a new table with the player rating
=> every non-key attribute in a table should depend on the key, the whole key and nothing but the key 
#### Boyce-Codd Normal form 
every attribute in a table should depend on the key, the whole key and nothing but the key

>usually these three levels allow us to fully normalize a table, however in some instances that might not be enough 

### Fourth Normal Form 
it addresses multivalue dependency 
{model} ->> {color} (each model has a **set** of available colors)

=> Multivalued dependencies in a table must be multivalued dependencies on the  key 

### Fifth Normal Form
we just ask the question if one table can be the result of joining multiple other tables together 

### example of a query  ![[Pasted image 20240805101753.png]] 