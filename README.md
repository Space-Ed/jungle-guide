# Jungle User Guide

welcome to the Documentation, Tutorials and Examples for the Jungle Framework for the Concepts, Philosophy and Vision of the project see the Jungle Way \#\#TODO

### A framework for organic modular systems capable of:

Composability - Components can made by composing other components

Dynamic Extension - Systems can grow and define new components over time

Adaptive Connectedness - The connections between components are declared as laws that will create links when the components appear and destroy them as they disappear.

Recoverability - Entire and partial systems can be described as serial data and recovered elsewhere and when

General Purpose - Wrap any interface and mount to any platform, operate with push or pull interoperability and functional or object-oriented paradigms.

### Basic definition and recovery of a cell

```js
import {j, J} from 'jungle-core'

// J is the domain that represents the set of foundational patterns that can be used
// j is a syntax sugar function. 
//   the first argument is the 'base' and second the 'description'
//   it gives us a standard jungle serial formatted object

// within the domain we define 'orgainism' based on cell, that is the foundational Object wrapper
J.define('organism', j('cell',{

    //head represents configuration of the top level object
    head:{
        exposure:'public' //make properties accessible to the outer js context
    },

    //properties 
    speed:10
    size:10
}))

//instantiation is by recovery from domain
let org = J.recover(j('organism'))

//this is the async safe way to 'pull' values from a system
org.extract({speed:12, size:null}).then(speed=>{
    //
})

//org does not contain the properties
org.exposed.speed = 12

//managed teardown 
org.dispose()

//serialize changes by describing in domain
let pattern = J.describe(org)

//define a new kind from the other
J.define('being', pattern)

//recovery with special properties
let org2 = J.recover(j('being',{
    size:15
})

//org2.exposed.size == 15, org2.exposed.speed == 12
```

### Definition of a context

Say we want to simulate an ecology of interacting organisms. This will require the creation of a context that models the relationships between organisms. One relationship that can be modelled is that of eating another organism that one might describe as follows.

as one of many kinds of organism, a predator must first come into contact with prey, which they do by hunting, waiting or chance, then they must overcome thier prey by speed, might and cunning and then they will consume their prey which bestows them with some resources and leaving a carcass of some kind, all while consuming energy and moving around over time through a complex landscape.

We will make a simplification that  whenever a preditor is hungry it will eat the thing it can get to first. To do this we create a race of prey\(the winner loses\). The prey will then be removed from the context. For now time is controlled from outside

```js
J.define('ecology',j('cell', {

    // define a medium within the space that dictates what eats what
   eats:j('media:race',{
      law:'*:predation->*:preyed' //many to many
   }),

   //distribute time to all members
   time:j('media:broadcast',{
       law:'tick->*:tick' //one to many
   }),

   // this is an op contact they appear on the membranes 'shell' and 'lining' 
   // in this case it is a tunnel from outside to in
   tick:j('op',{
       carry_in:true
   }

})

J.define('prey',j('organism',{
    preyed:j('op',{
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

J.define('predator', j({

    //ticks come in 
    behaviour:j('media:direct',{
        law:'tick->hunger->predation'
    }),

    tick:j('op',{
       carry_in:true
    }),

    hunger:j('op',{
        reflex_out(){
            return this.speed
        }
    },

    predation:j('rop',{
        minor_op(inp, carry){
            let predationRace = carry(inp)

            predationRace.then(food=>{
                this.eat(food)
            ))

        },

        minor_arg1:'carry'
    },

    stomach:0,

    //methods defined as normal
    eat(food){
        this.stomach += food
    }

})

const savanah = J.recover(j('ecology',{    
    lion:j('predator',{
        size:25,
    }),

    gazelle:j('prey',{
        size:24,
        speed:10,
    })
))

//trigger a time increment
savanah.put('tick')

//the result of this is that the lion eats the gazelle and attains
```

### A Changing Environment

an ecology is a constantly changing and evolving context, with new creatures being born all the time and the rules of the landscape changing. The same could be said of the  technological landscape we operate in today. Everything in the space will adapt to the addition and removal of new creatures automatically creating the relationships intrinsic to the context, even new forms and laws of connection can come and go.

```js
//apply a deep patch to the structure
savanah.patch({simba:j('predator')})
```

It is possible to reify into the domain as we proceed. This enables the domain to be used as a version history storing mechanism

In the following example we create a simulator outer layer that

```js
let simulator = domain.recover(j('cell', {

    //the heart manages exposure and behaviour of self modification
    heart:{ 
        exposed:true,    
    },

    simulation:j('ecology', {
        tick:j('    
    })

    run({nprey, npred, nframes}){
        for (let i = 0; i<nprey; i++){
            this.heart.patch({simulation:[j('prey')]});
        }

        for (let i = 0; i<nprey; i++){
            this.heart.patch({simulation:[j('prey')]});
        }

        for (let i = 0; i<nframes; i++){
            //need to define run as a spring
            this.simulation.tick()
        }

        this.save()
    }

    save(){
        this.heart.extract({simulation:null})).then((sim)=>{
            //patching with array is add all
            this.heart.patch({archive:[sim]})            
        }

    },

    archive:j([])
})

//pull out the anonymous values from the archive
simulator.extract({archive:[]}).then(archive=>{
    //save archive to Database/fs or recover in a viewing context.
}
```

### Designation & Matching

As we have seen with laws defined above they have the format of

"designator expression -&gt; designator expression"

### Asynchronicity

Obviously the real world is not always ready and external systems must be waited on to provide expected results, and quite often these results are failures. Jungle uses a special form of promise called a junction to manage all its asynchronicity. Junctions themselves have a modular style allowing for several different strategies for merging promised values.

The construction and destruction of organisms can be asyncronous, when components require time to construct they will return a junction from a prime method that fulfills when the component is ready to operate. That should be considered by any mount that uses components that are wrapping not to fire any events or require any response until the system is completely constructed.

### Anonymity

Often compex data is not named object structure, it is instead ordered or unordered collections of things, this requires the ability to have basic support for Arrays and Sets using Anonymous IDs

### Subdomains

Every Composite in jungle operates within a domain that defines the possible

It must be possible for different contexts to have different sets of building blocks to work with, for example an audio context involves routing audio between nodes that would be available in that context and not elsewhere.

To achieve this it is possible to create subdomains that are either isolated or inheriting which allow the redefinition of the existing constructs and creation of new constructs within a space that

### Integration

Jungle aims to make it simple to create wrappers allowing integration with systems and enabling them to benefit from the built in  features of jungle.

### Health

### Multiplexing

### Confluence

### Agents

### Extending the Foundation

### 



