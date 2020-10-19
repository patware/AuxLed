# Prototype for Auxiliary Lights

Info for the first prototype.

## Intro

4 auxiliary lights: 2 to spot things way in front at a distance (on highways for example), and 2 to spot things on the side of the road.

- Set 1 (left/right) for long distance (spot), mounted higher on the bike
- Set 2 (left/right) for sides (flood), mounted lower.

These lights will be controllable from a general switch on the handlebar, automatically from the bike's computer and from a phone.

- The handlebar switch will have an off/auto/on button, and a +/- intensity button.
- The bikes computer and sensors will also drive the lights
- An app on a mobile phone will do the same as above, but more finess and control.

### Pods

Each housing is a named a pod.  Each pod contains the LEDs and a controller.  The controller is responsible of handling which light is turned on, off or at what intensity.

### Main unit

The Main unit controller is the brain that instructs each pod individually, based on various input and settings.

### Features

In high beam mode - the bike is alone in the dark, and idea is prevent accidents as much as possible, obstacles far up front and animals on the side.

![High beam](images/spot%20versus%20flood.png)

In low beam, we don't want to affect others in front of us.

![High Low](images/spot%20high%20low%20beam.png)

Question: In low beam, what do we do with the top right ?  We want some light to detect people walking for example.

### Research

Research: in low beam, how to balance the viewing experience without affecting others.

Research: low beam scenario, the bike is approaching a hill, we want to see what's above the "low beam line".

Research: high speed versus low speed.  Should the lighting pattern vary with speed ?

Research: Windy roads.  In curves, should the inclination of the bike affect the lighting to reduce the inner-circle blind spot ?

## Build

Each pod will controlled individually, for features like turning, lane changing, etc.

Each pod will have their own set of LEDs, specs and power requirements.  Bikes typically run with 12VTo minimize the risk of interferences between the main unit and the pod, and for ease of programming, each pod will be connected to the main unit via a 3 wire cable: a 12V power, Gnd, Data.

### Pod Housing

Thinking of 3D printing, PETG resin.  But, if heat dissipation is needed, might be difficult perform.

![Housing](images/Housing%20proto%201.png)

Question: What's the relationship between the LED orientation with the lens ?  Should the lens control 100% (Single-Point Diamond Turning) light direction, or is there a combination between lens construction and LED location/orientation?

### Pod Lens

Polycarbonate seems the obvious choice for a lens.  The lens can be tailored for focused (spot for long distance) versus omni-direction (flooding).

Question: Can a 3D printer print polycarbonate lenses or is it better to buy directly from a manufacturer ?

### Pod LED pattern

Thinking of 3 LED layout patterns: Square, rectangle or round.

Will depend on the size of LEDs and throughput (Lumen), the lens capabilities versus LED orientation requirements.

- Square pattern: 1, 4, 9, 16  
- Rectangle patterns: 2, 6, 15
- Round: 1, 5, 12

![Patterns](images/Led%20layout%20patterns.png)

### LED questions

Many questions around LEDs

#### Heat

Question: Do LEDs generate a lot of heat ?  If so, need to find a way to include heat dissipation in the housing design.

#### Intensity

Question: Are LED variable intensity or fixed ?  If fixed, need to find a way to turn on/off LEDs individually to simulate gradation.

Partial answer: from CREE's literature (bellow), the input voltage (V) affects the current consumed (mA), and the current consumed (mA) affects the relative light intensity.

- 2.6V = 100 mA = 10% intensity
- 2.8V = 1000 mA = 100% intensity
- 3.1V = 2500 mA = 200% intensity

Question: What 200% intensity mean ?

#### LED life

At full intensity, what is the LED's life ?  What affects the LED's light? Does the LED loose lumens gradually over time ?  Might want to think about having a round-robin of active LEDs

## Hardware

### Cree XLamp XP-L LEDs

The specs for the LEDs from [cree.com](https://www.cree.com/led-components/media/documents/ds-XPL.pdf).

- CCT: Chromacity
  - Warm White: 2700K - 3500K
  - Neutral White: 3750K - 5000K
  - Cool White 5700K - 6200K
- Flux: Light output, in Lumens

Thinking of Kit 51, CCT 6200K

## Pod Data wire

The data wire will be used to instruct the pod, something like 8 bits instruction.

- Intensity (bits 0..2)
  - 0 - 000: off
  - 1 - 001: 15%
  - 2 - 010: 30%
  - 3 - 011: 45%
  - 4 - 100: 60%
  - 5 - 101: 75%
  - 6 - 110: 90%
  - 7 - 111: 100%
- Flash (bit 3..4)
  - 0 - 00: off
  - 1 - 01: slow (flasher speed)
  - 2 - 10: medium (warning, get attention)
  - 3 - 11: high (scare)
- Side (bits 5..6)
  - 0 - 00: off
  - 1 - 01: 33%
  - 2 - 10: 66%
  - 3 - 11: 100%
- Side flash (bit 7)
  - 0 - 0: off
  - 1 - 1: slow (flasher speed)
