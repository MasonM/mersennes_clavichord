# Mersenne's Clavichord

[Zoo](https://zoo.dev/) project to create a 3D CAD model of the clavichord described by Mersenne in "Harmonie universelle" (1636) and expounded on by Peter Bavington in [Reconstructing Mersenne's Clavichord](https://www.peter-bavington.co.uk/Mersennepaper.pdf).

Uses [Mersenne's Laws](https://en.wikipedia.org/wiki/Mersenne%27s_laws) to calculate the sounding length of each string, and supports two different temperaments: quarter-comma meantone and equal temperment using Mersenne's proposed semitone ratio. Set `useEqualTemperament` in [temperament.kcl](./temperament.kcl) to switch between them. See the `slotXForKeyFn()` function in [rack_utils.kcl](./rack_utils.kcl) for the math used to calculate this.

Here's what it sounds like: https://open.spotify.com/album/7y2jMKdg6wnGTF2KrCVYo6?si=OVO8V7e3RBundObkuwff6A

# Rendering

See [main.stl](./main.stl)