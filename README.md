Original comment,
https://github.com/kach/nearley/issues/358#issuecomment-714082295

1. `npm install`
2. `npm run compile` (It should work OK)
3. `npm run compile-large` (It should work OK, but it fails)
4. `npm run compile-large-2` (It should work OK, but it fails)
5. `npm run compile-large-3` (It should work OK, but it fails)

-----

Given enough repetitions of the following (or a large enough grammar file),

```

Identifier ->
    %Identifier {% function (_a) {
    var identifier = _a[0];
    return {
        start: identifier.start,
        end: identifier.end,
        syntaxKind: parser_node_1.SyntaxKind.Identifier,
        identifier: identifier.value,
        quoted: false,
    };
} %}
    
```

`nearleyc` will fail.

-----

If you remove one repetition of the above snippet from `large-grammar.ne`, and run `compile-large` again,
it will work okay.

If you remove one repetition of the above snippet from `large-grammar-2.ne`, and run `compile-large-2` again,
it will work okay.

This still happens with nearley 2.20.1.

Downgrading to 2.11.2 works.

-----

### `large-grammar-3.ne`

The file `large-grammar-3.ne` has an even simpler grammar,

```
Identifier ->
    %identifier
    
# this is a comment with 1,055 characters. this is a comment with 1,055 characters.this is a comment with 1,055 characters. this is a comment with 1,055 characters.this is a comment with 1,055 characters. this is a comment with 1,055 characters.this is a comment with 1,055 characters. this is a comment with 1,055 characters.this is a comment with 1,055 characters. this is a comment with 1,055 characters.this is a comment with 1,055 characters. this is a comment with 1,055 characters.this is a comment with 1,055 characters. this is a comment with 1,055 characters.this is a comment with 1,055 characters. this is a comment with 1,055 characters.this is a comment with 1,055 characters. this is a comment with 1,055 characters.this is a comment with 1,055 characters. this is a comment with 1,055 characters.this is a comment with 1,055 characters. this is a comment with 1,055 characters.this is a comment with 1,055 characters. this is a comment with 1,055 characters.this is a comment with 1,055 characters. this is a comment with 1,055 characters.
# this is a comment with 1,055 characters. this is a comment with 1,055 characters.this is a comment with 1,055 characters. this is a comment with 1,055 characters.this is a comment with 1,055 characters. this is a comment with 1,055 characters.this is a comment with 1,055 characters. this is a comment with 1,055 characters.this is a comment with 1,055 characters. this is a comment with 1,055 characters.this is a comment with 1,055 characters. this is a comment with 1,055 characters.this is a comment with 1,055 characters. this is a comment with 1,055 characters.this is a comment with 1,055 characters. this is a comment with 1,055 characters.this is a comment with 1,055 characters. this is a comment with 1,055 characters.this is a comment with 1,055 characters. this is a comment with 1,055 characters.this is a comment with 1,055 characters. this is a comment with 1,055 characters.this is a comment with 1,055 characters. this is a comment with 1,055 characters.this is a comment with 1,055 characters. this is a comment with 1,055 characters.
//snip repeated until 66k characters reached
```

If you remove one of the comments from `nearley-grammar-3.ne`, and run `npm run compile-large-3` again,
it will work okay.