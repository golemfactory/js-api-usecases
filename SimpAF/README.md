# Simple Arithmetic Framework
The Simple Arithmetic Framework (or SimpAF or SAF) is a super-validated business-friendly development framework focused on defining procedural steps for repetitive calculations.

There are previous partial implementations of the concept in the insurance field, and they are working as expected for customers of the software company I work for.

# Summary/Abstract
The purpose of the application will be to define groups of `Products` and `Objects` that contain the calculation of intermediary and final `Results` of `Formulas` in incremental `Stages`, by use of auxiliary elements: `Parameters`, `Coefficients`, `Operators` and `Helpers`. Every `Result`, `Parameter` or `Coefficient` that is defined will have a `Type` that defines the set of acceptable values and one or more optional `Segmentations`, each of which completely split that set into a discrete number of intervals for the purpose of administration.

The configuration should support **versions** that are valid for discrete time periods and are defined by Expert users (for example product designers or application administrators). Only complete and valid configurations for specific formulas should be published (usable).  

The project would involve the development of two applications:
* Configuration module - this would accommodate the declaration of formulas and their sub-elements by the Expert user
* Service module - this would be a plug-in that can be used by applications to submit requests and receive responses for defined and published formulas

##### Products

The `Product` element defines a context for the application that contains all the logic that is necessary for a specific domain. Considering for example the insurance applicability, this would generally be an Insurance Product or a Cover (depending on the complexity and the user’s choice of logic separation).

##### Objects
The `Object` element allows for separation of logic within a `Product` that makes the set of sub-elements more manageable. Distinct values that are calculated (for example **sum insured**, **premium**, **account value**, **claim amount**) are all good examples for this element, but also different sub-categories of these in case of complex _Products_ (for example **main cover premium**, **clause premium**, **individual sum insured**, **company sum insured**, **motorcycles premium**, etc.)

##### Results
A `Result` is an output parameter of a `Formula`. All the `Results` of a `Formula` are determined in the final `Stage` of the `Formula`. The `Result` has a `Type` as defined during the definition of the final `Stage`.

##### Formulas
`Formulas` are one of the main logic implementations in the system. A `Formula` consists of a series of `Stages` that transform the `Parameters` and the `Coefficients` that are received/calculated in the initial `Stage` into the `Results` that are calculated in the final `Stage`. The main prerequisite for a `Formula` definition is for it to be able to be defined procedurally. Complex iterations and conditional logic should be implemented outside the `Framework` and, if needed, multiple `Formulas` should be defined to calculate the same `Result` in different ways if it’s required.

##### Stages
`Stages` are elements that represent the calculation steps involved in the `Formula`. The initial `Stages` are always the declaration/interpretation of the `Parameters` needed by the other `Stages`, together with the preparation (calculation) of all the `Coefficients` that are needed by the other `Stages`. The final `Stages` are always the determination of the `Results` (the output parameters) of the `Formula`. Between these `Stages` it should be possible to define an unlimited number of intermediary `Stages` that create and/or set the value of intermediary variables needed by future `Stages`, based on the available `Parameters` and `Coefficients` of the `Formula` or the variables set in previous `Stages`.

As a side note, multiple variables could be set in the same `Stage`. The only reasons to create additional `Stages` should be when the new `Stage` requires a variable to be set by calculation in a previous `Stage`, when conditional or cycling clauses are needed, and when the user wants to better control the versioning (for instance creating two `Stages` that are not overlapping in order to separate variables that are changed often from the ones that are less likely to be adjusted).

An option should be available to group `Stages` in `Phases` in more complex scenarios in order to contain a conditional or cyclic logic that involves multiple `Stages`.

##### Parameters
The `Parameters` elements represent the set of values that the `Formula` requires in order to work correctly. Each `Parameter` will have a `Type` that defines the format and set of values that the `Formula` expects. It is expected that the calling system will provide the `Parameters`, but the `Framework` should include a functionality of generating a form that allows users to enter them in order to perform tests (and even to save sets of `Parameters` and `Results` in order to easily check the correct behavior of the `Formula` in the future).

