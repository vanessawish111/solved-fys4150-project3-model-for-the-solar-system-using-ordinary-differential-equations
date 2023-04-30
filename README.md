Download Link: https://assignmentchef.com/product/solved-fys4150-project3-model-for-the-solar-system-using-ordinary-differential-equations
<br>
<h2>Building a model for the solar system using ordinary differential equations</h2>

This project counts one third of the final grade. Projects 4 and 5 give the final two thirds. The total score is 100 out of 100 points. How the projects are graded and evaluated is described at <a href="https://github.com/CompPhysics/ComputationalPhysics/blob/master/doc/Projects/EvaluationGrading/EvaluationForm.md">https://github.com/CompPhysics/ComputationalPhysics/blob/master/doc/Projects/EvaluationGrading/EvaluationForm.md</a>.

<h3>Introduction</h3>

The aim of this project is to develop a code for simulating the solar system using a widely popular algorithm for solving coupled ordinary differential equations, the so-called velocity Verlet algorithm. An important aspect of this project is to be able to object orient your code. There are several coupled ordinary differenatial equations where the basic equations, except for various physical constants and variables, are rather similar. Thus, write once and run many times, one of the central points of object orientation, is something which makes our program easier to extend and build upon when we add more planets or moons or other astronomical objects. The basic equations which govern the system are rather simple, a set of coupled equations that codify Newton’s law of motion due the gravitational force.

In the first part however, we will limit ourselves (in order to test the algorithm) to a hypothetical solar system with the Earth only orbiting around the sun. The only force in the problem is gravity. Newton’s law of gravitation is given by a force <em><span data-mathml="<math xmlns=&quot;http://www.w3.org/1998/Math/MathML&quot;><msub><mi>F</mi><mi>G</mi></msub></math>">F</span></em>GFG

<em><span data-mathml="<math xmlns=&quot;http://www.w3.org/1998/Math/MathML&quot; display=&quot;block&quot;><msub><mi>F</mi><mi>G</mi></msub><mo>=</mo><mfrac><mrow><mi>G</mi><msub><mi>M</mi><mrow class=&quot;MJX-TeXAtom-ORD&quot;><mo>&amp;#x2299;</mo></mrow></msub><msub><mi>M</mi><mrow class=&quot;MJX-TeXAtom-ORD&quot;><mrow class=&quot;MJX-TeXAtom-ORD&quot;><mi mathvariant=&quot;normal&quot;>E</mi><mi mathvariant=&quot;normal&quot;>a</mi><mi mathvariant=&quot;normal&quot;>r</mi><mi mathvariant=&quot;normal&quot;>t</mi><mi mathvariant=&quot;normal&quot;>h</mi></mrow></mrow></msub></mrow><msup><mi>r</mi><mn>2</mn></msup></mfrac><mo>,</mo></math>">F</span></em>G=<em>GM</em>⊙MEarthr2,FG=GM⊙MEarthr2,

where <em><span data-mathml="<math xmlns=&quot;http://www.w3.org/1998/Math/MathML&quot;><msub><mi>M</mi><mrow class=&quot;MJX-TeXAtom-ORD&quot;><mo>&amp;#x2299;</mo></mrow></msub></math>">M</span></em>⊙M⊙ is the mass of the Sun and <em><span data-mathml="<math xmlns=&quot;http://www.w3.org/1998/Math/MathML&quot;><msub><mi>M</mi><mrow class=&quot;MJX-TeXAtom-ORD&quot;><mrow class=&quot;MJX-TeXAtom-ORD&quot;><mi mathvariant=&quot;normal&quot;>E</mi><mi mathvariant=&quot;normal&quot;>a</mi><mi mathvariant=&quot;normal&quot;>r</mi><mi mathvariant=&quot;normal&quot;>t</mi><mi mathvariant=&quot;normal&quot;>h</mi></mrow></mrow></msub></math>">M</span></em>EarthMEarth is the mass of the Earth. The gravitational constant is <em><span data-mathml="<math xmlns=&quot;http://www.w3.org/1998/Math/MathML&quot;><mi>G</mi></math>">G</span></em>G and <em><span data-mathml="<math xmlns=&quot;http://www.w3.org/1998/Math/MathML&quot;><mi>r</mi></math>">r</span></em>r is the distance between the Earth and the Sun. We assume that the Sun has a mass which is much larger than that of the Earth. We can therefore safely neglect the motion of the Sun in this problem. In the first part of this project, your aim is to compute the motion of the the Earth using different methods for solving ordinary differential equations.

We assume that the orbit of the Earth around the Sun is co-planar, and we take this to be the <em><span data-mathml="<math xmlns=&quot;http://www.w3.org/1998/Math/MathML&quot;><mi>x</mi><mi>y</mi></math>">xy</span></em>xy-plane. Using Newton’s second law of motion we get the following equations

<em><span data-mathml="<math xmlns=&quot;http://www.w3.org/1998/Math/MathML&quot; display=&quot;block&quot;><mfrac><mrow><msup><mi>d</mi><mn>2</mn></msup><mi>x</mi></mrow><mrow><mi>d</mi><msup><mi>t</mi><mn>2</mn></msup></mrow></mfrac><mo>=</mo><mfrac><msub><mi>F</mi><mrow class=&quot;MJX-TeXAtom-ORD&quot;><mi>G</mi><mo>,</mo><mi>x</mi></mrow></msub><msub><mi>M</mi><mrow class=&quot;MJX-TeXAtom-ORD&quot;><mrow class=&quot;MJX-TeXAtom-ORD&quot;><mi mathvariant=&quot;normal&quot;>E</mi><mi mathvariant=&quot;normal&quot;>a</mi><mi mathvariant=&quot;normal&quot;>r</mi><mi mathvariant=&quot;normal&quot;>t</mi><mi mathvariant=&quot;normal&quot;>h</mi></mrow></mrow></msub></mfrac><mo>,</mo></math>">d</span></em>2<em>xdt</em>2=<em>F</em>G,<em>x</em>MEarth,d2xdt2=FG,xMEarth,

and

<em><span data-mathml="<math xmlns=&quot;http://www.w3.org/1998/Math/MathML&quot; display=&quot;block&quot;><mfrac><mrow><msup><mi>d</mi><mn>2</mn></msup><mi>y</mi></mrow><mrow><mi>d</mi><msup><mi>t</mi><mn>2</mn></msup></mrow></mfrac><mo>=</mo><mfrac><msub><mi>F</mi><mrow class=&quot;MJX-TeXAtom-ORD&quot;><mi>G</mi><mo>,</mo><mi>y</mi></mrow></msub><msub><mi>M</mi><mrow class=&quot;MJX-TeXAtom-ORD&quot;><mrow class=&quot;MJX-TeXAtom-ORD&quot;><mi mathvariant=&quot;normal&quot;>E</mi><mi mathvariant=&quot;normal&quot;>a</mi><mi mathvariant=&quot;normal&quot;>r</mi><mi mathvariant=&quot;normal&quot;>t</mi><mi mathvariant=&quot;normal&quot;>h</mi></mrow></mrow></msub></mfrac><mo>,</mo></math>">d</span></em>2<em>ydt</em>2=<em>F</em>G,<em>y</em>MEarth,d2ydt2=FG,yMEarth,

