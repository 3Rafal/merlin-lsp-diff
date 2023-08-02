<!-- Reconstructed from src/frontend/query_protocol.ml -->
​
# Errors
​
- `Errors`: retrieves the errors (and warnings) of the buffer
  *Configuration options*:
  + one can ask to get only the lexing or parsing or typing errors; or any combination of the three
​
# Code completion/insertion
​
- `Complete_prefix`: takes a position, a string (possibly empty), and some extra options and return a list of values, constructors, types, etc.  
  *Configuration options*:
  + with or without docstrings: very noticeable impact on performances (as it implies loading cmt files, calling locate, etc)
  + with or without types: I don't think anyone uses this (default = with)
  + can be limited to only a subset of the environment (i.e. to only include types and values but nothing else, or whatever)
- `Expand_prefix`: used by vim and emacs as a fallback when `Complete_prefix` doesn't return anything: it does some fuzzy matching on the prefix
  *Configuration options*:
  + with or without types: I don't think anyone uses this (default = without)
  + can be limited to only a subset of the environment (i.e. to only include types and values but nothing else, or whatever)
- `Polarity_search`: search by type, not properly bound in any editor AFAICT
- `Construct`: if called on a `_`, tries to replace it with a skeleton matching the type expected at that position
  *Configuration options*:
  + local: TODO
- `Destruct`: used to replace an expr by a pattern matching on that expr, or to refine an existing pattern
​
# Codebase exploration
​
- `List mnodules`: list the compilation units in the given paths (used to jump quickly between comp units).
- `Locate`: jump to def/decl
  *Configuration options*:
  + ML or MLI: (poorly named) whether one wants the definition or the declaration
  + arbitrary string: if given, will locate that string instead of the word under the cursor (the typing environment used is still the one at the cursor)
- `Locate_type`: jumps to the definition of the type of the thing under the cursor
- `Document`: jumps to decl and retrieves the docstring associated to it if any
  *Configuration options*:
  + arbitrary string: if given, will locate that string instead of the word under the cursor (the typing environment used is still the one at the cursor)
- `Occurrences`: (BROKEN) list all the other occurrences in the current buffer of the ident under the cursor
- `Outline`: returns all the structure items of the buffer (though only their name and location) as a tree
   *Configuration options*:
   - can include the type as well as the name
- `Shape`: I can't remember who wanted this. I believe it is basically a tree of all the positions in the buffer?
​
# Retrieving type information
​
- `Type_expr`: accepts an arbitrary string, parses it as an expression and prints its type
- `Type_enclosing`: returns the type of the node under the cursor as well as the one of all its ancestors
  *Configuration options*:
  + verbosity (an int): the higher the number is, the more type aliases will be expanded, signatures unfolded, etc.
  + index (an int): by default the types of all the enclosing nodes is returned. If an index is given only one of the enclosing nodes gets a type and the others just indicate their index (so the type can be retrieved by a later query)
​
# Syntactic browsing
​
- `Enclosing`: returns all the nodes enclosing the cursor
- `Holes`: return the list of holes (`_` in expr position) in the buffer
- `Jump`: given a string (match, fun, let, ...) returns the position of the closest enclosing such item
- `Phrase`: ... I don't quite remember, I expect it returns the position of the next (or prev) structure/signature item?
​
# Misc
​
- `Refactor_open`: used to strip the prefix of paths when opening a module, or to reintroduce it if one wants to remove the open.
- `Dump`: can be used to explore merlin's vision of the buffer at various steps (before parsing, after parsing, after ppxes, after typing, after converting to its internal tree)