##### Coefficients
`Coefficients` are the other main logic implementation in the system. This element is based on constants that are defined in the system (usually in database tables) and can be interpreted based on available `Parameters` placed in a defined `Segmentation`. During the initial `Stage` of the `Formula`, all the coefficients that are required in further `Stages` should be determined.

Optionally `Coefficients` will need to be configurable (changed by a dedicated user). In this case, the framework should be able to generate forms representing the cartesian product of the `Segmentations` of `Parameters` that determine the `Coefficient`, based on a predefined configuration for the visual representation (rows/columns `Segmentations`).

##### Types
`Types` are definitions of the data type and possible values that specific `Parameters`, `Coefficients` and `Results` can take. This is useful for performing the necessary conversions and validations that are required in the calculation processes.

##### Segmentations
`Segmentations` are defined named criteria that split the full set of values a variable of a `Type` can take for a specific context related to `Coefficients` calculation or configuration in discrete intervals that should produce the same results. Each interval is called a `Segment`.

##### Operators
`Operators` are a set of symbols and keywords that are considered valid to be used within a `Stage` of a formula and help with the calculation (for example `+`, `-`, `*`, `/`, `POW`, `SQRT`, etc.) This list can be extended based on the final users’ needs, but it involves additional development regarding the mapping of the new `Operator` to the functions available in the underlying programming language.

##### Helpers
`Helpers` are elements that represent custom functions, implemented by developers at the final users’ request, in order to help contain the logic of a `Stage` for a `Formula` in the expected procedural format. Examples of Helpers would be functions like `getVehicleAgeInMonths(registration_date)` or `getVeryComplicatedResultThatDoesNotNeedVersioning(list_of_parameters[])`.

# Use case

User types involved in the use case:
* _Expert user_ - a product designer or application administrator with deep knowledge of the configured product/object and understanding of the framework
* _Developer_ - an IT specialist that is capable of integrating the configured formulas in the base application and (optionally) to defint additional `Operators` or `Helpers` as needed by the Expert user

1. _Expert user_ analyzes the `Formula` he needs to implement in order to achieve the `Result`
* the values that appear in the `Formula` need to be split in `Formulas` (with lesser scope), `Parameters` and `Coefficients` 
* the `Operators` and `Helpers` that are needed in order to perform the calculations must be identified or the _Developer_ should be instructed to implement new ones as needed
* the Stages and Phases of the calculation must be identified and enumerated
2. _Developer_ is informed about the expected `Parameters` and their `Types`, in order to prepare the call to the service
3. _Developer_ implements additional `Operators` and `Helpers` that are needed in order to have a correct `Formula`
4. _Expert user_ defines the needed `Products`, `Objects`, `Types`, `Segmentations` and `Coefficients` using the corresponding configuration modules
5. _Expert user_ defines the `Stages` of the `Formula` based on previously defined/configured elements
6. _Expert user_ defines test cases that need to be passed in order for the `Result` to be considered correct
7. _Expert user_ publishes the current version of the `Formula` (after each element is clearly specified and defined and all validations and tests are passed)
8. _Developer_ implements the integration between the core application and the published `Formula` 

While the above process would work in a stand-alone environment and various development technologies, the aim would be of course to have the published `Formulas` using the Golem network/processing power. 

I do not feel I have enough knowledge currently of the Golem/Yagna components hierarchy to suggest a split of functionality for the enumerated functionalities, however logically it seems like an appropriate model (in particular for calling and executing published `Formulas` in the Golem network).  

# Sample scenario

<blockquote>
We need a Formula that offers discounts to products that cost more than 100 EUR to customers based on their age:
* customers under 30 should get no discount
* customers between 30 and 50 should get 10% discount
* customers over 50 should get 20% discount

Individual product prices can be in a different currency than EUR.

