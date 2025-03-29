## THE  TRUCK DISPATCHING PROBLEM*

## G.  B.  DANTZIG 1  A.ND  J.  H.  RAMSER'

The  paper  is concerned  with  the  optimum  routing  of a fleet  of gasoline  de› livery  trucks  between  a  bulk  terminal  and  a large  number  of service  stations supplied  by  the  terminal. The  shortest routes  between  any  two  points  in  the system  are  given  and  a  demand  for  one or  several  products is  specified for  a number  of stations  within  the  distribution system.  It  is  desired  to  find a  way to assign stations  to trucks  in such a manner  that  station  demands  are satisfied and  total  mileage  covered  by  the  fleet is a minimum.  A procedure  based  on a linear  programming  formulation  is given for obtaining  a near  optimal  solution. The calculations  may be readily  performed  by hand  or by an automatic  digital computing  machine.  No practical  applications  of the  method  have  been made as yet.  A number  of trial  problems  have  been  calculated,  however.

## 1.  Introduction

The  "Truck  Dispatching  Problem"  formulated  in  this  paper  may  be  con› sidered asa  generalization of  the "Traveling-Salesman Problem" .(1)  In its simplest form  the  Traveling-Salesman  Problem  is  concerned with  the  determination  of the  shortest  route  which passes through  each of n given points  once. Assuming that  each pair  of points  is joined by  a link, the  total  number  of different routes through n points  is ‰n !. Even  for small values of n the  total  number  of routes is exceedingly large, e.g. for n =  15, there  are  653,837,184,000  different routes. One of the  authors  has  collaborated with  Fulkerson  and  Johnson  in developing a  "cutting plane"  approach  for  testing  whether  a  proposed  tour  is optimal  or finding an  improved  solution if it  is not.(2),  (3)

The  Traveling-Salesman  Problem  may  be  generalized  by  introducing  addi› tional conditions. Thus, the salesman may be required to return  to the  "terminal point"  whenever he has  contacted m of the n -I  remaining  points, m being a  divisor of n -1. For  given n and m the  problem  is to  find loops  such that all loops have a specified point  in common and  total  loop length  is a minimum. Since the  loops have  one  point  in  common,  this  problem  may  be  called  the "Clover  Leaf Problem".  If m is  small,  optimal  sets  of m points  may  often  be determined by inspection of a map which  contains the  points and  the  arcs con› necting  them.  One would look for  "clusters  of points"  and  determine  by  trial and  error the  order in which they  should be traversed,  taking  care that no loop crosses itself.  However, when  clusters are  not  present  in  sufficient numbers  or when m is  large,  this  procedure  becomes inapplicable.  In  this case  near-best solutions may  be obtained  by  the  algorithm  in  this  paper.

## 2. The  Truck  Dispatching Problem

The  Traveling  Salesman  Problem  may  also  be  generalized by  imposing the condition  that  specified deliveries qi be made  at  every  point P, (excepting the terminal  point).  If  the  capacity  of the  carrier

- * Received  November  1958
- 1 T'he RAND Corporation, Santa  Monica,  California
- 1 T'he  Atlantic  Refining  Company, Philadelphia,  Penmylvania

$$c \geq \sum _ { z } q _ { i },$$

the  problem is formally  identical  with  the  Traveling-Salesman Problem  in  its original form since the  carrier  can serve every delivery point  on one trip  which links all the  points.

In  the  "Truck  Dispatching  Problem"  the  relation  between C and  the q, is such that  the  carrier  can  only  make  between  1 and t deliveries on  each  trip. Thus, the  Truck  Dispatching  Problem is characterized  by the  relation

$$C \ll \sum _ { \xi } \,.$$

It  is obvious that  the  number  of carriers does not  enter  the  problem when they have the  same capacity.  Even when carriers of different capacities are involved, or when a  number  of different products  are  to  be delivered to  every point,  the same mathematical  model may  be used as  will be shown below. For  simplicity of presentation  it  will be assumed first that  only one product  is to  be delivered and that  all  trucks  have  the  same  capacity C.

