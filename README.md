
import math, operator, ast, statistics, cmath
import matplotlib.pyplot as plt
import numpy as np

print("="*45)
print(" RIZWAN AI MATH MASTER - 20 FEATURES")
print("="*45)

# --- SAFE CALC ---
OPS = {ast.Add: operator.add, ast.Sub: operator.sub, ast.Mult: operator.mul, ast.Div: operator.truediv, ast.Pow: operator.pow, ast.Mod: operator.mod, ast.USub: operator.neg, ast.UAdd: operator.pos}
def safe_cot(x): t=math.tan(math.radians(x)); return 1/t if abs(t)>1e-12 else float('inf')
def safe_sec(x): c=math.cos(math.radians(x)); return 1/c if abs(c)>1e-12 else float('inf')
def safe_cosec(x): s=math.sin(math.radians(x)); return 1/s if abs(s)>1e-12 else float('inf')

FUNCTIONS = {"sin": lambda x: math.sin(math.radians(x)), "cos": lambda x: math.cos(math.radians(x)), "tan": lambda x: math.tan(math.radians(x)), "cot": safe_cot, "sec": safe_sec, "cosec": safe_cosec, "sqrt": math.sqrt, "abs": abs, "log": math.log10, "ln": math.log, "asin": lambda x: math.degrees(math.asin(x)), "acos": lambda x: math.degrees(math.acos(x)), "atan": lambda x: math.degrees(math.atan(x))}

def calc(exp):
    def solve(node):
        if isinstance(node, ast.Constant) and isinstance(node.value, (int,float)): return node.value
        if isinstance(node, ast.BinOp):
            a,b=solve(node.left),solve(node.right)
            if isinstance(node.op, ast.Div) and b==0: raise ValueError("Divide by zero")
            return OPS[type(node.op)](a,b)
        if isinstance(node, ast.UnaryOp): return OPS[type(node.op)](solve(node.operand))
        if isinstance(node, ast.Call): return FUNCTIONS[node.func.id](solve(node.args[0]))
        raise ValueError("Invalid")
    return solve(ast.parse(exp, mode="eval").body)

def show_menu():
    print("""
1. Basic Math 11. Quadratic Equation
2. Trigonometry 12. LCM / HCF
3. Algebra ax+b=0 13. Factorial / nCr / nPr
4. Equation ax+b=c 14. Derivative (numeric)
5. Percentage 15. Integration (numeric)
6. Roots & Powers 16. Matrix (2x2)
7. Geometry 17. Complex Numbers
8. Statistics 18. Logarithms
9. Graph 19. Unit Converter
10. Step-by-Step 20. Table Generator
""")

# 1-10 old functions (with Question/Answer format)
def f1(): q=input("Enter expression: "); print(f"\nQuestion: {q}\nAnswer: {calc(q)}\n")
def f2(): q=input("Enter formula: "); print(f"\nQuestion: {q}\nAnswer: {round(calc(q),8)}\n")
def f3():
    a=float(input("a: ")); b=float(input("b: "))
    print(f"\nQuestion: {a}x + {b} = 0\nAnswer: x = {-b/a}\n" if a!=0 else "\nQuestion: ax+b=0\nAnswer: No/Infinite solution\n")
def f4():
    a=float(input("a: ")); b=float(input("b: ")); c=float(input("c: "))
    print(f"\nQuestion: {a}x + {b} = {c}\nAnswer: x = {(c-b)/a}\n" if a!=0 else "\nAnswer: No solution\n")
def f5(): v=float(input("Number: ")); p=float(input("%: ")); print(f"\nQuestion: {p}% of {v}\nAnswer: {v*p/100}\n")
def f6(): x=float(input("Number: ")); print(f"\nQuestion: sqrt & square of {x}\nAnswer: sqrt={math.sqrt(x) if x>=0 else 'N/A'}, square={x**2}\n")
def f7():
    ch=input("1.Circle 2.Rect 3.Tri 4.Cube: ")
    if ch=="1": r=float(input("r: ")); print(f"\nQuestion: Circle r={r}\nAnswer: {math.pi*r*r}\n")
    elif ch=="2": l=float(input("L: ")); w=float(input("W: ")); print(f"\nQuestion: Rect {l}x{w}\nAnswer: {l*w}\n")
    elif ch=="3": b=float(input("Base: ")); h=float(input("H: ")); print(f"\nQuestion: Tri base={b} h={h}\nAnswer: {b*h/2}\n")
    else: a=float(input("Side: ")); print(f"\nQuestion: Cube side={a}\nAnswer: {a**3}\n")
