# Toiminnot (actions)

Aloitamme määrittelemällä jokusen toiminnon.

**Toiminnot** ovat tietopaketteja, jotka lähettävät dataa ohjelmastasi säilöön. Ne ovat *ainoa* tiedonlähde säilölle. Tieto lähetetään kutsumalla [`store.dispatch()`](../api/Store.md#dispatch).

Tässä on esimerkki toiminnosta, joka esittää uuden todorivin lisäämistä:

```js
const ADD_TODO = 'ADD_TODO'
```

```js
{
  type: ADD_TODO,
  text: 'Luo ensimmäinen Redux-ohjelmani'
}
```

Toiminnot ovat tavanomaisia JavaScript-objekteja. Toiminnoilla tulee olla `type`-arvo, joka määrittää suoritettavan toiminnon tyypin. Tyyppien tulisi olla määritettynä merkkijonovakioina. Ohjelmasi kasvettua riittävän suureksi saatat haluta siirtää ne omaan moduuliinsa.

```js
import { ADD_TODO, REMOVE_TODO } from '../actionTypes'
```

>##### Huomautus pohjustuskoodista

>Sinun ei ole pakko määrittää toimintotyyppivakioita erillisessä tiedossa tai määrittää niitä ollenkaan. Pienessä projektissa voi olla helpompaa käyttää suoria merkkijonoarvoja suoraan koodin seassa. Isommissa projekteissa yksiselitteisellä vakioiden määrittämisellä on joitakin hyötyjä. Lue [pohjustuskoodin vähentämisestä](../recipes/ReducingBoilerplate.md) tutustuaksesi käytännönläheisiin vinkkeihin siitä, kuinka pitää koodi siistinä.

Muutoin kuin `type`:n osalta voit määrittää toiminto-objektin kuten haluat. Voit tutustua [Fluxin perustoimintoihin](https://github.com/acdlite/flux-standard-action) (englanniksi) nähdäksesi suosituksia siitä, kuinka toimintoja voi rakentaa.

Lisäämme yhden toimintotyypin kuvastaaksemme sitä, kun käyttäjä merkitsee todorivin tehdyksi. Viittaamme todoon käyttäen `index`:iä, koska säilmme niitä taulukossa. Oikeassa ohjelmassa on fiksumpaa luoda uniikki ID jokaisella uudelle luodulle asialle.

```js
{
  type: COMPLETE_TODO,
  index: 5
}
```

On hyvä idea kuljetuttaa kussakin toiminnossa niin vähän tietoa kuin vain mahdollista. Esimerkiksi on parempi kuljettaa indeksiä kuin koko todo-objektia.

Lopuksi lisäämme vielä yhden toimintotyypin vaihtamaan näytettyjen todojen tilaa.

```js
{
  type: SET_VISIBILITY_FILTER,
  filter: SHOW_COMPLETED
}
```

## Action Creators

**Action creators** are exactly that—functions that create actions. It's easy to conflate the terms “action” and “action creator,” so do your best to use the proper term.

In [traditional Flux](http://facebook.github.io/flux) implementations, action creators often trigger a dispatch when invoked, like so:

```js
function addTodoWithDispatch(text) {
  const action = {
    type: ADD_TODO,
    text
  }
  dispatch(action)
}
```

By contrast, in Redux action creators simply return an action:

```js
function addTodo(text) {
  return {
    type: ADD_TODO,
    text
  }
}
```

This makes them more portable and easier to test. To actually initiate a dispatch, pass the result to the `dispatch()` function:

```js
dispatch(addTodo(text))
dispatch(completeTodo(index))
```

Alternatively, you can create a **bound action creator** that automatically dispatches:

```js
const boundAddTodo = (text) => dispatch(addTodo(text))
const boundCompleteTodo = (index) => dispatch(completeTodo(index))
```

Now you’ll be able to call them directly:

```
boundAddTodo(text)
boundCompleteTodo(index)
```

The `dispatch()` function can be accessed directly from the store as [`store.dispatch()`](../api/Store.md#dispatch), but more likely you'll access it using a helper like [react-redux](http://github.com/gaearon/react-redux)'s `connect()`. You can use [`bindActionCreators()`](../api/bindActionCreators.md) to automatically bind many action creators to a `dispatch()` function.

## Source Code

### `actions.js`

```js
/*
 * action types
 */

export const ADD_TODO = 'ADD_TODO'
export const COMPLETE_TODO = 'COMPLETE_TODO'
export const SET_VISIBILITY_FILTER = 'SET_VISIBILITY_FILTER'

/*
 * other constants
 */

export const VisibilityFilters = {
  SHOW_ALL: 'SHOW_ALL',
  SHOW_COMPLETED: 'SHOW_COMPLETED',
  SHOW_ACTIVE: 'SHOW_ACTIVE'
}

/*
 * action creators
 */

export function addTodo(text) {
  return { type: ADD_TODO, text }
}

export function completeTodo(index) {
  return { type: COMPLETE_TODO, index }
}

export function setVisibilityFilter(filter) {
  return { type: SET_VISIBILITY_FILTER, filter }
}
```

## Next Steps

Now let’s [define some reducers](Reducers.md) to specify how the state updates when you dispatch these actions!

>##### Note for Advanced Users
>If you’re already familiar with the basic concepts and have previously completed this tutorial, don’t forget to check out [async actions](../advanced/AsyncActions.md) in the [advanced tutorial](../advanced/README.md) to learn how to handle AJAX responses and compose action creators into async control flow.
