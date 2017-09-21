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
domain.define('organism')

    //crown represents defaults for the public, mutable and deep children
    .crown({
        size:4
    })

    //form represents default properties innate to this kind of object
    .form({
        exposure:'public' //make size accessible to the outer js context
    })
    
    //the base is what we DEEPLY extend from
    .basis('cell')

        
let org = domain.recover(j('organism')) 

```

### Definition of a context

We want to create an ecology of interacting organisms, one seemingly simple relationship that can be shared is that of eating another organism,  actually it is quite a complex process, of a set of various kinds of organism, a predator must first come into contact with prey, which they do by hunting, waiting or chance, then they must overcome thier prey by speed, might and cunning and then they will consume their prey which bestows them with some resources and leaving a carcass of some kind, all while consuming energy and moving around over time through a complex landscape. Of course we will make a simplification that is big things eat smaller things and that whenever the lion is hungry it will eat the thing it can get to first. To do this we create a race of prey. 

```js
domain.define('ecology')
    .crown({
        
    ))
    .form({
        
    })      
        

domain.recover(j('ecology',
    {    
        form:{
        },
        
        lion:j('organism',{
            size:25,
            predator:(j('op',{
                
            })
        },
        gazelle:j('organism',{
            size:24,
            speed:10,
            prey:j('op',{
                resolve_in(predator){
                    let junc = new Junction()
                    let [done, raise] = junc.hold()
                    setInterval(()=>{
                        done()
                    }, 1000/this.speed) //how long it takes to catch the prey
                }
            })
        }
    }))

    
```

### Purpose

The core goal of jungle is to enable creation of contexts of interaction independently from the entities that interact within it. The framework provides the neccessary abstractions to create these entities with a standard interface that allows them to appear in constructions of any kind. 

The idea with this is to enable distributed systems capable of performing arbitrary functions within an environment. 



The gold standard of a system of interacting agents is one where in a distributed manner, with a minimal set of builtin types all the agents are capable of achieving full consensus, not witholding the ability for any member to raise a concern, 

 