where <em><span data-mathml="<math xmlns=&quot;http://www.w3.org/1998/Math/MathML&quot;><msub><mi>F</mi><mrow class=&quot;MJX-TeXAtom-ORD&quot;><mi>G</mi><mo>,</mo><mi>x</mi></mrow></msub></math>">F</span></em><em>G</em>,<em>x</em>FG,x and <em><span data-mathml="<math xmlns=&quot;http://www.w3.org/1998/Math/MathML&quot;><msub><mi>F</mi><mrow class=&quot;MJX-TeXAtom-ORD&quot;><mi>G</mi><mo>,</mo><mi>y</mi></mrow></msub></math>">F</span></em><em>G</em>,<em>y</em>FG,y are the <em><span data-mathml="<math xmlns=&quot;http://www.w3.org/1998/Math/MathML&quot;><mi>x</mi></math>">x</span></em>x and <em><span data-mathml="<math xmlns=&quot;http://www.w3.org/1998/Math/MathML&quot;><mi>y</mi></math>">y</span></em>y components of the gravitational force.

We will use so-called astronomical units when rewriting our equations. Using astronomical units (AU as abbreviation)it means that one astronomical unit of length, known as 1 AU, is the average distance between the Sun and Earth, that is <span data-mathml="<math xmlns=&quot;http://www.w3.org/1998/Math/MathML&quot;><mn>1</mn></math>">1</span>1 AU = <span data-mathml="<math xmlns=&quot;http://www.w3.org/1998/Math/MathML&quot;><mn>1.5</mn><mo>&amp;#x00D7;</mo><msup><mn>10</mn><mrow class=&quot;MJX-TeXAtom-ORD&quot;><mn>11</mn></mrow></msup></math>">1.5×1011</span>1.5×1011 m. It can also be convenient to use years instead of seconds since years match better the time evolution of the solar system. The mass of the Sun is <em><span data-mathml="<math xmlns=&quot;http://www.w3.org/1998/Math/MathML&quot;><msub><mi>M</mi><mrow class=&quot;MJX-TeXAtom-ORD&quot;><mrow class=&quot;MJX-TeXAtom-ORD&quot;><mi mathvariant=&quot;normal&quot;>s</mi><mi mathvariant=&quot;normal&quot;>u</mi><mi mathvariant=&quot;normal&quot;>n</mi></mrow></mrow></msub><mo>=</mo><msub><mi>M</mi><mrow class=&quot;MJX-TeXAtom-ORD&quot;><mo>&amp;#x2299;</mo></mrow></msub><mo>=</mo><mn>2</mn><mo>&amp;#x00D7;</mo><msup><mn>10</mn><mrow class=&quot;MJX-TeXAtom-ORD&quot;><mn>30</mn></mrow></msup></math>">M</span></em>sun=<em>M</em>⊙=2×1030Msun=M⊙=2×1030 kg. The masses of all relevant planets and their distances from the sun are listed in the table here in kg and AU.

