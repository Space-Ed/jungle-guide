# Jungle Documentation

welcome to the Documentation, Tutorials and Examples for the Jungle Framework for the Concepts, Philosophy and Vision of the project see the Jungle Way \#\#TODO here it is treated

Jungle is intended to model highly distributed and interconnected m

### Basic definition and recovery of a cell

```js
const Jungle = require('jungle')
const domain = Jungle.Core
const j = Jungle.j

// The domain represents the set of available patterns that can be built in a context

// within the domain we define orgainism
domain.define('organism', j('cell',{
    //form represents default properties innate to this kind of object
    form:{
        exposure:'public' //make size accessible to the outer js context
    },

    speed:10
    size:10  
}))

let org = domain.recover(j('organism'))
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

### Purpose

The core goal of jungle is to enable creation of contexts of interaction independently from the entities that interact within it. The framework provides the neccessary abstractions to create these entities with a standard interface that allows them to appear in constructions of any kind.

The idea with this is to enable distributed systems capable of performing arbitrary functions within an environment.

The gold standard of a system of interacting agents is one where in a distributed manner, with a minimal set of builtin types all the agents are capable of achieving full consensus, not witholding the ability for any member to raise a concern,