The method  of solution starts  from the  basic idea to  synthesize the  solution in  a  number  of  "stages  of aggregation"  in  which suboptimizations  are  carried out on pairs  of points  or  groups.  The  number  of stages of aggregations  to  be employed is  determined  as  follows: order  the  deliveries, q, , in  a  sequence qi , q2, · · · q,, q.+i, · · · q,. such  that qi ~ q.+i for  any  i =  1,  · · · n -1.  Then determine t such that

$$\sum _ { i = 1 } ^ { t } q _ { i } \leqq C \text{ and } \sum _ { i = 1 } ^ { t + 1 } q _ { i } > C.$$

t obviously represents  the  maximum number  of deliveries which a  truck  of ca› pacity C can make for a given set of q/s. Since the  sequence qi , q2 , · · · q, rep› resents a feasible combination it may  be in the  optimal  solution. Therefore, the method of calculating the  number  of aggregations to  be employed must  admit the combination  q1  , ~ ,  · · · q, in  the  final aggregation.  This  will be  the  case if the number  of stages of aggregations, N, is determined  such that

## N ~ lo~ t

since 2N is the  largest  number  of points  aggregated  in  the  Nth  and  final stage of aggregation.

For  example if C =  6000 and  the q. are  the  values  shown in  column Q of Table 1, the  ordered sequence of q/s is

$$1 1 0 0, \, 1 2 0 0, \, 1 2 0 0, \, 1 4 0 0, \, 1 4 0 0, \, 1 5 0 0, \, \cdots \, 1 9 0 0$$

and t =  4.  Therefore N =  lo~  4  =  2. In  the  first  stage  of aggregation  only those points  are  allowed to  pair  up  whose combined demand  does not  exceed ‰C.  n  the  second  stage  of aggregation any  pair  contained  in  the  suboptimal I solution  of stage  1 may then  be combined with any other pair without  exceeding the truck capacity.  If C = 7000 and the q/s are again the values listed in Table  1, t = 5 and N = 3. In the first stage of aggregation only those points are allowed to  pair  up  whose  combined demand  does not  exceed …C. The  pairings  in  the

TABLE 1

![Image](image_000000_92fe0c0a2ac3f237a72710e2c1849b75f20c1032a7f1b54e2485c0f4f176cad9.png)

I

second stage  then  do not exceed ‰C nd those in the  third  stage do not  exceed a C. Although N = 2 is a closer approximation  to log2  5 than N = 3, three  stages must  be employed since a total  of 2 stages does not admit  the  feasible combina› tion  1100, 1200, 1200, 1400, 1400, which may  be optimal.

The reader should note that  if a truck  were constrained  to visit precisely two points  and  return,  the  total distance  covered  would be the  constant sum  of distances  from the  terminal  to  each point P; plus the sum  of interpair  distances (we  would therefore  seek to  minimize the  latter). Accordingly, in  each  inter› mediate  stage  the  optimum  pairings  corresponding  to  minimum  interpair  dis› tances  are determined.  In  the  final stage optimum  pairings are determined  such that the  sum of trip  lengths  is a  minimum.  The  details  of this  procedure  will be illustrated  below by way of a numerical example.

The Truck  Dispatching  Problem may now be formally stated  as follows:

- [1] Given a  set of n "station  points" P, (i =  1, 2 ·  ·  · n)  to  which deliveries are  made from point  Po,  called the  "terminal  point".
- [2] A  "Distance  Matrix" [D]  =  [d;;] is  given  which  specifies the  distance d;; = d;; between  every  pair  of points  (i, j =  0,  1, · ·  · n).
- [3] A  "Delivery  Vector" (Q) = (q;) is  given which specifies the  amount q; to  be  delivered to  every  point P; (  i = 1, 2  ·  ·  · n).
- [5] If x;;  = x;; = 1 is interpreted  to  mean  that  points P, and P; are paired
- [4] The  truck  capacity is C, where C &gt;  max. q;.

