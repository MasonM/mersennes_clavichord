# Mersenne's Clavichord

[Zoo](https://zoo.dev/) project to create a 3D CAD model of the [clavichord](https://en.wikipedia.org/wiki/Clavichord) described by [Mersenne](https://en.wikipedia.org/wiki/Marin_Mersenne) in ["Harmonie universelle"](https://en.wikipedia.org/wiki/Harmonie_universelle) (1636) and expounded on by Peter Bavington in [Reconstructing Mersenne's Clavichord](https://www.peter-bavington.co.uk/Mersennepaper.pdf).

The model uses [Mersenne's laws](https://en.wikipedia.org/wiki/Mersenne%27s_laws) to calculate the sounding length of each string, and supports two different temperaments: quarter-comma meantone and equal temperament using Mersenne's proposed semitone ratio. Set `useEqualTemperament` in [temperament.kcl](./temperament.kcl) to switch between them.

Here's what it sounds like: https://open.spotify.com/album/7y2jMKdg6wnGTF2KrCVYo6?si=OVO8V7e3RBundObkuwff6A

# How Does this Work?

See [Modeling Mersenne's Clavichord](https://www.masonm.org/posts/meresennes-clavichord/) for details.

# Rendering

See [main.stl](./main.stl)