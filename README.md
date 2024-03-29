# Holomorph

Tools for visualizing Complex functions.
See examples in look at `example_*.py` files.

## ColorPlot
This is very powerful method for visualizing **complex valued** functions. And it becomes more powerful with interactive Matplotlib plot, where you can gat function value at any point:

<img src="https://github.com/ashmat98/Holomorph/blob/master/output/screenshot.jpg?raw=true">

On the left axis is plotted input values to the function, which is same as output of _identity_ function.<br/>
On the right axis is plotted function values.<br/>
On the yellow box you see function input and output at cursor point.<br/>
- Color corresponds to argument of the function value evaluated at the corresponding point: _Red_ for <img src="/tex/29632a9bf827ce0200454dd32fc3be82.svg?invert_in_darkmode&sanitize=true" align=middle width=8.219209349999991pt height=21.18721440000001pt/>, _Cyan_ for <img src="/tex/f30fdded685c83b0e7b446aa9c9aa120.svg?invert_in_darkmode&sanitize=true" align=middle width=9.96010619999999pt height=14.15524440000002pt/>, etc.
- Brightness corresponds to the magnitude/absolute value of the function value evaluated at the corresponding point: _Black_ for 0, _Brighter_ - higher absolute value.
```python
# see `example_holomorphic_function.py`
from plot_colors import ColorPlot

ColorPlot(lambda z: 0.5*(z +  1/z), 
	(-2, 2), (-2, 2), 0.005,
	color_power=(1/4), color_clip=4).show()
# this will open interactive window, with _annotation_ feature
```
## GridTransform
This is another method for visualizing complex valued (of any 2d-to-2d function), by viewing trajectory of each point of the input domain during transformation.
This transformation is **homeomorphism** between <img src="/tex/d9950175c5a8ebf52f3b908f740c9a20.svg?invert_in_darkmode&sanitize=true" align=middle width=104.2882401pt height=24.65753399999998pt/> and <img src="/tex/61c6e13634b03dcac7df3b15aba679b3.svg?invert_in_darkmode&sanitize=true" align=middle width=126.89108354999998pt height=24.65753399999998pt/> given by:
<p align="center"><img src="/tex/cc694c9396c7ff068d45bf8d2115658f.svg?invert_in_darkmode&sanitize=true" align=middle width=313.38637934999997pt height=16.438356pt/></p>
<p align="center">
 <img src="https://github.com/ashmat98/Holomorph/blob/master/output/sample_function.gif?raw=true"  class="center"> </p>
 (this is low resolution gif sample image)

By default, input 2d-plane grid-lines are added to transformable objects (or paths). In the example above, 3 circular paths are also added to the transformable objects.<br/>
this is the sample code:

```python
# see `example_holomorphic_function.py`
from grid_transform import GridTransformer
import numpy as np

gt = GridTransformer(lambda z: 0.5*(z +  1/z),
	(-4, 4), (-4, 4), 0.1, 0.01,
	plt_xlim=(-2., 2.), plt_ylim=(-2., 2.))

# add some curves
gt.add_curve(np.exp(np.pi * 2j * np.linspace(0,1,200, endpoint=True)), lw=4)
gt.add_curve(0.5*np.exp(np.pi * 2j * np.linspace(0,1,200, endpoint=True)), lw=4)
gt.add_curve( 1j+0.1*np.exp(np.pi * 2j * np.linspace(0,1,200, endpoint=True)), lw=2)

gt.transform("output/sample_function.mp4", seconds=16, fps=60,
	figsize=(10, 10), dpi=200, plus_reverse=True)
```
This code evaluates <img src="/tex/bca874059cff87d3bab83e6cffea542f.svg?invert_in_darkmode&sanitize=true" align=middle width=43.86614759999999pt height=24.65753399999998pt/> from <img src="/tex/1c899e1c767eb4eac89facb5d1f2cb0d.svg?invert_in_darkmode&sanitize=true" align=middle width=36.07293689999999pt height=21.18721440000001pt/> to <img src="/tex/ea8e02b76558beb2e7fbd75146337fe7.svg?invert_in_darkmode&sanitize=true" align=middle width=36.07293689999999pt height=21.18721440000001pt/> continuously, during 16 seconds, then runs backward animation.

Note that any 2d-to-2d function can be made into complex-valued, not necessarily holomorphic function. For example, we can visualize linear transformations as well.

```python
# see `example_linear_transformation.py`
def  linear_function(A):
	A = np.array(A)
	def  f(z):
		x, y = np.real(z), np.imag(z)
		result = np.dot(A, np.stack([x,y], axis=0))
		x, y = result
		return x + y * 1j
	return f
```

## Useful tools

From Complex analysis we know that there are [different branches of logarithm](https://en.wikipedia.org/wiki/Complex_logarithm). We introduce logarithm function for custom branch:

```python
from functions import log, PI
```
For example `log(x, -PI)` is the _Principal branch of logarithm_.
Argument of `log(x, q)` is in range <img src="/tex/8914f160bbccbb37bd8a72889441ef65.svg?invert_in_darkmode&sanitize=true" align=middle width=83.35044629999999pt height=24.65753399999998pt/>.
