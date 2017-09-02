The RAML type system borrows from object oriented programming languages such as Java, as well as from XSD and JSON Schemas.
We view type definitions as set sets of constraints on data and attached meta information. You can compose these sets in more complex type definitions by inheriting from already defined types. Later we use term 'facets' to represent both constraints and meta data attached to type definitions.
RAML Data Types
The RAML type system has the following types.
o	Any
	Scalar
	Array
	Object
	Union
	External
	Scalar
	String
	Data
	File
	Date
	Boolean
	Number
	Integer
RAML Constraints
	Constraints are always inherited when composing a new type from one or more super types.
#%RAML 1.0
baseUri: https://mocksvc.mulesoft.com/mocks/64d3dcc7-5a70-43e5-834a-097343139d54 # 
title: Constraints example
types: 
  Employee:
    type: object
    properties: 
      FirstName: string
      LastName: string
      EmployeeID: integer
      DOB: date-only
      Email: string
      Phone: integer

RAML Metadata
RAML defines metadata as a tuple of key and value. The key of this tuple defines the semantic nature of the metadata instance attached to a type. It is only allowed to use keys which are already defined.
The key which you can use to describe metadata, also defines a set of valid values of a tuple. Some metadata is inherited when defining a new type through inheritance, some are not
#%RAML 1.0
title: Metadata example
types: 
  Email:
    type: string
    description: email id
    example: testmail@gmail.com
User defined facets
RAML provides a way to declare metadata which must or may be filled in sub types of the type. This metadata is called user defined facets. To define your own facet, you should at first define a facet declaration and then you may or must fill this facet in the sub types (depending on facet declaration optionality). User defined facets are inherited. one type cannot have two user defined facets with the same key but different values. Facet declarations are inherited
#%RAML 1.0
title: User defined metadata
types: 
  ValidDate:
    type: date-only
    facets:
      officehours?: time-only
      noholidays?: boolean
  BusineessHours:
    type: ValidDate
    officehours: 08:49:37
    noholidays: true

 

Array Types
#%RAML 1.0
title: Array example
types:
  Organization:
    type: Employee[]
  Employee:
    properties:
      Name: string
      address: string
       
      
 
Unions
	#%RAML 1.0
title: Union Example
types: 
  employeeorindiv:
    type: employee | individual
  employee:
    type: object
    description: employeedetails
    properties: 
      name: string
      address: string
  individual:
    description: individual details
    properties: 
      name: string
      address: string


Object
#%RAML 1.0
title: Object Example
types: 
  Employee:
    type: object
    properties: 
      Name: string
      address: string
      email: string

Closed Types:
	By default RAML types are open. This means that there is no constraints restricting unknown/unmatched properties of object types. There is a way to restrict it by adding closed : true restriction to the type. Closed types has a closed restriction which means that any property of the object should match to declared additional/pattern or normal property of the type. Closed types have a nicer mapping to XSD

#%RAML 1.0
title: closed types example
types: 
  Employee: 
    type: object
    closed: true
    properties: 
      name: string
      address: string
  

    
Validations
#%RAML 1.0
title: validations examples
types: 
  DOB:
    type: Greaterthancurrentdate
    maximum: 2017
  Greaterthancurrentdate:
    type: number
    maximum: 2017


Patterns is an exception
#%RAML 1.0
title: String Pattern example
types: 
  Employee:
    type: Pattern
  Pattern:
    type: object
    properties: 
      firstName: string
         pattern: ".+"

Pattern property restrictions
#%RAML 1.0
title: String Pattern example
types: 
  Employee:
    type: Pattern
  Pattern:
    type: object
    properties: 
      firstName: string
      [.*]: string

Instance Validation
#%RAML 1.0
baseUri: https://mocksvc.mulesoft.com/mocks/e81f2163-3eaa-41aa-8050-f02624566bab # 
title: Instance validation example
types: 
  User:
    type: object
    properties: 
      name: string
      address: string
      email: string
/users/id:
  get:
    responses: 
      200:
        body: 
          application/json:
            type: User


Include
#%RAML 1.0 DataType

type: object
displayName: Type1
properties:
  ids:
    type: array
    items: 
      type: !include Type2.raml

#%RAML 1.0 DataType

type: object
displayName: Type2
properties:
  id: integer

#%RAML 1.0
baseUri: https://mocksvc.mulesoft.com/mocks/b6e03eab-85ee-496c-afc2-2aafb76939ed # 
title: External API Example
version: v1
types: 
  type1: !include Type1.raml

mediaType: application/json
/objects:
  get:
    responses: 
      200:
        body: 
          type: type1
  
  


Libraries
#%RAML 1.0
title: sample libraries
mediaType: application/json

uses: 
  common: Type1.raml

/users:
  get:
    responses: 
      200:
        body: 
          type: array
  
    

