---
title: "Fractal Object: Dimension, Self-Similarity, Infinity"
date: 2021-08-02T15:34:30-04:00
lang: 
  - NA
classes: wide
layout: single
categories:
  - blog
tags:
  - Fractal
  - Python
  - Julia
  - Mandelbrot
  - Récursivité
  - Infini
gallery:
  - image_path: /assets/images/Artifacts/fractal_1.png
  - image_path: /assets/images/Artifacts/fractal_2.png
  - image_path: /assets/images/Artifacts/fractal_3.png
header:
  teaser: /assets/images/Artifacts/teaser_fractal.jfif
---

> *They are present in tropical forests, at the forefront of medical research, in films and everywhere where wireless communication reigns. This mystery of nature has finally been unraveled. "Damn, but it's of course !". Perhaps you have never heard of these strange shapes, yet they are everywhere around you. Their name: fractals.*

<cite> ARTE report </cite> -- in search of the hidden dimension
{: .small}

{% include video id="Tpsu2uz9rCE" provider="youtube" %}

## Introduction

As you will have understood if you watched the excellent ARTE documentary presented above, fractals are *infinitely fragmented* geometric objects which have the particularity of presenting similar structures at all scales. This type of geometry makes it possible to model with simple recursive formulas infinitely complex figures but also to describe natural phenomena (patterns of snowflakes, path taken by lightning, shape of a cabbage of romanesco, structure of lungs ...) and to find applications in technological fields (antennas, transistors, graphic generation of landscapes...).

<p align="center">
   <img src="/assets/images/Artifacts/fractals_in_nature.png" width="100%"/>
</p>

**Note:** Fractals found in nature are finite approximations of real mathematical objects.
{: .notice--primary}

Fractals are notably characterized by the counter-intuitive notion of **non-integer dimension**. Indeed, we can define a general scaling rule that links the measure $N$, a scale factor $\varepsilon$ and the dimension $D$:

$$ N = \varepsilon^{-D} $$

For example, for a usual geometric figure like the square, its dimension is $D=2$ and if we subdivide it into $3$ its area is $N=9$, we have $9=\frac{1}{3} ^{-2}$. We can apply this same reasoning for a cube or even a line.

<p align="center">
  <img src="/assets/images/Artifacts/scaling_rule.png" width="60%"/>
</p>

Now, we seek to find the dimension of a simple fractal figure. The above formula gives us:
$$ D = -\frac{\log N}{\log \varepsilon} $$

If we are interested in a figure such as the Von Koch curve which consists, from a segment, of recursively constructing equilateral triangles on each sub-segment (see animation below). By counting the segments at each new scaling, we understand that the length of the Koch curve is multiplied by $4$ for each scaling $\varepsilon=\frac{1}{3}$ (we divides the segments by 3). We therefore find that its dimension is $D = \frac{\log 4}{\log 3} \approx 1.26$. It is not a simple one-dimensional curve, nor a surface but something "in between".

<p align="center">
  <img src="/assets/images/Artifacts/von_koch.gif" width="60%"/>
</p>

