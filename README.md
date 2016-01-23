# [Redux](http://rackt.github.io/redux)

Redux on ennustettava tilasäilö JavaScript-ohjelmille.  

Voit tämän avulla kirjoittaa ohjelmia, jotka käyttäytyvät määrätysti, ovat suoritettavissa eri ympäristöissä (selaimessa, palvelimella ja natiiveina) sekä ovat helposti testattavissa. Näiden etujen lisäksi Redux tarjoaa hyvän käyttökokemuksen kehittäjälle, kuten [suorituksenaikaisen koodinmuokkauksen ja aikamatkaavan debuggerin](https://github.com/gaearon/redux-devtools).

Reduxia voi käyttää yhdessä [Reactin](https://facebook.github.io/react/) tai minkä tahansa muun UI-kirjaston kanssa.  
Redux on pieni (2 kt) ja sillä ei ole riippuvuuksia.

[![build status](https://img.shields.io/travis/rackt/redux/master.svg?style=flat-square)](https://travis-ci.org/rackt/redux)
[![npm version](https://img.shields.io/npm/v/redux.svg?style=flat-square)](https://www.npmjs.com/package/redux)
[![npm downloads](https://img.shields.io/npm/dm/redux.svg?style=flat-square)](https://www.npmjs.com/package/redux)
[![redux channel on discord](https://img.shields.io/badge/discord-%23redux%20%40%20reactiflux-61dafb.svg?style=flat-square)](https://discord.gg/0ZcbPKXt5bZ6au5t)
[![#rackt on freenode](https://img.shields.io/badge/irc-%23rackt%20%40%20freenode-61DAFB.svg?style=flat-square)](https://webchat.freenode.net/)
[![Changelog #187](https://img.shields.io/badge/changelog-%23187-lightgrey.svg?style=flat-square)](https://changelog.com/187)

>**Uutta! Opi Reduxia suoraan luojaltaan:  
>[Alkuun Reduxin kanssa (Getting Started with Redux)](https://egghead.io/series/getting-started-with-redux) (30 englanninkielistä ilmaista videota)**

### Arvostusta

>[“Pidän siitä mitä olet saanut aikaan Reduxilla”](https://twitter.com/jingc/status/616608251463909376)  
>Jing Chen, Fluxin luoja

>[“Pyysin mielipiteitä Reduxista Facebookin sisäisellä JS-kanavalla ja sitä kehuttiin laidasta laitaan. Todella hienoa työtä.”](https://twitter.com/fisherwebdev/status/616286955693682688)  
>Bill Fisher, Flux-dokumentaation vastuuhenkilö

>[“On hienoa kuinka kehität paremman Fluxin ilman Fluxin häivääkään.”](https://twitter.com/andrestaltz/status/616271392930201604)  
>André Staltz, Cyclen alkuunpanija

### Kehittäjän käyttökokemus

Kirjoitin Reduxin työstäessäni React Europen esitystä [Välitön uusiolataaminen ja aikamatkustus (“Hot Reloading with Time Travel”)](https://www.youtube.com/watch?v=xsSnOQynTHs). Tavoitteeni oli luoda tilanhallintakirjasto minimalistisella API:lla ja silti ennustettavalla käyttäytymisellä, jotta olisi mahdollista kehittää lokitus, välitön uusiolatautuminen, aikamatkustus, universaalius, nauhoitus ja toisto ilman ylimääräistä lisätyötä kehittäjän toimesta.

### Vaikutteet

Redux kehittää [Fluxin](http://facebook.github.io/flux/) ideoita, mutta välttää sen monimutkaisuutta ottaen inspiraatiota [Elmistä](https://github.com/evancz/elm-architecture-tutorial/).  
Reduxilla pääsee nopeasti alkuun riippumatta siitä oletko käyttänyt kumpaakaan mainituista.

### Asennus

Vakaan version asentamiseksi:

```
npm install --save redux
```

Todennäköisimmin tarvitset myös [React-työkalut](https://github.com/rackt/react-redux) ja [kehitystyökalut](https://github.com/gaearon/redux-devtools).

```
npm install --save react-redux
npm install --save-dev redux-devtools
```

Oletuksena on että käytät [npm](https://www.npmjs.com/)-paketinhallintaa yhdessä moduuliniputtajan kuten [Webpackin](http://webpack.github.io) tai [Browserifyn](http://browserify.org/) kanssa, jotka syövät [CommonJS-moduuleja](http://webpack.github.io/docs/commonjs.html).

Mikäli et vielä käytä [npm:ää](https://www.npmjs.com/) tai modernia moduuliniputtajaa ja suosit mieluummin yhden tiedoston [UMD](https://github.com/umdjs/umd) buildia, joka tuo `Reduxin` saataville globaalina objektina, voit hakea valmiiksi luodun version [cdnjs:stä](https://cdnjs.com/libraries/redux). *Emme* suosittele tätä tapaa vakavaan käyttöön, sillä valtaosa Reduxia tukevista kirjastoista ovat ainoastaan saatavilla [npm:ssä](https://www.npmjs.com/).

### Pähkinänkuoressa

Kaikki ohjelman tila (state) on säilöttynä objektipuuna yksittäisessä *säilössä* (store).  
Ainut tapa muuttaa tilapuuta on lähettää *toiminto* (action), joka on muutoksen kuvaava objekti.  
Toimintojen vaikutus tilapuuhun määritellään käyttäen puhtaita *pelkistimiä* (pure reducer).

Siinä kaikki!

```js
import { createStore } from 'redux'

/**
 * Tämä on pelkistin, joka on (tila, toiminto) => tila -mallia noudattava puhdas funktio.
 * Se kuvaa kuinka toiminto muuttaa tilan seuraavaksi tilaksi.
 *
 * Tilan rakenne on päätettävissäsi: se voi olla primitiivi, taulukko, objekti tai jopa
 * Immutable.js:n tietorakenne. Ainut tärkeä asia on, että tilaobjektia ei saa mutatoida
 * vaan lopputulemana tulee palauttaa uusi objekti silloin kun tila muuttuu.
 *
 * Tässä esimerkissä käytämme `switch`:iä ja merkkijonoja, mutta voit käyttää eri konventiota
 * noudattavaa helpperiä (kuten funktiokartat), mikäli se tekee tolkkua projektissasi.
 */
function counter(state = 0, action) {
  switch (action.type) {
  case 'INCREMENT':
    return state + 1
  case 'DECREMENT':
    return state - 1
  default:
    return state
  }
}

// Luo redux-säilön ohjelman tilaa varten.
// Sen API on { subscribe, dispatch, getState }.
let store = createStore(counter)

// Päivityksiin voi liittyä manuaalisesti tai käyttäen näkymän sidoksia.
store.subscribe(() =>
  console.log(store.getState())
)

// Ainut tapa aiheuttaa mutaatio sisäiseen tilaan on välittää (dispatch) toiminto.
// Toimintoja voi serialisoida, lokittaa tai säilöä, ja toistaa myöhemmin.
store.dispatch({ type: 'INCREMENT' })
// 1
store.dispatch({ type: 'INCREMENT' })
// 2
store.dispatch({ type: 'DECREMENT' })
// 1
```

Suoran tilan mutatoinnin sijaan toteutettavat mutaatiot kuvataan käyttäen normaaleja objekteja, joita kutsutaan *toiminnoiksi* (action). Tämän jälkeen toteutetaan *pelkistin* (reducer), joka päättää kuinka kukin toiminto muokkaa koko ohjelman tilaa.

Mikäli taustasi on Fluxissa, niin on yksi tärkeä asia joka tulee huomioida. Reduxilla ei ole Dispatcheria eikä se tue useampaa säilöä. Näiden sijaan on olemassa vain yksi säilö ja sillä vain yksi juuritason pelkistinfunktio. Ohjelman kasvaessa säilöjen lisäämisen sijasta tehdään vastuunjakoa toteuttamalla pienempiä pelkistimiä, jotka vastaavat omasta osastaan tilapuuta itsenäisesti. Tämä on juuri kuten Reactissa, jossa on vain yksi juurikomponentti, mutta jonka sisällä on monia pienempiä komponentteja.

Tämänkaltainen arkkitehtuuri voi vaikuttaa liioittelulta laskuriohjelmalle, mutta tämän toimintamallin etuna on se, kuinka hyvin se skaalautuu suuriin ja monimutkaisiin ohjelmistoihin. Lisäksi tämä toimintamalli mahdollistaa erittäin hyödylliset kehitystyökalut, sillä jokainen mutaatio on mahdollista jäljittää toimintoon, joka sen aiheutti. Voit esimerkiksi nauhoittaa käyttäjäsessioita ja toistaa ne ajamalla niiden toiminnot järjestyksessä.

### Learn Redux from Its Creator

[Getting Started with Redux](https://egghead.io/series/getting-started-with-redux) is a video course consisting of 30 videos narrated by Dan Abramov, author of Redux. It is designed to complement the “Basics” part of the docs while bringing additional insights about immutability, testing, Redux best practices, and using Redux with React. **This course is free and will always be.**

>[“Great course on egghead.io by @dan_abramov - instead of just showing you how to use #redux, it also shows how and why redux was built!”](https://twitter.com/sandrinodm/status/670548531422326785)  
>Sandrino Di Mattia

>[“Plowing through @dan_abramov 'Getting Started with Redux' - its amazing how much simpler concepts get with video.”](https://twitter.com/chrisdhanaraj/status/670328025553219584)  
>Chris Dhanaraj

>[“This video series on Redux by @dan_abramov on @eggheadio is spectacular!”](https://twitter.com/eddiezane/status/670333133242408960)  
>Eddie Zaneski

>[“Come for the name hype. Stay for the rock solid fundamentals. (Thanks, and great job @dan_abramov and @eggheadio!)”](https://twitter.com/danott/status/669909126554607617)  
>Dan

>[“This series of videos on Redux by @dan_abramov is repeatedly blowing my mind - gunna do some serious refactoring”](https://twitter.com/gelatindesign/status/669658358643892224)  
>Laurence Roberts

So, what are you waiting for?

#### [Watch the 30 Free Videos!](https://egghead.io/series/getting-started-with-redux) 

If you enjoyed my course, consider supporting Egghead by [buying a subscription](https://egghead.io/pricing). Subscribers have access to the source code for the example in every one of my videos, as well as to tons of advanced lessons on other topics, including JavaScript in depth, React, Angular, and more. Many [Egghead instructors](https://egghead.io/instructors) are also open source library authors, so buying a subscription is a nice way to thank them for the work that they’ve done.

### Documentation

* [Introduction](http://rackt.github.io/redux/docs/introduction/index.html)
* [Basics](http://rackt.github.io/redux/docs/basics/index.html)
* [Advanced](http://rackt.github.io/redux/docs/advanced/index.html)
* [Recipes](http://rackt.github.io/redux/docs/recipes/index.html)
* [Troubleshooting](http://rackt.github.io/redux/docs/Troubleshooting.html)
* [Glossary](http://rackt.github.io/redux/docs/Glossary.html)
* [API Reference](http://rackt.github.io/redux/docs/api/index.html)

For PDF, ePub, and MOBI exports for offline reading, and instructions on how to create them, please see: [paulkogel/redux-offline-docs](https://github.com/paulkogel/redux-offline-docs).

### Examples

* [Counter](http://rackt.github.io/redux/docs/introduction/Examples.html#counter) ([source](https://github.com/rackt/redux/tree/master/examples/counter))
* [TodoMVC](http://rackt.github.io/redux/docs/introduction/Examples.html#todomvc) ([source](https://github.com/rackt/redux/tree/master/examples/todomvc))
* [Todos with Undo](http://rackt.github.io/redux/docs/introduction/Examples.html#todos-with-undo) ([source](https://github.com/rackt/redux/tree/master/examples/todos-with-undo))
* [Async](http://rackt.github.io/redux/docs/introduction/Examples.html#async) ([source](https://github.com/rackt/redux/tree/master/examples/async))
* [Universal](http://rackt.github.io/redux/docs/introduction/Examples.html#universal) ([source](https://github.com/rackt/redux/tree/master/examples/universal))
* [Real World](http://rackt.github.io/redux/docs/introduction/Examples.html#real-world) ([source](https://github.com/rackt/redux/tree/master/examples/real-world))
* [Shopping Cart](http://rackt.github.io/redux/docs/introduction/Examples.html#shopping-cart) ([source](https://github.com/rackt/redux/tree/master/examples/shopping-cart))

If you’re new to the NPM ecosystem and have troubles getting a project up and running, or aren’t sure where to paste the gist above, check out [simplest-redux-example](https://github.com/jackielii/simplest-redux-example) that uses Redux together with React and Browserify.

### Discussion

Join the [#redux](https://discord.gg/0ZcbPKXt5bZ6au5t) channel of the [Reactiflux](http://www.reactiflux.com) Discord community.

### Thanks

* [The Elm Architecture](https://github.com/evancz/elm-architecture-tutorial) for a great intro to modeling state updates with reducers;
* [Turning the database inside-out](http://www.confluent.io/blog/turning-the-database-inside-out-with-apache-samza/) for blowing my mind;
* [Developing ClojureScript with Figwheel](https://www.youtube.com/watch?v=j-kj2qwJa_E) for convincing me that re-evaluation should “just work”;
* [Webpack](https://github.com/webpack/docs/wiki/hot-module-replacement-with-webpack) for Hot Module Replacement;
* [Flummox](https://github.com/acdlite/flummox) for teaching me to approach Flux without boilerplate or singletons;
* [disto](https://github.com/threepointone/disto) for a proof of concept of hot reloadable Stores;
* [NuclearJS](https://github.com/optimizely/nuclear-js) for proving this architecture can be performant;
* [Om](https://github.com/omcljs/om) for popularizing the idea of a single state atom;
* [Cycle](https://github.com/cyclejs/cycle-core) for showing how often a function is the best tool;
* [React](https://github.com/facebook/react) for the pragmatic innovation.

Special thanks to [Jamie Paton](http://jdpaton.github.io) for handing over the `redux` NPM package name.

### Change Log

This project adheres to [Semantic Versioning](http://semver.org/).  
Every release, along with the migration instructions, is documented on the Github [Releases](https://github.com/rackt/redux/releases) page.

### Patrons

The work on Redux was [funded by the community](https://www.patreon.com/reactdx).  
Meet some of the outstanding companies that made it possible:

* [Webflow](https://webflow.com/)
* [Chess iX](http://www.chess-ix.com/)

[See the full list of Redux patrons.](PATRONS.md)

### License

MIT
