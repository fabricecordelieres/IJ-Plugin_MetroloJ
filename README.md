# The MetroloJ plugin

author:[Fabrice P. Cordelières](mailto:fabrice.cordelieres@gmail.com), [Cédric Matthews](mailto:cedric.MATTHEWS@univ-amu.fr)

<img src="https://github.com/fabricecordelieres/IJ-Plugin_MetroloJ/blob/main/img/logo_RT-MFM.jpg" width=300 align=center>    <img src="https://github.com/fabricecordelieres/IJ-Plugin_MetroloJ/blob/main/img/logo_mrct.jpg" width=150 align=center>

## Forewords
*This manual and plugin comes as a result of a collective work of the
"Metrology group" within the [French Technological Network of the
Multi-Dimensional Fluorescence Microscopies
(RT-MFM)](http://rtmfm.ibl.fr/), supported by the ["Mission Ressources
et Compétences Technologiques" (MRCT)](http://www.mrct.cnrs.fr/).\
The authors would like to thank:\
Aurélie Le Ru for reporting the time-shift bug.\
François Waharte (A COMPLETER) for beta-testing and bug tracking.\
Claude-Marie Bachelet and Aurélien Dauphin for the images used in fig.
[\[fig:gcoar-report\]](#fig:gcoar-report){reference-type="ref"
reference="fig:gcoar-report"}\
Aude Jobart for images of spinning disc PSFs\
The Metrology group\
**TO BE UPDATED***


## How to install the plugin ?

First, close ImageJ in case the software is already running. Copy and
paste the ***MetroloJ.jar*** file into the ImageJ/Plugins folder.
Download the ***iText library*** by following [this
link](http://prdownloads.sourceforge.net/itext/iText-2.1.5.jar). This
library will be used by the plugin to generate pdf reports. Copy and
paste it into the ImageJ/Plugins folder. Restart ImageJ.\
A ***MetroloJ*** entry should appear under the ImageJ's plugins menu. It
contains 2 entries:

---
## Generate PSF report

### How to generate the images ?

#### How to prepare the sample ?

*This sample preparation is aimed at obtaining an array of fluorescent
beads, well appart one from the other, and stably stuck to a coverslip.*

##### What do I need ?

***Fluorescent beads:*** their outer diameter should be below the
resolution of the system to test. Two types of beads might be used:

Mono-labelled beads. ex: Molecular Probes' [PS-Speck, Ref.
P-7220](http://probes.invitrogen.com/media/pis/mp07220.pdf). However,
their diameter of 0.17$\mu$m is a bit high for high NA objectives\...

Multi-labelled beads. ex: Molecular Probes' [TetraSpeck, Ref.
T-7279](http://probes.invitrogen.com/media/pis/mp07279.pdf). Their
diameter of 0.1$\mu$m is ideal, though having multi-labelled beads may
lead to weaker signals\...

***Regular slides***;

***Regular coverslips***: type 1.5 i.e. 0.17mm thick, either polylysine
coated or not;

If applicable, ***Poly-L-lysine solution*** (0.1 % (w/v) in $H_{2}O$)
ex: [Sigma-Aldrich, Ref.
P8920](http://www.sigmaaldrich.com/etc/medialib/docs/Sigma/generalinformation2/p8920.Par.0001.File.tmp/p8920.pdf);

***Ethanol***;

***Regular distilled water***;

***Mounting medium***: should be the same as for the regular, biological
samples. Avoid using DAPI containing mounting media;

***Nail polish***: only in case the mounting solution is not a setting
medium.

##### How do I do ?

Clean the coverslip and the slide using ethanol;

Dilute the polylysine stock solution to 1/5$^{th}$;

Poor a sufficient amount of the diluted polylysine solution onto the
coverslip surface (typically, 200$\mu$l for a 24x24mm coverslip) and
leave to coat for 15-30 minutes. The solution hardly covers the full
surface, so don't hesitate to use the pipet's tip to force the
polylysine into the coverslip's corners;

Rince the coverslip 2-3 times in distilled water, then drain off the
liquid;

Use a vortex to mix the beads' stock suspension;

Dilute the beads suspension in water. Here are some tips and facts:

Typically, 200$\mu$l for a 24x24mm coverslip will be requiered;

In general, the 0.1$\mu$m
[TetraSpeck](http://probes.invitrogen.com/media/pis/mp07279.pdf) should
be diluted to 1/1,000, the
[PS-Speck](http://probes.invitrogen.com/media/pis/mp07220.pdf) from
1/10,000 to 1/40,000;

It is advised to first dilute the beads to 1/1,000, then make further
serial dilution to achieve the appropriate beads' density on slide.

Tests should be done to define the appropriate dilution ratio;

Always vortex the suspensions between and after dilutions;

Poor a sufficient amount of the diluted beads suspension onto the
coverslip surface and leave to sediment for at least 30 minutes. The
solution hardly covers the full surface, so don't hesitate to use the
pipet's tip to force the suspension into the coverslip's corners;

Rince the coverslip 2-3 times in distilled water to remove the
unattached beads, then drain off the liquid;

Mount the coverslip on the slide by using the mounting medium. In case
of non setting media, the coverslip should be sealed onto the slide
using nail polish, while avoiding the latter to come under the
coverslip.

NB: Other methods exists in order to produce beads slides, involving
dilution in ethanol and leaving the suspension to evaporate. In this
case, tests of higher beads dilution might be required. Please note that
already prepared beads slides are commercially available (see
[TetraSpeck Fluorescent Microspheres Size Kit mounted on
slide](http://probes.invitrogen.com/media/pis/mp07279.pdf)). However,
this kind of preparation doesn't always match with the real *in situ*
resolution as the mounting medium might be different from the one used
in everyday acquisitions.

#### How to acquire the image ?

The following chart (see fig.
[\[fig:gpr-flowImg\]](#fig:gpr-flowImg)) summarises the procedure for optimal image
acquisition, in order to determine the resolutions on a confocal
microscope.


### What does it do ?

The plugin will generate a maximum intensity projection of the stack
along the z axis. The (x, y) coordinates of the maximum intensity pixel
(MIPix) are then collected. A XZ cross-section is generated, along a
line passing through the previously determined 2D MIPix. From this
image, the z coordinate of the MIPix is defined.

The z slice is set to the z MIPix coordinate. The x profile and y
profile are collected along the line passing through the MIPix. The z
profile is collected on the XZ view, along the line passing through the
MIPix.

All three profiles are fitted to a Gaussian, using the following
equation and ImageJ's built-in curve fitting function:
$$y = a + (b-a)*e^{\frac{-(x-c)^{2}}{2*d^{2}}}
            \label{eqn:gpr-gaussian}$$

The resolution i.e. the full-width at half-maximum (FWHM) is calculated
as follows for each profile, based on the parameters retrieved from the
fitting: $$FWHM=2d\sqrt{2ln(2)}
        \label{eqn:gpr-FWHM}$$

The theoretical resolutions are calculated as follows, depending on the
microscope's type: $$\begin{aligned}
            xy_{resol, wide-field}=\frac{0.61*\lambda_{emission}}{NA}
            \label{eqn:gpr-xyWF}\\
            z_{resol, wide-field}=\frac{2*\lambda_{emission}}{NA^2}
            \label{eqn:gpr-zWF}\\
            xy_{resol, confocal}=\frac{0.4*\lambda_{emission}}{NA}
            \label{eqn:gpr-xyConf}\\
            z_{resol, confocal}=\frac{1.4*\lambda_{emission}}{NA^2}
            \label{eqn:gpr-zConf}
        
\end{aligned}$$


### How to use it ?

Start ImageJ;

Open a stack containing exactly one bead;

Launch the plugin by going to Plugins/MetroloJ/Generate PSF report

In case the stack has not been spatially calibrated, a message error
pops up: click on Ok. In the calibration dialog box provide the
appropriate values, then re-launch the plugin;

The plugin's interface should appear (see fig.
[\[fig:gpr-interf\]](#fig:gpr-interf));

![image](img/gpr-interf.jpg)


Choose the microscope's type, enter the emission wavelength, the
numerical aperture of the objective and the pinhole aperture. Sample
informations and some comments might also be provided using the
appropriate boxes. Ticking the "Save image/plots/data" will generate:

a jpeg image of the side-views;

three files containing tabulation separated values of the x, y and z
profiles (excel files);

a file containing tabulation separated values of the report's summary.


Click on Ok: a new dialog box appears, inviting the user to choose a
folder where all data will be saved;

The pdf report is generated, and appropriate files saved.


## What's on the report ?

The PSF report is composed of two to three pages (see an example of
report on fig.
[\[fig:gpr-report\]](#fig:gpr-report)) depending on the informations provided by
the user. It is composed of up to 8 sections:

![image](img/gpr-report.jpg)

***Profile view***: image composed of three maximum intensity
projections, XY, XZ and YZ;

***Microscope infos***: summarizes informations about the acquisition
system and the image's calibration;

***Resolution table***: carries both the resolutions determined by
fitting and the theoretical, calculated ones;

***x profile and fitting parameters***: contains a plot of the intensity
profile along the x axis and the fitted curve. On the right side of the
graph stand the fitting parameters:

equation against which the profile is fitted;

number of iterations: self-explanatory;

sum of residuals squared: sum of the differences between the original
intensity values and the fitted ones, squared;

standard deviation: standard deviation of the residuals;

the correlation coefficient $R^{2}$ (gives indication on the fitting
goodness).

the Gaussian's constants a to d (see
[\[eqn:gpr-gaussian\]](#eqn:gpr-gaussian)), c being the position of the beads centre
along the x axis;

***y profile and fitting parameters***: same as previous along y axis;

***z profile and fitting parameters***: same as previous along z axis;

***Sample infos (optional)***: contains the informations entered by the
user in the "Sample infos" section of the interface;

***Comments (optional)***: contains the informations entered by the user
in the "Comments" section of the interface;
:::

## Known issues (to date\...) and workarounds (if any\...)

None\...yet.

---
## Generate co-alignement report

### How to generate the images ?

#### How to prepare the sample ?

*This sample preparation is aimed at obtaining an array of fluorescent,
multi-labelled beads, well appart one from the other, and stably stuck
to a coverslip.*

##### What do I need ?

***Fluorescent beads:*** their outer diameter should be well above the
resolution of the system to test. Two types of beads might be used:

Uniformly labelled beads. ex: Molecular Probes' [4$\mu$m TetraSpeck,
Ref. T-7283](http://probes.invitrogen.com/media/pis/mp07279.pdf).
However, their diameter is a bit small for low NA objectives\...

Non-uniformly labelled beads (inner core carrying one fluorescence,
outer ring another). ex: Molecular Probes'
[FocalCheck](http://probes.invitrogen.com/media/pis/mp07234.pdf).

***Regular slides***;

***Regular coverslips***: type 1.5 i.e. 0.17mm thick, either polylysine
coated or not;

If applicable, ***Poly-L-lysine solution*** (0.1 % (w/v) in $H_{2}O$)
ex: [Sigma-Aldrich, Ref.
P8920](http://www.sigmaaldrich.com/etc/medialib/docs/Sigma/generalinformation2/p8920.Par.0001.File.tmp/p8920.pdf);

***Ethanol***;

***Regular distilled water***;

***Mounting medium***: should be the same as for the regular, biological
samples. Avoid using DAPI containing mounting media;

***Nail polish***: only in case the mounting solution is not a setting
medium.
:::

##### How do I do ?

Clean the coverslip and the slide using ethanol;

Dilute the polylysine stock solution to 1/5$^{th}$;

Poor a sufficient amount of the diluted polylysine solution onto the
coverslip surface (typically, 200$\mu$l for a 24x24mm coverslip) and
leave to coat for 15-30 minutes. The solution hardly covers the full
surface, so don't hesitate to use the pipet's tip to force the
polylysine into the coverslip's corners;

Rince the coverslip 2-3 times in distilled water, then drain off the
liquid;

Use a vortex to mix the beads' stock suspension;

Dilute the beads suspension in water. Here are some tips and facts:

Typically, 200$\mu$l for a 24x24mm coverslip will be requiered;

In general, the beads should not be diluted much from 1/10 to 1/100;

Tests should be done to define the appropriate dilution ratio;

Always vortex the suspensions between and after dilutions;

Poor a sufficient amount of the diluted beads suspension onto the
coverslip surface and leave to sediment for at least 30 minutes. The
solution hardly covers the full surface, so don't hesitate to use the
pipet's tip to force the suspension into the coverslip's corners;

Rince the coverslip 2-3 times in distilled water to remove the
unattached beads, then drain off the liquid;

Mount the coverslip on the slide by using the mounting medium. In case
of non setting media, the coverslip should be sealed onto the slide
using nail polish, while avoiding the latter to come under the
coverslip.

NB: Other methods exists in order to produce beads slides, involving
dilution in ethanol and leaving the suspension to evaporate. In this
case, tests of higher beads dilution might be required. Please note that
already prepared beads slides are commercially available (see
[FocalCheck Fluorescent Microspheres Size Kit mounted on slide, Ref.
F-24633 (6$\mu$m beads) and Ref. F-24634 (15$\mu$m
beads)](http://probes.invitrogen.com/media/pis/mp07234.pdf)). However,
this kind of preparation doesn't always match with the real *in situ*
resolution as the mounting medium might be different from the one used
in everyday acquisitions.

#### How to acquire the image ?

The following chart (see fig.
[\[fig:gcoar-flowImg\]](#fig:gcoar-flowImg)) summarises the procedure for optimal
image acquisition, in order to determine the co-alignement quality on a
confocal microscope.


### What does it do ?

The plugin will generate two summed intensity projection of the stack
along the y and z axes;

On each projection, histogram segmentation is done on the log of
intensities, aiming at separating two populations of intensities
(background and signal);

Each projection is thresholded is order to highlight the "signal pixels'
population";

An ellipse is fitted to those pixels (i.e. to the bead's outline), and
the coordinates of its centre of mass is determined.

Once all coordinates have been retrieved for each channel, distance
between the centre from channel A and centre from channel B is
calculated (in fact, all combinaisons of distances are calculated):

$$dist_{A-B}=\sqrt{(x_{B}-x_{A})^2+(y_{B}-y_{A})^2+(z_{B}-z_{A})^2}
        \label{eqn:gcoar-dist}$$

For each couple of coordinates, a reference distance $r_{ref}$ is
calculated. This distance is quite easy to determine in 2D as it
corresponds to the xy resolution: while considering the centre of the
structure on image A, a structure of image B will be co-localized if it
is present within a circle traced around centre A of a radius equal to
the xy resolution. Due to the disparate resolutions over the three
dimensions, this distance is not so easy to calculate in 3D. However,
the answer might come from the observation of the factor limiting the
resolution: the PSF (Point Spread Function) and more precisely the first
Airy disc which might be approximated in 3D as having an ovoid shape
(see fig. [\[fig:gcoar-dist\]](#fig:gcoar-dist)).

![image](img/gcoar-dist.jpg)

Therefore in 3D, the reference distance is calculated by considering a
reference point and fitting a 3D ellipse around it for which the two
characteristic radii correspond to x/y and z resolutions. In this matter
changing from Cartesian coordinates to Polar coordinates make it more
easy to calculate the reference distance. The two characteristic angles,
the azimuth $\Phi$ and the zenith $\Theta$ (see expressions
[\[eqn:gcoar-phi\]](#eqn:gcoar-phi) and
[\[eqn:gcoar-theta\]](#eqn:gcoar-theta)) are first calculated, based on the
coordinates of the two centres to analyse. Knowing this orientation, as
well as the x, y and z resolutions ($res^\circ_{x}$, $res^\circ_{y}$ and
$res^\circ_{z}$ respectively), the distance from the reference centre to
the border of the ovoid shape **$r_{ref}$** is calculated (see
expression
[\[eqn:gcoar-refDist\]](#eqn:gcoar-refDist)). The inter-centre distance **$r$** is
then compared to this reference distance to assess if co-localization
occurs (see fig.
[\[fig:gcoar-dist\]](#fig:gcoar-dist)C) or not (see fig.
[\[fig:gcoar-dist\]](#fig:gcoar-dist)}B).

$$\begin{aligned}
        \Phi=\arccos\frac{x_{B}-x_{A}}{\sqrt{(x_{B}-x_{A})^2+(y_{B}-y_{A})^2}}\label{eqn:gcoar-phi}\\
        \Theta=\arccos\frac{z_{B}-z_{A}}{\sqrt{(x_{B}-x_{A})^2+(y_{B}-y_{A})^2+(z_{B}-z_{A})^2}}\label{eqn:gcoar-theta}\\
        r_{ref}=\sqrt{(res^\circ_{x}\times\sin\Theta\times\cos\Phi )^2+(res^\circ_{y}\times\sin\Theta\times\sin\Phi)^2+(res^\circ_{z}\times\cos\Theta)^2}\label{eqn:gcoar-refDist}
    
\end{aligned}$$

### How to use it ?

Start ImageJ;

Open two or three stacks containing exactly one bead;

Launch the plugin by going to Plugins/MetroloJ/Generate co-alignement
report

The plugin's interface should appear (see fig.
[\[fig:gcoar-interf\]](#fig:gcoar-interf));

![image](img/gcoar-interf.jpg)

Enter a title for the report, choose which stack should be used for
analysis (under "Stack 3", "none" might also be used), the microscope's
type, enter the emission wavelength for each stack, the numerical
aperture of the objective and the pinhole aperture. Sample informations
and some comments might also be provided using the appropriate boxes.
Ticking the "Save image/plots/data" will generate:

a jpeg image of the side-views;

a file containing tabulation separated values of all the numerical data
from the report.

Click on Ok: a new dialog box appears, inviting the user to choose a
folder where all data will be saved;

In case the stack selected under "Stack 1" has not been spatially
calibrated, a message error pops up: click on Ok. In the calibration
dialog box provide the appropriate values, then re-launch the plugin;

The pdf report is generated, and appropriate files saved.

### What's on the report ?

The co-alignement report is composed of two to three pages (see an
example of report on fig.
[\[fig:gcoar-report\]](#fig:gcoar-report)) depending on the informations provided by
the user. It is composed of up to 7 sections:

![image](img/gcoar-report.jpg)

***Profile view***: image composed of three maximum intensity
projections, XY, XZ and YZ;

***Microscope infos***: summarizes informations about the acquisition
system and the image's calibration;

***Pixel shift table***: considering as a reference the channel stated
at the beginning of each row, each column show how much pixels separate
one channel from the reference along x, y and z axis. This information
might be useful to compensate for chromatic aberration using image
processing softwares. On each row, resolutions and centre's coordinates
are given for the reference channel;

***Distance table (uncalibrated)***: contains distances calculated
between two channels, while not taking into account the actual images's
calibration;

***Distance table (calibrated)***: contains distances calculated between
two channels, while taking into account the actual images's calibration.
The distance printed between bracket is the reference distance (see
[3.2](#sec:gcoar-what){reference-type="ref" reference="sec:gcoar-what"}
and [\[eqn:gcoar-refDist\]](#eqn:gcoar-refDist));

***Sample infos (optional)***: contains the informations entered by the
user in the "Sample infos" section of the interface;

***Comments (optional)***: contains the informations entered by the user
in the "Comments" section of the interface;

### Known issues (to date\...) and workarounds (if any\...)

None\...yet.

---
## Generate axial resolution report

### How to generate the images ?

#### Prepare the sample

*This sample preparation is aimed at fixing a reflective surface on a
slide, overlaid by mounting medium, topped by a coverslip.*

##### What do I need ?

***Single reflector mirror:*** ex: Edmund optics' [4-6 Wave Mirror 20mm
x 20mm Enhanced Aluminum, Ref.
NT43-872](http://www.edmundoptics.com/onlinecatalog/displayproduct.cfm?productid=1754&showall).

***Ethanol***;

***Glue***;

***Regular slides***;

***Mounting medium***: should be the same as for the regular, biological
samples. Avoid using DAPI containing mounting media. Alternatively,
***immersion oil*** might also be used as a mounting solution;

***Regular coverslips***: type 1.5 i.e. 0.17mm thick;

***Nail polish***: only in case the mounting solution is not a setting
medium.
:::

##### How do I do ?

Clean the coverslip, the mirror and the slide using ethanol;

Glue the mirror to the coverslip. Warning: make sure that you glue the
glass side of the mirror ! Alternatively, setting mounting medium might
be used to stick the mirror on the slide;

Wait for the glue to set;

Mount the coverslip on the slide/mirror by using the mounting medium (or
immersion oil). Remove excess of solution by pressing firmly on the
coverslip. In case of non setting media, the coverslip should be sealed
onto the slide or the mirror (depending on its thickness) using nail
polish.

#### How to acquire the image ?

The following chart (see fig.
[\[fig:garr-flowImg\]](#fig:garr-flowImg){reference-type="ref"
reference="fig:garr-flowImg"}) summarises the procedure for optimal
image acquisition, in order to determine the axial resolution on a
confocal microscope.


### What does it do ?

After the user has defined a rectangular ROI, the plugin will generate
an average intensity projection of the image along the y axis.

The resulting 1D intensity profile is then fitted to a Gaussian, using
the following equation and ImageJ's built-in curve fitting function:
$$y = a + (b-a)*e^{\frac{-(x-c)^{2}}{2*d^{2}}}
            \label{eqn:garr-gaussian}$$

The axial resolution i.e. the full-width at half-maximum (FWHM) is
calculated as follows, based on the parameters retrieved from the
fitting: $$FWHM=2d\sqrt{2ln(2)}
        \label{eqn:garr-FWHM}$$

The theoretical resolutions are calculated as follows, depending on the
microscope's type: $$\begin{aligned}
            z_{resol, wide-field}=\frac{2*\lambda_{emission}}{NA^2}
            \label{eqn:garr-zWF}\\
            z_{resol, confocal}=\frac{1.4*\lambda_{emission}}{NA^2}
            \label{eqn:garr-zConf}
        
\end{aligned}$$

### How to use it ?

Start ImageJ;

Open the image of the z profile;

Launch the plugin by going to Plugins/MetroloJ/Generate axial resolution
report

In case the image has not been spatially calibrated, a message error
pops up: click on Ok. In the calibration dialog box provide the
appropriate values, then re-launch the plugin;

The plugin's interface should appear (see fig.
[\[fig:garr-interf\]](#fig:garr-interf));

![image](img/garr-interf.jpg)

Select the position of the ROI and adjust its width;

Choose the microscope's type, enter the emission wavelength, the
numerical aperture of the objective and the pinhole aperture. Sample
informations and some comments might also be provided using the
appropriate boxes. Ticking the "Save image/plots/data" will generate:

a calibrated jpeg image of the profile, including the ROI's outlines;

a file containing tabulation separated values of the z profile (excel
files);

a file containing tabulation separated values of all the numerical data
from the report.

Click on Ok: a new dialog box appears, inviting the user to choose a
folder where all data will be saved;

The pdf report is generated, and appropriate files saved.

### What's on the report ?

The axial resolution report is composed of two pages (see an example of
report on fig.
[\[fig:garr-report\]](#fig:garr-report)). It is composed of up to 6 sections:

![image](img/garr-report.jpg)

***Profile view***: a calibrated image of the profile, including the
ROI's outlines;

***Microscope infos***: summarizes informations about the acquisition
system and the image's calibration;

***Resolution table***: carries both the ROI's position, as well as
axial resolutions (experimental and theoretical);;

***z profile and fitting parameters***: contains a plot of the intensity
profile along the z axis and the fitted curve. On the right side of the
graph stand the fitting parameters:

equation against which the profile is fitted;

number of iterations: self-explanatory;

sum of residuals squared: sum of the differences between the original
intensity values and the fitted ones, squared;

standard deviation: standard deviation of the residuals;

the correlation coefficient $R^{2}$ (gives indication on the fitting
goodness).

the Gaussian's constants a to d (see
[\[eqn:gpr-gaussian\]](#eqn:gpr-gaussian)), c being the position of the beads centre
along the x axis;

***Sample infos (optional)***: contains the informations entered by the
user in the "Sample infos" section of the interface;

***Comments (optional)***: contains the informations entered by the user
in the "Comments" section of the interface;

### Known issues (to date\...) and workarounds (if any\...)

None\...yet.
