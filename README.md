# combinators-js

[![npm version](https://badge.fury.io/js/combinators-js.svg)](http://badge.fury.io/js/combinators-js)

Here are some instructions:

```bash
npm i -S combinators-js
```

```javascript
import {
  B, C, D, E, F, G, H, I, J, K, KI, L, M, O, Q, R, S, T, U, V, W, Y,
} from 'combinators-js';

// or use require or whatever, but you're good to go!
```

Here are the included combinators with their definitions (they are transpiled to standard functions so will run in any JS evironmment):

```javascript
const B = a => b => c => a(b(c));
const C = a => b => c => a(c)(b);
const D = a => b => c => d => a(b)(c(d));
const E = a => b => c => d => e => a(b)(c(d)(e));
const F = a => b => c => c(b)(a);
const G = a => b => c => d => a(d)(b(c));
const H = a => b => c => a(b)(c)(b);
const I = a => a;
const J = a => b => c => d => a(b)(a(d)(c));
const K = a => b => a;
const KI = a => b => b;
const L = a => b => a(b(b));
const M = a => a(a);
const O = a => b => b(a(b));
const Q = a => b => c => b(a(c));
const R = a => b => c => b(c)(a);
const S = a => b => c => a(c)(b(c));
const T = a => b => b(a);
const U = a => b => b(a(a)(b));
const V = a => b => c => c(a)(b);
const W = a => b => a(b)(b);
const Y = a => (b => b(b))(b => a(c => b(b)(c)));
```

Here are the tests:

```javascript
test('B')(S(K(S))(K));
test('C')(S(S(K(S(K(S))(K)))(S))(K(K)));
test('D')(S(K(S(K(S))(K))));
test('E')(S(K(S(K(S(K(S))(K)))(S(K(S))(K)))));
test('F')(S(K(S(S(K)(K))(K(S(K(S(S(K)(K))))(K)))))(S(K(S(K(S(K(S))(K)))(S(K(S))(K))))(S(K(S(S(K)(K))))(K))));
test('G')(S(K(S(K(S))(K)))(S(S(K(S(K(S))(K)))(S))(K(K))));
test('H')(S(K(S(K(S(S(K(S(S(K)(K))(S(K)(K))))(S(K(S(K(S))(K)))(S(K(S(S(K)(K))))(K))))))(K)))(S(K(S(S(K(S(K(S))(K)))(S))(K(K))))));
test('I')(S(K)(K));
test('J')(S(K(S(K(S(S(K(S(K(S))(K)))(S))(K(K))))))(S(S(K(S(S(K)(K))(S(K)(K))))(S(K(S(K(S))(K)))(S(K(S(S(K)(K))))(K))))(K(S(K(S(S(K(S(K(S))(K)))(S))(K(K))))(S(K(S(K(S(K(S))(K)))(S(K(S))(K)))))))));
test('K')(K);
test('KI')(K(S(K)(K)));
test('L')(S(S(K(S))(K))(K(S(S(K)(K))(S(K)(K)))));
test('M')(S(S(K)(K))(S(K)(K)));
test('O')(S(S(K)(K)));
test('Q')(S(K(S(S(K(S))(K))))(K));
test('R')(S(K(S(K(S))(K)))(S(K(S(S(K)(K))))(K)));
test('S')(S);
test('T')(S(K(S(S(K)(K))))(K));
test('U')(S(K(S(S(K)(K))))(S(S(K)(K))(S(K)(K))));
test('V')(S(K(S(S(K(S(K(S))(K)))(S))(K(K))))(S(K(S(S(K)(K))))(K)));
test('W')(S(K(S(S(K(S(S(K)(K))(S(K)(K))))(S(K(S(K(S))(K)))(S(K(S(S(K)(K))))(K))))))(K));
```

Here are some ideas:

```javascript
// LISP data structures
const cons = (a, b) => V(a)(b); // manual uncurry
const car = T(K);
const cdr = T(KI);

console.log(car(cons(0, 1))); // => 0
console.log(cdr(cons(0, 1))); // => 1

const nil = () => {};
const list = (...args) => args.reduce((l, arg) => V(arg)(l), nil);
const reverse = (l, m = nil) => l === nil ? m : reverse(l(KI), V(l(K))(m));
const reduce = f => l => m => l(KI) === undefined ? m : f(reduce(f)(l(KI))(m))(l(K))
const map = f => l => reduce(acc => val => V(f(val))(acc))(l)(nil);
const length = l => reduce(acc => val => 1 + acc)(l)(0);
const filter = f => l => reduce(acc => val => f(val) ? V(val)(acc) : acc)(l)(nil);

const arbitraryList = list(0, 1, 2, 3, 4, 5);

console.log(length(arbitraryList)); // => 6

const reduced = reduce(acc => val => V(val)(acc))(arbitraryList)(nil);
const filtered = filter(x => x > 2)(reduced);
const mapped = map(x => x ** 2)(filtered);
const reversed = reverse(mapped);

console.log(length(reversed)); // => 3
map(::console.log)(reversed); // => 25 16 9
```

```javascript
// recursion of anonymous functions
Y(recur => x => (x === 1 ? 1 : x * recur(x - 1)))(5); // => 120

// TCO'd recursion of anonymous functions using a modified Y
// taking a variadic non-combinator function
const Y_ = a => (b => a((...c) => b(b)(...c)))(b => a((...c) => b(b)(...c)));
Y_(recur => (x, y = 1) => x === 1 ? y : recur(x - 1, x * y))(5); // => 120
```

```javascript
// omega bird (mock a mockingbird)
M(M);
```

Here are some practical ideas:

```javascript

```

Here are some resources:
- [To Mock a Mockingbird](https://en.wikipedia.org/wiki/To_Mock_a_Mockingbird)
- [http://www.angelfire.com/tx4/cus/combinator/birds.html](http://www.angelfire.com/tx4/cus/combinator/birds.html)
- [https://github.com/raganwald/oscin.es](https://github.com/raganwald/oscin.es)
- [http://raganwald.com/2015/02/13/functional-quantum-electrodynamics.html](http://raganwald.com/2015/02/13/functional-quantum-electrodynamics.html)
- [http://dkeenan.com/Lambda/](http://dkeenan.com/Lambda/)