def f8(): s=input("Numbers: "); n=list(map(float,s.split())); print(f"\nQuestion: {s}\nAnswer: Mean={statistics.mean(n)}, Median={statistics.median(n)}, Mode={statistics.multimode(n)}\n")
def f9():
    eq=input("y = "); print(f"\nQuestion: Graph y={eq}")
    try:
        x=np.linspace(-10,10,400); y=eval(eq.replace("^","**"), {"__builtins__":{}}, {"x":x, "sin":np.sin, "cos":np.cos, "tan":np.tan, "sqrt":np.sqrt, "pi":np.pi})
        plt.plot(x,y); plt.grid(True); plt.show(); print(f"Answer: Graph displayed\n")
    except Exception as e: print(f"Answer: Error {e}\n")
def f10(): q=input("Expression: "); print(f"\nQuestion: {q}\nAnswer: {calc(q)}\n")

# 11-20 NEW
def f11(): # Quadratic
    a=float(input("a: ")); b=float(input("b: ")); c=float(input("c: "))
    q=f"{a}x^2 + {b}x + {c} = 0"; D=b*b-4*a*c
    if D>=0: x1=(-b+math.sqrt(D))/(2*a); x2=(-b-math.sqrt(D))/(2*a); print(f"\nQuestion: {q}\nAnswer: x1={x1}, x2={x2}\n")
    else: x1=(-b+cmath.sqrt(D))/(2*a); x2=(-b-cmath.sqrt(D))/(2*a); print(f"\nQuestion: {q}\nAnswer: Complex roots x1={x1}, x2={x2}\n")

def f12(): a=int(input("First number: ")); b=int(input("Second number: ")); print(f"\nQuestion: LCM/HCF of {a},{b}\nAnswer: LCM={math.lcm(a,b)}, HCF={math.gcd(a,b)}\n")

def f13():
    n=int(input("n: ")); print(f"\nQuestion: factorial of {n}\nAnswer: {math.factorial(n)}\n")
    r=int(input("For nCr/nPr enter r: ")); print(f"Answer: nCr={math.comb(n,r)}, nPr={math.perm(n,r)}\n")

def f14(): # derivative numeric
    eq=input("Enter f(x) e.g. x**2: "); x=float(input("x at which derivative: "))
    h=1e-6; f=lambda X: eval(eq, {"__builtins__":{}}, {"x":X, "sin":math.sin, "cos":math.cos, "sqrt":math.sqrt, "pi":math.pi, "exp":math.exp, "log":math.log})
    deriv=(f(x+h)-f(x-h))/(2*h); print(f"\nQuestion: derivative of {eq} at x={x}\nAnswer: {deriv}\n")

def f15(): # integration numeric
    eq=input("Enter f(x) e.g. x**2: "); a=float(input("lower limit: ")); b=float(input("upper limit: "))
    xs=np.linspace(a,b,10000); ys=[eval(eq, {"__builtins__":{}}, {"x":X, "sin":math.sin, "cos":math.cos, "sqrt":math.sqrt, "pi":math.pi}) for X in xs]
    area=np.trapz(ys,xs); print(f"\nQuestion: integration of {eq} from {a} to {b}\nAnswer: {area}\n")

def f16(): print("\nQuestion: Matrix 2x2\nEnter A = [a b; c d]"); a,b,c,d=map(float,input("A (a b c d): ").split()); det=a*d-b*c; print(f"Answer: Determinant = {det}\nInverse exists? {'Yes' if det!=0 else 'No'}\n")

def f17(): s=input("Complex e.g. 3+4j: "); z=complex(s); print(f"\nQuestion: {s}\nAnswer: |z|={abs(z)}, Phase={cmath.phase(z)} rad, Conjugate={z.conjugate()}\n")

def f18(): x=float(input("Number: ")); print(f"\nQuestion: log of {x}\nAnswer: log10={math.log10(x)}, ln={math.log(x)}\n")

def f19(): km=float(input("Enter KM: ")); print(f"\nQuestion: {km} KM\nAnswer: {km*1000} meters, {km*0.621371} miles\n")

def f20(): n=int(input("Number for table: ")); t="\n".join([f"{n} x {i} = {n*i}" for i in range(1,11)]); print(f"\nQuestion: Table of {n}\nAnswer:\n{t}\n")

mp={"1":f1,"2":f2,"3":f3,"4":f4,"5":f5,"6":f6,"7":f7,"8":f8,"9":f9,"10":f10,"11":f11,"12":f12,"13":f13,"14":f14,"15":f15,"16":f16,"17":f17,"18":f18,"19":f19,"20":f20}
while True:
    show_menu()
    ch=input("Choose (1-20): ").strip()
    if ch.lower()=="exit": print("Allah Hafiz!"); break
    try: mp.get(ch, lambda: print("Choose 1-20"))()
    except Exception as e: print(f"\nQuestion: Error\nAnswer: {e}\n")