<table>

 <thead>

  <tr>

   <td><strong>Planet</strong></td>

   <td><strong>Mass in kg </strong></td>

   <td><strong>Distance to sun in AU</strong></td>

  </tr>

 </thead>

 <tbody>

  <tr>

   <td>Earth</td>

   <td><em><span data-mathml="<math xmlns=&quot;http://www.w3.org/1998/Math/MathML&quot;><msub><mi>M</mi><mrow class=&quot;MJX-TeXAtom-ORD&quot;><mrow class=&quot;MJX-TeXAtom-ORD&quot;><mi mathvariant=&quot;normal&quot;>E</mi><mi mathvariant=&quot;normal&quot;>a</mi><mi mathvariant=&quot;normal&quot;>r</mi><mi mathvariant=&quot;normal&quot;>t</mi><mi mathvariant=&quot;normal&quot;>h</mi></mrow></mrow></msub><mo>=</mo><mn>6</mn><mo>&amp;#x00D7;</mo><msup><mn>10</mn><mrow class=&quot;MJX-TeXAtom-ORD&quot;><mn>24</mn></mrow></msup></math>">M</span></em>Earth=6×1024MEarth=6×1024 kg</td>

   <td>1AU</td>

  </tr>

  <tr>

   <td>Jupiter</td>

   <td><em><span data-mathml="<math xmlns=&quot;http://www.w3.org/1998/Math/MathML&quot;><msub><mi>M</mi><mrow class=&quot;MJX-TeXAtom-ORD&quot;><mrow class=&quot;MJX-TeXAtom-ORD&quot;><mi mathvariant=&quot;normal&quot;>J</mi><mi mathvariant=&quot;normal&quot;>u</mi><mi mathvariant=&quot;normal&quot;>p</mi><mi mathvariant=&quot;normal&quot;>i</mi><mi mathvariant=&quot;normal&quot;>t</mi><mi mathvariant=&quot;normal&quot;>e</mi><mi mathvariant=&quot;normal&quot;>r</mi></mrow></mrow></msub><mo>=</mo><mn>1.9</mn><mo>&amp;#x00D7;</mo><msup><mn>10</mn><mrow class=&quot;MJX-TeXAtom-ORD&quot;><mn>27</mn></mrow></msup></math>">M</span></em>Jupiter=1.9×1027MJupiter=1.9×1027 kg</td>

   <td>5.20 AU</td>

  </tr>

  <tr>

   <td>Mars</td>

   <td><em><span data-mathml="<math xmlns=&quot;http://www.w3.org/1998/Math/MathML&quot;><msub><mi>M</mi><mrow class=&quot;MJX-TeXAtom-ORD&quot;><mrow class=&quot;MJX-TeXAtom-ORD&quot;><mi mathvariant=&quot;normal&quot;>M</mi><mi mathvariant=&quot;normal&quot;>a</mi><mi mathvariant=&quot;normal&quot;>r</mi><mi mathvariant=&quot;normal&quot;>s</mi></mrow></mrow></msub><mo>=</mo><mn>6.6</mn><mo>&amp;#x00D7;</mo><msup><mn>10</mn><mrow class=&quot;MJX-TeXAtom-ORD&quot;><mn>23</mn></mrow></msup></math>">M</span></em>Mars=6.6×1023MMars=6.6×1023 kg</td>

   <td>1.52 AU</td>

  </tr>

  <tr>

   <td>Venus</td>

   <td><em><span data-mathml="<math xmlns=&quot;http://www.w3.org/1998/Math/MathML&quot;><msub><mi>M</mi><mrow class=&quot;MJX-TeXAtom-ORD&quot;><mrow class=&quot;MJX-TeXAtom-ORD&quot;><mi mathvariant=&quot;normal&quot;>V</mi><mi mathvariant=&quot;normal&quot;>e</mi><mi mathvariant=&quot;normal&quot;>n</mi><mi mathvariant=&quot;normal&quot;>u</mi><mi mathvariant=&quot;normal&quot;>s</mi></mrow></mrow></msub><mo>=</mo><mn>4.9</mn><mo>&amp;#x00D7;</mo><msup><mn>10</mn><mrow class=&quot;MJX-TeXAtom-ORD&quot;><mn>24</mn></mrow></msup></math>">M</span></em>Venus=4.9×1024MVenus=4.9×1024 kg</td>

   <td>0.72 AU</td>

  </tr>

  <tr>

   <td>Saturn</td>

   <td><em><span data-mathml="<math xmlns=&quot;http://www.w3.org/1998/Math/MathML&quot;><msub><mi>M</mi><mrow class=&quot;MJX-TeXAtom-ORD&quot;><mrow class=&quot;MJX-TeXAtom-ORD&quot;><mi mathvariant=&quot;normal&quot;>S</mi><mi mathvariant=&quot;normal&quot;>a</mi><mi mathvariant=&quot;normal&quot;>t</mi><mi mathvariant=&quot;normal&quot;>u</mi><mi mathvariant=&quot;normal&quot;>r</mi><mi mathvariant=&quot;normal&quot;>n</mi></mrow></mrow></msub><mo>=</mo><mn>5.5</mn><mo>&amp;#x00D7;</mo><msup><mn>10</mn><mrow class=&quot;MJX-TeXAtom-ORD&quot;><mn>26</mn></mrow></msup></math>">M</span></em>Saturn=5.5×1026MSaturn=5.5×1026 kg</td>

   <td>9.54 AU</td>

  </tr>

  <tr>

   <td>Mercury</td>

   <td><em><span data-mathml="<math xmlns=&quot;http://www.w3.org/1998/Math/MathML&quot;><msub><mi>M</mi><mrow class=&quot;MJX-TeXAtom-ORD&quot;><mrow class=&quot;MJX-TeXAtom-ORD&quot;><mi mathvariant=&quot;normal&quot;>M</mi><mi mathvariant=&quot;normal&quot;>e</mi><mi mathvariant=&quot;normal&quot;>r</mi><mi mathvariant=&quot;normal&quot;>c</mi><mi mathvariant=&quot;normal&quot;>u</mi><mi mathvariant=&quot;normal&quot;>r</mi><mi mathvariant=&quot;normal&quot;>y</mi></mrow></mrow></msub><mo>=</mo><mn>3.3</mn><mo>&amp;#x00D7;</mo><msup><mn>10</mn><mrow class=&quot;MJX-TeXAtom-ORD&quot;><mn>23</mn></mrow></msup></math>">M</span></em>Mercury=3.3×1023MMercury=3.3×1023 kg</td>

   <td>0.39 AU</td>

  </tr>

  <tr>

   <td>Uranus</td>

   <td><em><span data-mathml="<math xmlns=&quot;http://www.w3.org/1998/Math/MathML&quot;><msub><mi>M</mi><mrow class=&quot;MJX-TeXAtom-ORD&quot;><mrow class=&quot;MJX-TeXAtom-ORD&quot;><mi mathvariant=&quot;normal&quot;>U</mi><mi mathvariant=&quot;normal&quot;>r</mi><mi mathvariant=&quot;normal&quot;>a</mi><mi mathvariant=&quot;normal&quot;>n</mi><mi mathvariant=&quot;normal&quot;>u</mi><mi mathvariant=&quot;normal&quot;>s</mi></mrow></mrow></msub><mo>=</mo><mn>8.8</mn><mo>&amp;#x00D7;</mo><msup><mn>10</mn><mrow class=&quot;MJX-TeXAtom-ORD&quot;><mn>25</mn></mrow></msup></math>">M</span></em>Uranus=8.8×1025MUranus=8.8×1025 kg</td>

   <td>19.19 AU</td>

  </tr>

  <tr>

   <td>Neptun</td>

   <td><em><span data-mathml="<math xmlns=&quot;http://www.w3.org/1998/Math/MathML&quot;><msub><mi>M</mi><mrow class=&quot;MJX-TeXAtom-ORD&quot;><mrow class=&quot;MJX-TeXAtom-ORD&quot;><mi mathvariant=&quot;normal&quot;>N</mi><mi mathvariant=&quot;normal&quot;>e</mi><mi mathvariant=&quot;normal&quot;>p</mi><mi mathvariant=&quot;normal&quot;>t</mi><mi mathvariant=&quot;normal&quot;>u</mi><mi mathvariant=&quot;normal&quot;>n</mi></mrow></mrow></msub><mo>=</mo><mn>1.03</mn><mo>&amp;#x00D7;</mo><msup><mn>10</mn><mrow class=&quot;MJX-TeXAtom-ORD&quot;><mn>26</mn></mrow></msup></math>">M</span></em>Neptun=1.03×1026MNeptun=1.03×1026 kg</td>

   <td>30.06 AU</td>

  </tr>

  <tr>

   <td>Pluto</td>

   <td><em><span data-mathml="<math xmlns=&quot;http://www.w3.org/1998/Math/MathML&quot;><msub><mi>M</mi><mrow class=&quot;MJX-TeXAtom-ORD&quot;><mrow class=&quot;MJX-TeXAtom-ORD&quot;><mi mathvariant=&quot;normal&quot;>P</mi><mi mathvariant=&quot;normal&quot;>l</mi><mi mathvariant=&quot;normal&quot;>u</mi><mi mathvariant=&quot;normal&quot;>t</mi><mi mathvariant=&quot;normal&quot;>o</mi></mrow></mrow></msub><mo>=</mo><mn>1.31</mn><mo>&amp;#x00D7;</mo><msup><mn>10</mn><mrow class=&quot;MJX-TeXAtom-ORD&quot;><mn>22</mn></mrow></msup></math>">M</span></em>Pluto=1.31×1022MPluto=1.31×1022 kg</td>

   <td>39.53 AU</td>

  </tr>

 </tbody>

</table>

Pluto is no longer considered a planet, but we add it here for historical reasons. It is optional in this project to include Pluto and eventual moons.

In setting up the equations we can limit ourselves to a co-planar motion and use only the <em><span data-mathml="<math xmlns=&quot;http://www.w3.org/1998/Math/MathML&quot;><mi>x</mi></math>">x</span></em>x and <em><span data-mathml="<math xmlns=&quot;http://www.w3.org/1998/Math/MathML&quot;><mi>y</mi></math>">y</span></em>y coordinates. But you should feel free to extend your equations to three dimensions, it is not very difficult and the data from NASA are all in three dimensions.

