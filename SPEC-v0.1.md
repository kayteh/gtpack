# GTPack

This is how we make oatmeal cry.

## Abstract

This project is intended to solve a few problems in the same method (albeit non-extensible) as Webpack does. The primary goals are as follows,

- Ease of use and thorough documentation (especially for newbies to GTA:N/GT-MP client code)

- Provide a CommonJS (and possibly ES2016) module system
  + This shim must work exactly to CommonJS spec. Katalina's require package does not follow it to spec, only partially.

- To not ever touch the API in a meaningful way. It must remain intact, with bugs.
  + Exception 1: if certain events stop working, but are shimmable to make work, e.g. onResourceStart
    - onResourceStart is very special case, we may need to shim it solely to facilitate GT-UMD
  + Exception 2: local resource getters/setters will need to be recreated
  + Anything shimmed **must** still be accessible via `\_API` global.
  + Buggy, but fixable API systems should be done in other libraries.

- Transparent fallbacks for when GTPack isn't being used
  + See AMD/CommonJS/Browser shims (UMD) from the before-times.
  + This system will be called **GT-UMD**
  
- Facilitate a dependency management system, if one ever exists (which would complement GTPack.)

## Implementation

### Scaffolding Files - Loaders

This system will need two important files, let's call them the **loaders**. 

- One is a server-side file for fetching and resolving files.

- One is a client-side file for eval() and shimming the module system in.

#### Server-side Pseudocode

```
wait for event/channel `GTPACK/fetch`
  
  - resolve module path
     - look for parts in path
     - if reached last part, this is TARGET
     - else error NotFound
     
  - if TARGET is a file
     - 
