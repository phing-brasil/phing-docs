---
layout: page
title: Possibilidades de estensão
originalLink: https://www.phing.info/docs/stable/hlhtml/index.html#d5e1687
---

There are three main areas where Phing can be extended: Tasks, Types, Mappers. The following sections discuss these options.

6.1.1 Tasks

Tasks are pieces of codes that perform an atomic action like installing a file. Therefore a special worker class hast to be created and stored in a specific location, that actually implements the job. The worker is just the interface to Phing that must fulfill some requirements discussed later in this chapter, however it can - but not necessarily must - use other classes, workers and libraries that aid performing the operations needed.

6.1.2 Types

Extending types is a rare need; nevertheless, you can do it. A possible type you might implement is urlset, for example.

You may end up needing a new type for a task you write; for example, if you were writing the XSLTTask you might discover that you needed a special type for XSLTParams (even though in that case you could probably use the generic name/value Parameter type). In cases where the type is really only for a single task, you may want to just define the type class in the same file as the Task class, rather than creating an official stand-alone Type.

6.1.3 Mappers

Creating new mappers is also a rare need, since most everything can be handled by the Appendix F, Core mappers. The Mapper framework does provide a simple way for defining your own mappers to use instead, however, and mappers implement a very simple interface.