# Planet math

### Circle:

a = radius, [equation at wolframalpha](http://www.wolframalpha.com/input/?i=circle)

	x^2+y^2 = a^2

### Sphere:
 
a = radius, [equation at wolframalpha](http://www.wolframalpha.com/input/?i=sphere)

	x^2+y^2+z^2 = a^2

### Line:

P = point, V = direction normal, [equation at wolframalpha](http://www.wolframalpha.com/input/?i=line) 

	x = P.x + V.x * t
	y = P.y + V.t * t
	z = P.z + V.z * t

### Formula to solve:

a = radius, to simplify sphere origio is 0,0

find t where:

	(P.x+V.x*t)^2+(P.y+V.y*t)^2+(P.z+V.z*t)^2 = a^2

=>

	t = ???

is [this](http://www.wolframalpha.com/input/?i=solve+%28p1%2Bv1*t%29%5E2%2B%28p2%2Bv2*t%29%5E2%2B%28p3%2Bv3*t%29%5E2+%3D+a%5E2+for+t) a proper solution?

from the link above

	v1^2+v2^2+v3^2!=0

and

	t = (-1/2 sqrt((-2 p1 v1-2 p2 v2-2 p3 v3)^2-4 (-v1^2-v2^2-v3^2) (a^2-p1^2-p2^2-p3^2))-p1 v1-p2 v2-p3 v3)/(v1^2+v2^2+v3^2)
