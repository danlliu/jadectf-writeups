# Heritage
## OSINT (50 pts)

Writeup by danlliu (WolvSec)

## Challenge

We are given an image, and asked the following questions:

```
What are those protuding like structures?
What is this place?
How old are they?
Can you help me figure this out?
Flag: jadeCTF{name_place_age}
Replace spaces, if any, in name, place and age with underscore.
```

## Solution

We are given the following image:

![heritage](heritage_assets/IMG_20220105_132500.jpg)

Running `exiftool` gives us quite a lot of information; the most important is:

```
GPS Position                    : 23 deg 48' 48.93" N, 86 deg 26' 28.55" E
```

From here, we see that the image was taken at the Oval Ground on IIT's campus, next to a fossil called "Petrified Wood". Checking on IIT's website (https://www.iitism.ac.in/index.php/pages/hotspots) shows that the wood is 2.5 million years old.

`jadeCTF{Petrified_Wood_Oval_Ground_2.5_million_years}`
