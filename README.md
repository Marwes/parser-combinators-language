# parser-combinators-language
This a crate providing an easy way of constructing parsers which can easily parse various programming languages. It has much of the same API as [Text.Parsec.Token](hackage.haskell.org/package/parsec-3.1.9/docs/Text-Parsec-Token.html) but are otherwise a bit different to fit in to the ownership model of rust. The crate is an extension of the [parser-combinators](https://github.com/Marwes/parser-combinators) crate.

## Example
```rust
extern crate parser_combinators;
extern crate parser_combinators_language;
use parser_combinators::*;
use parser_combinators_language::*;
fn main() {
    let env = LanguageEnv::new(LanguageDef {
        ident: Identifier {
            start: letter(),
            rest: alpha_num(),
            reserved: ["if", "then", "else", "let", "in", "type"].iter().map(|x| (*x).into()).collect()
        },
        op: Identifier {
            start: satisfy(|c| "+-*/".chars().find(|x| *x == c).is_some()),
            rest: satisfy(|c| "+-*/".chars().find(|x| *x == c).is_some()),
            reserved: ["+", "-", "*", "/"].iter().map(|x| (*x).into()).collect()
        },
        comment_start: "/*",
        comment_end: "*/",
        comment_line: "//"
    });
    let id = env.identifier();//An identifier parser
    let integer = env.integer();//An integer parser
    let result = (id, integer).parse_state("this /* Skips comments */ 42");
    assert_eq!(result, Ok(((String::from("this"), 42), "")));
}
```

## Links

[Documention](https://marwes.github.io/parser-combinators-language/parser_combinators_language/index.html)

[parser-combinators](https://github.com/Marwes/parser-combinators)
