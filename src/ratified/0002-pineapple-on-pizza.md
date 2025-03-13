* [ROS-ORCA-0040](https://github.com/osrf/orca/pull/132)
* Title: Pineapple Belongs on Pizza
* Status: Ratified
* Created: 2025-03-13
* Last updated: 2025-03-13

# Abstract

The `pizza_msgs/PizzaToppings` definition should include a `pineapple` field. This field should be a `uint32` which indicates the number of pineapple slices on the pizza.

# Introduction

Pizza is a popular food item with a wide user base. Pizza also comes in many variations whose characteristics are largely determined by the mixture of optional ingredients on the pizza. There are widely varying opinions on what combinations of ingredients for a pizza are appropriate. Systems that transmit information about pizza need to accommodate the breadth of these opinions or else there is a risk of fragmentation in the community.

The initial `pizza_msgs/PizzaToppings` definition did not include `pineapple` as an ingredient because it was not well established to be a viable pizza ingredient at the time the definition was made. In recent years pineapple has gained traction in global discourse as an acceptable pizza topping. While still being a polarizing pizza ingredient, it does have enough of a user base to be accounted for in a standardized pizza message.

# Specification

A field of `uint32 pineapples` should be added to `pizza_msgs/PizzaToppings` in an alphabetical position relative to the other ingredient fields:

```
uint32 anchovy
uint32 bell_pepper
uint32 mushroom
uint32 olive
uint32 onion
uint32 pepperoni
uint32 pineapple
uint32 sausage
uint32 spinach
uint32 tomato
```

The integer value of the `pineapple` field represents how many slices of pineapple are present on the pizza that is being described.

# Rationale

The growing popularity of pineapple on pizza has created pressure for pizza-based systems to include pineapple as a topping option to meet customer demands. Since the original `pizza_msgs/PizzaToppings` definition did not include a `pineapple` field, we have witnessed fragmentation in the pizza provider community. Several major providers have forked `pizza_msgs` to add a `pineapple` field[^awesome_pizza_msgs] [^better_pizza_msgs]. In some cases these forks have been renamed and released as new diverging message packages. In other cases the forks are kept internal, and the providers need to manually patch their forks with new developments in `pizza_msgs`.

[^awesome_pizza_msgs]: https://github.com/pizzaz-org/awesome_pizza_msgs
[^better_pizza_msgs]: https://github.com/dough-re-me/better_pizza_msgs

This fragmentation means less reusability of packages that depend on one of these competing pizza message packages. This increases the overall maintenance burden of our open source community, and interferes with efforts to collaborate in the pizza domain.

# Drawbacks

This does not resolve the more general problem of new ingredients needing to be introduced in the future.

# Alternatives Considered

### Mass instead of count

An alternative to `uint32 pineapple` could be `float pineapple` where the value represents the mass of the pineapple present on the pizza in terms of kilograms. This was not chosen because a count of pieces is more consistent with the representation of other ingredients, and it is considered more feasible to count the pieces of pineapple than to detect its weight within the pizza.

### Named Pizza Ingredients

Instead of adding a new statically defined field to `pizza_msgs/PizzaIngredients` we considered adding a new message type `pizza_msgs/NamedIngredient` with the form

```
string name
uint32 count
```

then replace `pizza_msgs/PizzaIngredients ingredients` in the `pizza_msgs/Pizza` definition with `pizza_msgs/NamedIngredient[] ingredients`. This would allow anyone to introduce any new ingredients in the future without needing to diverge the message definition.

This option was not chosen because interoperability would then require an additional specification the lays out all recognized string values for the ingredient names, and community members would have to carefully abide by that specification or else their systems could break in subtle ways. For example, should the string value be `"pineapple"` or `"pineapples"`? Community members who choose differently would accidentally diverge from each other.

Furthermore representing keys as unchecked strings carries a risk of introducing typos.

Finally this change would cause an enormous disruption for the existing community of users.

# Unresolved Concerns

* Some PMC members and community members do not believe that pineapple belongs on pizza under any circumstances.