(i, j =  0,  1 ·  ·  · n) and  if X;; = X;; = 0 means  that  the  points  are  not paired, one obtains  the  condition

$$( 3 )$$

$$\sum _ { \alpha } x _ { i j } = 1 & & ( i = 1, 2 \cdots n )$$

since every  point P, is either  connected  with Po or at  most  one other  point P,. Furthermore,  by  definition, x;, =  0  for  every  i =  0,  1, ·  ·  · n.

[6] The  problem  is to  find those  values  of x,; which make  the  total  distance

$$D = \sum _ { i = 0 } ^ { n } d _ { i j } x, _ { j }$$

$$= \sum _ { i, j = 0 } ^ { n } d _ { i j } x _ { \bullet j }$$

a minimum under  the  conditions  specified in  [2] to  [5].

## 3. Computational Procedure

## 3.1. General Remarks

According to  condition  [5] x;; can  only assume values  which are either  1 or 0. At the  present  time  there  exists  no  general  applicable  method  for  solving  dis› crete variable  problems  except  possibly  some  variant of  a  recent  proposal  of R. Gomory (4) which is in too early  a  stage  of development  to  evaluate  for the problem at  hand.  However,  one may  determine  "best  solutions"  if one  admits the weaker condition

$$0 \leqq x _ { i j } \leqq 1$$

and then applies the well-known methods  of linear programming.  (5) The possible appearance  of fractional  values  in  the  "solution" indicates  the  existence  of al› ternative  pairings  of points  or groups  of points.  Experience  has  shown that  the number of such  alternative pairings  is  small,  so that the  pairing  yielding  least mileage can  be  readily  found  by  trial  and  error.  The  "solution" then  obtained satisfies the  requirement that X;; be  either  O or  1;  however,  the  method,  like other now available  algorithms  for  the  solutions  of  discrete-variable  problems, does not  guarantee  that  the  absolute  optimum  solution  is obtained.  It  is likely that  the  "best  solution" obtained  by  this  method  approaches  the  optimum  as the number  of points  increases.  Moreover,  an  estimate  is at  hand  on  the  error for the minimum D since x;; =  0 or  1 lies between  the  "best  solution"  obtained by the  method  of this  paper  and  the  minimum  satisfying  O ~ x; 3 ~ l.

## 3.2. First-Stage  Aggregations

The detailed  computational  procedure  may  best  be illustrated  by the  numeri› cal example  shown  in  Table 1. Deliveries  are  made  from  point Po to  points Pi, ·  ·  · P 1 2. The  entry  in  the  lower  right  corner  of  each  box  is  the  shortest distance d;; between  points P; and P; .  These  entries  are  the  elements  of  the distance matrix  discussed  in  [2]. The  delivery  vector (Q)  is  shown  at  the  left of the triangular  array.  If C =  6000 gal.,  the  number  of stages  is 2 (see Section 2). The x,; are  entered  into  the  left  upper  corner  of each  box. As a  start  all de› livery points  P 1 , ·  ·  · P12 may  be  paired  with  the  terminal  Po so that  there  are

12 entries  Xo,i = 1 where i =  1, · ·  · 12. These  12 entries  constitute the  "basic set" at  the  start of  computation.  During  each  following iteration  exactly  one element of the  basic set is eliminated and replaced by  a new element. The total number  of basic xwentries  remains  therefore  constant  during  stage  1. The  re› maining left upper corners in each box remain either empty  or are permanently supplied with  a star. The empty  boxes constitute  the  "non-basic set"  of entries which are all equal to O.  However, the  zeros are not  actually  entered  in order to distinguish  them  from the  zeros which belong to  the  basic set.  This  distinction is  necessary because there  are  no  more than  12 basic  entries  in  this particular example. A starred  entry  indicates  that  pairing  of the  respective  points  is not admissible during  first  stage  aggregations since the  combined demand  for such points  exceeds one half  the  truck  capacity.  Thus,  the  combined demand  for P5 and Pa is 1700 + 1900 = 3600 which is greater than  C/2  = 3000.

