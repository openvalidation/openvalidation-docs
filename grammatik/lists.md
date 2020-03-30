# Lists



Both the validation of individual objects from lists and lists as a whole can be realized with the help of openVALIDATION. This section deals with the processing possibilities of lists and introduces the basic concepts and functions on lists.

### 

### Defining custom lists

There are two ways in which lists are available for validation in openVALIDATION. In the first case, the list is defined as input via the schema, such as:

```yaml
numbers: [1, 2, 4, 8, 16]
```

However, it is also possible to define lists manually, which either contain elements that dont change or ones that represent other variables or schema properties. The following schema is given as an example:

```yaml
degree: Bachelor
```

Assuming the property `degree` is to be one of the three values `None`, `Bachelor` and `Master`, a list variable `degrees` that contains these three values can be defined as

```yaml
None, Bachelor and Master as degrees
```

The characters `,` , `and` and `or` separate the individual elements of the list. You can then formulate a rule that checks whether the `degree` is included in `degrees`.

```yaml
None, Bachelor and Master as degrees

The degree must be one of degrees
```

More information about the `ONE_OF` function can be found further down in the corresponding section. Similarly, lists can be defined outside of variables, directly in rules, for example:

```yaml
The degree must be one of None, Bachelor or Master
```

While the list may contain only words or only numbers, for example, its contents can be selected freely. This means that both properties from the schema and other variables can be defined as part of the list. The following list definition demonstrates this as an example using the schema

```yaml
degree: Kein
master_degree: Master
```

```yaml
Bachelor als bachelor degree

None, bachelor degree and master degree as degrees
```



### First and last element of a list \(FIRST and LAST function\)

As the names of the `FIRST` and `LAST` functions suggest, they are used to access the first and last elements of a list. For example, they can be used to define the following rule on the schema below, which fails each time the first element of the list is `1`:

```yaml
favourite_numbers: [1, 2, 4, 8, 16]
```

```yaml
The FIRST number FROM my favourite numbers AS first favourite number

My first favourite number MUST NOT be 1
```

It is also possible to get several elements of a list at once by additionally handing the function a number. For example, the following variable contains the list of the first three elements of the list:

```yaml
The FIRST 3 numbers from my favourite numbers AS Top 3
```

{% hint style="info" %}
If the number of desired elements exceeds the length of the list, the entire list is returned.
{% endhint %}

#### 

### Filtering of lists with conditions \(WITH function\)

When validating data, it is often necessary to consider only those elements of a list that meet a given condition. The function `WITH` exists for exactly that purpose. It allows you to filter lists according to any criteria, both on lists with simple contents such as numbers and on lists with complex contents such as persons, which in turn can have their own properties such as `name` and `age`. For example, to filter a simple list of numbers, you simply specify the list to be filtered by the keyword `FROM`, followed by the keyword `WITH` and the respective condition. The following scenario shows an example of such a filtering on the `numbers` list:

```yaml
numbers: [1,2,3,4,5,6,7,8,9,10]
```

The following expression is used to filter the numbers from 6 to 10 from the list and save them into the `my list` variable:

```yaml
All items FROM numbers WITH a value GREATER THAN 5 AS my list
```

The filtering of lists with complex contents is considered a frequent use case in the context of validation. Since this form of filtering differs from the previous, simpler variant, the following schema is given for orientation:

```yaml
applicants: [
    {name: "Peter", age: 17},
    {name: "Klaus", age: 19},
    {name: "Frieda", age: 38}]
```

The schema has a list called `applicants`. Each element \(one each in lines 2, 3 and 4\) within it has a uniform structure and has its own `name` and `age`. Thus for example, each element could be interpreted as a person. 

Suppose you want to filter the list of applicants according to their `age` so that only applicants younger than 20 are included. Since it is clear that each element has an `age` characteristic, it can be used in the filter condition. This example shows an expression that carries out such a filtering:

