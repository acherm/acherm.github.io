---
layout: post
title:  "Testing your DSLs in Langium"
date:   2023-11-05 011:54:29 +0200
tags: [xtext, langium, DSL, compiler, interpreter, testing]
---


[Langium](https://langium.org/) is a framework to build domain-specific languages (DSLs). 
The principle is to write a grammar-like specification and obtains for ~~free~~ zero-effort a parser, an abstract syntax tree (AST), a customizable and advanced editor that can run on the Web or on any modern IDEs such as VSCode, and facilities to interpret or compile programs written in your DSL. Basically, engineering external, textual DSLs at the speed of light! 
Langium is the successor of [Xtext](https://www.eclipse.org/Xtext/) and is under active development. 
Langium targets TypeScript, Language Server Protocol (LSP), VSCode, and Web technologies.
One "feature" I am missing from Xtext is the ability to programmatically test your DSL: test your syntax and conformant/illegal programs, test your interpreter or multiple compilers of your DSL, etc.
In this post, I want to outline a setup for facilitating the testing of a DSL using Langium.


## Running example

Let us consider a simple DSL for chess game. 

```typescript
grammar ChessGame

entry Game: "White:" whitePlayer=STRING "Black:" blackPlayer=STRING (moves+=Move)+;

Move: AlgebraicMove | SpokenMove;

AlgebraicMove: (piece=Piece)? source=Square (captures?='x')? dest=Square;

SpokenMove: piece=Piece 'at' source=Square (captures?='captures' capturedPiece=Piece 'at' | 'moves_to') dest=Square;

// regular expression ('a'..'h')('1'..'8');
terminal Square: /[a-h][1-8]/; 

Piece returns string: Piece_PAWN | Piece_KNIGHT | Piece_BISHOP | Piece_ROOK | Piece_QUEEN | Piece_KING;
Piece_PAWN returns string: 'P' | 'pawn';
Piece_KNIGHT returns string: 'N' | 'knight'; // not K, because of King
Piece_KING returns string: 'K' | 'king';
Piece_BISHOP returns string: 'B' | 'bishop';
Piece_QUEEN returns string: 'Q' | 'queen';
Piece_ROOK returns string: 'R' | 'rook';

hidden terminal WS: /\s+/;
terminal ID: /[_a-zA-Z][\w_]*/;
terminal STRING: /"[^"]*"/;

hidden terminal ML_COMMENT: /\/\*[\s\S]*?\*\//;
hidden terminal SL_COMMENT: /\/\/[^\n\r]*/;
```

It is actually based on the old tutorial of Xtext in 2009 (https://www.slideshare.net/HeikoB/xtext-at-eclipse-democamp-london-in-june-2009). 
The DSL has limited, practical interest ([PGN](https://en.wikipedia.org/wiki/Portable_Game_Notation) is better!), but why not for having a more human-readable DSL for chess games? 

Out of the grammar, you can enjoy typing a program (a chess game) using the VS Code generated editor.

![program](/assets/LangiumProgramDSL.png)

Playing with the editor is fun, but your tries and errors will go away: How to test more systematically and automatically the syntax of your DSL?

## Testing the syntax

I have used [Xtext](https://www.eclipse.org/Xtext/) for many years, as part of teaching and research, and I enjoyed facilities to test DSL (see eg https://blog.mathieuacher.com/XtextStandaloneParsing/).
Xtext was oriented towards Java, Eclipse, and EMF as opposed to Langium that targets TypeScript, VSCode, and Web technologies. 
I was seeking a solution for testing DSLs in Langium and I found a solution that I want to share.

We first need a helper function that loads any program written in your DSL and returns the AST. 
In TypeScript and leveraging the Langium API:

```typescript
import { describe, expect, test } from 'vitest';

import type { Game } from '../language/generated/ast.js';

import { AstNode, EmptyFileSystem, LangiumDocument } from 'langium';
import { parseDocument } from 'langium/test';
import { createChessGameServices } from '../language/chess-game-module.js';
const services = createChessGameServices(EmptyFileSystem).ChessGame; 
...

async function assertModelNoErrors(modelText: string) : Promise<Game> {
    var doc : LangiumDocument<AstNode> = await parseDocument(services, modelText)
    const db = services.shared.workspace.DocumentBuilder
    await db.build([doc], {validation: true});
    const model = (doc.parseResult.value as Game);
    expect(model.$document?.diagnostics?.length).toBe(0);
    return model;    
}
```

The imports and Langium facilities are not well documented right now. 
The `parseDocument` function is key to load a program and obtain the AST.
`Promise<Game>` is the type of the AST and is specific to our DSL.
Same for `const services = createChessGameServices(EmptyFileSystem).ChessGame;`.
You should adapt to your grammar and main entry point (rule) or DSL name.

You can then call this function in a test, and puts further assertions on the AST, leveraging [vitest](https://vitest.dev/).
For instance:

```typescript
describe('Test basic game', () => {
    test('Two moves', async () => {
        const game = await assertModelNoErrors(`
        White: "INSA INFO5"
        Black: "ChatGPT"
        
        e2 e4
        e7 e5
        `)
        expect(game.whitePlayer).toBe("INSA INFO5");
        expect(game.moves.length).toBe(2);
    });
...
});
```

You can check some representative programs of your DSL that should be accepted or rejected by the parser.
More examples here: https://github.com/acherm/dsl-langium/blob/main/Chess/src/test/validation-test.ts

## Testing interpreters and compilers

In the same spirit, you can test the interpreter or compiler of your DSL.

For instance:

```typescript
import { convertMovesToPGNWithPython, generateMoves } from '../generator/pgn_converter.js';

...

test('Immortal game', async () => {

        const game = await assertModelNoErrors(`
        White: "Adolf Anderssen"
        Black: "Jean Dufresne"
        
        pawn at e2 moves_to e4 
        pawn at e7 moves_to e5        
        pawn at f2 moves_to f4 
        P at e5 captures pawn at f4 
        bishop at f1 moves_to c4
        queen at d8 moves_to h4
        king at e1 moves_to f1 
        P at b7 moves_to b5
        bishop at c4 captures pawn at b5 
        knight at g8 moves_to f6        
        `);

        const pgn_moves = generateMoves(game.moves);
        expect(pgn_moves).toBe("1. e4 e5 2. f4 exf4 3. Bc4 Qh4+ 4. Kf1 b5 5. Bxb5 Nf6");
        (async () => {
            const pgn_moves_with_python = await convertMovesToPGNWithPython(game.moves);
            expect(pgn_moves).toBe(pgn_moves_with_python);
        })();
    });
```

I tested an interpreter based on [chess.js](https://github.com/jhlywa/chess.js/tree/master) that translates a chess game into [PGN](https://en.wikipedia.org/wiki/Portable_Game_Notation) format. 
I also tested another translation into PGN, this time generating and executing Python code (based on [python-chess](https://python-chess.readthedocs.io/en/latest/)).
More details here: https://github.com/acherm/dsl-langium/blob/main/Chess/src/generator/pgn_converter.ts and https://github.com/acherm/dsl-langium/blob/main/Chess/src/test/pgn-test.ts 

### Other technicalities

To make it work, you have to set up a few things in your project:
 * adding a `vite.config.ts` to set up `vite` testing framework (eg see https://github.com/acherm/dsl-langium/blob/main/Chess/vite.config.ts)
 * in the `package.json` adding a script rule `"test": "vitest"` and `devDependencies` something like `"vitest": "^0.27.3",`, in such a way you can use `npm run test` (eg see https://github.com/acherm/dsl-langium/blob/main/Chess/package.json)


## Conclusion

I have shown how to test your DSLs in Langium, leveraging the Langium API and the [vitest](https://vitest.dev/) testing framework.
For sure, other testing frameworks can be used as well.
The solution I have presented, quite closed to ParseHelper in Xtext, can be systematized to any DSL. 
I hope new versions of Langium will generate such testing facilities out of the box. 

Github of the illustrative project: https://github.com/acherm/dsl-langium/tree/main/Chess. 
See also DSLs in Langium here: https://github.com/acherm/dsl-langium/. 




















 







 







 