<a href="https://www.nasa.gov/index.html">NASA</a> has an excellent site at <a href="http://ssd.jpl.nasa.gov/horizons.cgi#top">http://ssd.jpl.nasa.gov/horizons.cgi#top</a>. From there you can extract initial conditions in order to start your differential equation solver. At the above website you need to change from <strong>OBSERVER</strong> to <strong>VECTOR</strong> and then write in the planet you are interested in. The generated data contain the <em><span data-mathml="<math xmlns=&quot;http://www.w3.org/1998/Math/MathML&quot;><mi>x</mi></math>">x</span></em>x, <em><span data-mathml="<math xmlns=&quot;http://www.w3.org/1998/Math/MathML&quot;><mi>y</mi></math>">y</span></em>y and <em><span data-mathml="<math xmlns=&quot;http://www.w3.org/1998/Math/MathML&quot;><mi>z</mi></math>">z</span></em>z values as well as their corresponding velocities. The velocities are in units of AU per day. Alternatively they can be obtained in terms of km and km/s.

For the first system below involving only the Earth and the Sun, you could just initialize the position with say <em><span data-mathml="<math xmlns=&quot;http://www.w3.org/1998/Math/MathML&quot;><mi>x</mi><mo>=</mo><mn>1</mn></math>">x</span></em>=1x=1 AU and <em><span data-mathml="<math xmlns=&quot;http://www.w3.org/1998/Math/MathML&quot;><mi>y</mi><mo>=</mo><mn>0</mn></math>">y</span></em>=0y=0 AU.

<strong>For this project you can hand in collaborative reports and programs.</strong>

<h3>Project 3a): The Earth-Sun system</h3>

We assume that mass units can be obtained by using the fact that Earth’s orbit is almost circular around the Sun. For circular motion we know that the force must obey the following relation

<em><span data-mathml="<math xmlns=&quot;http://www.w3.org/1998/Math/MathML&quot; display=&quot;block&quot;><msub><mi>F</mi><mi>G</mi></msub><mo>=</mo><mfrac><mrow><msub><mi>M</mi><mrow class=&quot;MJX-TeXAtom-ORD&quot;><mrow class=&quot;MJX-TeXAtom-ORD&quot;><mi mathvariant=&quot;normal&quot;>E</mi><mi mathvariant=&quot;normal&quot;>a</mi><mi mathvariant=&quot;normal&quot;>r</mi><mi mathvariant=&quot;normal&quot;>t</mi><mi mathvariant=&quot;normal&quot;>h</mi></mrow></mrow></msub><msup><mi>v</mi><mn>2</mn></msup></mrow><mi>r</mi></mfrac><mo>=</mo><mfrac><mrow><mi>G</mi><msub><mi>M</mi><mrow class=&quot;MJX-TeXAtom-ORD&quot;><mo>&amp;#x2299;</mo></mrow></msub><msub><mi>M</mi><mrow class=&quot;MJX-TeXAtom-ORD&quot;><mrow class=&quot;MJX-TeXAtom-ORD&quot;><mi mathvariant=&quot;normal&quot;>E</mi><mi mathvariant=&quot;normal&quot;>a</mi><mi mathvariant=&quot;normal&quot;>r</mi><mi mathvariant=&quot;normal&quot;>t</mi><mi mathvariant=&quot;normal&quot;>h</mi></mrow></mrow></msub></mrow><msup><mi>r</mi><mn>2</mn></msup></mfrac><mo>,</mo></math>">F</span></em>G=<em>M</em>Earthv2r=<em>GM</em>⊙MEarthr2,FG=MEarthv2r=GM⊙MEarthr2,

where <em><span data-mathml="<math xmlns=&quot;http://www.w3.org/1998/Math/MathML&quot;><mi>v</mi></math>">v</span></em>v is the velocity of Earth. The latter equation can be used to show that

<em><span data-mathml="<math xmlns=&quot;http://www.w3.org/1998/Math/MathML&quot; display=&quot;block&quot;><msup><mi>v</mi><mn>2</mn></msup><mi>r</mi><mo>=</mo><mi>G</mi><msub><mi>M</mi><mrow class=&quot;MJX-TeXAtom-ORD&quot;><mo>&amp;#x2299;</mo></mrow></msub><mo>=</mo><mn>4</mn><msup><mi>&amp;#x03C0;</mi><mn>2</mn></msup><msup><mrow class=&quot;MJX-TeXAtom-ORD&quot;><mi mathvariant=&quot;normal&quot;>A</mi><mi mathvariant=&quot;normal&quot;>U</mi></mrow><mn>3</mn></msup><mrow class=&quot;MJX-TeXAtom-ORD&quot;><mo>/</mo></mrow><msup><mrow class=&quot;MJX-TeXAtom-ORD&quot;><mi mathvariant=&quot;normal&quot;>y</mi><mi mathvariant=&quot;normal&quot;>r</mi></mrow><mn>2</mn></msup><mo>.</mo></math>">v</span></em>2r=<em>GM</em>⊙=4<em>π</em>2AU3/yr2.v2r=GM⊙=4π2AU3/yr2.

Discretize the above differential equations and set up an algorithm for solving these equations using Euler’s forward algorithm and the so-called velocity Verlet method discussed in the lecture notes and <a href="https://compphysics.github.io/ComputationalPhysics/doc/pub/ode/html/ode-reveal.html">lecture slides</a>.

You can choose if you wish to study the systems in this project in two or three dimensions.

<h3>Project 3b): Writing an object oriented code for the Earth-Sun system</h3>

Write then a program which solves the above differential equations for the Earth-Sun system using Euler’s method and the velocity Verlet method. Write these codes without object orientation first in order to make sure everything is running correctly. Thereafter you should start planning to object orient your code. Try to figure out which parts and operations could be written as classes and generalized. Your task here is to think of the program flow and figure out which parts can be abstracted and reused for many types of operations.

For those of you who will focus on the Molecular Dynamics version of Project 5, much of the structures developed here as well as the implementation of the Verlet algorithm, can be used in that project as well.

<h3>Project 3c): Tests of the algorithms</h3>

Find out which initial value for the velocity that gives a circular orbit and test the stability of your algorithm as function of different time steps <span data-mathml="<math xmlns=&quot;http://www.w3.org/1998/Math/MathML&quot;><mi mathvariant=&quot;normal&quot;>&amp;#x0394;</mi><mi>t</mi></math>">Δ<em>t</em></span>Δt. Make a plot of the results you obtain for the position of the Earth (plot the <em><span data-mathml="<math xmlns=&quot;http://www.w3.org/1998/Math/MathML&quot;><mi>x</mi></math>">x</span></em>x and <em><span data-mathml="<math xmlns=&quot;http://www.w3.org/1998/Math/MathML&quot;><mi>y</mi></math>">y</span></em>y values and/or if you prefer to use three dimensions the <em><span data-mathml="<math xmlns=&quot;http://www.w3.org/1998/Math/MathML&quot;><mi>z</mi></math>">z</span></em>z-value as well) orbiting the Sun.

