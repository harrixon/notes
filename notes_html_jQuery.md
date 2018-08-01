# Common DOM jQuery Method

## Insert Inside the elements selected([Reference](https://api.jquery.com/category/manipulation/dom-insertion-inside/)):

- `.append()`: To the end of the selected elements
- `.prepend()`: To the beginning of the selected elements
- `.appendTo()`: Inverse of append, this method is called on the element to be appended
- `.prependTo()`: Inverse of prepend, this method is called on the element to be prepended

## Insert Outside the elements selected([Reference](https://api.jquery.com/category/manipulation/dom-insertion-outside/)):

- `.after()`: insert the element after the selected elements
- `.before()`: insert the element before the selected elements
- `.insertAfter()`: Inverse of after, this method is called on the element to be inserted.
- `.insertBefore()`: Inverse of before, this method is called on the element to be inserted.

## Deleting content

- `.remove()`: Remove the selected elements from the DOM
- `.empty()`: Remove all the child elements of the selected elements.

## Changing txt

- `.html()`: Retrieve the text value from the selected tag.
- `.html(htmlString)`: Set the text value as the new text value of the selected text.

## Changing Attribute

#### `val`specifially selects the value of element therefore it is common to use `val` for inputs. 

#### `attr` allows you to manipulate the attributes values of the element. Before change

- `.attr(nameOfAttr)`: Get the value of the specific attr from the selected element.
- `.attr(nameOfAttr,newValue)` : Set the new value of the specific attr of the element to the new value.
- `.val()`: Get the value of the attribute `value` from the selected element.Commonly used in input
- `.val(newValue)`: Set the value of the attribute `value` to the new Value.

## Changing CSS

- `.css(nameOfProperty)`: Get the value of that property.
- `.css(nameOfProperty,newValue)`: Set the value of that property to a new Value.
- `.addClass(nameOfClass)`: Add a new class to the selected element
- `.removeClass(nameOfClass)`: Remove the class with that name from the selected element. No error even if that class does not exist.





# Q

given an array of hmtl obj eg <td> , need to use for...of... loop to access e' in the middle ? rather than using `[1][2]`??

A: use .each() to loop , use :nthchild to select in selector