**Note:** The approach presented above is conceptual. A rigorous and definite definition for any set is the [Hausdorff dimension](https://fr.wikipedia.org/wiki/Dimension_de_Hausdorff). It is not easy to implement...
{: .notice--primary}

We can differentiate 3 main categories of fractal:

- systems of **iterated functions**. They have a fixed geometric rule like Von Koch's snowflake, Sierpinski's carpet, Peano's curve.
- **random** fractals. They are generated by a stochastic process as in nature or fractal landscapes.
- the sets defined by a **recurrence relation** at each point of a space. We can cite the set of Julia, Mandelbrot, Lyapunov. They are sometimes called  *Escape-time fractals*.

## Julia's set

The Julia set associated with a fixed complex number $c$ is the set of initial values ​​$z_0$ for which the following sequence is bounded:

$$
\left\{
  \begin{array}{ll}
    z_0 \in \mathbb{C} \\
    z_{n+1} = z_n^2 + c
  \end{array}
\right.
$$

To generate a Julia set by computer, the idea is to discretize the space in a fixed interval to have a finite number of value $z_0$ for which we will test the convergence of the sequence.

```python
# INITIALIZATION
# value of c fixed
c_reel, c_imag = 0.3, 0.5 
# interval limit
x_min, x_max = -1, 1
y_min, y_max = -1, 1
# discretization
size = 5000 
x_step = (x_max - x_min)/size
y_step = (y_max - y_min)/size
M = np.zeros((size,size))
```

To be able to work with complex numbers, I chose to decompose the real part `z_real` and the imaginary part `z_image`. Then, we test the convergence for a given point by looking if we have not exceeded a finite number of iterations `n_iter < itermax`. We can also, in addition, check that the sequence $(z_n)$ is divergent if its modulus is strictly greater than $2$, `z_reel**2 + z_imag**2 < 4` (cf [demonstration](https://fr.wikipedia.org/wiki/Ensemble_de_Mandelbrot#Barri%C3%A8re_du_module_%C3%A9gal_%C3%A0_2)). Finally, we can fill a matrix `M` with $0$ or $1$ according to the convergence test. But, for a more aesthetic final visual rendering, we can also fill the `M` matrix according to the estimated convergence rate with `n_iter/itermax`.

```python
# LOOP ON ALL PIXEL = COMPLEX PLANE
for i in (range(size)):
    for j in range(size):
        n_iter = 0
        # convert pixel position to a complex number
        z_reel = i * x_step + x_min
        z_imag = j * y_step + y_min
        # update sequence until convergence
        while (z_reel**2 + z_imag**2 < 4) and (n_iter < itermax):
            z_reel, z_imag = z_reel**2 - z_imag**2 + c_reel, 2*z_imag*z_reel + c_imag
            n_iter = n_iter + 1
        # color image according to convergence rate
        M[j,i] = 255*n_iter/itermax
```

**Tip:** In python, we could have directly used the `complex()` function to have a complex object. In this case, the `z_reel` and `z_imag` variables would be useless and one could directly retrieve the absolute value and square a single complex variable `z`.
{: .notice--info}

Finally, we can generate Julia sets for different fixed values ​​of $c$ and to change the visual we can have fun testing different *colormap*. Below are some results I generated.

{% include gallery %}

Note that the figures obtained vary greatly depending on the value of the complex $c$ chosen. In fact, one can generate the sets of julia for a sequence of consecutive complexes to see how the figures evolve and make an animation of them.

<p align="center">
  <img src="/assets/images/Artifacts/fractal_julia.gif" width="40%"/>
</p>

## Mandelbrot set

The Mandelbrot set is strongly related to the Julia sets, indeed we can define the Mandelbrot set $M$ as the set of complexes $c$ for which the corresponding Julia set $J_c$ is **connected **, that is to say, it is made of a single piece. We can say that the Mandelbrot set represents a map of Julia sets. And, contrary to the name it bears, it was the mathematicians Julia and Fatou who discovered it and who showed that the previous definition is equivalent to the set of points $c$ of the complex plane $\mathbb{C }$ for which the following sequence is bounded:
{: .text-justify}

$$
\left\{
  \begin{array}{ll}
    z_0 = 0 \\
    z_{n+1} = z_n^2 + c
  \end{array}
\right.
$$

This definition is very similar to that of the Julia set except that we are interested in the variable $c$. In the previous code, the line `z_reel = i * x_step + x_min` should be modified by `c_reel = i * x_step + x_min` and set `z_reel = 0` (same for the imaginary part). We get the following figure:

<p align="center">
  <img src="/assets/images/Artifacts/mandelbrot.png" width="40%"/>
</p>

**Note:** Benoît Mandelbrot is the founder of fractal theory with his article _"How Long Is the Coast of Britain? Statistical Self-Similarity and Fractional Dimension"_ in 1967. It is also he who obtained for the first time, a computer visualization of this set.
{: .notice--primary}

## Software

Fractal generation is not an easy task: many parameters can be taken into account and calculation times are often long. In the figures that I generated, we do not see at first sight the self-similar character of fractals, it would be necessary to change scale by zooming in deeper and deeper on a point of the plane.

There are many free fractal generator software available. They are often optimized for multi-processing or GPU computing, have a graphical interface to manage the many parameters and are sometimes capable of creating 3D objects (like the 3 displayed below). A fairly comprehensive list is available [here](https://en.wikipedia.org/wiki/Fractal-generating_software#Programs).

| ![image](/assets/images/mandelbulb3d.png) | ![image](/assets/images/mandelbulber.png) | ![image](/assets/images/fragmentarium.png) |
|:----------------------------------------------:| :-----------------------------------------:| :-----------------------------------------: |
| [Mandelbul3D](https://www.mandelbulb.com/2014/mandelbulb-3d-mb3d-fractal-rendering-software/)| [Mandelbuler](https://mandelbulber.com/) | [Fragmentarium](https://syntopia.github.io/Fragmentarium/get.html) |

And for those who do not want to complicate themselves and just let themselves be carried away by the psychadelic geometry of fractals without effort, you can find people on the internet who have already done the work for you. There are a slew of videos on youtube such as *The Hardest Trip - Mandelbrot Fractal Zoom* which zooms in for 2h30 on a specific point of the complex plane.

{% include video id="LhOSM6uCWxk" provider="youtube" %}

---

[![Generic badge](https://img.shields.io/badge/écrit_avec-Jupyter_notebook-orange.svg?style=plastic&logo=Jupyter)](https://jupyter.org/try) [![Generic badge](https://img.shields.io/badge/License-MIT-blue.svg?style=plastic)](https://lbesson.mit-license.org/) [![Generic badge](https://img.shields.io/badge/acces_au_code-github-black.svg?style=plastic&logo=github)](https://github.com/julienguegan/notebooks_blog/blob/main/fractale.ipynb) [![Generic badge](https://img.shields.io/badge/execute_le_code-binder-ff69b4.svg?style=plastic&logo=data%3Aimage%2Fpng%3Bbase64%2CiVBORw0KGgoAAAANSUhEUgAAAMYAAADGCAMAAAC%2BRQ9vAAACOlBMVEX%2F%2F%2F9XmsrmZYH1olJXmsr1olJXmsrmZYH1olJXmsr1olJXmsrmZYH1olL1olJXmsr1olJXmsrmZYH1olL1olJXmsrmZYH1olJXmsr1olJXmsq%2FdJX1olLVa4pXmsrmZYH1olL1olJXmspXmsrmZYH1olJXmsr1olJXmspXmsr1olJXmsr1olJXmsrmZYH1olL1olL1olJXmspXmsrmZYH1olL1olL1olJXmsrmZYH1olL1olL1olJXmsrmZYHqdnT1olJXmsq6dZf1olJXmsrKk3rmZYH1olJXmsrCc5RXmsr0n1TtgWz1olJXmspXmsrmZYH1olJXmsqNhq%2Fzmlj1olJXmspZmshXmsr1olL1olJXmsrmZYH1olJXmsr1olL1olL1olJXmsr1olJXmsrtgGz1olL1olJXmsr1olJXmsrmZYH1olJXmsrbaYf1olJXmsr1olJXmsr1olLIcJFXmsr1olJXmsr1olJXmsr1olJXmsr1olL1olJXmspZmshZmsldmsZemsVfl8Zgl8Zom71pk8Frm7tvm7dxkL1ykLx0m7R4m7F6jbh7jbh8nK6CnKmDirOEibOGnKaInKWNhq%2BNnKGSnZ2Vg6qegKaff6WfnZSnfKGnno6ofKGvnoeweZyxeZy3noG5dpjCcpPDcpPGn3bLb4%2FPoG%2FVa4rXoGnYoGjdaIbeaIXhoWHmZYHnaX7obXvpcHjqdHXreHLroVrtgGzuhGnuh2bxk17yl1vzm1j0nlX1olIgJPdZAAAAfnRSTlMAEBAQHx8gICAuLjAwMDw9PUBAQEpQUFBXV1hYWFtgYGBkZnBwcHFxdHx8fn6AgICHiIuQkJCSnKCgoKavsLCwsLO4uMDAwMDBwcTFxsjO0NDQ09TW1tjY3Nzd4ODg4uLl5%2Bjo6uvr7O3v8PDw8%2FPz9vb39%2Fj5%2Bfv7%2FPz9%2Ff5K%2BfZ5AAAI4ElEQVR42uzWAWfDQBjG8Yc4qoihEApBIIoOOpaiFAUBBB3EjFDKRImZy0d7vtuYYWN36Zq4u5v7fYO%2FB%2B%2BLwENBEARBEAR32Zc0gpcWRXmS%2FO7SHPI5PDIvaip01TrypKGlXr2B6%2FKaV%2BirGA67v%2FBa9dKrCLWXGA5anvhXlYBjopI36DdwStrxNo2AO%2Fa8WZ%2FBEaLhGHs4YdFxnGME%2B5KeY7UCtq160v%2BOFUn%2FOxLyH3QkPafSwhrxzukcYcsrp7SFHSWnlcGGnEOaQ57i0ywrqo4DpIB5QlLruI7w07w4U%2BsZ5j1R420n8Ju46qmxhmkZ1WQBJVHq6gUM66hUCujEJ3e%2B3YIqMsWQLZVmMCmSVDgLDEskFR5h0m7kLRatC3NEckSFosPCHA%2FqitEdMxjzwbxZN7eRNGG8tcpr%2BS2vA3KFmZODoFLlDaOS4%2FXxleVj9OqYacLMzMzYR%2BHsZwtz5hnvSNOSf%2F97Vc%2F0NI%2B%2FBwM0q%2FQJMsjoynXfYFr%2BPxe9SgtVijdiLT3Jjrmxlu5UIf5wlLq%2BraqTD9dfqbSjFrhY1T5jLNkzMdbRUMVy6nsqgdpYx4TKbMViHXA2bm%2BOJqoEY7QlNpVEfayDKoD3eqzhBSqNpqo4R7dcyJdjDX%2BHuW7Ouq%2BhshqCiG9yTfPDV%2FgmUWCvpLbCmSMzqsC3%2BSvWcInvEOUyZEeL5mtzxUQEfI9%2FYw3%2F8X2mZsuOVUVxEUDGP%2FwQeZ%2BSM7pSocrL8cNciDXwowQeJaWhQjK6RfwIFzU%2Fe5UfIxpiI0M%2B4npTmduWcZmfIJ%2FU1yshIxtxiTI46tZuZAxhTipDQ659yPACLksG5712IMMLuUwZHHriMuxVYBlXGBD50pHKXgWWEbNJh72MtKgKnMX%2Fxjq8KmZxrALXVNb%2BIV9TBQyAFS4mrFqFO4oNxMDHIUGV%2Bo0sGwDdHxvoT5ChcmNcL2ITl2INF9hAlKlGLz6VjXwSgxoXE%2BI7JRZvu7GJwO8Y63jRaMJRpGcCnlNJXqkgg6aGX3ij7K9Vuig2NQwYkvcNe4GhlMkzZCrOfSKbgQxDhpjGhvH7RNQfWzKLPUMi%2BeUTVEd%2Fwgc4fggtifc0Alkjm6SmeEd%2FivWgikHmGCC3bQoSqKCBsZamtKbXwuaoL4rdqQxUATYcmusQJjNHuikW227kWEvBS7YXH22qjgOQvwX24iDS%2BI%2FHe%2FQqasBtk4KveNoCXcDB%2B6NIC2IMsEc3%2FBl4o%2B7RIFZN5eETAw0T0%2FA74YOEAVW4aDU81pKx%2Bo%2BNpvp7BQ38UPdijKgXKQpxWfdZjCiOJhpluFXp6TFkolg5FXlgooFpafAiWFiNLsaQopMSvWAzwpweG5g7je9y5sgtztw5EUoPbRF%2FUOyhCw2LbMw1PrJnx9qV6gEr1%2B48MAf%2FDfZvJ66RJ0T3GHJi21KlZ%2Fn2U%2FhK1crNQ%2FoTZEKs5dia%2BcrEos2n5GpCFO0zdrv589sWqrZZtPu83FOREKaspO5xeo1KyPz156S2yDZxSldrn16tbHhUSFNaQAZ0Dezm5zcoS%2BZvPw8zRulkEzQJuIPbP1%2FZs%2BjYg85RVIZHiXScX6FKY%2FN5tyqADDJyr847tECVysITcdxUS5WTgf18iyqHvRbeLSgj9ZYqj%2BepHcjo8Lkql5dTVZfR4RtVPp%2Bn5GXIq8A6xPMGUFF9HR5r6Gb27i%2BVK94mV6BGHPOuskY%2BXhVA1wSZp1wyjtyQt%2FTxkcotncgJOTvnSP2o2mDxxp2Hjxxn5uNHDu%2FcuFi1wXdu3Ly%2F3W5%2BijKycs9xfpTjO5YoI6%2BSC3y2qXH7mQPoD6yhd6M5tA0iF0Ro1Kch1aowH%2Fbqz8DRRpiE%2FJwSmykUSEuj4Y4PIwrxsKjxVwWZIeUcwBx1CjIv1cY0uKZZIT4mB2SSP%2ByarQC%2FD4NjVPbbNuWzAiMePB3pogA%2FdnpkcIeu59MK0JoSeXcL6kNkjG866EKe5jg6%2FSpoDi%2Fhe8E6qMK0w8xQAh3Ngg9G8snC1O%2F%2Ft%2FjICKWnn0DPoc%2FlKaWnh0kF9092FrMln4wECRL4OBC1Uf55U2mpEUgdWh2vGI4xSP7gMKV3j%2FESTYfm3XwNPkUv4MTGQGG3WfbVZ%2BFe9hoMI6UfWr3%2BBHG7RsA7NMXEFJS3Rtk8msRZdLCbigRTuH2mrXpjZMF9BBkUm2OKuxUgFgKOsG%2BeDQQ2TUurw%2BUZFvLcKvU4y3Z9xRj4RABZtk6gC9Rw8uDWdeoeq7buO8lmDA39eIFEDipEwNFbnOUE5AjSBQU9qTawdEIy0CpVj%2BAa1R6zY6BY9Qo5IhO5U%2BGTiWeVBnKF70yHT0a6CsgQ0NGfMNDH6yR1CKgAvUsXalc6oiy1ibQM8kMx7xaQgfHyXA6hRy5lCJSJVrm7%2BjJw9Y2x%2B6%2F3morIIC%2FHpTDVo2R0Een%2FNGTtPb2gi1AWHQeJ0N%2FuZkVDKDnjgYxqC4lGeWTBbJEKFwvJcxLC%2FmRFCjTjcmRyBTYT5XyypCtom0TxR4XYDrksWYEHuV1JHC878%2BjJx3vzo7te86gUUq2Vibdg7bdq3aZdd9i0blUZP90PTj%2Fl0Z5gI5VCM%2FyUPI3OJq%2F9xBY1Jf94oytjCLkGiPUO6rlnlY5XSBjzo5fmlH2ssB%2Boi98q22uVekVpSVGlaLVfouJIIV%2BJWJWlloOZwcrCxWSoUXputGuHuLKEQBSGDwaDQmAxrVFtyuDaswB2UIs4a395ueKKCcyd7g4wSX%2B%2BxJ8cWequDpMVA8nVjsiGiIEsGzReWiUrhrr0SmQOtkQMZZUtxaIvdG4xWGJbMmizmW0eo1W2aTPECjsEw3n2qDi8Cpk9ajDezr66B4NfNoqyL2CGwrf0kPRfPpRv7ZjCKe9UMEngjdRilo23UYd5hHeJmEkGVIwgwyrW74iYL%2FEi9VhBVF5RHdbgKs%2FLBqswmWdtWElQnlEc1mKEH9MN63EHPyMGS%2FKfhIjFsnzmn6hYLM2myndKNFif2yvbymbxLWyUwlfHHgy%2BjfMp5eOHpOQtHo%2FH4%2FEY7x8MZ7AAyatDDgAAAABJRU5ErkJggg%3D%3D)](https://hub.gke2.mybinder.org/user/julienguegan-notebooks_blog-z8qd9bd5/notebooks/fractale.ipynb)