Check also for the case of a circular orbit that both the kinetic and the potential energies are conserved. Explain why these quantities should be conserved for circular motion.

Discuss eventual differences between the Verlet algorithm and the Euler algorithm. Consider also the number of FLOPs involved and perform a timing of the two algorithms for equal final times.

We will use the velocity Verlet algorithm in the remaining part of the project.

<h3>Project 3d): Conservation of angular momentum</h3>

Use Kepler’s second law to show that angular momentum is conserved. Discuss your results with circular and elliptical orbits for the Earth-Sun system.

<h3>Project 3e): Testing forms of the force</h3>

Till now we have assumed that we have an inverse-square force

<em><span data-mathml="<math xmlns=&quot;http://www.w3.org/1998/Math/MathML&quot; display=&quot;block&quot;><msub><mi>F</mi><mi>G</mi></msub><mo>=</mo><mfrac><mrow><mi>G</mi><msub><mi>M</mi><mrow class=&quot;MJX-TeXAtom-ORD&quot;><mo>&amp;#x2299;</mo></mrow></msub><msub><mi>M</mi><mrow class=&quot;MJX-TeXAtom-ORD&quot;><mrow class=&quot;MJX-TeXAtom-ORD&quot;><mi mathvariant=&quot;normal&quot;>E</mi><mi mathvariant=&quot;normal&quot;>a</mi><mi mathvariant=&quot;normal&quot;>r</mi><mi mathvariant=&quot;normal&quot;>t</mi><mi mathvariant=&quot;normal&quot;>h</mi></mrow></mrow></msub></mrow><msup><mi>r</mi><mn>2</mn></msup></mfrac><mo>.</mo></math>">F</span></em>G=<em>GM</em>⊙MEarthr2.FG=GM⊙MEarthr2.

We replace our inverse-square force with

<em><span data-mathml="<math xmlns=&quot;http://www.w3.org/1998/Math/MathML&quot; display=&quot;block&quot;><msub><mi>F</mi><mi>G</mi></msub><mo>=</mo><mfrac><mrow><mi>G</mi><msub><mi>M</mi><mrow class=&quot;MJX-TeXAtom-ORD&quot;><mo>&amp;#x2299;</mo></mrow></msub><msub><mi>M</mi><mrow class=&quot;MJX-TeXAtom-ORD&quot;><mrow class=&quot;MJX-TeXAtom-ORD&quot;><mi mathvariant=&quot;normal&quot;>E</mi><mi mathvariant=&quot;normal&quot;>a</mi><mi mathvariant=&quot;normal&quot;>r</mi><mi mathvariant=&quot;normal&quot;>t</mi><mi mathvariant=&quot;normal&quot;>h</mi></mrow></mrow></msub></mrow><msup><mi>r</mi><mrow class=&quot;MJX-TeXAtom-ORD&quot;><mi>&amp;#x03B2;</mi></mrow></msup></mfrac><mo>,</mo></math>">F</span></em>G=<em>GM</em>⊙MEarthr<em>β</em>,FG=GM⊙MEarthrβ,

with <em><span data-mathml="<math xmlns=&quot;http://www.w3.org/1998/Math/MathML&quot;><mi>&amp;#x03B2;</mi><mo>&amp;#x2208;</mo><mo stretchy=&quot;false&quot;>[</mo><mn>2</mn><mo>,</mo><mn>3</mn><mo stretchy=&quot;false&quot;>]</mo></math>">β</span></em>∈[2,3]β∈[2,3]. Rerun your Earth-Sun system using the Velocity Verlet algorithm where you let <em><span data-mathml="<math xmlns=&quot;http://www.w3.org/1998/Math/MathML&quot;><mi>&amp;#x03B2;</mi><mo>&amp;#x2208;</mo><mo stretchy=&quot;false&quot;>[</mo><mn>2</mn><mo>,</mo><mn>3</mn><mo stretchy=&quot;false&quot;>]</mo></math>">β</span></em>∈[2,3]β∈[2,3]. What happens to the Earth-Sun system when <em><span data-mathml="<math xmlns=&quot;http://www.w3.org/1998/Math/MathML&quot;><mi>&amp;#x03B2;</mi></math>">β</span></em>β creeps towards <span data-mathml="<math xmlns=&quot;http://www.w3.org/1998/Math/MathML&quot;><mn>3</mn></math>">3</span>3? Comment your results. Can you use the observations of planetary motion to determine by what amount Nature deviates from a perfect inverse-square law?

Consider also an elliptical orbit with an initial position 1 AU from the Sun and an initial velocity of 5 AU/yr. Show that the total energy is a constant (the kinetic and potential energies will vary). Show also that the angular momentum is a constant. If you change the parameter <em><span data-mathml="<math xmlns=&quot;http://www.w3.org/1998/Math/MathML&quot;><mi>&amp;#x03B2;</mi></math>">β</span></em>β in <em><span data-mathml="<math xmlns=&quot;http://www.w3.org/1998/Math/MathML&quot;><mi>F</mi><mo stretchy=&quot;false&quot;>(</mo><mi>r</mi><mo stretchy=&quot;false&quot;>)</mo><mo>&amp;#x221D;</mo><mo>&amp;#x2212;</mo><mn>1</mn><mrow class=&quot;MJX-TeXAtom-ORD&quot;><mo>/</mo></mrow><msup><mi>r</mi><mrow class=&quot;MJX-TeXAtom-ORD&quot;><mi>&amp;#x03B2;</mi></mrow></msup></math>">F</span></em>(<em>r</em>)∝−1/<em>r</em><em>β</em>F(r)∝−1/rβ from <em><span data-mathml="<math xmlns=&quot;http://www.w3.org/1998/Math/MathML&quot;><mi>&amp;#x03B2;</mi><mo>=</mo><mn>2</mn></math>">β</span></em>=2β=2 to <em><span data-mathml="<math xmlns=&quot;http://www.w3.org/1998/Math/MathML&quot;><mi>&amp;#x03B2;</mi><mo>=</mo><mn>3</mn></math>">β</span></em>=3β=3, are these quantities conserved ? Discuss your results. (Hint: relate your results to Kepler’s laws).

<h3>Project 3f): Escape velocity</h3>

Consider then a planet which begins at a distance of 1 AU from the sun. Find out by trial and error what the initial velocity must be in order for the planet to escape from the sun. Can you find an exact answer? How does that match your numerical results?

<h3>Project 3g): The three-body problem</h3>

We will now study the three-body problem, still with the Sun kept fixed as the center of mass of the system but including Jupiter (the most massive planet in the solar system, having a mass that is approximately 1000 times smaller than that of the Sun) together with the Earth. This leads to a three-body problem. Without Jupiter, the Earth’s motion is stable and unchanging with time. The aim here is to find out how much Jupiter alters the Earth’s motion.