The data that is available in the calling application includes the product price in a specific currency and the customer's date of birth.
</blockquote>

1. _Expert user_ will analyze the formula and identify the elements and their type. There are many ways to describe the requirement above, let's go for this type of pseudocode:

`final_product_price` = ROUND(`product_price` * (100-`discount`) / 100, 2) 
`final_product_currency` = `product_currency`

Obviously in our situation `final_product_price` (**money**) and `final_product_currency` (**currency**) are the `Result`. 
The user identifies that `product_price` (**money**) and `product_currency` (**currency**) are `Parameters`.
User also identifies that `discount` (**discount**) can be defined as a `Coefficient` based on the following lookup values:
* `reference_date` (**date**) - this should be a default parameter for any `Coefficient` in order to establish the version of the lookup table that needs to be used. In our situation it's also useful for calculating the other two lookup values in a predictable manner
* `product_price_eur` (**money**) - this should be the product_price converted to the EUR currency in order to check if it's greater or less than _X_
* `customer_age` (**age**) - this should represent the age the customer has reached

The lookup values are not straightforward - the `product_price` is received in a (possibly) different currency, and also for this example's sake let's assume the `customer_age` is also not received as such, instead we receive a `date_of_birth` (**date**) parameter. This means we'll need to calculate the needed lookup values:
`product_price_eur` = CONVERT_CURRENCY(`product_price`, `product_currency`, 'EUR', `reference_date`)

`customer_age` = CALCULATE_AGE(`date_of_birth`, `reference_date`)

Next step is for the user to identify the `Operators` and `Helpers` that appear in the formulas and instruct the Developer what they need to provide.

Most likely the basic math operators are not a problem in any language. So we are left with the helpers CONVERT_CURRENCY and CALCULATE_AGE that would involve support from developers in order for them to be available in the `Formula` configuration module.

----

Considering the analysis above, we can come up with a proposed approach for the `Stages` of calculating the formula.

As mentioned above, the first stages are dedicated to establishing the values for the parameters and the coefficients involved in the formula.
* Parameters `reference_date`, `product_price`, `product_currency`, `date_of_birth` are received through the request and validated against their types
* Variables `product_price_eur` and `customer_age` are calculated using the helpers mentioned above and validated against their types
* Value of coefficient `discount` for our scenario is identified based on `reference_date`, `product_price_eur` and `customer_age`
* Results are calculated using the `Formula` mentioned above and sent as a response

This concludes the analysis stage for the scenario.

2. _Developer_ is informed about the `Parameters` and their `Types`.