## 3.3. Rapid Corrections

The  starting  solution,  in  which each  point  is paired  with  the  terminal,  may be readily improved by  a  series of rapid  corrections. This  is desirable since the number of subsequent iterations  decreases as the difference between the  starting and  optimal  solution decreases.

Rapid  corrections  may  be  made  by  bringing  into  the solution  non-basic entries  which correspond to  relatively  small d;,-values.  Thus, de.1 =  7 is  rela› tively  small and  may  be brought  into  the  basic set by  entering  the,  as yet  un› determined,  value O into the corresponding left upper corner. In order to  satisfy equations  (3),  the  sums of basic entries  for any  i  =  1, 2,  ·  ·  · n must  be equal to  1. Since the  distance  matrix  (D)  is symmetrical,  it  follows that the  sum of basic  values  in  row 6 and  column 6 in  the  triangular  array  shown in  Table  1 must  also be equal to 1. The same holds true  for row 7 and column 7. Therefore, the  following entries  are  made:

| Xe,1  =   | (J        | (non-basic entry  Xe,7  =  0 increased)   |
|-----------|-----------|-------------------------------------------|
| Xo,e      | =  1 - (J | (basic entry  xo,e  =  1 corrected)       |
| xo,1      | =  1 - (J | (basic entry  Xo,7  =  1 corrected).      |

The  largest  value  of (J consistent  with  the  inequalities  (  5)  is  1. Therefore

$$x _ { 6, 7 } = 1 \quad x _ { 0, 6 } = 0 \ \text{ and } \ x _ { 0, 7 } = 0.$$

Since one basic entry  must  be dropped,  there  is a  choice between Xo,e  and xo.1 . Since the  distance  do,7 is larger  than do.&amp; , the  basic entry xo,1 is  dropped  from the  basic set in order to  reduce total  distance  as much as possible. The  overall effect of the  sequence of operations just  discussed is that  the  entry xe,1 =  1 has been  added  to  the  basic set,  that  the  value  of Xo,e has  been changed  from  1 to 0 and  that xo,1 has been dropped. Geometrically this means that  points  6 and 7 have  been  severed  from  the  terminal  and  connected  with  each  other. This process of making rapid corrections is repeated  as long as non-basic entries  with obviously  low d;,-values  are  available.  A  good rule  for  obtaining  a  quick  im› provement of the solution is to skip over any dwvalues  if entries Xo,; or Xo,;  have

II

![Image](image_000001_705845a68a19f5b71e67eb6eb8508152fc92139230734e04c734dc720130760c.png)

already  been  dropped  from  the  basic  set  or  have  the  value  0.  Starting  with Table  I  the  first  four  iterations  are  as  follows:

```
<_SQL_>
```

The new basic set  is shown in  Table  2 in  which the  8-entries should be disre› garded for the  moment.  As a  result  of the  first four rapid  corrections the  sum of interpair  distances  has  been  reduced  from  364 to  170 units.

## 3.4. The DeUa-Function rel,ative ost  factors) ( c

When a  sufficient number  of pairs  of points  with  small interpoint  distances have been brought into the solution, it will become increasingly difficult to bring in additional pairs of points without  calculating the  total  distance in every case. This is  equivalent  to  a  trial  and  error  procedure  which  would  necessitate  an enormous amount  of computation  even if the  number  of points  is only moder› ately Jarge.  What  is needed therefore is a  criterion which enables the  computer to either  accept  or  reject  a  non-basic entry  as  the  array  in  Table  2 is scanned box by box. This criterion is provided  by  the  "Delta-Function" defined as

$$\delta _ { i j } ^ { ( n ) } = \pi _ { i } ^ { ( n ) } + \pi _ { j } ^ { ( n ) } - d _ { i j }$$

where r~"l and  1r;"l are  suitably  determined  constants  characteristic  for the nth iteration.  By  definition  1r~"&gt;  nd a 1r~"&gt; are  determined  so that

$$\delta _ { i j } ^ { ( n ) } = 0$$

for  all d,; corresponding to  basic entries.  For  non-basic entries

$$( 8 )$$

The  delta-function  indicates  how much  the  total  distance D will decrease  per unit  increase of a  non-basic entry x;; .  Thus,  if oW &gt; 0, the  total  distance D decreases if the  value  of  the  non-basic  entry x;; is  increased;  if o~7 1 &lt; 0  the total distance D increases  if  the  corresponding  non-basic  value  is  increased. Therefore,  in  scanning  through  the  array  in Table  2, boxes with oW &lt;  0  are disregarded  unless  the o~;&gt; for  all  non-basic  entries  are  negative.  In  this  case every possible choice of non-basic entries  leads to  an  increase of total  distance D represented  by  the  basic  set  of  entries.  The  particular set  obtained  at  this point  represents then  the  "best  solution".

However, as long as at  least  one o~J is positive, a reduction  of total  distance 1 is  possible. If  several o~j&gt; are  larger  than  0,  the  largest  possible reduction  of distance  relative  to  increase  in x;; is  obtained  with  the  largest  value  of ol;'. Therefore,  in  scanning  through  the  array  a  non-basic  entry  is  accepted  if  its o~j 1 is greater  than  the  largest  previously considered o~;&gt;  and  a  non-basic entry is rejected if its  o~;&gt;  s smaller than  the  largest previously noted i o~7· 1 In  the case of equality  it  may  also be  rejected  in order  to  eliminate  arbitrariness  of formal procedure.

The constants  1r~"&gt;  nd  1r}"&gt;  the  delta-function  (6)  may be determined  from a in condition  (7).  This yields the equation

$$\pi _ { i } ^ { ( n ) } + \pi _ { j } ^ { ( n ) } - d _ { i j } = 0$$

in  which one may  arbitrarily  set  ro  =  0  (any  choice of an  integer  ~O  is  ad› missible). Since there  are  12 basic entries  in the  numerical  example of Table  2, there  are  12 equations  of type  (9)  from  which the  12 1r;-values  corresponding to  each  delivery  point P, may  be  determined.  These  values  and  ro =  0  are entered in the lower right corner of the boxes containing the point identifications as shown in Table  2.

After  obtaining  the  1ri"&gt;-values,  the  oi;&gt; are  calculated  from  (6) and  max o~;&gt;  o~:&gt; = determined  in the manner  described above. The non-basic entry x~:&gt; is then  set  equal  to 8 and  the  basic entries  corrected  where necessary by  sub› stracting 8 in such a way that  the  sum of entries in the  rth  row and rth  column, as well as in the sth row and the sth column, are separately equal to 1. In Table 2 max oii =  o&amp;.10 = 25 + 42 17 = 50; accordingly, the non-basic entry  xe.10 = 0 was replaced by  xe.10 = 8. The  8-corrections of basic entries  are  then  made as shown. It  should  be  noted  that a  8-correction cannot  be  applied to the  basic entry  xe.7 =  1 since there  is no other basic entry  in the  7th  row and 7th  column to  which  a  corresponding  correction  can  be  applied.  From  this  point  on  the

procedure is exactly  the  same as discussed above  except for one additional  pro› vision: the  maximum  value  of fJ which leaves basic  entries  non-negative  is de› termined  and  of those  basic entries  which are  zero, but  which would have  gone negative  if fJ would have  been  chosen larger,  exactly  one  corresponding  to  the largest d;; is  dropped  from  the  basic  set.  In  this  case (J =  0  and  Xo, 6 =  0  is dropped. Although  the d;; corresponding to  Xo,n =  0 is larger  than  the d;; asso› ciated with  Xo,6 ,  the  basic  value  Xo,6 =  0 must  be dropped  instead  of Xo,n since a small positive  value  of (J would have  driven  xo, negative  but  would not  have 8 affected xo,n.  If  a  larger  value  of (J would have  driven  both  x0 ,6 and  x 0 , 11 nega› tive, then  Xo,u should be dropped  instead  of xo,6  .  This  provision  concerning the elimination of basic entries  which are  zero is necessary  to  prevent  cycling.

By  iterating this  procedure  non-basic  entries  are  brought  into  the  basic  set until no further  improvement  is possible. As noted  above,  this  is the  case when the  delta-function is  negative  or  zero for  all  non-basic  entries.  This  concludes the  first  stage  aggregations,  in  which  points  were  paired  whose combined  de› mand does not  exceed one half the  truck  capacity.  The  pairings  so obtained  are shown in Figure  I.  It  is  seen that 5 aggregations  contain  2 station  points  each whereas 2  contain  only  one  station point,  the  other  being  the  terminal  point Po.

## 3.5. Second-Stage  Aggregation

The  pairs  of  points  obtained  in  the  first-stage  aggregation  for  the  example may now be  combined with  each  other  without  exceeding the  truck  capacity. Therefore, in  the  second-stage aggregation  there  will, in  general,  be no  starred entries.  The  problem  is  to  combine  the  first-stage  aggregations  in  such  a  way that  the  total distance  becomes a  minimum.  The  first  step  will be  to  set  up  a triangular  distance  table  for pairs  of aggregates  similar to  Table  I.  Designating first-stage aggregations  with  A1, A2, ·  ·  · A7 (see  Figure  1)  and  the  point Po with Ao , one obtains  the  triangular  array  shown in Table  3. The distances shown in the lower right  corner of the  column headed  by Ao correspond to the minimal routes from  the  terminal  to  each point  in the  respective  aggregate  and  back  to the terminal.  The remaining  entries represent  the distances of the minimal routes from the  terminal  to each point  in'  the  pairs  of aggregates, A; and A;, and  back to  the  terminal. These  closed-loop routes  can  be  readily  determined  from  the distance matrix D if  care  is taken  that no  loop crosses itself.  For  example,  the minimal  route involving aggregates A. and A5 is PoPeP1P10P9Po and not PoPeP1oP1PJ'o .

The  procedure  for  finding  the  combination  of aggregates  which  yields mini› mum mileage is exactly  the  same  as  the  one  used  for  first-stage  aggregations. Table 3 shows the  optimal  solution  obtained.  If  it  were not  for  the  appearance of fractional  values  for some x;; ,  the  second stage  problem  would be  solved.

## 3.6. Treatment  of Fractional  x;;'s

According to  relations  (  5)  fractional  values are  permitted  during  the  compu› tation and no special treatment  is required  unless fractional  values appear  in the

![Image](image_000002_31e3676d77c103772ba920783baba3dcc20cd58f6d8e24b3b3f36a8827de51ba.png)

FIG.  1

## TABLE 3

A3

Ac&gt;

0

A1

17

1/2

A2

54

112

52

92

94

37

1/2

72

102

3!5

A4

84

110

87

0

72

80

102

74

45

0

92

I

84

As

•47

Ag

86

!9

I

112 120  120

112  112  112  112

A~, optimal  solution. A fractional value for any X&gt;J in  the  optimal  solution means that  no decision  has been reached as to whether points P; and P;, or aggregates A, and A;, are to  be linked or not.  Fractional  solutions always involve one or more groups of fractional x;;  , each composed of an odd number of basic entries. Table  3 shows that  the  optimal solution contains three  fractional  basic entries involving the  group of aggregates A1  , A2 and  Aa  . Any  two  of  them  may  be aggregated with each other  leaving the third  to be aggregated with the terminal

28

!54

44

84

86

Ao  . By trial  and  error  it  may  be readily  decided which one of the  three alterna› tives corresponds to minimum mileage. The symbolic diagram shown in Figure 2 may be  useful  for  carrying  out  the  computation most  expeditiously.  In  this diagram A1 , A2 and Aa are placed at  the vertices of a triangle; Ao is placed inside and connected with  the  vertices.  The  distances  corresponding to  the  6 possible aggregates are  then  taken  from  Table  3 and  placed  on  the  corresponding lines in Figure 2. It  is readily  seen that  the aggregates yielding minimum mileage are (A1 A2 ) and  (AoAa).  If the  number  of fractional  entries  is 5, the  symbols Ai are placed  at  the  vertices of a five-sided polygon and Ao is placed inside. The  value of such diagrams,  particularly when  the  number  of  fractional  entries  exceeds three, lies in  the  fact  that the  distance  relations  1nay be  seen at  a  glance.  If there are one or more such odd groups of fractional  values at  the  end of stage 2 or higher, it  is probably  best  to  select arbitrarily one of the  groups, determine the optimum aggregation  for the  group and  then  recompute  the  stage after  the paired A/s (i '¢ O) have  been removed  from the  array.  Thus,  in  the  example

![Image](image_000003_4ad40e0f6afd19ec1def6d3ad5b7532472a910b2cd9a8d1b25098f31b2a3fc6f.png)

shown in  Table  3,  it  is  recommended  that stage  2  be  re-solved under  the  as› sumption that  A1 and  A2 are aggregated,  but  that  A3, A4, A., A,, A1 are free to pair  up  as  they  please. This  leads to  the  array  shown in  Table  4, in  which there is again  an  odd  group  involving  the  pairs  of points A,, Ao,  A1. By  the procedure  illustrated  in Figure 2, one finds that A. must be aggregated with A1, leaving A3  A,, , As to  pair  up  as  they  please. Finally A3  A, , , A, are  resolved by the same method.

It  is possible, of course, to  get fractional  solutions in stage  1. If there  are one or more such odd groups of fractional  values at  the  end of stage  1, each should be resolved separately.  Then  one  may  proceed  to  stage  2.

## 3.7. Comparison of Final, Solution wuh  True Optimum

The final  solution of the  numerical problem discussed in this paper  is (A1A2), (A.A1), (A.As), A 3 · According  to  Figure  1 this  results  in  the  following trip assignments: (P 1 P 2 P 3 P 4 ), (P 7 P1~ 11 P9), (PJ\oPs), Po. According to  Table  3,

the  total  distance  covered on this  trip  is 294 units.  It  is believed that  the  trip assignment  (P1PsPaP,), (P1P12P11P10),  PGPJ'9), ( Pr, with  a  total distance  of 290 units  is actually  the  true  optimum  solution of this  problem. If  so, the  al› gorithm developed in this  paper  has  resulted  in a  "best  solution"  which comes very close  to the true  optimum for the numerical example in Table  1. Experience with  the  method  has  shown that  similar results  may  be  obtained  in  other  nu› merical cases particularly  if the  station  demands  do not  differ too widely. It  is also conjectured that  the difference  between the  distance for the  "best  solution" and that  of the true optimum decreases as the number of station points increases.

## 4.  Multiple Product  Demand

In  the Truck  Dispatching  Problem, as formulated above, it was assumed that a demand for only one product  is specified at  each point.  This restriction  is not necessary.  If  demands qi,k for n different products  are  specified at  each  point P,  (k = 1, 2,  · ·  · n), the  total  demand  for each point  is

---

TABLE  4

![Image](image_000004_bf4d14429ea9f793e4bd6db7ac92c09e9ecf75f884d43d16ea3bf7aadfb664ca.png)

$$Q _ { i } = \tilde { \sum _ { k = 1 } ^ { n } q _ { i, k } } \,.$$

If  one product  can be exchanged for an  equal  quantity (  weight or volume) of any other quantity  as far as the  carrier is concerned, it is clear that  for the  solu› tion  of the  problem only the  total  demand Q. at  any  point P, needs to be con› sidered. In  the  case of liquid  products,  which  are  delivered in  bulk,  the  ideal carrier,  satisfying  the  condition  of  complete  product  interchangeability,  must obviously have  moveable partitions.  In  practice  this  ideal may  be  closely ap› proximated  by  suitable  truck  compartmentation.

## 5.  Multiple Truck  Capacity

It  was  noted above that  the mathematical  problem of minimizing  truck mileage is independent of the number of trucks  assigned to the terminal when all trucks have the  same capacity.  This is no longer the  case when the fleet consists of n, trucks  of capacity C, where i  =  1, 2,  ·  ·  · m. While the  tacit  assumption  was

made above that  unused  capacity  incurs  no  loss, it  is  obvious that  in practice "maximum use"  should be made of the  entire  capacity  of the  fleet. To incorpo› rate unused capacity  into the  model, it  would be necessary to introduce the cost for unused unit  volume and  the cost of unit  Inileage. The problem then  becomes one of minimizing total  cost  rather  than  total  mileage. In  practice  the  cost  of unused unit  volume becomes of minor importance  if one permits  a  certain  per› centage of over- or  underdelivery.  For  this  reason it  may  be  best  to  solve the problem by  assuming  that  all trucks  have  capacity C,,. where C,,. is the  largest available truck  capacity.  The  assignment  of trucks  of different capacity  to  the various trips  must  then  be made  in accordance  with  the  total  demand  for each trip.  No  general  statements concerning  such  truck  assignments  can  be  made since  the availability  of trucks  of different capacity  at  any one time depends not only on the  length  of  the  various  trips  but  also on  random  conditions  such  as the severity of traffic.

## 6.  Other  Formulations of  the  Problem

In  the  problem  as  formulated  above  it  was  assumed  that at  every  station point Pi a demand for certain  products  was specified  and that  this demand must be satisfied by  one  delivery.  By  relaxing  the  condition  that demand  must  be satisfied in  full,  it  may  be  possible to  reduce  total  truck  mileage still  further. Thus, it  may  be  permissible to  underor  overdeliver  up  to  a  fixed percentage based on demand.  Underdelivery  may  permit  the  pairing  of near-lying  station points which could not  be paired if demand is to be satisfied in full. Overdelivery will  reduce unused  truck  capacity.  The  ultimate  in  "underdelivery"  is reached when deliveries to  station  points  are  made  in  quantities  such  that no  station ever runs  dry.  This  formulation  of  the  problem  would  not  only  reduce  total mileage  to an absolute  minimum but  should also considerably reduce the cost of computation since daily computation  of a truck  dispatch  plan becomes unneces› sary. The feasibility  of this  approach  depends,  of course, on the  degree of regu› larity of consumption  at  the  various station  points  and  the  several fixed charges incurred by  several  unloadings  to  satisfy  a  single order.

## Literature Cited

- 1. FLOOD,  ERRILL  M.,  ''The  Traveling-Salesman M Problem", J. Opns. Res.  Soc.  Am.  4, 61-75 (1955).
- 2. DANTZIG, . B.,  R.  FULKERSON ND  J.  JOHNSON, Solution  of a Large-Scale  Traveling› G A " Salesman  Problem", J.  Opns. Res. Soc. Am.  e, 393-410 (1954).
- 3. DANTZIG, .  B.,  "Discrete-Variable G Extremum Problems", J. Opns. Res.  Soc. Am.  5, 266-277 (1957).
- 4. Go.r.i:oaY, .,  ''Essentials R of  an  Algorithm for  Integer Solutions  to  Linear Programs", (communicated  to Bull.  Amer.  Math.  Society in  a  letter from  Princeton, dated  April 23, 1958).
- 5. DANTZIG, .  B.,  "Maximization G of  a  Linear Function of  Variables Subject to  Linear Inequalities", Chapter XXI  in  "Activity Analysis  of  Production and  Allocation", edited  by  T.  C.  Koopmans, Cowles  Commission  Monograph Nr. 13, John Wiley  &amp; Sons, New York,  1951.

Copyright  1959, by INFORMS, all rights reserved.  Copyright of Management Science is the property of INFORMS: Institute for Operations  Research and its content may not be copied  or emailed to multiple sites or posted to a listserv without the copyright holder's express written permission.  However, users may print, download, or email articles for individual use.