The program you have developed can easily be modified by simply adding the magnitude of the force betweem the Earth and Jupiter.

This force is given again by

<em><span data-mathml="<math xmlns=&quot;http://www.w3.org/1998/Math/MathML&quot; display=&quot;block&quot;><msub><mi>F</mi><mrow class=&quot;MJX-TeXAtom-ORD&quot;><mrow class=&quot;MJX-TeXAtom-ORD&quot;><mi mathvariant=&quot;normal&quot;>E</mi><mi mathvariant=&quot;normal&quot;>a</mi><mi mathvariant=&quot;normal&quot;>r</mi><mi mathvariant=&quot;normal&quot;>t</mi><mi mathvariant=&quot;normal&quot;>h</mi><mo>&amp;#x2212;</mo><mi mathvariant=&quot;normal&quot;>J</mi><mi mathvariant=&quot;normal&quot;>u</mi><mi mathvariant=&quot;normal&quot;>p</mi><mi mathvariant=&quot;normal&quot;>i</mi><mi mathvariant=&quot;normal&quot;>t</mi><mi mathvariant=&quot;normal&quot;>e</mi><mi mathvariant=&quot;normal&quot;>r</mi></mrow></mrow></msub><mo>=</mo><mfrac><mrow><mi>G</mi><msub><mi>M</mi><mrow class=&quot;MJX-TeXAtom-ORD&quot;><mrow class=&quot;MJX-TeXAtom-ORD&quot;><mi mathvariant=&quot;normal&quot;>J</mi><mi mathvariant=&quot;normal&quot;>u</mi><mi mathvariant=&quot;normal&quot;>p</mi><mi mathvariant=&quot;normal&quot;>i</mi><mi mathvariant=&quot;normal&quot;>t</mi><mi mathvariant=&quot;normal&quot;>e</mi><mi mathvariant=&quot;normal&quot;>r</mi></mrow></mrow></msub><msub><mi>M</mi><mrow class=&quot;MJX-TeXAtom-ORD&quot;><mrow class=&quot;MJX-TeXAtom-ORD&quot;><mi mathvariant=&quot;normal&quot;>E</mi><mi mathvariant=&quot;normal&quot;>a</mi><mi mathvariant=&quot;normal&quot;>r</mi><mi mathvariant=&quot;normal&quot;>t</mi><mi mathvariant=&quot;normal&quot;>h</mi></mrow></mrow></msub></mrow><msubsup><mi>r</mi><mrow class=&quot;MJX-TeXAtom-ORD&quot;><mrow class=&quot;MJX-TeXAtom-ORD&quot;><mi mathvariant=&quot;normal&quot;>E</mi><mi mathvariant=&quot;normal&quot;>a</mi><mi mathvariant=&quot;normal&quot;>r</mi><mi mathvariant=&quot;normal&quot;>t</mi><mi mathvariant=&quot;normal&quot;>h</mi><mo>&amp;#x2212;</mo><mi mathvariant=&quot;normal&quot;>J</mi><mi mathvariant=&quot;normal&quot;>u</mi><mi mathvariant=&quot;normal&quot;>p</mi><mi mathvariant=&quot;normal&quot;>i</mi><mi mathvariant=&quot;normal&quot;>t</mi><mi mathvariant=&quot;normal&quot;>e</mi><mi mathvariant=&quot;normal&quot;>r</mi></mrow></mrow><mn>2</mn></msubsup></mfrac><mo>,</mo></math>">F</span></em>Earth−Jupiter=<em>GM</em>JupiterMEarthr2Earth−Jupiter,FEarth−Jupiter=GMJupiterMEarthrEarth−Jupiter2,

where <em><span data-mathml="<math xmlns=&quot;http://www.w3.org/1998/Math/MathML&quot;><msub><mi>M</mi><mrow class=&quot;MJX-TeXAtom-ORD&quot;><mrow class=&quot;MJX-TeXAtom-ORD&quot;><mi mathvariant=&quot;normal&quot;>J</mi><mi mathvariant=&quot;normal&quot;>u</mi><mi mathvariant=&quot;normal&quot;>p</mi><mi mathvariant=&quot;normal&quot;>i</mi><mi mathvariant=&quot;normal&quot;>t</mi><mi mathvariant=&quot;normal&quot;>e</mi><mi mathvariant=&quot;normal&quot;>r</mi></mrow></mrow></msub></math>">M</span></em>JupiterMJupiter is the mass of the sun and <em><span data-mathml="<math xmlns=&quot;http://www.w3.org/1998/Math/MathML&quot;><msub><mi>M</mi><mrow class=&quot;MJX-TeXAtom-ORD&quot;><mrow class=&quot;MJX-TeXAtom-ORD&quot;><mi mathvariant=&quot;normal&quot;>E</mi><mi mathvariant=&quot;normal&quot;>a</mi><mi mathvariant=&quot;normal&quot;>r</mi><mi mathvariant=&quot;normal&quot;>t</mi><mi mathvariant=&quot;normal&quot;>h</mi></mrow></mrow></msub></math>">M</span></em>EarthMEarth is the mass of Earth. The gravitational constant is <em><span data-mathml="<math xmlns=&quot;http://www.w3.org/1998/Math/MathML&quot;><mi>G</mi></math>">G</span></em>G and <em><span data-mathml="<math xmlns=&quot;http://www.w3.org/1998/Math/MathML&quot;><msub><mi>r</mi><mrow class=&quot;MJX-TeXAtom-ORD&quot;><mrow class=&quot;MJX-TeXAtom-ORD&quot;><mi mathvariant=&quot;normal&quot;>E</mi><mi mathvariant=&quot;normal&quot;>a</mi><mi mathvariant=&quot;normal&quot;>r</mi><mi mathvariant=&quot;normal&quot;>t</mi><mi mathvariant=&quot;normal&quot;>h</mi><mo>&amp;#x2212;</mo><mi mathvariant=&quot;normal&quot;>J</mi><mi mathvariant=&quot;normal&quot;>u</mi><mi mathvariant=&quot;normal&quot;>p</mi><mi mathvariant=&quot;normal&quot;>i</mi><mi mathvariant=&quot;normal&quot;>t</mi><mi mathvariant=&quot;normal&quot;>e</mi><mi mathvariant=&quot;normal&quot;>r</mi></mrow></mrow></msub></math>">r</span></em>Earth−JupiterrEarth−Jupiter is the distance between Earth and Jupiter.

