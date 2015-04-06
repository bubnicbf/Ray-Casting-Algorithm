# Ray-Casting-Algorithm
checks to see if a point is inside or outside the polygon using the ray-casting algorithm

One simple way of finding whether the point is inside or outside a simple polygon is to test how many times a ray, starting from the point and going in any fixed direction, intersects the edges of the polygon. If the point in question is not on the boundary of the polygon, the number of intersections is an even number if the point is outside, and it is odd if inside. 

The algorithm is based on a simple observation that if a point moves along a ray from infinity to the probe point and if it crosses the boundary of a polygon, possibly several times, then it alternately goes from the outside to inside, then from the inside to the outside, etc. As a result, after every two "border crossings" the moving point goes outside. This observation may be mathematically proved using the Jordan curve theorem, which asserts that every Jordan curve divides the plane into an "interior" region bounded by the curve and an "exterior" region containing all of the nearby and far away exterior points, so that any continuous path connecting a point of one region to a point of the other intersects with that loop somewhere. 

If implemented on a computer with finite precision arithmetics, the results may be incorrect if the point lies very close to that boundary, because of rounding errors. This is not normally a concern, as speed is much more important than complete accuracy in most applications of computer graphics. However, for a formally correct computer program, one would have to introduce a numerical tolerance ε and test in line whether $P$ lies within $ε$ of $L$, in which case the algorithm should stop and report "P lies very close to the boundary."

Most implementations of the ray casting algorithm consecutively check intersections of a ray with all sides of the polygon in turn. In this case the following problem must be addressed. If the ray passes exactly through a vertex of a polygon, then it will intersect 2 segments at their endpoints. While it is OK for the case of the topmost vertex in the example or the vertex between crossing 4 and 5, the case of the rightmost vertex (in the example) requires that we count one intersection for the algorithm to work correctly. A similar problem arises with horizontal segments that happen to fall on the ray. The issue is solved as follows: If the intersection point is a vertex of a tested polygon side, then the intersection counts only if the second vertex of the side lies below the ray. This is effectively equivalent to considering vertices on the ray as lying slightly above the ray.

Once again, the case of the ray passing through a vertex may pose numerical problems in finite precision arithmetics: for two sides adjacent to the same vertex the straightforward computation of the intersection with a ray may not give the vertex in both cases. If the polygon is specified by its vertices, then this problem is eliminated by checking the y-coordinates of the ray and the ends of the tested polygon side before actual computation of the intersection. In other cases, when polygon sides are computed from other types of data, other tricks must be applied for the numerical robustness of the algorithm

####Example

An intuitive explanation of why it works is that every time we cross a border, we change "country" (inside-outside, or outside-inside), but the last "country" we land on is surely outside (since the inside of the polygon is finite, while the ray continues towards infinity). So, if we crossed an odd number of borders we were surely inside, otherwise we were outside; we can follow the ray backward to see it better: starting from outside, only an odd number of crossing can give an inside: outside-inside, outside-inside-outside-inside, and so on (the - represents the crossing of a border).

ray_intersects_segment:
    P : the point from which the ray starts
    A : the end-point of the segment with the smallest y coordinate
        (A must be "below" B)
    B : the end-point of the segment with the greatest y coordinate
        (B must be "above" A)
 if Py = Ay or Py = By then
   Py ← Py + ε
 end if
 if Py < Ay or Py > By then 
   return false
 else if Px > max(Ax, Bx) then 
   return false
 else
   if Px < min(Ax, Bx) then
     return true
   else
     if Ax ≠ Bx then
       m_red ← (By - Ay)/(Bx - Ax)
     else
       m_red ← ∞
     end if
     if Ax ≠ Px then
       m_blue ← (Py - Ay)/(Px - Ax)
     else
       m_blue ← ∞
     end if
     if m_blue ≥ m_red then
       return true
     else
       return false
     end if
   end if
 end if