Considering our analysis, we identified the following types (we'll use an SQL notation for simplicity):
* **money** would be a float value with two decimals: DECIMAL(18,2)
* **currency** would be a set of values containing all allowed currencies: ENUM('EUR', 'USD', 'CHF', 'GBP')
* **discount** would be a float value that is <=100: DECIMAL(12,6) (doesn't make sense to have more than 100% discount, but it does make sense to have -5000% discount that would be a loading)
* **date** is a normal calendar date: DATE
* **age** is a natural number that is <=200: UNSIGNED SMALLINT

Considering this, the _Developer_ would receive the list of parameters that are expected by the service: `reference_date` (**date**), `product_price` (**money**), `product_currency` (**currency**), `date_of_birth` (**date**)

3. _Developer_ is informed about the additional `Operators` and `Helpers` that are needed. 

In our situation, the CONVERT_CURRENCY() and CALCULATE_AGE() helpers would be required and need to be implemented. As a fun note, these functions could also be implemented as `Coefficients`, but it's probably optimal to have them defined at lower level (for example the currency converter may require a webservice).

After the implementation, the `Helpers` would become available in the application configuration screens.

4. Our _Expert user_ would start using the application configuration screens here to define:

`Products` - In our situation we would have an "Elderly benefits" product

`Objects` - Let's assume our first object would be "Elderly discount"

`Types` - The application would help the user define the types as mentioned above (the list at 2.)

`Segmentations` - This step would involve fully splitting the valid values for each type in discrete named segments. This is a mandatory step for any type that is used for `Coefficients` look-up values (except the mandatory `reference_date` parameter that is used to establish the version of the lookup table that is used). 

In our situation the `Segmentations` would probably look like this:
* **money** would have a `Segmentation` named _product_price_eur_ with two `Segments`: "Under 100" with the condition "{value}<=100"; "Over 100" with the condition "{value}>100"
* **age** would have a `Segmentation` named _customer_age_ with three `Segments`: "Under 30" with the condition "{value}<30"; "30 to 50" with the condition "{value}>=30 AND {value}<50"; "Over 50" with the condition "{value}>=50"

`Coefficients` - This step involves: 
* defining the `Coefficients` (in our case associate the `discount` coefficient with the appropriate `Segmentations` - money.product_price_eur and age.customer_age), 
* configuring the presentation of the `Coefficient` definition screen (which `Segmentations` are presented as rows and which ones are presented as columns); define rules for blacklisted combinations (sometimes there are invalid combinations of the segments)
* defining values for the coefficient for each set of coordinates resulting from the combination of `segments` in the used `Segmentations`. 
 
In our situation this last step would mean accessing a configuration table generated by the application in which the user defines the coefficient values:
* "Under 100" + "Under 30" => 0% discount
* "Under 100" + "30 to 50" => 0% discount
* "Under 100" + "Over 50" => 0% discount
* "Over 100" + "Under 30" => 0% discount
* "Over 100" + "30 to 50" => 10% discount
* "Over 100" + "Over 50" => 20% discount

The application would automatically validate that all possible combinations of values resulting from the declared `Types` are either defined or blacklisted.

The power of the system is that based on these configurations (`Segmentations` + `Coefficients`) it is easy to implement massive changes to the way the coefficient values are determined in a new version, without impacting the steps that are just using the calculated coefficients usually in the same way.

5. The _Expert user_ would use the application interface to configure the needed stages of calculation as mentioned in the analysis:

* Parameters `reference_date`, `product_price`, `product_currency`, `date_of_birth` are received through the request
* Variables `product_price_eur` and `customer_age` are calculated using the helpers mentioned above
* Value of coefficient `discount` for our scenario is identified based on `reference_date`, `product_price_eur` and `customer_age`
* Results are calculated using the `Formula` mentioned above and sent as a response

Validations would be performed automatically based on the declared `Types` for each `Parameter`. The application would also need to force the user correct syntax errors or other logical issues detected in the `Stages` definition, or alternatively offer a visual process that helps the user avoid such mistakes.

6. The _Expert user_ would define test cases for the formula in order to help the application automatically check for the predictability of the result both before publishing the solution and for future changes.

In our example a test scenario would look like: `reference_date` = '2022-06-06'; `product_price` = 500; `product_currency` = 'EUR'; `date_of_birth` = '1980-01-01' => `final_product_prince` = 450; `final_product_currency` = 'EUR'

7. The _Expert user_ would use a "Validate" action that checks that all tests pass, all bases are covered and no elements appearing in the `Formula` stages are undefined or incompletely defined. If validation is successful the user would Publish the `Formula`, making it available for use and read-only. If corrections are needed the user would have to create a new version based on the initial one and Publish it in a similar fashion.

8. The _Developer_ implements the request/response and described in the base application and tests the integration.

# Conclusion

I hope the concept makes sense, I do realize it sounds a bit like reinventing simple development concepts. I think the appeal of the system is the target audience (business users that feel it's a long bath from business decisions to application changes), and the power of the system is in the flexibility of the Coefficients/Segmentation approach that allows for a very intuitive way of controlling the outcome of calculation processes.

I also feel there's a lot of room for growth, starting with the "super-validation" that would really sell the concept (in my opinion), and continuing with things like Excel import/export of Coefficients (with our life insurance clients we have tables with tens of thousands of coefficient values based on a lot of information about the customers, and these tables are reviewed annually). 