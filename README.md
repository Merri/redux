# [Redux](http://rackt.github.io/redux)

Redux on ennustettava tilasäilö JavaScript-ohjelmille.  

Sen avulla voi kirjoittaa ohjelmia, jotka käyttäytyvät määrätysti, ovat suoritettavissa eri ympäristöissä (selaimessa, palvelimella ja natiiveina) sekä ovat helposti testattavissa. Näiden etujen lisäksi Redux tarjoaa hyvän käyttökokemuksen kehittäjälle, kuten [suorituksenaikaisen koodinmuokkauksen ja aikamatkaavan debuggerin](https://github.com/gaearon/redux-devtools).

Reduxia voi käyttää yhdessä [Reactin](https://facebook.github.io/react/) tai minkä tahansa muun näkymäkirjaston kanssa.  
Redux on pieni (2 kt) ja sillä ei ole riippuvuuksia.

[![build status](https://img.shields.io/travis/rackt/redux/master.svg?style=flat-square)](https://travis-ci.org/rackt/redux)
[![npm version](https://img.shields.io/npm/v/redux.svg?style=flat-square)](https://www.npmjs.com/package/redux)
[![npm downloads](https://img.shields.io/npm/dm/redux.svg?style=flat-square)](https://www.npmjs.com/package/redux)
[![redux channel on discord](https://img.shields.io/badge/discord-%23redux%20%40%20reactiflux-61dafb.svg?style=flat-square)](https://discord.gg/0ZcbPKXt5bZ6au5t)
[![#rackt on freenode](https://img.shields.io/badge/irc-%23rackt%20%40%20freenode-61DAFB.svg?style=flat-square)](https://webchat.freenode.net/)
[![Changelog #187](https://img.shields.io/badge/changelog-%23187-lightgrey.svg?style=flat-square)](https://changelog.com/187)

>**Uutta! Opi Reduxia suoraan luojaltaan:  
>[Alkuun Reduxin kanssa](https://egghead.io/series/getting-started-with-redux) (30 englanninkielistä ilmaista videota)**

### Arvostusta

>[“Pidän siitä mitä olet saanut aikaan Reduxilla”](https://twitter.com/jingc/status/616608251463909376)  
>Jing Chen, Fluxin luoja

>[“Pyysin mielipiteitä Reduxista Facebookin sisäisellä JS-kanavalla ja sitä kehuttiin laidasta laitaan. Todella hienoa työtä.”](https://twitter.com/fisherwebdev/status/616286955693682688)  
>Bill Fisher, Flux-dokumentaation vastuuhenkilö

>[“On hienoa kuinka kehität paremman Fluxin ilman Fluxin häivääkään.”](https://twitter.com/andrestaltz/status/616271392930201604)  
>André Staltz, Cyclen alkuunpanija

### Kehittäjän käyttökokemus

Kirjoitin Reduxin työstäessäni React Europen esitystä [Välitön uusiolataaminen ja aikamatkustus](https://www.youtube.com/watch?v=xsSnOQynTHs). Tavoitteeni oli luoda tilanhallintakirjasto minimalistisella API:lla ja silti ennustettavalla käyttäytymisellä, jotta olisi mahdollista kehittää lokitus, välitön uusiolatautuminen, aikamatkustus, universaalius, nauhoitus ja toisto ilman ylimääräistä lisätyötä kehittäjän toimesta.

### Vaikutteet

Redux jalostaa [Fluxin](http://facebook.github.io/flux/) ideoita, mutta välttää sen monimutkaisuutta ottaen inspiraatiota [Elmistä](https://github.com/evancz/elm-architecture-tutorial/).  
Reduxilla pääsee nopeasti alkuun riippumatta siitä onko käyttänyt kumpaakaan mainituista.

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

Oletuksena on että käytössä on [npm](https://www.npmjs.com/)-paketinhallinta yhdessä moduuliniputtajan kuten [Webpack](http://webpack.github.io) tai [Browserify](http://browserify.org/) kanssa. Ne syövät [CommonJS-moduuleja](http://webpack.github.io/docs/commonjs.html).

Mikäli et vielä käytä [npm:ää](https://www.npmjs.com/) tai modernia moduuliniputtajaa ja suosit mieluummin yhden tiedoston [UMD](https://github.com/umdjs/umd) buildia, joka tuo `Reduxin` saataville globaalina objektina, voit hakea valmiiksi luodun version [cdnjs](https://cdnjs.com/libraries/redux):stä. *Emme* suosittele tätä tapaa vakavaan käyttöön, sillä valtaosa Reduxia tukevista kirjastoista on ainoastaan saatavilla [npm](https://www.npmjs.com/):ssä.

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

// Päivityksiin voi liittyä manuaalisesti tai käyttäen näkymän sitojia.
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

### Opi Reduxia suoraan luojaltaan

[Alkuun Reduxin kanssa](https://egghead.io/series/getting-started-with-redux) on videokurssi, joka koostuu 30:sta Dan Abramovin, Reduxin luojan, juontamasta jaksosta. Se on luotu täydentämään dokumentaation perusteiden osioita tuoden kuitenkin lisää näkemystä asioihin kuten mutatoimattomuuteen, testaukseen, Reduxin suositeltuihin käytänöihin sekä Reduxin käyttöön Reactin kanssa. **Kurssi on ilmainen ja tulee aina olemaan.**

>[“Hieno kurssi egghead.io:ssa @dan_abramov'lta - pelkän käytön näyttämisen lisäksi hän kertoo miten ja miksi #redux luotiin!”](https://twitter.com/sandrinodm/status/670548531422326785)  
>Sandrino Di Mattia

>[“Kynnän läpi @dan_abramov'n 'Alkuun Reduxin kanssa' - on uskomatonta kuinka konseptit ovat yksinkertaisempia ymmärtää videolta katsellen.”](https://twitter.com/chrisdhanaraj/status/670328025553219584)  
>Chris Dhanaraj

>[“Tämä @dan_abramov'n videosarja Reduxista @eggheadio:ssa on loistava!”](https://twitter.com/eddiezane/status/670333133242408960)  
>Eddie Zaneski

>[“Tulin nimen hehkutuksen vuoksi. Pysyin kallion lujien perusteiden vuoksi. (Kiitos ja hyvää työtä @dan_abramov sekä @eggheadio!)”](https://twitter.com/danott/status/669909126554607617)  
>Dan

>[“Tämä videosarja Reduxista @dan_abramov'n toimesta räjäyttää tajuntaani toistuvasti - pittää refaktoroida rankasti”](https://twitter.com/gelatindesign/status/669658358643892224)  
>Laurence Roberts

Joten mitä vielä odotat?

#### [Katso 30 ilmaista videoita!](https://egghead.io/series/getting-started-with-redux) (englanniksi)

Jos pidit kurssista, niin harkitsethan Eggheadin tukemista [alkamalla tilaajaksi](https://egghead.io/pricing). Tilaajat saavat pääsyn lähdekoodiin jokaisessa videossani, sekä pääsyn lukuisiin muihin kehittyneisiin oppitunteihin muista aiheissa mukaanlukien JavaScript, React, Angular ja monta muuta. Monet [Eggheadin kouluttajista](https://egghead.io/instructors) ovat myös avoimen lähdekoodin kirjastojen tekijöitä, joten tilaus on hieno tapa kiittää heitä heidän tekemästään työstä.

### Dokumentaatio (linkit päivittämättä)

* [Esittely](http://rackt.github.io/redux/docs/introduction/index.html)
* [Perusteet](http://rackt.github.io/redux/docs/basics/index.html)
* [Kehittynyt käyttö](http://rackt.github.io/redux/docs/advanced/index.html)
* [Reseptit](http://rackt.github.io/redux/docs/recipes/index.html)
* [Ongelmienratkaisu](http://rackt.github.io/redux/docs/Troubleshooting.html)
* [Sanasto](http://rackt.github.io/redux/docs/Glossary.html)
* [API](http://rackt.github.io/redux/docs/api/index.html)

Halutessasi PDF-, ePub- tai MOBI-version offlinelukemista varten, sekä löytääksesi ohjeet kuinka luoda ne, katso see: [paulkogel/redux-offline-docs](https://github.com/paulkogel/redux-offline-docs) (englanniksi).

### Esimerkkejä (linkit päivittämättä)

* [Laskuri](http://rackt.github.io/redux/docs/introduction/Examples.html#counter) ([source](https://github.com/rackt/redux/tree/master/examples/counter))
* [TodoMVC](http://rackt.github.io/redux/docs/introduction/Examples.html#todomvc) ([source](https://github.com/rackt/redux/tree/master/examples/todomvc))
* [Todo ja peruminen](http://rackt.github.io/redux/docs/introduction/Examples.html#todos-with-undo) ([source](https://github.com/rackt/redux/tree/master/examples/todos-with-undo))
* [Asynkronisuus](http://rackt.github.io/redux/docs/introduction/Examples.html#async) ([source](https://github.com/rackt/redux/tree/master/examples/async))
* [Universaalius](http://rackt.github.io/redux/docs/introduction/Examples.html#universal) ([source](https://github.com/rackt/redux/tree/master/examples/universal))
* [Oikea maailma](http://rackt.github.io/redux/docs/introduction/Examples.html#real-world) ([source](https://github.com/rackt/redux/tree/master/examples/real-world))
* [Ostoskori](http://rackt.github.io/redux/docs/introduction/Examples.html#shopping-cart) ([source](https://github.com/rackt/redux/tree/master/examples/shopping-cart))

Jos NPM on uusi asia ja et pääse vauhtiin projektin kanssa, tai jos et ole varma minne pastettaa ylläolevia gistejä, niin kurkkaa [simplest-redux-example](https://github.com/jackielii/simplest-redux-example), joka käyttää Reduxia yhdessä Reactin ja Browserifyn kanssa.

### Keskustelu

Liity [#redux](https://discord.gg/0ZcbPKXt5bZ6au5t)-kanavalle [Reactiflux](http://www.reactiflux.com) Discord-yhteisössä.

### Kiitokset

* [Elm-arkkitehtuurille](https://github.com/evancz/elm-architecture-tutorial) upeasta johdatuksesta tilanmallintamiseen pelkistimiä käyttäen;
* [Tietokannan kääntö nurinkurin](http://www.confluent.io/blog/turning-the-database-inside-out-with-apache-samza/) räjäytti mieleni;
* [Kehittäminen ClojureScriptillä käyttäen Figwheeliä](https://www.youtube.com/watch?v=j-kj2qwJa_E) vakuutti minut siitä, että uudelleenevaluoinnin tulisi toimia suorilta;
* [Webpackille](https://github.com/webpack/docs/wiki/hot-module-replacement-with-webpack) välittömästä moduulin korvauksesta;
* [Flummox](https://github.com/acdlite/flummox) opetti minulle lähestymisen Fluxiin välttäen turhan boilerplaten ja singletoneihin tukeutumisen;
* [disto](https://github.com/threepointone/disto) toimi näytteenä säilöjen välittömästä uudelleenlatauksesta;
* [NuclearJS](https://github.com/optimizely/nuclear-js) osoitti sen, että tämänkaltainen arkkitehtuuri voi olla suorituskykyinen;
* [Om](https://github.com/omcljs/om) popularisoi idean yksittäisestä lähdetilasta;
* [Cycle](https://github.com/cyclejs/cycle-core) osoitti kuinka funktio on usein paras työkalu;
* [Reactille](https://github.com/facebook/react) käytännöllisestä innovoinnista.

Erityiskiitokset [Jamie Patonille](http://jdpaton.github.io) `redux`-pakettinimen luovuttamisesta NPM:ssä.

### Muutosloki

Tämä projekti noudattaa [semanttista versiointia](http://semver.org/).  
Jokainen julkaisu migraatio-ohjeineen on dokumentoitu Githubin [Releases](https://github.com/rackt/redux/releases)-sivulla.

### Tukijat (patrons)

Työ Reduxin eteen on [yhteisön rahoittama](https://www.patreon.com/reactdx).  
Tässä osa mahtavista yrityksistä, jotka tekivät työn mahdolliseksi:

* [Webflow](https://webflow.com/)
* [Chess iX](http://www.chess-ix.com/)

[Katso koko lista Reduxin tukijoista.](PATRONS.md)

### License

MIT
