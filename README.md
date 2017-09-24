# Jungle Documentation

welcome to the Documentation, Tutorials and Examples for the Jungle Framework for the Concepts, Philosophy and Vision of the project see the Jungle Way \#\#TODO 

### A framework for organic modular systems capable of: 

Composability - Components can made by composing other components

Dynamic Extension - Systems can grow and define new components over time

Adaptive Connectedness - The connections between components are declared as laws that will create links when the components appear and destroy them as they disappear. 

Recoverability - Entire and partial systems can be described as serial data and recovered elsewhere and when

General Purpose - Wrap any interface and mount to any platform, operate with push or pull interoperability and functional or object-oriented paradigms.

### Basic definition and recovery of a cell

```js
const Jungle = require('jungle')
const domain = Jungle.Core
const j = Jungle.j

// The domain represents the set of available patterns that can be built in a context

// within the domain we define orgainism based on cell
domain.define('organism', j('cell',{

    //head represents properties inate to this kind of object meta 
    head:{
        exposure:'public' //make properties accessible to the outer js context
    },
    
    //properties 
    speed:10
    size:10
}))

//instantiation is by recovery from domain
let org = domain.recover(j('organism'))

//org does not contain the properties
org.exposed.speed = 12

//serialize changes by describing in domain
let pattern = domain.describe(org)

//define a new kind from the other
domain.define('being', pattern)

//recovery with special properties
let org2 = domain.recover(j('copy',{
    size:15
})

//org2.exposed.size == 15, org2.exposed.speed == 12
```

### Definition of a context

We want to create an ecology of interacting organisms, one seemingly simple relationship that can be shared is that of eating another organism,  actually it is quite a complex process, as one of many kinds of organism, a predator must first come into contact with prey, which they do by hunting, waiting or chance, then they must overcome thier prey by speed, might and cunning and then they will consume their prey which bestows them with some resources and leaving a carcass of some kind, all while consuming energy and moving around over time through a complex landscape.

We will make a simplification that  whenever a preditor is hungry it will eat the thing it can get to first. To do this we create a race of prey\(the winner loses\). The prey will then be removed from the context.

```js
domain.define('ecology',j('cell', {
    // define a medium within the space that dictates what eats what
   eats:j('media:race',{
      law:'*:aspredator->*:asprey'
   })

})

domain.define('prey',j('organism',{
    asprey:j('op',{
         resolve_in(predator){
            //a promise to fulfill when caught
            let junc = new Junction()
            let [done, raise] = junc.hold()

            setInterval(()=>{
               done(this.size)
            }, 1000/this.speed) //how long it takes to catch the prey

            return junc
        }
    })

}))

domain.define('predator', j({
    hunger:j('op',{
       carry_in 
    }),
     aspredator:j('op',{

})

const ecology1 = domain.recover(j('ecology',{    
    lion:j('predator',{
        size:25,
    }),

    gazelle:j('prey',{
        size:24,
        speed:10,
    })
))
```

### 



