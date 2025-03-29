Expert  ystems  ith  pplications, S W A Vol.  ,  p. 5-27, 9 1 2 p 1 19 Printed    he  S A . int U

## Integration  f  Adaptive  Machine Learning  and Knowledgeo Based Systems  for  Routing  and Scheduling pplications A

## NAGESH KADABA

United  arcel rvice esearch d  Development  ivision), bury,  T P Se (R an D Dan C

## KENDALL E. NYGARD, AND PAUL  L.  JUELL

North akota tate iversity, go,  D D S Un Far N

Abstract~  he  combination f ood  mathematical  odels,  knowledge-based ystems,  rtificial ral T o g m s a neu networks, nd adaptive  enetic  earches re  shown to  be  synergistic.  actical  pplications  f his a g s a Pr a o t combination roduces  near-optimal  esults, ich  none  of he ndividual thods  can  produce  on  its p r wh t i me own. We have  developed ROUTE, X a software  ystem  that emonstrates  n integrated amework s d a fr for  this  ynergism,  n  the omain of omputer-aided  ehicle  outing  nd  scheduling  roblems.  The s i d c v r a p purpose  of his  ystem s o ssist searchers  nd  decision  akers  who are pplying  he athematical t s i t a re a m a t m models  to   specific uting  roblem  instance  y  "tuning"  he odels  to he roblem  description. e a ro p b t m t p Th neural  network  modules  store nowledge  of reviously  olved roblems  and their  olutions  hich k p s p s w facilitates   process f rriving  t  solutions   new  problems. he  knowledge-based  ystem tores the o a a to T s s partial  olutions om various  knowledge  sources, ike he  neural  network  and genetic  lgorithm s fr l t a modules, n  the orking emory and  closely pervises e olution  rocess  n  heuristic  thematical i w m su th s p i ma models.  XROUTE provides n experimental,  xploratory amework  that llows any variations, a e fr a m and compares the  alternatives   problems  with  different aracteristics.  e resultant  ystem  is on ch Th s dynamic,  expandable,  nd  adaptive  nd  typically  tperJbrms lternative  thods  in omputer-aided a a ou a me c vehicle  outing. r

## 1.  INTRODUCTION

## 1.1.  The Problem-Solving  Techniques

We  have  applied  he  following  roblem-solving cht p te niques:

- · A knowledge-based ystem s using rames or ymbolic f f s representation  s  shown to  be  a  useful  echanism ha m for  automating omplex decisionmaking.  his  AI c T technique  s  applied o  real-word  roblems  by ini t p corporating asoning  rocesses  hat  ncludes  se  of re p t i u facts and  rules  f humb gained rom  experience. o t f

WE HAVE  DESIGNED and implemented  a system  that uses lternative s  of mploying ethods  of rtificial a way e m a intelligence onjunction th  heuristic  thematical in  c wi ma models  for olving  ehicle  outing  roblems. he ars v r p T tificial  telligence blem-solving chniques  nclude in pro te i knowledge-based ystems, odular  neural etworks, s m n and adaptive  earch echniques  hat tilize th  artis t t u bo ficial  eural etworks  and  genetic  lgorithms  ithin n n a w the ame  system.  e established echanism  to  mans W a  m age  the nteraction ween  these  ery istinct  oblemi bet v d pr solving  echniques. t

It s ell  known that any problems  in  operations i w m research re  NP-complete  and,  therefore, nnot  be a ca solved  to  optimality   realistic  mputer  time. or in co F such  problems,  esearchers  ve  developed any heur ha m ristic  rategies.  major  problem  with  many of hese st A t heuristics   n  inability    deduce  the ind  of trategy isa to k s to  adopt, iven  the  characteristics he  problem  at g of  t hand.  The need  to  deduce  the arameters  o  use  in  a p t heuristic  r   given roblem  at and,  led o  the denfo a p h t i tification  d  use  of ertain  I problem  solving  echa c A t niques.

Requests  or  eprints uld  e ent  o agesh adaba,  nited  arcel f r sho b s t N K U P Service  Research d  Development ivision), 53 enosia  v-( an D 51K A enue,  anbury.  T 06810. D C

- · Neural  networks have  shown  to e  useful  n earning b i l from experience  sing  the  associative mory  cau me pabilities bib, 987). he  traditional roach o (Ar 1 T app t problem-solving amines  a  current  ituation   soex s ini lation.  significant antage f eural etworks s A adv o n n i that he  knowledge  is earned  rom  past xperience. t l f e In  contrast   "pure" nowledge-based  ystems,  he o k s t neural  etwork nd  adaptive  ystems ethodologies n a s m allow uch more  precise  epresentation omplex, m r fc ill-structured  ationships. rel
- · Genetic  algorithms are  search echniques  hich  bet w long o  the generate  nd  test"  I  paradigm.  enetic t " a A G algorithms  GA) (Holland, 975)  base  their  earch ( 1 s for etter  olutions  Darwinian  principles   surb s on of vival f  the  fittest es, here  the  genes  represent o gen w possible  olutions   the  problem, nd  the  fitness s to a is the  objective nction  alue. fu v
- · Heuristic  mathematical models are instruments

which transform ata  into nformation  hich can d i w aid  in he nferencing  chanism.  In  addition, ny t i me ma real-world ituations  n be modeled with  sets f s ca o equations.  hus, good mathematical odels are T m fundamentally portant  ieces f nowledge  in he im p o k t system.

This rticle  scribes  method  of ntegrating se  our a de a i the f very  different  rms  of  "problem-solving yles" to fo st in a  single  ystem. s

## 1.2.  The Problem Domain:  The Vehicle  Routing Problem

The vehicle  outing  roblem  (VRP) is  highly omr p a c binatorial oblem  (NP-Hard)  that as  been  studied pr h extensively    operations searchers  odin,  olden, by re (B G Assad,  &amp;  Ball, 983). n  the  VRP, there s  known 1 I i a collection   top oints  hat  ave  demands  for ervice ofs p t h s and  a  fixed leet  f imited  apacity  ehicles  o  serve f o l c v t the tops.  he  problem  is  o ind  he inimum-distance s T t f t m way  to ssign  he tops  o ehicles d  specify  he rders a t s t v an t o in  which  each  of  the  vehicles  isits s  tops.  ll  the v it s A vehicles  egin  and  end  their  ours t  a  fixed ocation b t a l depot.

The purpose  of  the  XROUTE system  is  to  assist researchers  nd decisionmakers  n  applying athea i m matical  odels  to   specific oblem  instance   "tunm a pr by ing" he  mathematical  odels  to he roblem  descript m t p tion,  nd  adaptively teering"  e athematical  odel a "s th m m as  a  solution olves.  he structural  stem  overview ev T sy of  XROUTE is  discussed  n  section  .  Sections  ,  4, i 2 3 and  5  provide unctional  scription  he nowledgef de oft k based  system  module,  the  modular  neural etworks n system odule, nd  the enetic  lgorithm-based  stem m a g a sy module.  Section  describes  he  results d  compari6 t an sons f he ROUTE o t X system ith  alternative  thods w m in  computer-aided hicle  outing. e  conclusions e ve r Th ar presented  n  section  . i 7

## 2.  SYSTEM OVERVIEW OF XROUTE

The XROUTE system  (Kadaha,  1990)  improves  the ability   heuristic  ocedures  o esign outing  lans, of pr t d r p by  "intelligently"  ing eir  arameters. e  overall sett th p Th system tarts th  problem  data onsisting   top-point s wi c ofs locations,  pot ocation,  mber  of ehicles  nd  their de l nu v a capacities.  e knowledge-based ystem  working  in Th s conjunction  ith   closely  oupled eural  etwork nd w a c n n a genetic  lgorithm  odule,  determines  he  "best"  aa m t p rameters or ontrolling  e  mathematical  odels  in f c th m the  model  base. he knowledge  sources  re he xplicT a t e itly  eclared  ules  n  the  knowledge-base, he  recomd r i t mendation of the modular neural  networks,  the "tuned" arameters  rom  the  genetic  lgorithms, d p f a an the  mathematical  euristic gorithms. nally, ese h al Fi th "tuned" arameters  re  applied o  the  models  which p a t produce  the  solution  oute lans iven  the haracterr p g c istics   the roblem  at  hand. of p

In the design of the knowledge-based  system XROUTE, there re  five mportant  oals: a i g

- 1.  to ave  the ystem's owledge  accessible d  comh s kn an prehensible   both  routing  xperts  nd  new knowlto e a edge  engineers;
- 2.  to  accommodate changes  as  specific  odules  are m being  developed ue  to  the  system  being uilt  nd b i crementally;
- 3.  to rovide nterfaces    integrate  ry istinct  pes p i to ve d ty of  problem  solving  aradigms; p
- 4.  to rovide  acilities   sing  rocedural  nformation p f foru p i expressed n  programming  languages  ike ISP,  C i l L and  Pascal;  nd a
- 5.  to erve s  a  modular nd  expandable  xperimental s a a e exploratory st  ench  that  s apable f apid rote b i c o r p totyping  n  vehicle  outing  nd  scheduling. i r a

The four  basic unctional mponents  of  the  system f co are  shown in  Figure . he user nterface mponent 1 T i co serves s  the  human-computer  interface    the  whole a to system.

The second  component  is  knowledge-based  ona c trol  acility  r he ystem.  he  third  omponent  is he f fo t s T c t model base  which contains  ll he  heuristic thea t ma matical odels  used  to  solve  routing  roblems. he m p T fourth  omponent  consists  he  modular  neural  etc oft n works  and  genetic  lgorithm  ystems hich  are  used a s w in  selecting ned  parameters  or he  heuristic thtu f t ma ematical  odels  in  the odel  base. he  structural  m m T de scription   each  of he  basic unctional mponents of t f co is escribed  n his  ection. gure    provides    detailed d i t s Fi 2 a view  of he  four asic omponents  of  XROUTE t b c that are  shown in  Figure  1.  The functional scription de of each  of he  modules  shown in  Figure   are  described t 2 in  the ollowing  ections. f s

FIGURE 1. The  Unified  Architecture  of XROUTE. The  Knowledge-Based System Working  in Conjunction  with a Closely Coupled  Neural Network  and  Genetic Algorithm System,  Determines  "Intelligent/Tuned"  Parameters for the  Heuristic Mathematical Models.

![Image](image_000000_fdfcea88a9e4d16662e09e6e613d4a0944cb2eb8db6ea3071b68d86666b99b14.png)

FIGURE  2. Overall  Structure f  XROUTE, o the  Computer-Aided Routing System.

![Image](image_000001_a9a57c6a4614c4d4bf664f25ded393cd2b375730cd2202393c92873dbf5b848e.png)

The XROUTE system  was developed o  provide t a framework  for nvestigating  e uning f  new and  exi th t o isting  euristic ocedures  o  the  characteristics  he h pr t of  t problem  instance  t  hand.  It s urrently nning n  a a i c ru o network  of UN  workstations. e ROUTE S Th (Kadaba, 1987) module  provides  a  sophisticated teractive in graphical  nterface r uickly  nd  accurately oviding i fo q a pr input ata, omparing  results,  iting  outes,  nd  prod c ed r a ducing  output eports  nd  plots.  he system lso llows r a T a a interactive  ewing  of  the  performance f  the  knowlvi o edge-based ystem. s

In the  knowledge-based  system,  XKBS,  a blackboard-like  ramework  (Erman,  Hayes-Roth, esser, f L &amp; Reddy,  1980)  is sed  as  a  central  ommunication  meu c dium and a depository  f  information  y  the  various o b independent  knowledge sources.  hus,  at  any given T time,  the  blackboard ramework  has  the  information f that s  utilized   the  knowledge-based ystem  schedi by s uling he  events n  the  routing  lan. t i p

The XMDL module is  a set  of  neural  networks trained ith many  examples of  problem data.  The w trained eural  networks  in XMDL n produces  a recommendation vector.  his  vector,  n  turn,  pdates he T i u t scheduling  lackboard rames. ehicle  routing ethb f V m ods  in he eneralized signment amily se  geographic t g as f u locations lled  eed-points   parameters  hich  drive ca s as w the  partitioning    stops mong the  available hicles of a ve (Fisher   Jaikumar, 981;  Nygard,  Greenberg, olkan, &amp; 1 B &amp; Swenson, 1987).  All  the eural etworks re  trained n n a using  the  Back Propagation  earning  aradigm. l p

The XVRP-NN module is  a modular neural  network system  which selects   promising  initial pua po lation  f eed-points r he  XVRP-GA o s fo t module.  Each of  these ctions  mong  the  modules  is ontrolled  ia a a c v the  ROUTE interface ygard, uell, Kadaba,  1988). (N J &amp; The XVRP-GA module searches or  seed-point  ocaf l tion arameters hat   heuristic signment rocedure p t a as p uses o  produce  high-performance  ssignments  f topt a o s points o  vehicles.  nce a good assignment  is  detert O mined,  the  XTSP-NN neural  network  system  is sed u to  select  good  set f ontrol  ules  or    traveling  lesa o c r f a sa man  selection-insertion  ristic olden  &amp;  Stewart, heu (G 1985) for  each vehicle.  n extensive omputational I c testing, ese  roblem-customized  ontrol  ules  elected th p c r s by  the eural etworks utperformed  very ixed  omn n o e f c bination  f ontrol  ules.  he XTSP-GA o c r T module uses a  genetic  lgorithm,  hat ynamically  hanges he ona t d c t c trol  ules  s  each  stop-point  laced nto he volving r a isp i t e tour. ach of  the  steps  n  each  of hese odules  posts E i t m information  o  the  blackboard  n  XKBS. t i

In summary, at  any given  time,  the  blackboard framework  has  the  information  hat s tilized   the: t i u by

- I.  Knowledge-base  that  schedules he  events n  the t i routing  lan; p
- 2. Genetic  algorithm s  a depository  f  partial  olua o s tions;
- 3. Neural  networks  as  a depository  f  selection  eco r ommendations;
- 4. Knowledge-base to deduce control  information supplied  o  the  genetic  lgorithm; t a
- 5. The graphical  ser nterface. u i

## 3. FUNCTIONAL DESCRIPTION OF  THE KNOWLEDGE-BASED MODULE

The top-level ntrol  tructure  ROUTE co s ofX is esponr sible  or electing propriate  outines  nd supplying f s ap r a current ersions  f  the  problem  description.  cause v o Be of  the  number of  procedures nvolved nd the  comi a plexity  f  the  interactions   he  various omponents o oft c of he  system,  e  choose ot o se  a  simple lgorithmic t w n t u a monitor  to  sequence  through  the  routines.  urther, F there re  many processes  hich  make up  the lanning a w p and scheduling ystem. he knowledge-based ystem s T s has  the  potential   raise  he  level f  performance f to t o o the  nonspecialist.  system  can  also ubstantially The s increase he  performance f  the  specialist    increasing t o by the  comprehensiveness  f  the  analysis, d the  speed o an in  obtaining    quality  olution. is  organization  n a s Th isi contrast  o  conventional  rograms, hich operate n t p w o "expected ata  in  known  formats  using  prespecified d and inflexible  ntrol  tructures."  aterman, 1986) co s (W

The  knowledge representation  sed here fundau mentally  impacts  the  mapping between  the  problem domain and  the nternal del  representation. ause i mo Bec of  the  systematic  nd  simple ay in  which  the  frames a w can store,  nherit, d manipulate ata, t eems  api an d i s propriate  hat hey  are  useful  o  model complex  plant t t ning  tasks ith  numerous submodules.  Typical  reaw soning  services  rovided y frames  that lay  an imp b p portant role in  any  planning system  include inheritance, stantiation,  iteriality,  ognition  f in cr rec o abnormal  situation,  ference  y  analogy,  efault  eain b d r soning, alue  class nd cardinality asoning,  onsisv a re c tency aintenance,  nd  alarms.  nother  novel eature m a A f

provided  y  the rame-based  epresentation    means b f r is a for eusing  ommon  information d  a  means  of onr c an c trolling  e  search erformed y  the  inheritance  oth p b pr cess.

FIGURE  4. The Generic Classes of  the  Vehicle  Routing Knowledge-base:  The  Four  Generic  Classes  of Frames Serve  as Fundamental  BuUding  Blocks of  the  Frame-Based Knowledge System.

![Image](image_000002_54f1fae6c7516d7cc061a41a4b564717c0677aec2813ec73b5f7bd80316272b6.png)

The stop-point  lass efines  he  details  f  each  stop c d t o location  hich  is erved y  the ehicles. is  class  t w s b v Th a any given  instant  rovides he  overall  tatus  f  the p t s o routing  rocess.  his  class as  the  information out p T h ab the top ocation,  s  osition,  e equence,  he ehicle s l it p th s t v it s ssigned  o, nd  the ime  at hich  it s erved. i a t a t w i s

## 3.2.  Definition  he  Frames for  the  Routing  System t

Basically e hrust  ere s  o dentify mall umber th t h i t i a  s n of  generic  ubmodules  that  an  be  represented th  a s c wi frame.  This  constitutes  e  knowledge-based enetic th g classes.  ese  generic  uilding ocks re sed  to efine Th b bl a u d each  submodule  as  an instance  f  a  genetic  lass.  n o c I particular, gle  rames re  used  to  represent ecific sin f a sp instances  f ata e.g., op  location  6), nd  generic o d ( St 5 a classes of  objects  procedures  elated  o  the  perfor-( r t mance  of he ubmodules).  n esigning  frame ystem, t s I d a s two  issues  re ddressed. e  first sue  s efining d a a Th is i d an instantiating    genetic  lasses  o  describe  he hole the c t t w planning  ystem.  he  second ssue  s ow these  rames s T i i h f will  e  organized  nto    hierarchy.  r he ehicle  outb i a Fo t v r ing  and scheduling  ystem, e defined our  genetic s w f classes of  frames o  serve s  the undamental  uilding t a f b blocks f  the  frame-based  nowledge  system.   brief o k A summary of  the  knowledge-base  lasses  s llustrated c i i in  Figure ,  and the  class ierarchy  s  illustrated 4 h i in Figure . 3

3.1.  Generic  Classes f  the  Knowledge-Base o The process-module  lass efines  he  module  used  to c d t perform he pecific  sk.  his lass  as he nformation  "fillers." have  implemented ach  class  s  a  frame, t s ta T c h t i about  its  tate  active   inactive),  urrent  orking s ( or thec w dataset,  he  current  est olution  t  any  given  time, t b s a total  erformance,  nd  the trategy ing sed. o,  at p a s be u S any  given ime,  he rocess-module  an  report  urrent t t p c c information  or he  routing  rocess. f t p are  five  ypes f nformation   the  strategy-  netic  arent  rame  named 'frame'   shown in he igstep  class;  he  name,  the  description   the  strategy-  ure  3.  The frame  named 'frame'  s  the  parent f  all A frame  provides    representation  r n  object,  itua fo a s ation,  r  class  n erms f   set f ttribute  mes  and o i t o a o a na values  for hose  attributes.  frame  consists  f  two t A o parts:  slot  ames"  and  their  espective lue,  alled " n r va c We e a using he  Pedagogic rame Representation nguage t F La (pfl)  Finin    Silverman, 986). isp/pfl de  shown ( &amp; 1 L co in  Figure (a)  illustrates  eneric rame  defined or 6 a g f f 'strategy-step.'    frame  is   child f   frame  called This a o a 'xroute.'  e frame  named 'xroute'   efined  y  a  geTh isd b p f as t F i o frames nd  all  lasses  n he nowledge-base.   shown a c i t k As in  Figure (a) he  ako  slot  f he  frame  specifies at 6 t o t th the arent f ny  strategy-step me  is n  strategy-step p o a fra a class.

There t o i in t of step,  he odules  it  ses o  accomplish  he rocedure, t m u t t p and  the  strategy-step ology maintained  y parent top ( b and  child  ointers).  gure    shows  the rocedures  erp Fi 5 p p formed  by XKBS.  This  diagram  is nly  a  part f he o o t routing  nd  scheduling  rocess,  ut  does  illustrate a p b the i T c d t c istics   vehicles  nd  their  outes. ere  are ive  ypes of a r Th f t i i c th n t v type  of he"  ehicle,  he  aximum  capacity, e  route t v t m th duration, d  the oute  o hich  the ehicle  s ssigned. an r t w v i a

![Image](image_000003_8df831d85c01a499041206091d64fe8f59f43aaa226ffe8bba54cc9dbea99b9f.png)

steps nvolved.  he  vehicle  lass  efines  he haracter-  all he  corresponding  lots  nd daemon procedures. of nformation  n  this  lass; e umber  of he ehicle,  how an  instance    generic  lass  rame s nstantiated. I FIGURE 3. Generic Class Taxonomy of  XKBS. When  any  generic  rame  is nstantiated,  nherits f i it i t s a The pfl  lisp ode  shown in  Figure (b)  demonstrates c 6 ofa c f i i Only  the pecific lues  hich  are eeded o efine  he s va w n t d t slots  f he eneric  lasses  re eclared  ith: alue.  n o t g c a d w v I the  instance  f he trategy-step  ss,  he parentproc' o t s cla t ' slot  s ot efined  ecause t nherits  e efault  alue i n d b i i th d v from the  generic rame. he 'bymodule'  lot f  the f T s o frame actually  ives  control o  the  process-module g t frame  which runs  numerous daemon procedures o t achieve he  result. e 'explain'  ot  elps he  XKBS t Th sl h t to  give xplanations  ny  stage f he outing  rocess e ata o t r p about  why a particular  del was chosen  and  allow mo the  user o uery lternative  deling echniques. is t q a mo t Th feature  enefits  hose sers nfamiliar  ith  the  prob t u u w cedures ecommended by  the  system. r

FIGURE 5. Strategy-Step  Diagram: The Strategy-Step  Class Defines the Various Steps Involved  in  the Routing  and Scheduling Process. It II,;s;~ites    Hierarchy I the of  the 'Strategy-Step'  eneric  Class. G

![Image](image_000004_705bd538f8ba3124716d2a92a95a5c17580dbb4c02274c467e780f3f3bf6b41f.png)

3.3.  Inferencing  echanism M When  the  inferencing chanism is ntegrated to me i in a frame-based  nowledge  representation  heme,  a  spek sc cific  earch ath  is ollowed  y  the ystem.  he  frames p f b s T based  representation  s  the dvantage f aking  art ha a o t p in  the  system's  easoning  ctivities   providing  ur a by a tomatic  nferences   art f ach  assertion  d  retrieval  rithms,  nd  the  mathematical  lgorithms. e blacki asp o e an operation.  hese automatic  inferences  re  accomT a plished  sing nheritance, dinality cking,  nd  atu i car che a taching  procedural nformation  xpressed  in some i e other  anguage  e.g.,  SP,  and  PASCAL).  In ddition l ( LI C a to  the utomatic  nference chanism, he rame-based a i me t f representation  n  be  used o ncode pecific  les.  he ca t e s ru T ability   ttach  rocedures  nd  rule  lasses  o he lots toa p a c t t s of  the  frame  that ehave  like  aemons has  been  used b d to  great dvantage o  control  easoning  n  many sysa t r i tems. e.g., YSSEY). ( OD These  attachments n  be  utica lized  s  sensors,  onitors,  r  alarms. a m o structure  aterman,  1986).  n  XKBS, the lackboard(W I b like  ramework  (Erman  et l.,  980) s sed s   central f a 1 i u a a communication  medium  and a depository  f  inforo mation  by  the arious  ndependent  nowledge  sources. v i k The knowledge  sources eing  the  explicitly  clared b de rules,  he ecommendation f he  modular  neural  ett r o t n works  and  the  tuned  parameters f  the  genetic  lgoo a a a Th board  is ivided  nto  wo  major  components: he ata d i t t d blackboard nd  the  scheduling  lackboard.  he data a b T blackboard s  a site or lobally  ccessible  top  data i f g a s and  partial lutions.  e scheduling  lackboard  s so Th b i a site  or ccessing  ynamically  hanging ontrol  nforf a d c c i mation.  The problem  of  determining  hich  strategy w step s o  be  executed ext nd  by  which  process  odi t n a m ule,  s tored  n he cheduling ackboard. e  control i s i t s bl Th information  vent-driven d  depends n  the urrent ise an o c state  f he ata lackboard, e cheduling ackboard, o t d b th s bl and  the  knowledge-base  ules.  nce a  strategy-step r O is selected  or rocessing, e  process xecutes  he  step, f p th e t causing ew events o  be placed  on the  blackboard. n t The result f  executing  strategy-step  oduces  a o a pr change  on the  data  blackboard nd the  scheduling a blackboard.

In  the  rest f  this ection,  give  an example  of o s we how the  inference  echanism is  started. e whole m Th routing  rocedure  s istributed  th rames rganized p i d wi f o in  a  hierarchy. ames  higher  p  in he ierarchy rFr u t h co respond  to  more general  oncepts  hile hose urther c w t f down correspond o  more specialized ncepts.  he t co T

The  goal f ur  inference chanism  is o inimize o o me t m the  total  istance  raveled  y all he  vehicles.  KBS d t b t X was designed o  assist  n  developing  ethods  for ept i m r resenting  nd applying iverse  ources f  knowledge a d s o to routing roblems.  The  knowledge-based  system p must handle  diverse  ype  of  knowledge, hat s,  pet t i s cialized bsystems  ach  designed  o  analyze    particsu e t a ular  type  of  knowledge, hen  communicate  relevant t findings  o  other  ubsystems.  hese  requirements gt s T su gest  the  need for  a blackboard rchitecture  ere a wh knowledge  sources  ommunicate hrough    central ta c t a da

6(a)

![Image](image_000005_a9fc13b187a8c619605e5509182a487d3bf6c42c68849d8d8a48da13b9cb16bb.png)

60)