We assume again that the orbits of the two planets are co-planar, and we take this to be the <em><span data-mathml="<math xmlns=&quot;http://www.w3.org/1998/Math/MathML&quot;><mi>x</mi><mi>y</mi></math>">xy</span></em>xy-plane (you can easily extend the equations to three dimensions). Modify your first-order differential equations in order to accomodate both the motion of the Earth and Jupiter by taking into account the distance in <em><span data-mathml="<math xmlns=&quot;http://www.w3.org/1998/Math/MathML&quot;><mi>x</mi></math>">x</span></em>x and <em><span data-mathml="<math xmlns=&quot;http://www.w3.org/1998/Math/MathML&quot;><mi>y</mi></math>">y</span></em>y between the Earth and Jupiter. Set up the algorithm and plot the positions of the Earth and Jupiter using the velocity Verlet algorithm. Discuss the stability of the solutions using your Verlet solver.

Repeat the calculations by increasing the mass of Jupiter by a factor of 10 and 1000 and plot the position of the Earth. Study again the stability of the Verlet solver.

<h3>Project 3h): Final model for all planets of the solar system</h3>

Finally, using our Verlet solver, we carry out a real three-body calculation where all three systems, the Earth, Jupiter and the Sun are in motion. To do this, choose the center-of-mass position of the three-body system as the origin rather than the position of the sun. Give the Sun an initial velocity which makes the total momentum of the system exactly zero (the center-of-mass will remain fixed). Compare these results with those from the previous exercise and comment your results. Extend your program to include all planets in the solar system (if you have time, you can also include the various moons, but it is not required) and discuss your results. Use the above NASA link to set up the initial positions and velocities for all planets.

<h3>Project 3i): The perihelion precession of Mercury</h3>

An important test of the general theory of relativity was to compare its prediction for the perihelion precession of Mercury to the observed value. The observed value of the perihelion precession, when all classical effects (such as the perturbation of the orbit due to gravitational attraction from the other planets) are subtracted, is <span data-mathml="<math xmlns=&quot;http://www.w3.org/1998/Math/MathML&quot;><msup><mn>43</mn><mo>&amp;#x2033;</mo></msup></math>">43′′</span>43″ (<span data-mathml="<math xmlns=&quot;http://www.w3.org/1998/Math/MathML&quot;><mn>43</mn></math>">43</span>43 arc seconds) per century.

Closed elliptical orbits are a special feature of the Newtonian <span data-mathml="<math xmlns=&quot;http://www.w3.org/1998/Math/MathML&quot;><mn>1</mn><mrow class=&quot;MJX-TeXAtom-ORD&quot;><mo>/</mo></mrow><msup><mi>r</mi><mn>2</mn></msup></math>">1/<em>r</em>2</span>1/r2 force. In general, any correction to the pure <span data-mathml="<math xmlns=&quot;http://www.w3.org/1998/Math/MathML&quot;><mn>1</mn><mrow class=&quot;MJX-TeXAtom-ORD&quot;><mo>/</mo></mrow><msup><mi>r</mi><mn>2</mn></msup></math>">1/<em>r</em>2</span>1/r2 behaviour will lead to an orbit which is not closed, i.e. after one complete orbit around the Sun, the planet will not be at exactly the same position as it started. If the correction is small, then each orbit around the Sun will be almost the same as the classical ellipse, and the orbit can be thought of as an ellipse whose orientation in space slowly rotates. In other words, the perihelion of the ellipse slowly precesses around the Sun.

You will now study the orbit of Mercury around the Sun, adding a general relativistic correction to the Newtonian gravitational force, so that the force becomes

<em><span data-mathml="<math xmlns=&quot;http://www.w3.org/1998/Math/MathML&quot; display=&quot;block&quot;><msub><mi>F</mi><mi>G</mi></msub><mo>=</mo><mfrac><mrow><mi>G</mi><msub><mi>M</mi><mrow class=&quot;MJX-TeXAtom-ORD&quot;><mi mathvariant=&quot;normal&quot;>S</mi><mi mathvariant=&quot;normal&quot;>u</mi><mi mathvariant=&quot;normal&quot;>n</mi></mrow></msub><msub><mi>M</mi><mrow class=&quot;MJX-TeXAtom-ORD&quot;><mi mathvariant=&quot;normal&quot;>M</mi><mi mathvariant=&quot;normal&quot;>e</mi><mi mathvariant=&quot;normal&quot;>r</mi><mi mathvariant=&quot;normal&quot;>c</mi><mi mathvariant=&quot;normal&quot;>u</mi><mi mathvariant=&quot;normal&quot;>r</mi><mi mathvariant=&quot;normal&quot;>y</mi></mrow></msub></mrow><msup><mi>r</mi><mn>2</mn></msup></mfrac><mrow><mo>[</mo><mrow><mn>1</mn><mo>+</mo><mfrac><mrow><mn>3</mn><msup><mi>l</mi><mn>2</mn></msup></mrow><mrow><msup><mi>r</mi><mn>2</mn></msup><msup><mi>c</mi><mn>2</mn></msup></mrow></mfrac></mrow><mo>]</mo></mrow></math>">F</span></em>G=<em>GM</em>SunMMercuryr2[1+3<em>l</em>2r2c2]FG=GMSunMMercuryr2[1+3l2r2c2]

where <em><span data-mathml="<math xmlns=&quot;http://www.w3.org/1998/Math/MathML&quot;><msub><mi>M</mi><mrow class=&quot;MJX-TeXAtom-ORD&quot;><mi mathvariant=&quot;normal&quot;>M</mi><mi mathvariant=&quot;normal&quot;>e</mi><mi mathvariant=&quot;normal&quot;>r</mi><mi mathvariant=&quot;normal&quot;>c</mi><mi mathvariant=&quot;normal&quot;>u</mi><mi mathvariant=&quot;normal&quot;>r</mi><mi mathvariant=&quot;normal&quot;>y</mi></mrow></msub></math>">M</span></em>MercuryMMercury is the mass of Mercury, <em><span data-mathml="<math xmlns=&quot;http://www.w3.org/1998/Math/MathML&quot;><mi>r</mi></math>">r</span></em>r is the distance between Mercury and the Sun, <em><span data-mathml="<math xmlns=&quot;http://www.w3.org/1998/Math/MathML&quot;><mi>l</mi><mo>=</mo><mrow class=&quot;MJX-TeXAtom-ORD&quot;><mo stretchy=&quot;false&quot;>|</mo></mrow><mrow class=&quot;MJX-TeXAtom-ORD&quot;><mover><mi>r</mi><mo stretchy=&quot;false&quot;>&amp;#x2192;</mo></mover></mrow><mo>&amp;#x00D7;</mo><mrow class=&quot;MJX-TeXAtom-ORD&quot;><mover><mi>v</mi><mo stretchy=&quot;false&quot;>&amp;#x2192;</mo></mover></mrow><mrow class=&quot;MJX-TeXAtom-ORD&quot;><mo stretchy=&quot;false&quot;>|</mo></mrow></math>">l</span></em>=|<em>r</em>⃗ ×<em>v</em>⃗ |l=|r→×v→| is the magnitude of Mercury’s orbital angular momentum per unit mass, and <em><span data-mathml="<math xmlns=&quot;http://www.w3.org/1998/Math/MathML&quot;><mi>c</mi></math>">c</span></em>c is the speed of light in vacuum. Run a simulation over one century of Mercury’s orbit around the Sun with no other planets present, starting with Mercury at perihelion on the <em><span data-mathml="<math xmlns=&quot;http://www.w3.org/1998/Math/MathML&quot;><mi>x</mi></math>">x</span></em>x axis. Check then the value of the perihelion angle <em><span data-mathml="<math xmlns=&quot;http://www.w3.org/1998/Math/MathML&quot;><msub><mi>&amp;#x03B8;</mi><mrow class=&quot;MJX-TeXAtom-ORD&quot;><mi mathvariant=&quot;normal&quot;>p</mi></mrow></msub></math>">θ</span></em>pθp, using

