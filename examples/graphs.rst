
{{{
from sympy import init_printing
init_printing(True, 'lex', True)
}}}

{{{
from sympy import var, Symbol, factor, groebner, solve, degree, split, exp, I, pi
}}}

{{{
V = range(1, 12+1)
E = [(1,2),(2,3),(1,4),(1,6),(1,12),(2,5),(2,7),(3,8),
     (3,10),(4,11),(4,9),(5,6),(6,7),(7,8),(8,9),(9,10),
     (10,11),(11,12),(5,12),(5,9),(6,10),(7,11),(8,12)]
}}}

{{{
X = [ Symbol('x' + str(i)) for i in V ]
E = [ (X[i-1], X[j-1]) for i, j in E ]
}}}

{{{
F3 = [ x**3 - 1 for x in X ]
Fg = [ x**2 + x*y + y**2 for x, y in E ]
}}}

{{{
G = groebner(F3 + Fg, X)
G != [1]
///
True
}}}

{{{
G
}}}

{{{
x3, x4 = X[2], X[3]
}}}

{{{
groebner(F3 + Fg + [x3**2 + x3*x4 + x4**2], X)
}}}

{{{
///
(x_i, x_j)
}}}

{{{
var('a,b')
factor(a**3 - b**3)
///
        ⎛   2        2⎞
(a - b)⋅⎝a  + a⋅b + b ⎠
}}}

{{{
zeta = exp(pi*I/3).expand(complex=True)
zeta
///
    ___
I⋅╲╱ 3
─────── - 1/2
   2
}}}

{{{
var('z')
factor(z**3 - 1, extension=zeta)
///
        ⎛        ___      ⎞ ⎛        ___      ⎞
        ⎜    I⋅╲╱ 3       ⎟ ⎜    I⋅╲╱ 3       ⎟
(z - 1)⋅⎜z + ─────── + 1/2⎟⋅⎜z - ─────── + 1/2⎟
        ⎝       2         ⎠ ⎝       2         ⎠
}}}

{{{
roots = [ solve(arg, z)[0] for arg in _.args if arg.is_Add ]
roots
///
⎡         ___            ___      ⎤
⎢     I⋅╲╱ 3         I⋅╲╱ 3       ⎥
⎢1, - ─────── - 1/2, ─────── - 1/2⎥
⎣        2              2         ⎦
}}}

{{{
var('red, green, blue')
///
(red, green, blue)
}}}

{{{
colors = zip(roots, _)
colors
///
⎡          ⎛      ___             ⎞  ⎛    ___            ⎞⎤
⎢          ⎜  I⋅╲╱ 3              ⎟  ⎜I⋅╲╱ 3             ⎟⎥
⎢(1, red), ⎜- ─────── - 1/2, green⎟, ⎜─────── - 1/2, blue⎟⎥
⎣          ⎝     2                ⎠  ⎝   2               ⎠⎦
}}}

{{{
key = lambda f: (degree(f), len(f.args))
groups = split(G, key, reverse=True)
}}}

{{{
groups[0]
}}}
{{{
groups[1]
}}}
{{{
groups[2]
}}}
{{{
groups[3]
}}}

{{{
colorings = solve(G, X)
}}}

{{{
len(colorings)
///
6
}}}

{{{
for coloring in colorings:
    print [ elt.expand(complex=True).subs(colors) for elt in coloring ]
///
[blue, green, red, red, blue, green, red, blue, green, blue, green, red]
[green, blue, red, red, green, blue, red, green, blue, green, blue, red]
[green, red, blue, blue, green, red, blue, green, red, green, red, blue]
[red, green, blue, blue, red, green, blue, red, green, red, green, blue]
[blue, red, green, green, blue, red, green, blue, red, blue, red, green]
[red, blue, green, green, red, blue, green, red, blue, red, blue, green]
}}}

