# Mersenne's Clavichord

[Zoo](https://zoo.dev/) project to create a 3D CAD model of the [clavichord](https://en.wikipedia.org/wiki/Clavichord) described by [Mersenne](https://en.wikipedia.org/wiki/Marin_Mersenne) in ["Harmonie universelle"](https://en.wikipedia.org/wiki/Harmonie_universelle) (1636) and expounded on by Peter Bavington in [Reconstructing Mersenne's Clavichord](https://www.peter-bavington.co.uk/Mersennepaper.pdf).

The model uses [Mersenne's laws](https://en.wikipedia.org/wiki/Mersenne%27s_laws) to calculate the sounding length of each string, and supports two different temperaments: quarter-comma meantone and equal temperament using Mersenne's proposed semitone ratio. Set `useEqualTemperament` in [temperament.kcl](./temperament.kcl) to switch between them.

Here's what it sounds like: https://open.spotify.com/album/7y2jMKdg6wnGTF2KrCVYo6?si=OVO8V7e3RBundObkuwff6A

# How Does this Work?

## Clavichord Actions and Mersenne's Laws

A clavichord action is a simple [class 1 lever](https://en.wikipedia.org/wiki/Lever#Types_of_levers), where one end of each key has a piece of metal called a "tangent". When the other end of the key is pressed, the tangent rises and strikes the string. The distance between where the tangent strikes the string and the bridge is called the "sounding length" for that string. The sounding length, along with the tension and mass per unit length of the string, determines the [fundamental frequency](https://en.wikipedia.org/wiki/Fundamental_frequency) of the key being played. Mersenne was the first to discover and prove the mathematical relationship between these variables, which is now known as [Mersenne's laws](https://en.wikipedia.org/wiki/Mersenne%27s_laws). The usual form of this relationship is given as:
```math
f_0=\frac{1}{2L}\sqrt{\frac{F}{\mu}}
```

where $f_0$ is the fundamental frequency, $L$ is the sounding length, $F$ is the force, and $\mu$ is the mass per unit length.

In [The Science of Music](https://public.websites.umich.edu/~mejn/tsom/index.html), Mark Newman gives a more convenient form of this equation, assuming the strings are cylinders with constant density:
```math
f_0=\frac{1}{Ld}\sqrt{\frac{T}{\pi\rho}}
```

where $d$ is the string diameter, $T$ is the tension, and $\rho$ is the density. This is the version we'll use.

## Fretting and Temperaments

Clavichords can be divided into two groups: fretted and unfretted. A fretted clavichord has multiple keys that share the same string (or group of strings). An unfretted clavichord has dedicated strings for each key. Most early clavichords were fretted, since they were easier to build, due to requiring fewer strings. An obvious disadvantage of fretted clavichords is you can't play multiple keys that share a string at the same time, which instrument makers tried to mitigate by fretting keys that aren't typically played together.

Another disadvantage, which was of interest to Mersenne, is you can't change the [temperament](https://en.wikipedia.org/wiki/Musical_temperament) of the clavichord. In Mersenne's day, the most common temperament was [meantone temperament](https://en.wikipedia.org/wiki/Meantone_temperament). Mersenne was an early advocate of [equal temperament](https://en.wikipedia.org/wiki/Equal_temperament), which is the most common temperament used today.

With an unfretted clavichord, you can change the temperament by adjusting the tension on each string so the corresponding key is in tune. But with a fretted clavichord, changing the tension on a string affects all the keys that share that string, which means you can't tune each key independently. Sometimes you can work around that by bending the tangents sideways to alter the sounding length, but that quickly ruins the key levers. To reliably change the temperament on a fretted clavichord, you need to reposition the tangents and key levers.

## Solving for the Sounding Length

How does this model know where to position the key levers and tangents? First, let's revisit Mersenne's laws. We know that for fretted keys, we must keep the tension and density of the string constant for every key sharing that string, so let's solve for that. Let $f_0(x)$ be the fundamental frequency for the key at index $x$:
```math
\begin{aligned}
f_0(x)=\frac{1}{L(x)d}\sqrt{\frac{T(x)}{\pi\rho(x)}} \\
\sqrt{\frac{T(x)}{\pi\rho(x)}}=f_0(x)L(x)d
\end{aligned}
```

Now, let's calculate the sounding length for an adjacent fretted key at index $x+1$:
```math
\begin{aligned}
L(x+1) &=\frac{1}{f_0(x+1)d}\sqrt{\frac{T(x)}{\pi\rho(x)}}\\
       &=\frac{1}{f_0(x+1)d}f_0(x)L(x)d\\
       &=\frac{f_0(x)L(x)d}{f_0(x+1)d}\\
       &=\frac{f_0(x)L(x)}{f_0(x+1)}
\end{aligned}
```

## Solving for the Tangent Position

Once we have the sounding length, we need to use that to determine the position of the tangents. In this clavichord, the $x$ coordinate for each tangent is equal to the $x$ coordinate of the corresponding rack slot. The rack slot is the small slot in the rack at the back of the instrument that the key lever fits into. The slot position is related to the sounding length and the $x$ coordinate of the bridge:
```math
Slot_x(x) = Bridge_x(x) - L(x)
```

where $Slot_x(x)$ is the $x$ coordinate for the slot of the key at index $x$, $Bridge_x(x)$ is the $x$ coordinate for the bridge corresponding to the string for that key, and $L(x)$ is the sounding length.

 Substituing the equation for $L(x+1)$ above, we end up with:
```math
\begin{aligned}
Slot_x(x+1) &= Bridge_x(x+1) - L(x+1) \\
            &= Bridge_x(x+1) - \frac{f_0(x)L(x)}{f_0(x+1)} \\
            &= Bridge_x(x+1) - \frac{f_0(x)(Bridge_x(x) - Slot_x(x))}{f_0(x+1)} \\
\end{aligned}
```

This is the final equation implemented by the `slotXForKeyFn()` function in [rack_utils.kcl](./rack_utils.kcl).

# Rendering

See [main.stl](./main.stl)