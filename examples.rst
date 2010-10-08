
::

    Added support for Assume objects in inequality solver

    In [1]: solve([x**2 - 1 <= 0])
    Out[1]: -1 ≤ re(x) ∧ im(x) = 0 ∧ re(x) ≤ 1

    In [2]: solve([x**2 - 1 <= 0, Assume(x, Q.real)])
    Out[2]: -1 ≤ x ∧ x ≤ 1

    In [1]: solve([x**2 > 0, False])
    Out[1]: False


    Added support for complex symbols to inequalities solver

    solve() will compute results in terms of re() and im()
    if symbols are complex (this is the default):

    In [1]: solve(x**2 > 1)
    Out[1]: (re(x) < -1 ∨ 1 < re(x)) ∧ im(x) = 0

    In [2]: global_assumptions.add(Assume(x, Q.real))

    In [3]: solve(x**2 > 1)
    Out[3]: x < -1 ∨ 1 < x

    In [4]: global_assumptions.remove(Assume(x, Q.real))

    Also added support for trivial multivariate systems:

    In [5]: solve([x**2 > 1, y**2 > 2])
    Out[5]:
                               ⎛  ⎽⎽⎽                      ⎽⎽⎽⎞
    (re(x) < -1 ∨ 1 < re(x)) ∧ ⎝╲╱ 2  < re(y) ∨ re(y) < -╲╱ 2 ⎠ ∧ im(x) = 0 ∧ im(y) = 0

    In [6]: solve(x**2*y + x > 1)
    (...)
    NotImplementedError: only univariate inequalities are supported

    In [1]: factor(Eq(x**2 + 2*x + 1, x**3 + 1))
    Out[1]:
           2           ⎛         2⎞
    (1 + x)  = (1 + x)⋅⎝1 - x + x ⎠

    Improved together() and moved to a separate file (#1069)

    In [1]: apart(1/(x + 1)/(x + 5))
    Out[1]:
          1           1
    - ───────── + ─────────
      4⋅(5 + x)   4⋅(1 + x)

    In [2]: together(_)
    Out[2]:
           1
    ───────────────
    (1 + x)⋅(5 + x)

    In [3]: together(1+1/(x+1)**2)
    Out[3]:
               2
    1 + (1 + x)
    ────────────
             2
      (1 + x)

    In [4]: together(1+1/(x*(x+1)))
    Out[4]:
    1 + x⋅(1 + x)
    ─────────────
      x⋅(1 + x)

    In [5]: together(1/(x*(x+1))+1/(x*(x+2)))
    Out[5]:
         3 + 2⋅x
    ─────────────────
    x⋅(1 + x)⋅(2 + x)

    Added more symbolic capabilities to factor() (#1568)

    In [1]: f = x**2 + 2*x + 1

    In [2]: factor(f**20000*y**2 + f**20000*2*y + f**20000)
    Out[2]:
           40000        2
    (1 + x)     ⋅(1 + y)

    (previously hanged)

    In [1]: gcd([x**3 - 1, x**2 - 1, x**2 - 3*x + 2])
    Out[1]: x - 1

    In [2]: lcm([x**3 - 1, x**2 - 1, x**2 - 3*x + 2])
    Out[2]:
     5    4      3    2
    x  - x  - 2⋅x  - x  + x + 2

    In [1]: var('x1,x2,i1,i2,i3')
    Out[1]: (x₁, x₂, i₁, i₂, i₃)

    In [2]: f1 = x1**2 + x2**2

    In [3]: f2 = x1**2*x2**2

    In [4]: f3 = x1**3*x2 - x1*x2**3

    In [5]: G = groebner([f1 - i1, f2 - i2, f3 - i3], wrt='x1,x2')

    In [6]: reduced(x1**7*x2 - x1*x2**7, G, wrt=[x1, x2])[1]
    Out[6]:
                  2
    -i₂⋅i₃ + i₃⋅i₁

    In [7]: _.subs({i1: f1, i2: f2, i3: f3}).expand()
    Out[7]:
        7        7
    x₂⋅x₁  - x₁⋅x₂



    In [5]: RootSum((x - 7)*(x**3 + x + 3)**5, Lambda(x, x**2))
    Out[5]:
                  ⎛ 3           ⎛    2⎞⎞
    49 + 5⋅RootSum⎝x  + x + 3, Λ⎝x, x ⎠⎠

    In [6]: _.doit()
    Out[6]:
           ⎛                     2                        2                        2⎞
           ⎜      ⎛ 3           ⎞          ⎛ 3           ⎞          ⎛ 3           ⎞ ⎟
    49 + 5⋅⎝RootOf⎝x  + x + 3, 0⎠  + RootOf⎝x  + x + 3, 1⎠  + RootOf⎝x  + x + 3, 2⎠ ⎠