<span data-mathml="<math xmlns=&quot;http://www.w3.org/1998/Math/MathML&quot; display=&quot;block&quot;><mi>tan</mi><mo>&amp;#x2061;</mo><msub><mi>&amp;#x03B8;</mi><mrow class=&quot;MJX-TeXAtom-ORD&quot;><mi mathvariant=&quot;normal&quot;>p</mi></mrow></msub><mo>=</mo><mfrac><msub><mi>y</mi><mrow class=&quot;MJX-TeXAtom-ORD&quot;><mi mathvariant=&quot;normal&quot;>p</mi></mrow></msub><msub><mi>x</mi><mrow class=&quot;MJX-TeXAtom-ORD&quot;><mi mathvariant=&quot;normal&quot;>p</mi></mrow></msub></mfrac></math>">tan<em>θ</em>p</span>=<em>y</em>pxptan⁡θp=ypxp

where <em><span data-mathml="<math xmlns=&quot;http://www.w3.org/1998/Math/MathML&quot;><msub><mi>x</mi><mrow class=&quot;MJX-TeXAtom-ORD&quot;><mi mathvariant=&quot;normal&quot;>p</mi></mrow></msub></math>">x</span></em>pxp (<em><span data-mathml="<math xmlns=&quot;http://www.w3.org/1998/Math/MathML&quot;><msub><mi>y</mi><mrow class=&quot;MJX-TeXAtom-ORD&quot;><mi mathvariant=&quot;normal&quot;>p</mi></mrow></msub></math>">y</span></em>pyp) is the <em><span data-mathml="<math xmlns=&quot;http://www.w3.org/1998/Math/MathML&quot;><mi>x</mi></math>">x</span></em>x (<em><span data-mathml="<math xmlns=&quot;http://www.w3.org/1998/Math/MathML&quot;><mi>y</mi></math>">y</span></em>y) position of Mercury at perihelion, i.e. at the point where Mercury is at its closest to the Sun. You may use that the speed of Mercury at perihelion is <span data-mathml="<math xmlns=&quot;http://www.w3.org/1998/Math/MathML&quot;><mn>12.44</mn><mspace width=&quot;thinmathspace&quot; /><mrow class=&quot;MJX-TeXAtom-ORD&quot;><mi mathvariant=&quot;normal&quot;>A</mi><mi mathvariant=&quot;normal&quot;>U</mi></mrow><mrow class=&quot;MJX-TeXAtom-ORD&quot;><mo>/</mo></mrow><mrow class=&quot;MJX-TeXAtom-ORD&quot;><mi mathvariant=&quot;normal&quot;>y</mi><mi mathvariant=&quot;normal&quot;>r</mi></mrow></math>">12.44AU/yr</span>12.44AU/yr, and that the distance to the Sun at perihelion is <span data-mathml="<math xmlns=&quot;http://www.w3.org/1998/Math/MathML&quot;><mn>0.3075</mn><mspace width=&quot;thinmathspace&quot; /><mrow class=&quot;MJX-TeXAtom-ORD&quot;><mi mathvariant=&quot;normal&quot;>A</mi><mi mathvariant=&quot;normal&quot;>U</mi></mrow></math>">0.3075AU</span>0.3075AU. You need to make sure that the time resolution used in your simulation is sufficient, for example by checking that the perihelion precession you get with a pure Newtonian force is at least a few orders of magnitude smaller than the observed perihelion precession of Mercury. Can the observed perihelion precession of Mercury be explained by the general theory of relativity?

<h2>Introduction to numerical projects</h2>

Here follows a brief recipe and recommendation on how to write a report for each project.

<ul>

 <li>Give a short description of the nature of the problem and the eventual numerical methods you have used.</li>

 <li>Describe the algorithm you have used and/or developed. Here you may find it convenient to use pseudocoding. In many cases you can describe the algorithm in the program itself.</li>

 <li>Include the source code of your program. Comment your program properly.</li>

 <li>If possible, try to find analytic solutions, or known limits in order to test your program when developing the code.</li>

 <li>Include your results either in figure form or in a table. Remember to label your results. All tables and figures should have relevant captions and labels on the axes.</li>

 <li>Try to evaluate the reliabilty and numerical stability/precision of your results. If possible, include a qualitative and/or quantitative discussion of the numerical stability, eventual loss of precision etc.</li>

 <li>Try to give an interpretation of you results in your answers to the problems.</li>

 <li>Critique: if possible include your comments and reflections about the exercise, whether you felt you learnt something, ideas for improvements and other thoughts you’ve made when solving the exercise. We wish to keep this course at the interactive level and your comments can help us improve it.</li>

 <li>Try to establish a practice where you log your work at the computerlab. You may find such a logbook very handy at later stages in your work, especially when you don’t properly remember what a previous test version of your program did. Here you could also record the time spent on solving the exercise, various algorithms you may have tested or other topics which you feel worthy of mentioning.</li>

</ul>

<h2>Format for electronic delivery of report and programs</h2>

The preferred format for the report is a PDF file. You can also use DOC or postscript formats or as an ipython notebook file. As programming language we prefer that you choose between C/C++, Fortran2008 or Python. The following prescription should be followed when preparing the report:

<ul>

 <li>Use <strong>Canvas</strong> to hand in your projects, log in at <a href="https://www.uio.no/english/services/it/education/canvas/">https://www.uio.no/english/services/it/education/canvas/</a> with your normal UiO username and password.</li>

 <li>Upload <strong>only</strong> the report file! For the source code file(s) you have developed please provide us with your link to your github domain. The report file should include all of your discussions and a list of the codes you have developed. Do not include library files which are available at the course homepage, unless you have made specific changes to them. Alternatively, you can just upload the address to your GitHub or GitLab repository.</li>

 <li>In your git repository, please include a folder which contains selected results. These can be in the form of output from your code for a selected set of runs and input parameters.</li>

 <li>In this and all later projects, you should include tests (for example unit tests) of your code(s).</li>

 <li>Comments from us on your projects, approval or not, corrections to be made etc can be found under your <strong>Canvas</strong> domain and are only visible to you and the teachers of the course.</li>

</ul>