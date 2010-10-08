
{{{
from sympy import init_printing
init_printing(True, 'lex', True)
}}}

{{{
from sympy import Symbol, symbols, var
}}}

{{{
Symbol('x')
///
x
}}}

{{{
Symbol('tau')
///
τ
}}}

{{{
from sympy.abc import tau, zeta
tau, zeta
///
(τ, ζ)
}}}

{{{
symbols('x,y,z')
///
(x, y, z)
}}}

{{{
symbols('a b c')
///
(a, b, c)
}}}

{{{
symbols('x:5')
///
(x0, x1, x2, x3, x4)
}}}

{{{
symbols(('x1:3', 'y:3'))
///
((x1, x2), (y0, y1, y2))
}}}

{{{
symbols('x:z')
///
(x, y, z)
}}}

{{{
symbols('a:d,x:z')
///
(a, b, c, d, x, y, z)
}}}

{{{
symbols(('a:d', 'x:z'))
///
((a, b, c, d), (x, y, z))
}}}

{{{
_ = symbols('u,v,w')
}}}

{{{
u**v**w
}}}

{{{
_ = var('u,v,w')
}}}

{{{
u**v**w
}}}