```yaml
All persons FROM the applicants WITH an age LESS THAN 20 AS juniors
```

The variable `juniors` now contains the two elements named Peter and Klaus, since their respective ages are less than 20. If the resulting list were to be written down as a schema, it would look like this:

```yaml
juniors: [
    {name: "Peter", age: 17},
    {name: "Klaus", age: 19}]
```

{% hint style="info" %}
The order of the values remains unchanged.
{% endhint %}

The list can then be used for further validation.



### FIRST and LAST in combination with conditions

Another part of the functionality of `FIRST` and `LAST` is the possibility of extension by a condition. The condition refers to the elements of the list and then determines the first or last element that fulfills the given condition. Since the two functions do not differ in their structure, the following examples are based on the `FIRST` function. The following schema is given with a list of numbers:

```yaml
favourite_numbers: [1,2,4,8,16,32]
```

The first element could be filtered from the list with this expression:

```yaml
The FIRST number FROM my favourite numbers AS favourite
```

In this case, the variable `favourite` would contain the number `1`. If you now want to extract the number `4` from the list instead, you can extend the above function with a `WITH` function. The resulting expression looks like this:

```yaml
The FIRST number FROM my favourite numbers WITH a value GREATER THAN 2 AS favourite
```

Lists with complex contents can also be processed in this way. The following schema shows, for example, the list of `applicants`, where each applicant has an `age` and a `name`:

```yaml
applicants: [
    {name: "Peter", age: 17},
    {name: "Klaus", age: 19},
    {name: "Frieda", age: 38}]
```

In order to extract the person with the name "Klaus" from the list, one could use the `age` of the applicants as a guide. Since Klaus is the first person in the list who is older than 18, he could be extracted using the following expression:

```yaml
The FIRST person FROM the applicants WITH an age GREATER THAN 18 AS Peter
```

### 

### Filtering of lists with complex contents by property

Another form of filtering lists with complex content is filtering by properties. In concrete terms, this means that a list of properties is generated from a list of complex elements that each have said property. The following schema is given for illustration:

```yaml
applicants: [
    {name: "Peter", age: 17},
    {name: "Klaus", age: 19},
    {name: "Frieda", age: 38}]
```

The list of `applicants` is a list of three persons \(lines 2-4\), each person having the properties `name` and `age`. The following expression is used, for example, to filter by `age`:

```yaml
applicants.age AS age list
```

As presented in the example, the filtering is done by specifying the list name and the property connected by a `.` . The order of the values is retained. The `age list` variable therefore contains the list of the ages of the persons in the applicant list and has the following contents:

```yaml
age_list: [17,19,38]
```



### Quantitative validation of lists

When validating lists, it is often necessary to be able to make quantitative statements about their content. For this purpose openVALIDATION provides the functions `ONE_OF` and `NONE_OF`, which can be used to check whether an element is contained in the list. As an example, the schema is first defined.

```yaml
magic_numbers: [65,43,21]
age: 33
```

For example, the following rule can be used to check whether the `age` matches an item in the `magic_numbers` list:

```coffeescript
The age must be one of the magic numbers
```

This rule fails precisely when the age is not `65`, `43` or `21`. It should be noted that there is a way to imply `ONE_OF` when using array that are defined inside a rule. For example

```coffeescript
The age must be 65, 43 or 21 
```



### Reference

openVALIDATION provides several functions for processing lists. They can be used to extract specific elements and filter lists, which can then be used for further validation. The following table gives an overview of all available functions:

| Function | Description | Aliases |
| :--- | :--- | :--- |
| FIRST | Takes the first _n_ elements of the list. The number _n_ is optional and is interpreted as 1 if no specification is made. | - |
| LAST | Takes the last _n_ elements of the list. The number _n_ is optional and is interpreted as 1 if no specification is made. | - |
| WITH | Gives the list of all elements of a list that fulfill the condition. | - |

{% hint style="warning" %}
Functions must always be defined within a variable.
{% endhint %}