## Instance rames f

(fdefineq  del-selection  ategy-step mo str

(procname  (:value  odel-selection")) "m

(description value  his  lrategy-step  okes (: "T s inv

the process-module dl xm for

recommendations  about the est  odel o b m t

be  used or    particular f a damseL"))

(childproe (:value "run-model"))

Coymodule (explain

(:value "xmdl-process-module"))

(:value get a

"Trying  to benchmark on the

performance  sing  tatic  uting dels"))) u s ro mo

stop-point  ames re nstantiated ng he atafile  d fr a i usi t d an a  few  queries  o he ser.  he  frame-based  ystem ffers t t u T s o a  natural  ay of sking uestions  nd  acquiring  nforw a q a i mation  using, f-needed: cets.  he process-module i fa T frame  and  the trategy-step me  are  instantiated s fra depending  on the  information  cquired rom  the  user. a f The vehicle  rames re  instantiated h  the  number f a wit of  vehicles  sed  in  the  routing  lan. he procedure u p T attachments  nstantiate  ny  more process-module i ma and  strategy-step mes uring he  planning  rocess. fra d t p As vehicle  outes  re eveloped,  he reviously stanr a d t p in tiated  top-point d  vehicle  rame  have  their  lot  als an f s v ues  periodically  anged. hus,  at ny  given oment ch T a m the  result  f  the  routing  rocess an  be reviewed y o p c b accessing  he lots  f he top-point d  vehicle  rames. t s o t s an f

As shown in  Figure (a) he rame  'task'  tablishes 7 t f es the lass  tructures  ich, n urn,  nstantiates   roc s wh i t i thep cess-module, trategy-step, op-point,  nd vehicle s st a frames.  he facet  acts  nitiates uire-facts,   roT f i acq thep cedural ttachment  f  the  'task"  rame  which  instana o f tiates l he  stop-point d  vehicle rames.  he facet al t an f T 'goal' itiates  rocedural  ttachment  hat erforms in a  p a t p goal-analysis.  e goal-analysis  ocedure, n  turn, Th pr i initiates   required  trategy-step me. he  strategythe s fra T step rame  has  its  wn taxonomy  which  the nferencing f o i mechanism uses  to  its dvantage.  he strategy-step a T frames  instantiate  e  process-module  rames, hich h f w keep  track  f he rocess  tatus. us,  by  initiating o t p s Th the following  wo commands  shown in  Figure  7(b),  he t t task s tarted. i s

## 4. FUNCTIONAL DESCRIPTION OF  THE NEURAL NETWORK MODULE

Neural  nets re  extremely owerful ools or  solving a p t f selection oblems  (Juell, gard,  &amp;  Kadaba, 1989). pr Ny In  particular, y re  one  of ew  tools  iable  hen we the a f v w do  not ave  prior  nderstanding  he rocess f aph u oft p o m ping  from  the  input ata o  the  correct  election td t s ca egory.  s  shown in  Figure   the hree  odules  XMDL, A 2 t m XTSP-NN,  and XVRP-NN make up the  neural etn work module.  The modular  neural  network  system XMDL, is  used  for electing  model that its  n ins a f a stance f  a  problem. he XVRP-NN o T is  used  to  store seed-point  ocations  or  a specific  roblem  data  del f p scription d  to  inject ood" eed-point cations to an "g s lo in the  initial pulation  f  the  genetic  lgorithm.  he po o a T XTSP-NN is  used  to  select  he  control  ules or he t r f t TSP selection-insertion  istic.  ch  of hese eural heur Ea t n networks re  previously ained.   this  ection  e dea tr In s w

![Image](image_000006_de3aa93b352f4cb44864d05baa54c6da7298111812566123d0857bb4adfe81bf.png)

FIGURE 7. Example Frame  Used  for  Inferancing.  The pfl  lisp Code  n (a)  Illustrates  n Inference Frame a Which  Initiates  he t Process  of Routing  and  Scheduling. The  Task  is  Started by issuing  the  Two  Commands Shown in  (b),

![Image](image_000007_9d46b42e13687ef8d6726359222d807c411e0d20683ef903e07e31462368be49.png)

scribe ach  of hese odular  neural etwork  systems. e t m n As  explained  n ection  .4,  he cheduling ackboard i s 3 t s bl keeps rack  f he trategy-step e  executed  ext nd t o t s to  b n a by  which  process-module. e effects  f ach  of hese Th o e t neural etwork  modules  described  n  this  ection  re n i s a seen  either  n  the ata omponent  of  the  knowledgei d c based  blackboard r  the  initial  rameters  or he eo pa f t g netic lgorithm.  hatever  the  recommendations f a W o each  of hese odular  neural etworks,  he nd  result t m n t e will e the  update  of  the  data lackboard hrough b b t a heuristic thematical  rocedure. ma p

## 4.1.  Overall trategy  f MDL S o X

feature  xtraction twork. his  network  compresses e ne T the  data o  a  small umber of eal-valued tput igt n r ou s nals. his  vector ay or  may not  have  a  meaningful T m interpretation  he ontext  f he eal-world  roblem, in  t c o t r p but  it  ondenses he elevant formation to    manc t r in in a ageable  ize or lassification  he ubsequent  eural s f c by  t s n network. he  vectors  f ompressed ata re assed s T o c d a p a input o  a classification  ral et, llustrated  the t neu n i on right-hand  ide f igure .  The mission f he  classis o F 8 o t fication t  is o  determine hich  of  several  eneralne t w g ization  ets s  likely  o  be capable f  correctly ten i t o ca gorizing  n  input  roblem. he  output  re eal-valued, a p T a r but  we interpret e  largest  ntry n  magnitude  as  a th e i boolean  that  hooses   specific neralization . c a ge net

In  Figure   we illustrate   eneral  chema  of MDL 8 theg s X (Nygard,  uell, Kadaba,  1989) or ategorizing  mJ &amp; f c co plex  problems. he first sk s o  encode  the riginal  nets,  ne  for ach  problem  class  sed  in  training.  e T a i t o input ata.  he  code  must  reflect aracteristics  he d T ch of t problem  that re  relevant  n  the ltimate  election a i u s of the  category. eally,  e ode  would  naturally  ovide Id th c pr a  means of artitioning   problems  used  in  training  encoded  problem  as  input.  or  each  network,  raining p the the  networks nto lasses. ese  classes  re  much like i c Th a the quivalence asses  sed  in  formal lgebra,  ut eed e cl u a b n not  be  defined  ith  the ame mathematical  igor. w s r Finally, e  generalization s re  used  to  actually th net a categorize  ach  problem.  There  is  a family f  these o o e u Th specific neralization  hich  is pplied  o  an  input ge netw a t problem  is etermined  y  the lassification  .  owd b c net H ever,  nce  chosen,  he eneralization  ses he ntire o t g netu t e F t information  s   vector f eal umbers  that  ives he i a o r n g t relative nkings,  s  measured  by fitness  riteria  ra a c ap propriate  o  the pplication. t a

Essentially,  ese lasses  hould  offer,  or raining th c s f t purposes,  epresentatives  roblem  variations  at  he r of p th t neural etwork  system  will ltimately    asked o  catn u be t egorize.  here  is ne  generalization  twork  for ach T o ne e of  the lasses. is  encoded  data s sed  as  input o  a c Th i u t

Once all  f he etworks ave  been  trained, DL o t n h XM serves s  a  delivery stem  for roblem  instances at a sy p th the etwork as  not xposed o n raining.    he lasses n w e t i t If t c used  in  training e epresentative he roblems  we ar r oft p wish  to ategorize,  e ncoded  values,  eature tracc th e f x

FIGURE 8. The Overall  Architecture  of  XMDL.

![Image](image_000008_0916e99313ef8b2acc1f004b86c862e89cbdb0a541737d907cd7ec37ec734e19.png)

tion et, nd classification   should ecommended n a net r a  generalization work hat  s   reasonable  atch  to net t i a m the  input roblem. he expectation  hat  he  partic-  nates f  the  stop  locations.  is  data s reprocessed p T ist t ular eneralization  ts ach  operate n  a  sufficiently  to ake it  nvariant   caling  nd  rotation ukushima g ne e i restricted  main  to ffer  igh  accuracy  n ategorizing  &amp;  Miyaki,  1982), roducing  30-unit roblem  dedo o h i c problems  that esemble  those  used  in  training. is r Th recommendation vector, hich indicates  he best w t mathematical  odel  to e  used  is icked p  by  XKBS, m b p u in  turn,  nvokes  he ecommended model  and  updates i t r the  data lackboard. b 4.2.  Overview of TSP-NN X A significant  oblem  associated th  application pr wi of the  Back Propagation  earning  aradigm  (Rumelhart, l p Hinton,   Williams,  986) or attern  lassification  on-line  ecall  y  the  XKBS &amp; 1 f p c s the  lack f  high  accuracy n  generalization  en the o i wh domain  is arge.  n  this  ection,  describe    multiple l I s we a neural  network system XTSP-NN (Kadaba et  al., 1990), hich  uses wo  self-organizing  ral  etworks w t neu n that ork as  teaching  ata  filters  eature tractors),  files.  ese  two files e  then  used  to  train  he enerw d (f x producing nformation  hat  s sed  to  train    generali t i u a ization  eural etwork  (Myers,  Kuczewski,  &amp; Crawn n ford, 987).  ll  the eural etworks re  trained  sing 1 A n n a u the  Back Propagation earning  aradigm.  The techl p nique  is pplied  o  the election  ontrol  ules  or a t s ofc r f a Traveling  Salesman Problem (TSP) heuristic. e Th overall  rchitecture    the  complete ystem  is hown a of s s in  Figure . 9 XTSP-NN basically  nsists  hree  tages,  ach  of co oft s e which  is escribed  n ore  detail  ollowing ctions.  training.    Figure 0, e illustrate  information ow d i m inf se The first age reates  nd  encodes he omain  specific st c a t d data. he problem  data  consists  f  cartesian ordiT o co o Th i p m i tos a (F p a p scriptor ctor.  his  encoded  data s sed  as  input o ve T i u t the  stop-data ature  xtraction  DFE) network. he fe (S T performance ata  consists  f  36 values f  distances. d o o These  distances present e otal  ileage f he our, re h t m o t t generated  y  the SP heuristic  r    given ombination b T fo a c of he  rules. e  36  performance  alues re ormalized t Th v a n between  0.0  and 1.0  in  the  output ncoding odule e m and is  used  as  a  teaching  utput n  the  performance o i feature  xtraction FE)  network. hese  are  the  pere (P T formance escriptors at  he ystem ategorizes ring d th t s c du r b system. he second  stage T extracts e  features om  the roblem  descriptors  d th fr p an performance escriptors  ich  is hen  fed  to  the end wh t g eralization  twork  GN  as  training ta. he SDFE ne da T vectors  nd  the FE vectors  re tored  n wo  separate a P a s i t Th ar t g alizing  etwork. hus,  we have  3  off-line  ained  etn T tr n works  (SDFE, PFE, and  GN) which  we subsequently use  for n-line  ecall. e third  tage s n-line  eno r Th s i o g eralization, ulting    the ule ecommendation ecres in r r v tor. he reconstruction  twork  is  shown in  dotted T ne lines  ecause t s sed  only  during he  on-line  ecall b i i u t r phase, nd  no  learning curs  ere.  he  trained ights a oc h T we are  copied rom  the FE network. he reconstruction f P T network  is he  bottom  half  f he  PFE network  after t o t In 1 w an fl

FIGURE 9. The Overall  Architecture  of  XTSP-NN.

![Image](image_000009_47a74b7e21ea78fac1910322931e3422ad5145981c4a479fc881acddfa1f6e3c.png)

36  traits

FIGURE 10. The  Information  Flow During the On-Line Recall Phase  of  the  Trained XTSP-NN.

![Image](image_000010_74eedbd9f59787ebec2180d3880302452d4d468592d37338529749dae8ced243.png)

diagram  during he  on-line  ecall  hase  once  all f he t r p o t networks  have  been  trained. module,  which  produces   30-unit  roblem  descriptor  the  neural etwork. he degree f  similarity   eterand the  other  half f  the  population ith random o w strings. so,  a matcher  first  hecks  if he  descriptors Al c t of  the  incoming  task re  similar  o  the  training  et f a t s o n T o isd mined  by  finding  he otal  um of quared rrors  f he t t s s e o t incoming  task  descriptor d the  set f  training  atan o p terns.  f  his  easure  is elatively  gh, he VRP-NN I t m r hi t X system  detects  o match,  and the  GA  population  s n i initialized  ly  with  random strings. reover, ue  to on Mo d the  parallel, lti-modal  nature  of  the  GA,  even if mu "'bad" nitial arting  oints n  the  search  space  are i st p i injected  nto he  population,  hey  will ie ut  in  subi t t d o sequent enerations. g

## 5. FUNCTIONAL DESCRIPTION OF  THE GENETIC ALGORITHM MODULE

4.3.  Overall  Strategy f o XVRP-NN The transfer  f  previous xperience s  of  significant  Algorithm  maintains    population  f hese  ontrol  aThe  Genetic  Algorithm exploits he accumulating t knowledge of  the  VRP solution  rocess eing  conp b trolled oldberg, 989). ach  point n  the ontrol  a(G 1 E i c p rameter  space  is  genetic  tring.  his  string  s eprea s T i r sented  in  binary.  ach string  as  a field  llocated r E h a fo the  performance unction f Fe, which  is eturned  y  the r b evaluation  unction.  or  each  generation, e  Genetic f F th a o t c p rameter  strings. ch individual  opulation ember Ea p m is valuated  s  a  set f ontrol  arameters  or he RP e a o c p f t V process,  nd the  associated  erformance  measure is a p saved. inally,  sing election obabilities se onF u s pr the c trol  arameter trings  ndergo  reproduction  ia rossp s u v c over  and mutation-genetic erators. e "best" aop Th p rameter et s elected  fter    fixed umber of terations s i s a a n i and supplied  to the mathematical  heuristic  odel m which,  in  turn, pdates he  frames  in  the  data  blacku t board.

The testing  ata  is resented  o  the  data  encoding d p t a p vector.  his  problem  descriptor   xposed o he DFE T ise t t S network  which,  in  turn,  roduces he  four-unit ature p t fe vector f  stop ata. his  feature  ector f  stop ata  is o d T v o d then  exposed  to  the  trained eneralization twork. g ne The generalizing twork, n recall, oduces he  fourne o pr t unit  feature ecommendation vector, hich is  prer w sented o  the  reconstruction  twork. he original  it ne T d mension  of  36  units n  the erformance ector  s eni p v i g eralized  y  the  reconstruction  twork. his  vector  s b ne T i decoded  by  using   high  value lose  o  1.0 o epresent a c t t r the  best election-insertion  trol  ule ombination s con r c for he  given  instance  f he est  top ata. inally, e t o t t s d F th raw stop  data  and the  best  rule ombination  is  prec sented  to  the  TSP  heuristic,  ich,  in  turn, pdates wh u the  frames  in  the  ata lackboard. d b

In  this  ection,  address  ow XVRP-NN s we h accumulates and  preserves owledge  gained rom  past xperiments. kn f e o e i benefit  n  acquiring  ask-dependent pertise cessary i t ex ne to  solve ore  complex  real-world oblems Carbonell m pr ( 1986).  The neural etwork  system  XVRP-NN n acts s a a pattern ssociator, tching  the  descriptors   the a ma of incoming  problems  with  that f  the  previously  olved o s problems.  The outcome of  this atching  provides m a promising  population  or he  GA  to  start  ith. hus, f t w T the  search pace  is ubstantially  uned  and the  coms s pr putational fort  educed Hayong~ 1985). he neural ef isr ( T networks  store arameters f  previously  olved robp o s p lems.  The parameters  re eed-points nerated  y  the a s ge b GA.  These seed-points  re  recorded n  the  course f a i o experimentations th  various roblems  and various wi p environments f  the  GA. o

## 5.1.  Overview of  XVRP-GA

One of  the ain objectivesin  itializing  A  popm in a  G ulation is avoiding premature  convergence. In XROUTE, this  s ccomplished y  initializing f  he i a b hal t population ith  the  recommendation of  XVRP-NN w

In the  parameter  optimization  ask  we find  a set  of t parameters,  nder  the  guidance  of  a  evaluation  uncu f tion  (Nygard,  Juell,    Kadaba, 1989).  GAs  use  bit &amp; strings  s  chromosonal  encodings f arameters  f he a o p o t problems  that re eing olved.  n  XVRP-GA a b s I the  con-

troi  arameters  re he  seed-point  ocations. e seedp a t l Th points re  models  of  the  direction  nd distance  rom a a f the  depot  that ach  vehicle  ravels.   used  binary it e t We b strings  o  encode  the  seed-points. e control  aramt Th p eters Sxl, yl, x2  ....  SxN,  S~,N S S represent  he  N  seedt points.  he parameters re  constrained  o  an interval T a t of  the  form  ai  &lt; SN &lt; bi where ai  is  and b~  is  1023. 0 Each seed-point  s dentified    an  x-coordinate d a i i by an y-coordinate  hich is  represented  y a 10-bit inary w b b string  o  represent  he  numbers 0 to 1023 inclusive.  is  set f  vehicles   and a set f  stops .  Each vehicle t t The delivery  ocations  re  scaled o  a  1024 ×  1024 l a t grid. he length f  the T o GA string  s i tering  f  the  stop oints s omplete. ow, the  CCAO o p i c N TSP heuristic olden  &amp;  Stewart. 985)  is  used  to  se(G 1 quence  the  stop oints.  he CCAO p T heuristic es  a  set us of  control  ules  e.g.. le  for election d rule  for r ( ru 2 s an 4 insertion).    an alternative  CCAO, As to the trained XTSP-NN  is onsulted  or ecommendation  about  the c f r control ules o  select  hich depends  on the  characr t w teristics  the ata n  hand.  The FAA  algorithm  hecks of d i c for he  capacity  onstraint  ach  of he ehicles. ere t c ofe t v Th a o K o J k in  set  has  an  available pacity K ca bk, and  the  assignment ofa  stopj  n  set to  vehicle    consumes i J k rkj units of  this apacity.  cost  coefficient is  a measure  of c A Ckj the  desirability    assigning  top   to  vehicle  . of s j k

The following  teps re  used  to  assign he  stops o s a t t a vehicle  n  the  FAA i assignment  algorithm:

$$L _ { G A } = \sum _ { j = 1 } ^ { N } L _ { j }$$

where  Lj  is he  length f  one  parameter tring.  ext o s An ample string  nd the  decoded  parameters re  shown a a below:

- · Step l:  Choose any seed-point  s the  active eeda s point.

| String:            | 0000101100   | 1010110010   | 1000110110   | 1110010010   | 0111110010   | 0010101100   |
|--------------------|--------------|--------------|--------------|--------------|--------------|--------------|
| Parameter:         | Sxl          | S.l.l        | Sx2          | S~.2         | Sx3          | sy3          |
| Decoded value:  55 |              | 803          | 987          | 739          | 348          | 200          |

This  example  string  epresents ree eed-points,  th r th s wi a  x-  and  y-coordinate r ach  seed-point. fo e

We  use  a  "cluster rst ute econd"  heuristic chfi ro s te nique  as  a  VRP  solver  n he valuation nction. ere i t e fu Th are wo  fundamental ecisions  hat ave  to  made before t d t h using  this euristic. rst,  e  need to  determine  the h Fi w clusters   some method.  Second, e need  to  sequence by w the  stop  locations  n  a cost-effective . The steps i way shown below  indicates e  method of  using he  GA.  to th t tune  the  parameters or  the  heuristic  athematical f m models  in  the  XVRP-GA module.

- · Stage  1: ccept  the eed-points commended by  the A s re Genetic  Algorithm.
- · Stage  2:  Use FAA  or  GAA method shown below  to determine he  clusters. t
- · Stage  3:  Use a TSP  heuristic   sequence  the  stop to points  in  e/mh cluster, d calculate  he  total  our an t t length or ach  cluster. f e
- · Stage  4:  Return  the  total  our ength o  the  Genetic t l t Algorithm.
- · Stage  5:  Use  the eed-points   model  which, n  turn, s on i updates  the  frames  in  the  data  blackboard.

The Fast  Assignment  Approach (FAA) is  a new algorithm  we developed hat  s elatively  st  nd  well uited t i r fa a s for  the  genetic  earch.  t ses  a simple  heuristic   do s I u to the  clustering. re  all he  seed-points e  actively  n He t ar i a  contest  or etting  ach  stop  point ssignment.  ach f g e a E stop  point    has  a  weighted istance  anking. he stop j d r T point ith  the  minimum  distance  o  any  seed-point  s w t i selected  nd assigned o  that eed-point  ntil  he  clusa t s u t

- · Step  2:  Assign  the top ointj  ith  the mallest  osts p w s c coefficient tO  the  active  eed-point Ckj s Sk.

$$c _ { k j } = D I S T A N C E \, ( s t o p \, j, \, s e e d \, k ).$$

- · Step  3:  If k has  reached  its ull  apacity,  top  any S f c s further  ssignments o  Sk. a t
- · Step  4:  Repeat  step   and  step   until  ll top oints 2 3 a s p are  assigned o  one  of  the  seed-points t Sk.

Clusters roduced in the  clustering  tep  are  fundap s mentally ependent  on  the  locations  he  seed-points. d oft The role  GA  is  to  use  the  fitness  alue  to  search or v f seed-points  hich the  FAA  can  use  to  produce  an asw signment.

## 5.2.  Overview of  XTSP-GA

In  XTSP-GA,  the  control  arameters  re  the  rules or p a f the  selection-insertion  ristic  Golden &amp;  Stewart, heu ( 1985). hese  control  ules  elect  stop,  nd  then nsert T r s a a i it nto he  evolving our. TSP-NN i t t X is  used  to  select a good set  of  control ules or  the  Traveling alesman r f S heuristic r  each  vehicle.  he set f  control  ule ecfo T o r r ommended  by  the  XTSP-NN system  remains  static as each  stop-point  laced nto he  tour.  he XTSP-GA isp i t T module  uses a genetic  algorithm  that  dynamically changes  the  control  ules s  each  stop-point   placed r a is into he  evolving  our.  here  are ix election les  nd t t T s s ru a six nsertion les  vailable.   used  a  three-bit  nary i ru a We bi

![Image](image_000011_e91027a280e7147ec1fd42c627f882616b90d14042aeb03e84afe4c1d406d880.png)

where  Lj  is he  length  f ne  parameter  tring .g., t o o s (e 3 bits).  he tour  produced  by the  selection-insertion T heuristic  e  fundamentally pendent n  the ontrol ar de o c rule t  every tep f he  heuristic. a s o t

## 6. RESULTS AND COMPARISONS

To evaluate  he ffectiveness he  XROUTE t e oft system, performance s  based  on the  following  easures:  (I) i m the inal  uality  he olution oduced otal  istance f q oft s pr t d traveled  y all he  vehicles,  nd (2) he  speed-up xb t a t e perienced  y  the ractitioner ng   knowledge-based b p usi a system.

Although  we do  not  have  any  valid  tatistical  ls ana ysis  f he econd easure,  e  estimate  hat  bout 0% o t s m w t a 8 of  the ime  is aved  on non-OR aspects  f he  vehicle t s o t routing  ork.  The  non-OR time s pent n hree  ays: w i s i t w (1)  keeping  log-books f  the  project, )  coding  and o (2 invoking  pecial  rograms o eformat  ata or se  with s p t r d f u alternative  gorithms,  nd (3)  keeping  track f  the al a o partial sults oduced  at  different  ages. re pr st string  o  represent  ach  control  ule.  here  are  two t e r T control  ules  sed  for ach top-point:  e  for election  problems  are  fully  ense  with  100 stop-points, er u e s on s and  one  for nsertion.   represent e omplete roi To h c p cess,  e need  a w GA string:

To  evaluate olution  uality,  5 problems  were s q 2 solved ith  each  of our ifferent  P  solvers  s  well w f d VR a as  XROUTE. A random problem  generator  as used w in  order o  test ROUTE t X (Nelson,  1983). he test T d 4 v hicles  ith   utilization  tor  5%,  and  are enerated w a fac of9 g in  a square, 023  miles n each  side. he problems 1 o T were  run  on  a  network  f UN  3  workstations. o S

$$L _ { G A } = \sum _ { j = 1 } ^ { \cdot \cdot } 2 L _ { j }$$

The four lternative  gorithms  re: a al a · Clarke-Wright,  venerable  euristic gorithm  aa h al c

FIGURE 12. Performance  of XROUTE in  Comparison With 4 Altemstive Algorithms. The  Objective is  to Minimize the Distance Traveled.  XROUTE Consistently  Performs  Better  Than  all he  Other Routing Models  Used  for  Benchmark. t

![Image](image_000012_8a1af43670d12ef7179edaa1e4fd5a403dcf17fa877c4715ca8ac889cfb4a0ba.png)

The Values Shown Are the Actual Performance of  the  Different lgorithms in  Miles.  In  All  Cases  XROUTE A Produced  the Best Solution.  The "%  of  Min" Column Indicates  how  XROUTE Performed in  Comparison With the Minimum of  the Four Altamate Algorithms. The "%  of  MAX" Column Indicates  the How XROUTE Performed  in  Comparison to  the Maximum of  the Four Alternate  Algorithms.

| Problems   |   CLRK |   FISH1 |   FISH22 |   NYWK |   XROUTE | %  of  Max   | %  of  Min   |
|------------|--------|---------|----------|--------|----------|--------------|--------------|
| p1.1       |  15164 |   14526 |    14526 |  14715 |    13956 | 7.966        | 3.924        |
| pl.2       |  16047 |   15137 |    15137 |  15041 |    14625 | 8.861        | 2.766        |
| pl.3       |  15207 |   14345 |    14339 |  14392 |    14068 | 7.490        | 1,890        |
| pl.4       |  15879 |   15038 |    15038 |  15097 |    14788 | 6.871        | 1.662        |
| pl.5       |  15612 |   14648 |    14648 |  15057 |    14564 | 6.713        | 0.573        |
| pl.6       |  15851 |   14462 |    14636 |  14318 |    13796 | 12.964       | 3.646        |
| pl.7       |  15415 |   14525 |    14587 |  15312 |    14369 | 6.786        | 1.074        |
| pl.8       |  15544 |   14849 |    14849 |  14977 |    14484 | 6.819        | 2,458        |
| pl.9       |  15448 |   14223 |    14223 |  14274 |    13482 | 12.727       | 5.210        |
| p1.10      |  15792 |   15615 |    15615 |  15420 |    14818 | 6.168        | 3.904        |
| p1.11      |  15585 |   14556 |    14556 |  14599 |    14157 | 9.163        | 2.741        |
| p1.12      |  15647 |   15226 |    15226 |  15289 |    14762 | 5.656        | 3,047        |
| p1.13      |  15757 |   15012 |    15012 |  14716 |    14591 | 7,400        | 0.849        |
| p1.14      |  14357 |   13861 |    13871 |  14438 |    13653 | 5.437        | 1.501        |
| p1.15      |  15507 |   14877 |    14877 |  14456 |    14201 | 8,422        | 1.764        |
| p1,16      |  15349 |   15322 |    15322 |  15011 |    14635 | 4.652        | 2.505        |
| p1.17      |  15555 |   15298 |    15298 |  15205 |    14558 | 6.410        | 4,255        |
| )1.18      |  15586 |   14611 |    14611 |  14373 |    14190 | 8.957        | 1.273        |
| )1.19      |  15936 |   14873 |    14919 |  15171 |    14547 | 8.716        | 2.192        |
| al.20      |  14914 |   14101 |    14261 |  14505 |    13583 | 8.925        | 3.673        |
| al.21      |  14856 |   14512 |    14512 |  14462 |    14098 | 5,102        | 2.517        |
| al.22      |  16468 |   15419 |    15419 |  15346 |    14929 | 9.345        | 2.717        |
| al.23      |  15544 |   14849 |    14649 |  14977 |    14509 | 6,659        | 2.290        |
| al.24      |  15280 |   14512 |    14512 |  14184 |    14015 | 8,279        | 1,191        |
| al.25      |  15442 |   14423 |    14423 |  14431 |    14032 | 9,131        | 2.711        |

pable  of roducing  ast  olutions  arge-scale  obp f s tol pr lems  with ultiple  ehicles  nd  an  objective  inm v a ofm imizing  otal  istance larke   Wright,  1964); t d (C &amp;

- · FISHER l,   heuristic  r he  multiple  ehicle  roba fo t v p lem that ses  a generalized signment odel and u as m produces  high-quality  lutions   small- nd meso to a dium-size  roblems  (Fisher   Jaikumar, 981); p &amp; 1
- · FISHER2, a  heuristic  r he  multiple-vehicle  bfo t pro lem  that  uses a  generalized  ssignment  model a and uses  a different  ed  setting  trategy  Nelson, se s ( i 83); nd 9 a
- · NYGARD-WALKER, a heuristic at ses  spaceth u filling rves  and Lagrangian elaxation   obtain cu r to solutions  o  very  large-scale ltiple-vehicle bt mu pro lems  (Nygard  &amp; Walker,  1988).

XROUTE system. he values hown in  the  table  re T s a the  total  iles raveled ing our ehicles. rther, m t us f v Fu as shown in  Figure  12,  each  model behaves  differently with  each  dataset, d XROUTE an performs etter  n b i each  case. his  is  due to  the  adaptive apability T c of XROUTE. Table  2 contains  escriptive  atistics d st derived rom  the alues hown  in able  1. he XROUTE f v s T T system, as  the  lowest  average iles  traveled  or ll h m f a the  25 problem  sets,  s  indicated  the  first w in a by ro Table  2.  The relatively  all tandard  eviation  ndism s d i cates hat he  solutions  btained hrough  XROUTE t t o t are  consistent d  reliable. an

A summary of he omputational  esults e hown t c r ar s in  Table  I. t llustrates   performance easure  of I i the m the  various  odels  used  for enchmark  testing  he m b oft

In  all ases,  ROUTE c X produces he  best olution. t s Percentage  mprovement  over he est f he  other  li t b o t a gorithms  anged s  high s  5.21%.  The  results e ery r a a ar v striking,  pecially  nce he our lternative orithms es si t f a alg represent  he  state-of-the-art  many years f  ret from o search y  operations searchers. b re

TABLE 2 The Values Indicated  Shown Here are Statistics  alculated From  the Values Shown C in  Table 1

|         |   CLRK |   FISHER1 |   FISHER2 |   NYWK |   XROUTE |
|---------|--------|-----------|-----------|--------|----------|
| Mean    |  15509 |     14752 |     14770 |  14790 |    14296 |
| S/D     |    410 |       434 |       420 |    391 |      406 |
| Vanance | 176903 |    188602 |    176690 | 153396 |   165063 |

Abbreviations: CLRK: Clarke-Wright  lgorithm,  ISH1:  Fisher1 lgorithm, SH2:Fisher2 lgorithm, A F A FI A and NYWK:  Nygard-Walker  Algorithm.

## 7. CONCLUSIONS

Development and experimentation  ith  the  XROUTE w system  leads o  four  primary  conclusions. t

- · First,  eural etworks  and genetic lgorithms  re  efn n a a fective  ools or ssisting d controlling thematt f a an ma ical lgorithms hrough  parameter  setting.  n  all f a t I o our  experiments,  he  XROUTE t system  improved  the ability  f  the  mathematical  algorithms o  produce o t quality olutions.  onsidering he  fact hat  for  cens C t t turies, eople have  been  devoted to developing p mathematical  algorithms, his  uniformly  superior t performance  is  especially  emarkable. r
- · Second, the modular architecture  nd blackboard a communication medium  of  the XROUTE system demonstrates hat  a highly  flexible  nd expandable t a system  can be built n  this pplication  rea. his  is i a a T of critical  mportance  in the vehicle  routing  and i scheduling  domain, because  new  methods and refinements  of  old  methods are  constantly  eing  deb veloped.
- · Third, we  conclude  that  the system we  describe greatly  educes he  time  required y  the  practitioner r t b to  develop  quality  olutions  o  vehicle outing robs t r p lems.
- · Finally,  e conclude  that  a multiparadigm  system w can behave synergistically.

The beneficial  ggregate ffect  esults  rom careful a e r f analysis f  important  features f  the  problems  to  be o o solved  and the  steps equired o  solve hem.  The conr t t stituents  ere carefully  atched with  the  capabilities w m of  the  available  aradigms, n  an effort  o  ensure  that p i t the  overall  ystem  was built rom  a position  f trength. s f o s Fundamentally, e conclude  that here s onsiderable w t i c promise for  expert  systems, eural  networks,  and gen netic  algorithms o  work synergistically  th mathet wi matical  models.

## REFERENCES

Arbib, .A.  (1987). M Brains.  achines,  and  Mathematics. M 2nd Ed., New York:  Springer-Verlag.

Bodin. ., olden,  B.R.,  ssad, .,  Ball,  . (1983).  outing nd L G A A &amp; M R a scheduling  f ehicles d  crews, o v an Computers  and  Operations  eR search. 10,  62-212.

Carbonell, G. 1986).  earning y  Analogy:  ormulating  nd  genJ. ( L b F a erating ans  rom  past  xperience. pl f e In Machine  learning (pp. 371 159).  ioga  Publishing . T Co

Clarke,  .  &amp; Wright,  . 1964).  cheduling  f ehicles om  a  central G J ( S o v fr depot o   number of elivery ints, t a d po Operations  esearch, R 12(4), 568-581.

Erman,  L.D.,  ayes-Roth,  ., esser, R.,  Reddy,  D.R.  (1980). H F L V. &amp; The HEARSAY-I1 speech  understanding  ystem:  ntegrating s I knowledge o esolve certainty, t r un ACM  Computing  Surveys, 12(2), 213-253.

- Finin. . &amp;  Silverman,  . (1986). nteractive  assification T D I cl as  a knowledge cquisition  ol.  n  L.  Kerschberg  Ed.), a to I ( Expert  database  systems (pp. 9-90).  enjamin/Cummings Publishing . 7 B Co

Fisher,   &amp; Jaikumar,  .  981 .    generalized  signment  euristic M. J (1 ) A as h for ehicle  outing, v r Net orks. w I  l, 09-124. 1

- Fukushima,  K.  &amp; Miyaki,  . 1982).  eocognitron:  new  algorithm S ( N A for attern  ecognition  lerant  eformation  nd  shifts    pop r to ofd a in sition, Pattern  ecognition, R 15(6),  55-469. 4
- Goldberg,  .  (1989). D Genetic  lgorithms  n earch, timization d a i s op an machine  learning. Reading, A: Addison-Wesley. M
- Golden, .L.  Stewart, R.  (1985).  mpirical alysis   euristics. B &amp; W. E an ofh In  E.  Lawler t l.  eds.), e a ( The  traveling  lesman roblem. sa p
- Grefenstette,  1984).  ENESIS: A system or sing  enetic arch J. ( G f u g se procedures. Proceedings f  the  1984  Conference n Intelligent o o Systems nd  Machines a (pp. 61  -  165 . 1 )
- Hayong,  Z.  (1985,  uly). assifier tem ith ong-term  emory J Cl sys w l m in  machine  learning. Proceedings  f he irst  nternational  no t F I Co ference n  Genetic lgorithms nd  their  pplications o A a A (pp. 781 182).  ittsburgh,  . P PA
- Holland,  . 1975). J ( Adaptation n  natural  nd  artificial  tems. i a sys Ann Arbor, l:  University  ichigan  Press. M ofM
- Juell, L.,  ygard, .E.,   gadaba, .  (1989,  une).  ultiple ural P. N K &amp; N J M ne networks or electing roblem  solving  echnique, f s a  p t Proceedings of  he  nternational nt  onference  n  Neural etworks IJCNN) t I Joi C o N ( (pp. I-607)  ashington,  C. I W D
- Kadaba,  N.  (1987). Man-Machine Interface  r ehicle uting MSc fo V Ro ( thesis). Department  of  Computer Science  nd Operations  ea R search,  orth  Dakota  State  niversity,  rgo.  D. N U Fa N

Kadaba,  N., ygard, .E.,  uell, L.,   Kangas, . 1990,  anuary). N K J P. &amp; L ( J Modular back-propagation ural  networks  for  large  omain e d pattern  lassification, c in Proceedings  f he nternational  ural o t I Ne Networks  Conference. Washington  DC.

Kadaba,  N., errizo,  ,  &amp; Ram, P. in  ress). P W. ( p Performance nalysis a of istributed  stems  or  arallelizing  etic gorithms.  DSU d sy f p gen al (N Technical  eport  NDSU-CS-  TR-  90-4 R )

Kadaba,  N.  (1990).  ROUTE: X A Knowledge-Based outing  System R Using  Neural  Networks  and Genetic lgorithms Ph.D. isserA ( D tation). Department  of  Computer Science nd  Operations  ea R search,  orth  Dakota  State  niversity,  rgo,  D. N U Fa N

- Myers,  M.,  Kuczewski, .,  Crawford, . (1987). R &amp; W Application  f o new  artificial  ormation  rocessing inciples   attern  lassiinf p pr top c fication  Final eport), ( R U.S.  Army  Research  Ol~ce, ontract C DAAG-29-85-C-0025.
- Nelson, .D. (1983). M Implementation  Techniques  or he  Vehicle f t Routing roblem  (MSc thesis). P North  Dakota  State  niversity, U Fargo,  D. N
- Nygard,  K.E.,  reenberg, ., oll&lt;an,  .,  &amp; Swenson,  E.  (1988). G P B W Generalized  ssignment  ethods  for  he eadline  ehicle uting a m t d v ro problem.  In  B.  Golden and A. Assad  (eds.), Vehicle  outing: r methods  and  studies. Amsterdam:  North-Holland.

Nygard,  K.E.,  uell, L.,   Kadaba,  N.  (1988).  n interactive J P. &amp; A d -cision  upport ystem or ehicle  outing.  n  R.  Shardra t  al. s s f v r I e (eds.), Impact  of ecent  omputer dvances n  operations search. r c a o re New York:  Elsevier.

Nygard, .E.,  uell, L.,   Kadaba,  N.  (1989).  eural etworks or K J P. &amp; N n f selecting hicle  outing euristics ve r h (NDSU Technical eport R NDSU-CS-  TR-89-20).

Nygard,  K.E.,  uell, L.,   Kadaba,  N.  (1989,  ctober).  rtificial J P. &amp; O A intelligence outing  nd  scheduling plications, in  r a ap Proceedings of he erospace pplications   rtificial  elligence  ference. t A A ofA Int Con Dayton,  OH.

Rumelhart, .E.  &amp; McCielland,  . 986). D J 0 Parallel  istributed  od pr cessing,  plorations   he icrostructure    ognition. ex int m ofc Cambridge, MA: MIT Press.

- Rumelhart, .E.,  inton, .E.,  Williams,  .J. 1986).  earning D H G &amp; R ( L representations back.propagating  rors, by er Nature, 323,  533536.
- Waterman, D.A.  (1986). A guide  to  expert  ystems. s Reading, A: M Addison-Wesley.