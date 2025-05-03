Maybe re-architecture for e2e type information.

Or fully rewrite the compiler

```
Source

Syntax Parser

Concrete Syntax Tree
 |               |

Mappings      Abstract Syntax Tree


              Typed Syntax Tree

 |
 |            WASM plugins

                                                           Doc IR
 |
 |            Typed Lambda IR

 |
              Tsx IR


Sourcemaps    JavaScript / TypeScript / d.ts Outputs       Docgen Program
```
