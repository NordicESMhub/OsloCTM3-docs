.. role:: math(raw)
   :format: html latex
..

.. role:: raw-latex(raw)
   :format: latex
..

Oslo CTM3 manual
================

The Oslo CTM3 is a three dimensional global chemical transport model
(CTM), and this manual explains how to use it. This manual also
describes the model, and is meant as a thorough description of the
model.

Generally, the use of the model is described in
Section [sxn:getstarted]–[sxn:programming], while the rest is describing
the different processes and the model code.

The Oslo CTM3 is available through a repository, as will be explained in
the coming sections.

First, some starting info about the Oslo CTM3.

Historical background
---------------------

Oslo CTM3 is developed at the Department of Geosciences at the
University of Oslo (UiO), and later at the CICERO Center for
International Climate Research. It is described by
:raw-latex:`\citet{SovdeEA2012}` and is an updated version of the
Oslo CTM2, which was based on a CTM from University of California,
Irvine (UCI), developed by Michael Prather & Xin Zhu (January 1996,
p-code 1.0/96), and where the chemical modules developed at the UiO were
included.

In roughly a decade the two models diverged substantially; the
Oslo CTM2 grew with additional chemistry/aerosol packages, while the UCI
group developed and improved the efficiency of the transport. In order
to save CPU hour requirements, the UCI group (Prather *et al.*) updated
their CTM to improve the structure, parallelisation and transport in
2007. Due to the large CPU requirements for the Oslo CTM2, it was
decided to implement the new UCI structure in the Oslo model, earning
the name Oslo CTM3.

By the end of 2007, the Oslo CTM2 was very messy, so the update of the
core structure also initiated a clean-up of the Oslo CTM2 code.

Version numbering
-----------------

The Oslo CTM3 documented by :raw-latex:`\citet{SovdeEA2012}` is to be
considered version v0.1, while the current version is v1.0. Version
numbering is not directly related to repository revision numbers,
however, in the Oslo CTM3 repository, the version documented by
:raw-latex:`\citet{SovdeEA2012}` was r143.

There are numerous small changes from version v0.1 to v1.0, e.g. that
the aerosol packages have undergone some revisions, and the scavenging
tables have been modified.

Oslo CTM3 vs the UCI CTM
------------------------

During the first five years of Oslo CTM3 development, the basic idea of
the Oslo CTM3 was to keep the UCI model core untouched, only adding
modules we want and thereby staying away from interfering too much with
the core.

However, in 2015, the UCI CTM had already diverged some bit from the
starting point of Oslo CTM3, being partly rewritten to use modules
instead of common blocks. So I decided to rewrite the Oslo CTM3 into
Fortran90. This was done for all files except the transport routines,
which are the basis for the transport model. Keeping those intact may be
wise in case of future UCI updates. However, all the common blocks are
now gone.

The model driver () is kept clean and short; the Oslo CTM2philosophy
that all goes into  is left for good!

So far the Oslo CTM3 changes have been large enough so it is not
possible to use the UCI chemistry just by operating a switch. The
original UCI code files are available through the UCI group, and will
not be available in CTM3. I do, however, have their older codes if
update history is needed.

Limitations to Oslo CTM3
------------------------

Oslo CTM3 is a good tool to study the atmospheric processes. However,
care should be taken when comparing model results with observations.
Even a high resolution in the Oslo CTM3 is actually fairly coarse, so
a perfect fit with observations, especially close to the Earth surface,
should not be expected.

The Oslo CTM3 is primarily set up to study current meteorology, so when
using it in pre-industrial conditions you have to make assumptions.

Get started
===========

The Oslo CTM3 is currently available through a SVN repository (see
Section [sxn:thesource]), and to get access to the model you have to
have a user account at the Department of Geosciences or at CICERO, and
you have to be member of the unix group ``gf-ozone``. This will change
eventually.

I have planned to eventually make the Oslo CTM3 open source, but that is
yet for the future.

When you are well into the model, you may need some utilities or
programs (for generating e.g. input files). Such CTM-files may not be
part of the SVN, and may otherwise be located at
*/div/amoc/d4-4/oslo-ctm/archive\_ctm/*.

Part of the SVN, though, are some tools for post processing. More of
this later.

In this Section you will find a short description of how to download the
model from the SVN repository, a short model overview and how to get it
running.

How to get the model – SVN
--------------------------

The Oslo CTM3 is available through the UiO central subversion system
(SVN). To access the code you have to be member of the group
``gf-ozone``.

SVN is a version control system, and if you are unfamiliar with it, the
SVN book offers a fairly good introduction to it:
*http://svnbook.red-bean.com/*.

To use SVN you need to have SVN installed/loaded. Contact your IT
services if you need help on this.

Important SVN commands are ``checkout``, ``add``, ``delete``,
``commit``, ``status``, ``diff``, ``resolve`` (for versions prior to 1.5
it is called ``resolved``), ``merge``.

You will find more about SVN in the SVN book, and a short summary in
Appendix [app:svn]. The repository structure is described in
Section [app:repository], where you will also see that there also are
some plotting utilities and other tools available.

| **Fetching the code**
| The code is located in a repository, and you get it by typing (in
  shell scripts, ’\ ``\``\ ’ allows you to write a command on several
  lines; you should skip character that and write everything on one
  line):

::

    svn checkout \
      svn+ssh://svn.uio.no/svnroot/osloctm3/ctm3f90 \
      <name of dir to put the code>

After doing this, you have a copy of the code in the directory you
specified. In the *.svn* directory the original revision files are also
available, but you will not use those directly; *.svn* is for SVN to
use.

If you don’t specify the name of directory where the code will be copied
to, it will get the repository name, i.e. \ *ctm3f90*.

| **Update an existing code**
| If one of the experienced users tell you to update your code, just go
  to your directory and execute the command:

::

    svn update

Note that if you have modified a file that needs update, you may
experience a conflict, i.e. SVN does not understand how to merge the
changes. If you are unfamiliar with this, ask the experienced users.

User manual
-----------

The Oslo CTM3 user manual (this document) is available as a pdf file
(*manual\_osloctm3.pdf*) in the model directory. The LaTeX source code
is located in a separate repository.

Source code directory
---------------------

When you have downloaded the source code, you find that it contains
several files and some directories. The transport source codes, i.e. the
UCI heritage of the model, are located at the top level of the
directory. Most of the files have the extension *.f90*, except for a few
of the transport subroutines which have extension *.f*.

All the specific Oslo chemistry/physics files are put in the directory
*OSLO*. You can find more about the directory structure in
Appendix [app:repository], and in Appendix [app:description\_of\_files]
you can find more information on the files.

Model grid
----------

As already noted, the Oslo CTM3 is a global model. It is divided into
grid boxes to cover the atmosphere and hence has a certain resolution.
When you specify a resolution, there are three numbers to keep in mind,
at least for ECMWF meteorology data. The truncation number is given by
’T’ (e.g. T159), indicating the spectral resolution of the native model
(e.g. ECMWF IFS). The vertical resolution is given by ’L’ (L60).
A forecast model can have both gridded data and spectral data, in
different resolutions, so to define the gridded data there is the ’N’
number (N80). At ECMWF the T and N numbers are closely connected. See
Appendix [app:metdata] for more.

In the Oslo CTM3 the spatial resolution is controlled by the following
parameters:

``IPAR``: Number of grid boxes in the zonal (east-west) direction.
``IPARW`` is the zonal resolution of the meteorological input data and
differs from ``IPAR`` if you run with “degraded” horizontal resolution
(see Section [sxn:degraded\_res] for more on degradation of resolution).

``JPAR``: Number of grid boxes in the meridional (north-south)
direction. ``JPARW`` is the meridional resolution of the meteorological
input data and differs from ``JPAR`` if you run with degraded horizontal
resolution (see Section [sxn:degraded\_res] for more on degradation of
resolution).

``LPAR``: Number of grid boxes in the vertical. If you collapse layers,
the vertical resolution of the meteorological input data is ``LPARW``
(see Section [sxn:degraded\_res] for more on collapsing layers).

There are several variables describing the grid. The horizontal grid is
set up at model start, with grid center longitudes defined in:

``XGRD(IPAR)``: Radians.

``XDGRD(IPAR)``: Degrees.

And grid center latitudes:

``YGRD(JPAR)``: Radians.

``YDGRD(JPAR)``: Degrees.

The grid edges are defined similarly by:

``XEDG(IPAR+1)``: Eastern edge longitude of grid box (radians).

``XDEDG(IPAR+1)``: Eastern edge longitude of grid box (degrees).

``YEDG(JPAR+1)``: Southern edge latitude of grid box (radians).

``YDEDG(JPAR+1)``: Southern edge latitude of grid box (degrees).

The vertical grid, however, is not fixed throughout a model run. It
depends on the meteorological input data, more specifically the
pressure. For a model layer :math:`L`, the pressure at the grid box
bottom is given by

.. math:: p_b(L) = \eta_a(L) + \eta_b(L)\,p_s

 where :math:`p_s` is surface pressure in hPa and :math:`\eta_a`
(``ETAA(LPAR+1)``, units of hPa) and :math:`\eta_b` (``ETAB(LPAR+1)``,
unitless) are hybrid sigma coordinates. If you collapse layers, the
original sigma coordinates are given by ``ETAAW(LPARW+1)``) and
``ETABW(LPARW+1)``.

The grid box center pressure of a level :math:`L` is halfway between the
edges of the box:

.. math::

   \begin{aligned}
     p_c(L) &=& \frac{1}{2}\left[\eta_a(L) + \eta_a(L+1)\right. \nonumber\\
                   && +\left.(\eta_b(L)+\eta_b(L+1))*p_s\right]\end{aligned}

 The height of grid box bottoms are given by the array
``ZOFLE(LPAR+1,IPAR,JPAR)``. Surface values of this variable act as
topography, and are calculated from a 2D annual mean surface pressure
field :math:`p_m`:

.. math:: Z_s = 16000\log10\left(\frac{1013.25\textrm{hPa}}{p_m}\right)

 The height levels above the surface (i.e. for the vertical range
``2:LPAR+1`` of ``ZOFLE``) are calculated from the thickness of each
layer, based on temperature (:math:`T`), specific humidity (:math:`q`)
and pressure at grid box edges (:math:`p_b`).

.. math::

   \begin{aligned}
     \Delta Z(L) &=& -29.27 T(L)\left[1-0.6q(L)\right] \nonumber \\
              && \cdot \log\left(\frac{p_b(L+1)}{p_b(L)}\right)\end{aligned}

 Although :math:`Z_s` is the topography in meters, this quantity is
mainly used for diagnostics. Physical processes generally use ``ZOFLE``
to find layer thickness, while it is the surface pressure that defines
the topography. See Appendix [app:layerheights] for some more info.

Degraded resolution
~~~~~~~~~~~~~~~~~~~

The Oslo CTM3 can be run with lower resolution than the meteorological
input data. It is possible to collapse as many layers as you like,
however, *Makefile* allows for only one automatic set-up, collapsing
layer 1–3 and 4–5 into two layers. See Section [sxn:makefile] for
*Makefile* user settings. While this is standard treatment by the
UCI group, the Oslo CTM3 has so far not been used in this fashion.

A newer feature to Oslo CTM3 is that the horizontal resolution may be
degraded, combining e.g. 4 boxes into one. This means that the model can
read e.g. 1.125\ :math:`^{\circ}`\ x1.125\ :math:`^{\circ}`
meteorological data, and convert them to
2.25\ :math:`^{\circ}`\ x2.25\ :math:`^{\circ}` resolution, but still
using the native resolution for calculating cloud properties. The
*Makefile* settings are:

``HWINDOW=HORIGINAL``: Use native resolution.

``HWINDOW=HTWO``: Combine 2x2 native boxes.

``HWINDOW=HFOUR``: Combine 4x4 native boxes.

Also described in Section [sxn:makefile].

Oslo CTM3 vs Oslo CTM2
----------------------

Skip this part if you are not familiar with Oslo CTM2.

The structure of the model has changed substantially since Oslo CTM2. In
the Oslo CTM2 all tracers were located in the tracer array ``STT``,
whereas in Oslo CTM3 only the transported tracers are in ``STT``.
Therefore also ``MTC`` is now only for the transported species. Other
variables have also been changed to separate the transported and
non-transported species. The non-transported species are stored in
``XSTT``, and will be further explained in Section [sxn:sourcecode].

The parallel structure has changed, and most processes (chemistry,
boundary layer mixing, emissions, etc.) are now integrated columnwise
instead of through a latitude band. Horizontal transport, however, is
calculated layer by layer (see Section [sxn:transport]).
Section [sxn:sourcecode] describes the source code more closely.

| **Input files and diagnostics**
| The diagnostics have changed, as well as the names of the input files.
  The input files are described in Section [sxn:setup], while
  diagnostics are described in Section [sxn:diagnostics].

| **C-code**
| The C-coding in the Oslo CTM2 has been removed, and replaced with
  dummy calls and logical switches. This will be explained thoroughly in
  this manual.

| **Changes in chemistry**
| There are some changes in the tracer list since Oslo CTM2:

Tracer number 2 (``NOX``), the sum of NOx components (NO,
NO\ :math:`_2`, NO\ :math:`_3`, 2xN\ :math:`_2`\ O\ :math:`_5`\ +PAN) is
only set in the tropospheric chemistry for stability. There is no need
to transport NOX since all its components are transported.

Tracer number 3 (``NOZ``), the sum of NO\ :math:`_3` and
N\ :math:`_2`\ O\ :math:`_5` is removed. It is now set in the
tropospheric chemistry, as was done already in the stratospheric
chemistry. NOZ is treated in chemistry to create stability, and does not
need to be transported as long as NO\ :math:`_3` and
N\ :math:`_2`\ O\ :math:`_5` are transported.

Tracer number 26 (``CH2O2OH``) was transported but not used. It is
removed.

Tracer number 45 (``O3NO``) is set inside the tropospheric chemistry
from O\ :math:`_3` and NO, also for stability. It was not transported,
not used in the stratosphere, and the diagnose was not useful, therefore
it was removed.

Tracer number 47 (``DMS``) is not in use in the tropospheric chemistry,
and is removed. It is used in the sulphur scheme where it has a new
number (which is 71).

| **NPAR**
| In contrast to the Oslo CTM2 the ``STT`` now *only handles the
  transported species*, given by ``NPAR``. Non-transported species are
  treated as a separate array ``XSTT``, of which there are ``NOTRPAR``.
  See Section [sxn:coresource] for more on this.

| **Convective activity**
| The convectivity files used for lightning emission in Oslo CTM2 are
  not needed in the Oslo CTM3. This is because the lightning routine is
  now more consistent with the meteorological data, not using the
  :raw-latex:`\citet{PriceEA1997a}` dataset.

In the Oslo CTM3 we calculate a somewhat similar convective activity at
each time step and scale it against a climatological mean. This mean is
specific for a certain meteorological dataset, and is sensitive for
resolution. An important difference to the Oslo CTM2 is that the
Oslo CTM2 divided the annual amount following the monthly totals of
:raw-latex:`\citet{PriceEA1997a}`, whereas Oslo CTM3 more physically
follows only the meteorological conditions. It could be noted that
:raw-latex:`\citet{MurrayEA2012}` argue that Northern Hemisphere summer
lightning produce more NOx than elsewhere and at other seasons, but this
is not included in Oslo CTM3. See Section [sxn:emissions\_lightning] for
more on lightning emissions.

| **fast-JX**
| The Fast-J2 :raw-latex:`\citep{BianPrather2002}` applied in the
  Oslo CTM2 has been replaced by fast-JX in the Oslo CTM3. There are
  some differences in the photochemistry, e.g. slight differences in
  cross sections.

| **Source code documentation**
| In the process of cleaning up the Oslo code, also the source code
  comments have been revised and improved. See Section [sxn:programming]
  for programming guidelines.

| **Variable names**
| There are several variable names that have been renamed from
  Oslo CTM2 to Oslo CTM3. The new variable names are more
  self-explaining.

A good example of a changed variable name is the tropopause level, which
is now called ``LMTROP``, since it is the “``LM`` of the troposphere”.
In the old model its name was ``LMSTRT``, which was somewhat misleading.

Another is the max number of component IDs, which has been changed from
``IREPMX`` to ``TRACER_ID_MAX``.

The tracer mapping from chemical id to transport number has changed name
from ``MTC`` to ``trsp_idx``. Similarly, the mapping the other way is
changed from ``IDMTC`` to ``chem_idx``.

``TMMVV``, the conversion factor from mass mixing ratio to volume mixing
ratio, is called ``TMASSMIX2MOLMIX``. There are also mappings the other
way, namely ``TMOLMIX2MASSMIX``.

Old variable names are not listed in the index list, so if you wonder
where to find the old variable, ask the experienced users.

| **Other differences**
| In the Oslo CTM2 the whole tracer array (``STT``) was often converted
  from one unit to another unit, just to access a few tracers in the
  correct units. This is no longer possible, and should be avoided.

Other differences are noted when necessary in the other sections.

Variable names
--------------

Most of the model variable names referred to in this manual are listed
in the index list at the end of the manual, under “variables”. If you
cannot find the variable names there, they are probably not mentioned
here, and you have to look in the model files. A good place to start is
the global variables in the files called ``cmn_*`` (cmn for common files
instead of common blocks).

If you look for a variable and do not find it in the indexed list, you
may also do a text search in this document; the variable may not have
been included in the index list.

Setting up the model
--------------------

This section will explain the steps to get the model running, and will
describe some important files.

You need to know the

Makefile

pmain.f90

input file (*LxxCTM.inp*), which lists some important flags and input
file names.

tracer list (*tracer\_list.d*)

wet scavenging list (*scavenging\_wet.dat*), which lists how to treat
wet scavenging of tracers.

dry scavenging list (*scavenging\_dry.dat*), which lists how UCI treats
dry deposition of tracers. NOT used for Oslo chemistry! Oslo chemistry
uses the file *drydep.ctm*, located in the directory *Input\_CTM3*.

meteorological data

other input data, e.g. how to treat CH\ :math:`_4` at the surface (and
possibly how to initialize the tracer array ``STT``).

*Makefile*
~~~~~~~~~~

The first file you need to know, is the *Makefile*. In *Makefile* you
can set user options, i.e. which modules or packages to apply, which
resolution to use and some compiler options. Your choices are:

``OPTS``: Optimize (``O``, ``A``) or debug (``D``).

``HNATIVE``: Horizontal resolution of meteorological data, e.g. T42,
T159.

``VNATIVE``: Vertical resolution of meteorological data, most likely
L60, but old files exist in e.g. L40.

``HWINDOW``: Model degradation of horizontal resolution. Setting it to
``HORIGINAL``, the model uses native resolution, and with ``HTWO`` it
combines 2x2 native grid boxes. A third option is ``HFOUR``.

``COLLAPSE``: Collapse layer 1-3 and 4-5 into two layers.

``OSLOCHEM``: Turn on Oslo chemistry/physics. For Oslo CTM3 you would
most likely never turn this off.

``TROPCHEM``: Oslo tropospheric chemistry (Section [sxn:tropchem]).

``STRATCHEM``: Oslo stratospheric chemistry (Section [sxn:stratchem]).

``SULPHUR``: Sulphur chemistry and sulphate (Section [sxn:sulphur]).

``BCOC``: Black and organic carbon package (Section [sxn:bcoc]).

``NITRATE``: Nitrate package (Section [sxn:nitrate]).

``SEASALT``: Sea salt package (Section [sxn:salt]).

``DUST``: Mineral dust package (Section [sxn:dust]).

``SOA``: Secondary organic aerosols package (Section [sxn:soa]).

``E90``: Turn on to use e90 tracer for STE flux calculations
(Section [sxn:ste]) and to produce the tropopause ``LSTRATAIR_E90`` (not
yet used to distinguish tropospheric and stratospheric chemistry).

``LINOZ``: Turn on to use Linoz O\ :math:`_3` for STE calculations.
Oslo CTM3 has not yet been set up to use Linoz to replace stratospheric
chemistry.

``LIT``: Can be used for generating lightning factors. Explanation is
given in *Makefile*.

``EMISDEP_TREATMENT``: Defines whether to treat emissions and deposition
as separate processes (``EMISDEP_TREATMENT :=U``) or as respectively
production and loss in chemistry (``EMISDEP_TREATMENT :=O``).

``FC``: Fortran compiler (``ifort``, ``pgf90``, ``openf90`` ...) See
Appendix [app:compiler] for more on the compiler options.

*Makefile* does *not* use a dependency generator, so if you add files to
be compiled you have to add dependency rules at the end of *Makefile*.
How to do this is described closer in Appendix [app:makefile].

The *Makefile* tokens set up the compilation and is used for setting
model parameters. The file *cmn\_size.F90* is the only file (except the
mineral dust code) containing C-style code, and thereby needs to be
preprocessed by the Fortran compiler.

See Section [sxn:getstarted\_compiling] if you don’t know how to
compile.

If you want to run non-standard resolutions, you need to check that the
*cmn\_size.F90* has the necessary parameters.

See Appendix [app:makefile] for more on *Makefile*.

Main program – 
~~~~~~~~~~~~~~~

The main program is located in . It is described in
Section [sxn:structure]. You should know the structure of , and how it
works. When you need to work on the tracer arrays, you should also know
how the parallel regions work.

Input file – *LxxCTM.inp*
~~~~~~~~~~~~~~~~~~~~~~~~~

There is one input file for each resolution, and it is typically called
*LxxCTM.inp*. The first part of the file is to set up the run, with
information about the date, time steps, meteorological fields, and
physical processes (boundary layer mixing, dry and wet deposition
schemes).

The second part lists some input file names, e.g. the tracer list file
(*tracer\_list.d*), while the third part covers information about the
diagnostics (covered in Section [sxn:diagnostics]).

The important parameters to set are

``IYEAR``: Reference year.

``NDAYI``: Day of year to begin CTM run.

``NDAYE``: Day at end of CTM run (finishes at end of day ``NDAYE-1``).

``LCLDQMD``: Use mid-point of quadrature cloud cover independent cloud
atmospheres (ICA) (for fast-JX, Section [sxn:cloudcover]).

``LCLDQMN``: Mean quadrature cloud cover ICAs (for fast-JX).

``LCLDRANA``: Random selected from all cloud cover ICAs (for fast-JX;
default treatment).

``LCLDRANQ``: Random selected from 4 mean quadrature cloud cover ICAs
(for fast-JX).

``RANSEED``: The seed number to create random numbers. Ensures that the
random cloud properties are the same in two runs).

``NROPSM``: Number of operator split steps per meteorological time step.
Default is 1hour, i.e. \ ``NROPSM=3``. Note that the meteorological
steps per day (``NRMETD``) is hard coded into the model because it is
necessary for e.g. the diagnostic tools.

``NRCHEM``: Number of chemical sub steps per operator split step.
Default is ``NRCHEM=1``.

``LJCCYC``: Flag for calculating J-values every internal chemical
cycling step. See Section [sxn:photochemistry] for more.

``LMTSOM``: Second order moment limiter. Should be 2 (monotonic), but
can also be set to 1 () and 3 (min/max). Do **not** change this unless
you know what you are doing.

``CFLLIM``: Global CFL limit for divergence, i.e. max allowed amount
taken from a grid box in advection. This value should be set to 0.95,
and you should not need to change it. With the Oslo CTM2 there were
a few instances where certain meteorological data would cause the model
to crash because 0.95 was too high, this behaviour has so far not been
seen in Oslo CTM3.

Next, there is a section for meteorological data, specifying which type
of data and where to read them:

``metTYPE``: Possible types are ``ECMWF_oIFS`` for OpenIFS generated
locally at CICERO/UIO, and ``ECMWF_IFS`` for the IFS data generated at
ECMWF/UIO.

``metCYCLE``: The cycle of the ECMWF IFS/OpenIFS model.

``metREVNR``: The revision number of the ECMWF IFS/OpenIFS model.

``LLPYR``: Allow for leap year.

``LFIXMET``: Annually recycle met fields.

``JMPOLAR``: Defines if polar grid box has same latitudinal size as
other grid boxes (``JMPOLAR=0``) or half size (``JMPOLAR=1``). Should be
0 for ECMWF data.

``GM0000``: I-coord of Greenwich Meridian. For ECMWF ``GM0000=1.5``,
meaning that Greenwich (0:math:`^{\circ}`\ E) is in the middle of grid
box 1 (so that 1 is left edge and 2 is right edge). Other meteorological
data may have different grids.

After this, the hybrid sigma coordinates are listed. Note that if you
collapse layers (``COLLAPSE`` in *Makefile*), the ``LMMAP`` must match
this. So if you collapse layers 1–3, the ``LMMAP`` of the first
3 entries must be 1. Native level 4 will then have ``LMMAP=2``.
Traditionally, the Oslo CTM3had always been run in native vertical
resolution, while UCI-CTM is often run with collapsed layers near the
surface.

Next:

``PFZON``: Allows polar latitudes to combine grid boxes horizontally.
However, this is not applied in Oslo CTM3. Entries should therefore be 1
(there are 25 entries).

``NBLX``: Boundary layer scheme (Section [sxn:transport\_blmix]).
Default should be 5.

``NDPX``: Dry deposition scheme (Section [sxn:drydep]). Only simple UCI
scheme is available, but it is modified/overwritten for the Oslo CTM3.

``NSCX``: Scavenging scheme for large scale scavenging (Section
[sxn:ls\_scav]). Should be set to 1.

Then input filenames for annual mean pressure and vegetation are listed,
followed by more tracer specific parameters, e.g. how to start the
model:

``LCONT``: Start from restart file (``T``) or not (``F``).

``START_AVG``: If ``LCONT = F``, this index specifies how else to
initialize the tracer field. ``START_AVG=0``: ``STT=0``,
``START_AVG=1``: start from Oslo CTM3 average file.

A chemistry run initialised to zero (``LCONT=F`` and ``START_AVG=0``)
will crash, probably reporting negative stratospheric NO\ :math:`_x` or
NO\ :math:`_y`.

In this tracer specific section, the filenames of the tracer list
(Section [sxn:tracerlist]), scavenging lists and emission list
(Section [sxn:emisfile]) are given.

Note that the tracer list is read immediately after it is defined. This
is to allow for checking against included tracers, e.g. by different
diagnostics.

Lastly, there are several settings and flag calendars for diagnostics.

``JDO_C``: When to save restart files.

``JDO_T``: When to do tendencies.

With the tendency tracer flags you specify which tracers to diagnose.
Note that these are transport numbers, not component IDs.

Then we have:

``JDO_A``: When to do averages.

``JDO_X``: When to do STE calculations.

And finally the UCI time series output. This is not the same as the
Oslo CTM3 time series (Section [sxn:vprofseries]), and is not used.

| **Future update**
| Using transport numbers to define output is problematic, because you
  need to keep track of the order of components. In the future this will
  be changed to component names.

Tracer list – *tracer\_list.d*
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

The tracer list (*tracer\_list.d*) lists all tracers needed in the
simulation. Names and molecular weights are listed, as well as some
diagnostic flags *that are not yet in use*.

The list is divided into two parts: the first lists the transported and
the second lists the non-transported species. The total listed numbers
of transported and non-transported species must match ``NPAR`` and
``NOTRPAR`` in *cmn\_size.F90*.

When read into the model, the tracer names are found in the variables
``TNAME`` for transported species and ``XTNAME`` for non-transported
species.

There may be a tracer list available for your choices, and they are
located in the directory *tables*. You may have to make your own tracer
list, depending on which applications you include.

It can be mentioned that eventually, the non-transported array should be
removed, but this is left for future work.

| **Where is the tracer list specified?**
| The path and file name of the tracer list is specified in the input
  file *LxxCTM.inp*.

Emissions list – *Ltracer\_emis\_xxxx.inp*
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

This file contains all emission information needed for the Oslo CTM3 to
run; information on forest fires, lightning, 2D and 3D monthly
emissions. The files are located in the directory *tables*. You may want
to build your own list; just save it under a different name. There are
files for different main inventories, such as CEDS, ECLIPSE, RETRO and
Lamarque, listed in the subdirectory *EMISSION\_LISTS*.

The most recent dataset is CEDS (CMIP6) version from May 2017.

| *Important 1*:
| You should keep good track of the emission inputs you use in different
  model runs; they are important to be able to reproduce the
  simulations. For simplicity, the whole list is printed to standard
  out, before reading the file again and doing the actual read-in of
  emission datasets.

| *Important 2*:
| There is not really a default/preferred emission dataset yet, so you
  have to ask the experienced users about which you should use. It will
  probably be best to use the newest emission estimates. There are
  several datasets for anthropogenic emissions, but fewer for natural
  emissions. Natural emissions will often have to be taken from several
  datasets.

| *Important 3*:
| If you want to include CH\ :math:`_4` emissions instead of using the
  standard fixed surface concentrations, you have to do some changes to
  the code and input files. To do this correctly, you **must** read
  Section [sxn:emissions\_ch4].

| *Important 4*:
| If you want to build your own emission list, please read
  Section [sxn:emissions\_howto].

File names of emission files are listed along with scaling for each
component which the file applies for. Also a few short term variations
can be specified.

Note that the whole path for the emission files should be given in the
emission list (*Ltracer\_emis\_xxxx.inp*), so you don’t have to link to
the emission directory.

Emission data can usually be found in a directory available for all
users. However, when you start using the Oslo CTM3, you should ask us
where to find the data.

See Section [sxn:emissions] for further information on the emissions and
the scaling possibilities.

Meteorological data
~~~~~~~~~~~~~~~~~~~

Traditionally, the Oslo CTM3 has been driven by meteorological data from
the ECMWF Integrated Forecast System (IFS) model, in both 40-layer and
60-layer versions. In 2015, the ECMWF openIFS model was applied to
generate new data locally at CICERO/UIO, covering a larger time period.
This model is in principle similar to the IFS model.

You define which meteorological data to use in the file *LxxCTM.inp*.
First you need to specify the dataset:

``metTYPE``: Which type of data. ``ECMWF_oIFSnc4`` is openIFS data in
netCDF4 format. ``ECMWF_oIFS`` is openIFS data in the UIO format.
``ECMWF_oIFS`` is the older IFS data in the UIO format.

``metCYCLE``: The cycle number of the model used to generate
meteorological data.

``metREVNR``: The revision number of the cycle version.

Note that the Oslo CTM3 is only set up for these datasets so far. When
other data is used, a new read-in routine must be made, and then the
cycle and revision numbers can still be used to define the model
version.

Next you define the path where the meteorological data is located,
``MET_ROOT``. The Oslo CTM3 will use ``metTYPE``, ``metCYCLE``,
``metREVNR`` and ``metTYPE`` to make the full path of file names. This
is carried out in the ``INPUT`` routine in file *initialize.f90*.

When you start using the Oslo CTM3, you should ask current users where
to find the data.

| *Important*
| The openIFS meteorological data was updated from binary files to
  netCDF4 files late 2015. You can find more about the meteorological
  data and the data formats in Appendix [app:metdata]. If you need to
  run the old format, there is a separate read-in available, called
  *metdata\_ecmwf\_uioformat.f90*. The Oslo CTM3 should give you a hint
  about this if you specify an old ``metTYPE``.

| *Important for older meteorology data*
| The old format will eventually be phased out. The new format has
  greater flexibility e.g. to read a field from the next time step and
  interpolate temporally between them. If that is included at some
  point, it will be difficult to use the binary files.

If you get very strange results in the first time steps, you may have
chosen the wrong resolution data. Reading 60-layer data in a L40 model
run, may not proceed past the end of the file. But a L60 run will try to
read past the end of file of 40-layer data, and the model will crash.

Restart file
~~~~~~~~~~~~

A restart file contains tracer distributions for all species in
a simulation, as well as the moments for all the transported species. It
allows you to continue a run without loss of tracer information, and
while some aerosol packages can be started from zero, chemical
components usually requires initial values.

There are several versions of read-in routines available. The standard
read-in reads netCDF4 files, and is called ``load_restart_file``,
located in the file *stt\_save\_load.f90*. It is called from subroutine
``SETUP_SPECIES`` in the file *initialize.f90*, which is is called from
.

In the restart file, transported species have prefix ``STT_``, and for
these species there are also moments available, having prefixes
``SUT_``, ``SVT_``, ``SWT_``, ``SUU_``, ``SVV_``, ``SWW_``, ``SUV_``,
``SUW_``, ``SVW_``. Non-transported species have prefix ``XSTT_``.

The read-in will interpolate horizontally if the model resolution
differs from the file resolution, but note that in such cases only two
moments are used: ``SWT`` and ``SWW_``.

A possibility for vertical interpolation should be included eventually.

The read-in assumes the restart file is called *restart.nc*, but note
that it is possible to read several restart files. As long as the read
in flag ``MODE`` is set correctly, only the uninitialised fields will
then be set.

Complementary, there is a routine to save the restart files, called
``save_restart_file``,

As a new user, you should regularly check that you actually use the
restart file you think you use.

Traditionally you can also restart from a monthly average, but this
needs more spin-up. There is yet no routine for reading average netCDF
files.

Other input data
~~~~~~~~~~~~~~~~

There are some additional data you need for running the Oslo CTM3. The
model reads these data from the directory *Input\_CTM3*, so you need to
link to that directory. On the Abel cluster this is located at
*/work/projects/cicero/ctm\_input*\ */*\ *Input\_CTM3*, and the data
consists of

2d-data for the stratospheric module (boundary condition data).

Background aerosol surface area densities for the stratospheric module.

CH\ :math:`_4` surface mixing ratios.

Dry deposition values.

| **2d-data**
| Stratospheric chemistry includes a number of species which have long
  tropospheric lifetimes, and thus is set as fixed mixing ratios in the
  model levels closest to the surface. At the top of the model, i.e.
  upper stratosphere, the model lacks information on how much is
  transported upwards (into the mesosphere) and also on mixing ratios to
  be transported downwards (from the mesosphere).

Species having very long lifetimes and are destroyed in the mesosphere,
they may build up in the stratosphere if the flux out is not considered.
Likewise, if a component is produced in the mesosphere and transported
downwards, it would not be included as a source for the stratosphere.

Traditionally, upper boundary conditions have been set for many species,
mimicking this, while doing chemistry up to the next-to-uppermost layer
(``LPAR-1``).

These boundary conditions are taken from simulations done with the Oslo
2D model, and are just called the 2d-data. The data are read from the
directory *Input\_CTM3*, so you have to link this directory to where you
run the model.

While such a method may hinder build-up of some species, it is for other
species not an ideal solution.

Firstly; the uppermost level of the 2D model is about 55km, well below
the L60 top level. The mixing ratios are therefore scaled to the L60
vertical grid using the uppermost mixing ratio gradient of the 2d-data
(see Section [sxn:stratchem\_lparchem] for more). Using 2d-data may
perhaps be OK for L40, but not for L60.

Secondly; depending on the tracer, it may effectively act as either
a sink or a source. This may not be what was intended.

A third obstacle is that upper boundary conditions must match the
simulated year. Removing it will make modelling e.g. pre-industrial and
future atmospheres easier.

Finally, the Oslo 2D model can no longer be run because its input files
are nowhere to be found. The 2D data are therefore not available for
years after 2011.

One solution is to use e.g. WACCM to produce boundary conditions.
Another, perhaps better, solution is to do chemistry all the way p to
the model top, so that there is a fixed lid on top of the model. If
a flux out or in is needed, it should be parameterised as a separate
process. **I am currently trying to revise this (started January
2015).**

The 2d-data are read from original Oslo 2D files, so-called *sr-files*,
and interpolation to model resolution is carried out on-line.

| **Stratospheric background aerosols**
| Background aerosol surface area density is important for the
  heterogeneous chemistry in the stratosphere. These data are also
  located in the directory *Input\_CTM3*\ */backaer\_monthly/*.

These data were compiled by David Considine and Larry Thomason at NASA
LaRC, based on SAGE II, SAGE I and SAM II satellites. Data was prepared
by the approach of :raw-latex:`\citet{ThomasonEA1997}`, and
a description can be found in the *backaer\_monthly/* directory.

The data are read from the original resolution and interpolated on-line
to the model resolution.

Only year 1979 to 1999 are available, where 1979 is used for all years
prior to 1979 and 1999 for all years after 1999.

| **Dry deposition**
| Dry deposition velocities are read from the file *drydep.ctm*, which
  is found in the *Input\_CTM3* directory. A new dry deposition scheme
  is under way, and will replace this method or most species. See
  Section [sxn:drydep] for more.

Surface CH4
~~~~~~~~~~~

Due to its long lifetime, CH\ :math:`_4` is usually fixed at the model
surface. This is the default treatment in Oslo CTM3. The files
containing this input data are located in the *Input\_CTM3* directory.
Surface volume mixing ratios were calculated in the project HYMN, where
CH\ :math:`_4` emissions were included, and monthly averages for 2003,
2004 and 2005 are available for Oslo CTM3. Currently we use 2003 monthly
values, however, these values should be scaled to match observed values
for the year you are simulating. This can be done by a separate routine,
scaling the HYMN 2003 dataset to marine global annual CH\ :math:`_4`
observed by ESRL Global Monitoring Division (see
Section [app:ch4routines]).

| **Important 1**
| For pre-industrial simulations, these will have to be scaled or
  updated.

To match these surface values, it is possible to also set the whole 3D
tracer field from the HYMN results.

| **Important 2**
| As noted above, the fixed CH\ :math:`_4` field from HYMN can be scaled
  to observed values. If you use a CH\ :math:`_4` field from a different
  year, you can make a similar scaling routine (see
  Appendix [app:ch4routines]).

| **Not so important**
| Also available are RETRO surface values, as zonal means for different
  years.

| **CH\ :math:`_4` emissions**
| The Oslo CTM3 can also be run with CH\ :math:`_4` surface emissions.
  Currently the possible set-up is a combination of anthropogenic
  emissions and natural emissions and soil uptake provided by Bousquet
  (project GAME). If you want to include CH\ :math:`_4` emissions, you
  **must** read Section [sxn:emissions\_ch4].

Transport options
~~~~~~~~~~~~~~~~~

Transport is explained in Section [sxn:transport], however, there are
two things worth mentioning before you start.

In the input file (*LxxCTM.inp*), you specify the duration of the
operator split time step, i.e. the duration of each process. Default is
``NROPSM=3``, which means 60minutes when there are 8 meteorological time
steps during the day (this is default in Oslo CTM3). This is not the
time step used in the transport routine; this is further explained in
Section [sxn:transport]. The operator split time step is the duration of
each process before the next is started.

It is possible to run the Oslo CTM3 with as short operator split time
step as wanted. Halving the value to ``NROPSM=6`` will improve the polar
vortex gradients, and should be used when studying the polar
stratosphere :raw-latex:`\citep{SovdeEA2012}`.

In addition, it is possible to use a more accurate treatment of the
polar cap transport. The improved transport improves cross-polar
gradients and should also be considered for polar stratosphere studies.
How to implement the more accurate transport is explained in the
horizontal transport section in .

Compiling
---------

When the *Makefile* is set up, it can be compiled e.g. using *gmake*.
Just type *gmake* and hit enter. Note that parallel compiling
(e.g. gmake -j8) is faster.

When you change global parameters you need to re-compile the whole
program, it is wise to first clean up the object files:

::

      gmake clean

This cleans all the files generated by gmake during a compilation.

You can also check the contents of *Makefile* by ``gmake check``.

Running the model
-----------------

You start the model by typing

::

      ./osloctm3 < LxxCTM.inp

where ``Lxx`` is the vertical resolution you compiled with (including
possible info on how layers are collapsed). Usually, Oslo CTM3 uses
*L60CTM.inp*. You use the same input file for all horizontal
resolutions. Horizontal resolution is set in *Makefile*.

Oslo CTM3 is parallelized using OpenMP (see Section [sxn:parallel]), and
for most machines you need to specify the number of threads in an
environment variable (``OMP_NUM_THREADS``). This can be handled
automatically by a job script, or you may have to specify yourself. You
can e.g. use 16CPUs by running:

::

      OMP_NUM_THREADS=16 ./osloctm3 < LxxCTM.inp

Note that this will print to screen. To print to a log file, use

::

      OMP_NUM_THREADS=16 ./osloctm3 < LxxCTM.inp > results.log

Add the standard ``&`` at the end to make the program run in the
background.

Model crashes
~~~~~~~~~~~~~

As a new user you will probably experience that the model crashes when
you try to run the model for the first time.

A couple of diagnostics require certain tracers to be included, and if
they are not, the model will crash. These are:

``satprofs_master``

``vprofs_master``

``caribic2_master``

``trocciXXX_master``

To solve this, you may either change the list of tracers in their
corresponding files, or you may comment out the calls to the routines.
The latter is done in the routine ``nops_diag`` in
*diagnostics\_general.f90*.

Note that the calls are by default commented out, so you have to put
them back if you need them.

You should also check out the Appendix [app:trouble] for trouble
shooting.

Publishing & documenting
------------------------

Not only should we publish research in peer-reviewed journals, we should
also document changes in the Oslo CTM3, at least if the changes are
introduced to the repository.

Publishing in journals
~~~~~~~~~~~~~~~~~~~~~~

When you publish results from the Oslo CTM3, please let the
Oslo CTM3 users know of it!

I have included a list of papers in Appendix [apx:peerreviewed].

Contact info is found in Appendix [app:contact].

Documenting changes
~~~~~~~~~~~~~~~~~~~

If you upgrade the Oslo CTM3 (other than small bug fixes) please write
up a description document, including the main effects.

Source code introduction
========================

This section provides a short description of the model source code,
which is mainly written in Fortran90 with file extension *-.f90*.
However, there are a few files written in fixed form with extension
*-.f*. The latter comprise the most important transport files inherited
from UCI, and were not converted to make possible future updates from
UCI easier.

Oslo CTM3 is free from common blocks. All variables are defined in
modules.

Model source code files are generally divided into core files and files
needed for Oslo chemistry and aerosols modules. The Oslo files are
located in the directory *OSLO*.

Model parameters and variables are found in the cmn-files, see
Section [sxn:cmnfiles].

Note that several modules also contain their own global variables.

See Section [sxn:structure] for program structure and Section
[sxn:programming] for programming guidelines.

The core source
---------------

The model core consists of the files (routines, variables, parameters)
necessary to run the model with transport only, e.g. meteorological
variables and tracer distribution variables. The core source is located
in the main directory, and are based on the files inherited from UCI.

The global variable cmn-files are placed in several files, described in
Section [sxn:cmnfiles].

Essential to the model is the tracer distribution of the transported
species, named ``STT``, which is a 4D array of model grid size (see
Section [sxn:modelgrid] for grid description) times the number of
*transported* components (``NPAR``). See Section [sxn:oslocore\_src] for
how to handle the non-transported species.

Inside the parallelisation loops, the tracer field (``STT``) is moved
into local arrays (``BTT``), which have a different structure. We will
call this a \ *B-array* (or private array), and its structure will be
explained in Section [sxn:parallel].

The second order moments scheme also transports the first and second
order moments, i.e. 9 moments for each of the transported species. These
moments are named ``SUT``, ``SVT``, ``SWT``, ``SUU``, ``SVV``, ``SWW``,
``SUV``, ``SUW``, and ``SVW``, and are described in Section
[sxn:transport]. Also the moments are transformed into B-arrays in the
parallel region.

The parameter files
~~~~~~~~~~~~~~~~~~~

Central to the model core is the parameter file, which sets the model
resolution (i.e. array sizes) through the parameters ``IPAR``, ``JPAR``,
``LPAR``, ``NPAR``, etc.

This file uses the *Makefile* tokens to include chunks of code defined
by the C-code (such as ``ifdef``). All parameters are set automatically
when compiling with well known user choices in *Makefile*.

Among the parameters are also logical switches for each of the chemistry
or aerosol modules, such as

``LOSLOCTROP``: Use Oslo tropospheric module.

``LOSLOCSTRAT``: Use Oslo stratospheric module.

``LSULPHUR``: Use Oslo sulphur module.

``LBCOC``: Use BCOC module.

*Makefile* was described in Section [sxn:makefile].

Global variable files
~~~~~~~~~~~~~~~~~~~~~

The global variable files (common files) are:

*cmn\_precision.f90*: Defines precision parameters.

*cmn\_size.F90*: Parameters for grid sizes and also logical parameters
needed for the run.

*cmn\_ctm.f90*: Transport variables and more.

*cmn\_chem.f90*: Emission variables and other variables related to
chemistry.

*cmn\_fjx.f90*: Variables for fast-JX (photochemistry).

*cmn\_met.f90*: Meteorological variables.

*cmn\_sfc.f90*: Surface (2-dimensional) variables.

*cmn\_diag.f90*: Diagnostic variables.

*cmn\_parameters.f90*: Parameters.

**OSLO*/cmn\_oslo.f90*: Variables for Oslo chemistry and aerosols.

Main program
~~~~~~~~~~~~

The main program is located in . It is described in
Section [sxn:structure].

Core diagnostics
~~~~~~~~~~~~~~~~

The main diagnostics are baked into the model core, and also have a few
B-arrays. Such B-arrays bring their diagnostics back to the upper level,
where they usually are put out every operator split time step, or
e.g. accumulated for averaged values. The diagnostics will be described
further in Section [sxn:diagnostics].

The Oslo core source
--------------------

The Oslo core comprises the files and variables necessary to run the
model with Oslo packages. The files are located in the directory *OSLO*.

Variables are generally located inside modules or in *cmn\_oslo.f90*,
whereas the subroutines are mostly located in modules.

Important variables
~~~~~~~~~~~~~~~~~~~

In chemistry, each component has a chemical id, and these ids must be
mapped to transport number. This is done in the variable ``trsp_idx``
maps the transported species (chemical IDs) into their transport number
– i.e. into their place in the ``STT`` array. In the same way
``Xtrsp_idx`` maps the non-transported species into their place in the
non-transported tracer array ``XSTT``. The sizes of the mapping arrays
are set by the maximum number of chemical IDs (``TRACER_ID_MAX`` in
*cmn\_size.F90*).

Similarly, two other index arrays map the other way; ``chem_idx`` (size
``NPAR``) and ``Xchem_idx`` (size ``NOTRPAR``), respectively.

These can be found in the files *cmn\_ctm.f90* and *cmn\_oslo.f90*,
respectively.

``XSTT`` is located in *cmn\_oslo.f90*.

Note that if you have no non-transported species, the array size will of
e.g. \ ``Xchem_idx`` will be zero. This means that before trying to
access this array, you need to check if ``NOTRPAR`` is greater than
zero, otherwise the program may stop.

To diagnose the non-transported species, a 3D average field is also
defined (``XSTTAVG``). This average field follows the Oslo CTM3 core
average treatment. Note that these arrays are reverse-indexed
(``LPAR, NOTRPAR, IPAR, JPAR``) to reduce striding, since they are only
accessed in the IJ-blocks. They keep this structure when written to the
restart file and to average files.

| *Important*
| The use of non-transported species should be out-phased and replaced
  by some steady-state considerations, but this will be left for later.

Application variables
~~~~~~~~~~~~~~~~~~~~~

Variables that are only used by a specific application are in general
defined in their respective modules or files.

Dummies
~~~~~~~

To be able to turn off an application, most applications have some dummy
routines, located in *OSLO*\ */DUMMIES*. See
Section [sxn:structure\_important] for more.

Program structure
=================

To get an overview of how the Oslo CTM3 works and how it is structured,
it is best to look into the main driver  ().

Main structure – 
-----------------

The main program () controls the main loops and calls to do the
calculations. Its general structure is outlined in Table
[table:mainstructure],

--------------

::

      <initialize model>
      !// MAIN LOOP
      do NDAY = NDAYI,NDAYE-1

        <do daily stuff>

        !// METEOROLOGICAL LOOP
        do NMET = 1,NRMETD

          <update meteorology etc>

          !// OPERATOR SPLIT LOOP
          do NOPS = 1,NROPSM

            !// SUB LOOP
            do NSUB = 1, LCM
              !// do master calls
              <chemistry>
              <transport>
              <diagnostics>
            end do

            <diagnostics after every NOPS>
          end do

          <diagnostics after every NMET>
        end do

        <daily diagnostics>
      end do

--------------

with the important loop variables ``NDAY``, ``NMET``, ``NOPS``, and
``NSUB``. They are all defined in , and are thus not global variables.

Main loops
~~~~~~~~~~

``NDAY`` is the day counter, looping through each day (from ``NDAYI`` to
``NDAYE-1``, see Section [sxn:maininput]). These variables are therefore
defined in  (not global). Things that need to be done on daily basis
will be placed in this loop. Daily diagnostics, however, should be
placed at the end of the day.

``NMET`` is the meteorological time step. For ECMWF IFS data, the
meteorological data is stored 8 times per day (00UTC, 03UTC, ...). The
number of meteorological time steps is set by the ``NRMETD`` parameter
defined in *cmn\_size.F90*. It should be noted that some ERA-40 data are
also available. These are also ECMWF data, but given 4 times per day.
The Oslo CTM3 is not set up to use them, however, if you want to use
ERA-40, you should change the number of meteorological time steps
``NRMETD``. A less optimal method is to read the data every second
``NMET`` and not changing ``NRMETD``. I would not recommend this, but if
you insist, remember in read-in to change the time step used for scaling
the accumulated data.

``NOPS`` is the operator splitting time step, with a duration of
``DTOPS``. Operator splitting means that the operations are done in
sequence, with a certain time step. There number of such sequences per
meteorological time step is given by ``NROPSM``. For a short enough time
step the operations should be close to reality, and the solution should
converge when further shortening the time step. Keep in mind that the
order of the processes may be important. Note that the meteorology is
kept constant through the meteorological step, no matter how many
operator split time steps are used.

Within each ``NOPS``, there is a sub-stepping loop where chemistry and
transport are done asynchronously if their time steps differ. This will
be explained next.

Sub-stepping loop
~~~~~~~~~~~~~~~~~

During one operator split loop (for each ``NOPS``) advection is done
``NADV`` times (calculated based on numerical stability), while
chemistry and boundary layer mixing are done ``NRCHEM`` times.
``NRCHEM`` is set by the user in *LxxCTM.inp*, with a default value
of 1. See section [sxn:internal\_chem\_loop] for more on this variable.
The corresponding time steps are ``DTADV=DTOPS/NADV`` and
``DTCHM=DTOPS/NRCHEM``, respectively.

Operations are in general done in sequence (operator splitting), but
when time steps ``DTADV`` and ``DTCHM`` differ the result is an
asynchronous stepping. The sequence of operations is solved by looping
through the least common multiple (``NLCM``) and do advection and
chemistry accordingly. This is done by ``NSUB`` in Table
[table:mainstructure]. Figuratively, this can be shown by some examples,
given in Table [table:substepping].

--------------

: ``NADV=1`` and ``NRCHEM=4``, with ``NLCM=4``:

::

       Step   What is done   Time duration
        1     CHEM  ADV      15min / 60min
        2     CHEM           15min
        3     CHEM           15min
        4     CHEM           15min
        1     CHEM  ADV      etc...

**Example 2**: ``NADV=3`` and ``NRCHEM=4``, with ``NLCM=12``:

::

       Step   What is done   Time duration's
        1     CHEM  ADV      15min / 20min
        2
        3
        4     CHEM           15min
        5           ADV              20min
        6
        7     CHEM           15min
        8
        9           ADV              20min
       10     CHEM           15min
       11
       12
        1     CHEM  ADV      etc...

--------------

It follows from these examples, that when ``NADV`` and ``NRCHEM`` are
larger than 1, the operations are done more frequently than once per
``NOPS``, and should therefore be closer to reality.

Note that the processes in general do not start at the same hours and
minutes, but are asynchronous: Example 2 in Table [table:substepping]
shows this. The processes are carried out using different time steps
(``DTADV=DTOPS/3`` vs ``DTCHM=DTOPS/4``), so that at ``NSUB=5``,
chemistry has already been calculated (4–6) when advection is calculated
(5–9)

Both chemistry and advection is done in the first ``NSUB``, so that in
Example 1, chemistry is done for 60 minutes consecutively (4 times of
15min), starting at step 2, before the next advection.

Internal chemistry loop
~~~~~~~~~~~~~~~~~~~~~~~

Considerable CPU time is used to go in and out of the B-array parallel
region. The reason for this is that advection needs two types of
parallel coding (IJ-block, Section [sxn:parallel:ij] and layer
parallelisation, Section [sxn:parallel:layer]). If the processes that
only need IJ-block parallelisation (e.g. chemistry) is carried out more
often, the time spent on moving in and out of the B-arrays will
increase.

It is therefore preferable to set ``NRCHEM`` as low as possible.
However, the number of operator splits is usually 3, which for
``NRCHEM=1`` will give a time step of one hour for each of the processes
(emissions, boundary layer mixing, chemistry and deposition). This may
be a little too long in the boundary layer, where mixing is relatively
fast.

Traditionally, the Oslo CTM2 solved this by looping boundary layer
mixing and chemistry with a time step of 15min. We adopt this in
Oslo CTM3, introducing an internal loop over emissions, boundary layer
mixing, chemistry and deposition in . The looping is carried out
``CHMCYCLES`` times per ``NOPS``; for ``NRCHEM=1``, ``CHMCYCLES=4``, for
``NRCHEM=2``, ``CHMCYCLES=2`` and otherwise ``CHMCYCLES=1``.

Note that the chemical section (emissions, boundary layer mixing,
chemistry and deposition) is still calculated for one NRCHEM before
advection is calculated.

| **Future update**
| I think it would be a good idea to let ``NRCHEM`` change according to
  ``NADV``; in T42 resolution, we often have ``NADV=1``, but also
  encounter larger values, especially in higher resolutions. The code
  should be modified to be able to account for this.

Important notes
~~~~~~~~~~~~~~~

| 
|  is supposed to be very short and easy to grasp. As few as possible
  calls should be made from , and the calls should preferably be to
  master routines (e.g. diagnostics or chemistry).

| *Keep the model clean of C-code!*
| If you do not know what C-code may do in the Fortran code, all is
  well. Or you can check Section [sxn:preproc]. C-code should not be
  necessary to include or exclude parts of code. It makes the code very
  difficult to read, at least for large chunks of code. Even if one
  programmer introduces a new small chunk of C-code, experience has
  shown that this practice will grow. If you insist on using them in
  your subroutines or modules, keep the existing code free of C-code.

In the model core, there is only one file containing C-code, and that is
*cmn\_size.F90*, which is the basis for *Makefile* to generate important
parameters. (In the Oslo core, the DUST code also contain some C-code.)

Instead of C-code, dummy routines should be used in the model code. The
time spent on calling a routine which in worst case does nothing (see
Section [sxn:preproc]), is very minute compared to spending time on
a messy program code. The compiler will in most cases remove the call to
an empty routine, removing calling overhead by inlining the code (which
is empty). See the files in *OSLO*\ */DUMMIES* for dummy examples.

The C-code preprocessing system
-------------------------------

The C-code preprocessing system is a way to include or exclude chunks of
code from being compiled. The preprocessor will look for specified
*tokens*, e.g. ``DO_THIS``. In the preprocessing the code located
between the statement ``ifdef DO_THIS and \verb``\ endif will then be
compiled. The C-compiler will make a temporary file which will be
compiled by the Fortran compiler.

Files containing such C-code will typically only be located in files
with extensions *-.F* or *-.F90*.

We will avoid C-code in the model, except in *cmn\_size.F90*, where the
tokens are coupled to settings in *Makefile*.

Parallelisation of the Oslo CTM3
--------------------------------

The Oslo CTM3 is parallelized using OpenMP. The general parallelisation
is done in  and is carried out in two different ways, which will be
described in Section [sxn:parallel:ij] and [sxn:parallel:layer]:

Over IJ-blocks: Applies for chemistry, boundary layer mixing, convection
and vertical advection.

Over vertical layers: Horizontal advection only.

In addition some other routines outside parallel regions are
parallelized, e.g. emission interpolation.

Here I go through the basics of these parallel regions and how they are
set up to work most efficiently.

OpenMP
~~~~~~

The OpenMP code can easily be located by the ``!$OMP`` at the beginning
of a line of code. It is followed by different specifications,
e.g. \ ``!$OMP PARALLEL``. One of the important issues is to understand
the meaning of ``PRIVATE`` variables; they are private to each
thread/CPU. ``SHARED`` variables are shared. By default, as a safety
measure, you have to define *all* variables inside a parallel region
(not necessary for parameters, which cannot be changed).

If you want to learn more about OpenMP, see *http://openmp.org/*. Also,
the Fortran company provides a tutorial on their web page
*http://www.fortran.com/*.

Parallel IJ-blocks (MP-blocks)
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Most processes are either independent of neighboring grid boxes, or they
depend on the grid box above or below. In the Oslo CTM3 these processes
are treated columnwise, and the columns are grouped together in blocks
of a certain horizontal extent.

In this way the model domain (``IPARxJPARxLPAR``) is split into blocks
(so-called IJ-blocks or MP-blocks) beneficial for parallel work:
``IPAR`` is divided into ``MPIPAR`` sections, and ``JPAR`` into
``MPJPAR`` sections, while ``LPAR`` is unchanged. This creates
``MPIPAR x MPJPAR`` blocks of size (``LPAR``, ``IDBLK``, ``JDBLK``)
which are fed into the parallelisation (``IDBLK=IPAR/MPIPAR`` and
``JDBLK=JPAR/MPJPAR``).

Useful related parameters are ``IDGRD=IPARW/IPAR`` and
``JDGRD=JPARW/JPAR``, giving the number of grid boxes combined in each
direction. Total number of grid boxes combined is ``NDGRD``.

For the IJ-blocks, the transported 4D arrays (``STT``, and moments) are
split into temporary private arrays (B-arrays) available for each
processor, where the spatial size is (``LPAR,IDBLK,JDBLK``). The index
order has been “reversed” (i.e. the ``LPAR`` first instead of last) to
minimize striding when working vertically. See
Section [sxn:programming\_loops] for more on striding.

.. figure:: figures/ctm3_mpblocks_t42_new
   :alt: Default IJ-block structure for T42 horizontal resolution, where
   one block covers half the zonal direction and one latitude band.

   Default IJ-block structure for T42 horizontal resolution, where one
   block covers half the zonal direction and one latitude band.

Due to the IJ-block array structure, the loops will produce less
striding for long zonal blocks. Depending on the resolution, the choice
of ``MPIPAR`` and ``MPJPAR`` must be tested to find which is faster. For
the older T42 and for newer 2x2 combined T159 horizontal resolutions the
default is ``MPIPAR=2`` and ``MPJPAR=JPAR``, so that the IJ-blocks cover
half of the zonal direction (``1:IPAR/2``), and one latitude band, as
shown in Figure [fig:mpblocks].

It may, however, be that other configurations are better when
transporting few tracers. For other resolutions there are other block
sizes (defined in *cmn\_size.F90*). A more thorough discussion on the
IJ-block sizes is included in Section [sxn:nrcpus].

In , the parallel index ``M`` loops through the number of IJ-blocks, and
is passed on to subroutines where it is usually named ``MP``. The global
indices are accessible by using the variables ``MPBLKIB``, ``MPBLKIE``,
``MPBLKJB`` and ``MPBLKJE`` (all of size ``MPBLK``). Their names
``MPBLKIB`` and ``MPBLKIE`` are somewhat self-explaining; the first
contains the zonal beginning point of all IJ-blocks (i.e. the global
zonal indices), while the latter contains the end points. Similarly,
``MPBLKJB`` and ``MPBLKJE`` are the starting and end points in the
meridional direction. For a given IJ-block (which have parallel index
``MP``), the first global zonal index therefore is given by
``MPBLKIB(MP)`` and ends at ``MPBLKIE(MP)``, while the first global
meridional index is given by ``MPBLKJB(MP)`` and ends at
``MPBLKJE(MP)``.

A typical IJ-block loop is outlined in Table [table:ijblock],

--------------

::

      !// Loop over latitudes in IJ-block
      do J = MPBLKJB(MP),MPBLKJE(MP)
         !// IJ-block index JJ
         JJ   = J - MPBLKJB(MP) + 1

         !// Loop over longitudes
         do I = MPBLKIB(MP),MPBLKIE(MP)
            !// IJ-block index II
            II   = I - MPBLKIB(MP) + 1

            !// Corresponding local/global
            !// indices
            BTT(L,N,II,JJ) = STT(I,J,L,N)

         enddo
      enddo

--------------

and you should understand how it works and why the reverse-ordered
B-arrays provide less striding (see Section [sxn:programming\_loops] for
more on striding). For global indices ``I,J`` the local/private indices
for IJ-block number ``MP`` are given by ``II = I - MPBLKIB(MP) + 1`` and
``JJ = J - MPBLKJB(MP) + 1``.

| A mapping from global indices ``I,J`` to IJ-block number and local
  indices can be found in the variable ``all_mp_indices``:
| ``(II,JJ,MP) = all_mp_indices(1:3,I,J)``

Parallel layers
~~~~~~~~~~~~~~~

Horizontal advection, i.e. transport between neighboring grid boxes,
have no need for information about boxes above or below. Hence, this
process carried out layer by layer, and a processor calculates transport
of all tracers for one layer, before being assigned (by OpenMP) a new
layer to transport.

It is also possible to do the transport component by component, so that
each processor work on each species, transporting them layer by layer.
Although this was done in Oslo CTM2, it is not done now. The experience
of the UCI group was that looping over layers is faster. *Note also that
studies with few tracers would limit effective use of the number of
CPUs, if parallelisation is done over components.*

OpenMP and advection
~~~~~~~~~~~~~~~~~~~~

The important consequences of the advection treatment
(Section [sxn:parallel:ij] and [sxn:parallel:layer]) is that advection
works both in IJ-block and layers and therefore need to go in and out of
IJ-blocks for each transported time step. It means that increasing the
number of operator split steps, also increases the time spent shuffling
data in and out of B-arrays; transport may be better resolved, but it
will be slightly more time consuming.

Writing parallelized code
~~~~~~~~~~~~~~~~~~~~~~~~~

When you write a new module, be sure to parallelize it! For most
processes or applications, it would be wise to use the IJ-block
structure, and therefore assign global arrays in reverse order
(``LPAR,IPAR,JPAR``), or even better by blocks
(``LPAR,IDBLK,JDBLK,MPBLK``).

If your application works in the horizontal (this is less likely),
parallelisation should be layerwise, and global arrays should *not* be
reverse order but have the usual structure (``IPAR,JPAR,LPAR``).

Example: If you use 4 processes and your unparallelized application uses
5seconds per time step (assuming one hour), it will contribute with
:math:`\sim 12`\ hours of computing time when simulating one year.
Effectively parallelized, you could possibly divide this by the numbers
of processors, so that in using 4 CPUs you save 9 hours of real
computing time.

+----------+--------------+--------------+
|          | T42L60\_32   | T42L60\_64   |
+==========+==============+==============+
| 4CPUs    | 1.72         | –            |
+----------+--------------+--------------+
| 8CPUs    | 0.92         | 1.95         |
+----------+--------------+--------------+
| 16CPUs   | 0.57         | 1.08         |
+----------+--------------+--------------+
| 32CPUs   | 0.46         | 0.67         |
+----------+--------------+--------------+
| 8:4      | 0.53         | –            |
+----------+--------------+--------------+
| 16:8     | 0.62         | 0.55         |
+----------+--------------+--------------+
| 32:16    | 0.81         | 0.62         |
+----------+--------------+--------------+

Table: Computational efficiency when increasing the number of CPUs for
T42L60 resolution. Timings are given in wall clock hours, for pure
transport of 32 and 64 tracers, T42L60 resolution, meteorological data
for January 2005

How many CPUs and IJ-blocks?
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

The more done in parallel, the more efficient and faster will the
program be. The Oslo CTM3 is better parallelized than the Oslo CTM2,
however, there are a few things you should be aware of when it comes to
the choice of CPU numbers and how it relates to the number of IJ-blocks.

The number of IJ-blocks (set up in *cmn\_size.F90* should be close to
a multiple of the number of CPUs, since the amount of work done in
a column should be approximately the same for all columns. However, this
may not be true; the vertical transport (such ad convection and
advection) may have a large impact on the time spent in an IJ-block.

It is more difficult to do such a choice for the horizontal transport,
since the amount of work done differ from layer to layer (shorter time
step for larger wind speeds). However, the time spent in horizontal
transport is relatively small, so a good choice for the number of CPUs
will be a multiple of the number of IJ-blocks. As will be explained, the
number of IJ-blocks should be at least twice as large as the number of
CPUs.

Since the time spent in each block may differ, it can be assumed that
OpenMP should use a dynamic schedule. To make that efficient, the number
of blocks must be larger than the number of CPUs. But there is also the
possibility that too many CPUs may give larger overhead. I will discuss
this further below.

Timing tests of pure transport are included in Table [table:cpu],
showing that transporting 32 tracers on 8 CPUs and switching to 16 CPUs
saves :math:`\sim`\ 40% time, while switching from 16 to 32 only
saves 20%. However, transporting 64 tracers and switching from 16 to
32 CPUs also saves about 40%. Hence, the number of IJ-blocks should be
at least twice as large as the number of CPUs.

The number of IJ-blocks and how they are defined also affect how
efficient the parallel code will be. For the setting ``MPIPAR=1`` and
``MPJPAR=JPAR/2``, each block will cover the whole zonal direction
(``1:IPAR``) and two latitude bands. Table [table:mpsize] shows a few
tests carried out for meteorologic data of 1 January 2005, in T42N32L60
horizontal resolution, comparing CPU timings for different IJ-block
sizes.

| l\|c\|c
| MPIPAR/MPJPAR & 16CPU & 32CPU
| 1/16 & 120.8 & 112.0
| 1/32 & 122.0 & 71.4
| 2/16 & 119.6 & 72.5
| 1/64 & 116.7 & 69.6
| 2/32 & 115.0 & 69.3
| 2/64 & 110.8 & 68.0
| MPIPAR/MPJPAR & 16CPU & 32CPU
| 1/32 & 256.3 & 151.7
| 2/32 & 245.9 & 144.8
| 2/64 & 245.0 & 145.0

Based on Table [table:cpu], the number of blocks should be at least
twice the number of CPUs. It should be easily recognized that the number
of IJ-blocks should be at least as large as the number of CPUs, since
most work is done in the IJ-blocks used in parallelization. 16 IJ-blocks
on 32 CPUs was only slightly faster than on 16 CPUs because horizontal
transport is parallelized over 60 model layers, where the main
improvement was.

Due to e.g. read access limits, the timings varied slightly. Therefore,
each test was done 4 times, and the values presented in
Table [table:mpsize] are averages. It seems that ``MPIPAR=2`` and
``MPJPAR=JPAR`` is fastest for T42N32L60 transport, followed closely by
``MPIPAR=2`` and ``MPJPAR=JPAR/2``.

Adding chemistry makes the array sizes larger, and relatively more work
is done in the IJ-blocks. One-day tests with full tropospheric and
stratospheric chemistry show that on 16 CPUs, ``MPIPAR=2`` and
``MPJPAR=JPAR`` saves about 10s per day, compared to ``MPIPAR=1`` and
``MPJPAR=JPAR/2``. Note that 10s per day amounts to :math:`\sim`\ 1hour
of computing time for one year. For T42N32L60, the configuration
``MPIPAR=2`` and ``MPJPAR=JPAR/2`` seems to be almost as fast as
``MPIPAR=2`` and ``MPJPAR=JPAR`` on 16 CPUs, and slightly faster on
32 CPUs. As the default IJ-block size for T42N32L60 we set ``MPIPAR=2``
and ``MPJPAR=JPAR``.

Using a different date, where the meteorological conditions impose
a shorter time step (10 Jan in this case), indicates that IJ-blocks
spanning more than 2 meridional boxes, e.g. \ ``MPIPAR=2`` and
``MPJPAR=JPAR/4``, seem to make transport slower.

| **Higher resolutions**
| For higher resolutions, the recommendation is a bit more difficult.
  Based on the transport tests, using ``MPIPAR=1`` and ``MPJPAR=JPAR``,
  was the fastest choice. Adding chemistry and more species will
  increase the memory use and potentially change this. In fact, tests
  done in 2018 suggested ``MPIPAR=8`` and ``MPJPAR=JPAR`` as a better
  option.

| *Important*
| However, the OpenMP scheduling has until 2018 been static (the default
  scheduling). This means that every CPU know which IJ-block it will
  calculate, and is likely not beneficial if the computing time differs
  for each block (which it often does). Therefore, the use of dynamic
  scheduling should be used. It adds some overhead, but is generally
  faster unless the number of IJ-blocks is very high compared to the
  number of CPUs used. The difference between static and dynamic
  scheduling for 1280 IJ-blocks in T159N80L60 resolution was about 12%.
  Thus, the default value for T159N80L60 is set to ``MPIPAR=8`` and
  ``MPJPAR=JPAR``, i.e. 1280 IJ-blocks.

Also when combining 2x2 grid boxes, dynamic was clearly beneficial,
saving 10–20% for ``MPIPAR=2`` and ``MPJPAR=JPAR``. It is not clear
whether ``MPIPAR=2`` is faster than ``MPIPAR=4`` for dynamic scheduling,
but with static scheduling ``MPIPAR=4`` is worse. As default, we keep
the ``MPIPAR=2`` and ``MPJPAR=JPAR`` and use dynamic scheduling for 2x2
combination of grid boxes.

The main lesson is: The number of IJ-blocks should be close to
a multiple (2/4/8) of CPUs. This should make sure that CPUs are not
partially idle during computation. E.g. when using 80 IJ-blocks for
T159N80L60 on 32 CPUs, half of the CPUs will on average do 3 IJ-blocks,
while the rest does 2.

As noted, vertical transport may change this slightly if time steps
differ greatly in different IJ-blocks. However, a multiple of the number
of CPUs seems to be the best choice.

Keep in mind that machines sometimes are set up with hyperthreading,
telling you it has more CPUs than it actually has. The Oslo CTM3 has
even shown slower performance when the number of CPUs requested is
higher than the number of physical CPUs (but within the threaded
number).

In essence, when using other resolutions, you should check different
choices of IJ-blocks to find which is faster.

If you plan to use only one processor (serial run), you should still use
several IJ-blocks. E.g. one global IJ-block will be large and not very
efficient, since the whole global arrays will have to be re-arranged.
Remember also that the efficiency is greatly reduced in a serial run,
since the re-arranging of the structure is time consuming *and carried
out by one processor only*.

Module based programming
------------------------

The model code has evolved from being partially Fortran90 to being fully
Fortran90 in 2015. Common blocks are no longer used, as they are marked
obsolete by the Fortran company.

When you add new packages, you should program them as modules. It is
more flexible, and allows combining fixed format code with free format
code. Another advantage is that you can define which parts of a module
you want to access. You access the whole module with

::

      use <module>

where ``<module>`` is the name of the module. This statement must be
placed before the ``implicit none`` statement.

However, you get a better code, which is easier to read and search or
debug, when you specify the variables and subroutines to use:

::

      use <module>, only: <variables, subroutines>

where ``<variables, subroutines>`` is the list of needed variables
and/or subroutines, separated by commas.

If you include a module A, which again includes a module B, you have
indirectly access to all variables and routines in B. By using the
``only`` statement, this can be restricted.

For programming guidelines on how to make your subroutines optimal for
the Oslo CTM3, see Section [sxn:programming].

Programming guidelines
======================

Think structure! If you do not understand the structure of the model
(Section [sxn:structure]), you will probably end up with a very messy
and inefficient code.

A messy code may solve your problem, but should *never* be added to the
Oslo CTM3 repository!

Comment your code!
------------------

Comment your code! Others should understand your code (and yourself
included after putting the code away for a while). If you do
simplifications or approximations, include a comment about why.

Comment so that a newbeginner should understand quickly. Never include
comments that are not understandable, such as “be careful”.

You should at least describe the following:

Each module at the top of the file; its purpose and what it contains.

Each subroutine, its purpose and variables, including the units of
variables.

All calculations. Include exact references if possible; if no reference
is available, write why you do what you do. Write a description that can
be included in this manual at a later stage.

Change existing code?
---------------------

You should try to stay away from the existing core code except for
making master calls at the top level () or in master routines
themselves. If you think you need to do changes (especially big changes)
in the existing code, check with the experienced programmers to find out
if there may be better ways.

Accessing variables in Fortran90 free format
--------------------------------------------

The Fortran90 free format is much easier to read than the fixed F77
style format, and is more elegant. E.g., there is no limit on the number
of characters used on each line.

You can access variables from other modules in this way:

::

      use cmn_size, only: IPAR

Adding a new subroutine
-----------------------

When you add a new subroutine it should be included in a module. Global
variables or parameters should also be specified in this module, or
possibly in common modules.

Still, there may be some very very few occasions, where it may be
necessary to add arrays to the core or the existing chemistry files, but
it should generally be avoided.

When adding a new file, you need to include it in *Makefile*. How to do
this is explained in Appendix [app:makefile].

Adding new components
---------------------

When you add components, you need to make sure to change the number of
tracers in *cmn\_size.F90*, so that *Makefile* selects the right numbers
in compiling. Also make sure the tracer list (*tracer\_list.d*) has the
correct tracer numbers, names and molecular masses.

The default length of tracer names (``TMASS`` and (``XTMASS``) is 10,
set by ``TNAMELEN`` in *cmn\_size.F90*. If you need longer names, you
have to modify ``TNAMELEN``.

Scavenging parameters are located in the file *scavenging\_wet.dat* and
*scavenging\_dry.dat*.

Efficient code
--------------

Think parallel! Whether you add processes or diagnostics, the work
should be done in parallel regions.

Diagnostics may be a little tricky, since they often require access to
the global arrays. In this case, try to keep the arrays small, and do
calculations in the parallel regions. The goal is to do as little as
possible outside of the parallel regions (see
Section [sxn:parallel:writing\_code]).

If you need to convert a few tracers to another unit, you should only
convert the ones you need. See Section [sxn:programming\_unit] for more
information on this.

Unit conversion
---------------

The tracer arrays are given in mass (kg) per grid box, and when you need
another unit you should create a temporary array and convert it on the
fly, avoiding routines converting the whole tracer array.

All tracers are, however, converted before chemistry, and put into the
local array ``ZC_LOCAL`` (also stratospheric components before doing
tropospheric chemistry). There is probably not much/anything to gain by
only converting the tropospheric components for tropospheric chemistry
and vice versa for stratospheric chemistry, but it may be revised at
a later stage. However, only the tropospheric column is converted before
tropospheric chemistry (``1:LMTROP(I,J)``), and only the stratospheric
column before stratospheric chemistry (``LMTROP(I,J)+1:LPAR``).

The conversion routines are located in *utilities\_oslo.f90*. Next
follows descriptions of the conversions, you will probably need them.

Mass to concentration
~~~~~~~~~~~~~~~~~~~~~

The unit of concentration is molec/cm\ :math:`^3`. Conversion from mass
(:math:`m_t`) to concentration (:math:`c_t`) involves the molecular mass
(unit g/mol) and volume (m:math:`^3`). The conversion is done by

.. math::

   c_t = m_t \frac{10^{-3}N_A}{M_t V}
     \label{mass2conc}

 where :math:`N_A` is the Avogadro’s number
(:math:`6.022149\times 10^{23}`\ molec/mol), :math:`M_t` is the
molecular mass (or weight) of the tracer (g/mol), and :math:`V` is the
grid box volume (m:math:`^3`). The factor :math:`10^{-3}` is
a combination of converting :math:`m_t` from kg to g and volume from
m\ :math:`^3` to cm\ :math:`^3`.

Converting the other way;

.. math::

   m_t = c_t \frac{10^3 M_tV}{N_A}
     \label{conc2mass}

:math:`M_t` is given in ``TMASS`` for transported species and ``XTMASS``
for non-transported species. Remember that they are indexed after
transported and non-transported numbers, not tracer IDs, so to get the
correct molecular masses you need the index arrays ``trsp_idx`` and/or
``Xtrsp_idx``.

Mass to mixing ratio
~~~~~~~~~~~~~~~~~~~~

By the term mixing ratio, the atmospheric chemistry community often mean
mole/number mixing ratio, which as I will show is the same as volume
mixing ratio for an ideal gas. In the aerosol field, however, mass
mixing ratio is more common.

Mass mixing ratio (mmr) unit is kg/kg, i.e. mass of tracer (:math:`m_t`)
divided by the mass of air (:math:`m_a`). On the other hand, mole (or
number) mixing ratio is the number of tracer molecules (:math:`n_t`)
divided by molecules of air (:math:`n_a`). For an ideal gas,
concentration is :math:`c_t=n_tN_A/V`, and :math:`c_a=n_aN_A/V`, so the
mixing ratio by volume is :math:`c_t/c_a`.

For a specific tracer, the relationship between mole (:math:`n_t`) and
mass (:math:`m_t`) is:

.. math::

   n_t = \frac{m_t}{M_t}
     \label{moles_from_mass}

Thus, the conversion from mmr to vmr only involves the tracer mass
(:math:`m_t`), air mass (:math:`m_a`) and the molecular weights of the
tracer (:math:`M_t`) and air (:math:`M_a`):

.. math::

   vmr = \frac{n_t}{n_a}
         = \frac{\frac{m_t}{M_t}}{\frac{m_a}{M_a}}
         = \frac{m_t}{m_a}\frac{M_a}{M_t}
     \label{mass2vmr}

The number :math:`M_a/M_t` is available as ``TMASSMIX2MOLMIX`` for
transported species and ``XTMASSMIX2MOLMIX`` for non-transported
species.

Note also that :math:`m_t/m_a` is the mass mixing ratio :math:`mmr`.

Converting back to mass:

.. math::

   m_t = vmr \times m_a \frac{M_t}{M_a}
     \label{mvr2mass}

 :math:`M_t/M_a` is available as the variable ``TMOLMIX2MASSMIX`` for
transported species and ``XTMOLMIX2MASSMIX`` for non-transported
species, so to convert to mass mixing ratio you multiply with
``TMOLMIX2MASSMIX`` (or ``XTMOLMIX2MASSMIX``) and then multiply with the
air mass.

vmr to mmr
~~~~~~~~~~

The conversion from mass mixing ratio (mmr) to volume mixing ratio (vmr)
is very short and easy. Given tracer mass (:math:`m_t`), tracer moles
(:math:`n_t`), air mass (:math:`m_a`), air moles (:math:`n_a`) and the
molecular weights of the tracer (:math:`M_t`) and air (:math:`M_a`):

.. math::

   \begin{aligned}
     mmr &=& \frac{m_t}{m_a} = \frac{n_t M_t}{n_a M_a}\nonumber\\
         &=& vmr \times \frac{M_t}{M_a}
     \label{vmr2mmr}\end{aligned}

 In other words: multiply volume mixing ratio by ``TMOLMIX2MASSMIX`` (or
``XTMOLMIX2MASSMIX`` for non-transported tracers).

Concentration to mixing ratio
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

You should not need this conversion, but I include it in case you are
interested. For an ideal gas the volume mixing ratio equals molecules of
tracer divided by molecules of air, i.e. concentration of tracer divided
by concentration of air:

.. math::

   vmr = \frac{c_t}{c_a}
     \label{conc2vmr}

 :math:`c_a` is the concentration of air – i.e. air density
(molec/cm:math:`^3`), while :math:`c_t` is the tracer concentration. The
air density is given as ``AIRMOLEC_IJ``, a global field on IJ-block
structure (``LPAR,IDBLK,JDBLK,MPBLK``) which is updated in the B-region
at each meteorological time step.

Equation ([conc2vmr]) can also be derived from Equation ([conc2mass])
and ([mass2vmr]):

.. math::

   \begin{aligned}
     vmr &=& c_t\frac{10^3 M_a V}{m_a N_A}\nonumber\\
         &=& \frac{c_t}{c_a}\end{aligned}

Back to concentration:

.. math::

   \begin{aligned}
     c_t &=& vmr \frac{10^{-3}m_a N_A}{M_a V}\nonumber\\
         &=& vmr \times c_a
     \label{vmr2conc}\end{aligned}

Keep  clean
-----------

As noted in Section [sxn:structure\_important], you should not make
large changes in . It is supposed to be very short and easy to grasp.
Only simple master calls should be made from  (which is possible; write
master routines!).

Precision of numbers
--------------------

Variables should in general be defined as double precision, although
there are some exceptions. The precision parameters are set in
*cmn\_precision.f90*, and you should follow the existing code. Do *not*
use the old ``real*8`` method.

There are several definitions for precision:

``r8`` is double precision.

``r4`` is single precision.

``rMom`` is the precision of the second order moments (standard is
single precision).

``rAvg`` is the precision of average diagnostic arrays.

``rTnd`` is the precision of budget tendency arrays.

Most computers work faster on double precision than on single precision,
so you should use double precision for floating point numbers. Only for
very large arrays a gain can be achieved by using single precision,
since it may reduce the number of cache misses. See
Section [sxn:transport\_som] for an example of this.

Stay away from C-code
---------------------

From the start, the goal has been to keep the Oslo CTM3 free of C-code!
Write dummy routines instead; if you need an example, take a look in the
directory *OSLO* vs *OSLO*\ */DUMMIES* while you study the *Makefile*.

The only C-code allowed should be in the file *cmn\_size.F90* (and of
course the DUST code which I have not updated).

Looping in Fortran
------------------

Multidimensional arrays should be traversed in the natural ascending
storage order, which is column-major order for Fortran. This means that
the leftmost index varies most rapidly with a stride of one. For a loop
through the array ``ARR(IPAR,JPAR)``, the correct traversing is shown in
Table [table:flooping].

--------------

::

      do J=1,JPAR
        do I=1,IPAR
          ARR(I,J) = ...
        enddo
      enddo

--------------

If you want to set the whole array (e.g. initialize it), use

::

      ARR(:,:) = 0.

not just ``ARR = 0.``, which is possible: It makes the code easier to
read. If you want to initialize only grid boxes I=10 to 20 and J=4 to
10, you can use ``ARR(10:20,4:10) = 0.``. The compiler will chose the
most efficient way to handle this.

If you are accustomed to C programming, you should note that C uses
row-major order, where the rightmost index varies most rapidly. If you
put your 3D or 4D array from C coding into Fortran, it will be very
ineffective, striding at every step: If the array is of dimension
(``IPAR``,\ ``JPAR``,\ ``LPAR``,\ ``NPAR``), and you loop through it
C-wise,

--------------

::

      do I=1,IPAR
        do J=1,JPAR
          do L=1,LPAR
            do N=1,NPAR
              ARR(I,J,L,N) = ...
            enddo
          enddo
        enddo
      enddo

--------------

for every step of ``N`` (from ``1`` to ``NPAR``) it must jump (stride)
over ``IPAR``\ x\ ``JPAR``\ x\ ``LPAR`` memory locations to get to the
next ``N`` (see Table [table:clooping]). **You must not do this in
Fortran!**

There are of course times when this needs to be violated, for example
when rearranging arrays into temporary arrays, with different structures
(see e.g. Section [sxn:parallel]). In cases where the temporary array is
smaller, the largest arrays should be traversed in column-major order,
to keep the memory jumps as small as possible. Sometimes it may be
difficult to decide which way to loop.

Implicit none
-------------

Always use implicit programming, starting each subroutine with
``implicit none``. Explicit programming is very difficult to debug if
necessary, and should be avoided.

Transport
=========

Transport of atmospheric species is done by large scale advection,
convection and turbulent mixing. The latter is most important in the
boundary layer. The basis for the Oslo CTM3 transport is the Secondary
Order Moments scheme introduced by :raw-latex:`\citet{Prather1986}`,
which was later re-structured and documented in
:raw-latex:`\citet{PratherEA2008}`.

Secondary Order Moments
-----------------------

In addition to transporting mean grid box values, the first and second
order moments are also transported. The first order moments carry
information about the slope between grid boxes, while the second order
moments carry information about the curvature (the slope of the slope).

The 3 first order moments are ``SUT``, ``SVT`` and ``SWT``, and the
6 second order moments are called ``SUU``, ``SVV``, ``SWW``, ``SUV``,
``SUW`` and ``SVW``). In result, there are 9 moments that need to be
transported.

The moment array sizes are (``IPAR``,\ ``JPAR``,\ ``LPAR``,\ ``NPAR``),
and their units are the same as for the mean grid value (i.e. kg/grid
box). This may be somewhat counter-intuitive, but is explained in
:raw-latex:`\citet{Prather1986}`.

The horizontal mass fluxes due to advection is stored in two arrays, one
for zonal divergence (``ALFA``) and one for meridional (``BETA``).

::

      ALFA(I,J,L) ==> [I,J,L] ==> ALFA(I+1,J,L)
      BETA(I,J,L) ==> [I,J,L] ==> BETA(I,J+1,L)

Their units are kg/s. See *p-dyn0.f* for more.

It is easy to see that transporting 10 variables per component will
require a lot of CPU power, and that the memory requirements also are
relatively large. The mass amounts carried by the moments are small
compared to the gridbox tracer average (``STT``), and can therefore be
stored in single precision (defined by ``rMom``). However, in the
1D transport subroutine, everything is carried out in double precision
(``r8``). Overall, this makes the code faster, due to reduced code size
and hence reduced cache misses. It could be mentioned that conversion
from single to double precision takes some extra time, but the gain in
a reduced code size is much larger. Comparison with double precision
moments has been done, finding that single precision do introduce some
noise, but very small.

Advection
---------

Advection is carried out through the use of Secondary Order Moments
scheme, as described by :raw-latex:`\citet{PratherEA2008}`. The
transport papers are available for free at his web page [1]_.

The global time step is based upon a Lifshitz criterion, which in our
case is a divergence criterion :raw-latex:`\citep{PratherEA2008}`. The
transporting routine – ``qvect3`` – has an internal CFL criteria / time
stepping. The latter allows a shorter time step at high latitudes where
the grid boxes are smaller compared to low latitudes.

Note that the Lifshitz criterion and internal CFL criterion may not
handle very rigorous deep convection well. Testing meteorological data
from an earth system model indicates this, giving negative air mass
after vertical transport if the number of advection steps is not
increased. I have included a crude fix for this in the routine
``CFLADV`` in *p-dyn0.f* – if you run into this problem, you may try
that solution.

One of the major improvements from Oslo CTM2 is that polar grid boxes
are no longer combined in transport (the so-called extended polar
zones).

There is, however, one important update from the 2008 description. In
:raw-latex:`\citet{SovdeEA2012}`, the treatment of horizontal transport
at the polar caps was updated (see Section [sxn:transport\_adv\_hor]).

Horizontal advection
~~~~~~~~~~~~~~~~~~~~

The horizontal advection is carried out layer by layer, so that each CPU
works on a whole layer. The routines are ``DYN2UL`` and ``DYN2VL``,
located in *p-dyn2.f*.

In :raw-latex:`\citet{PratherEA2008}`, the polar cap treatment of
horizontal transport was to combine the polar boxes ``I`` and
``I+IPAR/2``, for meridional transport, while maintaining the gradients.
This did not work well, and was updated in 2011 to allow for a more
accurate treatment, which are described in
:raw-latex:`\citet{SovdeEA2012}`.

For meridional transport, the two pie-shaped boxes on opposite sides of
the poles are no longer combined, and the V-flux across the pole is
zeroed and instead added to the U-flux (transport around the pole
point). To avoid short transport time steps due to small masses in the
polar-pie grid boxes, the default treatment in Oslo CTM3 is to combine
boxes ``1:2`` and ``JPAR-1:JPAR``, for a given ``I``, and maintain the
moments. An optional, more accurate treatment, is to skip the combining
of boxes, but that is more time consuming due to a shorter global time
step required in transport. The latter treatment improves cross-polar
gradients, and should be considered when studying e.g. frozen-in
anti-cyclones or O\ :math:`_3` holes.

To use this optional treatment involves using the files *p-dyn0-v2.f*
and *p-dyn2-v2.f* instead of the standard *p-dyn0.f* and *p-dyn2.f*, and
is explained in detail in the horizontal transport section of .

Vertical advection
~~~~~~~~~~~~~~~~~~

The vertical advection is carried out column by column. Large scale
advection is computed from the continuity equation, as the global field
``GAMA``. In the IJ-blocks it is put into the field ``GAMAB``. Both
advection and subsidence due to convection (``GAMACB``) are transported
together. I.e. vertical advection must be calculated *after* convection
(Section [sxn:transport\_conv]).

As the advection routine ``qvect3`` needs the transport pipe to be of
*even number length*, care must be taken when using degraded vertical
resolution (L37/ or L57) (to get a transport pipe of even length).

There are two possible ways to create even length transport pipes, and
for very short arrays (e.g. 19 layers) this may also improve the speed
of the vertical advection:

For each component, stack some columns on top of each other, to be
transported as a longer pipe.

Stack several components from the same column in the longer pipe.

The number of stacked columns or components is given by ``IMDIV`` in
*cmn\_size.F90*, and is therefore chosen automatically by *Makefile*.

Due to the model structure, stacking two components from the same column
works better than stacking two columns, since the minimum time step
needed in the pipe may differ in two columns: Combining different
columns means that the column with the shortest time step forces the
other columns to take a shorter time step. E.g. convection may cause
this, since it can vary much from column to column.

Due to the structure of the B-arrays, this stacking of columns also
strides more than for tracer stacking, although that may not be a big
problem for computers to handle.

For L60/L40 resolution, the fastest vertical advection is achieved by no
stacking at all. For special meteorological conditions using ``IMDIV=1``
halved the operator splitting time step compared to ``IMDIV=4``.

Stacking components has, however, a small disadvantage; If ``NPAR`` is
not divisible with IMDIV, the remaining part of the transport pipe will
have to be filled with dummy tracers. The cost of this is small.
Choosing ``IMDIV=2`` ensures the least number of dummies.

At some point the cost of creating a long pipe will be larger than the
gain of using fewer pipes, reducing the efficiency of this method. For
L60 and L40 we use ``IMDIV=1``, while for L37 and L57 we use a pipe with
2 components, for both T42 and 1x1 horizontal resolution.

Convection
----------

Convective transport is calculated as a separate process, and the
subsidence due to convection is calculated as a mass flux (``GAMACB``).
``GAMACB`` is treated together with the large scale vertical advection
``GAMA`` in the same transport routine (``DYN2W_OC``).

The wet removal of gases due to convective rain is described in
Section [sxn:conv\_scav], whereas the transport is described here.

Convective transport is calculated using mass fluxes of updrafts and
downdrafts. The ECMWF IFS convective scheme is based on
:raw-latex:`\citet{Tiedtke1989}`, so we use the same reference for the
Oslo CTM3 convection.

Two important processes that occur in convection are entrainment and
detrainment. They can be separated into (1) turbulent exchange through
cloud edges and (2) organized exchanges. For updrafts the entrainment
can be noted

.. math:: E_{up} = E_{up}^{(1)} + E_{up}^{(2)}

 and detrainment

.. math:: D_{up} = D_{up}^{(1)} + D_{up}^{(2)}

If you look at the IFS documentations, you will see that the
parameterisations change from cycle to cycle. In general

.. math:: E_{up}^{(1)} = f D_{up}^{(1)}

 where :math:`f` may be unity or parameterized. :math:`E_{up}^{(1)}` is
proportional to the incoming mass flux and inverse proportional to the
cloud radii:

.. math:: E_{up}^{(1)} = f\frac{0.2}{R_{up}}\frac{M_{up}}{\overline{\rho}}

 where :math:`\overline{\rho}` is the air density. In this equation we
locate the fractional entrainment (m:math:`^{-1}`) as:

.. math:: \varepsilon_{up}^{(1)} = \frac{0.2}{R_{up}}

 See the IFS documentation for more on this and on equations for
:math:`E_{up}^{(2)}` and for detrainment.

| *Important*
| While the detrainment rates are given as [s:math:`^{-1}`] in the IFS
  documentation, the meteorological fields available (archived data) are
  mass flux per height, i.e. accumulated [kg/(m:math:`^3`\ s)].

Available mass flux fields in the meteorological data are

Updraft mass flux (``CWETE``)

Downdraft mass flux (``CWETD``)

Updraft detrainment rate

Downdraft detrainment rate

The detrainment rates are converted to entrainment mass fluxes ``CENTU``
for updrafts and ``CENTD`` for downdrafts. See
Appendix [app:met\_ctm\_mflux] for some details on this.

For a given grid box, the Oslo CTM3 treatment of convection due to
updrafts consists of three parts, considering

Mass flux in at bottom and out on top.

Entrainment into updrafts from ambient air.

Entrainment or detrainment to balance the net updraft mass fluxes and
entrainment.

It can be noted that the organized entrainment in the IFS model takes
place in the lowest part of the cloud, below the level of strongest
vertical ascent (explained in IFS documentation). This information is
lost for our use, but the balancing due to net flux will retrieve some
of the lost information.

In the convective routine the entrainment ``CENTU`` is retrieved as
``ENT_U`` and mass flux ``CWETE`` as ``FLUX_E``. For a given layer
``L``, with short notation, they are related as

.. math::

   F_E(L) + E_U(L) - D_U(L) = F_E(L+1)
     \label{eq:fluxbalance}

 where :math:`F_E` is the mass flux from bottom of the given level,
:math:`E_U` is air entrained and :math:`D_U` is the detrained air at the
same level (positive if detrained). In this way we allow detrainment to
act as a vent as the air is rising, possibly increasing the mixing with
the surrounding air in the process (detrainment will leave polluted mass
at lower levels, transporting less to the plume top).

The detrainment :math:`D_U(L)` is positive when air leaves the
convective plume and is lost to the surroundings. From
Eq. ([eq:fluxbalance]) we have:

.. math:: D_U(L) = - F_E(L+1) + ( F_E(L) + E_U(L) )

 However, it may be that this results in a negative :math:`D_U`, which
means that an additional amount of ambient air needs to be entrained
from the surroundings to balance the mass fluxes.

Downdrafts are explained in Appendix [app:met\_ctm\_mflux].

Boundary layer mixing
---------------------

The boundary layer mixing scheme is selected by the flag ``NBLX`` in
*LxxCTM.inp*. In the UCI code only the Prather scheme is available, but
in the Oslo CTM3 the Holtslag scheme has been included from qcode 55.

The boundary layer is mixed each chemical time step, before chemistry.

It is important to notice that boundary layer height (``BLH``) is
usually an instantaneous field, which may be problematic during morning
hours when photochemistry becomes effective – especially for thin
boundary layer heights.

A method for solving this is included, namely interpolating the ``BLH``
linearly in time between the current and the next meteorological time
step (``BLH_CUR`` and ``BLH_NEXT``, respectively). The routine is called
``set_blh_ij``, and is called from , in the ``CCYC``-loop. To do this,
each IJ-block counts its elapsed seconds of the meteorological time
step, in the variable ``nmetTimeIntegrated`` defined in .

| *Important*
| This means that when using the time interpolation, ``BLH`` should be
  used with care in other routines.

Except from the boundary layer mixing routine, ``BLH`` is put out in
several routines (vertical profiles and such), outside the IJ-block.
These routines put out values interpolated to each NOPS (if ``BLH_NEXT``
is available).

NBLX=1: Prather scheme
~~~~~~~~~~~~~~~~~~~~~~

The Prather bulk scheme uses e-folding time assuming full mixing in
3hours. The scheme is set up to be applied to collapsed bottom layers
(layer 1 consists of layer 1:3 and layer 2 of 4:5). The bulk scheme
should in principle be applicable to the full resolution, but it may be
too fast or slow.

It has been tested in the UCI CTM to do well compared with other
boundary layer schemes, although much simpler. Some boundary layer
parameters are still calculated.

The Oslo CTM3 dry deposition used diffusitivities (``PBL_KEDDY``) for
the lowermost model level, which were only calculated in the Holtslag
method. A separate calculation of ``PBL_KEDDY`` has been included in the
Prather scheme.

NBLX=5: Holtslag
~~~~~~~~~~~~~~~~

The :raw-latex:`\citet{HoltslagEA1990}` k-profile scheme has been
retrieved from the previous version of the UCI model (qcode 55). The
boundary layer height needs to be doubled due to catch the whole
boundary layer. In L40, a maximum of 9000m was used, but for L60 this
had to be lowered to 8000m.

::

       ZBL = min(BLH(i,J)*2.d0,8000.d0)

Other schemes
~~~~~~~~~~~~~

No other schemes are available, but qcode 55 also had code for H&R
(NBLX=2), Louis (NBLX=3) and M-Y2.5 (NBLX=4).

Wet and dry scavenging
======================

Dry deposition is the process where gases are deposited on the ground,
i.e. either through gravitational settling of by uptake processes in the
soil or in plants. Thus it applies only to the lowermost model level.

Wet deposition, or scavenging, is when gases or aerosols are removed by
precipitation.

Wet scavenging
--------------

Wet scavenging is usually divided into three types:

*Rainout:* Used for aerosols when they act as cloud condensation nuclei
(CCN) and fall out as rain.

*Washout:* Gases/aerosols are deposited on rain drops. This is the usual
mechanism for scavenging gases.

*Sweepout:* When the rain droplets collect molecules or aerosols.
Sometimes called *impact washout*.

In Oslo CTM3 we treat washout for both gases and aerosols, since the
meteorology (rain) is prescribed: We do not calculate the precipitation
from CCN. But the large scale scavenging scheme does also calculate
sweepout of species with mass limited washout (i.e. species which easily
stick to water, such as HNO\ :math:`_3` and some aerosols), called
impact washout in the code.

It should be noted that the washout process is treated differently for
large scale and convective precipitation.

The wet scavenging parameters are found in the file
*scavenging\_wet.dat*, as listed in
Tables [table:scavlist1]–[table:scavlist3]. It differs from the original
UCI file, which is also available in *scavng55.dat* for the interested
reader. In general, the scavenging follows Henry’s law, so coefficients
for this are listed. How to specify more sophisticated effective
expressions is explained in Section [sxn:henryhardcode]. The settings
apply in general to the large scale wet scavenging, but there are a few
options that only apply for convective scavenging.

The wet scavenging list contains parameters for convective scavenging
and large scale scavenging – where some are specific for either
convective or large scale, but most apply for both. Here follows a list
of the parameters, which are also described at the end of the scavenging
list.

``SOLU``: Fraction of grid box available for wet scavenging. Applies for
both convective and large scale scavenging. For convective scavenging,
this must be accompanied by setting the flag ``CHN``

| ``CHN``: Defines treatment of convective scavenging, with the
  possibility to switch off liquid or ice large scale scavenging.
  Options are listed at the bottom of the scavenging file, and the most
  used flags are 0, 1 or 3. Large scale scavenging is treated unless
  stated otherwise:

No convective scavenging.

Fraction dissolved is calculated from Henry coefficients, either using
Henry’s law or mass-limited. This fraction is multiplied with ``QFRAC``
to get the fraction removed by scavenging
(Section [sxn:conv\_scav]–[sxn:conv\_scav\_henrytheory]).

Not in use.

Assumes fully dissolved tracer, so that ``QFRAC`` gives the fraction
removed.

Removes fraction given by ``SOLU`` and *not* ``QFRAC``. In other words:
Everything is removed for ``SOLU=1``. Should be used with care!

Same as (3), but large scale scavenging is turned off.

Same as (3), but large scale scavenging on liquid is turned off. Large
scale ice scavenging is included (if defined by ``ISCVFR``).

Convective removal only if minimum temperature in convective plume is
lower than 258K. Large scale ice scavenging is included, but not liquid
scavenging.

Convective removal only if minimum temperature in convective plume is
lower than 258K and maximum temperature in plume is lower than 273K.
Large scale ice scavenging is included, but not liquid scavenging.

``TCHENA``: First part of Henry expression, i.e. Henry coefficient at
298K.

``TCHENB``: Exponential part of Henry expression, i.e. the temperature
coefficient.

``TCKAQA``: Flag for denoting that Henry expression should be modified
by hard-coded settings. See [sxn:henryhardcode] for more.

``TCKAQB``: Zero is removal according to Henry expression, non-zero is
mass (or kinetically) limited removal, which is used for highly soluble
species.

``ISCVFR``: Fraction of gridbox available for large-scale ice
scavenging.

| ``IT``: Ice treatment when ``ISCVFR``>0.

No scavenging below 258K.

For temperatures below 258K use :raw-latex:`\citet{KarcherVoigt2006}`.

Use same treatment as for 258K–273K, i.e. with retention coefficient.

No removal for :math:`T`\ <258K, but set retention coefficient to 1
instead of 0.5.

Standard treatment (Henry’s law or kinetically limited) below 258K (as
in option 2), but set retention coefficient to 1 instead of 0.5.

Large scale scavenging
~~~~~~~~~~~~~~~~~~~~~~

The large scale scavenging master routine located in ``WASHOUT0``. While
the model in principle can use a simplified scheme (``WASHOUT1``), it
has been disabled for Oslo CTM3; we only use the more sophisticated
version by :raw-latex:`\citet{NeuPrather2012}` (``WASHOUT2``), which
scavenge separately by liquid and ice precipitation. It is still
possible to choose the simple scheme by changing the parameter ``NSCX``
in the input file *LxxCTM.inp*, but the data needed is not read into the
model. If you need that data, you can find it in *scavng55.dat*.

The ``WASHOUT2`` is a simple cloud model, dividing each grid box layer
in four parts:

Cloud core, with rain coming in from above. Depending on how much rains
out, rain may evaporate or be formed.

Cloudy, with no rain coming in, but rain may form.

Clear sky with rain from above.

Clear sky with no rain.

Fractional areas are calculated and may change e.g. due to evaporation.
A constant evaporation rate is used. More details are explained by
:raw-latex:`\citet{NeuPrather2012}`.

For some species, such as HNO\ :math:`_3` and some aerosols, uptake on
ice may be important. Uptake on ice is controlled by a non-zero ice
scavenging fraction ``ISCVFR`` in the scavenging list, denoting how much
of the grid box is available for ice scavenging. For 258K<T<273K, the
uptake is generally calculated using Henry’s law and the table-specified
Henry’s law coefficients, modified by a retention coefficient. This is
because Henry expressions are not given for temperatures below
0\ :math:`^{\circ}`\ C, and currently the retention coefficient is set
to 0.5 :raw-latex:`\citep{NeuPrather2012}`. Mass limited ice removal of
species is calculated assuming a Henry coefficient of
typically 10\ :math:`^8`, which will yield the species completely
dissolved, even if the retention coefficient is 0.5.

Note that the retention coefficient can possibly be overwritten by the
``IT`` flags. In the future, it could be that the retention coefficient
could become part of the scavenging table.

Uptake of HNO\ :math:`_3` on ice can also occur below 258K, and follows
:raw-latex:`\citet{KarcherVoigt2006}` when ``IT`` is set to 1. Other
options are also available.

Convective scavenging
~~~~~~~~~~~~~~~~~~~~~

Convective scavenging is adopted from the Oslo CTM2, and does not
separate between ice and liquid water; all is treated as rain. It
differs from the UCI method, which is rather crude. The routines are
called from ``CONVW_OC`` and are located in *cnv\_oslo.f90*.

If you need to do changes in that file, be certain that you understand
the units used; rain, liquid water and mass fluxes are kg/s. Note that
these values are accumulated in the meteorological data files, and then
converted. Entrainment into updrafts is originally not flux, and is
described in Section [sxn:transport\_conv].

Convection forms an elevator (or plume) transporting mass upwards. To
calculate the convective scavenging we need to know how much of a tracer
that is solved in the liquid water of the elevator (``elev_mass_lw``),
which is described at the end of this section. The ``elev_mass_lw`` also
covers the rain in the elevator.

The fraction of rain to liquid water in the elevator is called
``QFRAC``. Given an amount of tracer solved in the elevator liquid
water, the fraction ``QFRAC`` is subject for removal.

Calculation is done using the mass fluxes from the meteorological data.
At the lowest level of entrainment, we entrain air and humidity from the
surroundings, to form the base of the elevator. It is then lifted
according to the mass fluxes, and assuming adiabatic lifting, the
elevator temperature cools and eventually water will condense. The
condensed water goes into ``elev_mass_lw``. Entrainment or detrainment
is then calculated, before we remove the net rain out of the box. We do
not consider net rain into the box to increase ``elev_mass_lw``; with
our simplified elevator, this unfortunately not possible.

Eventually we reach the elevator top (determined by the mass fluxes) and
we have the following data for each level of the elevator: Amount of
liquid water, volume of elevator and volume fraction of liquid water
(droplets) in the elevator, which is called ``LW_VOLCONC`` (e.g. volume
concentration). ``LW_VOLCONC`` is used in calculating the amount of
tracer solved in the elevator. Some species are dissolved completely
(e.g. HNO:math:`_3`) and others are dissolved according to Henry’s law
(Section [sxn:conv\_scav\_henrytheory]).

In the scavenging list, the parameters ``SOLU`` and ``CHN`` defines
whether or not each tracer is washed out by convection. These data are
stored in the model arrays ``TCWETL(NPAR)`` and ``TCCNVHENRY(NPAR)``,
respectively. ``CHN`` controls how to calculate the fraction of tracer
removed. Options are listed at the bottom of the scavenging file, and
also in the previous Section.

The use of Henry’s law to find the fraction of tracer dissolved in the
elevator is described in the next Section.

Theory on Henry’s law
~~~~~~~~~~~~~~~~~~~~~

Looking at the convective scavenging code, you find a fraction of
dissolved tracer on the form

.. math:: f_{dissolved} = \frac{H_H LW_{volconc}}{H_H LW_{volconc} + 1}\label{eq:fdis}

 A similar expression can be found in the large scale scavenging routine
(subroutine HENRYS, although it is different below 273K). I will explain
the background of this equation here.

First of all, there are several data on Henry coefficients; usually they
are given at 298K, together with a temperature coefficient. Another
possibility, as in :raw-latex:`\citet{jpl10-06}`, coefficients are given
for the expression:

.. math:: \ln H(T) = A + \frac{B}{T} + C\,\ln T

 Coefficient :math:`C` is rarely used, and while :math:`B` is used by
the Oslo CTM3, I stress that this :math:`A`-coefficient differs from the
model definition: Oslo CTM3 applies the well used van’t Hoff’s
temperature extrapolation from 298K. This extrapolation assumes that
Henry’s law varies as

.. math:: \frac{d\,\ln H}{d\,T} = \frac{\Delta H_{sol}}{RT^2}

 where :math:`\Delta H_{sol}` is the solution enthalpy.
:math:`\Delta H_{sol}` is assumed constant (a fairly good
approximation), so that the expression can be re-arranged and integrated
between temperatures :math:`T_1` and :math:`T_2`:

.. math::

   H(T_2) = H(T_1)\,\exp\left[\frac{\Delta H_{sol}}{R}
              \left(\frac{1}{T_1}-\frac{1}{T_2}\right)\right]

 The temperature dependence

.. math:: \frac{\Delta H_{sol}}{R} = - \frac{d\,\ln H}{d(\frac{1}{T})}

 is the :math:`B` coefficient given e.g. by
:raw-latex:`\citet{jpl10-06}` and also used in the Oslo CTM3. Hence,
using :math:`T_2=298`\ K, the model finds the Henry expression at any
temperature:

.. math::

   H(T) = H(298\,\mathrm{K})\,\exp\left[B
              \left(\frac{1}{T}-\frac{1}{298\,\mathrm{K}}\right)\right]

 This means that the :math:`A`-coefficient used in the model (in
scavenging table), is in fact :math:`H(298\,\mathrm{K})`.

The following explanation applies to the convective scavenging, where
:math:`LW_{volconc}` is calculated. In the routine for large scale
scavenging, :math:`LW_{volconc}` is not explicitly calculated, which is
why Eq. ([eq:fdis]) differs slightly in that routine. However, the
basics behind it is the same.

| *Important notice*
| When Eq. ([eq:fdis]) is used and :math:`LW_{volconc}` is calculated,
  as in the convective routine, :math:`H(T)` has to be modified to get
  the correct units, i.e. we have to multiply it by :math:`R\,T`, where
  :math:`R=0.08206`\ atmLmol\ :math:`^{-1}`\ K\ :math:`^{-1}`.

| **Deriving Eq. ([eq:fdis])**
| Henry’s law coefficient for any gas is defined as

  .. math:: P_{gas} = k_H  X

   where :math:`P_{gas}` is the partial pressure of the gas above the
  solution, and :math:`X` is the molar fraction of the dissolved gas in
  the solution;

  .. math:: X = \frac{n_{aq}}{n_{aq} + n_{solvent}}

   where :math:`n_{aq}` is the number of moles solved, and
  :math:`n_{solvent}` is the number of moles of the solvent (i.e. water
  in our case).

Assuming ideal solution, we can change to concentration by dividing by
volume of the solution, and get:

.. math:: X = \frac{C_{aq}}{C_{aq} + C_{solvent}}\nonumber

 As long as Henry’s law applies, i.e. the species is *not* highly
soluble, we always have :math:`C_{aq} \ll C_{solvent}`, and can
approximate to:

.. math:: X = \frac{C_{aq}}{C_{solvent}}

Since the concentration of the solvent (water) is approximately
constant, we arrive at the other common form of Henry’s law:

.. math:: P_{gas}= k  C_{aq}

Units for :math:`k`: [atm L(solvent)/mol], and for :math:`P_{gas}`:
[atm].

If :math:`k` is high, it means the component prefers thermodynamically
to be in gas phase.

We are interested in calculating :math:`C_{aq}` from :math:`P_{gas}`, so
we introduce

.. math:: H_{STAR} = 1/k

 with units [mol/(atm L(solvent))], so that

.. math:: C_{aq} = H_{STAR}  P_{gas}

If we want to apply the calculations to molar concentration (mol/L), we
have to change some units:

.. math:: P_{gas} = C_g  R  T

 given correct units of R, i.e. [atm L /(mol K):

J = kgm\ :math:`^2`/s:math:`^2` = Pa m\ :math:`^3`

| R = 8.31451J/(mol K) / 101325Pa/atm
| x 1000 L/m\ :math:`^3` = 0.0820578 atm L/(mol K)

Henry’s law therefore implies that the concentration in the solution is
proportional to the atmospheric concentration:

.. math:: C_{aq} = H_{STAR}  R  T  C_g = H_H  C_g\label{eq:henry_caq}

 where :math:`H_H` has the units [mol/L(solvent) / (mol/L(air))].

We want the mass fraction of the dissolved gas, which equals the molar
fraction :math:`f_{dissolved}`.

.. math:: f_{dissolved} = \frac{n_{aq}}{n_{aq} + n_g}

The volumes have units [m:math:`^3`], while the number concentration of
tracer in air, :math:`C_g`, has units [mol/L(air)]. Hence, we get the
moles of tracer in gas phase:

.. math::

   n_g  = C_g  V_{elevator\_air}
        \frac{10^3\textrm{L(air)}}{\textrm{m}^3\textrm{(air)}}

:math:`C_{aq}` has units [mol/L(solvent)], and the number of moles in
the solution is

.. math::

   \begin{aligned}
     n_{aq} &=& C_{aq}  V_{elevator\_solvent}\label{eq:n_aq}\\
            &&  \cdot10^3\textrm{L(solvent)/m}^3\textrm{(solvent)}\nonumber\end{aligned}

As already explained, the solvent is liquid water. The volume of the
solvent is given by the liquid water content, and is calculated in
elevator fractions as volume concentration, i.e. volume of liquid water
in elevator to total elevator volume. As can be found in the source code
comments, the volume of liquid water is

.. math:: V_{elevator\_solvent} = \frac{\textrm{mass of liquid water}}{\rho}

 where :math:`\rho` is the density of water (which is
:math:`10^3`\ kg/m\ :math:`^3`). The liquid water volume concentration
is then

.. math::

   LW_{volconc} = \frac{V_{elevator\_solvent}}{V_{elevator\_air}}
     \label{eq:lwvolconc}

Using Equation ([eq:n\_aq]) and ([eq:lwvolconc]), the moles of gas
dissolved in water is

.. math::

   \begin{aligned}
     n_{aq} &=& C_{aq} LW_{volconc} V_{elevator\_air}\\
            &&    \cdot10^3\textrm{L(solvent)/m}^3\textrm{(solvent)}\end{aligned}

The mass fraction dissolved in the droplets (mass fraction equals mole
fraction in this case), which is subject to washout, is therefore

.. math::

   \begin{aligned}
     f_{dissolved} &=& \frac{n_{aq}}{n_{aq} + n_g}\nonumber\\
             &=& \frac{C_{aq} LW_{volconc}}{C_{aq} LW_{volconc} + C_g}\end{aligned}

 and by Equation ([eq:henry\_caq]) we get

.. math::

   f_{dissolved} = \frac{H_H LW_{volconc}}{H_H LW_{volconc} + 1}
     \label{eq:fdissolved}

 In the Oslo CTM3 code this fraction is called ``DISSOLVEDFRAC``.

When the retention coefficient is used in large scale scavenging, it is
multiplied by :math:`H_H` in Equation ([eq:fdissolved]).

Hard-coded Henry coefficients
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Some tracers have empirical Henry constants, which need to be
hard-coded. This is specified by a non-zero ``TCKAQA`` in the scavenging
list *scavenging\_wet.dat*.

The hard coding is done in the routine ``getHstar`` in
*scavenging\_largescale\_uci.f90*, called by either the large scale
scavenging routine or the convective scavenging routine.

Dry deposition
--------------

Deposition velocities are stored in ``VDEP`` of size
(``NPAR``,\ ``IPAR``,\ ``JPAR``), thus allowing for possible deposition
of any transported tracer.

``VDEP`` is given in units [m/s], and is set in subroutine ``setdrydep``
in the file *drydeposition\_oslo.f90*, which is called from .

| **Important 1**
| When these deposition velocities are to be used in QSSA, they have to
  be divided by the height of the lowermost layer, to get the
  unit [s\ :math:`^{-1}`]. This is carried out e.g. before chemistry
  (``MASTER_OSLO``) and in subroutine ``bcoc_master``.

| **Important 2**
| Inspired by CTM2-tests done by others, I started in 2013–2014 to make
  an update of the dry deposition scheme. However, I never got to finish
  it. Early 2018, I got a small opportunity to look at it again, and
  made some small changes, so the new scheme may be tested properly.

The Oslo CTM2 parameterisation, which is the standard scheme, is
explained very briefly in Section [sxn:drydeptech\_history], while the
new scheme is described in
Section [sxn:drydeptech\_new]–[sxn:drydeptech].

Historical note
~~~~~~~~~~~~~~~

In the Oslo CTM3 :raw-latex:`\citep{SovdeEA2012}` and its predecessor
Oslo CTM2 dry deposition has been parameterised based on
:raw-latex:`\citet{Wesely1989}`, not by calculating the deposition
velocities from equations, but rather by using seasonal day and night
averaged deposition velocities for different land use types. However,
some velocities seem to rather come from :raw-latex:`\citet{Hough1991}`.

Using 5 land-use types (water, forest, grass, tundra/desert and
ice/snow) in each gridbox, a mean velocity was then defined. Day and
night was defined by using the solar zenith angle (subroutine
``SOLARZ``), giving day if less than 90\ :math:`^{\circ}`. Winter was
defined as temperatures below 273.15K for grid boxes containing land
masses. Ocean gridboxes containing sea ice use ice/snow values, but
a distinction on season was not necessary because the summer and winter
values are equal. However, snow cover was taken into account, reducing
uptake when snow thickness was about 1m (10cm water equivalents) in
forest and 10cm (1cm water equivalents) on grass/tundra.

The old UCI read-in is disabled, but a short note about it is given in
Section [sxn:drydep\_uci].

The new dry deposition scheme
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

The dry deposition parameterisation is in the process of being updated
to follow the method of the EMEP model
:raw-latex:`\citep{SimpsonEA2012}`. It is a more physical approach and
is described in detail in Section [sxn:drydeptech].

The EMEP method is used for the gaseous species O\ :math:`_3`,
H\ :math:`_2`\ O\ :math:`_2`, NO\ :math:`_2`, PAN, SO\ :math:`_2`,
NH\ :math:`_3`, HCHO, CH\ :math:`_3`\ CHO. CO has a very small uptake
and is not included in the EMEP treatment, so we keep the old
Oslo CTM2 parameterisation.

Some of the aerosol deposition rates follow the EMEP aerosol
parameterisation, namely BC/OC aerosols, sulphur aerosols (SO:math:`_4`
and MSA), and secondary organic aerosols (SOA). Other aerosol modules
have their own parameterisations which are described in their own
sections of this manual.

My first tests showed that the new parameterisation improved the ability
to reproduce measured surface O\ :math:`_3`. Generally, the largest
impacts can be found for O\ :math:`_3`, HNO\ :math:`_3`, SO\ :math:`_2`
and NH\ :math:`_3`, but also for SO\ :math:`_4` due to changes in
SO\ :math:`_2`.

However, the 2018 version of the new scheme uses a more physically
correct value of the stomatal conductance, calculated by the MEGANv2.10
module. It is also possible to use a climatology of stomatal
conductance, but I would not recommend it.

Tropospheric burden of O\ :math:`_3` and HNO\ :math:`_3` increased
by 5–8% and 5–10%, respectively. A \ :math:`\sim`\ 20% decrease was
found for tropospheric SO\ :math:`_2` and SO\ :math:`_4`, while
NH\ :math:`_3` tropospheric burden decreased by 20–30%. The other
species do not change much (0–3%). Interestingly, NO\ :math:`_2`
decreases slightly during winter but increases more during summer,
showing that secondary effects coming from chemistry are also important.

Technical description: gaseous species
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Typically, deposition uptake follows an electric circuit analogy, where
the deposition velocity :math:`v_d^i` for species :math:`i` is

.. math:: v_d^i = \frac{1}{R_a + R_b^i + R_c^i}\label{eq:vdi}

 where :math:`R_a` is the aerodynamical resistance between the surface
and the top of the vegetation canopy (i.e. the altitude :math:`z_0`,
called roughness length), :math:`R_b^i` is the quasi-laminar layer
resistance to the gas and :math:`R_c^i` is the canopy resistance (often
called surface resistance and sometimes denoted \ :math:`r_s^i`).

:math:`R_a` is the same for all gases, depending only on the surface/air
properties. :math:`R_b^i` and :math:`R_c^i` are different for all gases,
and the latter also vary from vegetation type to vegetation type.

The inverse of a resistance is called conductance, which is
denoted \ :math:`G`:

.. math:: G = \frac{1}{R}

 Conductance is in essence the same as velocity. This is important to
keep in mind when calculating an average deposition value in a gridbox.

If Eq. ([eq:vdi]) is the gridbox average, then the :math:`R`\ s are grid
box effective averages. This must be kept in mind when calculating grid
box averages of :math:`R_c^i`, which depend on land-use types; then we
need to calculate resistances for all vegetation types and do a proper
weighting to get the gridbox average.

To do this weighting we recognise that a molecule can only choose one
land type; it will not take the least resistance of several land-use
types.

As an example, assume we have 2 land-use types with gridbox fractions
being 50% each, with resistances :math:`R_1=1`\ s/m and
:math:`R_2=10000`\ s/m. A molecule is located above one of these
surfaces and cannot choose where to go. This means that several land-use
types, e.g. forest and grass, cannot occupy the same area when the area
is infinitesimally small (approaches zero).

Each of these two areas would have their respective velocities
:math:`v_i=1/R_i`, i.e. \ :math:`v_1=1`\ m/s and
:math:`v_2=10^{-4}`\ m/s. The small velocity will remove little, but if
the big velocity would remove almost everything, it would in this case
remove half of the species in the whole gridbox.

Then it is easy to see that the gridbox effective average :math:`R` is
found by

.. math:: \frac{1}{R} = f_1 \cdot \frac{1}{R_1} + f_2 \cdot\frac{1}{R_2}

 which is actually the average conductance

.. math:: G = f_1\cdot G_1 + f_2 \cdot G_2

giving :math:`v \approx 0.5`\ m/s. If :math:`v=1`\ m/s would remove all
in the gridbox, :math:`v=0.5`\ m/s removes half, as expected.

Had we used (:math:`f_1*R_1+f_2*R_2`) as the average resistance, the
velocity would be :math:`0.0002`\ m/s, which is clearly not what we
want.

Hence, it is the velocities (or conductances) which have to be weighted
according to land-use fractions, to get a gridbox mean velocity.

The new dry deposition scheme calculates :math:`R_a`, :math:`R_b^i` and
:math:`R_c^i` following :raw-latex:`\citet{SimpsonEA2012}`, which will
be referred to as EMEP2012. Main gases included are O\ :math:`_3`,
SO\ :math:`_2`, NH\ :math:`_3`, NO\ :math:`_2`,
H\ :math:`_2`\ O\ :math:`_2` and HNO\ :math:`_3`, but in addition I have
included NO, HCHO and CH\ :math:`_3`\ CHO which to some extent are also
subject to dry deposition.

As already noted, dry deposition of CO is still treated with the old
scheme. This is because it is not available through EMEP2012. In
general, CO dry deposition is so small that models tend to exclude it.
It is set to 0.03cm/s over vegetated areas
:raw-latex:`\citet{ConradSeiler1985}`, and it is reduced when there is
snow.

Aerosols are not part of the new update and follows the old deposition
treatment, using fixed deposition velocities or calculated separately as
in the sea salt or mineral dust modules.

| **Important**
| To use this treatment, both the nitrate and sulphur modules should be
  included. They are needed for calculation of SO\ :math:`_2` deposition
  which is again needed to calculate dry deposition velocities for other
  gases. Note that :raw-latex:`\citet{SovdeEA2012}` concluded that these
  modules should be included anyway; doing so does not increase
  computing time by much. Still, if sulphur and nitrate are not
  included, a monthly model climatology of the parameters :math:`a_{sn}`
  and :math:`a_{sn}^{24h}` is needed.

However, this climatology has not yet been produced, so if you try to
run the model without sulphur and nitrate modules, the program will
stop.

| **Aerodynamical resistance R\ :math:`_a`**
| The aerodynamical resistance (:math:`R_a`) in EMEP2012 is not well
  defined in :raw-latex:`\citet{SimpsonEA2012}`. They first assess
  friction velocity :math:`u_*` by using stability functions (Eq. (52)
  in EMEP2012):

  .. math::

     u_* = \frac{V_H(z)\,k}{
         \ln\left(\frac{z-d}{z_0}\right)
          - \Psi_m\left(\frac{z-d}{z_0}\right)
          - \Psi_m\left(\frac{z_0}{L}\right)}

   Here, :math:`V_H(z)` is the wind speed at their reference height
  :math:`z` and :math:`\Psi_m` represents the integrated stability
  equations for momentum, :math:`d` is a constant (typically 0.7m), and
  :math:`L` is the Obukhov length. However, they do not list the
  equation for :math:`R_a`.

A somewhat similar expression is found for :math:`R_a` in the earlier
EMEP version :raw-latex:`\citep[EMEP2003,][]{SimpsonEA2003}`, namely

.. math::

   R_a = \frac{1}{k\,u_*}\left[
       \ln\left(\frac{z-d}{z_0}\right)
        - \Psi_h\left(\frac{z-d}{z_0}\right)
        - \Psi_h\left(\frac{z_0}{L}\right) \right]\label{eq:ra1}

However, for certain values of :math:`z`, :math:`z_0` and :math:`L`,
Eq. ([eq:ra1]) can produce a negative :math:`R_a`, which is wrong. This
also applies to the :math:`u_*` calculation of EMEP2012.

Therefore we use a different approach in Oslo CTM3, namely the method of
:raw-latex:`\citet{Monteith1973}`. Sensible heat flux (:math:`SHF`)
between surface and air is given by

.. math:: SHF = \rho c_p \frac{T_0-T_z}{R_{a,H}}\label{eq:ra2}

 where :math:`\rho` is air density, :math:`c_p` the specific heat at
constant pressure, :math:`T_0` is the surface temperature, :math:`T_z`
is the temperature at reference height :math:`z`, and the aerodynamic
resistance :math:`R_{a,H}` is the diffusion resistance to sensible heat
transfer between surface and the reference height \ :math:`z`. In
Oslo CTM3 it is assumed that :math:`R_a=R_{a,H}`.

Sensible heat flux can also be written

.. math:: SHF = -\rho c_p K_H\frac{\partial T}{\partial z}\label{eq:monSHF}

 and momentum flux using the wind :math:`u` profile

.. math:: MF = -\rho u_*^2 = \rho K_M\frac{\partial u}{\partial z}\label{eq:monMF}

 where :math:`K_H` and :math:`K_M` are the respective eddy
diffusitivities. When :math:`K_M=K_H`, Eq. ([eq:monSHF]) and
([eq:monMF]) gives

.. math:: SHF = - \rho c_p \frac{\partial T}{\partial u} u_*^2

 which in finite differences between surface and reference height
:math:`z` can be written

.. math:: SHF = \rho c_p (T_0-T_z) \frac{u_*^2}{u_z}

 where :math:`T_z` and :math:`u_z` are temperature and wind speed at
height \ :math:`z`. From Eq. ([eq:ra2]) we get the
:raw-latex:`\citet{Monteith1973}` equation for :math:`R_{a,H}`:

.. math:: R_{a,H} = \frac{u_z}{u_*^2}

 This is what we use for :math:`R_a` in Oslo CTM3, and as reference
height we use the midpoint of the surface model level. This level is
about 16m thick, so the reference height is about 8m. This is possible
because both :math:`u_z` and :math:`u_*` are available from the
meteorological data.

| **Quasi-laminar layer resistance R\ :math:`_b`**
| :math:`R_b^i` is species specific, and is defined differently over
  land and ocean. When a gridbox contain both land and ocean, a weighted
  mean :math:`R_b^i` is calculated using the respective land and ocean
  conductances.

Over land we use

.. math:: R_b^i = \frac{2}{k\,u_*} \left(\frac{Sc_i}{Pr}\right)^{2/3}

 where :math:`Pr` is the Prandtl number (0.72) and :math:`Sc_i` is the
Schmidt number for gas :math:`i`, defined as:

.. math:: Sc_i = \frac{\nu}{D_i}

 Here :math:`\nu` is the kinetic viscosity of air and :math:`D_i` is the
molecular diffusivity for gas :math:`i`. If you look at the code, you
will see that the :math:`Sc_{H_2O}=0.6` as well as
:math:`D_{H_2O}=0.21\cdot 10^{-4}` are defined, and that :math:`Sc_i` is
found from

.. math:: Sc_i = Sc_{H_2O}\frac{D_{H_2O}}{D_i}

 where :math:`D_{H_2O}/D_i` is listed in Table [table:wesely].

Over sea we use

.. math::

   R_b^i = \frac{1}{k\,u_*} \ln\left(\frac{z_0}{D_i}\,k\,u_*\right)
     \label{eq:rbiocean}

 with minimum limit of 10s/m and maximum of 1000s/m.

:math:`z_0` is available from monthly mean ISLSCP2/FASIR but have zero
value over ocean. Hence, a different approach is taken to find
:math:`z_0` over water; assuming different approaches over calm and
rough ocean, separated by wind speed of 3m/s. Calm sea follows
e.g. :raw-latex:`\citet{Hinze1975}` or :raw-latex:`\citet{Garratt1992}`,
but with a slightly higher coefficient:

.. math:: z_{0,w,calm} = \min\left(2\cdot10^{-3},\,0.135 \frac{\nu}{u_*} \right)

 where :math:`\nu` is kinematic viscosity for air, calculated from
surface pressure (:math:`P_{sfc}`), temperature (2-meter
temperature \ :math:`T_{2M}`) and gas constant for air
(:math:`R_{air}`):

.. math:: \nu = \frac{6.2\cdot10^{-8}\,T_{2M}}{\frac{P_{sfc}}{T_{2M}\,R_{air}}}

 Here the numerator is absolute viscosity and the denominator is air
density.

Rough sea follows the method of :raw-latex:`\citet{Wu1980}`, which is
the :raw-latex:`\citet{Charnock1955}` method with slightly higher
coefficient.

.. math:: z_{0,w,rough} = \min\left(2\cdot10^{-3},\, 0.018 \frac{u_*^2}{g} \right)

In addition, there is a maximum roughness length limit of 2mm imposed on
both cases. However, the calculated :math:`z_{0,w}` is usually so small
that the limits on Eq. ([eq:rbiocean]) kicks in, so in practice
:math:`z_{0,w}` could very often have been zero in this
parameterisation.

+--------------------------------+------------------------+-----------------+-------------------------+
| Species :math:`i`              | :math:`D_{H_2O}/D_i`   | :math:`f_0^i`   | :math:`H_*^i` [M/atm]   |
+================================+========================+=================+=========================+
| O\ :math:`_3`                  | 1.6                    | 1.0             | 10\ :math:`^{-2}`       |
+--------------------------------+------------------------+-----------------+-------------------------+
| SO\ :math:`_2`                 | 1.9                    | 0.0             | 10\ :math:`^5`          |
+--------------------------------+------------------------+-----------------+-------------------------+
| NO\ :math:`_2`                 | 1.6                    | 0.1             | 10\ :math:`^{-2}`       |
+--------------------------------+------------------------+-----------------+-------------------------+
| H\ :math:`_2`\ O\ :math:`_2`   | 1.4                    | 1.0             | 10\ :math:`^5`          |
+--------------------------------+------------------------+-----------------+-------------------------+
| HCHO                           | 1.3                    | 0.0             | :math:`6\cdot10^3`      |
+--------------------------------+------------------------+-----------------+-------------------------+
| NO                             | 1.3                    | 0.0             | :math:`2\cdot10^{-3}`   |
+--------------------------------+------------------------+-----------------+-------------------------+
| CH\ :math:`_3`\ CHO            | 1.6                    | 15.0            | 0.0                     |
+--------------------------------+------------------------+-----------------+-------------------------+

Table: Values of :math:`D_{H_2O}/D_i` and :math:`f_0^i` and
:math:`H_*^i`.

| **Surface resistance R\ :math:`_c`**
| Surface resistance (:math:`R_c^i`) consists of both stomatal and
  non-stomatal resistances. To find the gridbox average we use
  conductances, as explained earlier:

  .. math:: G_c^i = LAI \cdot G_{sto} + G_{ns}^i

   where :math:`LAI` is the leaf area index (zero for non-vegetated
  surfaces), :math:`G_{sto}` is the stomatal conductance and
  :math:`G_{ns}^i` is the non-stomatal conductance.

This is the so-called big leaf assumption, where :math:`G_{sto}` is leaf
stomatal conductance. It is fetched from the MEGANv2.10 module (see
Section [sxn:emissions\_megan]), for each of the vegetation types.
An average is calculated to use in the dry deposition scheme. The
:math:`LAI` used, is the Oslo CTM3 field taken from ISLSCP2/FASIR.

It is also possible to use a monthly mean :math:`G_{sto}`, and there are
fields available from the LPJ group (provided through HYMN project).
These are, as far as I understand, canopy stomatal conductances, which
should not be multiplied by :math:`LAI`.

| *Stomatal conductance*
| Note that EMEP2012 calculate :math:`G_{sto}` from a maximum stomatal
  conductance :math:`g_{sto}` multiplied by functions to take
  temperature, light, etc. into account. However, when using
  :math:`G_{sto}` from MEGAN, the light dependency is already taken into
  account. For monthly means, a light scaling is necessary. Both
  requires a temperature scaling.

The light scaling for monthly conductances can be done using the
photosynthetically active radiation (PAR) available through the
meteorological data, multiplying :math:`G_{sto}` at each time step (and
grid box) with PAR and dividing by some max or mean value. To avoid
precalculation of mean PAR for each month, I have decided to use
500W/m\ :math:`^2`:

.. math:: G_{sto}(t) = G_{sto} \frac{PAR(t)}{500}\label{eq:gstot}

I will let others decide whether 500 is a suitable value.

Temperature scaling is done using minimum, optimal and maximum
temperatures, however, for simplicity, I have not done this for each
canopy type. I picked average values from
:raw-latex:`\citet{SimpsonEA2012}`. Not the best for tropical forests,
so feel free to revise it.

.. math::

   f_{T2} = \frac{T_{2m} - T_{min}}{T_{opt} - T_{min}}
             \left(\frac{T_{max}- T_{2m}}{t_{max} - T_{opt}}
             \right)^{\frac{T_{max}-T_{opt}}{T_{opt}-T_{min}}}

 The values used are :math:`T_{min}=2^{\circ}`\ C,
:math:`T_{opt}=22^{\circ}`\ C, :math:`T_{max}=37^{\circ}`\ C, and
:math:`T_{2m}` is 2-meter temperature given by the meteorological data
(of course also given in :math:`^{\circ}`\ C).

| *Non-stomatal conductance*
| In the new scheme, non-stomatal conductance is calculated specifically
  for O\ :math:`_3`, SO\ :math:`_2`, HNO\ :math:`_3` and NH\ :math:`_3`.
  For other species, an interpolation between O\ :math:`_3` and
  SO\ :math:`_2` values are carried out.

| – O\ :math:`_3` –
| The non-stomatal conductance for O\ :math:`_3` consists of two terms,
  one depending on vegetation type and one depending on the
  soil/surface. For land-type :math:`N` of :math:`N_{max}` types, we can
  write:

  .. math::

     G_{ns}^{O_3}(N) = \frac{SAI(N)}{r_{ext}}
                       + \frac{1}{R_{inc}(N) + R_{gs}^{O_3}(N)}
        \label{eq:gnso3}

   :math:`SAI(N)` is the surface area index for vegetation type
  :math:`N`, which is :math:`LAI` plus some value representing cuticles
  and other surfaces, having external leaf resistance of
  :math:`r_{ext} = 2000F_T`\ s/m, where :math:`F_T` is a temperature
  correction factor for temperatures below \ :math:`-1^{\circ}`\ C. Note
  that :math:`F_T` has an upper limit of 2, hence has a range of 1–2:

  .. math:: F_T = \exp(-0.2(1+T_s)) \qquad;\quad 1\le F_T \le 2\label{eq:FT}

   We assume that :math:`T_s` in this equation is the surface
  temperature, i.e. 2-meter temperature :math:`T_{2M}`. Temperature unit
  is :math:`^{\circ}C`.

In general we have that :math:`SAI=LAI` for all land types, but there
are three exceptions. The first two exceptions are forest and wetlands,
for which we set :math:`SAI=LAI+1`. The last exception is for cropland;
during the first part of the growth season :math:`SAI=LAI\cdot5/3.5`
while for the second part :math:`SAI=LAI+1.5`. In winter, cropland act
as a non-vegetated surface with :math:`SAI=0`.

| The two parts of growth seasons are simply defined:
| NH: day 90–140 and 141–270.
| SH: day 272–322 and 323–452 (i.e. day 87).

In this way, vegetation affects the conductance also by being there, not
only by uptake through the stomata.

:math:`R_{inc}` is the in-canopy resistance, defined for each vegetated
land-type :math:`N` as

.. math:: R_{inc}(N) = b\,SAI(N)\frac{h(N)}{u_*}

 where :math:`h(N)` is the canopy height and
:math:`b=14`\ s\ :math:`^{-1}` is an empirical constant. The in-canopy
resistance is not affected by temperature or snow in EMEP2012, and is of
course zero for non-vegetated surfaces.

The term :math:`R_{gs}^{O_3}(N)` is found from tabulated values of all
land-types (:math:`\hat{R}_{gs}^{O_3}`), corrected by :math:`F_T` and
snow cover :math:`f_{snow}(N)`:

.. math::

   \frac{1}{R_{ns}^{O_3}(N)} = \frac{1-f_{snow}(N)}{\hat{R}_{ns}^{O_3}(N)}
                     + \frac{f_{snow}(N)}{R_{snow}^{O_3}}
      \label{eq:rgso3corr}

 Note that EMEP2012 uses :math:`2f_{snow}` with a range of [0,1]. This
is explained weirdly in :raw-latex:`\citet{ZhangEA2003}`, but we stick
to using \ :math:`f_{snow}` with range [0,1].

Table [table:landuse] lists all land-use types and according values of
:math:`h(N)`, :math:`SAI(N)`, :math:`\hat{R}_{ns}^{O_3}(N)` and
:math:`\hat{R}_{ns}^{SO_2}(N)`. Forest vegetation height is assumed to
be 20m up to 60degrees latitude, and 6m poleward of 75degrees. From
latitudes 60–75 a linear interpolation is carried out.

+------+--------------+---------------------------+----------------------------+--------------------+------------------+
|      | Vegetation   | :math:`\hat{R}_c^{O_3}`   | :math:`\hat{R}_c^{SO_2}`   | SAI                | h [m]            |
+======+==============+===========================+============================+====================+==================+
| 1    | Forest       | 200                       | –                          | LAI+1              | 20\ :math:`^*`   |
+------+--------------+---------------------------+----------------------------+--------------------+------------------+
| 2    | Crops        | 200                       | –                          | :math:`\diamond`   | 0.4              |
+------+--------------+---------------------------+----------------------------+--------------------+------------------+
| 3    | Moorland     | 400                       | –                          | LAI                | 0.7              |
+------+--------------+---------------------------+----------------------------+--------------------+------------------+
| 4    | Grassland    | 1000                      | –                          | LAI                | 0.4              |
+------+--------------+---------------------------+----------------------------+--------------------+------------------+
| 5    | Wetland      | 400                       | 50                         | LAI+1              | 0.5              |
+------+--------------+---------------------------+----------------------------+--------------------+------------------+
| 6    | Tundra       | 400                       | 500                        | LAI                | 0                |
+------+--------------+---------------------------+----------------------------+--------------------+------------------+
| 7    | Desert       | 2000                      | 1000                       | 0                  | 0                |
+------+--------------+---------------------------+----------------------------+--------------------+------------------+
| 8    | Water        | 2000                      | 1                          | 0                  | 0                |
+------+--------------+---------------------------+----------------------------+--------------------+------------------+
| 9    | Urban        | 400                       | 400                        | 0                  | 0                |
+------+--------------+---------------------------+----------------------------+--------------------+------------------+
| 10   | Snow/ice     | 2000                      | 1000                       | 0                  | 0                |
+------+--------------+---------------------------+----------------------------+--------------------+------------------+
+------+--------------+---------------------------+----------------------------+--------------------+------------------+
+------+--------------+---------------------------+----------------------------+--------------------+------------------+
+------+--------------+---------------------------+----------------------------+--------------------+------------------+
+------+--------------+---------------------------+----------------------------+--------------------+------------------+

Table: Land-use properties in the new dry deposition treatment.

| *Snow cover*
| Snow cover does complicate the treatment slightly, because it depends
  on the land-type category: While e.g. 10cm of snow may be enough to
  cover grass, it is not enough to cover forest. So we calculate a snow
  cover fraction using the snow depth :math:`S_D` available through the
  meteorological data (note that this is given in meter equivalent
  water, so we multiply by 10 to get snow depth in meter):

  .. math::

     f_{snow}(N) = \frac{S_D}{S_{D,max}} = \frac{10\,S_D}{0.1\,h(N)}
       \label{eq:fsnow}

   This is carried out for each land-type, under the simple assumption
  that :math:`S_{D,max}=0.1\,h(N)`, where :math:`h(N)` is the canopy
  height for the given land-type :math:`N`. For O\ :math:`_3` we
  assuming :math:`R_{snow}^{O_3}=2000`\ s/m as in EMEP2012, constant for
  all temperatures.

From the equations above, we see that this means that both the
:math:`SAI`-term and :math:`R_{inc}` are unaffected by temperature and
snow.

| *Adding it up*
| For each land-use type :math:`N` we now calculate
  :math:`G_{ns}^{O_3}(N)` according to Eq. ([eq:gnso3]) using the
  land-type specific :math:`SAI`, :math:`R_{inc}` and
  :math:`\hat{R}_{gs}^{O_3}` corrected by :math:`F_T` and
  :math:`f_{snow}`. Then we make a gridbox average using the according
  land-type fractions \ :math:`(f_L)`:

  .. math::

     G_{ns,tot}^{O_3} = \sum_{N=0}^{N_{max}}\,G_{ns}^{O_3}(N) \cdot f_L(N)
        \label{eq:gnstoto3}

Finally, we have the gridbox average :math:`R_c^{O_3}`:

.. math:: R_c^{O_3} = \frac{1}{LAI \cdot G_{sto} + G_{ns,tot}^{O_3}}

| – NH\ :math:`_3` –
| :math:`R_c^{NH_3}` is treated separately using the molar ratio of
  SO\ :math:`_2`/NH:math:`_3`:

  .. math:: a_{sn} = \frac{[\textrm{SO}_2]}{[\textrm{NH}_3]}

   An upper limit of 10 is assumed, and this limit is also used for
  [NH:math:`_3`]=0. Using the 2m temperature (:math:`T_{2M}`) we have
  that for :math:`T_{2M}>0^{\circ}`\ C:

  .. math:: R_c^{NH_3} = \beta\,F_1(T_{2M},RH)\,F_2(a_{sn})

   where :math:`\beta=1/22` is a normalising factor, and

  .. math:: F_1 = 10\log_{10}(T_{2M}+2)\,\exp\left(\frac{100-RH}{7}\right)

   where :math:`RH` is relative humidity in percent, and

  .. math:: F_2 = 10^{-1.1099\,a_{sn}+1.6769}

   See :raw-latex:`\citet{SimpsonEA2012}` for references.

If you run the Oslo CTM3 without the nitrate and sulphur modules,
:math:`a_{sn}` should be read from a monthly model climatology.

| – SO\ :math:`_2` –
| The EMEP2012 treatment is based on an empirical formula for vegetated
  surfaces, using the 24hour mean molar ratio of
  SO\ :math:`_2`/NH:math:`_3` (:math:`a_{sn}^{24h}`):

  .. math:: R_{ns}^{SO_2} = 11.84 \, \exp(1.1\,a_{sn}^{24h}) \, f_{RH}^{-1.67}

   Here :math:`a_{sn}^{24h}` is limited to 3, and :math:`f_{RH}` is
  fractional humidity (0–1). Minimum and maximum limits of 10s/m and
  1000s/m, respectively, are imposed, and if :math:`f_{RH}=0` we assume
  maximum value.

| Additionally, there is a temperature dependent limit for
  SO\ :math:`_2`, using the 2m temperature (:math:`T_{2M}`):
| :math:`R_{ns}^{SO_2}=500`\ s/m for :math:`T_{2M}\le -5^{\circ}`\ C and
| :math:`R_{ns}^{SO_2}=100`\ s/m for
  :math:`-5^{\circ}`\ C\ :math:`< T_{2M}\le 0^{\circ}`\ C.

For non-vegetated land surfaces, tabulated values
:math:`\hat{R}_{ns}^{SO_2}(N)` are used (Table [table:landuse], modified
with :math:`F_T` similarly as explained for O\ :math:`_3`:

.. math::

   \frac{1}{R_{ns}^{SO_2}(N)} = \frac{1-f_{snow}(N)}{\hat{R}_{ns}^{SO_2}(N)}
                     + \frac{f_{snow}(N)}{R_{snow}^{SO_2}}
      \label{eq:rgsso2corr}

 Here we have

.. math::

   \begin{aligned}
     R_{snow}^{SO_2} &=& 70 \qquad\qquad\qquad T_{2M} \le +1^{\circ}\textrm{C}\nonumber\\
                 &=& 70\cdot(2-T_{2M})
                 \quad -1^{\circ}\textrm{C} \le T_{2M} < +1^{\circ}\textrm{C}\nonumber\\
                 &=& 700 \qquad\qquad\qquad T_{2M} \le -1^{\circ}\textrm{C}\end{aligned}

The total conductance is then

.. math::

   G_{ns,tot}^{SO_2} = \sum_{N=0}^{N_{max}}\,\frac{1}{R_{ns}^{SO_2}(N)} \, f_L(N)
      \label{eq:gnstotso2}

 and because :math:`R_c^{SO_2}` does not have a SAI term:

.. math:: R_c^{SO_2} = \frac{1}{G_{ns,tot}^{SO_2}}

If you run the Oslo CTM3 without the nitrate and sulphur modules,
:math:`a_{sn}^{24h}` should be read from a monthly model climatology.

| – HNO\ :math:`_3` –
| The surface resistance of HNO\ :math:`_3` is generally very small, but
  is reduced at cold temperatures. We follow EMEP2012, but with
  a minimum value of 1s/m instead of 10:

  .. math:: R_{ns}^{HNO_3} = \max(1, -2\,T_{2M})

| – Other gases –
| Other gases (except CO; see below) are interpolated using conductances
  from O\ :math:`_3` and SO\ :math:`_2`;

  .. math::

     G_{ns,tot}^{i} = 10^{-5}\,H_*^i\,G_{ns,tot}^{SO_2} + f_0^i\,G_{ns,tot}^{O_3}
        \label{eq:gnstoti}

   where :math:`H_*^i` and :math:`f_0^i` are taken from tables in
  :raw-latex:`\citet{Wesely1989}`, also listed in Table [table:wesely].
  From these we find the surface resistance for gas :math:`i`:

  .. math:: R_c^{i} = \frac{1}{G_{ns,tot}^{i}}

   There is no need of doing this calculation for each land-use type, as
  the interpolation itself introduces uncertainties, and because there
  are so little data available on non-stomatal resistances, this simpler
  scaling is acceptable (EMEP Report 1/2003).

In Oslo CTM3, this method is applied for PAN,
H\ :math:`_2`\ O\ :math:`_2`, NO\ :math:`_2`, NO, HCHO and
CH\ :math:`_3`\ CHO.

| – CO –
| CO uptake is very small and the Oslo CTM2 parameterisation, namely
  a fixed deposition rate of 0.03cm/s over forest and grass lands.

| *Note on stability scaling*
| The old deposition scheme adjusted dry deposition velocities according
  to stability, following:

  .. math::

     v_{d,adj} = \frac{v_d}{1 + \frac{v_d\,z}{K_z}}
        \label{eq:oldDEPstabscale}

   where :math:`z` is the mid-point of the surface layer and :math:`K_z`
  the eddy coefficient calculated by the boundary layer mixing routine.
  Physically, it is the mixing of the surface level that is reduced, and
  it is carried out for species using the old treatment at the end of
  ``setdrydep``, and the unit is still [m/s].

This scaling is not used for the EMEP parameterisation, which already
takes stability into account through :math:`u_*` and other
meteorological variables.

Technical description: aerosols
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

The sea salt (Section [sxn:salt]) and mineral dust (Section [sxn:dust])
modules have their own deposition routines. For BC/OC, sulphate and SOA,
however, we use the EMEP treatment :raw-latex:`\citep{SimpsonEA2012}`.
They give the dry deposition velocity :math:`v_d` as

.. math::

   \frac{V_d}{u_*} = \left\{
     \begin{array}{ll}
       a_1 & : L \ge 0\\
       a_1 F_N \left[1+\left(\frac{-a_2}{L}\right)^{2/3}\right]
           & : L < 0\label{eq:a1fn}
     \end{array}
   \right.

 where :math:`F_N = 3` for fine-nitrate and ammonium, and :math:`F_N=1`
for all other aerosols. :raw-latex:`\citet{SimpsonEA2012}` limit this to
:math:`1/L<-0.04`\ m\ :math:`^{-1}`, which means that for
:math:`-25<L<0` we use :math:`L=-25` in Eq. ([eq:a1fn]).

The parameters :math:`a_1` and :math:`a_2` are

.. math::

   \begin{aligned}
     a_1 &=& \left\{
     \begin{array}{ll}
       0.002 & \textrm{non-forest}\\
       \max(0.002,0.008\frac{SAI}{10}) & \textrm{forest}
     \end{array}
     \right.\\
     a_2 &=& 300\,\textrm{m}\end{aligned}

In principle, :math:`a_1` could differ from dry to wet surfaces. This is
not yet taken into account for sulphate, MSA, nitrate and SOA. For
BC/OC, however, we have calculated our own parameters based on mean
values of Oslo CTM2 for water, land and ice surfaces, as well as the
annual mean friction velocity (:math:`\overline{u_*}`):

.. math::

   \begin{aligned}
     a_{1,L} &=& \frac{V_{land}}{\overline{u_*}}\\
     a_{1,W} &=& \frac{V_{water}}{\overline{u_*}}\\
     a_{1,I} &=& \frac{V_{ice}}{\overline{u_*}}\end{aligned}

The :math:`V`-values are defined in the BC/OC module
(Section [sxn:bcoc]) as 0.025cm/s, with the exception of hydrophilic
aerosols over wet surfaces which use 0.2cm/s.

Soil uptake of CH\ :math:`_4`
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

If CH\ :math:`_4` emissions are included (Section [sxn:emissions\_ch4]),
a soil sink also has to be included: CH\ :math:`_4` is taken up in soils
by microbacteria, and this process can be modelled as dry deposition. In
Oslo CTM3, this requires a deposition velocity, which currently is
calculated from two datasets; soil uptake in kg/s from a dataset
provided by Bousquet (Section [sxn:emissions\_ch4]), and CH\ :math:`_4`
mass in the surface gridboxes of T42L60 resolution, calculated during
the HYMN project.

The routines taking care of this are located in the file
*ch4routines.f90*.

Inside or outside chemistry?
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

There are two ways to treat deposition in the Oslo CTM3; the Oslo way
and the UCI core method.

In the UCI core method, tracers are removed by deposition as a separate
process in the operator split loop. This allows for better control over
diagnostics.

The Oslo method is to treat dry deposition as a loss term in chemistry,
along with treating emissions as source terms. You define the method you
need in the user specified section of *Makefile*. This is described in
Section [sxn:emissions]; remember that this also moves emissions into
chemistry. Note that e.g. mineral dust and sea salt modules have their
own deposition calculations.

The two methods should be tested thoroughly against each other.

UCI dry deposition
~~~~~~~~~~~~~~~~~~

It is in principle possible to use the UCI CTM deposition scheme, which
is very simple, listing deposition velocities for 3 different soil types
(listed in the old scavenge list *scavng55.dat*).

Uptake on aerosols
------------------

A third option for scavenging of gases is uptake on aerosols. This may
involve reactions which release products back to the atmosphere, and is
thus sometimes referred to as heterogeneous reactions.

Currently, there is only tropospheric uptake of
N\ :math:`_2`\ O\ :math:`_5`, producing HNO\ :math:`_3`, and uptake of
HO\ :math:`_2` and RO\ :math:`_2`. In the stratosphere there are a few
other aerosol uptake processes.

The tropospheric uptake is in the process of being revised.

Emissions
=========

Emissions can largely be divided into surface, lightning and aircraft
emissions. Surface emissions can further be divided into anthropogenic,
biomass burning (often distributed vertically), natural biogenic,
natural oceanic and natural soil emissions.

How emissions are treated in Oslo CTM3 is explained in
Section [sxn:emissions\_treatment], before a short note on NOx. How to
set up different emission sets and include new sets are described in
Section [sxn:emissions\_howto].

Different types of emissions are explained in the following sections.

Note that emissions are always stored with units of kg/s. When used as
chemical production terms, the units are converted to
molecules/cm\ :math:`^3`/s.

Emissions treatment
-------------------

Emissions can be treated in two ways in the Oslo CTM3.

Inside chemistry as a chemical source (Oslo method,
Section [sxn:emissions\_oslo]).

As a separate process (UCI method, Section [sxn:emissions\_uci]).

The choice of approach is set in *Makefile*, by the variable
``EMISDEP_TREATMENT``. The default choice is the Oslo treatment until
extensive testing has been carried out.

Emissions in chemistry
~~~~~~~~~~~~~~~~~~~~~~

In the traditional Oslo chemistry method, the emissions are treated as
chemical source terms (and deposition as a sink). This method aims to
combine several of the operator split processes in one.

One advantage of this treatment is a quasi steady state between sources
and sinks, so that parts of what is emitted is also lost (when a sink is
present). This method will give smoother time series, since the sources
and sinks compete in the calculation.

At the moment this is the default treatment in Oslo CTM3.

Emissions as a process
~~~~~~~~~~~~~~~~~~~~~~

The other approach is the UCI method (``EMISDEP_TREATMENT :=U`` in
*Makefile*), treating emissions as a separate process (routine
``SOURCE``). Emissions are put directly into the tracer array (as mass,
kg), before they are mixed in boundary layer mixing and then the
chemistry is calculated. For this method, the emissions may impose
a large change in the concentrations, causing a larger adjustment in the
chemistry. It is important to do boundary layer mixing after emissions
to reduce the artificial impact in the surface layer.

With this method it is possible to better diagnose every step of the
operator split processes. This is useful for evaluating the model and
its resolution (e.g. time step vs convergence).

After extensive testing is carried out, this may eventually become the
default emission treatment in Oslo CTM3.

| *Important*
| Note that e.g. sea salt has separate production routines, using QSSA.
  This will have to be revised if you want sea salt production as
  a separate process (see also Section [sxn:salt\_technical]). Probably
  less straight-forward is to change the mineral dust production from
  DEAD treatment to a separate source.

Important about NOx
-------------------

NOx is treated as a family in the Oslo chemistry. Traditionally this NOx
family was also transported, but since all its components are
transported there is no longer need for the family to be transported,
and hence not emitted.

How to set up emissions
-----------------------

The emissions are listed in the file *Ltracer\_emis\_xxxx.inp*, where
*xxxx* is e.g. \ *eclipse\_V5*. You will find some information at the
top of the file, followed by a section with flags and data for special
emissions (e.g. aircraft emissions). After this the 2D and 3D monthly
emissions are listed separately.

Next, there’s a section containing static fields used in the model to
generate other relevant emissions, e.g. emissions that are be updated
more frequently. The section is named STV for short term variation.
Examples are DMS emissions, volcanic emissions and dust emissions.

Finally, there is a section for biomass burning, denoted ’BBB’.

The emission datasets are described in next Sections, however, here is
a short how-to.

It is important to make sure you include all categories you need; and
that sources are not covered by different datasets at the same time:

aircraft emissions

anthropogenic emissions

biomass burning emissions

lightning emissions

natural biogenic emissions

natural oceanic emissions

natural soil emissions

volcanic emissions

Some datasets are given for one year only, e.g. 2000, while others are
given for different years. If you want to interpolate between the
datasets, the easiest way is to include both datasets in the
*Ltracer\_emis\_xxxx.inp* and apply weightings for each dataset.
E.g. for 2001 you can include 0.8 of the 2000 dataset and 0.2 of the
2005 dataset.

You should also be aware that some datasets, such as ECLIPSE, include
agricultural waste burning as anthropogenic emissions, while it is also
present in the GFED biomass burning datasets. You need to apply only one
of these. This will be explained in the subsections of
Section [sxn:emissions\_datasets].

For each section in the emission list, the read-in structure is shown
first, with a more detailed explanation at the bottom of the file. An
example from the 2D section is shown in Table [table:emis].

--------------

::

    -2D-- 2D emissions -------------------------------------------------------
    filename / description  (See detailed description below)
      ID     SCALE       RES MONTH YEAR  CAT TYPE UNIT DIURN VERT DATASET_NAME SCENYEAR
        SPECIES SCALING ('xxx' to close dataset)
    '/full_path/some_emission_file_of_anthropogenic_CO_2000_0.5x0.5.nc'
      601    1.0000d+00  HLF   99  9999  AGR   2    0    1    0   'emiss_agr'  0000
        CO  1.d0
        xxx 0 To close this data set
    '/full_path/some_emission_file_of_anthropogenic_NO_2000_0.5x0.5.nc'
      601    1.0000d+00  HLF   99  9999  AGR   2    0    1    0   'emiss_agr'  0000
        NO  0.96d0
        NO2 0.0613333333d0      # NO on file: 0.04*46/30
        xxx 0 To close this data set
    '/work/projects/cicero/ctm_input/EMIS/RETROEMIS_NEW/soils.nox.nc'   0000
      601    1.0000d+00  1x1   99  9999  BIO   1    1    0    0   'bio'
        NO  0.96d0      # molecules on file, no need to scale NO2/NO
        NO2 0.04d0
        xxx 0 To close this data set
      

--------------

The different variables to set are described at the bottom of the file:

``ID``: Tag referring to the format to read. When including a new set
with a new structure, a new read-in code has to be included.

``SCALE``: Scaling factor which applies for the whole dataset.

``RES``: String describing the resolution; ``1x1`` for 1 degree, ``HLF``
for 0.5 degree, and ``ZP1`` for 0.1 degree.

``MONTH``: The month the dataset applies for. ``99`` is all months.

``YEAR``: The year the dataset applies for. ``9999`` is all years.

``CAT``: The category for the dataset. This is used for diagnostics
only, summing up emissions for each category.

``TYPE``: Defines whether the field should be scaled with area or not.
``0`` when field is not per area, ``1`` when field is 1/cm\ :math:`^2`
and ``2`` when field is 1/m\ :math:`^2`.

``UNIT``: To define the unit of the dataset, so correct scaling is
applied in the emission routines. If per area, it is combined with
non-zero ``TYPE``. ``1`` for kg/s, ``2`` for moelc/s, ``3`` for kg/y,
and ``4`` for kg/month.

``DIURN``: Flags a dataset for diurnal variation. See
Section [sxn:emissions\_diurnal] for more. ``0`` for no diurnal
variation, ``1`` for RETRO local hour variations, ``2`` for +50% from
8am to 7pm and -50% from 8pm to 7am. ``DIURN=3``: Scaling with :math:`T`
and daylight, ``DIURN=4``: Scaling with daylight, ``DIURN=5``: Heating
degree day (HDD) scaling.

``VERT``: Flags whether 2D dataset should be distributed vertically on
the lowermost model levels (altitudes about
0-16m/16-41m/41-77m/77-128m). Several options are available, although
none are documented yet. They should be documented as soon as possible.
``VERT=1``: 0.25/0.125/0.125/0.5 (typical power/industrial combustion),
``VERT=2``: 0.6/0.2/0.2/0.0 (typical residential heating), ``VERT=3``:
0.3/0.4/0.3/0.0 (typical ship emissions).

``DATASET_NAME``: The name of the dataset on file, needed for
e.g. netCDF files.

``SCENYEAR``: The year to extract data for – used for read-ins where the
file contains several years of data. If the file only contain data for
a specific year, this is not used.

``SPECIES``: the component for which the dataset is to be applied. If
species differ from what the dataset applies for, a scaling factor may
be needed to account for differences in molecular masses (``SCALING``).

| ``SCALING``: Scaling factor for ``SPECIES``, for the given dataset.
  Use this for taking different molecular weights into account, e.g. as
  shown for ``NO`` in Table [table:emis].
| **Important**: The scaling of components must take into account
  differences in molecular weights. However, if the emission dataset is
  given in molecules, such a scaling should *not* be included. See the
  two NO/NO2 examples in Table [table:emis].

Emission datasets
-----------------

As already mentioned, there are several emission inventories available,
and most are listed in the directory *tables*\ */EMISSION\_LISTS*.
Usually, the natural and anthropogenic emissions are separated into
different datasets. E.g. natural biogenic, oceanic and soil emissions
may be taken from POET :raw-latex:`\citep{GranierEA2005,POETreport2}`.
Biogenic emissions may rather be taken from more recent MEGAN datasets,
but note that MEAGAN does not comprise oceanic emissions, nor soil
emissions of NO\ :math:`_x`. Recently, a newer MEGAN dataset has become
available, the so-called MEGAN-MACC
:raw-latex:`\citep{SindelarovaEA2014}`. Anthropogenic emission datasets
are e.g. CEDS, RETRO, :raw-latex:`\citet{LamarqueEA2010}`, EDGARv4.2 or
ECLIPSE.

You specify which emission datasets to include in the emission list
*Ltracer\_emis\_xxxx.inp*. See Section [sxn:emissions\_howto] for
a description on this.

All these emissions are read by the routine ``emis_input`` in
*emissions\_oslo.f90*. Any dataset can in principle be used, although
you may have to include new routines.

There are currently no studies on which dataset is the best, and how
they affect the model results. Generally, the newer datasets should be
better because they are more up-to-date. However, some datasets lack
seasonal variation, which could be important for your study.

Note that some specific components need more specific/hard-coded
parameterisations, like forest fires (Section [sxn:emissions\_forest])
and aircraft emissions (Section [sxn:emissions\_ac]). Such emissions are
updated by ``update_emis`` in *emissions\_oslo.f90*.

Monthly & annual emissions
~~~~~~~~~~~~~~~~~~~~~~~~~~

The monthly and annual emissions can be 2D or 3D. 2D fields are stored
in the array ``E2DS(IPAR,JPAR,NE2DS,ETPAR)``, where ``NE2DS`` is
usually 1, but can be 6 if you want moments included (described below).
``ETPAR`` is the max number of tables, and there is a counter for the
actual number of tables used, ``NE2TBL``.

If you have emissions as a separate process (UCI method), and if the
emissions are on a higher resolution than the model resolution and you
want higher accuracy for where emissions are put out, it is possible to
include moments in the 2D emissions. To do this you have to hard-code
``NE2DS=6`` in *cmn\_size.F90*). This is only for experienced users. It
is probably wise to leave out moments for horizontal resolutions higher
than T159.

When treating emissions inside chemistry, the moments *cannot* be
included (``NE2DS=1``).

There can be max ``E3PAR`` 3D fields, and they are given in
``E3DSNEW(LPAR,IPAR,JPAR,E3PAR)``, which is changed from the UCI core
due to striding in the original ``E3DS(IPAR,JPAR,LPAR,E3PAR)``. The
counter for used fields is ``NE3TBL``.

Forest fires are treated separately (Section [sxn:emissions\_forest])
and there are also some other short term variations based on monthly or
annual data (Section [sxn:emissions\_stv]).

The Oslo CTM3 should in general be modified to read monthly emissions
each month, to save memory requirements.

Diurnal scaling
~~~~~~~~~~~~~~~

Diurnal variations can be imposed on the 2D monthly/annual datasets.
This is done by setting the ``DIURN`` flag in *Ltracer\_emis\_xxxx.inp*.
The ``DIURN`` flag is described below, and *Ltracer\_emis\_xxxx.inp*\ in
Section [sxn:emissions\_howto]).

The ``DIURN`` can either scale to local hour or by a 2D field. Local
hour scaling emits more during certain hours than at other, presently
with an hourly temporal resolution. Scaling the dataset to a 2D field
usually means that it is scaled by meteorological properties such as
temperature. These scalings need reference fields, e.g. on monthly
basis.

| **Available diurnal variations**
| When the flag ``DIURN`` is set for a dataset listed in
  *Ltracer\_emis\_xxxx.inp*, a diurnal variation is applied to the
  dataset. Currently the possibilities are

``DIURN=0``: No diurnal variation.

``DIURN=1``: RETRO variations (TNO [2]_).

``DIURN=2``: +50% from 8am to 7pm and -50% from 8pm to 7am.

``DIURN=3``: Scaling with :math:`T` and daylight.

``DIURN=4``: Scaling with daylight.

``DIURN=5``: Heating degree day (HDD) scaling.

If you need other scalings, inclusion of new ones should be rather
straightforward once you know the system. ``DIURN=1`` and ``DIURN=2``
can in principle be set up to work on 3D fields, but is currently not
used.

| **Local hour scalings**
| These are found in the variable ``E2LocHourSCALE`` and of size
  ``24,NECAT,NE2LocHourVARS`` and are defined in *cmn\_oslo.f90* and set
  in *emisutils\_oslo.f90*, routine ``set_diurnal_scalings``

| **RETRO variations**
| ``DIURN=1`` sets the RETRO variations (TNO [3]_). There is one scaling
  per category.

*Note that these work for pre-defined categories.*

| **+50% :math:`-`\ 50%**
| ``DIURN=2`` sets +50% from 8am to 7pm and -50% from 8pm to 7am.

| **Variations with :math:`T` and daylight**
| Some species, typically naturally emitted isoprene and monoterpenes,
  are emitted only during daytime, whereas the emission files are
  usually monthly or annual means. In Oslo CTM3 we have the possibility
  to scale surface emissions so that they are emitted only during
  daytime. In addition, it is possible to scale according to
  temperature, i.e. daytime temperature. In other words, these are more
  complex diurnal scalings than the local hour predefined scaling.

| *Monthly-based daylight scaling*
| The daylight scaling is a 2D field of the fraction of daylight hours
  during the month, which we denote :math:`f_{day}`. It is possible to
  modify this to a daily basis, but the monthly basis was chosen to
  match monthly emissions.

For each hour we check if there is daylight, and scale up the
emissions \ :math:`E`:

.. math::

   E_{day} = E/f_{day}
     \label{eq:daylight}

 During night there is consequently no emissions, except that when there
is polar night, i.e. no daylight during a day, we set :math:`f_{day}=1`.
This is of minor importance for species such as isoprene and
monoterpenes, which should have very small emissions during the polar
night. You set this option by ``DIURN=4`` in the emission list.

| *Monthly-based day-temperature scaling*
| Another surface scaling field is based on temperature and daylight for
  the years 1997–2010 using our T42L60 meteorological data (ECMWF IFS
  cycle 36r1).

This method is based upon :raw-latex:`\citet{GuentherEA1995}`, who
described the effect of temperature on emissions of VOCs. They found
that the emissions \ :math:`E` could be described by variation around
a standard emission \ :math:`E_s` at a standard temperature :math:`T_s`:

.. math::

   \begin{aligned}
     E &=& E_s f_{T}\label{eq:guenther1}\\
     f_{T} &=& \frac{\exp\left(\frac{C_1 (T - T_s)}{R T_s T}\right)}
                  {1 + \exp\left(\frac{C_2 (T - T_m)}{R T_s T}\right)}
     \label{eq:guenther2}\end{aligned}

 Three empirical constants are used here: :math:`T_m=314`\ K,
:math:`C_1=95000` and :math:`C_2=230000`.

Using :math:`f_T` in Eq. ([eq:guenther2]), a standard temperature
:math:`T_s=303`\ K, and 3-hourly surface temperatures from our 1997–2010
meteorology, we have computed climatological monthly means of
:math:`f_T`, namely :math:`f_{Tclim}`. When applied to a monthly
emission inventory, we calculate :math:`E_s` from Eq. ([eq:guenther1]),
using :math:`f_{Tclim}`. Since :math:`E_s` is the climatological
emissions at the standard temperature, we can each hour in the model
scale with :math:`f_T` using :math:`T` at that time step.

Note that this method is also taking only daylight into account, as
described above, except in polar night, where temperature scaling is
carried out using temperatures for the whole day/night. You set this
option by ``DIURN=3`` in the emission list.

| *Heating degree day (HDD) scaling*
| HDD is defined as a difference temperature in Kelvin:

  .. math:: HDD = \max(0,288.15 - T_{sfc})

   Weighting the surface temperature :math:`T_{sfc}` by the time steps
  of the meteorological data (3hr), :math:`HDD` has been summed up on
  monthly and annual basis, giving the fraction of monthly to annual
  :math:`HDD`. This is done for specific years. To be applied monthly on
  annual data of units kg/s, this fraction has to be converted to
  a scaling by multiplying with 12; the sum of the 12 scalings for
  a grid box should not be 1, but 12, because we are scaling up or down
  annual mean kg/s and applying it for different months.

This is so far only set up on a monthly basis, but could in principle be
updated every meteorological time step.
:raw-latex:`\citet{StohlEA2013}`, however, found that using a monthly
variation was a good approximation.

Vertical distribution of 2D emissions
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

For 2D emission datasets, it is possible to distribute the emissions
into four of the lowermost model layers. These layers roughly cover
surface–16m, 16–41m, 41–77m and 77–128m (the actual thickness depends on
surface pressure, temperature and specific humidity, see
Appendix [app:layerheights]).

The distributions are not documented yet, but were used in Oslo CTM2:

``VERB=0``: All in surface layer.

``VERB=1``: 0.25/0.125/0.125/0.5, typical for power and industrial
combustion.

``VERB=2``: 0.6/0.2/0.2/0.0, typical for residential heating.

``VERB=3``: 0.3/0.4/0.3/0.0, typical for ships.

Defined in *cmn\_oslo.f90*, there are ``NE2vertVARS`` distributions, and
the distributions are stored in the variable ``E2vertSCALE``.

Short term variations
~~~~~~~~~~~~~~~~~~~~~

Some emissions are not given on monthly or annual basis, but depend
strongly on e.g. wind or temperature. Examples are DMS from ocean and
sea spray. Organic matter from sea spray is also difficult to retrieve
from a separate monthly dataset.

The STV section of the emission list allows inclusion of some of these
datasets.

+---------------+----------+------------------------------------------------------------------------------+
| Variable      | Option   | Description                                                                  |
+===============+==========+==============================================================================+
| ``FF_TYPE``   | 4        | GFEDv4 monthly (code 567)                                                    |
+---------------+----------+------------------------------------------------------------------------------+
|               | 5        | CEDS monthly (code 568)                                                      |
+---------------+----------+------------------------------------------------------------------------------+
|               | 51       | CEDS monthly (code 574), distributed according to                            |
+---------------+----------+------------------------------------------------------------------------------+
|               |          | model level thickness in the boundary layer.                                 |
+---------------+----------+------------------------------------------------------------------------------+
|               | 6        | GFEDv4 monthly (code 569), distributed according to air mass in the          |
+---------------+----------+------------------------------------------------------------------------------+
|               |          | boundary layer.                                                              |
+---------------+----------+------------------------------------------------------------------------------+
|               | 7        | GFEDv4 daily (code 570), distributed according to air mass in the            |
+---------------+----------+------------------------------------------------------------------------------+
|               |          | boundary layer.                                                              |
+---------------+----------+------------------------------------------------------------------------------+
|               | 8        | GFEDv4 monthly (code 571), distributed according to                          |
+---------------+----------+------------------------------------------------------------------------------+
|               |          | model level thickness in the boundary layer.                                 |
+---------------+----------+------------------------------------------------------------------------------+
|               | 9        | GFEDv4 daily (code 572), distributed according to                            |
+---------------+----------+------------------------------------------------------------------------------+
|               |          | model level thickness in the boundary layer. **DEFAULT.**                    |
+---------------+----------+------------------------------------------------------------------------------+
| ``FF_YEAR``   | 9999     | Use current meteorological year (``MYEAR``), or specify which year to use.   |
+---------------+----------+------------------------------------------------------------------------------+
| ``FF_PATH``   |          | Path to where emissions are.                                                 |
+---------------+----------+------------------------------------------------------------------------------+

| **DMS from ocean**
| DMS is emitted continuously from ocean water, and those emissions are
  parameterized using climatological concentrations of DMS for each
  month, combined with winds from the meteorological data. See
  Section [sxn:sulphur\_emis] for more.

The concentration is read into ``DMSseaconc``, and has units of nM
(nanoMolar), i.e. nmole/L. The units are then converted to kg/m and
multiplied with a velocity based on the wind speed. This is carried out
in the ``SOURCE`` routine or in ``emis4chem_oslo`` when using emissions
as production terms in chemistry.

| **Organic matter from ocean**
| The emission of organic matter from ocean follows
  :raw-latex:`\citet{GanttEA2015}`, based on chlorophyll A and sea
  spray.

Sea spray is taken from the sea salt module if it is included. Several
schemes are available, as described in Section [sxn:salt\_production],
using the flag ``SeaSaltScheme``. If the sea salt module is not
included, this sea spray is calculated by the organic matter emission
routine, using its own flag ``SeaSaltScheme``.

Oslo CTM3 use Chlorophyll A from MODIS, where monthly data for the years
2003–2014 are available. A climatology for the same years have been
generated, and the user specifies whether to use the climatology or not
in the emission list.

The MODIS monthly mean data are fetched from
https://oceancolor.gsfc.nasa.gov/cgi/l3 where you download the files
*V\*.L3b\_MO\_CHL.nc* by clicking on the lower right corner of each
thumbnail. The files are hdf5, even if the extension is .nc. Before
2015, the files were hdf4, with different names.

I have stored the original files at
*/div/amoc/d4-4/oslo-ctm/*\ */modis\_chlorophyllA/*.

Files (both hdf5 and hdf4) are converted to netcdf using IDL program
*read\_chla.pro* (located in the utilities directory) which applies the
program *readl3bin\_nc.pro*.

The climatology is generated from netCDF files using the IDL program
*chla\_avg.pro*.

| **DUST emissions**
| Emissions of mineral dust should be defined in the STV section, as
  explained in Section [sxn:dustinput].

Forest fires emissions
----------------------

Several species are affected by forest fires, and emissions may be
updated more frequently than each month. Due to this, the forest fires
need to be coded separately.

It is recommended to use daily GFEDv4 emissions, distributed in the
boundary layer, using the format code 572 in the emission list,
i.e. daily emissions. If daily emissions are not available (years prior
to 2003), you should use monthly emissions (code 571). Some other
options for GFEDv4 are described below.

The code defines ``FF_TYPE``, while the year defines ``FF_YEAR`` and the
path defines ``FF_PATH``. See Table [table:ffcode]. A diurnal cycle is
further added, according to :raw-latex:`\citet{RobertsEA2009}`, putting
increasing daytime emissions by 90% (7–19 local hours) and reducing
nighttime emissions to 10%.

An alternative dataset to GFEDv4 is monthly CEDS biomass burning, which
you set up with the old vertical profile (code 568) and the altitude
weighted boundary layer profile (code 574).

You find the inputs to the emission list in
*Ltracer\_emis\_biomassburning.dat* in *EMISSION\_LISTS*. The list
includes scaling factors for different species.

Historically, the biomass burning emissions have been distributed in the
vertical by a distribution from RETRO. This should be left behind, as it
is not very physical. We rather distribute the emissions within the
boundary layer. The old profile probably put too much above the boundary
layer, so with the new method, over-shooting is reduced.

If over-shooting is needed, you should make a distribution on the
emission strength in the emission dataset, and for the strongest sources
(e.g. defined by 90-percentile) select the grid boxes where
over-shooting may occur.

Two test options are included, distributing emissions according to air
mass in the boundary layer (air mass weighted), namely code 570
(``FF_TYPE=6``, monthly emissions) and 571 (``FF_TYPE=7``, daily
emissions). These put emissions a bit closer to the surface than the
altitude-weighted profile.

The read-in is called from ``update_emis`` (in *emissions\_oslo.f90*,
and there you can see which routines are called for the different
choices.

Note that the daily fractions require the monthly field to be read every
day (for the future, consider storing it). The daily fractions have to
be applied prior to regridding.

The species emitted are listed in the BBB section of the emission list,
and are typically CO, NOx, hydrocarbons, NH\ :math:`_3`, SO\ :math:`_2`
and black and organic carbon. The emissions are distributed vertically
based on RETRO vertical distribution.

The number of components treated is given as ``NEFIR``, with an upper
limit of ``EPAR_FIR``. Their transport numbers are given in
``ECOMP_FIR(EPAR_FIR)`` and emissions are stored in the array
``EMIS_FIR`` with dimension
``(EPAR_FIR_LM,EPAR_FIR,IDBLK,JDBLK,MPBLK)``. The array structure makes
it easy to use with very little striding.

Note that the read-in from file is placed outside the parallel region of
the model, because the interpolation is parallelized.

Other variables used for determining species and file names, which are
read from emission list, are:

``FF_CNAMES``: Emitted component.

``FF_BNAMES``: Component name used in file name.

``FF_SCALE``: Scaling factor.

``FF_PARTITIONS``: Partition factors (GFED only).

| **Forest fire partitions**
| It is possible to turn off specific partitions of the GFEDv4 data.
  There are six partitions:

agriculture

deforestation

forest

peat

savanna

woodland

The turning off is done by hard-coding the flags ``partitions`` in the
routine ``gfed4_rd_*``, the read-in located in *emisutils\_oslo.f90*.

One example where you have to do this, is for ECLIPSE emissions of black
carbon, where you have to turn off agricultural waste burning (AWB),
because it is included in the ECLIPSE dataset. See Section [sxn:bcoc]
for more.

Biogenic emissions: MEGAN
-------------------------

The emission model MEGAN, version 2.10
:raw-latex:`\citep{GuentherEA2012}`, is included in the Oslo CTM3. You
turn it on by including it in the emission list (STV section), as in
Table [table:emismegan].

--------------

::

    -STV- Used for short time variations; only one species per field ------------
    'Indata_CTM3/MEGAN/'
      557    1.00000d+00  HLF   99  9999  BIO   0   -1    0   '---'  9999
      ---   #This section defines the use of MEGAN and sets path to MEGAN datasets
      

--------------

**Turn off other biogenic emissions!** Make sure that other biogenic
emission datasets are not included in the 2D section of the emission
list. Also NOx from soil emissions should be turned off.

The MEGAN model uses leaf area index (``LAI``) and roughness length
(``ZOI``). See Section [sxn:leafareaindex] and Section [sxn:roughness],
respectively.

Here will follow a description of MEGAN and the changes done for
Oslo CTM3.

Modifications to MEGAN code
~~~~~~~~~~~~~~~~~~~~~~~~~~~

The MEGANv2.10 code has been adopted to Oslo CTM3, in other words
stripped and cleaned up to fit the Oslo CTM3 structure. Input to MEGAN
is through *megan\_tables.dat*, found in the *tables/* directory.

Here is a list of the tables (in appearing order):

#. Megan original species (20 of them). Note that soil NOx is included,
   even if it is not described by :raw-latex:`\citep{GuentherEA2012}`.

#. | Canopy types, or plant function types (PFTs). There are 16 of
     these, listed in Table [table:megan\_pft], and stored in the
     variable ``landSurfTypeFrac``. See also
     Section [sxn:landsurfacetypes].

#. | Canopy Characteristics, listed in Table [table:megan\_pft].

#. | Emission factors for the 20 MEGAN species for each canopy type.
     This is Table 2 in :raw-latex:`\citet{GuentherEA2012}`.

#. Emission factors for each specified species (151 of those). In the
   MEGAN original, these should sum up to 1 for each of the 20 species.
   Note that while MEGAN originally does not take molecular masses into
   account, we have to do this at least for NO and NO\ :math:`_2`. This
   is explained in the text.

+------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| #    | Canopy type                                                                                                                                                                                                            |
+======+========================================================================================================================================================================================================================+
| 1    | Needleleaf evergreen temperate tree                                                                                                                                                                                    |
+------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| 2    | Needleleaf evergreen boreal tree                                                                                                                                                                                       |
+------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| 3    | Needleleaf deciduous boreal tree                                                                                                                                                                                       |
+------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| 4    | Broadleaf evergreen tropical tree                                                                                                                                                                                      |
+------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| 5    | Broadleaf evergreen temperate tree                                                                                                                                                                                     |
+------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| 6    | Broadleaf deciduous tropical tree                                                                                                                                                                                      |
+------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| 7    | Broadleaf deciduous temperate tree                                                                                                                                                                                     |
+------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| 8    | Broadleaf deciduous boreal tree                                                                                                                                                                                        |
+------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| 9    | Broadleaf evergreen temperate shrub                                                                                                                                                                                    |
+------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| 10   | Broadleaf deciduous temperate shrub                                                                                                                                                                                    |
+------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| 11   | Broadleaf deciduous boreal shrub                                                                                                                                                                                       |
+------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| 12   | Cool/Arctic C3 grass                                                                                                                                                                                                   |
+------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| 13   | C3 grass (cool)                                                                                                                                                                                                        |
+------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| 14   | C4 grass (warm)                                                                                                                                                                                                        |
+------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| 15   | Crop 1 (Corn)                                                                                                                                                                                                          |
+------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| 16   | Crop 2 (Other crops)                                                                                                                                                                                                   |
+------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
+------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| #    | Canopy characteristics                                                                                                                                                                                                 |
+------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| 1    | canopy depth (height above ground is canopy height minus canopy depth)                                                                                                                                                 |
+------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| 2    | leaf width                                                                                                                                                                                                             |
+------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| 3    | leaf length                                                                                                                                                                                                            |
+------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| 4    | canopy height                                                                                                                                                                                                          |
+------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| 5    | scattering coefficient for PPFD                                                                                                                                                                                        |
+------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| 6    | scattering coefficient for near IR                                                                                                                                                                                     |
+------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| 7    | reflection coefficient for diffuse PPFD                                                                                                                                                                                |
+------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| 8    | reflection coefficient for diffuse near IR                                                                                                                                                                             |
+------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| 9    | clustering coefficient (accounts for leaf clumping influence on mean projected leaf area in the direction of the suns beam) use 0.85 for default, corn=0.4-0.9; Pine=0.6-1.0; oak=0.53-0.67; tropical rainforest=1.1   |
+------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| 10   | leaf IR emissivity                                                                                                                                                                                                     |
+------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| 11   | leaf stomata and cuticle factor: 1=hypostomatous, 2=amphistomatous, 1.25=hypostomatous but with some transpiration through cuticle                                                                                     |
+------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| 12   | daytime temperature lapse rate (K m-1)                                                                                                                                                                                 |
+------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| 13   | nighttime temperature lapse rate (K m-1)                                                                                                                                                                               |
+------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| 14   | warm (>283K) canopy total humidity change (Pa)                                                                                                                                                                         |
+------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| 15   | cool (>= 283K) canopy total humidity change (Pa)                                                                                                                                                                       |
+------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| 16   | normalized canopy depth where wind is negligible                                                                                                                                                                       |
+------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+
| 17   | canopy transparency (included in MEGAN3)                                                                                                                                                                               |
+------+------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------+

Table: Canopy types (PFTs) and canopy characteristics used in MEGAN.
[table:megan\_pft]

**Molecular weight**: MEGAN does take into account possible differences
in molecular weights of the 20 species when speciation to 150 species is
done. This means that it calculates a total mass for each of the 20
species, and then distributes the masses on each component.

This may be problematic when species of very different molecular weights
are lumped together. E.g. if we want to emit NO as 4% NO\ :math:`_2` or
some other fraction as NH\ :math:`_3`, it would distribute the total
mass of NO, not considering that NH\ :math:`_3` is lighter or
NO\ :math:`_2` heavier.

Taking the molecular weight of the 20-species into account would solve
this, but is clearly not what was intended in MEGAN.

**NO, NO\ :math:`_2` and NH\ :math:`_3`**: I have therefore kept the
MEGAN method, but for NO the speciation factors have to take into
account the difference in molecular weights. These are thus scaled so
that 4% of NO is emitted as NO\ :math:`_2`. The total atmospheric source
of NOx is 6Tg(N) from soil, based on :raw-latex:`\citet{HudmanEA2012}`,
assuming a total source of 7.5Tg(N) combined with canopy uptake of 20%.
Inclusion of NO\ :math:`_2` makes it 151 speciated species in total.

Emission factors for the 20-species NO have been increased to match WRF
factors. This increase is more than conversion from N to NO. This gave
an annual total of 4.93Tg(NO) for year 2000. We use the speciated
factors to scale up to 12.85Tg(NO)/year (i.e. 6Tg(N)/year). Putting 4%
on NO\ :math:`_2`, we get speciated factors for NO = 2.50 and
NO\ :math:`_2` = 0.16 (taking conversion of NO to NO\ :math:`_2` into
account).

NH\ :math:`_3` is also emitted from soils, and is scaled to match GEIA
totals of 7.32Tg(NH\ :math:`_3`) (4.36 from crops and 2.96 as natural
vegetation). Our value of 4.93Tg(NO)/year is used to estimate the
NH\ :math:`_3` speciation factor, i.e. \ :math:`2.620`. Since MEGAN
operates on mass, this scale will get us the mass we need for
NH\ :math:`_3` (unlike NO\ :math:`_2` where molecular masses are taken
into account, converting NO to NO\ :math:`_2` on the way into the
atmosphere.

The sum of speciation factors for nitrogen components is thus larger
than one.

**Global scaling factor**: The global scaling factor :math:`Cce` is set
to 0.466, to match the total isoprene emissions in MEGAN-MACC
:raw-latex:`\citep{SindelarovaEA2014}` for year 2000 (i.e. 572Tg/yr),
using 1982–1998 climatology of LAI and z0. The scaling factor was
originally 0.56 in the MEGAN code, whereas e.g. CLM uses 0.4
:raw-latex:`\citep{GuentherEA2012}`.

**DI initialised**: The Palmer severity drought index (DI) was never
initialised, and is set to zero. In the original MEGAN code it was
allocated and that supposedly set it to zero also.

**STRESS components**: Some of the 150 species (e.g. ethane) were
assigned to 20-component number 19, but named ’STRESS’, which is
component 20. This was corrected, giving about 50% lower emissions due
to the different emission factors.

MEGAN total emissions
~~~~~~~~~~~~~~~~~~~~~

The emissions of Oslo CTM3 species coming from MEGAN are diagnosed
following the BUDGETS calendar, and put in files
*emis\_accumulated\_3d\_megan\_.nc*, similar to regular emission
tendencies.

Table [table:megantotals] lists the Oslo CTM3 MEGAN emissions for year
2000, matching the values in :raw-latex:`\citet{GuentherEA2012}` fairly
well. While our isoprene is somewhat higher, other components are lower
than in :raw-latex:`\citet{GuentherEA2012}`, mainly due to different
input datasets (LAI, roughness length, temperature, photosynthetic
active radiation). Note that CH\ :math:`_3`\ OH was included in this
process – it was not accounted for in earlier versions of Oslo CTM3.

+----------------------------------------+-----------+
| Species                                | Tg/yr     |
+========================================+===========+
| Isoprene                               | 572.27    |
+----------------------------------------+-----------+
| CO                                     | 92.74     |
+----------------------------------------+-----------+
| Formaldehyde (CH:math:`_2`\ O)         | 4.94      |
+----------------------------------------+-----------+
| Methanol (CH:math:`_3`\ OH)            | 127.95    |
+----------------------------------------+-----------+
| Ethane (C:math:`_2`\ H\ :math:`_4`)    | 29.52     |
+----------------------------------------+-----------+
| Propane (C:math:`_3`\ H\ :math:`_6`)   | 13.90     |
+----------------------------------------+-----------+
| Acetone                                | 36.09     |
+----------------------------------------+-----------+
| :math:`\alpha`-pinene                  | 58.87     |
+----------------------------------------+-----------+
| :math:`\beta`-pinene                   | 18.57     |
+----------------------------------------+-----------+
| Limonene                               | 10.67     |
+----------------------------------------+-----------+
| Ocimene                                | 15.22     |
+----------------------------------------+-----------+
| D3carene                               | 7.37      |
+----------------------------------------+-----------+
| Myrcene                                | 7.68      |
+----------------------------------------+-----------+
| Sabinene                               | 8.22      |
+----------------------------------------+-----------+
| Terpinolene                            | 1.11      |
+----------------------------------------+-----------+
| Terpinene                              | 2.10      |
+----------------------------------------+-----------+
| Terpene Alcohol                        | 4.14      |
+----------------------------------------+-----------+
| Sesquiterpenes                         | 21.36     |
+----------------------------------------+-----------+
| Terpinene-ketone                       | 2.87      |
+----------------------------------------+-----------+
| Toluene++ (Tolmatic)                   | 1.53      |
+----------------------------------------+-----------+
| Acetaldehyde (CH3CHO)                  | 24.43     |
+----------------------------------------+-----------+
| Dimethylbenzene++ (C6HXR)              | 1.48      |
+----------------------------------------+-----------+
| Dimethylsulfide (DMS)                  | 1.45      |
+----------------------------------------+-----------+
| NO                                     | 12.62     |
+----------------------------------------+-----------+
| NO\ :math:`_2`                         | 0.81      |
+----------------------------------------+-----------+
| NH\ :math:`_3`                         | 7.30      |
+----------------------------------------+-----------+
| Total VOC+CO                           | 1064.45   |
+----------------------------------------+-----------+

Table: MEGAN annual emissions in Oslo CTM3, for year 2000.

Aircraft emissions
------------------

Aircraft emission routines are located in *emissions\_aircraft.f90*. You
define which scenario (``AirScen``) to use in *Ltracer\_emis\_xxxx.inp*,
along with the path to aircraft emissions (``AirEmisPath``). If you add
new emissions, the read-in will have to be updated. As for other gaseous
species, the emissions are added to the global emission arrays.

There is no plume chemistry yet, as we have left the NILU plume
parameterisation behind, but this can eventually be included.

Aircraft emissions of H\ :math:`_2`\ O are treated as a separate tracer,
since H\ :math:`_2`\ O is not transported in the troposphere, and
depending on your setup, maybe not in the stratosphere. This separate
aircraft H\ :math:`_2`\ O tracer is given the tracer number 148, and
must be transported. It is removed below 400hPa according to
:raw-latex:`\citet{DanilinEA1998}`, by assuming a lifetime of 1hour.

Lightning emissions
-------------------

Lightning is a major contributor to NOx in the atmosphere, assumed to
amount to :math:`5 \pm 3`\ Tg(N)/year
:raw-latex:`\citep{ShumannHuntrieser2007}`. As will be described below,
we use a climatological value of :math:`\sim`\ 5Tg(N)/year, i.e. the
emissions will vary somewhat from year to year due to different
meteorology.

To properly parameterise lightning emissions, a horizontal
(geographical) distribution of lightning strikes must be used, and then
a vertical distribution must be applied.

In general, lightning flash rates in Oslo CTM3 are connected to the
horizontal distribution of convection, thus depending on the
meteorological dataset you use. To convert to observed flash rates we
need scaling factors, and these will differ for different meteorological
data (also for different cycles or resolutions). A step-by-step recipe
on how to calculate these scaling factors is explained in
Section [sxn:emissions\_lightning\_scal], but first you should
understand the horizontal and vertical distributions.

The horizontal distribution of lightning NOx (L-NOx) was updated in 2011
by Chris D. Holmes at UCI, and was documented in
:raw-latex:`\citet{SovdeEA2012}`. However, it was locked to the T42
resolution, and had to be updated in order to properly use higher
resolutions. The current version is documented below as OAS2015
(Section [sxn:emissions\_lightning\_hor]), while the GMD version is
documented as GMD2012 (Section [sxn:emissions\_lightning\_horGMD]). In
addition, I have listed here also the 2015 version of UCI (UCI2015) and
another method (AP2002).

Horizontal distribution – OAS2015
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

The :raw-latex:`\citet{PriceEA1997a}` equations for flash rates over
land and ocean are the basis for Oslo CTM3 L-NOx:

.. math::

   f(\vec{x},t) = \left\{
        \begin{array}{ll}
           f_L(\vec{x},t) = s_L\,P_L(\vec{x},t) & \textrm{over land}  \\
           f_O(\vec{x},t) = s_O\,P_O(\vec{x},t) & \textrm{over ocean}
           \label{eq:litfreqPR}
        \end{array}
     \right.

 where :math:`P_L` and :math:`P_O` are the
:raw-latex:`\citet{PriceEA1997a}` equations for a given location
:math:`\vec{x}` and time :math:`t`, but *without* scaling factors.
:math:`s_L` and :math:`s_O` are scaling factors, however, the
:raw-latex:`\citet{PriceEA1997a}` scaling factors will most likely not
be suitable when using the global meteorological fields in the model.
Therefore, we generate our own scaling factors, so that the flash rates
calculated from meteorological data match what is observed. These will
be described below. :math:`P_L` and :math:`P_O` are:

.. math::

   \begin{array}{ll}
       P_L(\vec{x},t) = H(\vec{x},t)^{4.92} & \textrm{over land}  \\
       P_O(\vec{x},t) = H(\vec{x},t)^{1.73} & \textrm{over ocean}
       \label{eq:litfreqPR2}
     \end{array}

:math:`H` is the cloud top height (km), which we represent by the height
of the uppermost grid box having convective updraft mass flux coming
into it.

| *Scaling factors*
| After :math:`P_L` and :math:`P_O` are calculated, they need to be
  scaled to match observations. I.e. we need to find :math:`s_L` and
  :math:`s_O`.

In this process, we also use the scaling factors to change units from
flashes per minute :raw-latex:`\citep[as in][]{PriceEA1997a}` to annual
fraction of flashes per second (the reason will become evident in the
next paragraphs).

Modelled total annual unscaled flashes for land and ocean, respectively,
are then

.. math::

   \begin{array}{ll}
       f_{tot,L} = \sum_{\vec{x},t}{P_L}\,\Delta t\\
       f_{tot,O} = \sum_{\vec{x},t}{P_O}\,\Delta t
       \label{eq:litfreqPR3}
     \end{array}

 where :math:`\Delta t` is the model time step, which for
Oslo CTM3 means the meteorological time step.

In other words, :math:`P_L \cdot f_{tot,L} \cdot \Delta t` through the
year sums up to 1 and :math:`P_O \cdot f_{tot,O} \cdot \Delta t` through
the year sums up to 1.

Taking the observed fraction of ocean/land flashes into account, we get
total scaling factors:

.. math::

   \begin{aligned}
     s_L &=& \frac{\frac{f_{L,obs}}{f_{O,obs}+f_{L,obs}}}
                   {f_{tot,L}}\label{eq:scalefacL}\\
     s_O &=& \frac{\frac{f_{O,obs}}{f_{O,obs}+f_{L,obs}}}
                   {f_{tot,O}}\label{eq:scalefacO}\end{aligned}

For each time step, we can now calculate :math:`P_O` and :math:`P_L`,
and multiply them with :math:`s_O` and :math:`s_L`, respectively, to get
the annual fraction of flashes per second. Multiply by the time step and
climatological annual emission (e.g. 5Tg(N)/year) and we have the amount
emitted during the time step, over land and ocean separately.

Because a climatological value is assumed for the emission
(e.g. 5Tg(N)/year), the scaling factors should also be generated for
several years, to make climatological scaling factors.

| *Scaling factors for IFS cy36r1*
| For IFS cy36r1, the scaling factors are calculated using T42L60
  meteorology, years 1997–2010, to get a climatological mean. For
  T159L60 resolution, only one year (2007) is calculated and used
  together with the same year from T42L60, to scale the T42L60
  climatological factors.

| *Scaling factors for OpenIFS cy38r1*
| OpenIFS cy38r1 is only generated in T159L60, and the scaling factors
  are calculated as a mean of 1995–2005. When combining grid boxes 2x2,
  only the year 2005 has been used.

Observed flash rates are taken from :raw-latex:`\citet{CecilEA2014}`,
where the average flash rate over land is 36.9fls\ :math:`^{-1}` and
9.1fls\ :math:`^{-1}` over ocean, using the HRAC dataset
:raw-latex:`\citep{LISOTDHRAC}`. A climatological flash rate of
46s\ :math:`^{-1}` implies an average production rate of 3.45kg(N) per
flash, or 246mol/flash. An IDL program calculating these values is given
by *lightning\_lisotd.pro* in the IDL utilities.

| *Flash filter criteria*
| The challenge is to sort out which convective plumes that produce
  lightning. If no filtering is carried out, small flash rates can be
  found even in shallow convection. Based on some of the UCI2015 method
  and also my own testing, these are the best criteria I have found:

Convective mass flux is filtered before use: If several intervals of
fluxes are found, only the thickest interval is used. Intervals of one
or two layers are disregarded.

Convective mass flux below about 1200m is disregarded to filter away
some shallow convection. This is calculated for each time step, but it
can be noted that prior to 2018, the layer was hard coded to layer 12 –
based on average ECMWF 60-layer data. To be more flexible with other
meteorology, this was changed to be calculated on the fly.

Convective mass flux must reach at least 500hPa.

Entrainment into updrafts must be non-zero above 500hPa.

Maximum temperature in the cloud must be higher than 270.5K.

Minimum temperature in the cloud must be lower than 243K.

Typically, a cloud needs both liquid and ice phase particles to produce
lightning, so it is usually assumed that minimum cloud/updraft
temperature must be lower than 233K and maximum temperature must be
higher than 273K. To allow for some uncertainty around this, we include
two scaling factors taking the temperature into account.

The first considers minimum temperature, linearly increasing from 0 at
243K to 1 at 233K.

The second considers maximum temperature, linearly increasing from 0 at
270.5K to 1 at 280.5K.

| *Note*
| As will be described next, GMD2012 use cloud fraction to make grid
  averages. However, cloud fraction is an instant meteorological field,
  while convective mass flux is accumulated. To combine them is
  inconsistent, since cloud fraction may be zero even if there were
  convective flux during the accumulated period. Hence this approach
  cannot be used.

Horizontal distribution – GMD2012
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

The GMD2012 L-NOx method documented by :raw-latex:`\citet{SovdeEA2012}`
uses the :raw-latex:`\citet{PriceRind1992}` equations and measurements
by Optical Transient Detector (OTD,
:raw-latex:`\citet{ChristianEA1999a}`) and Lightning Imaging Sensor
(LIS, :raw-latex:`\citet{ChristianEA1999b}`). The difference to the
OAS2015 scheme is the filtering of convective instances producing
lightning.

There must be convective flux in the grid box column.

The grid box average updraft velocity at :math:`\sim 850`\ hPa must be
larger than :math:`0.01`\ ms\ :math:`^{-1}`.

The surface temperature must be higher than 273K while cloud top
temperature must be lower than 233K.

In addition, the cloud fraction was used. Convective vertical fluxes and
the clouds that contain them occupy a small fraction of the horizontal
area of a model grid square. Therefore, lightning also occupies a small
fraction, :math:`a`, of the model grid area. The grid-averaged lightning
flash rate (:math:`\bar{f}`) is then

.. math:: \bar f(\vec{x},t) = a(\vec{x},t) \; f(\vec{x},t).

 where :math:`a`, the area fraction experiencing lightning, is estimated
as a weighted average of the cloud fractions in the column:

.. math:: a(\vec{x},t) = \frac{ \sum_{l=1}^{l_t} f_{c,l} w_l } {\sum_{l=1}^{l_t}  w_l}

 Here, :math:`f_{c,l}` is the cloudy fraction at level :math:`l`,
:math:`w_l` is the wet convective mass flux at level :math:`l`, and
:math:`l_t` is the model level at the cloud top.

The observed flash rate values used here was 35s\ :math:`^{-1}` for land
and 11s\ :math:`^{-1}` for ocean, as given by OTD-LIS observations
during 1998–2005.

The grid box average flash rates are next constrained to match the
climatological averages of 35s\ :math:`^{-1}` for land and
11s\ :math:`^{-1}` for ocean, as given by OTD-LIS observations during
1998–2005. This is done by scaling factors :math:`\beta_l` and
:math:`\beta_o` for land and ocean, respectively, which in
Oslo CTM3 also converts units from 1/min to 1/s. The final scaled box
average flash rate is:

.. math::

   \bar f_{scaled}(\vec{x},t) = \left\{
        \begin{array}{ll}
           \beta_l\,\bar f(\vec{x},t) & \textrm{over land}  \\
           \beta_o\,\bar f(\vec{x},t) & \textrm{over ocean}
           \label{eq:litfreqscaled}
        \end{array}
     \right.

The average flash frequencies for our meteorological data 1999–2009
(ECMWF cycle 36r1, resolution T42L60), imply that :math:`\beta_l = 6`
and :math:`\beta_o = 210`. :raw-latex:`\citet{SovdeEA2012}` presented
the factors :math:`\beta_l = 0.10` and :math:`\beta_o = 3.50`, but these
were calculated using the original units of the
:raw-latex:`\citet{PriceRind1992}` equations (flashes/minute) and were
therefore a factor 1/60 smaller.

Compared to the :raw-latex:`\citet{PriceRind1992}` coefficients, the
gridbox average flash rates are thus scaled up by about 6 over land and
200 over ocean to match observed flash rates.

As for the OAS2015 method, scaling factors need to be generated, thereby
converting to fraction of annual flashes.

Horizontal distribution – AP2002
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

There are other possible parameterisation, e.g.
:raw-latex:`\citet{AllenPickering2002}`, where the cloud-to-ground (CG)
flash frequency (:math:`LF_{cg}`) is calculated from the mass flux
(:math:`M`) at 440hPa. Hence, the convective plumes must reach 440hPa,
and the equation is

.. math:: LF_{cg} = \frac{A}{A30} (a + bM + cM^2 + dM^3 + eM^4)

 where the coefficients :math:`a`-:math:`e` are different for land and
water, and:

.. math:: 0 < M < 9.6 \textrm{kg/(m}^2\textrm{min)}

 :math:`A` is the grid box area, while :math:`A30` is grid box area at
30 degrees latitude (actually it was the area of a specific size, but
this will be of no consequence since we scale the flash rates to match
observations).

To account for intra-cloud lightning, CG fraction can be found from the
cold-cloud thickness :raw-latex:`\citep[$\Delta Z$,][]{PriceRind1993}`,
i.e. the depth (km) of the cloud above the freezing level. In the model,
it is calculated between cloud top and the midpoint of the highest layer
where the temperature exceeds 273K.

.. math::

   f_{CG} = \frac{1}{A\Delta z^4 B\Delta z^3 + C\Delta z^2
              + D\Delta z + E + 1} \label{eq:allen2002fcg}

 In Eq. ([eq:allen2002fcg]) it is assumed that :math:`\Delta z` is never
smaller than 5.5km. In addition, if :math:`\Delta z > 14`\ km,
:math:`f_{CG}` is set to 0.02, acting as an asymptotic value.
:math:`A=0.021`, :math:`B=-0.648`, :math:`C=7.493`, :math:`D=-36.54`,
:math:`E=63.09`.

The total flash rate for this scheme is then:

.. math:: f = \frac{LF_{CG}}{f_{CG}}

This method has been briefly tested in Oslo CTM3, but I found the
seasonal variation of globally averaged flash rate to misplace when
minimum and maximum occur, ans also it was very noisy. While the annual
horizontal flash rate distribution was not too bad, the variation was
also very large. This method would therefore probably not be used, but
it is included in the source code anyway.

Horizontal distribution – UCI2015
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

The 2015 method of UCI is explained in the lightning source code and
will not be mentioned here. I have decided not to use it, because it has
a very weak flash rate seasonal cycle, and the distribution is not
documented anywhere. It is also not very physical when it comes to
filtering the convective lightning events.

Horizontal distribution – other
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

The parameterisation described by :raw-latex:`\citet{GreweEA2001}` could
be an option for Oslo CTM3, but this method is rather similar to the
current approach.

Vertical distribution
~~~~~~~~~~~~~~~~~~~~~

Many CTMs distribute L-NOx vertically by using the vertical profiles of
:raw-latex:`\citet{PickeringEA1998}`. Those profiles have recently been
revised by :raw-latex:`\citet{OttEA2010}`, where the “C-shaped” emission
profile is replaced by a “backward-C” shaped profile, and is now the
default distribution in the Oslo CTM3.

The vertical distribution :raw-latex:`\citep{OttEA2010}` needs different
geographic regions to be defined, and these are taken from
:raw-latex:`\citet{AllenEA2010}`. The vertical profiles are scaled to
match the height of the convective plumes, as found in the
meteorological data.

When the Oslo CTM3 was in its early start, UCI CTM used a different
vertical distribution, namely by :raw-latex:`\citet{StockwellEA1999}`.
This treatment placed lightning emissions too low in the atmosphere, and
should not be used.

Lightning scaling factors
~~~~~~~~~~~~~~~~~~~~~~~~~

To match the lightning frequency in Eq. ([eq:litfreqPR]) to observed
values, we need to find :math:`s_O` and :math:`s_L`
(Eq. ([eq:scalefacO]–[eq:scalefacL]). To find proper values, at least
a whole year of meteorological data should be applied. Preferably,
several years should be used, to create a climatological mean.

This calculation is already done in the lightning subroutine
*lightning.f90*. Accumulated flash rates for land and ocean are
calculated using the original :raw-latex:`\citet{PriceRind1992}`
equations scaled to grid box averages, i.e. Eq. ([eq:litfreqPR]), and
from these the scaling factors are calculated. Running average values
are printed to screen at the last meteorological time step of each day,
average scaling factors are printed to screen. When you start
a simulation at day 1, the annual average scaling factor is thus found
at day 365.

The quickest way to generate scaling factors is to set ``LIT=:Y`` in
*Makefile*, and use a stripped  which only reads meteorological data and
calculates lightning.

When you have a new meteorological dataset, you should let the model run
through these calculations, and use the new factors to replace
``scaleLand`` and ``scaleOcean``.

If you have one year of e.g. T159L60 and T42L60, and know the scaling
factors for one of them, you may probably obtain suitable scaling
factors without using the climatological values mentioned above, but
instead calculate for one year of both datasets.

| *Alternative method*
| To make more specific calculations on the rates, e.g. standard
  deviations, you can also specify ``getFlashFiles=.true.`` in
  *lightning.f90*, to produce files containing the 3-hourly flash rates
  for both land and ocean. These files can be read using the IDL program
  *lightning\_flashrate.pro* or the Fortran program
  *lightning\_flashrate.f90*, found in the repository utilities.

These files get rather big, and the IDL program may be very slow for
high resolutions.

CH\ :math:`_4` emissions
------------------------

Traditionally, CTMs have used fixed surface fields of CH\ :math:`_4`,
because of its relatively long lifetime in the atmosphere. It is now
possible to rather run the Oslo CTM3 with emissions of CH\ :math:`_4`.
Whereas several emission datasets include anthropogenic
CH\ :math:`_4` emissions (e.g. EDGARv4.2 and ECLIPSE), natural emissions
are often not available. There is also soil uptake to be accounted for,
a process that is not well quantified. Our current natural
CH\ :math:`_4` source is generated by Bousquet for the GAME project,
providing emissions from biomass burning, wetlands and oceans, and
uptake by soil. This dataset also contain anthropogenic sources, which
is not used in the Oslo CTM3.

| What to remember when using CH\ :math:`_4` emissions (also described
  in the README file in model directory):

``LOLD_TREATMENT=.false.`` in *strat\_h2o.f90*.

Transport stratospheric H\ :math:`_2`\ O, H\ :math:`_2` and
H\ :math:`_2`\ Os; this requires ``NPAR_STRAT`` to be increased by 3 and
``NOTRPAR_STRAT`` to be decreased by 2 in *cmn\_size.F90*. This means
that in the *tracer\_list.d* file you have to to move H\ :math:`_2` and
H\ :math:`_2`\ Os to the transported section. And you have to add tracer
number 114 H\ :math:`_2`\ O.

Increase ``JPPJ`` in *cmn\_size.F90*, and add H2O in *ratj\_oc.d*. The
entry is listed at the bottom of the file.

Add H2O in *FJX\_spec.dat*; entry at the bottom of the file. You must
also increase the first number on the second line of the file by 1; if
this is 62, you change it to 63.

Scale the CH\ :math:`_4` emissions up or down to match steady state. Or
run the model until steady state is reached.

Include appropriate CH\ :math:`_4` emissions in the emission file.

If CH\ :math:`_4` dataset includes agricultural emissions, you need to
turn off GFED emissions from that sector. This is done by the
``partitions`` variable in *emisutils\_oslo.f90*.

| **Stratospheric H\ :math:`_2`\ O**
| When calculating CH\ :math:`_4` emissions, stratospheric
  H\ :math:`_2`\ O must also be calculated. The old treatment, using the
  sum of H\ :math:`_2`, 2xCH\ :math:`_4` and a fixed sum of these two
  and H\ :math:`_2`\ O, will not hold when CH\ :math:`_4` is allowed to
  increase or decrease. That would require the sum of the three species
  is allowed to vary (on short time scales, at least).

So when CH\ :math:`_4` emissions are included, we also calculate gaseous
H\ :math:`_2`\ O in the stratosphere. It has a tropospheric source, due
to upward transport, which is parameterized assuming tropopause
H\ :math:`_2`\ O to be 3.7ppm, but limited to the saturation value
(which is controlled by temperature).

| **Emission files**
| Anthropogenic emissions of CH\ :math:`_4` are available from several
  datasets, e.g. EDGAR v4.2 and CEDS. See *tables*\ */EMISSION\_LISTS*
  for options.

Natural CH\ :math:`_4` emissions are taken from Bousquet data, given in
a file called *fch4.ref.mask11.nc*. Its natural sources are the fields
biomass burning (``bbur``), wetlands (``wetlands``) and other sources
(``other``). The Oslo CTM3 also reads the field ``soils`` in the
deposition routine, to calculate surface dry deposition (see
Section [sxn:drydep\_ch4]). Note that when biomass burning is included
from the Bousquet data, the GFED data should not be used.

| *Important note*
| The CH\ :math:`_4` emissions must probably be scaled up or down to
  match the total loss in the model. If you are not familiar with this
  approach, ask the experienced users. Total CH\ :math:`_4` loss is
  written to standard out every month, and the emitted CH\ :math:`_4`
  should match this when in steady state.

Emission diagnoses
------------------

The emissions of each species are accumulated in the array
``DIAGEMIS_IJ``. From this array, the daily accumulated global totals
are found in the subroutine ``tnd_emis_daily`` in
*diagnostics\_general.f90*. At the dates specified by the
diagnostics/budget calendar in *LxxCTM.inp* file, the following are done
(subroutine ``tnd_emis2file``):

Globally accumulated emissions of each species since last printout are
written to standard output (i.e. result file).

The horizontal distribution of the accumulated emissions are written to
files *emis\_maps\_YYYYMMDD.dta*. If emissions are located at several
model levels, they are all written to file.

Daily accumulated emission totals are written to file
*emis\_daily\_totals\_YYYY.dta*. These are only written at the days
specified by the budget calendar.

If emissions are treated as a separate process, most of this diagnostic
is unnecessary. However, since we treat emissions as production terms in
chemistry, this diagnose must be included to assess what is actually
emitted.

Pre-defined datasets
--------------------

Here you can find some info on the different pre-defined collections of
emission files. Generally, these can be found through the ECCAD
(Emissions of atmospheric Compounds & Compilation of Ancillary Data) web
page *http://eccad.sedoo.fr/*. Emission lists can be found in the
directory *tables* and the subdirectory *EMISSION\_LISTS*. These latter
lists have to be combined into a total emission list in *tables*. Some
lists of both anthropogenic and natural emissions are already generated;
see the *tables*\ */Ltracer\_emis.inp*-files.

Keep in mind which year the files are defined for! This is usually given
by their file names.

CEDS
~~~~

Lists for CEDS anthropogenic emissions can be found at
*Ltracer\_emis\_ceds.dat*.

These are the CMIP6 emission datasets, but note that I have generated
new files for the anthropogenic emissions. This is because the CEDS
format is ill suited for extracting separate sectors.

CEDS has monthly variation.

ECLIPSE
~~~~~~~

Lists for ECLIPSE anthropogenic emissions can be found at
*Ltracer\_emis\_eclipse5.dat* and *Ltracer\_emis\_eclipse4.dat*.

ECLIPSE includes the agricultural waste burning, which is usually a part
of biomass burning. This means that when including GFED biomass burning,
you have to skip that partition. This is hard-coded in the GFED routine,
through the variable ``partitions``,

ECLIPSE has no monthly variation; it is annual mean only.

EDGAR v4.2
~~~~~~~~~~

EDGAR v4.2 anthropogenic emissions are available, and can be found in
*Ltracer\_emis\_edgar42.dat*. This file contains CH\ :math:`_4`
emissions, so if you use fixed surface CH\ :math:`_4`, you cannot
include those entries.

EDGARv4.2 has no monthly variation; it is annual mean.

Lamarque/IPCC
~~~~~~~~~~~~~

:raw-latex:`\citet{LamarqueEA2010}` anthropogenic emissions
*Ltracer\_emis\_lamarque.dat*.

The Lamarque dataset has a monthly variation.

HTAP
~~~~

More information can be found at the HTAP web page *http://htap.org/*.
HTAP emissions are given on a monthly time scale, and are based on EDGAR
data. *Ltracer\_emis\_htap.dat*.

RETRO
~~~~~

RETRO anthropogenic emissions, combined with POET natural emissions
:raw-latex:`\citep{GranierEA2005,POETreport2}`, can be found in
*Ltracer\_emis\_retro.dat* in *tables/EMISSION\_LISTS/*.

This dataset has a monthly variation.

POET
~~~~

POET provided datasets of natural emissions from ocean.
*tables/EMISSION\_LISTS/Ltracer\_emis\_ocean.dat*.

CH\ :math:`_4` emissions Bousquet
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Through the project GAME, a dataset of CH\ :math:`_4` emissions, based
on :raw-latex:`\citet{BousquetEA2011}` was generated:
*Ltracer\_emis\_ch4\_bousquet.dat*.

Keep in mind that this dataset also includes biomass burning, so you
must make sure CH\ :math:`_4` is not included in GFED.

The dataset contains several sources of CH\ :math:`_4`, however, we have
never used the anthropogenic part. Also soil sink is given.

Dataset has monthly variation for year 1984 – 2009.

Volcanic emissions
~~~~~~~~~~~~~~~~~~

Volcanic emissions of SO\ :math:`_2` was provided by both AEROCOM and
later HTAP: *Ltracer\_emis\_volcanoes.dat*.

Note that HTAP emissions are based on volcanic events, so make sure the
emissions actually cover your simulation period.

Soil emissions
~~~~~~~~~~~~~~

Soil emissions for some species: *Ltracer\_emis\_soils.dat*.

Biomass burning
~~~~~~~~~~~~~~~

Lists for biomass burning (CEDS and GFEDv4) emissions can be found at
*Ltracer\_emis\_biomassburning.dat*.

QSSA chemical integrator
========================

The numerical integration of chemical kinetics is done applying the
Quasi Steady State Approximation (QSSA) introduced by
:raw-latex:`\citet{HesstvedtEA1978}`, using three different integration
methods depending on the chemical lifetime of the species, to save
computing time.

For a given integration time step :math:`\Delta t`, the definition of
the three methods are

Long-lived: Loss :math:`< 0.1/\Delta t`

Short-lived: Loss :math:`> 10/\Delta t`

Intermediate: :math:`0.1/\Delta t \leq` Loss :math:`\leq 10/\Delta t`

The change of a tracer concentration depends on the production and loss
terms

.. math:: \frac{dC}{dt} = P - LC

 where :math:`C` is tracer concentration [molec/cm:math:`^3`], :math:`P`
is production [molec/(cm:math:`^3`\ s)] and :math:`L` is loss rate
[1/s].

Re-arranging, we get

.. math:: \frac{dC}{P - LC} = dt

 which can be integrated using the kernel rule:

.. math:: -\frac{1}{L} d\ln(P - LC) = dt

 Integrating from (:math:`C_1,t_1`) to (:math:`C_2,t_2`), we have

.. math:: \frac{P - LC_2}{P - LC_1} = e^{-(t_2 - t_1)L}

Setting :math:`\Delta t = t_2 - t_1` and solving for :math:`C_2` we get

.. math:: C_2 = \frac{P}{L} + (C_1 -\frac{P}{L}) e^{-L \Delta t}\label{eq:qssa1}

Long-lived species
------------------

Long-lived species have small :math:`L` and we can approximate

.. math:: \exp\left(-L \Delta t\right) \approx 1 - L \Delta t

 and using this, Equation ([eq:qssa1]) becomes

.. math:: C_2 = C_1 + (\frac{P}{L} - C_1) L \Delta t

Short-lived species
-------------------

Short-lived species have larger :math:`L` and we can approximate

.. math:: \exp\left(-L \Delta t\right) \approx 0

 and get

.. math:: C_2 = \frac{P}{L}

However, for :math:`P=0`, short-lived species are treated like
intermediate species.

Species of intermediate lifetime
--------------------------------

Species of intermediate lifetime and short-lived species without
production terms are integrated using the full Equation ([eq:qssa1]).

Tropospheric chemistry
======================

The tropospheric chemistry routine is in principle a 1D model, looping
through a column. It is turned on by setting ``TROPCHEM :=Y`` in the
user specified section of *Makefile*.

The tropospheric chemistry was first introduced in the CTM-1
:raw-latex:`\citep{BerntsenIsaksen1997}`, and in its general form it
contains 46 components given in Table [table:tropcomp]. Originally there
were 51 species, but 2 were not used (nr 26:
CH\ :math:`_2`\ O\ :math:`_2`\ OH and nr 47: DMS, which has a new number
in sulphur scheme) and are removed in the Oslo CTM3. Also the NOX and
NOZ families used to be unnecessarily transported, so they have been put
solely into the chemistry. O3NO (O:math:`_3`–NO) is also treated in
chemistry only (commented in Section [sxn:o3vso2]).

Transporting a species with lifetime much shorter the transport time
step (usually 15 minutes for boundary layer mixing/chemistry) may seem
inconsistent, because the species may have changed a lot during
transport. Therefore, some tracers have not been transported in
Oslo CTM3, listed with a \ **No** in the Table [table:tropcomp]. See
Section [sxn:tropchem\_nontransported] for more on this.

Some species in Table [table:tropcomp] are labeled **Maybe**, indicating
that they may be left out of transport. I advise against this. Species
should either be transported or set from some steady state initial value
in the chemistry routines.

Tropospheric water vapor is set from meteorological data and is kept
fixed for the whole meteorological time step.

+------+--------------------------------------------------+---------------------------------------------------------------------------------------------------------------+-----------------------+-------------+------------+
| Nr   | Component name                                   | Remarks                                                                                                       | Approximate           | Transport   | Wet dep.   |
+------+--------------------------------------------------+---------------------------------------------------------------------------------------------------------------+-----------------------+-------------+------------+
|      |                                                  |                                                                                                               | lifetime              |             |            |
+------+--------------------------------------------------+---------------------------------------------------------------------------------------------------------------+-----------------------+-------------+------------+
| 1    | O\ :math:`_3`                                    | Ozone                                                                                                         | :math:`\sim` months   | Yes         | Yes        |
+------+--------------------------------------------------+---------------------------------------------------------------------------------------------------------------+-----------------------+-------------+------------+
| 4    | HNO\ :math:`_3`                                  | Nitric acid                                                                                                   | :math:`\sim` weeks    | Yes         | Yes        |
+------+--------------------------------------------------+---------------------------------------------------------------------------------------------------------------+-----------------------+-------------+------------+
| 5    | PANX                                             | PAN + CH\ :math:`_3`\ COO\ :math:`_2`                                                                         |                       | Yes         | No         |
+------+--------------------------------------------------+---------------------------------------------------------------------------------------------------------------+-----------------------+-------------+------------+
| 6    | CO                                               |                                                                                                               |                       | Yes         | No         |
+------+--------------------------------------------------+---------------------------------------------------------------------------------------------------------------+-----------------------+-------------+------------+
| 7    | C\ :math:`_2`\ H\ :math:`_4`                     | Ethane [CH2CH2]                                                                                               |                       | Yes         | No         |
+------+--------------------------------------------------+---------------------------------------------------------------------------------------------------------------+-----------------------+-------------+------------+
| 8    | C\ :math:`_2`\ H\ :math:`_6`                     | Ethane [CH3CH3]                                                                                               |                       | Yes         | No         |
+------+--------------------------------------------------+---------------------------------------------------------------------------------------------------------------+-----------------------+-------------+------------+
| 9    | C\ :math:`_3`\ H\ :math:`_6`                     | Propane [CH:math:`_3`\ CHCH\ :math:`_2`]                                                                      |                       | Yes         | No         |
+------+--------------------------------------------------+---------------------------------------------------------------------------------------------------------------+-----------------------+-------------+------------+
| 10   | C\ :math:`_4`\ H\ :math:`_{10}`                  | Butane [CH:math:`_3`\ CH\ :math:`_2`\ CH\ :math:`_2`\ CH\ :math:`_3`]                                         |                       | Yes         | No         |
+------+--------------------------------------------------+---------------------------------------------------------------------------------------------------------------+-----------------------+-------------+------------+
| 11   | C\ :math:`_6`\ H\ :math:`_{14}`                  | Dimethylbutane                                                                                                |                       | Yes         | No         |
+------+--------------------------------------------------+---------------------------------------------------------------------------------------------------------------+-----------------------+-------------+------------+
| 12   | C\ :math:`_6`\ HXR                               | m-Xylene/1,3-Dimethylbenzene [C8H10]                                                                          |                       | Yes         | No         |
+------+--------------------------------------------------+---------------------------------------------------------------------------------------------------------------+-----------------------+-------------+------------+
| 13   | CH\ :math:`_2`\ O                                | Formaldehyde                                                                                                  |                       | Yes         | Yes        |
+------+--------------------------------------------------+---------------------------------------------------------------------------------------------------------------+-----------------------+-------------+------------+
| 14   | CH\ :math:`_3`\ CHO                              | acetaldehyde                                                                                                  |                       | Yes         | Yes        |
+------+--------------------------------------------------+---------------------------------------------------------------------------------------------------------------+-----------------------+-------------+------------+
| 15   | H\ :math:`_2`\ O\ :math:`_2`                     | hydrogenperoxide                                                                                              |                       | Yes         | Yes        |
+------+--------------------------------------------------+---------------------------------------------------------------------------------------------------------------+-----------------------+-------------+------------+
| 16   | CH\ :math:`_3`\ O\ :math:`_2`\ H                 | methyl hydroperoxide                                                                                          |                       | Yes         | Yes        |
+------+--------------------------------------------------+---------------------------------------------------------------------------------------------------------------+-----------------------+-------------+------------+
| 17   | HO\ :math:`_2`\ NO\ :math:`_2`                   | Peroxynitric acid                                                                                             |                       | Yes         | Yes        |
+------+--------------------------------------------------+---------------------------------------------------------------------------------------------------------------+-----------------------+-------------+------------+
| 18   | CH\ :math:`_3`\ COY                              | Bi-acetyl [CH:math:`_3`\ COCOCH\ :math:`_3`]                                                                  |                       | Yes         | No         |
+------+--------------------------------------------------+---------------------------------------------------------------------------------------------------------------+-----------------------+-------------+------------+
| 19   | C\ :math:`_3`\ COX                               | Methylethyl ketone (2-butanone),                                                                              |                       | Yes         | No         |
+------+--------------------------------------------------+---------------------------------------------------------------------------------------------------------------+-----------------------+-------------+------------+
|      |                                                  | CH\ :math:`_3`\ COC\ :math:`_2`\ H\ :math:`_5`                                                                |                       |             |            |
+------+--------------------------------------------------+---------------------------------------------------------------------------------------------------------------+-----------------------+-------------+------------+
| 20   | ISORPRENE                                        | Isoprene/2-methylbuta-1,3-diene [C5H8]                                                                        |                       | Yes         | No         |
+------+--------------------------------------------------+---------------------------------------------------------------------------------------------------------------+-----------------------+-------------+------------+
| 21   | HO\ :math:`_2`                                   |                                                                                                               | short                 | Maybe       | No         |
+------+--------------------------------------------------+---------------------------------------------------------------------------------------------------------------+-----------------------+-------------+------------+
| 22   | CH\ :math:`_3`\ O\ :math:`_2`                    | methyldioxy radical                                                                                           | short                 | No          | No         |
+------+--------------------------------------------------+---------------------------------------------------------------------------------------------------------------+-----------------------+-------------+------------+
| 23   | C\ :math:`_2`\ H\ :math:`_5`\ O\ :math:`_2`      | ethyldioxy radical                                                                                            | short                 | No          | No         |
+------+--------------------------------------------------+---------------------------------------------------------------------------------------------------------------+-----------------------+-------------+------------+
| 24   | C\ :math:`_4`\ H\ :math:`_9`\ O\ :math:`_2`      | butyldioxy radical                                                                                            | short                 | No          | No         |
+------+--------------------------------------------------+---------------------------------------------------------------------------------------------------------------+-----------------------+-------------+------------+
| 25   | C\ :math:`_6`\ H\ :math:`_{13}`\ O\ :math:`_2`   |                                                                                                               | short                 | No          | No         |
+------+--------------------------------------------------+---------------------------------------------------------------------------------------------------------------+-----------------------+-------------+------------+
| 27   | CH\ :math:`_3`\ COB                              | RO\ :math:`_2` from C\ :math:`_4`\ H\ :math:`_{10}`\ +OH, CH\ :math:`_3`\ COCH(O\ :math:`_2`)CH\ :math:`_3`   |                       | Yes         | No         |
+------+--------------------------------------------------+---------------------------------------------------------------------------------------------------------------+-----------------------+-------------+------------+
| 28   | CH\ :math:`_3`\ CXX                              | CH\ :math:`_3`\ CH(O\ :math:`_2`)CH\ :math:`_2`\ OH                                                           |                       | Yes         | No         |
+------+--------------------------------------------------+---------------------------------------------------------------------------------------------------------------+-----------------------+-------------+------------+
| 29   | AR1                                              | RO\ :math:`_2` from C\ :math:`_6`\ HXR + OH                                                                   | short                 | Maybe       | No         |
+------+--------------------------------------------------+---------------------------------------------------------------------------------------------------------------+-----------------------+-------------+------------+
| 30   | AR2                                              | Ketone from AR1                                                                                               |                       | Yes         | No         |
+------+--------------------------------------------------+---------------------------------------------------------------------------------------------------------------+-----------------------+-------------+------------+
| 31   | AR3                                              | RO\ :math:`_2` from AR3                                                                                       | short                 | Maybe       | No         |
+------+--------------------------------------------------+---------------------------------------------------------------------------------------------------------------+-----------------------+-------------+------------+
| 32   | ISOR1                                            | RO\ :math:`_2` from ISOPRENE + OH                                                                             | short                 | Maybe       | No         |
+------+--------------------------------------------------+---------------------------------------------------------------------------------------------------------------+-----------------------+-------------+------------+
| 33   | ISOK                                             | methylvinylketone + methacrolein (ketones)                                                                    |                       | Yes         | Yes        |
+------+--------------------------------------------------+---------------------------------------------------------------------------------------------------------------+-----------------------+-------------+------------+
| 34   | ISOR3                                            | RO\ :math:`_2` from ISOK                                                                                      | short                 | Maybe       | No         |
+------+--------------------------------------------------+---------------------------------------------------------------------------------------------------------------+-----------------------+-------------+------------+
| 35   | HCOHCO                                           | Glyoxal                                                                                                       |                       | Yes         | No         |
+------+--------------------------------------------------+---------------------------------------------------------------------------------------------------------------+-----------------------+-------------+------------+
| 36   | RCOHCO                                           | Methyl glyoxal, CH\ :math:`_3`\ COCHO                                                                         |                       | Yes         | No         |
+------+--------------------------------------------------+---------------------------------------------------------------------------------------------------------------+-----------------------+-------------+------------+
| 37   | CH\ :math:`_3`\ X                                | Peroxyacetyl radical, CH\ :math:`_3`\ COO\ :math:`_2`                                                         |                       | Yes         | No         |
+------+--------------------------------------------------+---------------------------------------------------------------------------------------------------------------+-----------------------+-------------+------------+
| 38   | O(\ :math:`^3`\ P)                               |                                                                                                               | short                 | No          | No         |
+------+--------------------------------------------------+---------------------------------------------------------------------------------------------------------------+-----------------------+-------------+------------+
| 39   | O(\ :math:`^1`\ D)                               |                                                                                                               | short                 | No          | No         |
+------+--------------------------------------------------+---------------------------------------------------------------------------------------------------------------+-----------------------+-------------+------------+
| 40   | OH                                               |                                                                                                               | short                 | No          | No         |
+------+--------------------------------------------------+---------------------------------------------------------------------------------------------------------------+-----------------------+-------------+------------+
| 41   | NO\ :math:`_3`                                   |                                                                                                               |                       | Yes         | No         |
+------+--------------------------------------------------+---------------------------------------------------------------------------------------------------------------+-----------------------+-------------+------------+
| 42   | N\ :math:`_2`\ O\ :math:`_5`                     |                                                                                                               |                       | Yes         | No         |
+------+--------------------------------------------------+---------------------------------------------------------------------------------------------------------------+-----------------------+-------------+------------+
| 43   | NO                                               |                                                                                                               |                       | Yes         | No         |
+------+--------------------------------------------------+---------------------------------------------------------------------------------------------------------------+-----------------------+-------------+------------+
| 44   | NO\ :math:`_2`                                   |                                                                                                               |                       | Yes         | No         |
+------+--------------------------------------------------+---------------------------------------------------------------------------------------------------------------+-----------------------+-------------+------------+
| 46   | CH\ :math:`_4`                                   | Methane                                                                                                       | :math:`\sim` years    | Yes         | No         |
+------+--------------------------------------------------+---------------------------------------------------------------------------------------------------------------+-----------------------+-------------+------------+
| 48   | C\ :math:`_3`\ H\ :math:`_8`                     | Propane                                                                                                       |                       | Yes         | No         |
+------+--------------------------------------------------+---------------------------------------------------------------------------------------------------------------+-----------------------+-------------+------------+
| 49   | C\ :math:`_3`\ H\ :math:`_7`\ O\ :math:`_2`      | Propyl peroxide; from (propane+OH)+O:math:`_2`                                                                |                       | Yes         | No         |
+------+--------------------------------------------------+---------------------------------------------------------------------------------------------------------------+-----------------------+-------------+------------+
| 50   | Acetone                                          | CH\ :math:`_3`\ COCH\ :math:`_3`                                                                              |                       | Yes         | No         |
+------+--------------------------------------------------+---------------------------------------------------------------------------------------------------------------+-----------------------+-------------+------------+
| 51   | C\ :math:`_3`\ COD                               | Propyldioxy, 2-oxo-propyldioxy,                                                                               |                       | Yes         | No         |
+------+--------------------------------------------------+---------------------------------------------------------------------------------------------------------------+-----------------------+-------------+------------+
|      |                                                  | CH\ :math:`_3`\ COCH\ :math:`_2`\ (O:math:`_2`)                                                               |                       |             |            |
+------+--------------------------------------------------+---------------------------------------------------------------------------------------------------------------+-----------------------+-------------+------------+
| 52   | CH\ :math:`_3`\ OH                               | Methanol                                                                                                      | Included 2018         | Yes         | Yes        |
+------+--------------------------------------------------+---------------------------------------------------------------------------------------------------------------+-----------------------+-------------+------------+

Non-transported species
-----------------------

One issue that has to be resolved in the Oslo CTM3 is that
non-transported species should not be used as historic values: After
other species have been transported, the non-transported species should
not be used in chemistry for the same grid box. Instead some initial
steady state value should be calculated before chemistry. Then the array
of non-transported species could be removed or kept as a diagnostic
tool.

Be sure that if the species labeled **Maybe** in Table [table:tropcomp]
are removed from transport, you should set initial values in the
chemistry. These will have to be hard coded to some steady state
expression/value, and should *never* be from a non-transported array.

It can be argued that transporting very short-lived species (labeled
**No** in Table [table:tropcomp]) makes little sense, since the tracer
concentration will have changed before getting to a new location.
However, we still keep some tracers in the non-transported array.
I mention again that historical values of the non-transported tracers
are of little value, and in worst case erratic. For OH this may not be
a big problem, due to the iteration to get a stable value, although the
old value is used for some calculations before the new value is
calculated.

Tropospheric domain
-------------------

When running tropospheric chemistry, you may or may not include
stratospheric chemistry (see Section [sxn:stratchem] for stratospheric
chemistry).

If you run *without* stratospheric chemistry, tropospheric chemistry is
calculated up to a certain altitude, but when running full chemistry
(stratospheric chemistry included) it stops at the actual tropopause
level for each column. The tropopause level for a grid box is defined by
the global array ``LMTROP`` (see Section [sxn:lmtrop]).

It is also possible to use the e90-tracer to define stratospheric air
(Section [sxn:e90]), either as a 3D field or by selecting the uppermost
tropospheric grid box as tropopause. The program code is not set up for
a 3D definition.

Without stratospheric chemistry; O\ :math:`_3`, NOx and HNO\ :math:`_3`
-----------------------------------------------------------------------

O\ :math:`_3`, NOx and HNO\ :math:`_3` are very important in the
stratosphere, and are transported down into the troposphere. In
Oslo CTM2 a flux of O\ :math:`_3` was set in the uppermost model layer
(the SynOz approach, :raw-latex:`\citet{McLindenEA2000}`), but this is
only possible for L40 vertical resolution. We have left this behind, and
now use a climatology calculated from a full-chemistry T42L60 simulation
for the years 2000–2008.

For each model latitude band (for each ``J``), tropospheric chemistry is
calculated up to a few levels above max(\ ``LMTROP(:,J)``). The number
of levels added are controlled by the parameter ``LVS2ADD2TC`` in
*cmn\_oslo.f90*. Above this, O\ :math:`_3` is set from the model
climatology.

The climatology of O\ :math:`_3` also affects ``DO3`` used in
calculation of photochemical reaction rates
(Section [sxn:photochemistry]).

Above ``max(LMTROP(:,J))+LVS2ADD2TC`` we also set HNO\ :math:`_3` and
NOx species based on climatologies. For consistency, these are from the
same T42L60 simulation as the O\ :math:`_3` climatology. This is carried
out to improve important UTLS NOx chemistry.

The old Oslo CTM2 method is still available, but should not be used. It
was based on observed mixing ratio (:math:`\mu`) correlations of NOy
with O\ :math:`_3`, assuming 40% to be NOx\ :math:`=`\ NO and 60% to be
HNO\ :math:`_3`:

.. math::

   \begin{aligned}
     \mu_{HNO_3} &=& 0.0024\mu_{O_3} \\
     \mu_{NO}   &=& 0.0016\mu_{O_3} \end{aligned}

The routines are shortly described in Appendix [app:strato3clim].

Box version of tropospheric chemistry code
------------------------------------------

A box version of the tropospheric chemistry code was produced by Alf
Grini and it is available in the Oslo CTM3 tool box at
*/div/amoc/d4-4/oslo-ctm/archive\_ctm/*\ *box\_models/*.

This directory includes some matlab scripts which will plot the
concentration daily cycles. The box can be a good educational tool, and
has been used in a bachelor level atmospheric chemistry course.

A user manual for the box application is available as tex-file in the
*photochem\_box/manual*-directory.

Stratospheric chemistry
=======================

The stratospheric application is turned on by setting ``STRATCHEM :=Y``
in the user specified section of *Makefile*.

+-------+---------------------------------+-----------------------------------------------------------+----------+
| Nr    | Component                       | Remarks                                                   | Trsp.    |
+-------+---------------------------------+-----------------------------------------------------------+----------+
+-------+---------------------------------+-----------------------------------------------------------+----------+
| 101   | MCF                             | CH\ :math:`_3`\ CCl\ :math:`_3`                           | Yes      |
+-------+---------------------------------+-----------------------------------------------------------+----------+
| 102   | HCFC22                          | CHF\ :math:`_2`\ Cl                                       | Yes      |
+-------+---------------------------------+-----------------------------------------------------------+----------+
| 103   | CFC11                           | CCl\ :math:`_3`\ F                                        | Yes      |
+-------+---------------------------------+-----------------------------------------------------------+----------+
| 104   | CFC12                           | CCl\ :math:`_2`\ F\ :math:`_2`                            | Yes      |
+-------+---------------------------------+-----------------------------------------------------------+----------+
| 105   | CCl\ :math:`_4`                 |                                                           | Yes      |
+-------+---------------------------------+-----------------------------------------------------------+----------+
| 106   | CH\ :math:`_3`\ Cl              |                                                           | Yes      |
+-------+---------------------------------+-----------------------------------------------------------+----------+
| 107   | N\ :math:`_2`\ O                |                                                           | Yes      |
+-------+---------------------------------+-----------------------------------------------------------+----------+
| 108   | Clx                             |                                                           | Yes      |
+-------+---------------------------------+-----------------------------------------------------------+----------+
| 109   | NOx\_str                        |                                                           | Yes      |
+-------+---------------------------------+-----------------------------------------------------------+----------+
| 110   | SO                              | O\ :math:`_3` + O(\ :math:`^3`\ P) + O(\ :math:`^1`\ D)   | Yes      |
+-------+---------------------------------+-----------------------------------------------------------+----------+
|       |                                 | - NO - Cl - Br                                            |          |
+-------+---------------------------------+-----------------------------------------------------------+----------+
| 111   | HCl                             |                                                           | Yes      |
+-------+---------------------------------+-----------------------------------------------------------+----------+
| 112   | Cly                             |                                                           | Yes      |
+-------+---------------------------------+-----------------------------------------------------------+----------+
| 113   | H\ :math:`_2`                   |                                                           | No/Yes   |
+-------+---------------------------------+-----------------------------------------------------------+----------+
| 114   | H\ :math:`_2`\ O                | May be used                                               |          |
+-------+---------------------------------+-----------------------------------------------------------+----------+
| 115   | SH                              | H + OH + H\ :math:`_2`\ O\ :math:`_2`                     | Yes      |
+-------+---------------------------------+-----------------------------------------------------------+----------+
| 116   | CH\ :math:`_3`\ Br              |                                                           | Yes      |
+-------+---------------------------------+-----------------------------------------------------------+----------+
| 117   | H1211                           | CF\ :math:`_2`\ ClBr                                      | Yes      |
+-------+---------------------------------+-----------------------------------------------------------+----------+
| 118   | H1301                           | CF\ :math:`_3`\ Br                                        | Yes      |
+-------+---------------------------------+-----------------------------------------------------------+----------+
| 119   | Bry                             |                                                           | Yes      |
+-------+---------------------------------+-----------------------------------------------------------+----------+
| 120   | H2402                           | CF\ :math:`_2`\ BrCF\ :math:`_2`\ Br                      | No       |
+-------+---------------------------------+-----------------------------------------------------------+----------+
| 121   | F113                            | CF\ :math:`_2`\ ClCFCl\ :math:`_2`                        | Yes      |
+-------+---------------------------------+-----------------------------------------------------------+----------+
| 122   | F114                            | CF\ :math:`_2`\ ClCF\ :math:`_2`\ Cl                      | Yes      |
+-------+---------------------------------+-----------------------------------------------------------+----------+
| 123   | F115                            | CF\ :math:`_3`\ CF\ :math:`_2`\ Cl                        | Yes      |
+-------+---------------------------------+-----------------------------------------------------------+----------+
| 124   | HNO\ :math:`_3`\ s              | Solid HNO\ :math:`_3`                                     | Yes      |
+-------+---------------------------------+-----------------------------------------------------------+----------+
| 125   | H\ :math:`_2`\ Os               | Solid H\ :math:`_2`\ O                                    | Yes      |
+-------+---------------------------------+-----------------------------------------------------------+----------+
| 126   | HCls                            | Not in use                                                |          |
+-------+---------------------------------+-----------------------------------------------------------+----------+
| 127   | HCFC123                         | CF\ :math:`_3`\ CHCl\ :math:`_2`                          | Yes      |
+-------+---------------------------------+-----------------------------------------------------------+----------+
| 128   | HCFC141                         | CF\ :math:`_3`\ FCl\ :math:`_2`                           | Yes      |
+-------+---------------------------------+-----------------------------------------------------------+----------+
| 129   | HCFC142                         | CF\ :math:`_3`\ CF\ :math:`_2`\ Cl                        | Yes      |
+-------+---------------------------------+-----------------------------------------------------------+----------+
| 130   | H                               |                                                           | No       |
+-------+---------------------------------+-----------------------------------------------------------+----------+
| 131   | HNO\ :math:`_2`                 | Not in use                                                |          |
+-------+---------------------------------+-----------------------------------------------------------+----------+
| 132   | Cl                              |                                                           | No       |
+-------+---------------------------------+-----------------------------------------------------------+----------+
| 133   | ClO                             |                                                           | Yes      |
+-------+---------------------------------+-----------------------------------------------------------+----------+
| 134   | OHCl                            |                                                           | Yes      |
+-------+---------------------------------+-----------------------------------------------------------+----------+
| 135   | ClONO\ :math:`_2`               |                                                           | Yes      |
+-------+---------------------------------+-----------------------------------------------------------+----------+
| 136   | Cl\ :math:`_2`                  |                                                           | Yes      |
+-------+---------------------------------+-----------------------------------------------------------+----------+
| 137   | OClO                            |                                                           | Yes      |
+-------+---------------------------------+-----------------------------------------------------------+----------+
| 138   | HBr                             |                                                           | Yes      |
+-------+---------------------------------+-----------------------------------------------------------+----------+
| 139   | Br                              |                                                           | Yes      |
+-------+---------------------------------+-----------------------------------------------------------+----------+
| 140   | BrO                             |                                                           | Yes      |
+-------+---------------------------------+-----------------------------------------------------------+----------+
| 141   | BrONO\ :math:`_2`               |                                                           | Yes      |
+-------+---------------------------------+-----------------------------------------------------------+----------+
| 142   | OHBr                            |                                                           | Yes      |
+-------+---------------------------------+-----------------------------------------------------------+----------+
| 143   | Br\ :math:`_2`                  |                                                           | Yes      |
+-------+---------------------------------+-----------------------------------------------------------+----------+
| 144   | ClOO                            |                                                           | Yes      |
+-------+---------------------------------+-----------------------------------------------------------+----------+
| 145   | Cl\ :math:`_2`\ O\ :math:`_2`   |                                                           | Yes      |
+-------+---------------------------------+-----------------------------------------------------------+----------+
| 146   | BrCl                            |                                                           | Yes      |
+-------+---------------------------------+-----------------------------------------------------------+----------+
| 147   | NOy\_str                        |                                                           | Yes      |
+-------+---------------------------------+-----------------------------------------------------------+----------+

Table: The components of the stratospheric application. Trsp. denotes
transport or not.

Species treated in the stratospheric chemistry are listed in
Table [table:stratcomp]. As for tropospheric chemistry there are some
non-transported species, which should either be changed into transported
species or set inside chemistry. E.g. H\ :math:`_2` must be transported
when stratospheric H\ :math:`_2`\ O is calculated
(Section [sxn:stratchem\_strath2o]), but should otherwise be a static
field.

The stratospheric chemistry was originally based on the Oslo 2D model
:raw-latex:`\citep{StordalEA1985}`, later included in the stratospheric
3D model Oslo SCTM-1
:raw-latex:`\citep{Rummukainen1996,RummukainenEA1999}` before included
in the Oslo CTM2 :raw-latex:`\citep{Gauss2003, SovdeEA2008}`.

Technical information
---------------------

The stratospheric master routine is called from the chemistry master
routine in *main\_oslo.f90*. Working on an IJ-block
(Section [sxn:parallel:ij]), it loops first through the latitudes and
longitudes of the IJ-block, calling the chemistry (``OSLO_CHEM_STR_IJ``)
column-wise.

Reaction rates dependent on temperature and pressure are defined as
arrays of length ``LPAR``, but only the stratospheric values are
calculated, i.e. from ``LMTROP+1`` to ``LPAR`` (even though chemistry is
not currently calculated in LPAR). Although the tropospheric part of the
arrays are not used, this approach is faster than allocating the arrays
on the fly. In addition, the arrays are small and minimal in numbers.
Also heterogeneous reaction rates are calculated in this way.

Stratospheric H\ :math:`_2`\ O
------------------------------

In general H\ :math:`_2`\ O is not transported in the Oslo CTM3;
tropospheric H\ :math:`_2`\ O is set from the meteorological data, while
stratospheric H\ :math:`_2`\ O is calculated from the sum of potential
hydrogen, denoted sumH2, as:

.. math::

   \textrm{H$_2$O} = \textrm{sumH$_2$} - 2\textrm{CH$_4$}
                        - \textrm{H$_2$}\label{sumh2}

 where sumH\ :math:`_2` = 7.72ppmv :raw-latex:`\citep{ZogerEA1999}`. The
old sum was 6.97ppmv, which was slightly low, although the difference
did not change the chemistry much.

Note that this value has to be lowered if you simulate pre-industrial
times..

It is also possible to transport H\ :math:`_2`\ O and calculate
H\ :math:`_2`\ O in the stratospheric chemistry, by setting
``LOLD_H2OTREATMENT=.false.`` in *strat\_h2o.f90*. If you want to do
this, you need to

-  Set ``LOLD_H2OTREATMENT=.false.``.

-  Transport H\ :math:`_2`\ O (component 114), H\ :math:`_2` (113) and
   solid H\ :math:`_2`\ O (125), i.e. in *cmn\_size.F90* you must add 3
   (113, 114 and 125) to ``NPAR_STRAT`` and remove 2 from
   ``NOTRPAR_STRAT`` (113 and 125).

-  Include cross section information for fast-JX in *FJX\_spec.dat*, put
   them after Acet-b (H:math:`_2`\ O values can be found in
   *FJX\_spec\_h2o.dat*).

-  Add 1 to the first two numbers in the header of *FJX\_spec.dat*,
   i.e. change ``62`` to ``63``.

-  Also add H2O in *ratj\_oc.d*.

-  In *cmn\_size.F90* you must add 1 to ``JPPJ``.

| **Tropospheric source of stratospheric H\ :math:`_2`\ O**
| H\ :math:`_2`\ O is transported throughout the model domain. However,
  we only calculate H\ :math:`_2`\ O chemistry in the stratosphere. In
  the troposphere, H\ :math:`_2`\ O is set from the meteorological data.
  Consequently, our H\ :math:`_2`\ O tracer is a purely stratospheric
  tracer, with a tropospheric source. The tropospheric source is set
  before transport, and is calculated assuming a mixing ratio of 3.7ppm
  at the tropopause, but limited to saturation mixing ratios.

To avoid large values coming up, the tropospheric values are set in the
tropopause level and 4 levels below it. From there and down to the
surface, a very small value is set.

In this treatment, H\ :math:`_2`\ O mixing ratio in the uppermost model
layer is assumed to be the same as in the layer below (routine
``strat_h2o_ubc2`` in *strat\_h2o.f90*).

| *Important*
| This new H\ :math:`_2`\ O treatment is better than the old version,
  which should eventually be abandoned. It should be applicable also for
  pre-industrial simulations, assuming the tropopause mixing ratio has
  not changed much. However, tropopause temperatures may indeed have
  changed, but the largest contribution to changes in stratospheric
  H\ :math:`_2`\ O from pre-industrial times is due to increased
  CH\ :math:`_4`.

Further testing is needed before using the new treatment widely.

Microphysics
------------

The microphysics scheme calculates the formation and evolution of polar
stratospheric clouds (PSCs). It is a column model, which fits very well
into the IJ-block structure. The microphysics are explained in detail by
:raw-latex:`\citet{SovdeEA2008}`, but will be included here as well.

You can turn off PSC heterogeneous chemistry by the logical ``LPSC`` in
*psc\_microphysics.f90*, where you also can turn off heterogeneous
chemistry on stratospheric background aerosols (``LAEROSOL``).

Particles are calculated for :math:`N_B` size bins, originally set to
40, but since early 2018 the default is 15 bins to save computing time
(sedimentation is done for each bin). Using 15 bins covering the same
radius range only changes total O\ :math:`_3` column by up to 0.2%, also
in O\ :math:`_3` hole conditions (year 2016 have been tested). If you
experience larger errors in the O\ :math:`_3` hole conditions, you may
consider increasing :math:`N_B`.

The calculated surface area densities ``PSC1`` and ``PSC2`` are given in
global fields of size ``(LPAR,IPAR,JPAR)``. These are used to calculate
reaction rates in the stratospheric chemistry master routine.

An important aspect of this microphysics is that sulfuric acid
(H:math:`_2`\ SO\ :math:`_4`) is needed in the calculations. As long as
H\ :math:`_2`\ SO\ :math:`_4` is not calculated as a species in the
stratosphere, it is computed from an existing background aerosol
distribution (see Section [sxn:stratchem\_hetchem]), assuming some
content of H\ :math:`_2`\ SO\ :math:`_4` (see
:raw-latex:`\citet{SovdeEA2008}` for more). As explained in the
following text, the background aerosol surface area may be modified by
the microphysics scheme, and to keep the consistency, the amount of
sulfuric acid is always computed from the original satellite data.

| **Important**
| The PSC microphysics module limits calculations to an altitude of
  30km. This is to reduce the number of unnecessary calculations.
  However, it means that for other aerosols, the satellite based surface
  aerosol density will be used. It could be that using the PSC
  microphysics above 30km could alter possible aerosols there.

| **Formation**
| Given an amount of HNO\ :math:`_3`, H\ :math:`_2`\ SO\ :math:`_4` and
  H\ :math:`_2`\ O, the composition of a ternary solution is calculated
  according to :raw-latex:`\citet{CarslawEA1995}`. This includes
  HNO\ :math:`_3` freezing on ice assuming 3K supercooling below
  :math:`T_{ICE}` from :raw-latex:`\citet{MartiMauersberger1993}`:

  .. math:: T_{ICE} = \frac{2663.5}{12.537 - \log_{10}p_{H_2O}}

   with :math:`p_{H_2O}` in Pa.

The formed particle is treated as background aerosols (temporarily
overwriting the background aerosol satellite data) unless the
temperature is below :math:`T_{NAT}` given by
:raw-latex:`\citet{HansonMauersberger1988}` for pressures in Torr:

.. math::

   \begin{aligned}
     \log p_{HNO_3} &=& m(T)\log p_{H_2O} + b(T)\\
     m(T) &=& -2.7836 - 0.00088T\nonumber\\
     b(T) &=& 38.9855 - \frac{11397}{T} + 0.009179T\nonumber\end{aligned}

 At equilibrium we have :math:`T=T_{NAT}`. Below this temperature the
particle is treated as supercooled ternary solution (STS, also known as
PSC1b).

Further, this ternary solution is assumed to be liquid until freezing at
a temperature

.. math:: T_{FRZ} = \frac{-9384}{\log_{10}\left(p_{HNO_3}p_{H_2O}\right)-39}

 where the pressures are in Torr. This expression is close to the
freezing temperature SAT:MixH reported by
:raw-latex:`\citet{FoxEA1995}`, of a mixture of SAT
(H:math:`_2`\ SO\ :math:`_4\cdot`\ 4H\ :math:`_2`\ O) and
HNO\ :math:`_3\cdot` H\ :math:`_2`\ SO\ :math:`_4\cdot`
5H\ :math:`_2`\ O. :math:`T_{FRZ}` is in general 3-6K lower than
:math:`T_{NAT}`, in agreement with several studies
:raw-latex:`\citep[e.g.][]{VoigtEA2005}`. The particle is then assumed
to be nitric acid trihydrate (NAT, or PSC1a). Once formed, the NAT will
stay NAT until it melts at the temperature reported by
:raw-latex:`\citet{ZhangEA1993}`:

.. math:: T_{MELT} = \frac{3236}{11.502 - \log_{10}p_{H_2O}}

 where :math:`p_{H_2O}` is in Torr.

:raw-latex:`\citet{SovdeEA2011}` let the NAT particles grow, also
letting the volume of PSCs grow without a limit, whereas the original
treatment in :raw-latex:`\citet{SovdeEA2008}` used a volume limit as in
the routine it was based on. We keep this limit in Oslo CTM3 because
otherwise the particle area density will grow too large. This was never
an issue in the Antarctic, but made too large PSCs when modelling the
2011 Arctic spring.

| **Sedimentation**
| Sedimentation is done according to :raw-latex:`\citet{Kasten1968}` for
  each size bin in the log-normal size distributions.

Heterogeneous chemistry
-----------------------

In Oslo CTM3 stratosphere, heterogeneous chemistry comprises reactions
on PSCs and background aerosols. The reaction rates depend on the
surface area density of the particles, which are either given by monthly
averaged satellite data or calculated by the microphysics scheme (see
Section [sxn:stratchem\_microphysics]).

Input files needed
------------------

The stratospheric module needs information about stratospheric
background aerosols and also boundary conditions for the species treated
only in the stratosphere (surface conditions to simulate emissions, and
upper boundary conditions to simulate interaction with the atmosphere
above).

The data are located in the directory *Input\_CTM3*:

Climatological background aerosol data are given in the directory
*backaer\_monthly* (described in Section [sxn:otherinput]).

Boundary conditions generated from the Oslo 2D model are located in the
directory *2d\_data*. Note that the last year of data is for 2011 due to
the Oslo 2D model being discontinued. See also next Section for upper
boundary alternatives.

You also need to have the correct tracer list *tracer\_list.d*, with the
correct number of species, which you can find in the directory
*tables/*.

Upper boundary / Chemistry in LPAR?
-----------------------------------

So far, the Oslo CTM3 and Oslo CTM2 have not calculated chemistry at the
top model layer (``LPAR``). This may be revised in the future. The use
of Oslo 2D data as upper boundary conditions must be revised! The
uppermost level of the 2D model is about 55km, well below the L60 top
level. Using 2d-data is OK for L40, but not ideal for L60. However, in
lack of other boundary conditions we scale the 2D data to L60 using the
uppermost mixing ratio gradient.

The best solution is to do chemistry for all species in ``LPAR``.
However, it can be tricky for some tracers if a flux in or out is
needed, such as species with long stratospheric lifetimes where the
mesosphere provides an important sink. Although a bit tricky, that
should rather be parameterised.

Using another model (e.g. WACCM) or observations as upper boundary
condition is better than using Oslo 2D results. However, any fixed upper
boundary will possibly act as a sink or source, and the upper boundary
will depend on whether pre-industrial, current or future atmosphere is
modelled.

If chemistry is to be calculated in ``LPAR``, this must be done:

Reaction rates dependent on temperature and pressure must be calculated
in ``LPAR`` (in subroutine ``TCRATE_TP_IJ_STR`` and ``TCRATE_HET_IJ``).

The routine updating the upper boundary must be removed
(``update_strat_boundaries`` in *stratchem\_oslo.f90*).

Climatological O\ :math:`_3`, temperature and mass for layer ``LPAR``,
used in ``photoj`` must be removed. Can also be removed from ``set_atm``
in *fastjx.f90*.

Linoz
-----

Linoz v2 :raw-latex:`\citep{McLindenEA2000}` is included in the file
*p-linoz.f*. However, the Oslo CTM3 is not set up to use this instead of
stratospheric chemistry, only as part of the STE diagnostic tool
(Section [sxn:ste]).

However, all UCI Linoz codes are present, so setting it up as
an alternative stratosphere should be rather straight-forward.

Photochemistry
==============

The photodissociation rates (J-values) are calculated on-line using the
fast-JX method, version 6.7c :raw-latex:`\citep{PratherFastJX67c}`.
fast-JX calculates dissociation rates in the troposphere and
stratosphere. When treating only the troposphere, there are 20 values
calculated, and for the stratospheric application there are 49. The
parameter name for this is ``JPPJ`` and is automatically set in
*cmn\_size.F90*. The reactions are listed in *ratj\_oc.d*.

You can set up the model to calculate J-values every chemical time step
(``NRCHEM``), or to calculate only once every step of the operator split
(``NOPS``). This is controlled in the input file (see
Section [sxn:maininput]) using the parameter ``LJCCYC`` (J-values
constant in the chemistry cycle). Each parallel block (IJ-block)
calculates its J-values, and stores them in the array ``JVAL_IJ``, which
is a global B-like array of size ``(JPPJ,LPAR,IDBLK,JDBLK,MPBLK)``. This
minimises the striding necessary for each parallel block.

The master routine is ``jvalues_oslo``, where J-values for each column
is calculated by the driver ``jv_column``, and stored in the global
``JVAL_IJ``. These subroutines are located in *main\_oslo.f90* (see
Appendix [app:core\_pphot] and [app:main\_oslo]).

The driver calls the fast-JX routine ``PHOTOJ`` located in the file
*p-phot\_oc.f*.

The fast-JX uses the ozone column to calculate radiative properties. To
account for the atmosphere above the model, a climatology is applied.
This is carried out in the subroutine ``SET_ATM`` in *fastjx.f90*.

``SET_ATM`` provides information about what is above the model top
(above ``ETAA(LPAR+1)``) as

``TOPT``: Temperature

``TOPM``: Mass

``TOP3``: O3 concentration

In addition we have computed the values for layer ``LPAR`` (between
``ETAA(LPAR)`` and ``ETAA(LPAR+1)``), since a climatology is thought to
be more reliable than the 2D upper boundary conditions. These arrays are
``LPART``, ``LPARM`` and ``LPAR3``.

Important input to the J-values are cloud cover and aerosol
distribution, since they have important scattering properties, and these
will be addressed next. First a short note about solar flux.

Solar flux
----------

The solar flux is listed at the top of the table *FJX\_spec.dat*, named
#/cm2/s. It is the average of solar low (11 Nov 1994) and 80% of solar
high (29 Mar 1992). The values apply for the wave lengths given in the
same file. The values are from the Solar-Spectral Irradiance Monitor
(SUSIM).

The solar low and solar max values are found in the file
*FJX\_solminmax.dat*. It is possible to include an interpolation between
these based on observed solar cycle. See *fastjx.f90* and search for
``FLmin`` or ``FLmax`` to find out more. I am not completely sure that
a linear interpolation between min and max is correct for all wave
length bins, but it is the easiest way to do this at the moment.

Solar flux can be found at Natural Resources Canada,
http://www.spaceweather.gc.ca/solarflux/sx-5-en.php, and when I tested
this I made 7-month running mean of the fraction of maximum flux (i.e. 1
is max and 0 is solar min). A file can be found in the *tables*
directory: *solflux\_7month\_running\_mean\_1990\_2016.dat*. You have to
specify ``FLscale`` (see *fastjx.f90*) fits the number of years in this
file.

Note that including the solar cycle will only affect photochemistry.
This effect is not so large as the effect of changed atmospheric
temperatures, which is already included in the meteorological data.

Cloud cover
-----------

If no clouds are found in a model column, the J-values in a model column
are calculated straight-forward; only one calculation is necessary.
Clouds, however, makes the calculations more complicated.

In a given model column, there may be different types of clouds with
different cloud fractions, which may overlap or not. Using the cloud
cover in the meteorological data, there are several ways to calculate
the cloud optical properties used for J-values in fast-JX.

How to treat the clouds in fast-JX is defined in the *LxxCTM.inp*-files,
where you can set a random cloud cover treatment; so if you set all to
false, the result will be the classic average cloud cover as in
Oslo CTM2. The default treatment is ``LCLDRANA``.

The treatment of overlapping clouds is described by
:raw-latex:`\citet{NeuEA2007}`, and is also described shortly below.

Average cloud cover
~~~~~~~~~~~~~~~~~~~

Average cloud cover means that clouds covering a fraction of a gridbox
is averaged to a cloud covering the whole grid box. E.g. a convective
cloud with optical depth of 20 covering 20% of the grid box, will be
treated as a cloud covering the whole grid box by an optical depth of 4.
:raw-latex:`\citet{NeuEA2007}` showed that this is not a good
approximation.

Random cloud cover
~~~~~~~~~~~~~~~~~~

Clouds with cloud fraction (CF) less than 1 does not cover the whole
grid box. They may be located at different places and they may overlap.
There are two main types of treating this overlap; random and
maximum-random overlap :raw-latex:`\citep{NeuEA2007}`.

In the Oslo CTM3 the calculations are carried out over
``NDGRD``\ =\ ``IDGRDxJDGRD`` columns of cloud properties.
``IDGRD``\ =\ ``IPARW/IPAR``, and similarly for ``JDGRD``; in other
words they will be 1 for non-degraded horizontal resolution. ``NDGRD``
is therefore the number of native columns covered by a column in
a degraded run. For non-degraded, ``NDGRD=1``. For a degraded-grid
simulation where each column covers several native columns, the native
information from the meteorological data is used.

For each column the clouds are grouped into maximum-overlapping clouds:
Clouds from neighboring levels are expected to be closely connected and
assumed to form a maximum-overlap group.

For each maximum-overlap group, the number of possible combinations of
columns are calculated. These are the independent cloud atmospheres,
ICAs). In this process, as described by :raw-latex:`\citet{NeuEA2007}`,
the total number of ICAs has been reduced by grouping cloud fractions
into bins (there are ``CBIN_`` of them (set in *cmn\_fjx.f90*).

For each ICA the total column optical depth (TOD) is calculated. How the
TOD values are treated depends on your random scheme choice. The TOD
values are the basis for creating 4 quadrature atmospheres (QA), which
are defined by optical depths:

QA(1): 0-0.5 (clear sky)

QA(2): 0.5-4 (cirrus hazy)

QA(3): 4-30 (stratus)

QA(4): >30 (cumulus)

For further reading see :raw-latex:`\citet{NeuEA2007}`. As a general
user, your choices are to set the *LxxCTM.inp* switches, which are:

| **``LCLDQMD``**
| Use mid-point of quadrature cloud cover ICAs. This is the original
  method published by the UCI group :raw-latex:`\citep{NeuEA2007}`. It
  generates all the possible max-ran overlap cloud profiles, then sorts
  them in order of increasing optical depth (OD), and breaks those into
  4 groups based on the standard breakpoints defined above.

Next, each group (may be less than 4) has a fractional area, from which
the mid-point of that area is picked to find the ICA that is used.

This is the most expensive because it sorts a large number of ICAs and
then results on average in 2.5 to 2.8 calls to fast-JX per time step.

| **``LCLDQMN``**
| Mean quadrature cloud cover ICAs. Generates all max-ran ICAs and then
  averages them into the four quadrature bins. No sorting by OD, but
  still used up to 4 ICAs per step. **``LCLDRANA``**
| Random selected from all cloud cover ICAs. This is the cheapest and
  fastest - it picks a random number (fractional area = 0–1) and then
  generates the ICAs until it reaches that fractional area and selects
  that ICA. No sorting and only 1 call to fast-JX per time step. This is
  the default.

| **``LCLDRANQ``**
| Random selected from 4 mean quadrature cloud cover ICAs. Generates the
  ICAs and up to 4 quad atmospheres (as for ``LCLDQMN``), but then picks
  one of the 4 based on a random number.

Aerosols in fast-JX
-------------------

fast-JX can handle different aerosol types, usually fetched from the
tracer array (``STT``), but can also be set from climatologies. The
arrays and routines used for this is found in *aerosols2fastjx.f90*,
where climatologies from previous simulations also can be read in.

You can turn off the effect of aerosols on fast-JX with the switch
``LJV_AEROSOL`` in *aerosols2fastjx.f90*. Here you also define which
aerosols that can affect fast-JX. If you do a simulation without these
aerosols, fast-JX will skip them also, unless a climatology is read in.

Aerosols may grow in size due to relative humidity (RH), and the optical
properties for such aerosols are treated slightly different than for dry
aerosols. The properties are listed in the files
*tables/*\ *FJX\_scat.dat* for aerosols independent of RH and
*tables/*\ *FJX\_UMaer.dat* for RH-dependent. These files will be
further described in Section [sxn:scat\_prop].

Using this module requires initialisation (separate routine in the
module) and then to set the aerosol path arrays before fast-JX. This is
done in *main\_oslo.f90*.

Each aerosol type to include must be given a matching entry in the files
listing optical properties (*tables/*\ *FJX\_scat.dat* or
*tables/*\ *FJX\_UMaer.dat*). The scattering properties themselves are
described in the respective aerosol sections.

If climatologies are used, they are given as monthly means of aerosol
paths and are interpolated linearly in time, between two monthly means,
at 00UTC every day.

If you change or add aerosol types, you may need to do changes in this
module.

Generating cross sections
-------------------------

A program is available on Prather’s web page, that allows you to
generate cross sections. You only need to write your own small
subroutine, which should be fairly easy after looking at the existing
ones. You will need to look up JPL :raw-latex:`\citep{jpl06-2,jpl10-06}`
or IUPAC :raw-latex:`\citep{IUPACweb}` to get the data needed. You can
also find a version of this program in the utilities repository (see
Appendix [app:repository]).

Generating scattering properties
--------------------------------

When an aerosol is taken into account by fast-JX, its scattering and
extinction properties are needed. You find pre-calculated values for
aerosols independent on humidity in the file *FJX\_scat.dat*, while
scattering properties for aerosols depending on humidity can be found in
*FJX\_UMaer.dat*.

There are several ways to generate these numbers. The original one is by
the program ``FJ_phase``.

The better option is, however, a method by Mishchenko. It is better
documented and also works better for producing humidity dependent
parameters for *FJX\_UMaer.dat*. See Section [mishchenko\_spher] for
more.

Both methods can be found in the utilities repository (see
Section [app:repository] to find where).

An entry in the *FJX\_scat.dat* is shown in Table [table:fjxscat].

| llllllllll
| 200 & 2.5935 & 1.0000 & 2.092 & 2.914 & 2.880 & 3.295 & 3.185 & 3.430
  & 3.379
| 300 & 2.6669 & 1.0000 & 2.121 & 2.861 & 2.792 & 2.936 & 2.733 & 2.703
  & 2.568
| 400 & 2.5588 & 1.0000 & 2.144 & 2.813 & 2.711 & 2.695 & 2.425 & 2.257
  & 2.069
| 600 & 2.1893 & 1.0000 & 2.149 & 2.713 & 2.547 & 2.362 & 2.018 & 1.740
  & 1.499
| 999 & 1.4540 & 1.0000 & 2.118 & 2.537 & 2.277 & 1.951 & 1.555 & 1.229
  & 0.972
| :math:`\lambda` & :math:`Q_{ext}` & :math:`\overline{w}_0` &
  :math:`\omega^1` & :math:`\omega^2` & :math:`\omega^3` &
  :math:`\omega^4` & :math:`\omega^5` & :math:`\omega^6` &
  :math:`\omega^7`

An aerosol entry in *FJX\_UMaer.dat* consists of 21 lines covering
optical parameters for relative humidity 0% to 95 % with increments of
5, and finally adding 99%. For 6 wavelengths, 200nm, 300nm, 400nm,
550nm, 600nm and 1000nm, 3 optical variables are stored: single
scattering albedo, asymmetry parameter (:math:`g`) and the extinction
coefficient (:math:`Q_{ext}`).

| *Size bins / Monodisperse particles*
| Some aerosols are assumed to have fixed size, as for dust and sea
  salt, so that several tracers make up the size distribution. Separate
  properties may then be calculated assuming monodisperse aerosols, as
  explained in the Mishchenko code.

FJ\_phase
~~~~~~~~~

This is the generator I got from UCI, originating from
:raw-latex:`\citet{HansenTravis1974}`. The process is as follows:

Compile *mie.for*.

``mie < mie.in > mie.out``

Compile *leg\_mieout.for*.

``leg_mieout < mie.out > leg.out``; leg.out contains the Legendre
expansion, plus phase function vs. angle. The *FJX\_scat.dat* values are
found in the file fort.7.

You may also do the following:

Compile *rd\_mieout.for*.

``rd_mieout < mie.out``; Look at stdout and check summary of aerosol
distributions.

| **mie.in**
| The *mie.in* file consists of these parameters:

--------------

::

     NSTEPS=04 NUMSD=01 NG=192IPART=10 NSIZEP=0 MIESPR=0 MIEOUT=3 SIZLIM=1.0D-09
     STEP= 0.05 FROM=00.00 TO= 2.00
     STEP= 0.10 FROM= 2.00 TO= 8.00
     STEP= 0.25 FROM= 8.00 TO=10.00
     STEP= 0.25 FROM=10.00 TO=90.00
     NSD=3  A=  0.080 B=  0.800 C=  0.000 RR1=  0.00 RR2=  2.50
     WAVL= 0.200 NR=1.460 NI=0.000
     WAVL= 0.300 NR=1.460 NI=0.000
     WAVL= 0.400 NR=1.460 NI=0.000
     WAVL= 0.600 NR=1.460 NI=0.000
     WAVL= 0.999 NR=1.460 NI=0.000

--------------

| ``NSTEPS``
| The NSTEPS are set up to feed into the Legendre fitter that is the
  second part of the code, do not change these unless you want more
  forward peak resolution. It breaks the solution at the scattering
  phase angle into sections of different resolution (more at the forward
  and backward peak as noted).

| ``NSD``
| Size distribution type. All are described in
  :raw-latex:`\citet{HansenTravis1974}`.

-  Two-parameter Gamma function.

-  Bi-modal Gamma function.

-  Log-normal.

-  Power law.

| ``IPART`` and ``NG``
| IPART and NG are for the integration over the size distribution (R).
  This is obscure, but historical in a way I haven’t fully figured out.
  The size distribution is broken into IPART segments and each segment
  is then integrated over R in the range from RBOT to RBOT + RDIV using
  NG (96 in this case) Gauss-point quadrature. Thus the NG\*IPART is the
  effective number of R’s used to integrate the scattering.

| ``A, B, C``
| The parameters A, B and C depends on the size distribution.

| *NSD=1; Gamma function*
| 

  .. math:: n(r) = \textrm{constant } r^{(1/B-3)} \exp(-r/(AB))

   where the constant is given in the fortran program and

  .. math::

     \begin{aligned}
       A &=& \textrm{effective radius} \nonumber\\
       B &=& \textrm{effective variance} \nonumber\end{aligned}

This probably comes from Deirmendjian’s modified GAMMA (pure GAMMA when
:math:`\gamma = 1`)

.. math:: n(r) = a r^{\alpha} \exp(-b r^{\gamma})

.. math:: N = a / \gamma  b^{[-(\alpha+1)/\gamma]}  \Gamma[ (\alpha+1)/\gamma ]

 The mode radius is :math:`r_c`:

.. math:: r_c^{\gamma} = \alpha / (\gamma * b)

 where

.. math::

   \begin{aligned}
     \alpha &=& \textrm{dimensionless integer} \nonumber\\
     b &=& \textrm{has units of 1/radius} \nonumber\end{aligned}

Lacis’s GAMMA: (NSD=1)

.. math::

   \begin{aligned}
      A &=& (\alpha + 3) / b = r_{eff} \\
        &&  \Rightarrow 1/b = r_{eff}/(\alpha+3) \nonumber\\
      B &=& 1 / (\alpha + 3) \\
      r_c &=& A * B  * (1/B - 3) = A * (1 - 3*B) \\
      r_c &=& r_{mode} = r_{eff} * \alpha/(\alpha+3)\end{aligned}

| *NSD=2; bi-modal Gamma distribution*
| 

  .. math::

     \begin{aligned}
       n(r) &=& \frac{1}{2} \frac{r^{1/B-3}\exp[-\frac{r}{AB}]}
                               {(AB)^{(1/B-2)}\Gamma[1/B-2]} \nonumber\\
            &&+ \frac{1}{2} \frac{r^{1/B-3}\exp[-\frac{r}{CB}]}
                               {(CB)^{(1/B-2)}\Gamma[1/B-2]}\end{aligned}

   Here we have

  .. math::

     \begin{aligned}
       A &=& \textrm{effective radius of mode 1} \nonumber\\
       B &=& \textrm{effective variance} \nonumber\\
       C &=& \textrm{effective radius of mode 2} \nonumber\end{aligned}

| *NSD=3; Log-normal distribution*
| 

  .. math::

     n(r) = \frac{1}{\sqrt{2\pi}\sigma_g}\frac{1}{r}
              \exp\left[-\frac{(\ln r - \ln r_g)^2}{2\sigma_g^2}\right]

   where

  .. math::

     \begin{aligned}
       A &=& r_g \nonumber\\
       B &=& \ln(\sigma_g)  \nonumber\end{aligned}

| *NSD=4; Power-law*
| For :math:`a\ne 1` we have

  .. math::

     \begin{aligned}
       n(r) &=& \frac{(a-1)r_1^{(a-1)}r_2^{(a-1)}}{r_2^{(a-1)} - r_1^{(a-1)}}r^{-a}\nonumber\\
            &=& \frac{1-a}{r_2^{(1-a)} - r_1^{(1-a)}}r^{-a}\quad\textrm{for }r_1\le r \le r_2\\
            &=& 0 \quad \mathrm{otherwise}\end{aligned}

   and for :math:`a=1`

  .. math::

     \begin{aligned}
       n(r) &=& \frac{r}{\ln{r_2/r_1}} \quad\textrm{for }r_1\le r \le r_2 \\
            &=& 0 \quad \mathrm{otherwise}\end{aligned}

   where

  .. math::

     \begin{aligned}
       A &=& a \nonumber \\
       C &=& r_2 \nonumber\\
       B &=& r_1  \nonumber\end{aligned}

Mishchenko spher.f
~~~~~~~~~~~~~~~~~~

There are several codes for scattering available online at Mike
Mishchenko’s web page at GISS. His code for spherical aerosols is given
in the *spher.f*, and is clearly based on the same source as
``FJ_phase``, the work of :raw-latex:`\citet{HansenTravis1974}`. This
program is very well documented in the program file.

As already noted, you can find this code in the utilities of the SVN
repository (Section [app:repository]), and a modified version for the
Oslo CTM3 can be found in the file *spher\_ctm3.f*.

If you use the original program, you will have to hard-code the
parameters and run the program for each wavelength you need.

The modified *spher\_ctm3.f* loops over the wave lengths used in the
Oslo CTM3, and may also loop over relative humidity.

| *RH-dependent or -independent*
| For RH-dependent aerosols, we set ``LRHDEPENDENT=.true.``, otherwise
  to false. When ``.true.``, the program will loop through RH from 0%
  with increments of 5.

If RH-dependent, look for results in *umdata.dat*, and for
RH-independent in *scatdata.dat*.

| *What to change*
| Refractive indices are listed in ``ASMRR`` and ``ASMRI``, and apply
  for wave lengths given in ``ASLAM``. You have to define size
  distributions and how the radius grow due to RH. See the file to find
  out more.

Sulphur module
==============

The tropospheric sulphur chemistry is turned on by setting
``SULPHUR :=Y`` in the user specified section of *Makefile*. It is
documented by :raw-latex:`\citet{BerglenEA2004}`.

To run this application you need to include at least the tropospheric
chemistry module. The scheme needs 5 additional tracers, which are
listed in Table [table:sulfcomp]. They need to be transported, and 4 of
them are subject to wet deposition.

Originally two additional species included in the tracer array;
CS\ :math:`_2` and OCS (carbonyl sulfide). However, they were calculated
(no chemistry, no transport), so they are left out. They are not
included because they are not important in the troposphere, but they do
play a role in the stratosphere. When including sulphur chemistry in the
stratosphere, these should be included.

+------+-----------------------------------+-----------+
| Nr   | Component name                    | Wet dep   |
+======+===================================+===========+
| 71   | DMS (dimethyl-sulfide [4]_)       | Yes       |
+------+-----------------------------------+-----------+
| 72   | SO\ :math:`_2`                    | Yes       |
+------+-----------------------------------+-----------+
| 73   | SO\ :math:`_4`                    | Yes       |
+------+-----------------------------------+-----------+
| 74   | H\ :math:`_2`\ S                  | No        |
+------+-----------------------------------+-----------+
| 75   | MSA (methanesulfonic acid [5]_)   | Yes       |
+------+-----------------------------------+-----------+

Table: The components of the sulphate application.

Sulphur emissions
-----------------

For SO\ :math:`_2` emissions it is usually assumed that 2.5% is emitted
as sulphate, to account for oxidation. There are several datasets
available for anthropogenic emissions. Biomass burning is usually from
the Global Forest fires Emissions Database, with a vertical distribution
from RETRO. Volcanic emissions are usually taken from AEROCOM or HTAP.
Note that HTAP emissions are based on volcanic events, so make sure the
emissions actually cover your simulation period.

No information is available on H\ :math:`_2`\ S emissions from
vegetation and soil, but they could be as described by
:raw-latex:`\citet{BerglenEA2004}`.

DMS from ocean is parameterized as in
:raw-latex:`\citet{NightingaleEA2000}`, where the DMS ocean
concentration for each month is taken from
:raw-latex:`\citet{KettleAndreae2000}`, a climatology based on
observations. The Kettle climatology for DMS was updated in 2010 by
:raw-latex:`\citet{LanaEA2011}`, and this field is now also available.
The updated climatology should be tested before it is used extensively.
Also see Section [sxn:emissions\_stv].

In the Oslo CTM3 this climatology is given in the variable
``DMSseaconc``, and is converted from units of nM (i.e. nmole/L) to
kg/m. This is further converted to a flux using the parameterisation of
:raw-latex:`\citet{NightingaleEA2000}`, based on wind speed from the
meteorological data. The flux calculation is carried out in the
``SOURCE`` or ``emis4chem`` routine.

SO\ :math:`_4` scattering and absorption
----------------------------------------

Sulphate aerosols have optical properties that can be taken into account
when calculating photochemical reaction rates.

Particles are assumed to be of log-normal size distribution, with dry
radius of 0.05\ :math:`\mu`\ m and :math:`\sigma=2`, and will grow due
to relative humidity according to :raw-latex:`\citet{Fitzgerald1975}`.

Optical properties are found in *FJX\_UMaer.dat*, in the entry
``GM_SO4``. Refractive indices used for calculating these data are for
ammonium sulphate according to :raw-latex:`\citet{ToonEA1976}`, and the
value at 200nm is set to be equal to 300nm, in lack of measurements
below 300nm.

Black carbon and organic matter
===============================

The black carbon and organic matter (BC/OM) application
:raw-latex:`\citep{BerntsenEA2006}` is turned on by setting ``BCOC :=Y``
in the user specified section of *Makefile*.

This application is a stand-alone part of the Oslo CTM3 and needs the
14 tracers listed in Table [table:bcomcomp].

+-------+-------------+--------------------------------+
| Nr    | Component   | Remarks                        |
+=======+=============+================================+
| 230   | omBB1fob    | Insoluble OM biomass burn 1.   |
+-------+-------------+--------------------------------+
| 231   | omBB1fil    | Soluble OM biomass burn 1.     |
+-------+-------------+--------------------------------+
| 232   | omFF1fob    | Insoluble OM fossil fuel 1.    |
+-------+-------------+--------------------------------+
| 233   | omFF1fil    | Soluble OM fossil fuel 1.      |
+-------+-------------+--------------------------------+
| 234   | omBF1fob    | Insoluble OM biofuel 1.        |
+-------+-------------+--------------------------------+
| 235   | omBF1fil    | Soluble OM biofuel 1.          |
+-------+-------------+--------------------------------+
| 236   | omOCNfob    | Insoluble OM from ocean.       |
+-------+-------------+--------------------------------+
| 237   | omOCNfil    | Soluble OM from ocean.         |
+-------+-------------+--------------------------------+
| 240   | bcBB1fob    | Insoluble BC biomass burn 1.   |
+-------+-------------+--------------------------------+
| 241   | bcBB1fil    | Soluble BC biomass burn 1.     |
+-------+-------------+--------------------------------+
| 242   | bcFF1fob    | Insoluble BC fossil fuel 1.    |
+-------+-------------+--------------------------------+
| 243   | bcFF1fil    | Soluble BC fossil fuel 1.      |
+-------+-------------+--------------------------------+
| 244   | bcBF1fob    | Insoluble BC biofuel 1.        |
+-------+-------------+--------------------------------+
| 245   | bcBF1fil    | Soluble BC biofuel 1.          |
+-------+-------------+--------------------------------+

Table: The components of the black carbon (BC) and organic matter (OM)
application. All are transported. Wet scavenging is done for hydrophilic
BC/OM, but also for 20% of hydrophobic BC aerosols on large scale ice
precipitation. Most are numbered ’1’ to allow for more size bins or
particle types.

Both water-soluble and insoluble species are emitted from different
sources into the atmosphere (Section [sxn:bcoc\_emis]). The species are
subject for deposition on the Earth surface (dry deposition,
Section [sxn:bcoc\_dep]), and water soluble particles are also subject
to wash-out.

Further, the insoluble species are transformed to soluble species after
given *aging times*. The aging times are dependent on latitude and
season, based on a simulation with the M7 application in the
Oslo CTM2 :raw-latex:`\citep{SkeieEA2011,LundBerntsen2012}`. These are
read from file in ``bcoc_chetinit`` in *bcoc\_oslo.f90* , and stored in
the array ``CHET``. Input data is T42, which is interpolated to the
model resolution.

BC can be deposited on snow, thereby changing the albedo of snow.
A module for calculating BC on snow
:raw-latex:`\citep{RypdalEA2009, SkeieEA2011}` is included in
Oslo CTM3 (BCsnow, Section [sxn:bcsnow]).

BC/OM emissions
---------------

The main sources of black carbon and organic matter comes from:

Fossil fuel emissions

Biofuel emissions

Biomass burning

In addition, organic matter is emitted from the ocean, which is
explained below.

Several datasets are available for the non-oceanic anthropogenic
emissions, such as :raw-latex:`\citet{BondEA2007}`. However, other
inventories have recently become available, e.g. from the project
ECLIPSE. Traditionally, fossil fuel and biofuel have been combined to
one dataset, although recently it has become necessary to separate them.

Biomass burning is taken from Global Fire Emissions Database, which are
distributed vertically according to the RETRO vertical distribution.

Organic matter released from the ocean follows
:raw-latex:`\citet{GanttEA2015}`, and is calculated in the file
*emissions\_ocean.f90*.

| **Important 1**
| The is method is based on sea spray production, but the Oslo CTM3 sea
  spray function is not necessarily the same as in
  :raw-latex:`\citet{GanttEA2015}`. You should make sure the
  Oslo CTM3 does what you want it to.

If the SALT module (Section [sxn:salt]) is included, sea spray is
retrieved from that module. If SALT is not included, sea salt production
is calculated separately.

In the *emissions\_ocean.f90* file, you can specify which sea spray
scheme to use (parameter ``SeaSaltScheme``; it may not be the same as
used by the SALT module (it has a parameter with the same name).

However, the actual schemes are listed in
Section [sxn:salt\_production], and can be found in the file
*seasaltprod.f90*.

| **Important 2**
| Note that when using ECLIPSE emissions, you have to turn off
  agricultural waste burning (AWB) in GFED. This is done by hard-coding
  the flags ``partitions`` in the GFED read-in routine
  *emisutils\_oslo.f90*.

BC/OM deposition
----------------

All BC/OM tracers are dry deposited at the surface. The dry deposition
velocities are simple; they consist of three constant values for each
component, and applies for sea, land and ice. The land fraction controls
when to use land or sea, while snow cover and ice cover (also sea ice)
controls when to use deposition on ice. Generally the deposition rate is
0.025cm/s, except for hydrophilic BC/OM over water, which is assumed to
be 0.2cm/s.

As for all Oslo CTM3 dry deposition velocities, these are modified due
to the stability of the meteorological conditions. When deposited on the
ground, the particles are lost to the model. However, the BCsnow module
(if included) will keep track of the amount of BC deposited on snow
(Section [sxn:bcsnow]).

The hydrophilic aerosols are also washed out by rain, as will be
described next.

BC/OM wet scavenging
--------------------

BC and OM aerosols are scavenged by rain (Section [sxn:wetscav]), and as
most aerosols they are assumed to be dissolved completely – i.e. mass
limited removal. However, the removal on large scale ice precipitation
is reduced, assuming that 20% of the grid box is available for
scavenging. The 20% is somewhat arbitrary, but stems from the fact that
these aerosols are scavenged by collision processes in rain, but in ice
they are removed mainly by acting as ice nuclei
:raw-latex:`\citep{BrowseEA2012}`. As will be explained below, we also
apply the 20% on large scale ice for hydrophobic BC/OC, and hydrophobic
BC is also fully subject to convective wet scavenging.

Although :raw-latex:`\citet{BrowseEA2012}` suggested there should be no
large scale wet scavenging for temperatures below 258K, we also assume
that 20% of the grid box aerosols can be subject for large scale ice
scavenging. Convective scavenging is assumed to be rain, having no such
limit.

Most models seem to overestimate the stratospheric amount of BC,
suggesting that tropospheric loss mechanisms are missing. Stratospheric
loss processes are not very effective, covering gravitational settling
and coagulation to mixed aerosols (e.g. together with sulphate).

As a possible tropospheric loss mechanism it has been suggested that
hydrophobic BC is probably activated more quickly in convective rain,
because of the large mixing going on within convective plumes. In
principle there are two ways of parameterising this; either by
calculating the aging time on-line, or to scavenge hydrophobic BC
directly in convective rain.

We have chosen the second option, and we assume the wet scavenging to be
similar to hydrophilic BC (but not for OM). The first option would
require a microphysical parameterisation, and could in principle be
taken from the M7 application not yet included in Oslo CTM3. Another
possibility is to build a simpler scheme based on available model
variables and tracers, such as sulphate and temperature. We have not yet
looked into this.

BC/OM scattering and absorption
-------------------------------

Black carbon (soot) and organic matter have some optical properties that
can be taken into account. Properties depend on whether the aerosols are
of fossil fuel origin (FF), biofuel (BF), or comes from biomass burning
(BB).

| BCFF
| Dry aerosols, log-normal size distribution with radius
  0.012\ :math:`\mu`\ m, standard deviation :math:`\sigma=2`. Refractive
  indices are from soot :raw-latex:`\citep{WCP112-86}`. Hydrophilic BCFF
  is assumed to have 50% larger extinction coefficient, giving two
  entries in *FJX\_scat.dat*: ``31 GM_BCFF_PHOB`` and
  ``32 GM_BCFF_PHIL``.

| BCBB and OCBB
| Dry aerosols, bi-log-normal size distribution, with radii
  :math:`r_1=0.08` and :math:`r_2=0.16`\ :math:`\mu`\ m, standard
  deviations :math:`\sigma_1=1.5` and :math:`\sigma_1=1.25`, and
  :math:`\Gamma=0.25/0.75`. Refractive indices are from the SAFARI
  campaign :raw-latex:`\citep{HaywoodEA2003b, MyhreEA2003a}`. Entry in
  *FJX\_scat.dat* is ``33 GM_BCOCBB``

| OCFF
| Dependent on relative humidity. Log-normal size distribution with
  radius 0.5\ :math:`\mu`\ m, and :math:`\sigma=2`. Refractive indices
  are from ammonium sulphate :raw-latex:`\citep{ToonEA1976}`, and the
  value at 200nm is set to be equal to 300nm, in lack of measurements
  below 300nm. Growth due to humidity follows
  :raw-latex:`\citet{PengEA2001}`. Entry in *FJX\_UMaer.dat* is labeled
  ``GM_OCFF``.

BC on snow – BCsnow
-------------------

The BCsnow module was first introduced by
:raw-latex:`\citet{RypdalEA2009}` and later used by
:raw-latex:`\citet{SkeieEA2011}`. It diagnoses the amount of BC
deposited on snow; it does not allow the deposited BC to get back into
the atmosphere. When snow evaporates or melts, the BC is assumed to
deposit on the ground and is thereby lost. Technically, it is a simple
parameterisation for generating snow layers based on meteorological
data, using snowfall and dry deposition values to account for the BC.

You turn this on by setting the logical parameter ``LBCsnow=.true.`` in
*bcoc\_oslo.f90*.

| **How it works**
| There are two processes depositing BC on snow: dry deposition and wet
  scavenging. During each operator split time step, dry deposition is
  diagnosed by the BCOM master routine (variables ``bcsnow_dd_ffc``,
  ``bcsnow_dd_bfc`` and ``bcsnow_dd_bio``), while a separate routine in
   diagnoses wet scavenging based on the private arrays ``BTT`` and
  ``BTTBCK`` (variables ``bcsnow_prec_ffc``, ``bcsnow_prec_bfc`` and
  ``bcsnow_prec_bio``).

The parameter ``ILMM`` defines the maximum number of snow layers
allowed. Snow is built in ``BSNOWL``, while BC layers are built in
``BBCFFC``, ``BBCBFC`` and ``BBCBIO``. These four variables are of size
``(ILMM,IDBLK,JDBLK,MPBLK)``.

When initializing from zero, the first snow layer is fetched from the
meteorological data snow depth, but with maximum of 0.2m water
equivalents (:math:`\approx`\ 2m snow). Next, snow layers are produced
by the amount of snowfall. This process is carried out after wet
scavenging, by the BCsnow master routine ``bcsnow_master``, before a few
other processes/adjustments are taken into account:

bcsnow\_collect\_ij: Collects BC deposited on snow.

bcsnow\_evapmelt\_ij: Calculates evaporation and melting of snow.

bcsnow\_seaice\_ij: Adjustment for sea ice.

bcsnow\_adjustment\_ij: Other adjustments.

| **Collection**
| When BC is collected, it is always assumed that the uppermost layer is
  very thin (1cm of snow, or 0.1cm of water equivalents). This is to
  account for dry deposition being deposited only on the snow layer
  surface. If there is snowfall, both dry deposited and wet scavenged BC
  is first added to the uppermost thin layer. If there has been snowfall
  during the last 24hours, the uppermost layer is merged with the layer
  below, i.e. building a thicker layer. Finally, a new thin layer, with
  the same BC concentration, is made for the next round. Note, however,
  that the new thin layer is formed only if the current top layer is
  thicker than a threshold value, set to be 1.1cm of snow. This is to
  avoid generating extremely thin layers.

| **Evaporation and melting**
| Evaporation and melting is treated as one process (sum of the
  meteorological fields). The levels below the topmost layer will be
  processed first, and when a layer is evaporated (fully or partially),
  its corresponding fraction of BC is merged with the topmost layer. The
  topmost layer is not affected until all layers below have disappeared.

Note that evaporation can be negative, and this is treated as extra
snowfall, adding to the topmost snow layer.

| **Sea-ice treatment**
| The ECMWF meteorological data does not include snow depth, snow melt
  and evaporation on sea ice. We parameterise this as a linear reduction
  of the snow layer (on sea ice coverage larger than 30%) between spring
  and summer. Snow layers are not allowed if sea ice coverage is less
  than 30%.

| **Further adjustments**
| It may be that the snow layers generated from snowfall does not
  correspond fully with the snow depth given in the meteorological data.
  This is adjusted for here, but only for land surface, not for sea.

Secondary organic aerosols
--------------------------

Secondary organic aerosols (SOA) are part of the organic matter, and
when running the stand-alone BCOM code, organic carbon may comprise both
primary organic aerosols (POA or POM), and SOA. If, however, a more
sophisticated SOA scheme is applied (see Section [sxn:soa]), the organic
carbon species of the BCOM module act as POA only.

Mineral dust
============

The Dust Entrainment and Deposition (DEAD) Model
:raw-latex:`\citep{ZenderEA2003}` has been coupled to the Oslo CTM3. You
include it by setting ``DUST =:Y`` in *Makefile*.

Based on wind and surface properties, it calculates the vertical
(upward) flux of mineral dust (i.e. production), and based on observed
size distributions (source modes) of mineral dust, the flux is
distributed onto the model size bins. The DEAD module also calculates
gravitational settling and deposition at the surface, although this can
be implemented with other settling parameterisations. Washout is carried
out in the Oslo CTM3 washout (see Section [sxn:dust\_wdep]). It is,
however, possible to let DEAD take care of washout, but this option is
disabled in Oslo CTM3.

The DEAD code for the Oslo CTM3 is located in the directory
*OSLO*/*DEAD\_COLUMN*, and the DUST master routines connecting it to the
Oslo CTM3 can be found in the master module *dust\_oslo.f90* in *OSLO*.
The original Oslo CTM2 files are located in the directory
*OSLO*/*DEAD\_COLUMN*\ */dead\_ctm2*.

Get DUST running
----------------

The dust application can be run alone with two or more dust tracers. The
number of tracers must be set in *cmn\_size.F90* (``NPAR_DUST``).
Default is 8 tracers, as explained in Section [dust:sizebins].

DUST tracer names must be DUST01, DUST02, etc, and can be placed
anywhere in the tracer list; they do not need to be listed after each
other. But it is wise to declare them in increasing order, representing
increasing size bins: The routines will not consider the numbering, but
assign the first dust tracer to the smallest bin and the last to the
largest bin. A separate array (``dust_trsp_idx``) keeps track of the
transport numbers of the DUST species.

If you need to change the size distribution, see
Section [dust:sizebins].

Dust size bins
--------------

In the standard set-up of Oslo CTM3, mineral dust is calculated using
8 size bins with diameters ranging log-normally from
0.06\ :math:`\mu`\ m to 50\ :math:`\mu`\ m. The diameter size parameters
are set in: ``dmt_min`` and ``dmt_max``.

The dust source is controlled by the observed source modes described
next. In principle, it should not be necessary to change the size range,
and probably not the number of bins. If you want to do changes to the
model size bins, this is done in *cmn\_size.F90* (``NPAR_DUST``). You
also have to change the properties used in fast-JX.

--------------

::

    'DEAD_Mineral_Dust_FudgeFactor_and_Mobilisation_Dataset_Name'
      556    1.4686d-04  HLF   99  9999  NAT   0   -1    0   'bsn_mds_sqr'
      ---   #This section sets mobilisation map name and fudge factor for DUST

--------------

Dust sources
------------

Mineral dust is produced by wind blowing across surfaces where mineral
dust occur. Information on earth surface is read from the DUST input
file *dst\_Txxx.nc*, and the user can select a mobilisation map and
a matching scaling factor. This is described in Section [sxn:dustinput]
for more.

How the production is technically done, is described in
Section [sxn:dusttech].

DEAD can use mean winds or it can use a probability density function for
wind speed. This treatment is discussed in Section [sxn:dustwindspeed].

Input files needed
~~~~~~~~~~~~~~~~~~

DEAD reads time invariant (static) and time variant surface fields from
the file *dst\_Txxx.nc*, where *xxx* should match the native metdata
resolution (only either T159 or T42 are available). The files are
located in the directory *OSLO*/*DEAD\_COLUMN*\ */dustinput*, and you
need to copy the correct file to the directory where you run the model.

Such input files can be generated with the *map* module written by
Charlie, see Appendix [app:dustmap] for more on *map*. Note that this
program has not been compiled and used since around 2005.

| **Mobilisation/erodibility dataset**
| Several fields are read from *dst\_Txxx.nc*, but there is only one
  field where the user can make a choice, namely which mobilisation
  dataset/map to read. This field is also referred as the *erodibility*
  or *erodibility factor*.

There are several erodibility maps available, as listed in the left
column of Table [table:fdg], but not all should be used. The standard
field for Oslo CTM3 should be ``bsn_mds_sqr``, as found by
:raw-latex:`\citet{GriniEA2005}`, but it seems Oslo CTM2 has used
``mbl_bsn_fct`` ever since, which is what Oslo CTM3 has inherited.

It should be noted that in the DEAD code, the erodibility map is called
``mbl_bsn_fct`` (file *dsttidbs.F90*), even though another map is used.
This is somewhat unfortunate, but I didn’t want to change the variable
names in *dst\_Txxx.nc*.

| **Fudge factor**
| In addition, the user much define a global scaling factor (fudge
  factor) that should match the mobilisation dataset. You specify this
  with the ``EFAC`` number in the STV section of
  *Ltracer\_emis\_xxxx.inp*, as shown in Table [table:dustemis].

The fudge factor is used to scale dust production up or down, to get
a certain annual total of emissions. The fudge factor is somewhat
resolution dependent, so you need to make sure you use the correct value
(check annual total production). What you specify for ``EFAC`` in the
STV section of *Ltracer\_emis\_xxxx.inp*, is stored in
``flx_mss_fdg_fct0`` in *dstcst.F90*, and used later in *dstmbl.F90*.

Note that the fudge factors are not well tested in Oslo CTM3, and if
’N/A’ is listed in Table [table:fdg], you should use it with care.

+------------------------------+---------------------------------------------+
| Mobilisation dataset         | fudge factor                                |
+==============================+=============================================+
| bsn\_mds\_sqr (pdf winds)    | 1.4686\ :math:`\times`\ 10\ :math:`^{-4}`   |
+------------------------------+---------------------------------------------+
| bsn\_mds\_lnr (pdf winds)    | 1.0780\ :math:`\times`\ 10\ :math:`^{-4}`   |
+------------------------------+---------------------------------------------+
| bsn\_mds\_lnr (mean winds)   | 2.5046\ :math:`\times`\ 10\ :math:`^{-4}`   |
+------------------------------+---------------------------------------------+
| mbl\_bsn\_fct (pdf winds)    | 2.1994\ :math:`\times`\ 10\ :math:`^{-4}`   |
+------------------------------+---------------------------------------------+
| area\_acm\_fct (pdf winds)   | N/A                                         |
+------------------------------+---------------------------------------------+

Table: Mobilisation dataset and fudge factors: Your choices must be set
in *Ltracer\_emis\_xxxx.inp*, and it is important that the fudge factor
matches the mobilisation dataset. The factors were made for Oslo CTM2,
and are to some extent resolution dependent, so you have to check the
annual total production to find the correct factor for your runs.

Technical
~~~~~~~~~

DEAD calculates first the horizontal flux of mineral dust, which is not
what is transported horizontally in the model, but the basis for
calculating the vertical flux, i.e. the mobilisation of dust. From
observed source size modes of dust, the vertical flux is next
partitioned into the model size bins, using the overlap fraction
``ovr_src_snk_frc``, i.e. the fraction of each source mode going into
each model size bin.

There are currently 3 source modes (``dst_src_nbr`` in *dstgrd.F90*),
given by parameters in the routine ``dst_psd_src_ini`` in *dstpsd.F90*:

``mss_frc_srcx``: Mass fraction.

``dmt_vma_srcx``: Mass median diameter [m].

``gsd_anl_srcx``: Geometric standard deviation.

These are the parameters you need to change if you want a different size
distribution of the emissions.

The overlap fraction is thus a matrix of size (``dst_src_nbr,DST_NBR``),
giving the fraction of size distribution \ :math:`i` in source modes
that overlap with model bin \ :math:`j`.

There are two methods available for calculating the fluxes:

Horizontal flux from :raw-latex:`\citet{White1979}` in subroutine
``flx_mss_hrz_slt_ttl_Whi79_get`` in *dstmblutl.F90*. Vertical flux from
:raw-latex:`\citet{MarticorenaBergametti1995}` in routine
``flx_mss_vrt_dst_ttl_MaB95_get`` (same file).

Follows :raw-latex:`\citet{AlfaroGomes2001}`. **Should be checked
against a more recent version of DEAD before use.**

Option 1 is default, and to use Option 2 you have to include the
appropriate DUST token (``AlG01``) in *Makefile*.

Depending on your bins and distributions, the model bins may or may not
cover all sizes given by the defined source bins. If the :math:`i`-th
source is completely bracketed by model bins, then

.. math:: \sum_{j=1}^{j=\mathrm{dst\_nbr}} \mathrm{ovr\_src\_snk\_frc}(i,j) = 1

 and similarly, if the :math:`j`-th model bin completely brackets the
sources, then:

.. math:: \sum_{i=1}^{i=\mathrm{dst\_src\_nbr}} \mathrm{ovr\_src\_snk\_frc}(i,j) = 1

 The part of the sources bins being smaller or larger than the model
size bin range will not be accounted for.

Note that inputs for calculating the overlap fraction are generic median
diameters of log-normal distributions. Thus, if the routine is called
with number median diameters it will compute the overlap factors for
number concentration, and if it is called with mass median diameters
then it will compute the overlap factors for mass.

For the default Option 1, this is done in ``ovr_src_snk_frc_get`` in
*psdlgn.F90*, called from ``dst_psd_ini`` in *dstpsd.F90*. This fraction
is converted to mass (``ovr_src_snk_mss``). If you use Option 2, the
overlap (``ovr_src_snk_mss``) is updated each time step in ``dst_mbl``
in *dstmbl.F90*.

| **Scaling the fluxes**
| After horizontal fluxes are calculated, they are scaled linearly with:

Bare ground fraction.

Erodibility map/mobilisation factor.

Fudge factor.

Wind speed variability
~~~~~~~~~~~~~~~~~~~~~~

Dust is formed due to wind at the surface, and although the grid box
wind is given from the meteorological data, the actual wind will vary
around the mean wind. Because the dust production is sensitive to the
wind speed, the inclusion of wind speed variability may be important. By
default we apply a variability of 5 steps (parameter ``wnd_mdp_nbr`` in
*dstmbl.F90*), so that the calculation of dust production (i.e. the flux
from the surface) is done for 5 wind speeds; the mean wind, two lower
and two higher wind speeds, before being weighted properly to form the
total dust flux.

For a grid value of 10m/s and ``wnd_mdp_nbr=5``, the dust flux will be
calculated for winds of approximately 6, 8, 10, 12 and 14m/s, and then
weighted to one flux.

You can turn this off by setting the variable ``wnd_mdp_nbr=1`` in
*dstmbl.F90*. It is also possible to increase this number.

Dust sinks
----------

Mineral dust can be removed by precipitation and by gravitational
settling.

Gravitational settling
~~~~~~~~~~~~~~~~~~~~~~

Currently, DEAD handles the gravitational settling, but this should
probably be revised when a second order moment routine is available. The
fall velocity is calculated from Stokes theory, and at the surface the
velocity is adjusted due to turbulent mixing.

Wet scavenging
~~~~~~~~~~~~~~

Dust tracers are washed out as regular tracers, following the parameters
listed in the file *scavenging\_wet.dat*. All dust tracers are assumed
to be soluble, as they generally are good cloud condensation nuclei.
They are also removed by ice.

If you include more DUST tracers, make sure they are defined to be wet
scavenged in the file *scavenging\_wet.dat*.

DUST scattering and absorption
------------------------------

Mineral dust have optical properties that can be taken into account when
calculating photochemical reaction rates.

The dust bins (8 by default) are assumed to logarithmically span the
diameter range of 0.06\ :math:`\mu`\ m to 50\ :math:`\mu`\ m, so that
the size of aerosols in each bin are monodisperse (no variation of
diameter in each bin). However, within each bin, the mass weighted
diameter is calculated, based on the bin sizes and mass median diameter
(``dmt_vma`` in routine ``dst_psd_ini`` in *dstpsd.F90*) and standard
deviation of the distribution (``gsd_anl``, same location). Default
value for ``dmt_vma`` is 2.524\ :math:`\mu`\ m, with a standard
deviation ``gsd_anl`` of 2. These numbers are supposedly taken from
:raw-latex:`\citet{Shettle1984}`, but I could not manage to find it.
Zender has probably a copy if you need it.

Particle density is assumed to be 2.6g/cm\ :math:`^3`, and refractive
indices are taken from the SHADE campaign
:raw-latex:`\citep{HaywoodEA2003a, MyhreEA2003b}`.

Mineral dust does not swell with increased humidity, so the scattering
and absorbing properties are listed in the file *FJX\_scat.dat*, having
entries ``GM_DUST1`` to ``GM_DUST8``.

| *Important*
| If you change either the size of the bins, or the mass median diameter
  (``dmt_vma``) or standard deviation (``gsd_anl_dfl``), you have to
  re-compute the table values for ``GM_DUST`` in *FJX\_scat.dat*.

Things to watch out for
-----------------------

Oslo CTM3 and DEAD are since 2015 fully consistent in resolution
parameters.

Note that DEAD has level 1 on the top (i.e. the top of the atmosphere
has index 1). The transformation is taken care of in the subroutines in
*dust\_oslo.f90*.

In the DEAD file *blmutl.F90* two fixes have been added. The first is to
make sure the displacement height is never higher than 70% of the
midpoint height. The second is to make sure that roughness height is
never higher than 70cm, which seemed to give problems in iterations for
some grid points at some specific times (that is: the energy budget for
the first layer did not converge).

Dust budgets
------------

The dust module outputs a file called *ctm3dstbudget.nc* which is the
budget for dust. The file contains 5 temporally averaged fields, which
are average fluxes (kg/m:math:`^2`/s) of dust for different processes,
and one average for the burden (kg/m:math:`^2`):

Convective wet deposition [kg/m:math:`^2`/s]

Large scale wet deposition [kg/m:math:`^2`/s]

Dry deposition [kg/m:math:`^2`/s]

Production [kg/m:math:`^2`/s]

Total burden [kg/m:math:`^2`]

The time span for the average is defined as for the SALT module; using
the core budget tendencies calendar (“BUDGET tendencies calendar” flags
in *LxxCTM.inp*). Also grid area is put out.

By default, the budget terms are summed up over all dust tracers, but
you can also put out budget terms for each of the tracers. The latter is
done by setting ``LDUSTDIAG3D=.true`` in *dust\_oslo.f90*.

3D averages of dust mass is put out as usual by the core diagnostics.
For dust, it makes more sense to talk about mass mixing ratio (kg/kg)
since the dust is not molecules as such.

Additional notes
----------------

The original DEAD model available from Zender was coded to be
parallelized over latitudes, and had to be re-structured for the column
treatment in Oslo CTM3; to run the original DEAD model as a column model
(with parameters ``PLON=``\ ``PLAT=1``) does not fit the treatment of
input data for a global model. So the code was re-organized to yield
a purely column treatment.

| **Important**
| If you include more DUST tracers, make sure they are defined to be wet
  scavenged in the file *scavenging\_wet.dat*.

The DEAD version implemented is 1.3.2. There are, however, newer
versions available; check the web site [6]_ for information. If an
update is needed, check the new version against the code we use. Be
certain that you keep the column treatment of the Oslo CTM3; if
necessary, you can compare the new version to the old code in
*OSLO*/*DEAD\_COLUMN*\ */dead\_ctm2*.

Into the future
---------------

The DEAD model included should be updated to a more recent version.

Sea salt
========

The sea salt application was introduced by
:raw-latex:`\citet{GriniEA2002}`. It is an independent application which
can be included in any transport model.

In the Oslo CTM3, the salt code is located in the file *seasalt.f90*. It
is primarily based on :raw-latex:`\citet{Fitzgerald1975}`.

Sea salt production, however, is located in a separate module
*seasaltprod.f90*, because it is used to calculate emissions of organic
matter from ocean.

Sea salt production
-------------------

The sea salt production, or flux, is found in the array
``seasalt_flux``, and is of size ``(NPAR_SALT, IDBLK, JDBLK, MPBLK)``
and has units kg/s.

The flux is calculated from the winds, and hence carried out every
meteorological time step, by the subroutine ``seasalt_emis``. Several
methods are available, set by ``SeaSaltScheme`` in *seasalt.f90*:

:raw-latex:`\citet{MonahanEA1986}` for small particles, as suggested by
:raw-latex:`\citet{GongEA1997}`, and :raw-latex:`\citet{SmithEA1993}`
for large particles. This is the default heritage from Oslo CTM2.

:raw-latex:`\citet{MartenssonEA2003}`.

:raw-latex:`\citet{GanttEA2015}`, i.e. :raw-latex:`\citet{Gong2003}`
with sea surface temperature adjustment as in
:raw-latex:`\citet{JaegleEA2011}`.

:raw-latex:`\citet{WitekEA2016}`, i.e. :raw-latex:`\citet{SofievEA2011}`
without salinity effects, using sea surface temperature adjustment as in
:raw-latex:`\citet{JaegleEA2011}`.

Seasalt flux is used to calculate emissions of organic matter from
ocean, in *emissions\_ocean.f90*. Also see Section [sxn:bcoc\_emis] for
some more info.

Technical information
---------------------

The salt aerosols are divided into bins, and there is one tracer field
for each bin. The tracer names are SALT01, SALT02, etc. They do not need
to be listed after each other, but should be (it is wise to keep the
tracers close to each other in the transport array). The tracers should
be listed in increasing order (01, 02, 03, ...) since the size bins will
be assigned from small to large, using the names encountered in the
tracer list.

The QSSA solver is used to integrate sea salt (for each bin). The
production terms are surface emissions (only in the lowermost layer) and
gravitational settling from the layer above (except for the top layer).
The loss term includes gravitational settling to layers below (all
layers except lowest layer) and dry deposition (lowest layer). Hence
surface layer dry deposition differs from gravitational settling.

So far the emissions and dry deposition are treated as production and
loss terms in the QSSA solver. *This may be non-compatible with the UCI
emission treatment, and may need to be revised.* Water makes the sea
salt particles grow to larger sizes. However, this water is not
transported. The growth is calculated locally each time step, depending
on the local relative humidity, and the information on how large the
aerosols are are used to adjust their falling velocity and their dry
deposition velocity.

Wet scavenging
~~~~~~~~~~~~~~

Sea salt aerosols are assumed to be absolutely soluble and are removed
in the convective and stratiform precipitation processes in the
Oslo CTM3. Wet scavenging parameters are set in the file
*scavenging\_wet.dat*.

| **Important**
| If you include more SALT tracers, make sure they are defined to be wet
  scavenged in the file *scavenging\_wet.dat*.

SALT scattering and absorption
------------------------------

Sea salt have optical properties that can be taken into account when
calculating photochemical reaction rates.

For dry aerosols, the 8 size bins are assumed to logarithmically span
the diameter range of 0.03\ :math:`\mu`\ m to 25\ :math:`\mu`\ m, so
that the size of aerosols in each bin are monodisperse (no variation of
diameter in each bin).

Salt particles will grow due to relative humidity, and optical
properties are found in *FJX\_UMaer.dat*, where the entries are
``GM_SALT1`` to ``GM_SALT8``. The growth follows
:raw-latex:`\citet{Fitzgerald1975}`, and refractive indices used for
calculating these data are taken from
:raw-latex:`\citet{ShettleFenn1979}` and :raw-latex:`\citet{hitran2k}`.

Salt budgets
------------

The SALT module produces averages of the salt budget, on the same days
as the core budget tendencies does; it uses the “BUDGET tendencies
calendar” flags in *LxxCTM.inp*. The routine is called from  after core
tendencies have been processed, and the netCDF output file is called
*ctm3sltbudget.nc*.

Several processes are diagnosed:

Production [kg/m:math:`^2`/s]

Dry deposition [kg/m:math:`^2`/s]

Large scale wet deposition [kg/m:math:`^2`/s]

Total burden [kg/m:math:`^2`]

Convective wet deposition [kg/m:math:`^2`/s]

By default, the budget terms are summed up over all sea salt tracers,
but you can also put out budget terms for each of the tracers. The
latter is done by setting ``LSALTDIAG3D=.true`` in *seasalt.f90*.

In the netCDF file you find all the information you need, including grid
area, the time span of accumulation, etc.

Nitrate
=======

Aerosol nitrate can be simulated with the equilibrium module developed
by :raw-latex:`\citet{MetzgerEA2002}`. In the Oslo CTM3 it needs to be
run together with tropospheric chemistry (Section [sxn:tropchem]),
sulphate module (Section [sxn:sulphur]) and the SALT module (Section
[sxn:salt]). These must be turned on in the user section in *Makefile*.

The nitrate master routine is located in the file *nitrate\_oslo.f90*,
and it works on IJ-blocks, calling the Metzger program as a column
model.

Note that the nitrate module is not used in the stratosphere. When
stratospheric chemistry is included, the nitrate module is applied up to
the tropopause height (``maxval(LMTROP(I,J))``). When stratospheric
chemistry is turned off, nitrate is calculated up to the maximum
tropopause height of the latitude band (``maxval(LMTROP(:,J))``) plus
a few layers (``LVS2ADD2TC``).

In the stratosphere nitrate species are converted to HNO\ :math:`_3`,
and therefore also added to NOy. This stratospheric loss was not
included in CTM2.

The Metzger program takes into account sulphate, total HNO\ :math:`_3`
(i.e. HNO:math:`_3`\ (gas) + NO\ :math:`_3`\ (aerosol)), total
NH\ :math:`_3` (i.e. NH:math:`_3`\ (gas) + NH\ :math:`_4`\ (aerosol)).
The basic principles for gas/aerosol partitioning in Metzger’s program
are:

All sulphate exists as aerosols.

The most stable form of sulphate is Na\ :math:`_2`\ SO\ :math:`_4`.

The next most stable form of sulphate is
(NH:math:`_4`)\ :math:`_2`\ SO\ :math:`_4`.

The most stable for of aerosol ammonium is
(NH:math:`_4`)\ :math:`_2`)SO\ :math:`_4`.

Nitrate can exist in aerosols if neutralized by NH\ :math:`_3`.

Nitrate only exists in aerosols if it is cold (due to the Clausius
Clapeyron equation).

Nitrate physics – modes
-----------------------

In nature, condensation/evaporation drives the partitioning of
HNO\ :math:`_3`. In the equilibrium-module, however, there is no way to
follow the path from gas to aerosol. What is found is the partitioning
with lowest Gibbs energy of the mixture we put in.

The equilibrium routine will predict Na\ :math:`_2`\ SO\ :math:`_4`
aerosols if both Na\ :math:`^+` and SO\ :math:`_4^{2-}` are in the
mixture. However, since both SO\ :math:`_4^{2-}` and Na\ :math:`^+` are
non-volatile, given a mixture of NaCl, H\ :math:`_2`\ SO\ :math:`_4`,
HNO\ :math:`_3` and NH\ :math:`_3` the equilibrium model could predict
aerosol Na\ :math:`_2`\ SO\ :math:`_4` and otherwise gaseous components.

However, if sulphates are small aerosols (accumulation mode) and NaCl
are large aerosols (coarse mode), the only way Na can mix with
SO\ :math:`_4` is through coagulation and not through condensation.

To treat this right, we make the (simple) assumption that sulphate
exists as a “fine” mode, and sea salt as a “coarse” mode. We calculate
chemical equilibrium for both modes.

For the first mode, we calculate chemical equilibrium for the mixture of
NH\ :math:`_3`, HNO\ :math:`_3` and H\ :math:`_2`\ SO\ :math:`_4`, which
will normally predict aerosol
(NH:math:`_4`)\ :math:`_2`\ SO\ :math:`_4`. If any excess NH\ :math:`_3`
is present and if it is cold enough, aerosol
NH\ :math:`_4`\ NO\ :math:`_3` will be predicted.

For the coarse mode, equilibrium between the leftover from the first
equilibrium (HNO:math:`_3` and NH\ :math:`_3`) and NaCl is calculated.
Normally this will transfer a large part of HNO\ :math:`_3` to the
aerosol phase, creating NaNO\ :math:`_3`. The “small” mode should have
the chance to reach equilibrium first; because of their higher surface
to mass ratio, they reach equilibrium much quicker than the larger ones.

To make things simple, aerosols are assumed meta-stable in the
Oslo CTM3, so that we don’t need to take into account particle history.

Nitrate tracers
---------------

As noted, the nitrate application depends on tropospheric chemistry,
sulphate and sea salt applications. The new tracers needed for this
simulation are given in Table [table:nitrate].

+------+------------------+---------------------------------+---------------+-------------+------------+
| Nr   | Component name   | Remarks                         | Approximate   | Transport   | Wet dep.   |
+------+------------------+---------------------------------+---------------+-------------+------------+
|      |                  |                                 | lifetime      |             |            |
+------+------------------+---------------------------------+---------------+-------------+------------+
| 61   | NH3              | Gas, emitted                    | weeks         | Yes         | Yes        |
+------+------------------+---------------------------------+---------------+-------------+------------+
| 62   | NH4fine          |                                 | weeks         | Yes         | Yes        |
+------+------------------+---------------------------------+---------------+-------------+------------+
| 63   | NH4coarse        |                                 | days          | Yes         | Yes        |
+------+------------------+---------------------------------+---------------+-------------+------------+
| 64   | NO3fine          |                                 | weeks         | Yes         | Yes        |
+------+------------------+---------------------------------+---------------+-------------+------------+
| 65   | NO3coarse        |                                 | days          | Yes         | Yes        |
+------+------------------+---------------------------------+---------------+-------------+------------+
| –    | H2Ofine          | Aerosol water adjusts rapidly   | short         | —           | —          |
+------+------------------+---------------------------------+---------------+-------------+------------+
|      |                  | to ambient RH                   |               |             |            |
+------+------------------+---------------------------------+---------------+-------------+------------+
| –    | H2Ocoarse        |                                 | short         | —           | —          |
+------+------------------+---------------------------------+---------------+-------------+------------+

Wet scavenging of NH\ :math:`_3` is based on Henry’s law, assumed not to
be taken up in ice, while the aerosols are assumed to be mass limited
uptake (see Section [sxn:ls\_scav]).

H2Ofine and H2Ocoarse are not used in the Oslo CTM3, since
H\ :math:`_2`\ O is not transported. They are only used as diagnostics,
and are therefore left out until transport is necessary; they are
calculated privately in the nitrate module *nitrate\_oslo.f90*.

Sea salt in the nitrate model
-----------------------------

Sea salt is crucial to the nitrate application. The stand-alone sea salt
application is treated in mass space and does not need a specific molar
mass. However, when nitrate is included it is important that you put the
correct molecular mass to the sea salt tracers (which is 58g/mol),
because the equilibrium model evaluates how much of the Cl in NaCl which
can be replaced with NO\ :math:`_3`.

Note that after this has been evaluated, sea salt is transported as NaCl
again. This might seem inconsistent since strictly speaking, they may
contain some NaNO\ :math:`_3`. However, this is not important since
NaNO\ :math:`_3` is more stable than NaCl and if we have
NO\ :math:`_3^-` available it will replace the Cl\ :math:`^-` in the
next time step also. The reaction happening is:

However, the fact that we set [Cl:math:`^-`] = [Na:math:`^+`] as input
to the equilibrium calculations does not really change the output since
HCl is the first gas to evaporate anyway. If HNO\ :math:`_3` is
available, it will replace Cl\ :math:`^-` until [NO:math:`_3^-`] =
[Na:math:`^+`]. Thus the important thing is to keep track of
Na\ :math:`^+`, which we do in the sea salt tracers.

Nitrate emissions
-----------------

The nitrate application needs emissions of NH\ :math:`_3`. Different
datasets are available, e.g. GEIA :raw-latex:`\citep{BouwmanEA1997}`
which has traditionally been used in Oslo CTM2. However, newer sets are
also available, e.g. from :raw-latex:`\citet{LamarqueEA2010}`. While the
latter covers only anthropogenic emissions, the GEIA data covers both
anthropogenic and natural emissions, given on 1x1 resolution on an
annual basis. Following :raw-latex:`\citet{AdamsEA1999}`, we impose
a monthly variation on three datasets by weighting emissions by the
number of daylight hours during the year: Domestic animals, fertilizers,
and crops sources.

The GEIA dataset is rather old, so it should probably be updated.

The Oslo CTM2 emissions were summed up in one file, which made them less
flexible. Therefore, the format is updated, and the GEIA datasets are
available on a netCDF file, scaled to kg(NH\ :math:`_3`)/s for
12 months. Three of them are scaled with sunlit hours as described
above.

All original files are available in
*/div/amoc/d4-4/oslo-ctm/archive\_ctm/*\ *input\_files/*, in the
subdirectory *emissions/NH3\_GEIA*. Here you also find the program
*sumdata\_as.f*, which does the scalings (the original file scaled all
datasets with sunlit hours). While producing monthly totals in separate
files, it also produces a file that the IDL program
*make\_geia\_netcdf.pro* uses to make the netCDF file.

Emissions files can be found in the emission directories, and must be
included in the emission list *Ltracer\_emis\_xxxx.inp*.

Remaining problems
------------------

The dry deposition of NH\ :math:`_3` is set to some numbers found in an
old paper by :raw-latex:`\citet{SortebergHov1996}`. There seems to be
agreement that drydep velocities of NH\ :math:`_3` should exceed
velocities of NH\ :math:`_4^+`, but it is unclear by how much. The new
dry deposition scheme (Section [sxn:drydep]) may improve this.

Based on the first nitrate studies with Oslo CTM2, it seems that the
amount of NO\ :math:`_3` fine aerosol is heavily dependent on how
efficiently NH\ :math:`_3` is mixed up to altitudes where it is cold
enough to form NH\ :math:`_4`\ NO\ :math:`_3`. At ground it is usually
quite warm, while at higher altitudes aqueous production of
H\ :math:`_2`\ SO\ :math:`_4` makes aerosols too acidic to form
NH\ :math:`_4`\ NO\ :math:`_3`.

A scientific analysis of the Oslo CTM3 should perhaps include
sensitivity to vertical mixing, since the first CTM2 results seem to
indicate that NH\ :math:`_3`/NH:math:`_4^+` is not mixed to cold enough
layers, and the cold layers thus stay acidic due to high sulphate
production there.

Box version
-----------

A box version of the nitrate code exists in the Oslo CTM3 tool box at
*/div/amoc/d4-4/oslo-ctm/archive\_ctm/*\ *box\_models/*.

M7
==

M7 is not yet implemented, and there is no plan yet to do so.

Secondary organic aerosols (SOA)
================================

Secondary organic aerosols (SOA) was implemented in Oslo CTM2 by
:raw-latex:`\citet{HoyleEA2007}`, and utilised later in
e.g. :raw-latex:`\citet{HoyleEA2009}`.

Several precursor hydrocarbons are included (Section [sxn:soa\_prec]).
The precursors also react within the tropospheric gas phase chemistry,
oxidised by OH, O\ :math:`_3` and NO\ :math:`_3`, forming SOA gas phase
components (Section [sxn:soa\_tracers]). A separate module calculates
SOA from an equilibrium approach, as explained in
:raw-latex:`\citet{HoyleEA2007}`.

Stochiometric coefficients are used for the chemical products, based on
a two-product model :raw-latex:`\citep{HoffmannEA1997}`, and the
partitioning (or separation) between the gas and aerosol phases is
calculated assuming equilibrium and using partitioning coefficients
:raw-latex:`\citep{HoyleEA2007}`.

Importantly, in Oslo CTM3 we do the separation in both the troposphere
and the stratosphere, but the chemical conversion from precursors is
only carried out in the troposphere. Stratospheric separation was not
treated in Oslo CTM2.

SOA precursor tracers
---------------------

The SOA precursor VOCs are listed in Table [table:soapreccomp]. In
addition, the Oslo CTM3 component ``C6HXR`` has been split in three,
namely ``C6HXR_SOA`` for trimethyl-benzenes, ``Benzene`` for benzene and
``Tolmatic`` for toluene and other aromatics.

+-------+--------------+------------------------------------------------+
| Nr    | Component    | Remarks                                        |
+-------+--------------+------------------------------------------------+
+-------+--------------+------------------------------------------------+
| 280   | Apine        | :math:`\alpha`-Pinene                          |
+-------+--------------+------------------------------------------------+
| 281   | Bpine        | :math:`\beta`-Pinene                           |
+-------+--------------+------------------------------------------------+
| 282   | Limon        | Limonene                                       |
+-------+--------------+------------------------------------------------+
| 283   | Myrcene      | Myrcene                                        |
+-------+--------------+------------------------------------------------+
| 284   | Sabine       | Sabinene                                       |
+-------+--------------+------------------------------------------------+
| 285   | D3carene     | :math:`\Delta^3`-Carene                        |
+-------+--------------+------------------------------------------------+
| 286   | Ocimene      | Ocimene                                        |
+-------+--------------+------------------------------------------------+
| 287   | Trpolene     | Terpinolene                                    |
+-------+--------------+------------------------------------------------+
| 288   | Trpinene     | :math:`\alpha`- and :math:`\gamma`-Terpinene   |
+-------+--------------+------------------------------------------------+
| 289   | TrpAlc       | Terpinoid alcohols                             |
+-------+--------------+------------------------------------------------+
| 290   | Sestrp       | Sesquiterpenes                                 |
+-------+--------------+------------------------------------------------+
| 291   | Trp\_Ket     | Terpenoid ketones                              |
+-------+--------------+------------------------------------------------+
| 192   | Benzene      | Benzene                                        |
+-------+--------------+------------------------------------------------+
| 193   | Tolmatic     | Toluene and                                    |
+-------+--------------+------------------------------------------------+
|       |              | other aromatics                                |
+-------+--------------+------------------------------------------------+
| 12    | C6HXR\_SOA   | Trimethyl-benzenes and                         |
+-------+--------------+------------------------------------------------+
|       |              | xylene (replaces C6HXR)                        |
+-------+--------------+------------------------------------------------+

Table: The secondary organic aerosol precursor components. Bottom three
replaces the usual C6HXR in Oslo CTM3.

SOA tracers
-----------

The SOA tracers are several gas components ``SOAGASxy``, where ``x`` is
the class number (1–8), and ``y`` is the oxidising component (1–3,
:math:`O_3`, OH, NO\ :math:`_3`). Similarly, there are aerosol
components ``SOAAERxy``. These components have component IDs between 150
and 191. See :raw-latex:`\citet{HoyleEA2007}` for more information.

SOA sources and sinks
---------------------

The SOA precursors are emitted from the earth surface, using a rather
old dataset, which should be updated. These precursors, and also
isoprene, are emitted assuming emissions only at daytime and scaled to
sunlight, as described in Section [sxn:emissions\_stv].

SOA are produced or lost according to the separation method, which is
based on temperature; i.e. the separation can be either production or
loss.

SOA gases and the precursor gases are assumed to have a stratospheric
lifetime of 1 month, assuming the SOA are converted into species not
treated in the model. Oslo CTM2 used a lifetime of 1 week, which was
considered too short and hence upped in Oslo CTM3. See *strat\_loss.f90*
for more. SOA aerosols are subject to gravitational settling (also
treated in *stratloss\_loss.f90*), and if you run the Oslo CTM3 without
stratospheric chemistry, the 1 month lifetime also applies for the
aerosols.

Physics
=======

Based on the meteorological data, some physical properties may be
calculated. These include tropopause height, equivalent latitude,
potential vorticity, etc. The physics routines are located in the
physics module, which can be found in the file *physics\_oslo.f90* in
the *OSLO* directory. Also see Appendix [app:technicalnotes], where
e.g. airmass, layer thickness and relative humidity is calculated.

Tropopause height
-----------------

In Oslo CTM3, several routines need to distinguish between tropospheric
and stratospheric air. This is typically done by finding the tropopause,
which is 2-dimensional, or it can be done in 3D.

The chemistry is so far set up to use a 2D tropopause to select when to
use the tropospheric module and when to use the stratospheric module.
Some diagnostics such as STE flux uses 3D fields to separate
tropospheric from stratospheric air.

In any case there are several ways to calculate the tropopause. The 3D
representation is explained in Section [sxn:ste], while the 2D
representation is described here. As noted above, the routine can be
found in the file *physics\_oslo.f90*.

What we call the tropopause (2D) is defined as the uppermost layer of
the troposphere, and is therefore named ``LMTROP`` (historically, ``LM``
was often used as parameter instead of ``LPAR``).

PVU based tropopause
~~~~~~~~~~~~~~~~~~~~

First, the tropopause level is not allowed to have potential temperature
higher than 380K :raw-latex:`\citep{HoltonEA1995}`. The uppermost layer
having potential temperature below 380K is called ``L380K``.

Traversing downwards from ``L380K``, the tropopause (top layer of
troposphere) is defined at the level where the PVU (10\ :math:`^6`\ PV)
is lower than 2.5PVU :raw-latex:`\citep{HoltonEA1995}`, but not lower
than 5km (a somewhat arbitrary value, not often encountered). If the
maximum PVU is lower than this limit (typical for low latitudes) the
tropopause is defined at ``L380K``; the upper-most level where potential
temperature is lower than 380K.

Sometimes the minimum (absolute value) PVU in a column can be greater
than 2.5 (e.g. in the polar vortex). In that case, two options remains;
if there exists an altitude of minimum absolute PVU, it is chosen as
tropopause. If PVU decrease monotonically with depth (i.e. downwards),
the tropopause is set to a default minimum height of 5km.

The routine is called ``tp_pvu_ij`` and you set this option using
``TP_TYPE=1`` in *cmn\_oslo.f90*.

Lapse rate based tropopause
~~~~~~~~~~~~~~~~~~~~~~~~~~~

The tropopause can also be found traversing upwards until the level
where the lapse rate (calculated between the current level and the next)

.. math:: -dT/dz = -\frac{T_{L+1} - T_L}{z_{L+1} - z_L}

 goes below 2K/km.

It assumes a minimum tropopause height of 5km, and a maximum height
given by 50hpa (top of grid box must have larger pressure than this).

The routine is called ``tp_dtdz_ij``, and you set this option using
``TP_TYPE=2`` in *cmn\_oslo.f90*.

Lapse rate based on E90 tracer
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

The tropopause defined by the E90 tracer
:raw-latex:`\citep{PratherEA2011}` can be used if the E90 tracer is
included. It is the uppermost level where the 3D ``LSTRATAIR_E90`` is
``.false.`` (i.e. tropospheric air), as given by the variable
``LPAUZTOP``.

The routine is called ``tp_e90_ij``, and you set this option using
``TP_TYPE=3`` in *cmn\_oslo.f90*.

Potential vorticity
-------------------

Potential vorticity (PV) is available in the L60 meteorological data,
but only in a few L40 data sets. If not available, PV will be calculated
from the meteorological data (temperature, wind and pressure). A routine
for this is available in *physics\_oslo.f90*.

Equivalent latitude
-------------------

In the physics module (in the file *physics\_oslo.f90*) we also
calculate equivalent latitude. Equivalent latitude is calculated from
PVU according to :raw-latex:`\citet{NashEA1996}`, based upon the fact
that PV is assumed to be conserved on potential temperature
(:math:`\theta`) surfaces. All necessary parameters and variables are
defined in the physics module.

| **How it is done**
| We define ``NTHE`` :math:`\theta` levels in the parameter
  ``pvthe(NTHE)``, and we interpolate PV onto each of those levels. Then
  we find equivalent latitudes for all horizontal grid boxes for those
  :math:`\theta` surfaces.

The calculation of equivalent latitude is carried out for each
hemisphere separately. Max absolute PV is in the polar vortex, and is
therefore assumed to be the pole in equivalent latitudes. Minimum is at
Equator. The PV span is divided into bins, and the area covering each
binned value of PV is calculated. Depending on the area containing each
PV, each grid box is assigned an equivalent latitude. See the routine
for equations and description.

Remember that equivalent latitude is only useful in the uppermost
troposphere and in the stratosphere, which should be reflected in the
``pvthe`` values.

| **How to use the data**
| For a given grid box, find the potential temperature and interpolate
  equivalent latitude from the closest :math:`\theta` levels in
  ``pvthe``.

Boundary layer height
---------------------

The routine ``set_blh_ij`` sets ``BLH`` from the boundary layer height
at the current and next meteorological time step (``BLH_CUR`` and
``BLH_NEXT``, respectively). This is interpolated linearly in time, and
we interpolate halfway into the time step
:math:`\Delta t_{\textrm{chem2}}` of the CCYC-loop. The weighting
fraction of current and next fields are then

.. math::

   \begin{aligned}
     f_{\textrm{next}} &=& \frac{\textrm{nmetTimeIntegrated}
                   + \frac{\Delta t_{\textrm{chem2}}}{2}}
                          {\Delta t_{\textrm{met}}}\\
     f_{\textrm{cur}} &=& 1 - f_{\textrm{next}}\end{aligned}

 where ``nmetTimeIntegrated`` is the elapsed time of the meteorological
time step, and :math:`\Delta t_{\textrm{met}}` is the meteorological
time step.

For diagnostic output which are done every ``NOPS``, the weighting is
done to get ``BLH`` at each ``NOPS``:

.. math::

   \begin{aligned}
     f_{\textrm{next}} &=& \frac{\texttt{NOPS}-1}{\texttt{NROPSM}}\\
     f_{\textrm{cur}} &=& 1 - f_{\textrm{next}}\end{aligned}

Both ``BLH_CUR`` and ``BLH_NEXT`` needs to be set when reading
meteorological data. If ``BLH_NEXT`` cannot be found, it should be set
to the same as ``BLH_CUR``.

Land surface types
------------------

The Oslo CTM3 needs information about land surface types, also called
land use types or plant functional types (PFT). These are used e.g. for
dry deposition scheme or in the biogenic emission routines.

Prior to the inclusion of online biogenic emissions (see
Section [sxn:emissions\_megan]), a MODIS dataset was used. Now we use
the PFT field from the Community Land Model (CLM), giving vegetation
information for 16 types for all years since 1860. Specifics of the PFTs
are listed in Table [table:pftclm] and Table [table:pftmodis].

The land surface type fractions for a grid point ``I,J`` are stored in
``landSurfTypeFrac(:,I,J)``. You define which year to apply in the input
file, stored in ``LANDUSE_YEAR``. If you set 9999, the meteorological
year will be used. The variable ``LANDUSE_IDX`` sets the dataset you
want, 2 is MODIS and 3 is CLM.

+------+---------------------------------------+
| #    | Name                                  |
+======+=======================================+
| 1    | Needleleaf evergreen temperate tree   |
+------+---------------------------------------+
| 2    | Needleleaf evergreen boreal tree      |
+------+---------------------------------------+
| 3    | Needleleaf deciduous boreal tree      |
+------+---------------------------------------+
| 4    | Broadleaf evergreen tropical tree     |
+------+---------------------------------------+
| 5    | Broadleaf evergreen temperate tree    |
+------+---------------------------------------+
| 6    | Broadleaf deciduous tropical tree     |
+------+---------------------------------------+
| 7    | Broadleaf deciduous temperate tree    |
+------+---------------------------------------+
| 8    | Broadleaf deciduous boreal tree       |
+------+---------------------------------------+
| 9    | Broadleaf evergreen temperate shrub   |
+------+---------------------------------------+
| 10   | Broadleaf deciduous temperate shrub   |
+------+---------------------------------------+
| 11   | Broadleaf deciduous boreal shrub      |
+------+---------------------------------------+
| 12   | Cool/Arctic C3 grass                  |
+------+---------------------------------------+
| 13   | C3 grass (cool)                       |
+------+---------------------------------------+
| 14   | C4 grass (warm)                       |
+------+---------------------------------------+
| 15   | Crop 1 (Corn)                         |
+------+---------------------------------------+
| 16   | Crop 2 (Other crops)                  |
+------+---------------------------------------+
| 17   | Barren land                           |
+------+---------------------------------------+

Table: CLM-PFTs.

+------+--------------------------------------+
| #    | Name                                 |
+======+======================================+
| 1    | Evergreen Needleleaf Forests         |
+------+--------------------------------------+
| 2    | Evergreen Broadleaf Forests          |
+------+--------------------------------------+
| 3    | Deciduous Needleleaf Forests         |
+------+--------------------------------------+
| 4    | Deciduous Broadleaf Forests          |
+------+--------------------------------------+
| 5    | Mixed Forests                        |
+------+--------------------------------------+
| 6    | Closed Shrublands                    |
+------+--------------------------------------+
| 7    | Open Shrublands                      |
+------+--------------------------------------+
| 8    | Woody Savannas                       |
+------+--------------------------------------+
| 9    | Savannas                             |
+------+--------------------------------------+
| 10   | Grasslands                           |
+------+--------------------------------------+
| 11   | Permanent Wetlands                   |
+------+--------------------------------------+
| 12   | Croplands                            |
+------+--------------------------------------+
| 13   | Urban and Built-Up                   |
+------+--------------------------------------+
| 14   | Cropland/Natural Vegetation Mosaic   |
+------+--------------------------------------+
| 15   | Permanent Snow and Ice               |
+------+--------------------------------------+
| 16   | Barren or Sparsely Vegetated         |
+------+--------------------------------------+
| 17   | Unclassified                         |
+------+--------------------------------------+

Table: MODIS-PFTs.

Leaf area index
---------------

The leaf area index (LAI) in Oslo CTM3 is actually green leaf area
index, but we still call it ``LAI``. It is taken from ISLSCP2, FASIR
adjusted, in 0.25 degree resolution, :raw-latex:`\citep{SietseEA2010}`.
The field is interpolated to Oslo CTM3 resolution.

The path to the file is set in the input file, along with the year to
apply, ``LAI_YEAR``. Only year 1982 to 1998 are available, so for
general model studies we use the climatological mean of these, set by
``LAI_YEAR=0000`` If you set ``9999``, the meteorological year will be
used (if available).

Roughness length
----------------

The roughness length (``ZOI``) is taken from ISLSCP2, FASIR adjusted, in
0.25 degree resolution, :raw-latex:`\citep{SietseEA2010}`. The field is
interpolated to Oslo CTM3 resolution.

The path to the file is set in the input file, along with the year to
apply, ``ZOI_YEAR``. Only year 1982 to 1998 are available, so for
general model studies we use the climatological mean of these, set by
``LAI_YEAR=0000`` If you set ``9999``, the meteorological year will be
used (if available).

Diagnostics
===========

In the Oslo CTM3 we try to do all processes in parallel regions (private
arrays), and this also applies for the diagnostics. This is done either
by creating private arrays for diagnostics, which are then put back to
global arrays at the end of the parallel region, or by letting the
parallel region work directly on its part of a global array – in that
case the global array should be set up for IJ-block structure (see
Section [sxn:parallel:ij] for more on that).

Core diagnostics
----------------

The simplest diagnostics is the 3D averages, but the core can also
diagnose each process.

The process diagnostics can be calculated in 2D (zonal means or layers),
for pre-defined boxes, and in addition time series can be produced. The
core diagnostics are controlled by flags in the input file *LxxCTM.inp*.

3D averages
~~~~~~~~~~~

Averages of all species are calculated. The 3D average in the
Oslo CTM2 was monthly averages. In the Oslo CTM3, however, you are more
free to set the temporal span of the average.

In the file *LxxCTM.inp* you can set flags for when to calculate
averages (AVERAGES calendar ``JDO_A``). The shortest time span is one
day, and the time span is the period since the last average calculation.
Setting the flags to ``1`` generates the files, while setting it higher
than ``1`` also prints some data to screen.

Since mid-2017, The file format for averages is netCDF4, and the file
name is *avgsavFROMDATE\_TODATE.nc*, where ``FROMDATE`` and ``TODATE``
are on the form ``YYYYMMDD``. Hourwise, the accumulation starts at 00UTC
on ``FROMDATE``, and runs until 00UTC on ``TODATE`` (00UTC on ``TODATE``
is thus taken into account in the next average).

Note that the program will stop if the avgsav-file already exist. This
is to prevent results from being overwritten.

The plan is that only selected species (listed in input file) will be
written to 3D averages, to save space.

All variables put to file are described in the file, and they are:

The variable dimensions on file:

lat: Latitudinal dimension

lon: Longitudinal dimension

lev: Vertical dimension

ilat: lat+1

ilon: lon+1

ilev: lev+1

tname\_len: Length of character strings in ``TNAME``

date\_size: 6 (year, month, date, hour, minutes, seconds)

The fixed (not average) variables are:

lon: Longitudes at grid box center. Unit is degrees East.

lat: Latitudes at grid box center. Unit is degrees North.

lev: Pressure at mass-weighted grid box center (mean of pressures at
upper and lower edges), assuming surface at 1000hPa. Unit is hPa.

ilon: Longitude at at eastern edges of grid boxes. Unit is degrees East.

ilat: Latitudes at southern edges of grid boxes. Unit is degrees North.

ilev: Pressure at lower edge of grid box, assuming surface at 1000hPa.
Unit is hPa.

ihya: Sigma hybrid coordinate A, called ``ETAA`` in Oslo CTM3. Unit is
hPa.

ihyb: Sigma hybrid coordinate B, called ``ETAB`` in Oslo CTM3. Unitless.

IPARW: Meteorological data native longitudinal resolution.

JPARW: Meteorological data native latitudinal resolution.

LPARW: Meteorological data vertical resolution.

NRAVG: Number of time steps accumulated.

START\_TIME: Start time [YYYY,MM,DD,hh,mm,ss] for accumulating data.

END\_TIME: End time [YYYY,MM,DD,hh,mm,ss] for accumulating data.

START\_DAY: Start day for accumulating data (model counter NDAY).

END\_DAY: End day for accumulating data (model counter NDAY).

VERSION: Output file version number.

NPAR: Number of transported species in simulation.

NOTRPAR: Number of non-transported species in simulation.

tracer\_idx: ID numbers for species used in Oslo CTM3 (``chem_idx`` and
``Xchem_idx``).

tracer\_molweight: Molecular weight for transported species. Unit is
g/mol.

tracer\_name: Tracer/species names.

tracer\_transported: Flag if tracer was transported (1) or not (0).

gridarea: Grid box area. Unit is m\ :math:`^2`.

The averaged variables are:

Psfc: Surface pressure. Unit is hPa.

AIR: Grid box air masses. Unit is kg.

volume: Grid box air volumes. Unit is m\ :math:`^3`.

air\_density: Grid box air density. Unit is molec/cm\ :math:`^3`.

temperature: Grid box temperature: Unit is K.

height: Height of grid box bottom, so it is of size ``LPAR+1``, and it
has reverse order (ilev,lon,lat).

LMTROP: Uppermost model level in troposphere.

H2O\_all: H\ :math:`_2`\ O from metdata :math:`Q`, but overwritten in
the stratosphere by calculated H\ :math:`_2`\ O when available.

Q: Specific humidity retrieved from meteorological data. Unit is
kg(H\ :math:`_2`\ O)/kg(air).

Tracers: All included tracers. Units are kg of species.

Process diagnostics / budget diagnostics
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

All processes are diagnosed. However, when the Oslo treatment of
emissions and deposition is used, those processes will not be diagnosed
well, as they are part of the chemistry. However, they are diagnosed
separately.

Budget diagnostics of different processes are controlled by the BUDGET
tendencies calendar (``JDO_T``) in the input file.

e90 tracer
~~~~~~~~~~

:raw-latex:`\citet{PratherEA2011}` presented a tracer that with given
surface emissions and an e-folding atmospheric lifetime of 90days, will
follow the observed tropopause closely at about 90ppbv.

This is used to set the 3D logical variable ``LSTRATAIR_E90``, where
``.true.`` is stratospheric air. It uses a specific volume mixing ratio
for the E90 tracer, given by ``E90VMR_TP``
:raw-latex:`\citep{PratherEA2011}`.

You can turn this tracer on in *Makefile*.

The e90 tracer is so far used for flux calculations (Section [sxn:ste]),
but could in the future also be used for selecting tropospheric or
stratospheric chemistry scheme, either as a 3D field or by selecting the
uppermost tropospheric grid box as tropopause, similar to the PVU-based
tropopause. Note that the program code is not set up for a 3D
definition.

Stratosphere-Troposphere-Exchange
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

In Oslo CTM3 the stratosphere-troposphere-exchange (STE) calculation
follows :raw-latex:`\citet{HsuEA2005}`, and requires that the e90 tracer
is included (Section [sxn:e90]). STE is calculated based on the
calculated O\ :math:`_3`, with the possibility to also calculate STE for
a Linoz O\ :math:`_3` tracer.

During a certain time period in Oslo CTM3 (e.g. a month), STE of
O\ :math:`_3` is calculated as a residual of column mass budgets between
the model surface and a certain O\ :math:`_3` isopleth
:raw-latex:`\citep{HsuEA2005}`. Below the given isopleth and for the
given time period, this can be written

.. math::

   \left(\frac{dM}{dt}\right)_{tot} =
           \left(\frac{dM}{dt}\right)_{chem} - S + F_{s\rightarrow t} - F_{h,t\rightarrow t}
           \label{eqn:ste}

 where :math:`\left(\frac{dM_t}{dt}\right)_{tot}` is the total change
in O\ :math:`_3`, :math:`\left(\frac{dM_t}{dt}\right)_{chem}` is the
corresponding chemical tendency of O\ :math:`_3` and :math:`S` is sink
processes such as dry deposition at the surface and wet scavenging.
:math:`F_{s\rightarrow t}` is the STE to be inferred (both horizontal
and vertical), and :math:`F_{h,t\rightarrow t}` is the horizontal flux
of O\ :math:`_3` from tropospheric air to tropospheric air (positive out
of the column). The latter is found by comparing the volume mixing ratio
of the flux to the isopleth values, and account for values lower than
the isopleth (i.e. the partly tropospheric air in a gridbox). The
calculation of :math:`F_{h,t\rightarrow t}` is explained at the end of
this Section.

For post-processing, it is also worth noting that
:math:`F_{h,t\rightarrow t}` and
:math:`\left(\frac{dM_t}{dt}\right)_{tot}` may need to be filtered to
reduce noisy plots – see the end of this Section for more on that.

Back to Eq ([eqn:ste]), the left-hand side is thus diagnosed as the
change from the beginning to the end of the time period, while the
right-hand side terms (except :math:`F_{s\rightarrow t}`) are summed up
for all time steps during the time period.

Note that this is the same treatment as used by
:raw-latex:`\citet{StevensonEA2006}`; but instead of separating the
:math:`P` and :math:`L` terms for tropospheric O\ :math:`_3` (which is
somewhat arbitrary and often based on a concept of odd-oxygen) we
diagnose the sum :math:`P-L` as :math:`(dM/dt)_{chem}` in Eq ([eqn:ste])
and the deposition :math:`D` as :math:`S`. Assuming no tropospheric
trend, as in :raw-latex:`\citet{StevensonEA2006}`, means that
:math:`(dM/dt)_{tot} + F_{t\rightarrow t} = 0` in our equation.

The use of the 120ppb O\ :math:`_3` surface for calculating STE may be
problematic in very polluted regions, e.g. in biomass burning areas.
This has been solved by forcing the diagnose to assume all air below 4km
altitude to be tropospheric, and between 30S and 30N air is assumed to
be tropospheric all the way to the tropopause. Similarly, the use of
150ppb may confuse stratospheric O\ :math:`_3`-hole air with
tropospheric air if O\ :math:`_3` is below 150ppb.

A better diagnose is probably to use a surface independent of
O\ :math:`_3`, such as the e90-stratosphere definition, as basis for
Eq ([eqn:ste]). This is included in the CTM3, found to produce
approximately similar STE as the 120ppb O\ :math:`_3` surface
:raw-latex:`\citep{SovdeEA2012}`. Also for this diagnose air below 4km
is assumed tropospheric.

An additional possibility is to use Linoz O\ :math:`_3` tracer for STE
calculations. The Linoz tracer is turned on with a separate option in
*Makefile*, and this tracer needs to be called ``O3LINOZ``. It should be
noted that CTM3 is not set up to use Linoz instead of the stratospheric
chemistry module, only as an STE diagnostic. See Section [sxn:linoz] for
more.

How often STE is to be diagnosed, is set in the input file, by the flags
in ``JDO_X`` calendar. The whole STE budget is written to file
*ste\_YYYYMMDD\_YYYYMMDD.nc*, where the first date is the start time
(00UTC) of STE calculation and the latter date is the end of the
calculation (also at 00UTC).

To summarize, STE can be calculated when stratospheric O\ :math:`_3` is
present, either through CTM3 stratospheric chemistry or by Linoz. Also,
you have to include the e90 tracer in the *Makefile* settings.

| **Calculation of :math:`F_{h,t\rightarrow t}` using e90-tracer**
| This is also explained in detail in the source code (*steflux.f90*).
  This flux is called ``QFU`` and ``QFV`` in Oslo CTM3, and in the
  source code you find them in *steflux.f90* and also in *p-vect3.f*.
  These variables give the tracer mass transported from one grid box to
  the next, and also the first moments For :math:`U` these are
  ``QFU(:,:,1)`` and ``QFU(:,:,2)``, respectively. Similarly,
  ``QFV(:,:,1)`` and ``QFV(:,:,2)``, are for \ :math:`V`).

In the STE diagnose, we need to keep track of the flux from tropospheric
to tropospheric grid boxes, i.e. \ :math:`F_{h,t\rightarrow t}`. To
decide whether the air is tropospheric, stratospheric or both, we apply
the mean flux of the e90 tracer out of the grid box (``Q0F_E90``) and
the first moment (``Q1F_E90``). Figuratively, ``Q0F_E90`` and
``Q1F_E90`` set up a trapezoid, with left top point at

.. math:: h_L = |Q_{0F,E90}| + |Q_{1F,E90}|\label{eq:ste_hL}

 and right top point at

.. math:: h_R = |Q_{0F,E90}| - |Q_{1F,E90}|\label{eq:ste_hR}

 The signs in front of the moments are opposite of the original CHEMFLUX
routine, where the O\ :math:`_3` gradient is used: e90 and O\ :math:`_3`
has opposite gradients.

Stratospheric air is found when the mass mixing ratio of the e90-tracer
is smaller than the tropopause value, called \ :math:`F_0` in the
routine, and we do this comparison for the mass mixing ratio in the
flux:

| *Tropospheric air*
| Mass in left corner of trapezoid is higher than the stratospheric
  limit :math:`F_0*\texttt{abs(QU)}`, where :math:`QU` is air flux:

  .. math:: F_0 |Q_U| < |Q_{0F,E90}| - |Q_{1F,E90}|

   *Stratospheric air*
| Mass in right corner of trapezoid is lower than the stratospheric
  limit:

  .. math:: F_0  |Q_U| \ge |Q_{0F,E90}| + |Q_{1F,E90}|

| *Partly tropospheric and partly stratospheric air*
| We need to locate the tropopause first. Having the range of the
  trapezoid x-axis [0,1], the tropopause lies within, at a point
  :math:`X`. :math:`X` is therefore the fraction of tropospheric air,
  and lies between :math:`h_L` in Eq. ([eq:ste\_hL]) and :math:`h_R` in
  Eq. ([eq:ste\_hR]):

  .. math:: |Q_{0F,E90}| + |Q_{1F,E90}| - 2 X |Q_{1F,E90}| = F_0 |Q_U|

   Rearranging:

  .. math:: X = - \frac{F_0 |Q_U| - |Q_{0F,E90}| - |Q_{1F,E90}|}{2 |Q_{1F,E90}|}

   The stratospheric fraction is therefore (perhaps more easily) derived
  by looking from right to left on the trapezoid:

  .. math:: X_f = (1-X) = \frac{F_0 |Q_U| - |Q_{0F,E90}| + |Q_{1F,E90}|}{2 |Q_{1F,E90}|}

   To avoid dividing by zero, we limit the divisor
  (:math:`|Q_{1F,E90}|`) by :math:`10^{-10}|Q_{0F,E90}|`.

Again: Note that when using :math:`|Q_{0F}|` and :math:`|Q_{1F}|` for
O\ :math:`_3` to calculate the tropospheric fraction, :math:`X_f` is
effectively the *tropospheric* fraction because its gradient is opposite
of E90.

Having the stratospheric fraction :math:`X_f`, and the tropospheric
fraction :math:`X=(1-X_f)`, we calculate the O\ :math:`_3` flux at
:math:`X`, which we call \ :math:`Y`.

.. math:: Y = |Q_{0F,O3}| - |Q_{1F,O3}| + (1-X_f)\,2\, |Q_{1F,O3}|

 The basis for this equation is that the O\ :math:`_3` gradient and
E90 gradient have opposite signs, so we interpolate the
O\ :math:`_3` flux linearly from the left corner of the
O\ :math:`_3` trapezoid (tropospheric part), to the tropopause value
at \ :math:`X`.

Having the flux (:math:`|Q_{0F,O3}| - |Q_{1F,O3}|`) at the left corner
of the O\ :math:`_3` trapezoid and :math:`Y` at the point
(:math:`1-X_f`), we integrate over the trapezoid, i.e. finding the area
bound by :math:`x=[0,1-X_f]` and :math:`y=[|Q_{0F,O3}|-|Q_{1F,O3}|,Y]`
to find the tropospheric part of the flux:

.. math:: F_{h,t\rightarrow t} = (|Q_{0F,O3}| - |Q_{1F,O3}| + Y) (1-X_f) / 2

Why integrate? While :math:`Q_{0F,O3}` is the mean flux out of the box,
we use the moments and find the flux (i.e the grid box mass transported
to the neighbor box) at both ends of the trapezoid. The total mass
transported equals the area of the trapezoid.

| **Filters**
| The calculation of STE may result in a noisy figure when plotted as
  a map. To reduce the noise, UCI uses a 1–2–1 filter (0.25:0.5:0.25
  weights) for :math:`F_{h,t\rightarrow t}` and for
  :math:`\left(\frac{dM_t}{dt}\right)_{tot}`, given in
  Table [table:stefilter].

I am not totally confident in this method, but in any case, it should be
done in post-processing data, not for calculating the global STE in
Oslo CTM3. Hence, this is not included in the netCDF output file.

--------------

--------------

Oslo CTM3 diagnostics
---------------------

There are several diagnostics developed for the Oslo CTM2 that are
inherited by the Oslo CTM3. The diagnoses are generally located in the
files *diagnostics\_general.f90*. However, there are also some
scavenging diagnostics given in *diagnostics\_scavenging.f90*.

Chemical production and loss
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Production and loss terms for selected species are accumulated during
chemistry routines. Only some species are included in this diagnose so
far, namely CH\ :math:`_4`, N\ :math:`_2`\ O and a few others. The
routine doing the summing is ``save_PL``.

If you need to include more diagnostics, look for ``CHEMLOSS`` and
``CHEMPROD`` in the chemistry routines.

These budget terms, along with the tracer burdens, can be put out in the
routine ``chembud_output``.

Note that some species are difficult to diagnose, such as O\ :math:`_3`,
which is calculated by the use of the O\ :math:`_x` family concept.

The diagnosed losses are used to calculate lifetimes, as will be
described in Section [sxn:ch4n2oburdens].

CH\ :math:`_4` and N\ :math:`_2`\ O burdens and lifetimes
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

When running the Oslo CTM3 with full chemistry, the burdens and
lifetimes of N\ :math:`_2`\ O and CH\ :math:`_4` are calculated out. The
diagnoses can be found in *diagnostics\_general.f90*.

Average burden of N\ :math:`_2`\ O and CH\ :math:`_4` are diagnosed in
``ch4n2o_burden``, while chemical loss is summed up in ``ch4_loss3`` and
``n2o_loss3``.

Calculations are done between the surface and

the model tropopause level.

200hPa.

the 150ppbv O\ :math:`_3` surface.

the model top.

Instant, monthly means and running monthly means are printed to standard
out. This is done in the routine ``report_ch4n2o``, called from
``REPORTS_CHEMISTRY``.

Note that while CH\ :math:`_4` is lost only to OH in the troposphere, it
is also lost to O(\ :math:`^1`\ D) and Cl in the stratosphere, reducing
the lifetime compared to a pure OH diagnostic. See also
Section [sxn:ohch4lifetimes] for atmospheric OH and additional
CH\ :math:`_4` lifetime calculations.

Also note that even though all the loss terms may be diagnosed from
chemistry, the lifetime concept may not hold if there are large sources.
If you include CH\ :math:`_4` emissions, the method may not give the
correct lifetime. However, we assume minimal impact on the loss and
hence report the calculated lifetime anyway.

OH and CH\ :math:`_4` lifetimes
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

The routine ``sumup_burden_and_lifetimes`` calculates atmospheric OH in
different ways:

OH-average CH\ :math:`_4`-kernel, Eq. ([eq:ohch4kernel]), from surface
to model top.

OH-average CH\ :math:`_4`-kernel from surface to ``LMTROP`` (not printed
by default).

OH-average CH\ :math:`_4`-kernel from surface to 200hPa (not printed by
default).

OH-average CO-kernel, Eq. ([eq:ohcokernel]), from surface to model top.

:raw-latex:`\citet{SpivakovskyEA2000}`

:raw-latex:`\citet{LawrenceEA2001}`

Summing up throughout the domains listed above.

CH\ :math:`_4` kernel:

.. math::

   \overline{\textrm{OH}_{\textrm{CH$_4$}}} =
            \frac{\sum m_a\,k_{\textrm{OH+CH$_4$}}\,[\textrm{OH}]}
                 {\sum m_a\,k_{\textrm{OH+CH$_4$}}} \label{eq:ohch4kernel}

 where :math:`m_a` is air mass (kg), and [OH] is concentration
(molecules/cm:math:`^3`) and :math:`k_{\textrm{OH+CH$_4$}}` is the
reaction rate for OH + CH\ :math:`_4`.

CO kernel:

.. math::

   \overline{\textrm{OH}_{\textrm{CO}}} =
            \frac{\sum m_a\,k_{\textrm{OH+CO}}\,[\textrm{OH}]}
                 {\sum m_a\,k_{\textrm{OH+CO}}} \label{eq:ohcokernel}

Instantaneous and running averages are reported by
``report_burden_and_lifetime``, which is called from
``REPORTS_CHEMISTRY``.

Lifetimes using the CH\ :math:`_4` kernel and the Lawrence method are
printed to standard out.

Atmospheric burdens
~~~~~~~~~~~~~~~~~~~

The routine ``diag_burden_snapshot``, called from ``REPORTS_CHEMISTRY``,
puts out instantaneous tropospheric and stratospheric burdens of
selected species. This is done for each meteorological time step, and
currently only O\ :math:`_3`. Other species can be included easily.

Generally it is not very useful to print instantaneous burden that
often, so I have restricted it to O\ :math:`_3` so far.

Atmospheric lifetimes
~~~~~~~~~~~~~~~~~~~~~

It is possible to add calculation of other chemical lifetimes; if you
want to do that, you should follow the method for CH\ :math:`_4` and
N\ :math:`_2`\ O.

Note also that aerosol packages generally have their own calculations of
lifetimes.

Time series of vertical profiles
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Time series of vertical profiles, for given stations, are included. The
code can be found in the file *verticalprofiles\_stations2.f90* in the
*OSLO* directory. Species are diagnosed at the beginning of each
``NOPS`` (i.e. for every hour in standard settings), for each station
grid box.

Using the station location (latitude and longitude), this routine
linearly interpolates horizontally from the 4 closest grid boxes.

An IDL program for reading this output is available in the SVN
repository (see Section [app:svn]).

You specify species to diagnose in the file
*verticalprofiles\_stations2.f90*. Some meteorological components are
also diagnosed, e.g. temperature, pressure, model grid height, air mass,
equivalent latitude, etc.

As input the program reads a file *stationlist\_verticalprofiles.dat* at
model start, while output is done at the beginning of each day
(i.e. after start day), in the file
*hourly\_station\_vprof\_YYYYMMDD.dta*. The stations used in the
standard output are shown in Figure [fig:vprofseriesmap].

.. figure:: figures/stations
   :alt: Map of stations included in the standard output for vertical
   profiles.

   Map of stations included in the standard output for vertical
   profiles.

List of output data for each daily file:

``year``: Year.

``month``: Month of year.

``date``: Date of month.

``LPAR``: Number of vertical layers.

``ntracer``: Number of tracers diagnosed.

``nrofstations``: Number of stations.

``NTHE``: Number of :math:`\theta`-levels for equivalent latitude.

``actual_diag``: Number of ``NOPS`` steps diagnosed, i.e.
``NROPSM * NRMETD``.

``etaa``: Sigma coordinate A (:math:`\eta_a`) for all levels.

``etab``: Sigma coordinate B (:math:`\eta_b`) for all levels.

``stt_components``: Tracer ID numbers diagnosed.

``mole_mass``: Molecular mass of tracers.

``pvthe``: Theta levels for equivalent latitude.

List of output data for each station:

``locname``: Station name.

``loccode``: Station 3-char code.

``lat``: Latitude.

``lon``: Longitude.

``alt``: Altitude.

``ii``: CTM zonal grid box index.

``jj``: CTM meridional grid box index.

``nb_ii``: Zonal neighbor grid box index.

``nb_jj``: Meridional neighbor grid box index.

``nb_xfrac``: Fractional distance to zonal neighbor.

``nb_yfrac``: Fractional distance to meridional neighbor.

``areaxy``: Grid box area [m:math:`^2`] (interpolated).

For all diagnostic steps, the following data are stored:

``psfc``: Surface pressure [hPa].

``BLH``: Boundary layer height [m].

``mass``: Tracer masses [kg/grid box].

``h2o``: H\ :math:`_2`\ O mass [kg/grid box].

``temperature``: Temperature [K].

``airmass``: Air mass [kg/grid box].

``zoflev``: Height of grid box bottoms [m].

``eqlat``: Equivalent latitude on theta levels.

``PVU``: Potential vorticity units.

``tph_pres``: Tropopause pressure [hPa].

Satellite vertical profiles
~~~~~~~~~~~~~~~~~~~~~~~~~~~

A routine for putting out vertical profiles at given locations and
times, e.g. satellite positions, are included in the file
*satelliteprofiles\_mls.f90*. In this file you define which tracers to
put out. Some meteorological components are also diagnosed,
e.g. temperature, pressure, model grid height, air mass, equivalent
latitude, etc. Using the satellite profile location (latitude and
longitude), this routine linearly interpolates horizontally from the
4 closest grid boxes. In the future, it may be better to save data for
all columns and do interpolation in post-processing. Due to the large
amount of data per file, this is not done now.

Since each profile is taken between some start time of ``NOPS`` and
start time of ``NOPS+1``, output is given for both ``NOPS`` and
``NOPS+1``, allowing temporal interpolation as well.

List of output data for each daily file:

``year``: Year.

``month``: Month of year.

``date``: Date of month.

``LPAR``: Number of vertical layers.

``ntracer``: Number of tracers diagnosed.

``nsatprofs``: Number of profiles.

``NTHE``: Number of :math:`\theta`-levels for equivalent latitude.

``etaa``: Sigma coordinate A (:math:`\eta_a`).

``etab``: Sigma coordinate B (:math:`\eta_b`).

``stt_components``: Tracer ID numbers diagnosed.

``mole_mass``: Molecular mass of tracers.

``pvthe``: Theta levels for equivalent latitude.

List of output data for each profile:

``time``: Time of observation.

``time_sec``: CTM ``NOPS`` times before and after.

``lat``: Latitude.

``lon``: Longitude.

``ii``: CTM zonal grid box index.

``jj``: CTM meridional grid box index.

``nb_ii``: Zonal neighbor grid box index.

``nb_jj``: Meridional neighbor grid box index.

``nb_xfrac``: Fractional distance to zonal neighbor.

``nb_yfrac``: Fractional distance to meridional neighbor.

``areaxy``: Grid box area [m:math:`^2`] (interpolated).

For the two closest ``NOPS`` steps, the following data are stored:

``psfc``: Surface pressure [hPa].

``BLH``: Boundary layer height [m].

``mass``: Tracer masses [kg/grid box].

``H2O``: H\ :math:`_2`\ O mass [kg/grid box].

``temperature``: Temperature [K].

``airmass``: Air mass [kg/grid box].

``zoflev``: Height of grid box bottoms [m].

``eqlat``: Equivalent latitude on theta levels.

``PVU``: Potential vorticity units.

``tph_pres``: Tropopause pressure.

O\ :math:`_3` column
~~~~~~~~~~~~~~~~~~~~

Tropospheric and total columns of O\ :math:`_3` are diagnosed in the
subroutine *du\_columns* in the file *diagnostics\_general.f90*. It is
called from *mp\_diag*, which does diagnoses in each IJ-block.

The columns are diagnosed every meteorological time step (``NMET``) and
are stored once every day. This is carried out in the routine
``write_ducolumns``.

3-hourly output
~~~~~~~~~~~~~~~

Oslo CTM3 has the possibility of putting out 3-hourly instantaneous
fields, mostly O\ :math:`_3` and aerosols. These are e.g. used for RF
calculations.

You turn this diagnose on by setting ``LDUMP3HRS=.true.`` at the top of
the file *gmdump3hrs.f90*, and the main routine is ``dump3hrs``.

If you want to add other components than O\ :math:`_3`, this is of
course possible. When you understand the code, it should be easy to
implement.

Snapshots on :math:`\theta`-levels
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

In the routine ``write_snapshot_the``, located in
*diagnostics\_general.f90*, you can put out instantaneous mixing ratios
of selected species, interpolated to the potential temperature
(:math:`\theta`) levels defined in the physics routine, see
Section [sxn:eqlat]. The number of :math:`\theta`-levels (``pvthe``) are
given by ``NTHE``.

Routine puts out data every hour, and the default species put out are
O\ :math:`_3` and N\ :math:`_2`\ O, along with PVU and equivalent
latitude.

Routine can put out data for only one hemisphere or both. See the source
code for how to change it.

The routine is called from ``nops_diag``, but is commented out by
default. A second routine, ``write_snapshot``, is also available,
putting out gridbox mass of selected species on model levels.

Emission tendencies
~~~~~~~~~~~~~~~~~~~

When emissions are treated as production terms in chemistry, i.e. in
standard Oslo chemistry, the total emitted per day is calculated for all
species, in ``tnd_emis_daily``. This is explained in
Section [sxn:emissions\_tnd].

Description of model files
==========================

Here the source files are described. Generally, files inherited from UCI
are placed in the main directory, while the OSLO chemistry and physics
are placed in the directory *OSLO*. The files in the main directory is
often referred to as the core files, and will be described next.

UCI core source files
---------------------

The Oslo CTM3 started out based on the UCI CTM version 5.6d, but was
later updated to 6.1b, before most of the UCI code was restructured into
Fortran90 free format, and all common blocks were abandoned. In this
process I renamed most of the files. Future core update should be fairly
straight-forward for a somewhat experienced user.

This Section describes the core files necessary to run the Oslo CTM3. If
you want to go back and see the UCI codes, you should contact UCI.

First the global variable files (common files) are listed, then the core
files are listed alphabetically.

*cmn\_precision.f90*
~~~~~~~~~~~~~~~~~~~~

[app:core\_precision] This file defines the precision of the
Oslo CTM3 floating point variables. Several are used:

Double precision. Most variables use this.

Single precision.

Precision of the second order moments. By default this is single
precision.

Precision of the global average arrays. By default this is single
precision.

Precision of the global tendency arrays. By default this is single
precision.

*cmn\_size.F90*
~~~~~~~~~~~~~~~

[app:core\_size] Contains the array size parameters and flags. Examples
are ``IPAR``, ``IPARW``, ``JPAR``, ``JPARW``, ``LPAR``, ``LPARW``,
``MPIPAR``, ``MPJPAR``, ``NRMETD``, ``NPAR``, etc.

*cmn\_parameters.f90*
~~~~~~~~~~~~~~~~~~~~~

[app:core\_parameters] Contains chemical and physical parameters, such
as the Earth radius (``A0``), Avogadro’s number (``AVOGNR``), Apparent
molecular weight of dry air (``M_AIR``), gas constants (``R_AIR``,
``R_UNIV``, ``R_ATM``), and many more. These are all listed in
Table [table:parameters].

+-----------------+--------------------------------+--------------------------------------------------------------------------------+
| Name            | Value                          | Description                                                                    |
+=================+================================+================================================================================+
| ``A0``          | 6371000                        | Earth radius [m]                                                               |
+-----------------+--------------------------------+--------------------------------------------------------------------------------+
| ``G0``          | 9.80665                        | Gravitational constant [m/s:math:`^2`]                                         |
+-----------------+--------------------------------+--------------------------------------------------------------------------------+
| ``CPI``         | 3.141592653589793              | :math:`\pi`                                                                    |
+-----------------+--------------------------------+--------------------------------------------------------------------------------+
| ``C2PI``        | ``2*CPI``                      | 2\ :math:`\pi`                                                                 |
+-----------------+--------------------------------+--------------------------------------------------------------------------------+
| ``CPI180``      | ``CPI/180``                    | Conversions to radians                                                         |
+-----------------+--------------------------------+--------------------------------------------------------------------------------+
| ``ZPI180``      | ``1/CPI180``                   | Conversions from radians                                                       |
+-----------------+--------------------------------+--------------------------------------------------------------------------------+
| ``atm2Pa``      | 101325                         | Conversion from atmospheres to Pa                                              |
+-----------------+--------------------------------+--------------------------------------------------------------------------------+
| ``Pa2atm``      | ``1/atm2Pa``                   | Conversion from Pa to atm                                                      |
+-----------------+--------------------------------+--------------------------------------------------------------------------------+
| ``J2kcal``      | 4186.8                         | Numbers of kcal in a J                                                         |
+-----------------+--------------------------------+--------------------------------------------------------------------------------+
| ``M_AIR``       | 28.97                          | Molecular mass for air [g/mol]                                                 |
+-----------------+--------------------------------+--------------------------------------------------------------------------------+
| ``R_UNIV``      | 8.31446                        | Universal gas constant [J/(Kmol)] = [m:math:`^3`\ \*Pa/(Kmol)]                 |
+-----------------+--------------------------------+--------------------------------------------------------------------------------+
| ``R_ATM``       | ``R_UNIV * Pa2atm``            | Gas constant in [m:math:`^3`\ \*atm/(Kmol)] (:math:`\sim`\ 8.205e-5)           |
+-----------------+--------------------------------+--------------------------------------------------------------------------------+
| ``R_AIR``       | ``R_UNIV / M_AIR * 1000``      | Specific gas constant for air [J/(Kkg)] (:math:`\sim`\ 287)                    |
+-----------------+--------------------------------+--------------------------------------------------------------------------------+
| ``R_H2O``       | ``R_UNIV / 18.01528 * 1000``   | Specific gas constant for water vapor [J/(Kkg)] (:math:`\sim`\ 461)            |
+-----------------+--------------------------------+--------------------------------------------------------------------------------+
| ``AVOGNR``      | ``6.022149e23``                | Avogadro’s number [molecules/mol]                                              |
+-----------------+--------------------------------+--------------------------------------------------------------------------------+
| ``BOLTZMANN``   | ``1.38063e-23``                | Boltzmann’s const [J/(Kmolecules)]                                             |
+-----------------+--------------------------------+--------------------------------------------------------------------------------+
| ``KBOLTZ``      | ``1.e4 * BOLTZMANN``           | Boltzmann’s const [mbcm3/(Kmolecules)]                                         |
+-----------------+--------------------------------+--------------------------------------------------------------------------------+
| ``cp_air``      | 1004                           | Specific heat of dry air at constant pressure [J/(Kkg)                         |
+-----------------+--------------------------------+--------------------------------------------------------------------------------+
| ``Lv_0C``       | ``2.501e6``                    | Latent heat of vaporization at 0\ :math:`^{\circ}`\ C [J/kg]                   |
+-----------------+--------------------------------+--------------------------------------------------------------------------------+
| ``dLv_dT``      | ``-0.00237e6``                 | Gradient of Lv between 0C and 100C [(J/kg)/K]                                  |
+-----------------+--------------------------------+--------------------------------------------------------------------------------+
|                 |                                | :raw-latex:`\citep[Table C-5 in][]{Stull1988}`                                 |
+-----------------+--------------------------------+--------------------------------------------------------------------------------+
| ``TK_0C``       | 273.15                         | Temperature [K] at 0\ :math:`^{\circ}`\ C                                      |
+-----------------+--------------------------------+--------------------------------------------------------------------------------+
| ``es_0C``       | 611.2                          | Saturation vapor pressure of H\ :math:`_2`\ O at 0\ :math:`^{\circ}`\ C [Pa]   |
+-----------------+--------------------------------+--------------------------------------------------------------------------------+
| ``secDay``      | 86400                          | Seconds in a day                                                               |
+-----------------+--------------------------------+--------------------------------------------------------------------------------+
| ``secYear``     | 31536000                       | Seconds in a year                                                              |
+-----------------+--------------------------------+--------------------------------------------------------------------------------+
| ``MINTEMP``     | 150                            | Minimum temperature allowed in chemistry                                       |
+-----------------+--------------------------------+--------------------------------------------------------------------------------+
| ``MAXTEMP``     | 350                            | Maximum temperature allowed in chemistry                                       |
+-----------------+--------------------------------+--------------------------------------------------------------------------------+
| ``TEMPRANGE``   | ``MAXTEMP - MINTEMP``          | Temperature range.                                                             |
+-----------------+--------------------------------+--------------------------------------------------------------------------------+
| ``LDEBUG``      | .true.                         | Global flag for including more extensive debugging.                            |
+-----------------+--------------------------------+--------------------------------------------------------------------------------+

*cmn\_ctm.f90*
~~~~~~~~~~~~~~

[app:core\_ctm] Defines the main arrays for the transport model, such as
``STT`` and the moments (``SUT``, ``SVT``, ``SWT``, ``SUU``, ``SVV``,
``SWW``, ``SUV``, ``SUW``, ``SVW``).

Also grid info such as longitude (``XGRD``, ``XDGRD``, ``XEDG``,
``XDEDG``), latitude (``YGRD``, ``YDGRD``, ``YEDG``, ``YDEDG``), and
sigma hybrid coordinates (``ETAA``, ``ETAAW`` ``ETAB``, ``ETABW``) are
defined here.

*cmn\_chem.f90*
~~~~~~~~~~~~~~~

[app:core\_chem] Defines the main arrays for emissions and chemistry,
such as ``E3DSNEW(LPAR,IPAR,JPAR,E3PAR)`` and the tables mapping species
to emission datasets:

``NE2TBL``: Total number of 2D emission datasets.

``NM2TBL``: Table of months the emissions apply for.

``NY2TBL``: Table of months the emissions apply for.

``E2DS``: All 2D emission datasets.

``E2LTBL``: Table mapping species to 2D emission datasets.

``E2STBL``: Table of scaling factors for the mapping of each species and
2D emission datasets.

Similar arrays exist for 3D emissions.

Here you also find the tracer name ``TNAME``, tracer molecular weight
``TMASS``, and other variables such as conversion factors
``TMASSMIX2MOLMIX`` and ``TMOLMIX2MASSMIX``.

The variables are generally well described in the source code.

*cmn\_diag.f90*
~~~~~~~~~~~~~~~

[app:core\_diag] Defines arrays for diagnostics, such as ``STTAVG``,
``AIRAVG``, ``JDO_A``.

*cmn\_fjx.f90*
~~~~~~~~~~~~~~

[app:core\_fjx] Defines fast-JX parameters and arrays.

*cmn\_met.f90*
~~~~~~~~~~~~~~

[app:core\_met] Defines meteorological arrays.

*cmn\_sfc.f90*
~~~~~~~~~~~~~~

[app:core\_sfc] Defines surface variables such as land surface type
(``landSurfTypeFrac``), land-sea mask (``LSMASK``), and dry deposition
velocity (``VDEP``).

*averages.f90*
~~~~~~~~~~~~~~

Contains routines for summing up 3D averages.

``AVG_WRT_NC4``: Write core 3D averages (the avgsav-files), including
``XSTT`` averages, surface pressure averages (which is 2D), and some
meteorological averages.

``AVG_ADD2``: Add to 3D averages.

``AVG_CLR2``: Clear 3D averages.

``AVG_P1``: Write 1D averages to screen.

``AVG_WRT2``: Only included for history. See ``AVG_WRT_NC4``. Write core
3D averages (the avgsav-files), including ``XSTT`` averages, surface
pressure averages (which is 2D), and several meteorological averages.

*budgets.f90*
~~~~~~~~~~~~~

Contains routines for summing up tendency budgets.

``TBGT_G``: Clear budget arrays.

``TBGT_L``: Accumulate budgets layerwise.

``TBGT_IJ``: Accumulate budgets in IJ-block.

``TBGT_P0``: 0D print to screen.

``TBGT_P1``: 1D print to screen.

``TBGT_P2``: 2D print to screen.

*cloudjx.f90*
~~~~~~~~~~~~~

Routines for treating cloud-J. Currently set up for cloud2 as used by
fast-JX 6.7c, but will be updated when fast-JX 7.3c is included.

``cloud_init``: Initialise cloud-J.

``RANSET``: Set random number. Random seed is ``RANSEED``, which is set
in *LxxCTM.inp*.

Some important variables for cloud2:

``LCLDAVG``:

``LCLDQMD``:

``LCLDQMN``:

``LCLDRANA``:

``LCLDRANA``:

*convection.f90*
~~~~~~~~~~~~~~~~

This is the f90 version of the UCI *p-cnvw.f*. It has been modified to
Oslo CTM3, calculating elevator fractions, and also the Henry
coefficients used in convective washout. A new convective wet loss
variable is included; ``CNV_WETL(LPAR,NPAR)``.

``CONVW_OSLO``: The main routine.

``ADJFLX2``: Adjust convective fluxes so that no more than a fraction of
the grid box mass is moved each time step.

``ADJFLX`` Old version of ``ADJFLX2``.

``QCNVW2_OSLO``: Column model calculating convective transport, and also
convective scavenging by updrafts.

``QCNVW2``: UCI routine kept for history.

``SCAV_UPD``: UCI routine kept for history.

*fastjx.f90*
~~~~~~~~~~~~

[app:core\_fastjx] Contains routines for fastJX, except the main driver
which is so far located in *p-phot\_oc.f*. That will change when fast-JX
is updated to 7.3.

``RD_XXX``: Reads *FJX\_spec.dat*.

``RD_MIE``: Reads *FJX\_scat.dat*.

``RD_JS``: Reads *ratj\_oc.d* file.

``RD_O1D``: Reads ``O1D`` entry of *ratj\_oc.d* file.

``SET_ATM``: Reads O\ :math:`_3` and temperature climatologies to use
above the model domain. It can possibly be used in the stratosphere when
stratospheric chemistry in not included, but Oslo CTM3 uses its own
climatology for that. Sets e.g. ``TOPT``, ``TOPM``, ``TOP3``.

| *Changes compared to UCI p-phot.f file*
| In the subroutine ``PHOT_IN`` a small piece of ASAD code was removed.
  Some other ASAD variables has been removed.

Also, the subroutine ``SET_ATM`` had to be modified to include
climatology data for the uppermost layer (``LPAR3``, ``LPART`` and
``LPARM``).

*grid.f90*
~~~~~~~~~~

Routines for setting up model grid.

``SET_GRID``: Sets up global grid, such as latitude, longitude, area of
grid boxes, etc.

``LABELG``: For printing grid info to screen.

``DIAGBLK``: Finds grid box start/end indices for box diagnose.

``DIAG_LTSTN``: Finds grid box indices for station diagnose.

``DIAG_LTGL``: Finds grid box indices for local time station diagnose.

``GAUSST2``: Calculate Gaussian quadrature points and weights.

``DBLDBL``: Define longitude and latitude of horizontally degraded grid.

``AIRSET``: Sets up 3D air mass.

``SURF_IN``: Reads land fraction datasets.

``gridICAO``: Not used. I think this finds grid for some ICAO emission
data.

*initialize.f90*
~~~~~~~~~~~~~~~~

[app:core\_initialize] Routines for initializing the Oslo CTM3.

``INPUT``: Reads *LxxCTM.inp*.

``SETUP_SPECIES``: Reads restart file.

``SETUP_UNF_OUTPUT``: Opens unformatted output files (UCI heritage).

``report_zeroinit``: Reports which tracers are initialized to zero
(i.e. not read from file).

*lightning.f90*
~~~~~~~~~~~~~~~

[app:core\_lightning] Contains routines for calculating lightning NOx
(L-NOx) emissions. There are several methods for doing this, as
described in Section [sxn:emissions\_lightning].

``getScaleFactors``: Sets scaling factors based on which meteorological
dataset is used.

``LIGHTNING_OAS2015``: Default method for calculating L-NOx.

``LIGHTNING_GMD2012``: L-NOx as described by
:raw-latex:`\citet{SovdeEA2012}`.

``LIGHTNING_UCI2015``: L-NOx as used by UCI-CTM in 2015. I do not
recommend using this.

``LIGHTDIST``: Sets up vertical distribution of L-NOx.

``filterLNC``: Filter convective mass fluxes to reduce noise in L-NOx
horizontal distribution.

``distland``: Finds grid boxes that are land or have land in 300km
proximity. Test routine to make land near-land lightning behaving like
land. **Not used.**

``LIGHTNING_ALLEN2002``: L-NOx as described by
:raw-latex:`\citet{AllenPickering2002}`. Not evaluated.

*metdata\_ecmwf.f90*
~~~~~~~~~~~~~~~~~~~~

[app:core\_metdataecmwfnc] Read-in for ECMWF IFS meteorological data on
netCDF4 format.

``update_metdata``: Updates meteorological data.

``fluxfilter2``: Filter convective mass fluxes; removes noise.

``data2mpblocks``: Puts globally gridded (``IPAR,JPAR``) data into
IJ-block (``IDBLK,JDBLK,MPBLK``).

``gotData``: Prints info to standard out.

``skipData``: Prints info to standard out.

*metdata\_ecmwf\_uioformat.f90*
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

[app:core\_metdataecmwfuio] Traditional read-in for ECMWF IFS
meteorological data on UIO binary format.

*omp.f90*
~~~~~~~~~

Routines for switching between global and IJ-block structures. Slightly
modified compared to UCI routines, e.g. looping is changed to reduce
striding.

``MPSPLIT``: Split global arrays into IJ-block arrays.

``MPBIND``: Convert back to global arrays.

``get_iijjmp``: From global indices ``I,J``, find IJ-block indices
``II,JJ,MP``. Not really necessary because of the array
``all_mp_indices`` and the next routine:

| ``get_all_mpind``: Sets up array ``all_mp_indices`` to map global
  indices to IJ-block indices.
| ``all_mp_indices(1,I,J)=II``
| ``all_mp_indices(2,I,J)=JJ``
| ``all_mp_indices(3,I,J)=MP``

*pbl\_mixing.f90*
~~~~~~~~~~~~~~~~~

Routines for calculating planetary boundary layer (PBL) mixing. This is
the f90 version of the UCI file *p-pbl.f*, but modified to use the
scheme by :raw-latex:`\citet{HoltslagEA1990}`.

``CNVBDL``: Main routine for calculating PBL mixing.

``BULK``: Prather bulk scheme.

``get_keddy_L1``: Calculates diffusivity in the lowermost model layer,
as done in the routine ``KPROF``.

``KPROF2``: Calculates diffusivity of heat used in PBL closure, as used
by the :raw-latex:`\citet{HoltslagEA1990}` scheme. Should be used
instead of ``KPROF``, since unnecessary calculations have been removed.

``KPROF``: Calculates diffusitivities used in PBL closure, as used by
the :raw-latex:`\citet{HoltslagEA1990}` scheme.

``PHIM``: Calculates similarity theory stability correction for
momentum.

``PHIH``: Calculates similarity theory stability correction for heat.

``TRIDIAG``: Solves tri-diagonal system.

``INTERP``: Mass weighted interpolation routine to determine PBL
profiles from similarity. Ekman solution.

``QXZON``: Combines mass boxes into extended zones.

``QZONX``: Divides one extended zone into equal-mass boxes.

*pmain.f90*
~~~~~~~~~~~

[app:core\_pmain] The main program  has been modified for the Oslo CTM3.
Input routines are changed and also some calls to master routines. There
is also an internal chemistry loop of max 15minutes for the processes
mixing/emissions/chemistry/deposition. This is to reduce the large
changes imposed by emissions and deposition. For large deposition
values, all of a tracer can be removed at the surface if the time step
is too long. For a 16m layer, a deposition velocity of 1.77cm/s will
remove everything in 15minutes. Linoz calls has been removed.

See Section [sxn:internal\_chem\_loop] for more.

*regridding.f90*
~~~~~~~~~~~~~~~~

[app:core\_regridding] Routines for regridding data.

``E_GRID``: Regridding of 2D horizontal field. Field must **not** be per
area, i.e. concentrations and mixing ratios must be multiplied by area
before interpolation, and divided by new area afterwards. Handles
emission dataset which include moments (usually not used in Oslo CTM3).
``XBEDG`` and ``XDEDG`` may have shift at dateline. ``YBEDG`` and
``YDEDG`` must start at 90S.

``E_GRID_Y``: Regridding of array in meridional direction.

``TRUNG8``: Convert double precision (``r8``) field from native
resolution to degraded horizontal resolution.

``TRUNG4``: Convert single precision (``r4``) field from native
resolution to degraded horizontal resolution.

*source\_uci.f90*
~~~~~~~~~~~~~~~~~

[app:core\_sourceuci]

| ``SOURCE``: Routine for adding emissions to emission array each time
  step, when treated as a separate process (i.e. not as production term
  in chemistry).
| **Not tested with Oslo chemistry**.
| When treated as chemistry production term, the routine ``emis4chem``
  in *emisdep4chem\_oslo.f90* is used (see
  Appendix [app:emisdep4chem\_oslo]).

*scavenging\_largescale\_uci.f90*
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Large scale scavenging as described in Section [sxn:ls\_scav]. Some
modifications from Oslo CTM3 are implemented.

``WASHO``: Master routine calling ``WASH1`` or ``WASH2``.

``WASH1``: Simple UCI wet scavenging routine. Kept for history; should
not be used.

``WASH2``: Routine by :raw-latex:`\citet{NeuPrather2012}`, partly
modified for Oslo CTM3.

``DISGAS``: Calculates the mass of tracer dissolved in aqueous phase.
Uses the flag ``IT258K`` to define how to use Henry’s law.

``HENRYS``: Use Henry’s law to calculate mass dissolved in aqueous
phase. No dissolved tracer below 258K, and uses retention coefficient
between 258K and 273K.

``HENRYSallT``: Same as ``HENRYS``, but uses retention coefficient also
below 258K.

``RAINGAS``: Calculates tracer (kg) picked up by *new* rain.

``WASHGAS``: Calculates tracer picked up by falling precipitation (kg)
and also evaporated from precipitation (kg).

``DIAMEMP``: Empirical fit of precipitation diameter to cloud water
density and rain rate, following
:raw-latex:`\citet{FieldHeymsfield2003}`.

``GAMMAX``: Calculates gamma using ln gamma algorithm.

``WETSET_CTM3``: Initialises and sets wet scavenging parameters for
Oslo CTM3.

*spectral\_routines.f*
~~~~~~~~~~~~~~~~~~~~~~

Spectral routines, not collected as module. These are purely inherited
from UCI/Oslo CTM2.

``SPE2GP``: Converts spectral field to gridded field.

``ZD2UV``: Converts spectral fields of vorticity and divergence to
gridded field of zonal wind (``U``) and meridional wind (``V``).

``UVCOEF``: Used by ``ZD2UV`` to calculate spectral coefficients.

``FFT_99``: Multiple fast real periodic transform of length N performed
by removing redundant operations from complex transform length N.

``FFT_RPASS``: Performs one pass-thru data as part of multiple FFT
(``FFT_99``).

``FFT_DDSS``: Calculate constant arrays needed for evaluating spectral
coefficients for ``U`` and ``V`` from vorticity and divergence.

``FFT_SET``: Computes factors of N and trigonometric functions needed by
``FFT_99``

``LEGGEN``: Calculate Legendre functions.

``GAUSST``: Old routine for calculating Gaussian quadrature points and
weights. Use ``GAUSST2`` (*grid.f90*) instead.

*steflux.f90*
~~~~~~~~~~~~~

[app:core\_steflux] Routines for calculating stratosphere-troposphere
exchange (STE) as explained in Section [sxn:ste]. This is the f90
version of UCI code *p-chemflux.f*.

``CHEMFLUX``: Saves the horizontal flux of specified tracer (usually
O\ :math:`_3`) in the troposphere, but only for tropospheric to
tropospheric boxes.

``CHEMFLUX_E90``: Finds horizontal fluxes within the troposphere based
on e90-tracer.

``dumpuvflux``: Accumulates horizontal fluxes into 3D arrays.
Accumulates over time period defined by flux calendar (``JDO_X``).

``DUMPTRMASS``: Accumulates tropospheric mass of tracer (O:math:`_3`).

``DUMPTRMASS_E90``: Accumulates tropospheric mass of tracer
(O:math:`_3`) based on e90 tracer.

``SAVETRMASS``: Saves daily average mass of tracer (O:math:`_3`).

``STEBGT_CLR``: Clears STE diagnostics.

``STEBGT_WRITE``: Writes STE budget to netCDF4 file.

``ctm3_pml``: Calculates tendency (production minus loss; PML) below
predefined O\ :math:`_3` surfaces.

``ctm3_o3scav``: Calculates net scavenged O\ :math:`_3` below predefined
O\ :math:`_3` surfaces.

``chemflux_setup``: Set up STE diagnostics.

*stt\_save\_load.f90*
~~~~~~~~~~~~~~~~~~~~~

Contains routines to save and load STT from restart files.

``load_restart_file``: Restart from netCDF4 files saved by
``save_restart_file``. Setting argument ``MODE`` to zero will read the
main restart file (also setting AIR), while non-zero will read
additional fields from specified files. This has to be hard-coded in the
subroutine ``SETUP_SPECIES`` in the file *initialize.f90*.

``getField_and_interpolate``: This routine is bound to
``load_restart_file``, and reads a 3D field from a netCDF4 file, with
``ncid`` as file id and ``var_id`` as variable id. If resolution on file
differs from model resolution, the field is interpolated to model
resolution. Vertical interpolation is not possible yet.

``save_restart_file``: Saves netCDF4 restart file. Contains variables
necessary for restarting a run, as well as useful variables for other
purposes. Transported species have prefix ``STT_``, and for these
species there are also moments available, having prefixes ``SUT_``,
``SVT_``, ``SWT_``, ``SUU_``, ``SVV_``, ``SWW_``, ``SUV_``, ``SUW_``,
``SVW_``. Non-transported species have prefix ``XSTT_``.

``OSLO_CON_RUN``: Restart from files saved by ``OC_CON_SAV``. Setting
argument ``MODE`` to zero will read the main restart file, while
non-zero will read additional fields from any specified file. Must match
model resolution.

``OSLO_CON_SAV``: Old routine for saving restart file; instant fields,
component by component. Called based on input file flag ``JDO_C``. The
restart file also contains grid info to use when e.g. interpolating to
other resolutions. Should not be used.

``restart_from_CTM3avg``: Starts from Oslo CTM3 *avgsavDDDDD.dta* files,
version 7. Reads the file *avgsav\_input* (no extension), so you must
rename the input file accordingly. Must match model resolution.

``restart_from_CTM3avg_T42``: Same as ``restart_from_CTM3avg``, but
reads T42 resolution *avgsavDDDDD* files and interpolates to current
resolution. It reads the file *avgsav\_input* (no extension).

``OSLO_CON_RUN42``: Reads Oslo CTM3 T42 resolution restart file, and
interpolates to current resolution. Works on sav-files version 1 and 2.

``OSLO_CON_RUNxx``: Reads any Oslo CTM3 resolution restart file, and
interpolates to current resolution. Requires that the sav-file is
version 2 or higher. Will stop if model resolution is the same as on
file, because not all moments are used.

``OSLO_RESTARTFILE_INFO``: Retrieves resolution info from the old binary
restart files.

*utilities.f90*
~~~~~~~~~~~~~~~

Various utilities for Oslo CTM3. More utilities are given in
*utilities\_oslo.f90*.

``write_log``: Writes start and end of model to screen.

``model_info``: Prints info about run to screen.

``calendar``: Calculates day counters, but also some info about next
time step, such as ``JYEAR_NEXT``, ``JMON_NEXT``, ``JDAY_NEXT``,
``JDATE_NEXT``.

``is_leap``: Calculates whether a year is leap year or not.

``get_soldecdis``: Calculates solar declination and distance to Sun.
Same as originally found in ``CALENDR``.

``CALENDR_OLD``: Old routine to calculate day counters and also solar
declination and distance to Sun. Not used.

``CALENDL``: Looks up calendar and checks for flags.

``LOCSZA``: Calculates cosine of local solar zenith angle, and the solar
flux factor.

``LCM``: Calculates the least common multiple of two numbers.

``ctmExitC``: Exit routine printing message.

``ctmExitL``: Exit routine printing message and label.

``ctmExitIJL``: Exit routine printing message and ``I,J,L``.

``get_free_fileid``: Finds a free file id number.

``get_dinm``: Returns the number of days in months, depending on whether
year is leap year or not.

``CFRMIN``: Set minimum limit on cloud fraction.

``CIWMIN``: Set minimum limit on in-cloud water and ice ratios (kg/kg).

``check_btt``: Checks ``BTT`` array for negatives and NaNs. Allows
tracer id 110 (sum of oxygen, SO) to be negative.

``adjust_moments``: Scales down moments if ``BTT`` has been reduced
during a process (if it is smaller than ``BTTBCK``).

*p-cloud2.f*
~~~~~~~~~~~~

[app:core\_cloud2] Routines for calculating cloud2. Will be updated when
cloud-J is included.

``CLOUD``: Calculates cloud properties.

``QUADCA``: Generates 4 quadrature independent column atmospheres
(ICAs).

``OD_LIQ``: Sets effective radius and extinction for liquid clouds.

``CLDQUAD``: Generates independent ICAs from max-random overlap
criteria.

``QUADMD``: Sort ICAs in order of increasing optical depth. Calculates
weights.

``ICANR``: Evaluate number of ICAs.

``HEAPSRT``: Heap sort.

*p-dyn0.f*
~~~~~~~~~~

Sets up atmospheric variables after they have been read from file.

``DYN0``: Sets up advective fields.

``EPZ_UV``: Redistributes ``U`` and ``V`` fluxes to smooth over extended
polar zones.

``EPZ_TQ``: Average temperature (``T``) and specific humidity (``Q``)
over extended polar zones.

``EPZ_P``: Average surface pressure (``P``) over extended polar zones.

``PFILTER``: Global filter to smooth pressure errors. Also adjusts the
horizontal fluxes accordingly.

``CFLADV``: Calculates global Lifshitz (not CFL) time step limiter for
advection step. ``NADV`` is the number of global advection steps.

*p-dyn0-v2.f*
~~~~~~~~~~~~~

Same as *p-dyn0.f*, but for more accurate polar cap treatment.

*p-dyn2.f*
~~~~~~~~~~

Contains advection subroutines.

``DYN2UL``: Zonal advection.

``DYN2VL``: Meridional advection.

``DYN2W_OC``: Vertical advection.

``QLIMIT2``: Quick SOM limiter only for LIM=2 (pos+mono) in the
X direction.

``POLES1``: Combine polar-pie box with next lower latitude (``J=1,2``
and ``J=JM-1,JM``) using SOM.

``POLES2``: Split extended polar-pie box back into two (``J=1,2`` and
``J=JM-1,JM``). Use SOM to split, need to know final air mass in ``J=1``
and ``J=JM``.

*p-dyn2-v2.f*
~~~~~~~~~~~~~

Same as *p-dyn2.f*, but for more accurate polar cap treatment. Does not
use ``POLES1`` and ``POLES2``. Calls from  needs to be modified
(described in ).

*p-linoz.f*
~~~~~~~~~~~

[app:core\_linoz] Linoz is available in Oslo CTM3, but only for STE
calculation (Section [sxn:ste]). Here are the available routines, but
note that some are not used because Linoz is only used for STE.

``LNZ_INIT``: Read/init Linoz data and other strat chem tables from
pratmo box model.

``LNZ_SET``: Set up Linoz (and other stratosphere) table data for each
day/month/year.

``LNZ_SETO3``: Initialize Linoz O\ :math:`_3` (N=N\_LZ) based on
supplied climatology.

``LNZ_PML``: Linoz = linearize P-L for stratospheric ozone based on
tables from the PRATMO model using climatological T, O\ :math:`_3`,
Month.

``INT_LAT``: Interpolate from pratmo standard tables (85S to 85N) to CTM
J-grid.

``INT_MID``: Interpolation routine, using grid box mid-point.

``INT_SOM``: Interpolation routine using moments.

``DECAY``: Simple e-fold decay of species throughout the model domain.
**Not used; Oslo CTM3 uses its own routine**.

``TPAUSEB``: Defines the tropopause level based on Linoz O\ :math:`_3`.
**Not used; rather use ``TPAUSEB_E90``**.

``TPAUSEG``: **Not used; rather use ``TPAUSE_E90``**.

*p-vect3.f*
~~~~~~~~~~~

[app:core\_vect3] The second order moment 1D transport routine
``QVECT3``.

*p-phot\_oc.f*
~~~~~~~~~~~~~~

Contains the column model for fast-JX, used by the main driver
``jv_column`` in *main\_oslo.f90*.

``PHOTOJ``: Column model for J-values. ``T``, ``O3`` and mass is also
set from climatology in the uppermost layer (``TTJ``, ``DDJ`` and
``ZZJ`` for ``L1_-1``).

``OPTICL``: Sets cloud fast-JX properties at the std 5 wavelengths (200,
300, 400, 600, 999nm).

``OPTICA``: Sets aerosol fast-JX properties at the std 5 wavelengths
(200, 300, 400, 600, 999nm).

``OPTICM``: Sets fast-JX properties for Mie scattering.

``SOLARZ``: Solar zenith angle.

``SPHERE2``: Calculation of spherical geometry.

``EXTRAL``: Adds sub-layers (JXTRA) to thick cloud/aerosol layers.

``FLINT``: Three point linear interpolation function.

``JRATET``: Interpolates J-values.

``JP_ATM``: Print out atmosphere used in J-value calc

``OPMIE``: Mie scattering code.

``MIESCT``: Mie scattering, used by ``OPMIE``.

``LEGND0``: Calculates ordinary Legendre functions.

``BLKSLV``: Sets up and solves the block tri-diagonal system.

``GEN_ID``: Generates coefficient matrices for the block tri-diagonal
system.

````:

Oslo source files
-----------------

[app:description\_OSLO\_files] The Oslo source files are located in the
directory *OSLO*.

*cmn\_oslo.f90*
~~~~~~~~~~~~~~~

[app:cmn\_oslo] Defines variables needed for Oslo chemistry, which are
not part of the transport code. It may be that some of these variables
should have been in the other *cmn*-files.

Physical variables:

``LMTROP(IPAR,JPAR)``: The uppermost level of troposphere. Stratosphere
starts at ``LMTROP+1``.

``NEW_TP``: Switch used to diagnose the calculated tropopause.

``PARTAREA(LPAR,IPAR,JPAR)``: Background aerosol surface area density.
Be aware that the indexing has changed since Oslo CTM2.

Convective washout variables:

``TCCNVHENRY(NPAR)``: Flag to decide which Henry constant to use for
convective wash out.

``LELEVTEMP(2,IDBLK,JDBLK,MPBLK)``: Flag for convective plume/elevator
temperatures. If minimum plume temperature is below 258K the first entry
is 1, and if maximum temperature is below 273.15K the second entry is 1.
Otherwise these flags are 0.

``QFRAC`` Fraction of elevator convective precipitation / elevator
liquid water volume.

``LW_VOLCONC`` Convective elevator liquid water volume concentration.

Air values:

``AIRMOLEC_IJ(LPAR,IDBLK,JDBLK,MPBLK)``: Air density.

``DV_IJ(LPAR,IDBLK,JDBLK,MPBLK)``: Gridbox volume.

Tracer related variables:

``trsp_idx(TRACER_ID_MAX)``: Mapping from chemical ID to transport
number.

``chem_idx(NPAR)`` Inverse mapping for ``trsp_idx`` (from transport
number to chemical ID).

``Xtrsp_idx(TRACER_ID_MAX)`` Mapping from chemical ID to non-transported
number (place in ``XSST``).

``Xchem_idx(NOTRPAR)`` Reverse mapping for ``Xtrsp_idx``.

``XTNAME(NOTRPAR)`` Name array for non-transported tracers.

``XTMASS(NOTRPAR)`` Tracer mass for non-transported tracers.

``XTMASSMIX2MOLMIX(NOTRPAR)`` Conversion from kg/kg to mole/mole
(i.e. ``M_AIR/XTMASS``).

``XTMOLMIX2MASSMIX(NOTRPAR)`` Conversion from mole/mole to kg/kg
(i.e. ``XTMASS/M_AIR``).

``XSTT(LPAR,NOTRPAR,IPAR,JPAR)``: The non-transported tracer
distribution.

``XSTTAVG(LPAR,NOTRPAR,IPAR,JPAR)``: The diagnostic (avgsav) array of
the non-transported tracers.

Chemical variables:

``JVAL_IJ(JPPJ,LPAR,IDBLK,JDBLK,MPBLK)``: J-values.

``STT_2D_LB``: Lower boundary conditions for tracers.

``STT_2D_LT``: Upper boundary conditions for tracers.

``TROPCHEMnegO3(LPAR,MPBLK)``: Counting number of negative O\ :math:`_3`
occurring in the troposphere (I have only found this in the surface
layer).

``PR42HET(IPAR,JPAR)``: Aerosol surface conversion of
N\ :math:`_2`\ O\ :math:`_5` to HNO\ :math:`_3`.

``CH4FIELD(IPAR,JPAR)``: Surface field for CH\ :math:`_4` set each
month. Not used when CH\ :math:`_4` emissions are turned on.

Emission variables:

``EMIS_IJ(LPAR,NPAR,IDBLK,JDBLK,MPBLK)``: Emissions for Oslo chemistry
treatment as production in chemistry, i.e. units
[molec/(cm:math:`^3`\ s)].

``DIAGEMIS_IJ``: Diagnose emissions accumulated (kg) over time span
defined by budget calendar (``JDO_T``).

``emisTotalsDaily(NPAR,366)``: Diagnose daily accumulated emissions
(kg).

``emisTotalsOld(NPAR)``: Used for the daily accumulated emissions
diagnose.

``METHANEMIS``: Logical specifying whether CH\ :math:`_4` emissions are
to be used or not.

``NECAT``: Number of emission categories to diagnose (only used for
diagnostics).

``ECATNAMES(NECAT)``: 3-character names for each emission category. So
far the RETRO categories are used.

``E2CTBL(ETPAR)``: Mapping category number to emission table.

``E2LocHourTBL(ETPAR)``: Table mapping 2D emission table to diurnal
variation index. See Section [sxn:emissions\_diurnal].

``NE2LocHourVARS``: Number of local hour scalings.

``E2LocHourSCALE(24,NECAT,NE2LocHourVARS)``: Local hour scalings.

``E22dTBL(ETPAR)``: Table mapping 2D emission table to horizontal 2D
variation index. See Section [sxn:emissions\_diurnal].

``NE22dVARS``: Number of horizontal 2D types.

``E22dSCALE(IDBLK,JDBLK,MPBLK,NE22dVARS)``: 2D scalings.

``E2L2dTBL(NE22dVARS)``: Keep track of 2D scalings used.

``E2vertTBL(ETPAR)``: Table mapping 2D emissions to vertical
distributions.

``NE2vertVARS``: Number of vertical distributions.

``NE2vertLVS``: Number of vertical levels for the distributions.

``E2vertSCALE(NE2vertLVS,NE2vertVARS)``: The vertical scalings.

Forest fires variables:

``FF_TYPE``: Type of forest fires emissions.

``FF_YEAR``: Year of forest fires dataset.

``FF_PATH``: Path of forest fires, set in *Ltracer\_emis\_xxxx.inp*.

``NEFIR``: Number of components with forest fires emissions. Must not be
larger than ``EPAR_FIR``.

``EPAR_FIR``: Max number of components with forest fires emissions.

``EPAR_FIR_LM``: Vertical CTM layers covered.

``ECOMP_FIR(EPAR_FIR)``: Components emitted.

``EMIS_FIR``: The forest fires emission array of size
``(EPAR_FIR_LM,EPAR_FIR,IDBLK,JDBLK,MPBLK)``.

Global diagnose variables:

``DIAGEMIS_IJ``: Diagnose emissions accumulated (kg) over time span
defined by budget calendar (``JDO_T``).

``emisTotalsDaily(NPAR,366)``: Diagnose daily accumulated emissions
(kg).

``emisTotalsOld(NPAR)``: Used for the daily accumulated emissions
diagnose.

``CONVWASHOUT(LPAR,NPAR,IDBLK,JDBLK,MPBLK)``: Diagnose tracer removed by
convective scavenging (kg), accumulated over time span defined by budget
calendar (``JDO_T``).

``dobson_snapshot(IPAR,JPAR,24)``: Snapshots of total O\ :math:`_3`
column each hour (DU).

``dobson_snapshot_ts(IPAR,JPAR,24)``: Snapshots of tropospheric
O\ :math:`_3` column each hour (DU).

``TEMPAVG``: Temperature average, accumulated over time span defined by
budget calendar (``JDO_T``).

``H2OAVG``: H\ :math:`_2`\ O average, accumulated over time span defined
by budget calendar (``JDO_T``).

``QAVG``: Specific humidity average, accumulated over time span defined
by budget calendar (``JDO_T``).

``AMAVG``: Air density average, accumulated over time span defined by
budget calendar (``JDO_T``).

``LMTROPAVG``: Average level of tropopause height (``LMTROP``),
accumulated over time span defined by budget calendar (``JDO_T``).

``SCAV_LS``: Accumulated amount (kg) removed by large scale scavenging.

``SCAV_CN``: Accumulated amount (kg) removed by convective scavenging.

``SCAV_DD``: Accumulated amount (kg) removed by dry deposition.

``SCAV_BRD``: Accumulated burden to be used for calculating average
burden.

``SCAV_DIAG(NPAR,4,366)``: Daily accumulated values. Last entry is
average burden.

``SCAV_MAP_WLS``: Total (kg) scavenged by large scale scavenging at the
surface.

``SCAV_MAP_WCN``: Total (kg) scavenged by convective scavenging at the
surface.

``SCAV_MAP_DRY``: Total (kg) scavenged by dry deposition at the surface.

Help variables:

``DINM``: Number of days in month, changes depending on leap year or
not.

``ZEROINIT``: Flag to keep track of initialised components.

``XZEROINIT``: Flag to keep track of initialised non-transported
components.

``RESULTDIR``: Can be used to put results in a specified directory.

``dustbinsradii``: Radius of dust bins. Only set if mineral dust module
is included.

*aerosols2fastjx.f90*
~~~~~~~~~~~~~~~~~~~~~

[app:jvaer] Routine for handling tropospheric aerosols in fast-JX.

``initialize_tropaerosols``: Initialize arrays.

``set_aer4fjx``: Calculate the path of each aerosols, to be used in
fast-JX calculations.

``get_tropaerosols``: Reads monthly model climatology for specified
aerosols. *Not fully implemented*.

``update_tropaerosols``: Sets the climatological values of aerosols,
interpolates temporally between two monthly climatologies.

``set_aer4fjx_ctm2``: Sets simple BC profile as in CTM2.

*bcoc\_oslo.f90*
~~~~~~~~~~~~~~~~

Routines for treating the BCOM module (Section [sxn:bcoc]).

``bcoc_init``: Initializes the BCOM; indices are set and dry deposition
is set.

``bcoc_master``: Master routine for calculating BCOC. Loops through
IJ-blocks and their columns and integrates aerosol masses using
``QSSA``.

``bcoc_setdrydep``: Puts deposition rates into ``VDEP``. It is called
from the routine ``setdrydep``. Stability is treated in the latter after
``VDEP`` has been set.

``bcoc_chetinit``: Initialize latitude dependent aging times.

``bcsnow_init``: Initialize BCsnow.

``bcsnow_diagwetrm``: Diagnose BC removed by wet scavenging and
deposited on snow.

``bcsnow_save_restart``: Save restart file for BCsnow diagnostics.

``bcsnow_status``: Print status of BCsnow.

``bcsnow_check_snow`` (private): Debug routine for BCsnow.

``bcsnow_getspringsummer`` (private): Get dates for spring and summer to
be used in melting calculations.

``bcsnow_nmet_output``: Puts BCsnow data to file every NMET.

``bcsnow_nmet_output_nc``: Puts BCsnow data to netCDF file every NMET.

``bcsnow_collect_ij`` (private): Collects diagnosed amount of BC
deposited on snow, from wet scavenging and dry deposition, thus building
snow layers.

``bcsnow_meltevap_ij`` (private): Calculates evaporation.

``bcsnow_seaice_ij`` (private): Corrects calculations over sea ice.

``bcsnow_adjustment_ij`` (private): Adjusts calculated snow depth to
snow depth from meteorological data.

``bcsnow_master``: BCsnow master routine.

*caribic2.f90*
~~~~~~~~~~~~~~

Routines for producing vertical profiles at CARIBIC measurement
locations and times. CARIBIC input is available from 2005. Called from
``nops_diag`` in *diagnostics\_general.f90*.

``get_new_events`` (private): Find observations for this day.

``caribic_output`` (private): Produces output for the given NOPS and the
previous NOPS.

``caribic_data_to_file`` (private): Write collected flight path data to
file.

``caribic2_master``: Process profiles. Called outside parallel region.

*ch4routines.f90*
~~~~~~~~~~~~~~~~~

[app:ch4routines] Routines for treating CH\ :math:`_4` as fixed at
surface or as emissions. To choose type of constant surface data, use
``CH4TYPE`` at the top of this file. To use emissions, set flag
``METHANEMIS`` in *cmn\_oslo.f90*.

``update_ch4surface``: Updates surface mixing ratios. This is mainly
done from .

``ch4surface_hymn`` (private): Reads surface data from HYMN, given for
years 2003-2005. This is standard.

``ch4surface_scale_hymn`` (private): Scales HYMN 2003 dataset to marine
global annual CH\ :math:`_4` observed by ESRL Global Monitoring Division
http://www.esrl.noaa.gov/gmd/ccgg/trends\_ch4/

``ch4surface_poet`` (private): Reads surface data from POET.

``ch4surface_retro`` (private): Reads surface data from RETRO.

``READCH4`` (private): Carries out the actual read-in for POET data.

``set_ch4_stt``: Sets 3D CH\ :math:`_4`, i.e. in the ``STT`` array, to
values found in HYMN.

``updateSOILUPTAKEbousquet``: Updates soil uptake each month, according
to the Bousquet data.

``read_ch4sfc4soiluptake`` (private): Reads HYMN surface data [kg], to
convert Bousquet data from [kg/s] to [1/s].

``read_ch4bousquet`` (private): Reads the Bousquet data. Can read any
year in dataset, but standard is to read 2003, matching the HYMN surface
data for 2003.

``ch4drydep_bousquet``: Calculates the dry deposition velocity of
CH\ :math:`_4` to be used in Oslo CTM3.

``reportsfcch4``: Reports global average surface mixing ratio of
CH\ :math:`_4`.

``setch4sfc``: Set CH\ :math:`_4` at surface to keep it constant.

*chem\_oslo.f90*
~~~~~~~~~~~~~~~~

This is where Oslo chemistry master routine will be located in the
future. For now tropospheric chemistry is in *tropchem\_oslo.f90* and
stratospheric chemistry is in *stratchem\_oslo.f90*.

*chem\_oslo\_rates.f90*
~~~~~~~~~~~~~~~~~~~~~~~

Routines for setting up chemistry reaction rates.

``TCRATE_CONST2``: Constant rates and rates depending only on
temperature. The latter are stored in intervals of 1K. Rates used in the
troposphere are listed first, then the rates only used in the
stratosphere are listed.

``set_pr42het``: Sets aerosol surface uptake reaction for hydrolysis of
N\ :math:`_2`\ O\ :math:`_5` in the troposphere. This aerosol
climatology is an annual mean height-latitude distribution from
:raw-latex:`\citep{DentenerCrutzen1993}`, and should be revised.

``getRQAER`` (private): Find removal rate by aerosol (``RQAER``), only
dependent upon land/sea and vertical variation. Crude approximation,
should be revised.

``TCRATE_TP_IJ_TRP``: Rates dependent on temperature and pressure in
troposphere.

``TCRATE_TP_IJ_STR``: Stratospheric rates dependent on temperature and
pressure.

``TCRATE_onAER``: Gaseous uptake and heterogeneous chemistry on
aerosols. Includes new and old parameterisations. Tropospheric
chemistry.

``TCRATE_HET_IJ``: Heterogeneous chemistry reactions in the
stratosphere.

*cnv\_oslo.f90*
~~~~~~~~~~~~~~~

Oslo wet removal due to convective rain is treated in two steps:

Calculating the fraction of convective rain to cloud water in the
elevator (``QFRAC``).

Calculating the fraction of tracer in cloud water (``DISSOLVEDFRAC``).

See Section [sxn:transport\_conv] for details. The subroutines in this
file are:

``elevator_fractions``: Calculates ``QFRAC`` and ``LW_VOLCONC``. Called
from ``CONVW_OC``.

``liquid_fractions``: Calculates ``DISSOLVEDFRAC`` and returns
``CNV_WETL``. Called from ``CONVW_OC``.

``wf_henry`` (private): Calculates Henry coefficients, possibly modified
by hard coding.

*dateconv.f90*
~~~~~~~~~~~~~~

Routines for date conversion, used e.g. by *caribic2.f90*.

``itau2idate``: Calculate date from given time in seconds relative to
reference year.

``idate2itau``: Calculate time in seconds since 1 Jan 1995.

``julianday``: Calculate julian day from given date.

``calendardate``: Calculate date from given julian day.

*drydeposition\_oslo.f90*
~~~~~~~~~~~~~~~~~~~~~~~~~

[app:drydepositionoslo] Sets up dry deposition velocities.

``drydepinit``: Reads *drydep.ctm* from directory *Input\_CTM3*.

``update_drydepvariables``: Sets up special drydep treatments, e.g. soil
uptake of CH\ :math:`_4` and variables for the new dry deposition
treatment. Called from subroutine ``oc_update_chemistry``.

``setdrydep``: Sets drydeposition velocities each time step, in
subroutine ``oc_main``.

``get_ctm2dep`` (private): Calculate old (CTM2) treatment dry deposition
rates.

``get_vdep2`` (private): Calculate new dry deposition velocities.

``get_STC`` (private): Get monthly mean stomatal conductances.

``get_PARMEAN`` (private): Get monthly mean photosynthetically active
radiation (PAR).

``get_asm24h`` (private): Get 24-hour mean of :math:`a_{sn}` variable
for new dry deposition treatment. I.e. daily mean of the molar ratio of
SO\ :math:`_2` and NH\ :math:`_3`. Uses all ``NOPS`` steps to calculate
daily mean.

*diagnostics\_general.f90*
~~~~~~~~~~~~~~~~~~~~~~~~~~

| [app:diagnostics\_general] Oslo chemistry diagnostics. Scavenging
  diagnostics are located in *diagnostics\_scavenging.f90*
  (Appendix [app:diagnostics\_scavenging]).
| Variables (private):

``o3lim``: O\ :math:`_3` mixing ratio used to find tropopause level
``tp_o3``, based on where O\ :math:`_3` :math:`>` ``o3lim``.

``tp_o3``: From surface upwards, this is the uppermost level where
O\ :math:`_3` :math:`<` ``o3lim``.

``preslim``: Pressure limit for hPa-based tropopause ``tp_hpa``.

``tp_hpa``: From surface upwards, this is the uppermost level where p
:math:`<` 100hPa.

Plus several variables to get running averages of tropospheric OH
concentration and CH\ :math:`_4` lifetime.

Subroutines:

``diag_ohch4n2o_init``: Initializes the lifetime diagnostics.

``init_lifetime`` (private): Initialize CH\ :math:`_4` and
N\ :math:`_2`\ O accumulated losses and burdens.

``du_columns``: Calculate tropospheric and total O\ :math:`_3` columns
each meteorological time step.

``write_ducolumns``: Write the columns to file *dobson\_nmet.nc*.

``init_daily_diag``: Daily initialisations in the beginning of the day.

``daily_diag_output``: Daily output carried out at the end of the day.

``nops_diag``: Diagnoses for each ``NOPS``, before parallel loop.

``mp_diag``: Diagnoses for each IJ-block.

``TBGT_2FILE``: Processes 2D tendency budgets and dumps to file.

``REPORTS_CHEMISTRY``: Print out different kinds of reports.

``sumup_burden_and_lifetimes``: Sum up OH burden and CH\ :math:`_4`
lifetime for different tropopause definitions, for a given IJ-block.

``report_burden_and_lifetime`` (private): Print out the burdens and
lifetimes.

``diag_burden_snapshot`` (private): Print out burden at specific times.

``write_snapshot``: Writes out snapshots of selected species/metdata for
hemisphere or global. Mass space is put out.

``write_snapshot_the``: Writes out snapshots of selected species on
potential temperature surfaces. Put out as mixing ratio. See
Section [sxn:snapshot\_theta].

``ch4n2o_burden2``: Sums up burden of CH\ :math:`_4` and
N\ :math:`_2`\ O.

``ch4_loss3``: Sums up monthly chemical loss of CH\ :math:`_4`.

``n2o_loss3``: Sums up monthly chemical loss of N\ :math:`_2`\ O.

``ocdiags_tpset``: Sets tropopause for burden and lifetime diagnostics.

``report_negO3``: Print out the number of negative O3 reported by
tropospheric chemistry routine.

``tnd_emis_daily`` (private): Saves accumulated emissions for each
species per day, in array ``emisTotalsDaily``.

``tnd_emis2file``: Writes the daily emission totals to file
*emis\_daily\_totals\_YYYY.nc*. Writing follows the budget calendar, and
is called from . Also writes accumulated emissions in 3D following the
tendency calendar, to file
*emis\_accumulated\_3d\_YYYYMMDD\_YYYYMMDD.nc*.

``save_chemPL``: Accumulate chemistry losses, convert to units [kg].

``chembud_output``: Write chemistry budgets to file. Burden, CHEMLOSS
and CHEMPROD, all in units kg/gridbox. Only for selected species. Also
see Section [sxn:chemprodloss].

*diagnostics\_scavenging.f90*
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

[app:diagnostics\_scavenging] Routines for diagnosing dry and wet
scavenging.

``scav_diag_init``: Initialises separate diagnostics for wet scavenging
and dry deposition.

``scav_diag_put_ddep``: Accumulates kg of tracer deposited by dry
deposition.

``scav_diag_brd``: Accumulates burden of tracers each meteorological
time step to generate averages.

``scav_diag_ls``: Diagnose large scale scavenging.

``scav_diag_cn``: Diagnose convective scavenging.

``scav_diag_collect_daily``: Collect daily scavenged tracer masses and
also the average tracer burden.

``scav_diag_2fileA``: Write daily totals of scavenged tracer, plus their
burdens, to file *scavenging\_daily\_totals\_YYYY.nc*. Scavenging
includes wet removal (large scale and convective) and dry deposition.

``scav_diag_2fileB``: Write 2D daily totals of scavenged tracer
(i.e. maps) to file *scavenging\_daily\_2d\_YYYYMMDD.nc*. Scavenging
includes wet removal (large scale and convective) and dry deposition.

*dust\_oslo.f90*
~~~~~~~~~~~~~~~~

Routines for treating the DUST module (Section [sxn:dust]).

``dust_init``: Initializes the DUST.

``dustinput_met``: Special input for dust calculations.

``SWradbdg_get`` (private): calculate approximate solar radiation in and
out at the surface. Not used when available from meteorological data.

``psadjust`` (private): Adjust Ps, account for advection.

``dust_globalupdate``: Update global DUST parameters.

``oro_set`` (private): Set orography.

``plevs0`` (private): Get pressure, temperature and height properties
for DUST.

``dust_master``: Master routine for calculating DUST. Calls column
solver for DUST.

``dust_column`` (private): Column treatment of DUST; calls DEAD routines
to solve DUST in the column.

``dustbdg2d``: Writes dust budget to file.

``dist_set_ssrd``: Sets meteorological variable ``SSRD`` in dust code.

``dist_set_strd``: Sets meteorological variable ``STRD`` in dust code.

``dist_set_SWVL1``: Sets meteorological variable ``SWVL1`` in dust code.

``dustbdg2file``: Puts out dust budget to netCDF file.

*emisdep4chem\_oslo.f90*
~~~~~~~~~~~~~~~~~~~~~~~~

[app:emisdep4chem\_oslo] Routines to treat get emissions and deposition
values and convert them for use in chemistry as production and loss
terms, respectively.

``emis4chem``: Routine similar to the core ``SOURCE``, fetching all
emissions from the emissions array (as kg/s). The output array is
``EMIS_IJ(LPAR,NPAR,IDBLK,JDBLK,MPBLK)``.

``getEMISX``: Fetches the column emissions for all chemical IDs,
converts from kg/s to molec/(cm\ :math:`^3`\ s) and puts them into
``EMISX(TRACER_ID_MAX,LPAR)``. Called from chemistry master routine
where column values are retrieved.

``getVDEP_oslo``: Converts ``VDEP(NPAR,IPAR,JPAR)`` [m/s] to
``VDEP_OSLO(TRACER_ID_MAX)`` [1/s] to use as loss term in chemistry. See
Section [sxn:drydep] for more.

*emissions\_aircraft.f90*
~~~~~~~~~~~~~~~~~~~~~~~~~

Routines for reading and updating aircraft emissions. Possible datasets
cover TradeOff, React4C and Quantify. In the *Ltracer\_emis\_xxxx.inp*,
you specify ``AirScen`` to define the dataset, and ``AirEmisPath`` to
define the path to where aircraft emissions are in general.

Emissions are interpolated to the model resolution. Old routines are
also available for having the possibility to read old resolution
specific emission data. Emissions are read into a separate array, which
is used either in ``SOURCE`` or in ``emis4chem``.

``aircraft_emis_master``: Master routine for treating aircraft
emissions.

``aircraft_set_species``: Defines which species to emit.

``aircraft_emis_update`` (private): Read data from file.

``aircraft_emis_vinterp`` (private): Vertical interpolation.

``ac_interp`` (private): Interpolation routine used
by\ ``aircraft_emis_vinterp``.

``read_original_res`` (private): Read original (e.g. 1x1degree) data.

``read_tradeoff_original`` (private): Read original TradeOff data.

``read_react4c_original`` (private): Read original React4C data.

``read_quantify_original`` (private): Read Quantify data.

``readmass_qfyAIR_month`` (private): Routine to do the netCDF reading of
Quantify data, called by ``read_quantify_original``.

``read_ceds_original``: (private): Read CEDS data.

``get_xyedges`` (private): Get grid box info for interpolation.

``aircraft_zero_400hpa`` (private): Below 400hPa, zero out the aircraft
tracers (H:math:`_2`\ O). This is done by assuming 1hour lifetime.
Setting to zero may cause negative tracers due to moments not being
zeroed.

It should be possible to include a diurnal variation for aircraft
emissions, but it has not been implemented yet.

*emissions\_megan.f90*
~~~~~~~~~~~~~~~~~~~~~~

Contains routines for calculating MEGANv2.10 emissions
:raw-latex:`\citep{GuentherEA2012}`. Oslo CTM3 routines are:

``megan_report``: reports daily totals of MEGAN. Should not be included
as default.

``add_meganBiogenic``: Adds MEGAN emissions to species in Oslo CTM3.

``megan_get_co2``: Reads file to get monthly CO\ :math:`_2` for isoprene
activity factor.

``megan_input``: Reads input table *tables/megan\_tables.dat*.

``megan_update_metdata``: Updates meteorological data needed in MEGAN
(e.g. accumulated rain). Also sets monthly CO2.

``megan_emis``: Main routine for calculating emissions.

``getWP``: Get wilting point for soil.

``megan_emis2file``: Puts emitted amounts to file following the BUDGETS
calendar. See Section [sxn:megan\_emis2file].

MEGAN routines adjusted to Oslo CTM3:

``GAMMA_AGE``: Calculate leaf age activity.

``GAMMA_CANOPY``: Calculate activity from canopy.

``DIstomata``: Calculate drought-induced effect on stomata (range 0–1)
based on the Palmer Severity Drought Index. Note that MEGAN sets the
drought index to zero as default.

``Ea1t99``: Temperature dependence activity factor for emission type 1
(e.g. isoprene, MBO).

``Ealti99``: Calculate light independent algorithms.

``Ea1p99``: Not sure what this does.

``WaterVapPres``: Convert water mixing ratio (kg/kg) to water vapor
pressure.

``Stability``: Temperature lapse rate in canopy, using canopy
characteristics and solar input.

``GaussianIntegration``: Gaussian distribution.

``SolarFractions``: Calculate transmission, the fraction of
photosynthetic photon flux density (PPFD) that is diffuse, and fraction
of solar rad that is PPFD.

``WeightSLW``: Calculates a Gaussian weighting function for LAI in the
canopy layers.

``CanopyRad``: Canopy light environment model.

``CalcExtCoeff``: Calculates extinction coefficient.

``CalcRadComponents``: Radiative calculations.

``CanopyEB``: Canopy energy balance model for estimating leaf
temperature.

``LeafEB``: Leaf energy balance.

``ConvertHumidityPa2kgm3``: Convert Pa to kg/m\ :math:`^3`.

``ResSC``: Leaf stomatal resistance. Not well referenced.

``LeafIROut``: IR thermal radiation energy output by leaf.

``LHV``: Calculate latent heat of vaporisation from
:raw-latex:`\citet{Stull1988}`, p.641.

``LeafLE``: Latent energy term in energy balance.

``LeafBLC``: Boundary layer conductance.

``LeafH``: Convective energy term in energy balance (W/m:math:`^2` heat
flux from both sides of leaf).

``SvdTk``: Calculate saturation vapor density.

``CalcEccentricity``: Calculate eccentricity.

``UnexposedLeafIRin``: Calculate IR into leaf that is not exposed to the
sky.

``ExposedLeafIRin``: Calculate IR into leaf that is exposed to the sky.

``SOILNOX``: Calculation of soil NOx based on
:raw-latex:`\citet{YiengerLevy1995}`.

``FERTLZ_ADJ``: Computes fertilizer adjustment factor for soil NOx.

``GROWSEASON``: Computes day of growing season for soil NOx.

``prec_adj``: Calculates precipitation pulse adjustment factor for soil
NOx.

``getPulseType``: Computes the pulse type from a rainfall rate
:raw-latex:`\citep{YiengerLevy1995}`.

``precipfact``: Computes a precipitation adjustment factor.

*emissions\_ocean.f90*
~~~~~~~~~~~~~~~~~~~~~~

Routines for treating emissions from ocean. Also sea spray routines will
be placed here eventually.

``emissions_ocean_organiccarbon_init``: Initialise variables for using
oceanic emissions.

``emissions_ocean_getChlA``: Fetches chlorophyll A for specified month.
Uses MODIS chlorophyll A, which has been binned into 0.5x0.5 degree
grid. Uses meteorological year for years 2003–2012, and otherwise a
climatology of those years.

``emissions_ocean_organiccarbon``: Calculate emissions of oceanic
organic carbon aerosols (POA), following
:raw-latex:`\citet{GanttEA2015}`. If sea salt module is included, use
sea salt production rate, otherwise calculate it separately (slightly
different method for now). This routine only depend on meteorological
variables, and is called from ``update_emis_ii``
(*emissions\_oslo.f90*).

``emissions_ocean_total``: Print out current total POA flux as [Tg/yr].

``add_oceanOCemis``: Routine to add oceanic carbon emissions. Called
from either ``emis4chem_oslo`` or ``SOURCE``.

*emissions\_oslo.f90*
~~~~~~~~~~~~~~~~~~~~~

[app:emissions\_oslo] Routine for reading in emissions, and master
routine for short term variations.

``emis_input``: Reads in emission data based on
*Ltracer\_emis\_xxxx.inp*.

``update_emis``: Updates short term variations/emissions globally.

``update_emis_ij``: Update short term emissions in an IJ-block.

``emis_prop`` (private): Set properties for input data.

``emis_unit`` (private): Unit conversions for datasets.

``emis_check2dscal`` (private): Check 2D scaling flag and keep track of
which ones are used.

*emissions\_volcanoes.f90*
~~~~~~~~~~~~~~~~~~~~~~~~~~

Variables and routines for volcanic emissions.

Variables:

``volc_emis_so2``: Emissions of SO\ :math:`_2` for each event.

``volc_elev``: Elevation of the volcano.

``volc_cch``: Cloud column height, i.e. the height of the cloud rising
from the volcano. When equal to the elevation, the volcano is
non-eruptive, i.e. gassing from the surface.

``volc_ii``: IJ-array zonal index of event.

``volc_jj``: IJ-array meridional index of event.

``volc_mp``: IJ-array number.

``volc_event_start(31,12)``: Keeps track of first event for each day,
through the year.

``volc_event_end(31,12)``: Keeps track of last event for each day,
through the year.

Subroutines:

``init_volcPATH``: Initialize file path and year for volcanic emissions.

``read_volcEMIS``: Master routine for reading volcanic emissions.

``read_volcEMIS_HTAP``: Reads HTAP volcanic emissions (1979–2010).

``read_volcEMIS_ACOM``: Reads AEROCOM volcanic emissions.

``add_volcEMIS``: Adds volcanic SO\ :math:`_2` emissions to emission
array.

*emisutils\_oslo.f90*
~~~~~~~~~~~~~~~~~~~~~

Routines used by *emissions\_oslo.f90*. When including new types of
emissions/datasets, new read-in routines should be placed here.

``reademis_2d``: Controls the read-in of 2D emission data and its
interpolations. This is where you will find info on the 2D format codes
used in the emission file *Ltracer\_emis\_xxxx.inp*.

``reademis_3d``: Controls the read-in of 3D emission data and its
interpolations. This is where you will find info on the 3D format codes
used in the emission file *Ltracer\_emis\_xxxx.inp*.

``reademis_stv``: Controls the read-in and interpolation of 2D fields
used for short term variations (see Section [sxn:emissions\_stv]). This
is where you will find info on the STV format codes used in the emission
file *Ltracer\_emis\_xxxx.inp*.

``emisinterp2d`` (private): Interpolates 2D field and designates dataset
numbers.

``emis_setscalings``: Set scaling for tracers using different datasets.

``scale_area`` (private): Scales with dataset by grid box area.

``get_xyedges``: Calculate grid box properties.

``set_diurnal_scalings``: Sets simple diurnal scalings. These are
e.g. from TNO/RETRO or such as 50% up in daytime and 50% down during
night.

``emis_setscaling_2dfields``: Reads scaling data to use for 2D scaling,
e.g. daylight temperatures or heating-degree-days.

``emis_diag``: Accumulates the emissions of each species into
``DIAGEMIS_IJ``.

``ctm2_rdmolec2`` (private): Reads CTM2 POET format.

``iiasa_read2d`` (private): Reads IIASA 2D data as in CTM2.

``gfed_read3d``: Reads GFED 3D data. Not used in the GFED emissions now;
this routine was used to read monthly emissions into the core emission
array.

``volc_read1`` (private): Reads volcanic emissions.

``read_retrobbh`` (private): Reads the vertical distribution of forest
fires.

``read_bcoc_bond_2d``: Reads Tami Bond data version 4 and 5.

``read_aerocom_2d``: Reads ascii data from AEROCOM.

``gfed4_rd``: Reads monthly GFEDv4 data along with partitioning.

``gfed4_init``: Reads emission factors from GFEDv4.

``gfed4_rd_novert``: Reads monthly GFEDv4 emissions for distributing
according to boundary layer height.

``gfed4_rd_novert_daily``: Reads monthly GFEDv4 emissions along with
daily fraction of emissions per month.

*eqsam\_v03d.f90*
~~~~~~~~~~~~~~~~~

Subroutine ``eqsam_v03d_sub`` is the main nitrate (eqsam) driver, based
on :raw-latex:`\citet{MetzgerEA2002}`.

*fallingaerosols.f90*
~~~~~~~~~~~~~~~~~~~~~

Contains routines for calculating gravitational settling of aerosol
using second order moments transport. Not used yet.

``aerosolsettling``: Master routine.

``readkasten1968``: Reads aerosol fall speeds of
:raw-latex:`\citet{Kasten1968}`.

``getGAMAAER``: The vertical velocity of particles are found based on
their radius. Assumes fixed radius for the whole IJ-block.

``GRAV_SETN``: Routine for SOM transport of a tracer downwards.

``fallspeed``: Purpose: Given size and density of particle, and column
thermodynamic profile, compute terminal fall speed. Based on DEAD/DUST
code.

``turbfallspeed``: Purpose: Given size and density of particle, and
column thermodynamic profile, compute Based on DEAD/DUST code.

``getGAMAAER_NS``: Same as ``getGAMAAER`` but handles different radii
and vertical properties.

*gmdump3hrs.f90*
~~~~~~~~~~~~~~~~

Routine for dumping selected components to netCDF file every 3 hours.
Turn on this diagnose by setting its flag ``LDUMP3HRS=.true.``.

``dump3hrs``: Dumps the components.

``gm_dump_nc`` (private): Does the writing to netCDF files. Converts
from kg/gridbox to kg/m\ :math:`^3`.

*hippo.f90*
~~~~~~~~~~~

Routines for producing vertical profiles at HIPPO measurement locations
and times. Called from ``nops_diag`` in *diagnostics\_general.f90*.

``get_new_events`` (private): Find observations for this day.

``hippo_output`` (private): Produces output for the given NOPS and the
previous NOPS.

``hippo_data_to_file`` private): Write collected flight path data to
file.

``hippo_master``: Process profiles. Called outside parallel region.

*input\_oslo.f90*
~~~~~~~~~~~~~~~~~

Subroutines to initialize the Oslo CTM3.

``clear_oslo_variables``: Clear arrays used by chemistry
(e.g. ``trsp_idx``). Called by routine ``INPUT``.

``init_oslo``: Initialize Oslo chemistry. Tracer related data is read,
and constant chemical reaction rates are set. Dry deposition parameters
are read and some diagnostics are also initialized.

``info_oslo`` (private): Print out info about Oslo chemistry.

``info_qssa`` (private): Print out info about QSSA solver.

``read_tracer_list`` (private): Read tracer list, initialize Oslo
chemistry.

``tracer_specific_input`` (private): Read in the tracer-specific run
instructions and data sets. Based on UCI ``CHEM_IN``, but set up for
Oslo chemistry.

``set_resultdir`` (private): Sets a result directory if defined.

``read_lsmask``: Reads the land-sea mask ``LSMASK``.

*main\_oslo.f90*
~~~~~~~~~~~~~~~~

[app:main\_oslo] The ``module main_oslo`` comprises the master
subroutine for chemistry and aerosol packages.

``master_oslo``: Master routine to control Oslo chemistry. Processes for
each IJ-block is called separately.

``jvalues_oslo`` (private): Collects J-values in the IJ-block.

``jv_column`` (private): Sets photolysis rates in the column. Based on
UCI subroutine ``PHOTOL``.

``update_chemistry``: Update chemistry, e.g. boundary conditions.

*ncutils.f90*
~~~~~~~~~~~~~

Contains netCDF utilities.

``readnc_3d_from4d``: Read netCDF file containing 4D fields
(DIM1,DIM2,DIM3,DIM4), and returns 3D-field for a DIM4 entry.

``readnc_2d_from3d``: Read netCDF file containing 3D fields
(DIM1,DIM2,DIM3), and returns 2D-field for a DIM3 entry.

``readnc_1d``: Opens a netCDF file and returns a 1D field of specified
size from the file.

``readnc_1d_from2d``: Opens a netCDF file and extracts a 1D from a 2D
field on file. Specifying an entry of DIM2, a 1D array is returned from
the 2D array (DIM1,DIM2).

``get_netcdf_var_1d``: Reads 1D variable from netCDF file. Array is
allocated in this routine, and must be deallocated after use.

``get_netcdf_var_2d``: Reads 2D variable from netCDF file. Array is
allocated in this routine, and must be deallocated after use.

``get_netcdf_var_3d``: Reads 3D variable from netCDF file. Array is
allocated in this routine, and must be deallocated after use.

``get_netcdf_var_4d``: Reads 4D variable from netCDF file. Array is
allocated in this routine, and must be deallocated after use.

``get_netcdf_att_char``: Reads attribute data for variable from netCDF
file.

``get_netcdf_var_dims``: Reads dimensions of variable from netCDF file.

``get_netcdf_r4var_1d_from_2d``: Returns single precision (``r4``) 1D
array from a 2D netCDF field. Used for reading meteorological data on
netCDF format.

``get_netcdf_r4var_2d_from_3d``: Returns single precision (``r4``) 2D
array from a 3D netCDF field. Used for reading meteorological data on
netCDF format.

``get_netcdf_r4var_3d_from_4d``: Returns single precision (``r4``) 3D
array from a 4D netCDF field. Used for reading meteorological data on
netCDF format.

``handle_err``: Handle netCDF errors. This routine does not print out
much helpful information.

``handle_error``: Handle netCDF errors slightly better than
``handle_err``.

*nitrate.f90*
~~~~~~~~~~~~~

Nitrate routines. Note that the nitrate equilibrium model is located in
its own file *eqsam\_v03d.f90*.

``nitrate_init``: Initialise nitrate module.

``nitrate_master``: Nitrate master routine.

*pchemc\_ij.f90*
~~~~~~~~~~~~~~~~

[app:oc\_pchemc] The tropospheric 1D chemical integrator, integrating
the chemistry in the tropospheric column.

The Oslo chemistry was originally set up to use emissions as production
terms and deposition as loss terms. This is still default, and is
controlled by the user settings in *Makefile* (Section [sxn:makefile]).
Note that treating them in chemistry, the lightning and aircraft
emissions are also included in the emission array. See
Section [app:emisdep4chem\_oslo] for more.

*pchemc\_str\_ij.f90*
~~~~~~~~~~~~~~~~~~~~~

[app:oc\_pchemc\_str] The stratospheric 1D chemical integrator,
integrating the chemistry in the stratospheric column.

*physics\_oslo.f90*
~~~~~~~~~~~~~~~~~~~

[app:physics\_oslo] The ``module physics_oslo`` contains subroutines for
doing physical calculations in the Oslo CTM3. Parameters and variables
are

``NTHE``: number of theta levels to calculate equivalent latitudes.

``pvthe``: Potential vorticity on the theta levels.

``theqlat``: Equivalent latitude on the theta levels.

The subroutines are

``update_physics``: Updates physical variables such as tropopause level.

``defineTP``: Defines the tropopause level, based on the parameter
``TP_TYPE=1`` in *cmn\_oslo.f90*. Default for the Oslo CTM3 is
a PVU-based tropopause:

``tp_pvu_ij`` (private): Calculates the tropopause level from PVU and
potential temperature (``TP_TYPE=1``).

``tp_dtdz_ij`` (private): Not tested. Calculate the tropopause level
using lapse rate (``TP_TYPE=2``).

``tp_e90_ij`` (private): Not tested. Uses tropopause level from E90
tracer, i.e. as given in ``LPAUZTOP`` (``TP_TYPE=3``).

``tp_03_150`` (private): Not in use. Tropopause based on where
O\ :math:`_3` < 150ppbv.

``get_pvu``: If the meteorological data do not provide PV, it may be
calculated from the winds in this routine.

``ijlw2lij``: Converts a field of size (``IPARW,JPARW,LPARW``) to the
possibly degraded and vertically collapsed (``LPAR,IPAR,JPAR``).

``theta_pv``: Interpolate PV to pre-defined theta levels.

``theta_eqlat``: Calculates equivalent latitudes for pre-defined theta
levels.

``IJLfield2ThetaLvs``: Interpolates standard IJL-field to
:math:`\theta`-levels.

``metdata_ij``: Sets arrays of air molecular density ``AIRMOLEC_IJ`` and
box volume ``DV_IJ``. They are used in converting to concentration.

``check_lmtrop`` (private): If E90 tracer needs to be initialised,
``LMTROP`` must be defined. If ``LMTROP`` is calculated from
E90-tropopause, it must be set in another fashion, and for this the PVU
tropopause is used. Only carried out at the first time step.

*psc\_microphysics.f90*
~~~~~~~~~~~~~~~~~~~~~~~

Variables and routines to calculate formation and evolution of PSCs.
Some important variables:

``N_B``: The number of PSC particle bins. Originally set to 40, but
since early 2018 the default is 15 bins to save computing time
(sedimentation is done for each bin). Using 15 bins covering the same
radius range only changes total O\ :math:`_3` column by up to 0.2%, also
in O\ :math:`_3` hole conditions (year 2016 have been tested). If you
model :math:`O_3` loss specifically, you might want to consider
increasing :math:`N_B`.

``LAEROSOL``: Flag to include aerosol heterogeneous chemistry (in
stratosphere).

``LPSC``: Flag to include PSC1 heterogeneous chemistry.

``LPSC2``: Flag to include PSC2 heterogeneous chemistry. Requires
``LPSC=.true.``.

``PSC1``: PSC1 surface area density.

``PSC2``: PSC2 surface area density.

``VOLA``: Volume of frozen aerosols.

``SAT``: Flags when STS have frozen to NAT.

``SPS_PARTAREA``: Surface area density of aerosols after PSC
modification

And subroutines:

``oslochem_psc``: Master routine.

``get_psc12_sad``: Get column values of PSC1, PSC2 and SAD.

``get_mw``: Fetches coefficients for mass to concentration conversion.

``PSC_1d_07``: Column model for PSC surface area calculation.

``CARS``: Carslaw routine.

``H2O_SAT``: Calculate saturation concentration of H\ :math:`_2`\ O.

``WSED``: Get falling speed of aerosols.

``sedimentation``: Sedimentation routine.

``SED``: Old sedimentation routine

``AM_BIN``: Calculate H\ :math:`_2`\ O in binary solution.

``X_CARS``: Used by ``AM_BIN``.

``HENRIC``: Calculate Henry’s law constants.

``RHO_S_CAR``: Calculate density of H\ :math:`_2`\ SO\ :math:`_4` in
binary solution.

``RHO_N_CAR``: Calculate density of HNO\ :math:`_3` in binary solution.

``LOGN``: Get log-normal distribution.

``MOM``: Calculate moments of distribution.

``DIS_LN``: Routine for calling ``LOGN`` and ``MOM``.

``sps_SURF``: Calculate H\ :math:`_2`\ SO\ :math:`_4` mixing ratio from
aerosol surface area density.

``sps_WSASAS``: Get H\ :math:`_2`\ SO\ :math:`_4` mass fraction in
a solution with plane surface, and in equilibrium with ambient water
vapor pressure.

``sps_ROSAS``: Get density of the sulphuric acid solution.

``PSC_diagnose``: Simple diagnostic of PSCs.

``set_psc_constants``: Initializes the PSC constants.

``getTnat``: Calculate T\ :math:`_{\textrm{NAT}}`, the freezing
temperature of NAT.

*qssa\_integrator.f90*
~~~~~~~~~~~~~~~~~~~~~~

Routine for integrating chemistry.

``qssa``: Integrates chemistry.

``qssastr``: Same as ``qssa``, but allows negative production due to the
treatment of the sum of oxygen (SO).

*satelliteprofiles\_mls.f90*
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

[app:satelliteprofiles\_mls] Routines for diagnosing satellite profile
measurements. Called from ``nops_diag`` in *diagnostics\_general.f90*.

``get_satprofiles_mls`` (private): Get profiles for specified time step.

``initialize_satprofiles_mls`` (private): Initialize.

``satprofs_mls_to_file`` (private): Write to file.

``satprofs_mls_master``: Control the other routines.

*seasalt.f90*
~~~~~~~~~~~~~

[app:seasalt] Routines for treating the SALT module
(Section [sxn:salt]).

``seasalt_init``: Initializes the SALT.

``seasalt_master``: Master routine for calculating SALT.

``dry2wet`` (private): Calculate the density and diameter of wet sea
salt particles.

``growth_factor`` (private): Calculate growth factor.

``falling`` (private): Gravitational settling.

``drydeppart`` (private): Dry deposition.

``addtogether`` (private): Add the parts together.

``saltbdg2file``: Puts out salt budget to netCDF file.

*seasaltprod.f90*
~~~~~~~~~~~~~~~~~

[app:seasaltprod] Contains different sea salt production routines. They
are used by both the SALT application and the BCOC application.

``seasalt_production``: The standard Oslo CTM3 sea salt production,
using small particles as in :raw-latex:`\citet{MonahanEA1986}` as
suggested by :raw-latex:`\citet{GongEA1997}`, and large particles as in
:raw-latex:`\citet{SmithEA1993}`.

``seasalt_production_maartenson03``: Sea salt production following
:raw-latex:`\citet{MartenssonEA2003}`.

``seasalt_production_gantt15``: Sea salt production following
:raw-latex:`\citet{GanttEA2015}`, i.e. parameterisation of
:raw-latex:`\citet{Gong2003}` with sea surface temperature adjustment as
in :raw-latex:`\citet{JaegleEA2011}`.

*soa\_oslo.f90*
~~~~~~~~~~~~~~~

Routines for calculating secondary organic aerosols (SOA).

``soa_init``: Initialise SOA.

``soa_setdrydep``: Set dry deposition for SOA.

``SOA_v9_separate``: Master routine for calculating SOA separation,
i.e. the amount in gas phase and in aerosol phase.

``FINDM0`` (private): Find concentration of total organic aerosol.

``FM0``: Used by ``FINDM0``.

``soa_diag_drydep``: Diagnose SOA drydep.

``soa_diag_separate``: Diagnose changes in SOA due to separation
routine.

``soa_diag2file_nc4``: Writes SOA diagnostics to netCDF4 files, one for
3D for all SOA species (*soa\_budgets\_*), and one for daily global
totals (*soa\_dailybudgets\_YYYY*).

``soa_nopsdiag``: SOA diagnoses each ``NOPS``.

``soa_diag_lsscav``: Diagnose SOA large scale scavenging.

*strat\_aerosols.f90*
~~~~~~~~~~~~~~~~~~~~~

Routines for setting stratospheric aerosols, i.e. the variable
``PARTAREA``. Aerosol dataset was made by Considine at NASA.

``update_strat_backaer``: Update background aerosols by adjusting to
meteorology.

``update_ba``: Update background aerosols. Done every month.

``ctm2_set_partarea``: Adjust satellite data to meteorology, mainly
pressure. Assumes that SAD data are means on geographical latitude
(i.e. not equivalent latitude).

``ctm2_set_partarea5``: If a zonal mean dataset uses equivalent
latitudes, it is possible to interpolate those to the model by using the
model equivalent latitude. This routine tries to do that, but is not in
use.

``meri_interpol``: Interpolate linearly in latitude.

*strat\_h2o.f90*
~~~~~~~~~~~~~~~~

[app:strath2o] Module for H\ :math:`_2`\ O treatment in the
stratosphere. Also sets H\ :math:`_2`\ O in the troposphere.

Variables and parameters:

``sumH2``: Sum of H\ :math:`_2`\ +2CH\ :math:`_4`\ +H\ :math:`_2`\ O in
the stratosphere, as volume mixing ratio.

``LOLD_H2OTREATMENT``: .true. means H\ :math:`_2`\ O is calculated from
the ``sumH2`` (old treatment). .false. means H\ :math:`_2`\ O is
calculated in the chemistry, and requires H\ :math:`_2`,
H\ :math:`_2`\ O and H\ :math:`_2`\ Os to be transported.

``str_h2o``: Array containing stratospheric H\ :math:`_2`\ O when
calculating from the sum.

``d_h2o``: Sedimented ice from PSC microphysics.

Subroutines:

``set_trop_h2o_b4trsp_clim``: Sets tropospheric H\ :math:`_2`\ O values
before transport based on model climatology.

``reset_trop_h2o``: Resets tropospheric H\ :math:`_2`\ O to values from
specific humidity.

``set_strat_h2o_b4chem``: For old treatment, this calculates
H\ :math:`_2`\ O from ``sumH2``.

``set_d_h2o``: Sets ``d_h2o``, called from PSC microphysics.

``strat_h2o_init``: Initialize arrays for H\ :math:`_2`\ O treatment.
Will overwrite a possible stored field if H\ :math:`_2`\ O is
transported.

``strat_h2o_ubc``: Set upper boundary conditions for H\ :math:`_2`\ O.

``strat_h2o_ubc2``: Set upper boundary conditions for H\ :math:`_2`\ O
and H\ :math:`_2` using same mixing ratio as layer below.

``strat_h2o_max``: Report max H\ :math:`_2`\ O in the stratosphere.

``zc_strh2o``: Put H\ :math:`_2`\ O into ``ZC_LOCAL`` before chemistry
if H\ :math:`_2`\ O is not transported.

``set_h2_eurohydros``: Sets H\ :math:`_2` based on monthly means from
the EUROHYDROS project.

``strat_h2o_init_clim``: Initialise tropopause mixing ratio of
H\ :math:`_2` used as climatology.

*strat\_loss.f90*
~~~~~~~~~~~~~~~~~

Subroutines:

``stratloss_oslo``: Stratospheric loss of tracers not handled by
stratospheric chemistry routine.

*strat\_o3noy\_clim.f90*
~~~~~~~~~~~~~~~~~~~~~~~~

[app:strato3clim] When calculating only tropospheric chemistry, the
``module strat_o3noy_clim`` sets up stratospheric O\ :math:`_3`, NOx and
HNO\ :math:`_3`.

``read_o3clim``: Reads netCDF file with climatologies of O\ :math:`_3`,
HNO\ :math:`_3`, PANX, HO\ :math:`_2`\ NO\ :math:`_2`, NO\ :math:`_3`,
N\ :math:`_2`\ O\ :math:`_5`, NO and NO\ :math:`_2`, based on
a Oslo CTM2 T42L60 simulation. Reads monthly mean for current and next
month.

``get_strato3noy_clim``: Main routine for non-parallel region. Called
00UTC each day.

``stratO3_interp`` (private): Interpolates linearly in time between two
monthly means of climatology, called from ``get_strato3noy_clim`` once
each day.

``update_stratO3``: Sets O\ :math:`_3` from time interpolated monthly
mean climatology. Routine works in parallel over IJ-blocks!

``stratNOY_interp`` (private): Interpolates NOy components linearly in
time between two monthly means of climatology, called from
``get_strato3noy_clim`` once each day.

``update_stratNOX`` (private): Sets NOx and HNO\ :math:`_3` similar to
Oslo CTM2. Should **not** be used.

``update_stratNOX2`` (private): Inert stratospheric NOx and
HNO\ :math:`_3` are scaled to the NOx and HNO\ :math:`_3` values used in
Oslo CTM2. Should **not** be used.

``update_stratNOY`` (private): Updates stratospheric NOx and
HNO\ :math:`_3` based on the climatology.

``update_strato3ctm2`` (private): Old CTM2 routine for L40. Updates
stratospheric O\ :math:`_3` in the uppermost layer assuming an influx of
O\ :math:`_3`. HNO\ :math:`_3` and NO are set in the stratospheric
domain, based on O\ :math:`_3`. Should be replaced with an interpolation
to L60 climatology.

*stratchem\_oslo.f90*
~~~~~~~~~~~~~~~~~~~~~

The ``module stratchem_oslo`` sets up the stratospheric chemistry and
calls ``OSLO_CHEM_STR`` located in *pchemc\_str\_ij.f90*.

``oslochem_strat``: Master routine for stratospheric chemistry.

``read_oslo2d2``: Reads original output from Oslo 2D model and
interpolates on the fly. Data are read in ``STT_2D_LT`` for upper layer
(T for top) and ``STT_2D_LB`` for bottom (B).

``update_strat_boundaries``: Updates ``BTT`` from ``STT_2D_LT`` at
surface and ``STT_2D_LB`` at model top.

``set_fam_in_trop``: Sums up stratospheric families in the troposphere.

*sulphur\_oslo.f90*
~~~~~~~~~~~~~~~~~~~

Contains a few variables, and also the ocean concentration of DMS, in
the variable ``DMSseaconc``.

``TCRATE_CONST_S``: Sets constant reaction rates for sulphur chemistry.

``TCRATE_TP_S_IJ``: Calculates heterogeneous reaction rates for sulphur
chemistry.

*troccinox\_xxx.f90*
~~~~~~~~~~~~~~~~~~~~

Routines for producing vertical profiles at TROCCINOX2 measurement
locations and times, from January 2005 to early March 2005. ’xxx’ is
’fal’ for Falcon, ’geo’ for Geophysica and ’ban’ for Bandeirante. These
files are practically identical, and routines are: Called from
``nops_diag`` in *diagnostics\_general.f90*.

``get_new_events``:

``flight_output``:

``flight_data_to_file``:

``troccixxx_master``:

*tropchem\_oslo.f90*
~~~~~~~~~~~~~~~~~~~~

The ``module tropchem_oslo`` takes care of the tropospheric chemistry
calls in the IJ-block. It calls ``OSLO_CHEM`` located in
*pchemc\_ij.f90*.

``oslochem_trop``: Master routine for tropospheric chemistry.

*utilities\_oslo.f90*
~~~~~~~~~~~~~~~~~~~~~

Contains utilities for the Oslo CTM3, mainly related to Oslo chemistry.

``gotoZC_IJ``: Put ``BTT`` into a local column array for all tracers,
indexed by tracer IDs.

``backfromZC_IJ``: Convert local tracer array back to ``BTT``.

``ZC_MASS2CONC``: Convert local tracer array from mass to concentration.

``ZC_CONC2MASS``: Convert local tracer array back to mass.

``troe``: Old routine to calculate reaction rates for three body
reactions.

``rate3b``: Calculate reaction rates for three-body reactions. Used in
both troposphere and stratosphere.

``get_chmcycles``: Find number of internal loops (``CHMCYCLES``) for
chemistry/boundary layer mixing.

``US76_Atmosphere``: Compute properties of the 1976 standard atmosphere.

``h2o_sat``: Calculate saturation concentration of H\ :math:`_2`\ O at
a given temperature.

``source_e90``: Calculate source for e90 tracer.

``decay_e90``: Calculate decay for e90 tracer.

``init_e90``: Initialise e90 tracer.

``tpause_e90``: Find tropopause based on e90 tracer.

``tpauseb_e90``: Find tropopause in IJ-block based on e90 tracer.

``tpauseb_o3``: Find tropopause in IJ-block based on Linoz O\ :math:`_3`
isopleths.

``SZA_PN``: Calculates solar zenith angle and also whether it is polar
night (PN), i.e. when sun does not go above the horizon.

``stringUpCase``: Changes a string to upper case letters (English
alphabet only).

*verticalprofiles\_stations2.f90*
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Routines for diagnosing vertical profiles at stations. Called from
``nops_diag`` in *diagnostics\_general.f90*.

``vprof_stations`` (private): Get profiles for specified time step.

``initialize_stations`` (private): Initialize.

``vprofs_to_file`` (private): Write to file.

``vprofs_master``: Control the other routines.

DUST source code
----------------

The mineral DUST code is located in the directory
*OSLO*\ */DEAD\_COLUMN*.

Oslo dummy files
----------------

When the Oslo CTM3 is compiled without Oslo chemistry, i.e. run in
transport mode only, the Oslo modules used by the core source are
replaced with dummy modules in Makefile. The code is designed so that
there are very few such files, and further development should also keep
such files at a minimum. The files are located in the directory
*OSLO*\ */DUMMIES*.

SVN – Subversion
================

The Oslo CTM3 is available through the UiO central subversion system
(SVN). To access the code you have to be member of the group
``gf-ozone``.

SVN is a version control system, and a good introduction is the SVN
book, available at *http://svnbook.red-bean.com/*.

To use SVN you need to have SVN installed/loaded. If it is not installed
on the computer, it could be available as a module which you can usually
load it with

::

    module load svn

Important SVN commands are ``checkout``, ``add``, ``delete``/``remove``,
``commit``, ``status``, ``diff``.

For version 1.5 you also have ``resolve``, which differs from previous
versions using ``resolved``. You can find documentation of earlier
versions on the SVN book home page.

Repository structure
--------------------

Only the Oslo CTM3 code, i.e. the ``ctm3f90`` of SVN, is described in
this manual. But there are other tools available through SVN, given in
different directories of the repository. One of the most important ones
is ``utilities``. When you are experienced enough to use branches and
tags, you have separate directories for them.

::

      ctm3f90/         (Oslo CTM3 code)
        MANUAL/        (this manual)
          LATEX/       (latex code manual)
        OSLO/          (Oslo chemistry code)
          DEAD_COLUMN/ (Dust code)
          DUMMIES/     (Dummy routines)
        tables/        (tables)
          PMEAN/       (to generate annual mean sfcP)
        tmp/           (where .o-files end up)
        mod/           (where .mod-files end up)
      branches/
      tags/
      utilities/
        generateOpenIFS/
        fast_jx/
          scattering/
            fj_phase/
            mishchenko/
          xsection/
        fortran/
          ec2netcdf4/
          generate_ctm3_restart/
        idl/
        matlab/
      ctm2/            (old CTM2 code)
      trunk/           ("old" Oslo CTM3 code)
        MANUAL/        (this manual)
        OSLO/          (Oslo chemistry code)
          DEAD_COLUMN/ (Dust code)
          DUMMIES/     (Dummy routines)
          ROUTINES_NOT_USED/   (Old routines)
        UCI_Q56D/      (original UCI code)
        UCI_Q60A/      (original UCI code)
        tables/        (tables)
          PMEAN/       (to generate annual mean sfcP)
        tmp/           (for compilation)

Checkout
--------

The Oslo CTM3 code is located in a \ *repository*, and you get it by
typing:

::

    svn checkout \
      svn+ssh://svn.uio.no/svnroot/osloctm3/ctm3f90 \
      <name of dir to put the code>

You can write this on one line in the command window; the backslash
allows you to write a command on several lines. The code is now
available in your working directory. In the *.svn* directory the
original revision files are also available.

If you don’t specify the name of directory where the code will be copied
to, it will be named *ctm3f90*.

Similarly you can fetch the ``utilities`` or any other directory from
the repository, just swap “ctm3f90” with the directory you want. For
example, if you only want this manual, use ``ctm3f90/MANUAL``.

Status
------

SVN can check your working directory files against the original revision
of the repository. *It does not check the repository itself!* The status
tells you whether the file has changed or not.

::

    svn status <file or path>

If a file is specified, only that file is checked. If a directory is
specified (no specified path or file means current directory) all files
in the directory are checked.

The status returns a letter for each file specified:

? File does not exist in repository.

A File is scheduled for addition.

D File is scheduled for removal.

C File is in conflict with repository.

M File is locally modified.

| **Important**
| The current version (i.e. the latest repository version) may have
  changed even though the file status reports the file to be unchanged.
  Adding an option to the command allows you to see which files that
  have newer versions in the repository (marked by ``*``):

::

    svn status -u <file or path>

You can also add a verbose option ``-v``.

Conflicts & resolve
-------------------

Conflicts occur when you try to update your files, and SVN does not know
how to implement your changes into the repository. This must then be
*resolved* before you can commit the changes. Resolving can be carried
out immediately or be postponed, which is usually the best way.

| **SVN version 1.4 and older**
| If you use svn older than version 1.5, you solve a conflict by doing
  changes to the file and then type:

::

    svn resolved <file in conflict>

| **SVN version 1.5 and newer**
| When getting a conflict for a file, several files are generated, their
  file names being the original file name + an extension:

-  .mine Your working copy based on revision XX.

-  .rXX Revision which your working copy was based on.

-  .rYY The new file in the repository.

For version 1.5 and newer, the command is resolve, and it takes several
options. You can decide to use your version

::

    svn resolve --accept mine-full \
      <file in conflict>

This can also be done by manually correcting the conflicts in the file,
followed by

::

    svn resolve --accept working \
      <file in conflict>

Other possibilities are to accept the repository version (no changes
will be committed)

::

    svn resolve --accept theirs-full \
      <file in conflict>

For resolving conflicts, see Chapter 2 in the SVN book.

Adding a file
-------------

When adding a new file or directory you must first update to the most
recent repository version, and then you write

::

    svn add <the new file>

which schedules the file for the repository. Then you can commit the
file:

::

    svn commit <the new file>

Deleting a file
---------------

If you want to delete a file, simply use the ``delete`` or ``remove``
commands, e.g.

::

    svn delete <remove-file>

which schedules the file for removal, and also removes your local copy.
It will be deleted in the repository when you do the commit:

::

    svn commit <remove-file>

Remember to have the last repository version before deleting files.

You can also delete directories in the same way. In that case, however,
the local directory is probably not removed until you commit the
changes.

Committing changes
------------------

Not everybody should be allowed to commit changes. A moderator should
approve your work before it is committed.

When it has been agreed that your modified files are to be included to
the repository, you first need to make sure the file is otherwise
updated:

::

    svn update

Then you can commit the changes, unless there are conflicts;

::

    svn commit -m ``msg''

where “msg” is a message to tag the changes. Instead of a message you
can send in a file where you describe the changes. This is done by
``-F <filename>`` instead of ``-m ``msg''``.

If you only want to commit some of the changed files, use

::

    svn commit <files to commit> -m ``msg''

Go back to previous version
---------------------------

If you commit and want to go back to another version, here is what you
have to do. Say you want to go back to revision r203, then you write:

::

    svn merge -r HEAD:r203 .

You may have to give password several times. Probably, also
``svn merge -c r203`` works. Then, your working copy is updated to r203
and you have to commit it:

::

    svn commit -m ``msg''

Diff
----

Diff will give you the differences between your working copy and the
revision of the repository it is updated to, i.e. what is located in the
*.svn* directory. It does not check the repository!

If you want to compare with other revisions, use the option
``-r revision_number``; this will check the repository. The
``revision number`` can take several forms, which you should find in the
SVN book. The useful revision is usually the latest, and the best
solution for this is probably:

::

      svn diff -r revision <file>

where ``revision`` may be the wanted revision number, or it may be some
date format e.g. \ ``{YYYY-MM-DD}`` or ``{HH:MM}`` (must include
parentheses). See the SVN book for more.

Log
---

The changes you specify with ``-m`` when you commit will be stored in
a log. You access the log by

::

      svn log

to get all changes reported, or by

::

      svn log <file>

to get changes for a specific file.

Help
----

You find out more about the SVN commands by typing

::

      svn help <command>

Branching
---------

When you start working on a large modification to the Oslo CTM3, it is
wise to create a \ *branch* containing your code. SVN will keep track of
your files as before, but it will not affect the trunk – only the
branch. Later, when the branch is working well, the branch may be merged
into the trunk.

Read the SVN book to find out more on this.

Technical notes
===============

In this Appendix some technicalities are described, e.g. how the
atmosphere is set up.

AIR mass
--------

The air mass (``AIR``) is calculated from the pressure, in the routine
``AIRSET``.

Since pressure is force (:math:`mg_0`) per area, we find the mass of
each grid box from the pressure difference :math:`\Delta p` between grid
box top and bottom, and the grid box area :math:`A`:

.. math:: m = \frac{\Delta p A}{g_0}

This air mass is then the wet air (``AIRWET``), and dry air is
calculated by using the specific humidity (model field Q):

.. math:: m_d = m (1 - Q)

When initializing from a restart file, ``AIR`` is also initialized from
that file.

Calculation of layer heights
----------------------------

The height of a layer :math:`L` is calculated by the hypsometric
equation

.. math:: \Delta z(L) = \frac{R_d T_v(L)}{g_0} \ln\left(\frac{p(L)}{p(L+1)}\right)

 where :math:`p` is the pressure array for the grid box bottom edges,
starting at the surface. The model variable is called ``ZOFLE``.

By using specific humidity, the virtual temperature can be calculated:

.. math:: T_v = T (1-Q)

 UCI assumes 0.5% mixing ratio, giving :math:`q=0.005`, and instead of
calculating :math:`T_v`, they calculate the corresponding average gas
constant

.. math:: R = R_d(1-q) + R_v q \approx 288

 and use temperature directly. You find this value where they multiply
with :math:`29.36 \approx 288/g_0`. This is not done in the Oslo CTM3;
we use :math:`Q` from the meteorological data.

Calculation of relative humidity
--------------------------------

Relative humidity is not available as meteorological field. But it can
be calculated from specific humidity (:math:`Q`). Note that when ECMWF
reduced Gaussian fields are interpolated, e.g. from T319 to T42,
temperature and specific humidity are treated differently. Temperature
interpolation takes altitude into account. Thus, in regions of steep
mountains, the calculated relative humidity may become unrealistically
high. It may also be that the conversion from reduced Gaussian grid to
regular grid may produce some of the same inconsistencies.

However, the openIFS meteorological data is produced in T159N80L60
resolution, so no conversion to coarser resolution is done.

Specific humidity is defined as

.. math::

   Q = \frac{\varrho_v}{\varrho}
       = \frac{\varrho_v}{\varrho_a + \varrho_v}\label{eq:qhum}

 where :math:`\varrho_v` is density of water vapor and :math:`\varrho_a`
is density of dry air.

From the equation of state, we have densities expressed by pressure
:math:`p` and temperature :math:`T`:

.. math::

   \begin{aligned}
     \varrho_a &=& \frac{p_a}{R_a\,T}\label{eq:stateair}\\
     \varrho_v &=& \frac{p_v}{R_v\,T}\label{eq:statewater}\end{aligned}

From Eq. ([eq:qhum]) we see that

.. math:: \varrho_v (1 - Q) = \varrho_a \, Q

 and from Eq. ([eq:stateair]-[eq:stateair]) we get

.. math:: \frac{R_a}{R_d}\,p_v\,(1 - Q) = p_a\,Q

 With the usual approximation that :math:`p_a \gg p_v`, so that
:math:`p \simeq p_a`, denoting :math:`R_a/R_v = \varepsilon`:

.. math:: p_v = p \frac{Q}{\varepsilon (1 - Q)}

For temperature in Kelvin, saturation water vapor pressure
(:math:`e_w(T)`) is given by

.. math:: e_w(T) = 611.2\,\exp(\frac{17.67\,(T - 273.15)}{T - 29.65})

 giving relative humidity

.. math:: RH = \frac{p_v}{e_w}

Relative humidity with respect to ice is another matter, and requires
that you calculate :math:`e_i(T)` instead of :math:`e_w(T)`.

How Makefile works
------------------

The code is compiled with GNU make; just write ``gmake`` (or ``make``)
in the code directory). It reads the *Makefile* and sets up the list of
source files and compiles them. All object files are put into the
temporary directory *tmp/*. The compilation will be faster by using the
option ``-j``, e.g. \ ``gmake -j8``.

When you add a file to the code, you need to include some information
about it in *Makefile*. This may be a little tricky, so I will go
through the steps here.

| **Adding a new file**
| Say you add a file to the *OSLO* directory, called *myfile.f90*. Then
  follow these two steps:

| Step 1
| Add the file to the source list in *Makefile*: Locate the ``OSLO_SRC``
  variable and add the file name there.

| Step 2
| Add dependencies; which objects does your file depend on, and which of
  the existing files depend on your file. These are listed at the bottom
  of *Makefile*. If your file depend on *cmn\_size.f90*, you add

::

    $(filter %myfile.o, $(ALL_OBJ)): \
        $(filter %cmn_size.o, $(ALL_OBJ))

In the same way you must add ``%myfile.o`` to the file using your file.

Try to keep the files in alphabetic order in the dependency listing. You
only need to list module files and common files, *not* regular
subroutine files since those are checked only at the last program
linking (but you may still include dependencies if you like).

| **Adding a new directory**
| Follow the structure of ``OSLO_PATH`` and ``OSLO_SRC`` and create your
  similar directory ``MY_PATH`` and list ``MY_SRC``.

The path must be added to ``ALL_DIRS`` and perhaps ``INCLUDES``. At the
end of the source listing, add ``MY_SRC`` to ``ALL_SRC`` (as is done for
``OSLO_PATH``).

DUST map
--------

Input files for the DUST application (Section [sxn:dust]) can be
generated with the *map* module written by Charlie Zender. It is located
in the directory
*/div/amoc/d4-4/oslo-ctm/archive\_ctm/*\ *ctm\_tools/*\ *map*. If you
want a newer version of *map* you have to contact Zender who develops
*map*.

The map-module takes several 1x1 or 0.25x0.25degree files and
interpolates them to the desired resolution. Commands to generate
datasets are given in the *bds.sh* file in the map-directory.

Having the correct input file, it is important to decide what kind of
*erodibility factor* you want to use. The program *bds.F90* contains
commands to set the right erodibility factor using the ``ncap`` command.
See Section [sxn:dust] for more on erodibility factor.

All input data needed for running *map* are available on
*/div/amoc/d4-4/oslo-ctm/archive\_ctm/*\ *input\_files/*\ *data*.

Libraries needed to run the map module
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

The map module needs several libraries, also written by Charlie Zender.
These libraries are in general non-standard Fortran codes, and are not
compiled.

If you want to compile the libraries from scratch (for example on Linux,
SGI or HP-UX), you have to compile Charlies f-module (available on
*/div/amoc/d4-4/oslo-ctm/archive\_ctm/*\ *ctm\_tools/*\ *libraries/zender\_f*),
Charlie’s c-module (similarly on *zender\_c*). In theory: get the files
and write *gmake*, in practice, this might cause some pain...

You should have all the compiled library files in one directory, with
write access to the directory (you need to write to this directory when
running the map module).

After you have the libraries, you need to set the environment variable
``MY_LIB_DIR`` to be this directory.

All Charlie’s programs use the environment variable ``NETCDF_INC`` and
``NETCDF_LIB``, so you might want to set those before you start doing
anything else. See Appendix [app:libraries:netcdf] for paths to netCDF
libraries.

Future work/revisions
=====================

During my (Amund) 15-ish years of working with this model, here are some
of the changes I think are needed for the Oslo CTM3.

Optimisation
------------

The parallel region for chemistry spends a lot of time moving data from
regular arrays (e.g. size ``IPAR,JPAR,LPAR,NPAR``) to private arrays
(e.g. size ``LPAR,NPAR,IDBLK,JDBLK``).

I think the following should be changed:

``T`` should be converted to ``T_LIJ``.

``GAMA`` should be converted to ``LIJ``, which could make ``GAMAB``
unnecessary.

Diagnostic array ``STTTND`` should be converted.

It should be considered to convert all transport arrays also, and do
layerwise conversion for horizontal transport. This is a large job and
will make the Oslo CTM3 quite different to work with (if you are used to
how it is today).

Dry deposition update
---------------------

The dry deposition is very crude and should be updated. I have included
a first try of the EMEP dry deposition scheme (Section [sxn:drydep]) and
would suggest a proper testing of that.

New chemistry solver
--------------------

Ideally, the chemistry routine should cover both troposphere and
stratosphere, but this may be a lot of work with current schemes. The
best solution, which makes the Oslo CTM3 more flexible (e.g. diagnosing
production and loss terms), is to use the kinetic preprosessor (KPP).

Using KPP would make it easier to diagnose chemical production and loss
terms.

Tropospheric heterogeneous reactions should be revised, as well as
uptake and conversion on tropospheric aerosols.

Sulfur chemistry should be included in the stratosphere. Probably a
microphysics scheme for sulfur aerosols should be included.

Uptake and conversion on aerosols
---------------------------------

Uptake of species and conversion on tropospheric aerosols should be
revised. I have started this work, it can be found in
*aerosols2fastjx.f90*, see Section [sxn:photo\_aer].

J-values
--------

FastJX 7.3 should be implemented. Not very important, though. If not
updating to 7.3, OpenMP should be considered included in CLOUD and then
tested saved time in T159. Will not save a lot, since it is called only
every NMET.

Aerosol sedimentation
---------------------

Sedimentation of aerosols should probably be done as separate process,
probably using the moments. A first try is given in subroutine
``aerosolsettling`` in *fallingaerosols.f90*. This will require that
gravitational settling in the DUST module will have to be turned off.

PSC microphysics
----------------

I think it should be considered to replace PSC microphysics
(Section [sxn:stratchem\_microphysics]) with an even more detailed
scheme, without the current assumptions which are somewhat unexplained
(nonphysical?).

At least, the current 40 size bins makes the code rather slow. Using
kess bins may be an option; tests have shown that 15 bins may be enough
and would save some computational time.

Upper and lower boundary conditions
-----------------------------------

The upper and lower boundary conditions must be revised: They are taken
from the Oslo 2D model, which is outdated and not possible to run. The
last year available is 2011. For upper boundary, a zero flux or other
treatment may be considered, otherwise such fields could be generated
using e.g.WACCM.

It should be noted that the Oslo 2D model actually uses zero flux
conditions, i.e. it could be possible also in Oslo CTM3. But it could
lead to a larger drift – may want to have UBC unchanged from year to
year. Zero flux is that no tracer leaves UB; we could do chemistry in
LPAR or set mixrat(LPAR) = mixrat(LPAR-1). For long-lived species we
could use mixing ratio gradient, but I’m not sure why this would be
better than doing chemistry.

Lightning factors
-----------------

Note that scaling factors for lightning may not have been produced for
all different resolutions and meteorological data.

Compiler options and optimisation
=================================

In *Makefile*, there are three main choices when it comes to
optimisation: ``OPTS=D`` for debug options, ``OPTS=O`` for O2
optimisation, and ``OPTS=A`` (denoting more aggressive optimisation) for
O3 and a few more additions. O2 is typically judged to be “safe”,
whereas O3 may cause small inaccuracies to speed up the code. I have
never found O3 to fail, and recommend using ``OPTS=A``.

Depending on the machine structure, there are different compilers
available, and they have different options.

Linux
-----

On Linux machines the usual compilers are ``intel`` and ``portland``.

intel – ifort
~~~~~~~~~~~~~

For more information, see manual pages for ``ifort`` (man ifort).

**Standard options**

``-auto``: Cause all local variables to be allocated on the run time
stack. Default if OpenMP.

``-fpp``: Enable use of C preprocessor really only needed for *.F90*
files and not *.f90* files, but doesn’t harm compilation of *.f* files.

``-ftz``: Set abnormally low values to zero; i.e. values below
:math:`10^{-34}` will be zero if ``real*4`` is used.

``-mcmodel=large``: Allow for large executable file as we will get with
many components and high resolution (medium can be set to ``large``).

``-shared-intel``: Libraries provided by intel are linked dynamically
(old intel versions: ``i-dynamic``).

**Debug options**

``-mp``: Maintain floating point precision. This will reduce
optimisations!

``-check bounds``: Check array bounds.

``-check uninit``: Check whether variables are used without being
initialised first.

``-traceback``: Should trace back the error to a specific line in the
code.

``-openmp``: Enable OpenMP.

**Optimizing options**

``-inline-forceinline``: Inline code (see O2 option).

``-O2``: Including global code scheduling [7]_, software
pipelining [8]_, predication, and speculation [9]_. It also enables
inlining [10]_ and removes unreferenced variables.

``-openmp``: Enable OpenMP.

``-mp1``: As ``-mp``, it tries to maintain floating point precision, but
disables fewer optimisations than ``-mp`` (see below).

| **Aggressive optimizing options**
| As for Optimizing options, but instead of ``-O2``, use

``-O3``: Enables ``-O2`` optimisations plus more aggressive
optimisations, such as prefetching [11]_, scalar replacement [12]_, and
loop transformations. Enables optimisations for maximum speed, but does
not guarantee higher performance unless loop and memory access
transformations take place.

In addition use

``-fno-alias``: Specifies that aliasing should not be assumed in the
program.

``-fomit-frame-pointer``: Disables use of EBP as a general purpose
register so it can be used as a stack frame pointer.

portland – pgf90
~~~~~~~~~~~~~~~~

For more information, see manual pages for ``pgf90`` (man pgf90 or pgf90
-help).

**Standard options**

``-mcmodel=medium``: Allow for large executable file. Necessary to run
the Oslo CTM3.

**Debug options**

``-Mbounds``: Check array bounds.

``-g``: Generate symbolic debug information.

**Optimizing options**

``-O2``: Including global code scheduling.

``-mp``: Enable OpenMP.

``-Minline``: Pass options to the function inliner.

``-Mprefetch``: Pass options to the function inliner.

| **Aggressive optimizing options**
| As for Optimizing options, but instead of ``-O2``, use

``-O3``: Enables ``-O2`` optimisations plus more aggressive
optimisations.

``-mp``: Enable OpenMP.

``-Minline=reshape``: Pass options to the function inliner. Also reshape
arrays that would hinder normal inlining.

``-Mprefetch``: Add (don’t add) prefetch instructions for those
processors that support them.

``-Munroll``: Unroll loops.

HPC
===

You will probably use a high performance computing (HPC) facility to run
the model. These usually require that you submit a simulation to a queue
system, using a job script.

Some smaller systems may not have queues, but use the traditional
``nohup`` command. Queue systems usually also have a possibility to run
jobs *interactively*, meaning that you can log on to a computer and run
the model in your terminal window. This is the typical choice for
testing your program.

You will have to find a suitable computer or cluster to run the model.

Some computing clusters use the slurm queue system. An example can be
found in the job file example *c3run.job* in your working directory, or
the example given in Table [tbl:jobscripthpc\_1]
and [tbl:jobscripthpc\_2].

Basically, the script consists of four steps: First is the setting of
number of CPUs and memory usage, second copying and linking to work
(scratch) directory, then running the model and copying the results to
somewhere else.

Directory names can be modified, as well as the copying and linking
statements.

It is vital to set the stack size properties, also when you do not use
a queue system.

--------------

::

    #!/bin/bash
    # Job name:
    #SBATCH --job-name=testscript
    # Project:
    #SBATCH --account=account_name
    # Wall clock limit:
    #SBATCH --time=1:0:0
    # Max memory usage:
    #SBATCH --mem-per-cpu=2000M
    # Possibly use hugemem nodes
    ####SBATCH --partition=hugemem
    # Number of cores:
    #SBATCH --ntasks-per-node=8
    # Number of nodes:
    #SBATCH --nodes=1
    # Work disk space
    #SBATCH --tmp=60G
    # Set up job environment
    source /site/bin/jobsetup
    # ------------------------------
    # No need to change this section

    # Must set large stack size (unlimited)
    ulimit -s unlimited
    # Set ulimit also to unlimited
    ulimit unlimited
    # Print out information about ulimit
    ulimit -a

    # allocate specified memory to each thread
    export THREAD_STACKSIZE=100m
    #   ifort: KMP_STACKSIZE
    export KMP_STACKSIZE=$THREAD_STACKSIZE

    # number of OpenMP threads (number of CPUs)
    export OMP_NUM_THREADS= \
           $SLURM_NTASKS_PER_NODE

--------------

--------------

::

    # the PID number (if you want it)
    export PID=`date +"%d%m%y".$$`

    # ---------------------------
    # Do changes here

    # Model dir
    export MODELDIR=$HOME/WHERE_THE_MODEL_IS

    # Scenario name (use e.g. job name)
    export SCEN=$SLURM_JOB_NAME

    # Result directory at $HOME
    export RESULTDIR=$HOME/DONE.$SCEN

    # Copy or link files to work directory:
    cp $MODELDIR/myprogram  $SCRATCH/

    # Go to work directory
    cd $SCRATCH

    # Run command (using $SCEN)
    ./myprogram > results.$SCEN

    # Make the result directory
    mkdir $RESULTDIR

    # Copy files to result directory
    cp results* $RESULTDIR/

    # Done

--------------

slurm interactively
-------------------

To run the model interactively, use ``qlogin``. You will also need to
specify the account, CPUs etc (see top of Table [tbl:jobscripthpc\_1]).

You need to be a bit careful when setting the memory per CPU; the total
(number of CPUs times memory per CPU) should not exceed what is
available on a node.

It may be that the cluster also have some nodes with more memory than
this, possibly defined by an option such as hugemem (see top of
Table [tbl:jobscripthpc\_1]).

To have an idea on the total memory required to run the model, use the
UNIX command ``size``:

::

    size osloctm3

and look at the number ``bss``, which for T42L60 with tropospheric and
stratospheric chemistry is typically between 5GB and 9GB depending on
which modules you include.

While running your program interactively, you can also use the command
``top`` to see the virtual memory used by your program.

**Interactively 8 CPUs**

If your specified amount of memory is too low, or the defined stack size
is too low, the program may stop. The latter will usually only produce
a segmentation fault, while the first may produce better info.

| Given that the program stops with a segmentation fault, try setting:

::

    ulimit -s unlimited

If that does not help, try to increase the thread stack size:

::

    export THREAD_STACKSIZE=300m
    export KMP_STACKSIZE=$THREAD_STACKSIZE

These should be set in job scripts, but may be too low for some
applications.

Remember the UNIX command ``size`` to get an idea on the total memory
required.

Checking your run
-----------------

You can check your run using the command ``squeue``. If you want more
info than standard output, you can e.g. use (put everything on one
line):

::

      squeue -o '%.8i %16j %8u %.1t %.10M %.4C %.5D
                  %R' -S '-t' -u USER

where ``USER`` is your user name.

To check end time (put everything on one line):

::

      squeue -o '%.8i %16j %8u %9a %.1t %.19e %.10M %.4C %.5D
                 %.7m %R' -S '-t' -u USER

To check time and time left (put everything on one line):

::

      squeue -o '%.8i %12j %8u %7a %.1t %.10M %.10L %.3C %.3D
                 %R' -S '-e' -u USER

External libraries
==================

So far the only external libraries needed to run the Oslo CTM3 is
netCDF.

netCDF
------

To run the model, you need the netCDF libraries, and their path should
be exported as environment variables ``NETCDF_INC`` and ``NETCDF_LIB``.
This is done in *Makefile*, and the paths are given here for different
machines.

| **Abel**
| Use ``module load netcdf.intel`` or similar for other compilers. This
  sets environment variables used by *Makefile*.

HDF
---

No need for HDF, but if you are considering it, here is a note: Using
both HDF and netCDF may not work unless they are compiled with the same
of compiler version.

Meteorological data
===================

So far the Oslo CTM3 only reads ECMWF data produced at UiO, but can be
set up to use data from any GCM, as long as the required meteorological
fields are available.

Until 2015, the meteorological data were produced with the Integrated
Forecast System (IFS) model at the ECMWF computing facilities. During
2015, the openIFS model was installed at the UiO computing facility
Abel. This allowed production of more years of data.

The openIFS data are available in netCDF4 format, and also as old UiO
binary format for some years. Standard read-in is netcdf4.

At CICERO, the data are stored at */div/pdo/metdata/*, and as the
catalog names indicate, there are data from different cycles of IFS, and
also data from the openIFS model.

Note that the data must be copied to the computer system where the
Oslo CTM3 is to be run. At CICERO we have put most of what is needed at
*/div/amoc/d4-4/oslo-ctm/* and */work/projects/cicero/ctm\_input*.

ECMWF meteorological data
-------------------------

The IFS/openIFS data are 36-hour forecasts, with 12hours spin-up,
starting from reanalyses fields at 12UTC the day before. Since
cycle 36r1, the reanalyses have been from ERA-Interim, but before that
the IFS model was started from operational analyses.

Typical ECMWF horizontal resolutions are listed in
Table [table:ecmwf\_res]. We use T159/N80 in openIFS, while the older
IFS cycle 36r1 used T319/N160.

+------------+--------+-----------+------------+------------+
| Spectral   | Grid   | Degrees   | ``IPAR``   | ``JPAR``   |
+============+========+===========+============+============+
| T42        | N32    | 2.8125    | 128        | 64         |
+------------+--------+-----------+------------+------------+
| T63        | N48    | 1.875     | 192        | 96         |
+------------+--------+-----------+------------+------------+
| T106       | N80    | 1.125     | 320        | 160        |
+------------+--------+-----------+------------+------------+
| T159       | N80    | 1.125     | 320        | 160        |
+------------+--------+-----------+------------+------------+
| T319       | N160   | 0.5625    | 640        | 320        |
+------------+--------+-----------+------------+------------+
| T511       | N256   | 0.351     | 1024       | 512        |
+------------+--------+-----------+------------+------------+
| T799       | N400   | 0.225     | 1600       | 800        |
+------------+--------+-----------+------------+------------+
| T1023      | N512   | 0.176     | 2048       | 1024       |
+------------+--------+-----------+------------+------------+

Table: Resolutions available from the ECMWF, taken from
*http://www.ecmwf.int/*\ 

The IFS and openIFS models are usually run in Gaussian reduced grids,
but at least the IFS model has previously been used to put out data on
regular grids.

Conversion from reduced Gaussian grid to regular Gaussian grid is done
with the GRIB API, either at the ECMWF or locally.

File format
~~~~~~~~~~~

Until 2015, the meteorological data were provided in a binary
little-endian format, the UiO binary format.

Since late 2015, the meteorological data are available in netCDF4
format, which is now the standard read-in for Oslo CTM3.

You define which format to use by the ``metTYPE`` in *LxxCTM.inp*:

``ECMWF_oIFSnc4``: OpenIFS generated in netCDF4 format.

``ECMWF_oIFS``: OpenIFS generated in UiO binary format.

``ECMWF_IFS``: IFS data generated in UiO binary format.

However, to use the UiO binary format you need to use a different
read-in, which can be found in *metdata\_ecmwf\_uioformat.f90*. The
Oslo CTM3 should tell you if that is the case.

| **UiO binary format**
| The UiO binary format provides data in 3 files per day:

Files *EC\*.b01*: Spectral data.

Files *EC\*.b02*: 3D data on Gaussian grid.

Files *EC\*.b03*: Surface (2D) data.

All data for one meteorological time step is written before the next
time step is written, allowing a sequential read. This is rather quick,
since when continuing read-in for a time step always continues where you
left off in the previous time step. Spectral data are converted using
a fast Fourier transform.

| **netCDF4 format**
| In the netCDF4 format, all meteorological fields are given on one file
  per meteorological time step. This means that there are 8 files per
  day; 2790 files per year. They have therefore been grouped in
  directories for each month. This is the quickest way to access all
  data for a time step, so the netCDF will not have to locate a part of
  a 3D or 4D array, but can access the fields almost sequential.

Available fields
~~~~~~~~~~~~~~~~

The meteorological fields, including their names, details and ID numbers
(the UiO format IDs) are given in
Table [table:metfields1]–[table:metfields3] (tables located after
references). The netCDF4 files also contain info about the grid, which
are also listed in the tables.

IFS and openIFS generally use the local table 2 version 128 for naming
and numbering of output variables. The UiO format IDs generally follow
this table, with the exception of the extra fields put out.

ECMWF uses GRIB output format, so their result files have to be
converted to our format. This is done locally at CICERO/UiO, and
currently involves creating UiO binary format files, which are then
converted to netCDF4. A short description on how to do this is described
in Appendix [app:metdata\_prodopenifs].

Producing openIFS data
~~~~~~~~~~~~~~~~~~~~~~

At Abel, the openIFS model was set up in 2015. As the IFS model before
it, it includes a special branch that diagnoses the convective mass
fluxes and 3D precipitation fluxes (convective and large scale).

To run the openIFS model, it must be compiled, and at Abel it can be
loaded using module:

::

    module load openifs

In the *openifs/38r1v04/* directory (or *openifs/40r1v1/* for cycle
40r1), located in */cluster/software/VERSIONS/*, you find the
subdirectory *osloctm*. In principle this should be copied to your own
suitable directory, but in my opinion the files should be slightly
modified. Such modified files can be found in the repository directory
*utilities/*\ *generateOpenIFS*.

There are four job scripts that you will use, and one input file:

*getEIdata.job*

*runInterpolation.job*

*runOpenIFS.job*

*runECgrib2CTM.job*

*initrun*

The scripts should be run separately, although it is possible to let one
start the next. That has been tested and may cause problems.

In *initrun*, you define the resolution and date to retrieve. It should
look like this for T159L60N80 resolution:

::

    # Experiment id (has to stay the same from
    # interpolation to running openIFS.
    EXPVER=b0z5
    # Resolution
    RESOL=159
    # Number of vertical levels
    LEVELS=60
    # reduced GG to regular GG
    NGG=80
    # for openIFS l_2 if higher resolution is used
    GRID_TYPE="l_2"

    # start date YYYYMMDD
    SDATE=20150801
    # end date YYYYMMDD
    EDATE=20150831
    # TIME is given as a list of times separated
    # by / i.e. 00/12 
    TIME=12
    # TIMESTEP timestep to run openIFS
    TIMESTEP=1200.0
    # FCLENGTH is the FORECAST LENGTH (hours) to
    # run openIFS
    FCLENGTH=36

``EXPVER`` is experiment version you may alter, however, it has to stay
the same from interpolation to running the OpenIFS.

``TIME`` is the start hour of the run, which is noon. If you specify
other values also, you will get more data. This is not necessary.

To use coarser resolution such as ``RESOL=42``, you need to set
``GRID_TYPE=_2``.

The job scripts needs an environment variable ``WORKDIR``, which on Abel
is your work directory. These are set up when you load openIFS using
module.

| **Retrieve analyses from ECMWF**
| To retrieve restart fields for openIFS, you must have a registered
  account at ECMWF, see *software.ecmwf.int/wiki/display/OIFS/* for more
  on this.

The *getEIdata.job* will put retrieved reanalysis fields in the
directory *$WORKDIR/EI/*. Retrieving one month of data usually takes
less than one hour. It is wise to download several months of data before
doing the next steps.

| *Important*
| ECMWF wants you to download *no more* than one month of data in each
  job.

| **Interpolate to specified resolution**
| I have set up *runInterpolation.job* to use 12CPUs on one node, since
  I found it most effective (less queue, still fairly fast).

Interpolated fields will be located in the directory *$WORKDIR/$RESOL/*.

Interpolating one month of data usually takes less than 15 minutes.
I suggest running at least 3 months at a time.

| **Run the openIFS**
| I have set up *runOpenIFS.job* to use 16CPUs on one node, since
  I found it most effective. Running one month usually takes about
  2 hours. 32CPUs on several nodes is usually not much faster; it is
  very often slower.

Output will be located in the directory *$WORKDIR/$RESOL/*.

| **Convert to UiO binary format**
| *runECgrib2CTM.job* runs on 1CPU, and takes a bit of time. It reads
  GRIB files and converts to UiO binary format.

Output will be located in the directory *$WORKDIR/OSLOCTM/$RESOL*.

Processing one month usually takes about 2–3 hours.

Note that the file day number changes from openIFS to CTM fields, which
is because the openIFS was started at noon and we only use data for the
next day.

| **Convert to netCDF4**
| In the repository utilities, there is a fortran program available
  which converts UiO binary format to netCDF4. The program is available
  at *utilities/fortran/ec2netcdf4/*, and at the top of the source code
  you find info on how to compile it.

You must specify where the UiO binary files are. After compiling the
program, you run it using

::

    ec2ncdf4 YYYY

where ``YYYY`` is the year to process. Output data will be placed in the
directory where you run the program.

Producing IFS data
~~~~~~~~~~~~~~~~~~

Since 2015 we started using the openIFS model for producing
meteorological data, but here is a very short description of how IFS
retrieval worked.

The IFS model is run through web interface at the ECMWF, where you
select the wanted resolution.

After that the archived data are retrieved in the same or lower
resolution (it is interpolated at ECMWF). Note that the native
resolution usually is *reduced* Gaussian, and we need to specify we need
*regular* Gaussian data. This is a user choice in the retrieval of data
from the MARS archive at ECMWF.

The files retrieved from ECMWF are on GRIB format, from which the UiO
binary format files are retrieved locally at UiO.

Meteorological fields in the Oslo CTM3
--------------------------------------

[app:met\_ctm] The Oslo CTM3 puts the meteorological data into arrays,
where some are used directly, and others are converted to the suitable
units. As already explained, the variable names are listed and described
in Table [table:metfields1]–[table:metfields3] (tables located after
references).

Some meteorological fields are described more closely here.

Convective mass flux
~~~~~~~~~~~~~~~~~~~~

[app:met\_ctm\_mflux] The updraft mass flux (``CWETE``) and downdraft
mass flux (``CWETD``, which is negative downwards) is given on
*half-levels*, meaning they are values valid at the *bottom of each grid
box*.

Their original units are accumulated kg/(m\ :math:`^2`\ s), and since
the flux up or down at the surface it is always zero, the *first level
stored is for layer 2.*

Conversion to kg/s is done by dividing by the time span (3 hours for
ECMWF IFS and openIFS) and multiplying with grid box area.

More special is the entrainment into updrafts ``CENTU`` and into
downdrafts ``CENTD``. These are built from detrainment rates, which are
given as accumulated kg/(m\ :math:`^2`\ s) per grid height, so the units
are accumulated [kg/(m:math:`^3`\ s)]. Detrainment rates are given for
*full-levels*, i.e. grid center values.

To convert to detrainment fluxes, these rates are multiplied with grid
height and area and divided by the time span. Entrainment (:math:`E`) is
build from the mass flux (:math:`F`) in at bottom and out of top and
detrainment (:math:`D`) by considering incoming versus outgoing air:

.. math:: F(L) + E(L) = F(L+1) + D(L)\label{eq_mfbalance}

 Another way to look at this is similarly to the IFS documentation

.. math:: \frac{dF}{dz}dz = E(L) - D(L)

 which gives the same result

.. math:: F(L+1) - F(L) = E(L) - D(L)

Equation ([eq\_mfbalance]) applies for downdrafts as well, since the
downdraft mass fluxes are negative (total mass in: :math:`-F(L+1) + E`,
total mass out: :math:`-F(L) - D`).

GCM meteorological data
-----------------------

If you want to set up the model to use meteorological data from a GCM,
you need the correct hybrid coordinates, and you need to make a new
routine reading the data. Note that if the GCM puts out spectral data,
the Oslo CTM3 routines for converting to grided data may not be
suitable, so it may be best to use gridded data directly.

Also note that for the boundary layer height\ ``BLH``, the retrieval of
the next time step value (``BLH_NEXT``) may be difficult if the data are
structured in a bad way (but not impossible, of course).

Read-in routine
~~~~~~~~~~~~~~~

There is no read-in available for other meteorological data, so you have
to make your own. Use the ECMWF read-in as starting point.

Trouble shooting
================

When you log on to the computers, you may experience a few problems. Do
not hesitate to ask the experienced Oslo CTM3 users about this, they
have probably been there!

One of the first problem you will run into is the compiling. You choose
the compiler in *Makefile*, and it is typically intel.

The compilers may not be loaded by default, you may need to load them
manually. Ask the experienced users or see the user manual for the
computing facility.

The next problem is probably that the model crashes, so you should look
up Appendix [app:hpc] for environment variables you need to set;
``OMP_NUM_THREADS`` and ``KMP_STACKSIZE`` (for intel). And you also need
to set

::

      ulimit -s unlimited

If the program says “OpenMP cannot create thread”, it is often a stack
size problem, but it can also be problems with arrays not declared
correctly (e.g. in arguments etc.). Look in parallel regions first.

Also, the program may just say “Segmentation fault” if you have set the
``KMP_STACKSIZE`` too low.

| **-O3 floating point exception**
| If you compile a loop where one index of the loop is zero (set by
  parameter), the compiling may fail in the Optimizer loop nesting. Can
  usually be solved by better programming on the loop.

| **To stop when array is out of bounds**
| You force the program to stop at array out of bounds by setting the
  environment variable

::

      export F90_BOUNDS_CHECK_ABORT=YES

| **Strange things happen**
| If suddenly very strange messages occur when compiling, there may have
  been e.g. updates to compilers. Clear the compilation
  (``gmake clean``) and compile all from start.

| **Negative mass after mp-splitting**
| This is not trivial. Locate the call to ``check_btt`` in  and insert
  similar calls after each process.

If you are using non-ECMWF meteorological data, it may be that
convection creates a very large ``GAMACB`` (vertical subsidence due to
convective updrafts), so that vertical advection (``DYN2W_OC``)
transports too much out of a grid box (Lifshitz-criterion may not be
well defined). This may mean that the convective mass flux is not good.
To continue using the meteorological data as is, a solution is to
increase the number of advection steps ``NADV``. See
Section [sxn:transport\_adv] for a possible work-around.

Chemical reactions
==================

Reactions taken into account in the Oslo CTM3 are listed here, separated
into photolytic reactions, bi-molecular reactions, tri-molecular
reactions, and heterogeneous reactions.

Listed are the JPL :raw-latex:`\citep{jpl06-2}` reaction numbers, that
consist of a letter followed by number (e.g. A1), and the IUPAC
:raw-latex:`\citep{IUPACweb}` numbers which use more letters to describe
the reaction. The numbering should be evident once you look at
JPL/IUPAC.

If reaction data are provided by both JPL and IUPAC , the number in
*italics* is not used. In the tables below, the JPL/IUPAC values are
listed as ’Reference’ or ’Ref.’.

Sometime during 2015-2016, I changed the reaction names in Oslo CTM3 so
that reaction of A + B is called ``r_a_b``. They are listed in the file
*chem\_oslo\_rates.f90*.

Photolytic reactions
--------------------

| **Tropospheric chemistry module**

+-----------------------------------------------------------------------------------------------------------+---------------+
| Reaction                                                                                                  | Reference     |
+===========================================================================================================+===============+
| **Ox**                                                                                                    |               |
+-----------------------------------------------------------------------------------------------------------+---------------+
| O\ :math:`_3` + h\ :math:`\nu` :math:`\longrightarrow` O\ :math:`_2` + O(\ :math:`^3`\ P)                 | A1/           |
+-----------------------------------------------------------------------------------------------------------+---------------+
| O\ :math:`_3` + h\ :math:`\nu` :math:`\longrightarrow` O\ :math:`_2` + O(\ :math:`^1`\ D)                 | A2/           |
+-----------------------------------------------------------------------------------------------------------+---------------+
| **HOx**                                                                                                   |               |
+-----------------------------------------------------------------------------------------------------------+---------------+
| H\ :math:`_2`\ O\ :math:`_2` + h\ :math:`\nu` :math:`\longrightarrow` 2OH                                 | B3/           |
+-----------------------------------------------------------------------------------------------------------+---------------+
| **NOx**                                                                                                   |               |
+-----------------------------------------------------------------------------------------------------------+---------------+
| NO\ :math:`_2` + h\ :math:`\nu` :math:`\longrightarrow` NO + O(\ :math:`^3`\ P)                           | C1/PNOx4      |
+-----------------------------------------------------------------------------------------------------------+---------------+
| NO\ :math:`_3` + h\ :math:`\nu` :math:`\longrightarrow` NO\ :math:`_2` + O(\ :math:`^3`\ P)               | C2a/PNOx5     |
+-----------------------------------------------------------------------------------------------------------+---------------+
| N\ :math:`_2`\ O\ :math:`_5` + h\ :math:`\nu` :math:`\longrightarrow` NO\ :math:`_2` + NO\ :math:`_3`     | C5/PNOx7      |
+-----------------------------------------------------------------------------------------------------------+---------------+
| HNO\ :math:`_3` + h\ :math:`\nu` :math:`\longrightarrow` NO\ :math:`_2` + OH                              | C7/PNOx2      |
+-----------------------------------------------------------------------------------------------------------+---------------+
| HO\ :math:`_2`\ NO\ :math:`_2` + h\ :math:`\nu` :math:`\longrightarrow` NO\ :math:`_2` + HO\ :math:`_2`   | C8a+b/PNOx3   |
+-----------------------------------------------------------------------------------------------------------+---------------+
| **Organic**                                                                                               |               |
+-----------------------------------------------------------------------------------------------------------+---------------+
| CH\ :math:`_2`\ O + h\ :math:`\nu` :math:`\longrightarrow` H + HCO                                        | D1/P1         |
+-----------------------------------------------------------------------------------------------------------+---------------+
| CH\ :math:`_2`\ O + h\ :math:`\nu` :math:`\longrightarrow` H\ :math:`_2` + CO                             | D1/P1         |
+-----------------------------------------------------------------------------------------------------------+---------------+
| CH\ :math:`_3`\ CHO + h\ :math:`\nu` :math:`\longrightarrow` CHO + CH\ :math:`_3`                         | D2/P3         |
+-----------------------------------------------------------------------------------------------------------+---------------+
| CH\ :math:`_3`\ O\ :math:`_2`\ H + h\ :math:`\nu` :math:`\longrightarrow` OH + CH\ :math:`_3`\ O          | D7/P12        |
+-----------------------------------------------------------------------------------------------------------+---------------+
| CH\ :math:`_3`\ COCH\ :math:`_2`\ CH\ :math:`_3` + h\ :math:`\nu` :math:`\longrightarrow`                 |               |
+-----------------------------------------------------------------------------------------------------------+---------------+
| CH\ :math:`_3`\ CO + C\ :math:`_2`\ H\ :math:`_5`\ O\ :math:`_2`                                          | /P8           |
+-----------------------------------------------------------------------------------------------------------+---------------+
| CH\ :math:`_3`\ COCOCH\ :math:`_3` + h\ :math:`\nu` :math:`\longrightarrow` 2 CH\ :math:`_3`\ CO          | use D18/P6    |
+-----------------------------------------------------------------------------------------------------------+---------------+
| HCOHCO + h\ :math:`\nu` :math:`\longrightarrow` CO + CH\ :math:`_2`\ O                                    | D17/P4        |
+-----------------------------------------------------------------------------------------------------------+---------------+
| CH\ :math:`_3`\ COHCO + h\ :math:`\nu` :math:`\longrightarrow`                                            |               |
+-----------------------------------------------------------------------------------------------------------+---------------+
| CO + CH\ :math:`_3`\ CHO                                                                                  | D18/P6        |
+-----------------------------------------------------------------------------------------------------------+---------------+
| CH\ :math:`_3`\ COCH\ :math:`_3` + h\ :math:`\nu` :math:`\longrightarrow`                                 |               |
+-----------------------------------------------------------------------------------------------------------+---------------+
| CH\ :math:`_3`\ CO + CH\ :math:`_3`                                                                       | /0.7P8        |
+-----------------------------------------------------------------------------------------------------------+---------------+

| 

+-----------------------------------------------------------------------------------------------------------+-------------+
| Reaction                                                                                                  | Reference   |
+===========================================================================================================+=============+
| **Ox**                                                                                                    |             |
+-----------------------------------------------------------------------------------------------------------+-------------+
| O\ :math:`_3` + h\ :math:`\nu` :math:`\longrightarrow` O\ :math:`_2` + O(\ :math:`^3`\ P)                 | A2/POx1     |
+-----------------------------------------------------------------------------------------------------------+-------------+
| O\ :math:`_3` + h\ :math:`\nu` :math:`\longrightarrow` O\ :math:`_2` + O(\ :math:`^1`\ D)                 | A2/POx1     |
+-----------------------------------------------------------------------------------------------------------+-------------+
| O\ :math:`_2` + h\ :math:`\nu` :math:`\longrightarrow` 2 O(\ :math:`^3`\ P)                               | A1/POx2     |
+-----------------------------------------------------------------------------------------------------------+-------------+
| **HOx**                                                                                                   |             |
+-----------------------------------------------------------------------------------------------------------+-------------+
| H\ :math:`_2`\ O + h\ :math:`\nu` :math:`\longrightarrow` OH + H                                          | B2/PHOx1    |
+-----------------------------------------------------------------------------------------------------------+-------------+
| H\ :math:`_2`\ O\ :math:`_2` + h\ :math:`\nu` :math:`\longrightarrow` 2OH                                 | B3/PHOx2    |
+-----------------------------------------------------------------------------------------------------------+-------------+
| **NOx**                                                                                                   |             |
+-----------------------------------------------------------------------------------------------------------+-------------+
| NO + h\ :math:`\nu` :math:`\longrightarrow` N + O                                                         | N/A         |
+-----------------------------------------------------------------------------------------------------------+-------------+
| NO\ :math:`_2` + h\ :math:`\nu` :math:`\longrightarrow` NO + O(\ :math:`^3`\ P)                           | C1/PNOx4    |
+-----------------------------------------------------------------------------------------------------------+-------------+
| NO\ :math:`_3` + h\ :math:`\nu` :math:`\longrightarrow` NO\ :math:`_2` + O(\ :math:`^3`\ P)               | C2a/PNOx5   |
+-----------------------------------------------------------------------------------------------------------+-------------+
| NO\ :math:`_3` + h\ :math:`\nu` :math:`\longrightarrow` NO + O\ :math:`_2`                                | C2b/PNOx5   |
+-----------------------------------------------------------------------------------------------------------+-------------+
| N\ :math:`_2`\ O + h\ :math:`\nu` :math:`\longrightarrow` N2 + O(\ :math:`^1`\ D)                         | C3/PNOx6    |
+-----------------------------------------------------------------------------------------------------------+-------------+
| N\ :math:`_2`\ O\ :math:`_5` + h\ :math:`\nu` :math:`\longrightarrow` NO\ :math:`_2` + NO\ :math:`_3`     | C5/PNOx7    |
+-----------------------------------------------------------------------------------------------------------+-------------+
| HNO\ :math:`_3` + h\ :math:`\nu` :math:`\longrightarrow` NO\ :math:`_2` + OH                              | C7/PNOx2    |
+-----------------------------------------------------------------------------------------------------------+-------------+
| HO\ :math:`_2`\ NO\ :math:`_2` + h\ :math:`\nu` :math:`\longrightarrow` NO\ :math:`_2` + HO\ :math:`_2`   | C8a/PNOx3   |
+-----------------------------------------------------------------------------------------------------------+-------------+
| HO\ :math:`_2`\ NO\ :math:`_2` + h\ :math:`\nu` :math:`\longrightarrow` NO\ :math:`_3` + OH               | C8b/PNOx3   |
+-----------------------------------------------------------------------------------------------------------+-------------+
| **Organic**                                                                                               |             |
+-----------------------------------------------------------------------------------------------------------+-------------+
| CH\ :math:`_2`\ O + h\ :math:`\nu` :math:`\longrightarrow` H + HCO                                        | D1/P1       |
+-----------------------------------------------------------------------------------------------------------+-------------+
| CH\ :math:`_2`\ O + h\ :math:`\nu` :math:`\longrightarrow` H\ :math:`_2` + CO                             | D1/P1       |
+-----------------------------------------------------------------------------------------------------------+-------------+
| CH\ :math:`_3`\ O\ :math:`_2`\ H + h\ :math:`\nu` :math:`\longrightarrow` CH\ :math:`_3`\ O + OH          | N/A         |
+-----------------------------------------------------------------------------------------------------------+-------------+

+----------------------------------------------------------------------------------------------+-------------+
| **ClOx**                                                                                     |             |
+----------------------------------------------------------------------------------------------+-------------+
| Cl\ :math:`_2` + h\ :math:`\nu` :math:`\longrightarrow` Cl + Cl                              | F1/PCl11    |
+----------------------------------------------------------------------------------------------+-------------+
| ClO + h\ :math:`\nu` :math:`\longrightarrow` Cl + O(\ :math:`^3P`)                           | F2/         |
+----------------------------------------------------------------------------------------------+-------------+
| OClO + h\ :math:`\nu` :math:`\longrightarrow` O(\ :math:`^3P`) + ClO                         | F4/PCl3     |
+----------------------------------------------------------------------------------------------+-------------+
| Cl\ :math:`_2`\ O\ :math:`_2` + h\ :math:`\nu` :math:`\longrightarrow` ClOO + Cl             | F7/PCl5     |
+----------------------------------------------------------------------------------------------+-------------+
| OHCl + h\ :math:`\nu` :math:`\longrightarrow` OH + Cl                                        | F14a/PCl2   |
+----------------------------------------------------------------------------------------------+-------------+
| ClONO\ :math:`_2` + h\ :math:`\nu` :math:`\longrightarrow` Cl + NO\ :math:`_3`               | F18/PCl10   |
+----------------------------------------------------------------------------------------------+-------------+
| CF\ :math:`_2`\ Cl\ :math:`_2` (CFC12) + h\ :math:`\nu` :math:`\longrightarrow` Clx          | /PCl15      |
+----------------------------------------------------------------------------------------------+-------------+
| CFCl\ :math:`_3` (CFC11) + h\ :math:`\nu` :math:`\longrightarrow` Clx                        | PCl16       |
+----------------------------------------------------------------------------------------------+-------------+
| CHF\ :math:`_2`\ Cl (HCFC22)+ h\ :math:`\nu` :math:`\longrightarrow` Clx                     | / PCl14     |
+----------------------------------------------------------------------------------------------+-------------+
| CH\ :math:`_3`\ Cl + h\ :math:`\nu` :math:`\longrightarrow` Clx                              | /PCl12      |
+----------------------------------------------------------------------------------------------+-------------+
| CH\ :math:`_3`\ CCl\ :math:`_3` + h\ :math:`\nu` :math:`\longrightarrow` Clx                 | /PCl20      |
+----------------------------------------------------------------------------------------------+-------------+
| CF\ :math:`_3`\ CHCl\ :math:`_2` (HCFC123) + h\ :math:`\nu` :math:`\longrightarrow` Clx      | F42/PCl22   |
+----------------------------------------------------------------------------------------------+-------------+
| CF\ :math:`_2`\ ClCFCl\ :math:`_2` (CFC113) + h\ :math:`\nu` :math:`\longrightarrow` Clx     | /PCl23      |
+----------------------------------------------------------------------------------------------+-------------+
| CF\ :math:`_2`\ ClCF\ :math:`_2`\ Cl (CFC114)+ h\ :math:`\nu` :math:`\longrightarrow` Clx    | /PCl24      |
+----------------------------------------------------------------------------------------------+-------------+
| CF\ :math:`_3`\ CF\ :math:`_2`\ Cl (CFC115) + h\ :math:`\nu` :math:`\longrightarrow` Clx     | /PCl25      |
+----------------------------------------------------------------------------------------------+-------------+
| CH\ :math:`_3`\ CFCl\ :math:`_2` (HCFC141b) + h\ :math:`\nu` :math:`\longrightarrow` Clx     | F45/PCl19   |
+----------------------------------------------------------------------------------------------+-------------+
| CH\ :math:`_3`\ CF\ :math:`_2`\ Cl (HCFC142b) + h\ :math:`\nu` :math:`\longrightarrow` Clx   | F46/PCl18   |
+----------------------------------------------------------------------------------------------+-------------+
| **BrOx**                                                                                     |             |
+----------------------------------------------------------------------------------------------+-------------+
| Br\ :math:`_2` + h\ :math:`\nu` :math:`\longrightarrow` Br + Br                              | G1/PBr9     |
+----------------------------------------------------------------------------------------------+-------------+
| HBr + h\ :math:`\nu` :math:`\longrightarrow` H + Br                                          | G2/PBr1     |
+----------------------------------------------------------------------------------------------+-------------+
| BrO + h\ :math:`\nu` :math:`\longrightarrow` Br + O                                          | G3/PBr3     |
+----------------------------------------------------------------------------------------------+-------------+
| OHBr + h\ :math:`\nu` :math:`\longrightarrow` OH + Br                                        | G6/PBr2     |
+----------------------------------------------------------------------------------------------+-------------+
| BrONO\ :math:`_2` + h\ :math:`\nu` :math:`\longrightarrow` BrO + NO\ :math:`_2`              | G9/PBr7     |
+----------------------------------------------------------------------------------------------+-------------+
| BrCl + h\ :math:`\nu` :math:`\longrightarrow` Br + Cl                                        | G10/PBr8    |
+----------------------------------------------------------------------------------------------+-------------+
| CH\ :math:`_3`\ Br + h\ :math:`\nu` :math:`\longrightarrow` Bry                              | G12/PBr10   |
+----------------------------------------------------------------------------------------------+-------------+
| CF\ :math:`_2`\ ClBr (Halon-1211) + h\ :math:`\nu` :math:`\longrightarrow` Bry               | G25/PBr12   |
+----------------------------------------------------------------------------------------------+-------------+
| CF\ :math:`_3`\ Br (Halon-1301) + h\ :math:`\nu` :math:`\longrightarrow` Bry                 | G26/PBr11   |
+----------------------------------------------------------------------------------------------+-------------+

Bi-molecular reactions
----------------------

| **Tropospheric chemistry module**

+--------------------------------------------------------------------------------------------------------+-------------+
| Reaction                                                                                               | Ref.        |
+========================================================================================================+=============+
| **O(\ :math:`^1`\ D)**                                                                                 |             |
+--------------------------------------------------------------------------------------------------------+-------------+
| O(\ :math:`^1`\ D) + O\ :math:`_2` :math:`\longrightarrow` O(\ :math:`^3`\ P) + O\ :math:`_2`          | A3          |
+--------------------------------------------------------------------------------------------------------+-------------+
| O(\ :math:`^1`\ D) + H\ :math:`_2`\ O :math:`\longrightarrow` 2OH                                      | A6          |
+--------------------------------------------------------------------------------------------------------+-------------+
| O(\ :math:`^1`\ D) + N\ :math:`_2` :math:`\longrightarrow` O(\ :math:`^3`\ P) + N\ :math:`_2`          | A7          |
+--------------------------------------------------------------------------------------------------------+-------------+
| **HOx**                                                                                                |             |
+--------------------------------------------------------------------------------------------------------+-------------+
| O\ :math:`_3` + OH :math:`\longrightarrow` O\ :math:`_2` + HO\ :math:`_2`                              | B6          |
+--------------------------------------------------------------------------------------------------------+-------------+
| OH + H\ :math:`_2` :math:`\longrightarrow` H\ :math:`_2`\ O + H                                        | B7          |
+--------------------------------------------------------------------------------------------------------+-------------+
| OH + HO\ :math:`_2` :math:`\longrightarrow` H\ :math:`_2`\ O + O\ :math:`_2`                           | B10         |
+--------------------------------------------------------------------------------------------------------+-------------+
| OH + H\ :math:`_2`\ O\ :math:`_2` :math:`\longrightarrow` H\ :math:`_2`\ O + HO\ :math:`_2`            | B11         |
+--------------------------------------------------------------------------------------------------------+-------------+
| O\ :math:`_3` + HO\ :math:`_2` :math:`\longrightarrow` OH + 2O\ :math:`_2`                             | B12         |
+--------------------------------------------------------------------------------------------------------+-------------+
| HO\ :math:`_2` + HO\ :math:`_2` :math:`\longrightarrow` H\ :math:`_2`\ O\ :math:`_2` + O\ :math:`_2`   | B13         |
+--------------------------------------------------------------------------------------------------------+-------------+
| HO\ :math:`_2` + HO\ :math:`_2` + M :math:`\longrightarrow`                                            |             |
+--------------------------------------------------------------------------------------------------------+-------------+
| H\ :math:`_2`\ O\ :math:`_2` + O\ :math:`_2` + M                                                       | B13         |
+--------------------------------------------------------------------------------------------------------+-------------+
| **NOx**                                                                                                |             |
+--------------------------------------------------------------------------------------------------------+-------------+
| O(\ :math:`^3`\ P) + NO\ :math:`_2` :math:`\longrightarrow` NO + O\ :math:`_2`                         | C1          |
+--------------------------------------------------------------------------------------------------------+-------------+
| OH + HNO\ :math:`_3` :math:`\longrightarrow` NO\ :math:`_3` + H\ :math:`_2`\ O                         | C9          |
+--------------------------------------------------------------------------------------------------------+-------------+
| NO + HO\ :math:`_2` :math:`\longrightarrow` NO\ :math:`_2` + OH                                        | C12         |
+--------------------------------------------------------------------------------------------------------+-------------+
| OH + HO\ :math:`_2`\ NO\ :math:`_2` :math:`\longrightarrow`                                            |             |
+--------------------------------------------------------------------------------------------------------+-------------+
| NO\ :math:`_2` + O\ :math:`_2` + HO\ :math:`_2`                                                        | C10/R4017   |
+--------------------------------------------------------------------------------------------------------+-------------+
| O\ :math:`_3` + NO :math:`\longrightarrow` NO\ :math:`_2` + O\ :math:`_2`                              | C20         |
+--------------------------------------------------------------------------------------------------------+-------------+
| NO + NO\ :math:`_3` :math:`\longrightarrow` 2NO\ :math:`_2`                                            | C21         |
+--------------------------------------------------------------------------------------------------------+-------------+
| O\ :math:`_3` + NO\ :math:`_2` :math:`\longrightarrow` NO\ :math:`_3` + O\ :math:`_2`                  | C22         |
+--------------------------------------------------------------------------------------------------------+-------------+
| NO\ :math:`_2` + NO\ :math:`_3` :math:`\longrightarrow`                                                |             |
+--------------------------------------------------------------------------------------------------------+-------------+
| NO + NO\ :math:`_2` + O\ :math:`_2`                                                                    | C23         |
+--------------------------------------------------------------------------------------------------------+-------------+
| **Organic**                                                                                            |             |
+--------------------------------------------------------------------------------------------------------+-------------+
| O\ :math:`_3` + C\ :math:`_2`\ H\ :math:`_4` :math:`\longrightarrow` HCHO                              | D8          |
+--------------------------------------------------------------------------------------------------------+-------------+
| + 0.12HO\ :math:`_2` + 0.44CO                                                                          |             |
+--------------------------------------------------------------------------------------------------------+-------------+
| O\ :math:`_3` + C\ :math:`_3`\ H\ :math:`_6` :math:`\longrightarrow` 0.25HO\ :math:`_2`                | Ox\_VOC6/   |
+--------------------------------------------------------------------------------------------------------+-------------+
| + 0.5(HCHO + CH\ :math:`_3`\ CHO)                                                                      |             |
+--------------------------------------------------------------------------------------------------------+-------------+
| + 0.15OH + 0.03CH\ :math:`_3`\ O                                                                       |             |
+--------------------------------------------------------------------------------------------------------+-------------+
| + 0.31CH\ :math:`_3` + 0.07CH\ :math:`_4`                                                              |             |
+--------------------------------------------------------------------------------------------------------+-------------+
| + 0.4CO                                                                                                |             |
+--------------------------------------------------------------------------------------------------------+-------------+
| OH + CH\ :math:`_4` :math:`\longrightarrow` CH\ :math:`_3` + H\ :math:`_2`\ O                          | D11         |
+--------------------------------------------------------------------------------------------------------+-------------+
| OH + C\ :math:`_2`\ H\ :math:`_6` :math:`\longrightarrow`                                              | D21         |
+--------------------------------------------------------------------------------------------------------+-------------+
| C\ :math:`_2`\ H\ :math:`_5`\ O\ :math:`_2` + H\ :math:`_2`\ O                                         |             |
+--------------------------------------------------------------------------------------------------------+-------------+
| OH + CH\ :math:`_3`\ OH :math:`\longrightarrow`                                                        | JPL17       |
+--------------------------------------------------------------------------------------------------------+-------------+
| CH\ :math:`_3`\ O/CH\ :math:`_2`\ OH + H\ :math:`_2`\ O                                                |             |
+--------------------------------------------------------------------------------------------------------+-------------+
| :math:`\longrightarrow` 0.15 (CH:math:`_3` + HO\ :math:`_2` + H\ :math:`_2`\ O)                        |             |
+--------------------------------------------------------------------------------------------------------+-------------+
| :math:`\longrightarrow` 0.85 (CH:math:`_2`\ O + HO\ :math:`_2` + H\ :math:`_2`\ O)                     |             |
+--------------------------------------------------------------------------------------------------------+-------------+

+----------------------------------------------------------------------------------------------------------------+-----------------+
| OH + C\ :math:`_3`\ H\ :math:`_8` :math:`\longrightarrow`                                                      | D22/HOx\_VOC6   |
+----------------------------------------------------------------------------------------------------------------+-----------------+
| C\ :math:`_3`\ H\ :math:`_7`\ O\ :math:`_2` + H\ :math:`_2`\ O                                                 |                 |
+----------------------------------------------------------------------------------------------------------------+-----------------+
| OH + C\ :math:`_4`\ H\ :math:`_{10}` :math:`\longrightarrow`                                                   | /HOx\_VOC7      |
+----------------------------------------------------------------------------------------------------------------+-----------------+
| C\ :math:`_4`\ H\ :math:`_9`\ O\ :math:`_2` + H\ :math:`_2`\ O                                                 |                 |
+----------------------------------------------------------------------------------------------------------------+-----------------+
| OH + C\ :math:`_6`\ H\ :math:`_{14}` :math:`\longrightarrow`                                                   |                 |
+----------------------------------------------------------------------------------------------------------------+-----------------+
| C\ :math:`_6`\ H\ :math:`_{13}`\ O\ :math:`_2` + H\ :math:`_2`\ O                                              | NA              |
+----------------------------------------------------------------------------------------------------------------+-----------------+
| OH + m-Xylene :math:`\longrightarrow` 0.6AR1                                                                   | NA              |
+----------------------------------------------------------------------------------------------------------------+-----------------+
| OH + Isoprene :math:`\longrightarrow` ISOR1                                                                    | HOx\_VOC8       |
+----------------------------------------------------------------------------------------------------------------+-----------------+
| OH + HCHO :math:`\longrightarrow` CHO + H\ :math:`_2`\ O                                                       | D14             |
+----------------------------------------------------------------------------------------------------------------+-----------------+
| CH\ :math:`_3`\ O\ :math:`_2` + HO\ :math:`_2` :math:`\longrightarrow`                                         | D35             |
+----------------------------------------------------------------------------------------------------------------+-----------------+
| CH\ :math:`_3`\ O\ :math:`_2`\ H + O\ :math:`_2`                                                               |                 |
+----------------------------------------------------------------------------------------------------------------+-----------------+
| CHO + O\ :math:`_2` :math:`\longrightarrow` HO\ :math:`_2` + CO                                                | D44             |
+----------------------------------------------------------------------------------------------------------------+-----------------+
| CH\ :math:`_3`\ O + O\ :math:`_2` :math:`\longrightarrow` HO\ :math:`_2` + HCHO                                | D46             |
+----------------------------------------------------------------------------------------------------------------+-----------------+
| CH\ :math:`_3`\ O\ :math:`_2` + CH\ :math:`_3`\ O\ :math:`_2` :math:`\longrightarrow`                          | D50             |
+----------------------------------------------------------------------------------------------------------------+-----------------+
| 0.4(2CH\ :math:`_3`\ O + O\ :math:`_2`)                                                                        |                 |
+----------------------------------------------------------------------------------------------------------------+-----------------+
| + 0.6HCHO                                                                                                      |                 |
+----------------------------------------------------------------------------------------------------------------+-----------------+
| OH + CH\ :math:`_3`\ O\ :math:`_2`\ H :math:`\longrightarrow`                                                  | D16             |
+----------------------------------------------------------------------------------------------------------------+-----------------+
| CH\ :math:`_3`\ O\ :math:`_2` + H\ :math:`_2`\ O                                                               |                 |
+----------------------------------------------------------------------------------------------------------------+-----------------+
| OH + CH\ :math:`_3`\ O\ :math:`_2`\ H :math:`\longrightarrow`                                                  | D16             |
+----------------------------------------------------------------------------------------------------------------+-----------------+
| HCHO + OH + H\ :math:`_2`\ O                                                                                   |                 |
+----------------------------------------------------------------------------------------------------------------+-----------------+
| OH + AR2 :math:`\longrightarrow` AR3                                                                           | NA              |
+----------------------------------------------------------------------------------------------------------------+-----------------+
| OH + CH\ :math:`_3`\ COCH\ :math:`_2`\ CH\ :math:`_3` :math:`\longrightarrow`                                  | HOx\_VOC20      |
+----------------------------------------------------------------------------------------------------------------+-----------------+
| CH\ :math:`_3`\ COCH(O\ :math:`_2`)CH\ :math:`_3`                                                              |                 |
+----------------------------------------------------------------------------------------------------------------+-----------------+
| OH + ISOK :math:`\longrightarrow`                                                                              | HOx\_VOC2/      |
+----------------------------------------------------------------------------------------------------------------+-----------------+
| ISOR2                                                                                                          |                 |
+----------------------------------------------------------------------------------------------------------------+-----------------+
| OH + HCOHCO :math:`\longrightarrow`                                                                            | HOx\_VOC16      |
+----------------------------------------------------------------------------------------------------------------+-----------------+
| fa\ :math:`_{45}` (CHO + CO)                                                                                   |                 |
+----------------------------------------------------------------------------------------------------------------+-----------------+
| + fb\ :math:`_{45}` (CH:math:`_3`\ CO)                                                                         |                 |
+----------------------------------------------------------------------------------------------------------------+-----------------+
| + fc\ :math:`_{45}` (2CO + HO\ :math:`_2`)                                                                     |                 |
+----------------------------------------------------------------------------------------------------------------+-----------------+
| OH + RCOHCO :math:`\longrightarrow`                                                                            | HOx\_VOC18      |
+----------------------------------------------------------------------------------------------------------------+-----------------+
| CH\ :math:`_3`\ CO + CO                                                                                        |                 |
+----------------------------------------------------------------------------------------------------------------+-----------------+
| OH + CH\ :math:`_3`\ COCH\ :math:`_3` :math:`\longrightarrow`                                                  | HOx\_VOC19      |
+----------------------------------------------------------------------------------------------------------------+-----------------+
| H\ :math:`_2`\ O + CH\ :math:`_3`\ COCH\ :math:`_2`                                                            |                 |
+----------------------------------------------------------------------------------------------------------------+-----------------+
| :math:`\stackrel{\textrm{O}_3}{\longrightarrow}`\ CH\ :math:`_3`\ COCH\ :math:`_2`\ (O:math:`_2`)              |                 |
+----------------------------------------------------------------------------------------------------------------+-----------------+
| OH + CH\ :math:`_3`\ CHO :math:`\longrightarrow`                                                               | HOx\_VOC12      |
+----------------------------------------------------------------------------------------------------------------+-----------------+
| CH\ :math:`_3`\ CO + H\ :math:`_2`\ O                                                                          |                 |
+----------------------------------------------------------------------------------------------------------------+-----------------+
| OH + PAN :math:`\longrightarrow` HCHO + NO\ :math:`_2`                                                         | HOx\_VOC44      |
+----------------------------------------------------------------------------------------------------------------+-----------------+
| + CO\ :math:`_2` + H\ :math:`_2`\ O                                                                            |                 |
+----------------------------------------------------------------------------------------------------------------+-----------------+
| NO + CH\ :math:`_3`\ O\ :math:`_2` :math:`\longrightarrow`                                                     | D51             |
+----------------------------------------------------------------------------------------------------------------+-----------------+
| fa\ :math:`_{48}` (NO:math:`_2` + CH\ :math:`_3`\ O)                                                           |                 |
+----------------------------------------------------------------------------------------------------------------+-----------------+
| NO + C\ :math:`_2`\ H\ :math:`_5`\ O\ :math:`_2` :math:`\longrightarrow` fa\ :math:`_{49}`\ (NO:math:`_2`      |                 |
+----------------------------------------------------------------------------------------------------------------+-----------------+
| + (1:math:`-`\ fb\ :math:`_{49}`)(HCHO + CH\ :math:`_3`)                                                       | *D57*/ROO\_2    |
+----------------------------------------------------------------------------------------------------------------+-----------------+
| + fb\ :math:`_{49}`\ (HO:math:`_2` + CH\ :math:`_3`\ CHO))                                                     |                 |
+----------------------------------------------------------------------------------------------------------------+-----------------+
| NO + C\ :math:`_3`\ H\ :math:`_7`\ O\ :math:`_2` :math:`\longrightarrow` fa\ :math:`_{50}`\ (NO:math:`_2`      |                 |
+----------------------------------------------------------------------------------------------------------------+-----------------+
| + (1-fb:math:`_{50}`\ (CH:math:`_3`\ CHO + CH\ :math:`_3`\ O\ :math:`_2`)                                      | *ROO\_4*        |
+----------------------------------------------------------------------------------------------------------------+-----------------+
| + fb\ :math:`_{50}`\ (HO:math:`_2` + CH\ :math:`_3`\ COCH\ :math:`_3`))                                        |                 |
+----------------------------------------------------------------------------------------------------------------+-----------------+
| NO + C\ :math:`_4`\ H\ :math:`_9`\ O\ :math:`_2` :math:`\longrightarrow` fa\ :math:`_{51}`\ (NO:math:`_2`      | use ROO\_4      |
+----------------------------------------------------------------------------------------------------------------+-----------------+
| + (1:math:`-`\ fb\ :math:`_{51}`\ (CH:math:`_3`\ CHO                                                           |                 |
+----------------------------------------------------------------------------------------------------------------+-----------------+
| + C\ :math:`_2`\ H\ :math:`_5`\ O\ :math:`_2`)                                                                 |                 |
+----------------------------------------------------------------------------------------------------------------+-----------------+
| + fb\ :math:`_{51}`\ (HO:math:`_2`                                                                             |                 |
+----------------------------------------------------------------------------------------------------------------+-----------------+
| + CH\ :math:`_3`\ COCH\ :math:`_2`\ CH\ :math:`_3`))                                                           |                 |
+----------------------------------------------------------------------------------------------------------------+-----------------+
| NO + C\ :math:`_6`\ H\ :math:`_{13}`\ O\ :math:`_2` :math:`\longrightarrow` fa\ :math:`_{52}`\ (NO:math:`_2`   | use ROO\_4      |
+----------------------------------------------------------------------------------------------------------------+-----------------+
| + C\ :math:`_4`\ H\ :math:`_9`\ O\ :math:`_2` + CH\ :math:`_3`\ CHO)                                           |                 |
+----------------------------------------------------------------------------------------------------------------+-----------------+
| NO + AR1 :math:`\longrightarrow` NO\ :math:`_2` + HO\ :math:`_2`                                               | use D51         |
+----------------------------------------------------------------------------------------------------------------+-----------------+
| + AR2 + RCOHCO                                                                                                 |                 |
+----------------------------------------------------------------------------------------------------------------+-----------------+
| NO + AR3 :math:`\longrightarrow` NO\ :math:`_2` + HO\ :math:`_2`                                               | use D51         |
+----------------------------------------------------------------------------------------------------------------+-----------------+
| + HCOHCO + RCOHCO                                                                                              |                 |
+----------------------------------------------------------------------------------------------------------------+-----------------+
| NO + ISOR1 :math:`\longrightarrow` fa\ :math:`_{55}`\ (NO:math:`_2` + HO\ :math:`_2`                           | use D51         |
+----------------------------------------------------------------------------------------------------------------+-----------------+
| + ISOK + HCHO)                                                                                                 |                 |
+----------------------------------------------------------------------------------------------------------------+-----------------+
| 1.5NO + ISOR2 :math:`\longrightarrow`                                                                          | use D51         |
+----------------------------------------------------------------------------------------------------------------+-----------------+
| 1.5NO\ :math:`_2` + 0.67HO\ :math:`_2`                                                                         |                 |
+----------------------------------------------------------------------------------------------------------------+-----------------+
| + 0.43HCHO + 0.67RCOHCO                                                                                        |                 |
+----------------------------------------------------------------------------------------------------------------+-----------------+
| + 0.33CH\ :math:`_3`\ CO + 0.33CH\ :math:`_3`\ CHO                                                             |                 |
+----------------------------------------------------------------------------------------------------------------+-----------------+
| NO + CH\ :math:`_3`\ COCH(O\ :math:`_2`)CH\ :math:`_3` :math:`\longrightarrow`                                 | use D51         |
+----------------------------------------------------------------------------------------------------------------+-----------------+
| fa\ :math:`_{57}` (NO:math:`_2` + HO\ :math:`_2`                                                               |                 |
+----------------------------------------------------------------------------------------------------------------+-----------------+
| + CH\ :math:`_3`\ COCOCH\ :math:`_3`)                                                                          |                 |
+----------------------------------------------------------------------------------------------------------------+-----------------+
| NO + CH\ :math:`_3`\ COCH\ :math:`_2`\ (O:math:`_2`) :math:`\longrightarrow`                                   | use D51         |
+----------------------------------------------------------------------------------------------------------------+-----------------+
| NO\ :math:`_2` + HO\ :math:`_2` + CH\ :math:`_3`\ COCHO                                                        |                 |
+----------------------------------------------------------------------------------------------------------------+-----------------+
| NO + CH\ :math:`_3`\ CH(O\ :math:`_2`)CH\ :math:`_2`\ OH :math:`\longrightarrow`                               | use D51         |
+----------------------------------------------------------------------------------------------------------------+-----------------+
| NO\ :math:`_2` + HCHO                                                                                          |                 |
+----------------------------------------------------------------------------------------------------------------+-----------------+
| + CH\ :math:`_3`\ CHO + HO\ :math:`_2`                                                                         |                 |
+----------------------------------------------------------------------------------------------------------------+-----------------+
| NO\ :math:`_3` + CH\ :math:`_3`\ CHO :math:`\longrightarrow`                                                   | D41             |
+----------------------------------------------------------------------------------------------------------------+-----------------+
| HNO\ :math:`_3` + CH\ :math:`_3`\ CO                                                                           |                 |
+----------------------------------------------------------------------------------------------------------------+-----------------+
| CH\ :math:`_3`\ CO + O\ :math:`_2` :math:`\longrightarrow`                                                     | R\_Oxygen11     |
+----------------------------------------------------------------------------------------------------------------+-----------------+
| CH\ :math:`_3`\ COO\ :math:`_2`                                                                                |                 |
+----------------------------------------------------------------------------------------------------------------+-----------------+
| CH\ :math:`_3`\ COO\ :math:`_2` + NO :math:`\longrightarrow` CH\ :math:`_3`                                    | D59             |
+----------------------------------------------------------------------------------------------------------------+-----------------+
| + NO\ :math:`_2` + CO\ :math:`_2`                                                                              |                 |
+----------------------------------------------------------------------------------------------------------------+-----------------+

+-----------------------------------------------------------------------------------------------------------------------------+----------------------+
| CH\ :math:`_3`\ COO\ :math:`_2` + CH\ :math:`_3`\ O\ :math:`_2` :math:`\longrightarrow`                                     | D52/\ *ROO\_23*      |
+-----------------------------------------------------------------------------------------------------------------------------+----------------------+
| CH\ :math:`_3`\ O + CH\ :math:`_3` + CO\ :math:`_2` + O\ :math:`_2`                                                         |                      |
+-----------------------------------------------------------------------------------------------------------------------------+----------------------+
| CH\ :math:`_3`\ COO\ :math:`_2` + CH\ :math:`_3`\ O\ :math:`_2` :math:`\longrightarrow`                                     | D52/\ *ROO\_23*      |
+-----------------------------------------------------------------------------------------------------------------------------+----------------------+
| HCHO + CH\ :math:`_3`\ COOH + O\ :math:`_2`                                                                                 |                      |
+-----------------------------------------------------------------------------------------------------------------------------+----------------------+
| CH\ :math:`_3`\ O\ :math:`_2` + C\ :math:`_2`\ H\ :math:`_5`\ O\ :math:`_2` :math:`\longrightarrow`                         | ROO\_41              |
+-----------------------------------------------------------------------------------------------------------------------------+----------------------+
| 0.5(CH\ :math:`_3`\ O                                                                                                       |                      |
+-----------------------------------------------------------------------------------------------------------------------------+----------------------+
| + (1-fb:math:`_{68}`)(HCHO + CH\ :math:`_3`)                                                                                |                      |
+-----------------------------------------------------------------------------------------------------------------------------+----------------------+
| + fb\ :math:`_{68}` (HO:math:`_2` + CH\ :math:`_3`\ CHO))                                                                   |                      |
+-----------------------------------------------------------------------------------------------------------------------------+----------------------+
| + 0.25(HCHO + CH\ :math:`_3`\ CHO)                                                                                          |                      |
+-----------------------------------------------------------------------------------------------------------------------------+----------------------+
| CH\ :math:`_3`\ O\ :math:`_2` + C\ :math:`_3`\ H\ :math:`_7`\ O\ :math:`_2` :math:`\longrightarrow` 0.5(CH\ :math:`_3`\ O   | use ROO\_41          |
+-----------------------------------------------------------------------------------------------------------------------------+----------------------+
| + (1:math:`-`\ fb\ :math:`_{69}`)(CH\ :math:`_3`\ CHO                                                                       |                      |
+-----------------------------------------------------------------------------------------------------------------------------+----------------------+
| + CH\ :math:`_3`\ O\ :math:`_2`)                                                                                            |                      |
+-----------------------------------------------------------------------------------------------------------------------------+----------------------+
| + fb\ :math:`_{69}`\ (HO:math:`_2` + CH\ :math:`_3`\ COCH\ :math:`_3`))                                                     |                      |
+-----------------------------------------------------------------------------------------------------------------------------+----------------------+
| + 0.25(HCHO + CH\ :math:`_3`\ COCH\ :math:`_3`)                                                                             |                      |
+-----------------------------------------------------------------------------------------------------------------------------+----------------------+
| CH\ :math:`_3`\ O\ :math:`_2` + C\ :math:`_4`\ H\ :math:`_9`\ O\ :math:`_2` :math:`\longrightarrow` 0.5(CH\ :math:`_3`\ O   | use ROO\_41          |
+-----------------------------------------------------------------------------------------------------------------------------+----------------------+
| + (1:math:`-`\ fb\ :math:`_{70}`)(CH\ :math:`_3`\ CHO                                                                       |                      |
+-----------------------------------------------------------------------------------------------------------------------------+----------------------+
| + C\ :math:`_2`\ H\ :math:`_5`\ O\ :math:`_2`)                                                                              |                      |
+-----------------------------------------------------------------------------------------------------------------------------+----------------------+
| + fb\ :math:`_{70}`\ (HO:math:`_2` +                                                                                        |                      |
+-----------------------------------------------------------------------------------------------------------------------------+----------------------+
| CH\ :math:`_3`\ COCH\ :math:`_2`\ CH\ :math:`_3`))                                                                          |                      |
+-----------------------------------------------------------------------------------------------------------------------------+----------------------+
| + 0.25(HCHO                                                                                                                 |                      |
+-----------------------------------------------------------------------------------------------------------------------------+----------------------+
| + CH\ :math:`_3`\ COCH\ :math:`_2`\ CH\ :math:`_3`)                                                                         |                      |
+-----------------------------------------------------------------------------------------------------------------------------+----------------------+
| CH\ :math:`_3`\ O\ :math:`_2` + C\ :math:`_6`\ H\ :math:`_{13}`\ O\ :math:`_2` :math:`\longrightarrow`                      | use ROO\_41          |
+-----------------------------------------------------------------------------------------------------------------------------+----------------------+
| CH\ :math:`_3`\ O + CH\ :math:`_3`\ CHO + C\ :math:`_4`\ H\ :math:`_9`\ O\ :math:`_2`                                       |                      |
+-----------------------------------------------------------------------------------------------------------------------------+----------------------+
| CH\ :math:`_3`\ O\ :math:`_2` + CH\ :math:`_3`\ COCH\ :math:`_2`\ (O:math:`_2`) :math:`\longrightarrow`                     | ROO\_24              |
+-----------------------------------------------------------------------------------------------------------------------------+----------------------+
| CH\ :math:`_3`\ O + CH\ :math:`_3`\ COCHO                                                                                   |                      |
+-----------------------------------------------------------------------------------------------------------------------------+----------------------+
| + HCHO + HO\ :math:`_2`                                                                                                     |                      |
+-----------------------------------------------------------------------------------------------------------------------------+----------------------+
| CH\ :math:`_3`\ O\ :math:`_2` + CH\ :math:`_3`\ CH(O\ :math:`_2`)CH\ :math:`_2`\ OH :math:`\longrightarrow`                 | use ROO\_41          |
+-----------------------------------------------------------------------------------------------------------------------------+----------------------+
| CH\ :math:`_3`\ O + CH\ :math:`_3`\ CHO                                                                                     |                      |
+-----------------------------------------------------------------------------------------------------------------------------+----------------------+
| + HCHO + HO\ :math:`_2`                                                                                                     |                      |
+-----------------------------------------------------------------------------------------------------------------------------+----------------------+
| CH\ :math:`_3`\ O\ :math:`_2` + CH\ :math:`_3`\ COCH(O\ :math:`_2`)CH\ :math:`_3` :math:`\longrightarrow`                   | ?                    |
+-----------------------------------------------------------------------------------------------------------------------------+----------------------+
| CH\ :math:`_3`\ O + HO\ :math:`_2` + CH\ :math:`_3`\ COCOCH\ :math:`_3`                                                     |                      |
+-----------------------------------------------------------------------------------------------------------------------------+----------------------+
| CH\ :math:`_3`\ O\ :math:`_2` + ISOR1 :math:`\longrightarrow`                                                               | use ROO\_41          |
+-----------------------------------------------------------------------------------------------------------------------------+----------------------+
| HCHO + CH\ :math:`_3`\ O + HO\ :math:`_2` + ISOK                                                                            |                      |
+-----------------------------------------------------------------------------------------------------------------------------+----------------------+
| CH\ :math:`_3`\ O\ :math:`_2` + ISOR2 :math:`\longrightarrow`                                                               | use ROO\_41          |
+-----------------------------------------------------------------------------------------------------------------------------+----------------------+
| RCOHCO + CH\ :math:`_3`\ O + HO\ :math:`_2`                                                                                 |                      |
+-----------------------------------------------------------------------------------------------------------------------------+----------------------+
| HO\ :math:`_2` + CH\ :math:`_3`\ O(O\ :math:`_2`) :math:`\longrightarrow`                                                   | HOx\_VOC54           |
+-----------------------------------------------------------------------------------------------------------------------------+----------------------+
| CH\ :math:`_3`\ O\ :math:`_2`\ H + O\ :math:`_2`                                                                            |                      |
+-----------------------------------------------------------------------------------------------------------------------------+----------------------+
| HO\ :math:`_2` + C\ :math:`_2`\ H\ :math:`_5`\ O\ :math:`_2` :math:`\longrightarrow` CH\ :math:`_3`\ O\ :math:`_2`\ H       | D36                  |
+-----------------------------------------------------------------------------------------------------------------------------+----------------------+
| HO\ :math:`_2` + C\ :math:`_3`\ H\ :math:`_7`\ O\ :math:`_2` :math:`\longrightarrow` CH\ :math:`_3`\ O\ :math:`_2`\ H       | D36                  |
+-----------------------------------------------------------------------------------------------------------------------------+----------------------+
| HO\ :math:`_2` + C\ :math:`_4`\ H\ :math:`_9`\ O\ :math:`_2` :math:`\longrightarrow` CH\ :math:`_3`\ O\ :math:`_2`\ H       | D36                  |
+-----------------------------------------------------------------------------------------------------------------------------+----------------------+
| HO\ :math:`_2` + C\ :math:`_6`\ H\ :math:`_{13}`\ O\ :math:`_2` :math:`\longrightarrow` CH\ :math:`_3`\ O\ :math:`_2`\ H    | D36                  |
+-----------------------------------------------------------------------------------------------------------------------------+----------------------+
| HO\ :math:`_2` + CH\ :math:`_3`\ COCH\ :math:`_2`\ (O:math:`_2`) :math:`\longrightarrow`                                    | D36/\ *HOx\_VOC57*   |
+-----------------------------------------------------------------------------------------------------------------------------+----------------------+
| CH\ :math:`_3`\ O\ :math:`_2`\ H                                                                                            |                      |
+-----------------------------------------------------------------------------------------------------------------------------+----------------------+
| HO\ :math:`_2` + CH\ :math:`_3`\ CH(O\ :math:`_2`)CH\ :math:`_2`\ OH :math:`\longrightarrow`                                | use D36              |
+-----------------------------------------------------------------------------------------------------------------------------+----------------------+
| CH\ :math:`_3`\ O\ :math:`_2`\ H                                                                                            |                      |
+-----------------------------------------------------------------------------------------------------------------------------+----------------------+
| HO\ :math:`_2` + CH\ :math:`_3`\ COCH(O\ :math:`_2`)CH\ :math:`_3` :math:`\longrightarrow`                                  | use D36              |
+-----------------------------------------------------------------------------------------------------------------------------+----------------------+
| CH\ :math:`_3`\ O\ :math:`_2`\ H                                                                                            |                      |
+-----------------------------------------------------------------------------------------------------------------------------+----------------------+
| HO\ :math:`_2` + ISOR1 :math:`\longrightarrow` CH\ :math:`_3`\ O\ :math:`_2`\ H                                             | use D36              |
+-----------------------------------------------------------------------------------------------------------------------------+----------------------+
| HO\ :math:`_2` + ISOR2 :math:`\longrightarrow` CH\ :math:`_3`\ O\ :math:`_2`\ H                                             | use D36              |
+-----------------------------------------------------------------------------------------------------------------------------+----------------------+

| 

+--------------------------------------------------------------------------------------------------------------+---------+
| Reaction                                                                                                     | Ref.    |
+==============================================================================================================+=========+
| **Ox**                                                                                                       |         |
+--------------------------------------------------------------------------------------------------------------+---------+
| O(\ :math:`^3`\ P) + O\ :math:`_3` :math:`\longrightarrow` O\ :math:`_2` + O\ :math:`_2`                     | A1      |
+--------------------------------------------------------------------------------------------------------------+---------+
| **O(\ :math:`^1`\ D)**                                                                                       |         |
+--------------------------------------------------------------------------------------------------------------+---------+
| O(\ :math:`^1`\ D) + N\ :math:`_2`\ O :math:`\longrightarrow` N\ :math:`_2` + O\ :math:`_2`                  | A8      |
+--------------------------------------------------------------------------------------------------------------+---------+
| O(\ :math:`^1`\ D) + N\ :math:`_2`\ O :math:`\longrightarrow` NO + NO                                        | A7      |
+--------------------------------------------------------------------------------------------------------------+---------+
| O(\ :math:`^1`\ D) + H\ :math:`_2`\ O :math:`\longrightarrow` OH + OH                                        | A6      |
+--------------------------------------------------------------------------------------------------------------+---------+
| O(\ :math:`^1`\ D) + CH\ :math:`_4` :math:`\longrightarrow` OH + CH\ :math:`_3`                              | A11,B   |
+--------------------------------------------------------------------------------------------------------------+---------+
| O(\ :math:`^1`\ D) + CH\ :math:`_4` :math:`\longrightarrow` HCHO + H\ :math:`_2`                             | A11     |
+--------------------------------------------------------------------------------------------------------------+---------+
| O(\ :math:`^1`\ D) + H\ :math:`_2` :math:`\longrightarrow` OH + H                                            | A5      |
+--------------------------------------------------------------------------------------------------------------+---------+
| O(\ :math:`^1`\ D) + N\ :math:`_2` :math:`\longrightarrow` O(\ :math:`^3`\ P) + N\ :math:`_2`                | A7      |
+--------------------------------------------------------------------------------------------------------------+---------+
| O(\ :math:`^1`\ D) + O\ :math:`_2` :math:`\longrightarrow` O(\ :math:`^3`\ P) + O\ :math:`_2`                | A3      |
+--------------------------------------------------------------------------------------------------------------+---------+
| O(\ :math:`^1`\ D) + HCl :math:`\longrightarrow` Cl + OH                                                     | A12     |
+--------------------------------------------------------------------------------------------------------------+---------+
| O(\ :math:`^1`\ D) + CFCl\ :math:`_3` :math:`\longrightarrow` Clx                                            | A19     |
+--------------------------------------------------------------------------------------------------------------+---------+
| O(\ :math:`^1`\ D) + CF\ :math:`_2`\ Cl\ :math:`_2` :math:`\longrightarrow` Clx                              | A20     |
+--------------------------------------------------------------------------------------------------------------+---------+
| O(\ :math:`^1`\ D) + CHCl\ :math:`_2`\ CF\ :math:`_3` :math:`\longrightarrow` Clx                            | A42     |
+--------------------------------------------------------------------------------------------------------------+---------+
| O(\ :math:`^1`\ D) + CH\ :math:`_3`\ CCl\ :math:`_2`\ F :math:`\longrightarrow` Clx                          | A36     |
+--------------------------------------------------------------------------------------------------------------+---------+
| O(\ :math:`^1`\ D) + CH\ :math:`_3`\ CClF\ :math:`_2` :math:`\longrightarrow` Clx                            | A37     |
+--------------------------------------------------------------------------------------------------------------+---------+
| **HOx**                                                                                                      |         |
+--------------------------------------------------------------------------------------------------------------+---------+
| O(\ :math:`^3`\ P) + OH :math:`\longrightarrow` O\ :math:`_2` + H                                            | B1      |
+--------------------------------------------------------------------------------------------------------------+---------+
| O(\ :math:`^3`\ P) + HO\ :math:`_2` :math:`\longrightarrow` OH + O\ :math:`_2`                               | B2      |
+--------------------------------------------------------------------------------------------------------------+---------+
| H + O\ :math:`_3` :math:`\longrightarrow` OH + O\ :math:`_2`                                                 | B4      |
+--------------------------------------------------------------------------------------------------------------+---------+
| H + HO\ :math:`_2` :math:`\longrightarrow` 2OH                                                               | B5      |
+--------------------------------------------------------------------------------------------------------------+---------+
| H + HO\ :math:`_2` :math:`\longrightarrow` O + H\ :math:`_2`\ O                                              | B5      |
+--------------------------------------------------------------------------------------------------------------+---------+
| H + HO\ :math:`_2` :math:`\longrightarrow` H\ :math:`_2` + O\ :math:`_2`                                     | B5      |
+--------------------------------------------------------------------------------------------------------------+---------+
| OH + O\ :math:`_3` :math:`\longrightarrow` HO\ :math:`_2` + O\ :math:`_2`                                    | B6      |
+--------------------------------------------------------------------------------------------------------------+---------+
| OH + H\ :math:`_2` :math:`\longrightarrow` H\ :math:`_2`\ O + H                                              | B7      |
+--------------------------------------------------------------------------------------------------------------+---------+
| OH + OH :math:`\longrightarrow` H\ :math:`_2`\ O + O(\ :math:`^3`\ P)                                        | B9      |
+--------------------------------------------------------------------------------------------------------------+---------+
| OH + HO\ :math:`_2` :math:`\longrightarrow` H\ :math:`_2`\ O + O\ :math:`_2`                                 | B10     |
+--------------------------------------------------------------------------------------------------------------+---------+
| OH + H\ :math:`_2`\ O\ :math:`_2` :math:`\longrightarrow` H\ :math:`_2`\ O + HO\ :math:`_2`                  | B11     |
+--------------------------------------------------------------------------------------------------------------+---------+
| HO\ :math:`_2` + O\ :math:`_3` :math:`\longrightarrow` OH + 2O\ :math:`_2`                                   | B12     |
+--------------------------------------------------------------------------------------------------------------+---------+
| HO\ :math:`_2` + HO\ :math:`_2` :math:`\longrightarrow` H\ :math:`_2`\ O\ :math:`_2` + O\ :math:`_2`         | B13     |
+--------------------------------------------------------------------------------------------------------------+---------+
| HO\ :math:`_2` + HO\ :math:`_2` + M :math:`\longrightarrow`                                                  | B13     |
+--------------------------------------------------------------------------------------------------------------+---------+
| H\ :math:`_2`\ O\ :math:`_2` + O\ :math:`_2` + M                                                             |         |
+--------------------------------------------------------------------------------------------------------------+---------+
| **NOx**                                                                                                      |         |
+--------------------------------------------------------------------------------------------------------------+---------+
| O(\ :math:`^3`\ P) + NO\ :math:`_2` :math:`\longrightarrow` NO + O\ :math:`_2`                               | C1      |
+--------------------------------------------------------------------------------------------------------------+---------+
| OH + HNO\ :math:`_3` :math:`\longrightarrow` H\ :math:`_2`\ O + NO\ :math:`_3`                               | C9      |
+--------------------------------------------------------------------------------------------------------------+---------+
| OH + HO\ :math:`_2`\ NO\ :math:`_2` :math:`\longrightarrow`                                                  | C10     |
+--------------------------------------------------------------------------------------------------------------+---------+
| O\ :math:`_2` + H\ :math:`_2`\ O + NO\ :math:`_2`                                                            |         |
+--------------------------------------------------------------------------------------------------------------+---------+
| NO + HO\ :math:`_2` :math:`\longrightarrow` NO\ :math:`_2` + OH                                              | C12     |
+--------------------------------------------------------------------------------------------------------------+---------+
| N + O\ :math:`_2` :math:`\longrightarrow` NO + O(\ :math:`^3`\ P)                                            | C16     |
+--------------------------------------------------------------------------------------------------------------+---------+
| N + NO :math:`\longrightarrow` N\ :math:`_2` + O(\ :math:`^3`\ P)                                            | C18     |
+--------------------------------------------------------------------------------------------------------------+---------+
| O\ :math:`_3` + NO :math:`\longrightarrow` NO\ :math:`_2` + O\ :math:`_2`                                    | C20     |
+--------------------------------------------------------------------------------------------------------------+---------+
| NO + NO\ :math:`_3` :math:`\longrightarrow` 2NO\ :math:`_2`                                                  | C21     |
+--------------------------------------------------------------------------------------------------------------+---------+
| O\ :math:`_3` + NO\ :math:`_2` :math:`\longrightarrow` NO\ :math:`_3` + O\ :math:`_2`                        | C22     |
+--------------------------------------------------------------------------------------------------------------+---------+
| NO\ :math:`_2` + NO\ :math:`_3` :math:`\longrightarrow`                                                      | C23     |
+--------------------------------------------------------------------------------------------------------------+---------+
| NO + NO\ :math:`_2` + O\ :math:`_2`                                                                          |         |
+--------------------------------------------------------------------------------------------------------------+---------+
| **Organic**                                                                                                  |         |
+--------------------------------------------------------------------------------------------------------------+---------+
| O(\ :math:`^3`\ P) + HCHO :math:`\longrightarrow` HCO + OH                                                   | D4      |
+--------------------------------------------------------------------------------------------------------------+---------+
| OH + CH\ :math:`_4` :math:`\longrightarrow` CH\ :math:`_3` + H\ :math:`_2`\ O                                | D11     |
+--------------------------------------------------------------------------------------------------------------+---------+
| OH + HCHO :math:`\longrightarrow` H\ :math:`_2`\ O + HCO                                                     | D14     |
+--------------------------------------------------------------------------------------------------------------+---------+
| OH + CH\ :math:`_3`\ OH :math:`\longrightarrow`                                                              | JPL17   |
+--------------------------------------------------------------------------------------------------------------+---------+
| CH\ :math:`_3`\ O/CH\ :math:`_2`\ OH + H\ :math:`_2`\ O                                                      |         |
+--------------------------------------------------------------------------------------------------------------+---------+
| :math:`\longrightarrow` CH\ :math:`_2`\ O + HO\ :math:`_2` + H\ :math:`_2`\ O                                |         |
+--------------------------------------------------------------------------------------------------------------+---------+
| OH + CH\ :math:`_3`\ O\ :math:`_2`\ H :math:`\longrightarrow`                                                | D16     |
+--------------------------------------------------------------------------------------------------------------+---------+
| 0.7 (CH:math:`_3`\ O\ :math:`_2` + H\ :math:`_2`\ O)                                                         |         |
+--------------------------------------------------------------------------------------------------------------+---------+
| OH + CH\ :math:`_3`\ O\ :math:`_2`\ H :math:`\longrightarrow`                                                | D16     |
+--------------------------------------------------------------------------------------------------------------+---------+
| 0.3 (CH:math:`_2`\ O + OH + H\ :math:`_2`\ O)                                                                |         |
+--------------------------------------------------------------------------------------------------------------+---------+
| HO\ :math:`_2` + CH\ :math:`_3`\ O\ :math:`_2` :math:`\longrightarrow` CH\ :math:`_3`\ OOH + O\ :math:`_2`   | D35     |
+--------------------------------------------------------------------------------------------------------------+---------+
| CH\ :math:`_3`\ O\ :math:`_2` + NO :math:`\longrightarrow` CH\ :math:`_3`\ O + NO\ :math:`_2`                | D51     |
+--------------------------------------------------------------------------------------------------------------+---------+
| **ClOx**                                                                                                     |         |
+--------------------------------------------------------------------------------------------------------------+---------+
| O(\ :math:`^3`\ P) + ClO :math:`\longrightarrow` Cl + O\ :math:`_2`                                          | F1      |
+--------------------------------------------------------------------------------------------------------------+---------+
| O(\ :math:`^3`\ P) + OClO :math:`\longrightarrow` ClO + O\ :math:`_2`                                        | F2      |
+--------------------------------------------------------------------------------------------------------------+---------+
| O(\ :math:`^3`\ P) + HCl :math:`\longrightarrow` OH + Cl                                                     | F4      |
+--------------------------------------------------------------------------------------------------------------+---------+
| O(\ :math:`^3`\ P) + ClONO\ :math:`_2` :math:`\longrightarrow` NO\ :math:`_3` + ClO                          | F6      |
+--------------------------------------------------------------------------------------------------------------+---------+
| OH + ClO :math:`\longrightarrow` HO\ :math:`_2` + Cl                                                         | F10     |
+--------------------------------------------------------------------------------------------------------------+---------+
| OH + ClO :math:`\longrightarrow` HCl + O\ :math:`_2`                                                         | F10     |
+--------------------------------------------------------------------------------------------------------------+---------+
| OH + HCl :math:`\longrightarrow` H\ :math:`_2`\ O + Cl                                                       | F12     |
+--------------------------------------------------------------------------------------------------------------+---------+
| OH + OHCl :math:`\longrightarrow` H\ :math:`_2`\ O + ClO                                                     | F13     |
+--------------------------------------------------------------------------------------------------------------+---------+
| OH + ClONO\ :math:`_2` :math:`\longrightarrow` OHCl + NO\ :math:`_3`                                         | F15     |
+--------------------------------------------------------------------------------------------------------------+---------+
| OH + CH\ :math:`_3`\ Cl :math:`\longrightarrow` Clx                                                          | F16     |
+--------------------------------------------------------------------------------------------------------------+---------+
| OH + CHF\ :math:`_2`\ Cl :math:`\longrightarrow` Clx                                                         | F22     |
+--------------------------------------------------------------------------------------------------------------+---------+
| OH + CH\ :math:`_3`\ CCl\ :math:`_3` :math:`\longrightarrow` Clx                                             | F26     |
+--------------------------------------------------------------------------------------------------------------+---------+
| OH + CH\ :math:`_3`\ CCl\ :math:`_2`\ F :math:`\longrightarrow` Clx                                          | F27     |
+--------------------------------------------------------------------------------------------------------------+---------+
| OH + CH\ :math:`_3`\ CClF\ :math:`_2` :math:`\longrightarrow` Clx                                            | F28     |
+--------------------------------------------------------------------------------------------------------------+---------+
| OH + CHCl\ :math:`_2`\ CF\ :math:`_3` :math:`\longrightarrow` Clx                                            | F33     |
+--------------------------------------------------------------------------------------------------------------+---------+
| HO\ :math:`_2` + Cl :math:`\longrightarrow` HCl + O\ :math:`_2`                                              | F45     |
+--------------------------------------------------------------------------------------------------------------+---------+
| HO\ :math:`_2` + Cl :math:`\longrightarrow` OH + ClO                                                         | F45     |
+--------------------------------------------------------------------------------------------------------------+---------+
| HO\ :math:`_2` + ClO :math:`\longrightarrow` OHCl + O\ :math:`_2`                                            | F46     |
+--------------------------------------------------------------------------------------------------------------+---------+

+----------------------------------------------------------------------------------+---------+
| NO + OClO :math:`\longrightarrow` NO\ :math:`_2` + ClO                           | F48     |
+----------------------------------------------------------------------------------+---------+
| Cl + O\ :math:`_3` :math:`\longrightarrow` ClO + O\ :math:`_2`                   | F52     |
+----------------------------------------------------------------------------------+---------+
| Cl + H\ :math:`_2` :math:`\longrightarrow` HCl + H                               | F53     |
+----------------------------------------------------------------------------------+---------+
| Cl + H\ :math:`_2`\ O\ :math:`_2` :math:`\longrightarrow` HCl + HO\ :math:`_2`   | F54     |
+----------------------------------------------------------------------------------+---------+
| Cl + CH\ :math:`_4` :math:`\longrightarrow` HCl + CH\ :math:`_3`                 | F59     |
+----------------------------------------------------------------------------------+---------+
| Cl + HCHO :math:`\longrightarrow` HCl + HCO                                      | JPL17   |
+----------------------------------------------------------------------------------+---------+
| Cl + CH\ :math:`_3`\ OH :math:`\longrightarrow`                                  | JPL17   |
+----------------------------------------------------------------------------------+---------+
| CH\ :math:`_2`\ OH + HCl                                                         |         |
+----------------------------------------------------------------------------------+---------+
| :math:`\longrightarrow` CH\ :math:`_2`\ O + HO\ :math:`_2` + HCl                 |         |
+----------------------------------------------------------------------------------+---------+
| Cl + ClONO\ :math:`_2` :math:`\longrightarrow` 2Cl + NO\ :math:`_3`              | F85     |
+----------------------------------------------------------------------------------+---------+
| ClO + NO :math:`\longrightarrow` NO\ :math:`_2` + Cl                             | F111    |
+----------------------------------------------------------------------------------+---------+
| ClO + CO :math:`\longrightarrow` Cl + CO\ :math:`_2`                             | F114    |
+----------------------------------------------------------------------------------+---------+
| **BrOx**                                                                         |         |
+----------------------------------------------------------------------------------+---------+
| O(\ :math:`^3`\ P) + BrO :math:`\longrightarrow` Br + O\ :math:`_2`              | G1      |
+----------------------------------------------------------------------------------+---------+
| O(\ :math:`^3`\ P) + HBr :math:`\longrightarrow` OH + Br                         | G2      |
+----------------------------------------------------------------------------------+---------+
| OH + Br\ :math:`_2` :math:`\longrightarrow` OHBr + Br                            | G5      |
+----------------------------------------------------------------------------------+---------+
| OH + BrO :math:`\longrightarrow` HBr + O\ :math:`_2`                             | G6      |
+----------------------------------------------------------------------------------+---------+
| OH + BrO :math:`\longrightarrow` Br + HO\ :math:`_2`                             | G6      |
+----------------------------------------------------------------------------------+---------+
| OH + HBr :math:`\longrightarrow` H\ :math:`_2`\ O + Br                           | G7      |
+----------------------------------------------------------------------------------+---------+
| OH + CH\ :math:`_3`\ Br :math:`\longrightarrow` Bry                              | G8      |
+----------------------------------------------------------------------------------+---------+
| HO\ :math:`_2` + Br :math:`\longrightarrow` HBr + O\ :math:`_2`                  | G24     |
+----------------------------------------------------------------------------------+---------+
| HO\ :math:`_2` + BrO :math:`\longrightarrow` OHBr +O\ :math:`_2`                 | G25     |
+----------------------------------------------------------------------------------+---------+
| Br + O\ :math:`_3` :math:`\longrightarrow` BrO + O\ :math:`_2`                   | G31     |
+----------------------------------------------------------------------------------+---------+
| Br + H\ :math:`_2`\ O\ :math:`_2` :math:`\longrightarrow` HBr + HO\ :math:`_2`   | G32     |
+----------------------------------------------------------------------------------+---------+
| Br + HCHO :math:`\longrightarrow` HBr + CHO                                      | G34     |
+----------------------------------------------------------------------------------+---------+
| BrO + O\ :math:`_3` :math:`\longrightarrow` Br + 2O\ :math:`_2`                  | G38     |
+----------------------------------------------------------------------------------+---------+
| BrO + NO :math:`\longrightarrow` NO\ :math:`_2` + Br                             | G39     |
+----------------------------------------------------------------------------------+---------+
| BrO + ClO :math:`\longrightarrow` Br + OClO                                      | G41     |
+----------------------------------------------------------------------------------+---------+
| BrO + ClO :math:`\longrightarrow` Br + ClOO                                      | G41     |
+----------------------------------------------------------------------------------+---------+
| BrO + ClO :math:`\longrightarrow` BrCl + O\ :math:`_2`                           | G41     |
+----------------------------------------------------------------------------------+---------+
| BrO + BrO :math:`\longrightarrow` 2Br + O\ :math:`_2`                            | G42     |
+----------------------------------------------------------------------------------+---------+
| BrO + BrO :math:`\longrightarrow` Br\ :math:`_2` + O\ :math:`_2`                 | G42     |
+----------------------------------------------------------------------------------+---------+

Tri-molecular reactions
-----------------------

| **Tropospheric chemistry module**

+------------------------------------------------------------------------------------+---------------------+
| Reaction                                                                           | Ref.                |
+====================================================================================+=====================+
| **Ox**                                                                             |                     |
+------------------------------------------------------------------------------------+---------------------+
| O(\ :math:`^3`\ P) + O\ :math:`_2` + M :math:`\longrightarrow` O\ :math:`_3` + M   |                     |
+------------------------------------------------------------------------------------+---------------------+
| **HOx**                                                                            |                     |
+------------------------------------------------------------------------------------+---------------------+
| OH + OH + M :math:`\longrightarrow`                                                |                     |
+------------------------------------------------------------------------------------+---------------------+
| H\ :math:`_2`\ O\ :math:`_2`                                                       | B2                  |
+------------------------------------------------------------------------------------+---------------------+
| HO\ :math:`_2` + HO\ :math:`_2` + M :math:`\longrightarrow`                        |                     |
+------------------------------------------------------------------------------------+---------------------+
| H\ :math:`_2`\ O\ :math:`_2` + O\ :math:`_2` + M                                   | See bi-molecular    |
+------------------------------------------------------------------------------------+---------------------+
| **NOx**                                                                            |                     |
+------------------------------------------------------------------------------------+---------------------+
| OH + NO\ :math:`_2` + M :math:`\longrightarrow` HNO\ :math:`_3` + M                | C4                  |
+------------------------------------------------------------------------------------+---------------------+
| NO\ :math:`_2` + HO\ :math:`_2` + M :math:`\longrightarrow`                        | C5                  |
+------------------------------------------------------------------------------------+---------------------+
| HO\ :math:`_2`\ NO\ :math:`_2` + M                                                 |                     |
+------------------------------------------------------------------------------------+---------------------+
| NO\ :math:`_2` + NO\ :math:`_3` + M :math:`\longrightarrow`                        | C6                  |
+------------------------------------------------------------------------------------+---------------------+
| N\ :math:`_2`\ O\ :math:`_5` + M                                                   |                     |
+------------------------------------------------------------------------------------+---------------------+
| HO\ :math:`_2`\ NO\ :math:`_2` + M :math:`\longrightarrow`                         | NOx17               |
+------------------------------------------------------------------------------------+---------------------+
| HO\ :math:`_2` + NO\ :math:`_2` + M                                                |                     |
+------------------------------------------------------------------------------------+---------------------+
| N\ :math:`_2`\ O\ :math:`_5` + M :math:`\longrightarrow`                           | NOx32               |
+------------------------------------------------------------------------------------+---------------------+
| NO\ :math:`_2` + NO\ :math:`_3` + M                                                |                     |
+------------------------------------------------------------------------------------+---------------------+
| **Organic**                                                                        |                     |
+------------------------------------------------------------------------------------+---------------------+
| OH + CO + O\ :math:`_2` :math:`\longrightarrow`                                    | HOx\_VOC10          |
+------------------------------------------------------------------------------------+---------------------+
| HO\ :math:`_2` + CO\ :math:`_2`                                                    |                     |
+------------------------------------------------------------------------------------+---------------------+
| OH + C\ :math:`_2`\ H\ :math:`_4` + M :math:`\longrightarrow`                      | D5                  |
+------------------------------------------------------------------------------------+---------------------+
| CH\ :math:`_3` + HCHO + M                                                          |                     |
+------------------------------------------------------------------------------------+---------------------+
| OH + C\ :math:`_3`\ H\ :math:`_6` + M :math:`\longrightarrow`                      | /HOx\_VOC5          |
+------------------------------------------------------------------------------------+---------------------+
| CH\ :math:`_3`\ CH(O\ :math:`_2`)CH\ :math:`_2`\ OH + M                            |                     |
+------------------------------------------------------------------------------------+---------------------+
| CH\ :math:`_3` + O\ :math:`_2` + M :math:`\longrightarrow`                         | D3/\ *R\_Oxygen1*   |
+------------------------------------------------------------------------------------+---------------------+
| CH\ :math:`_3`\ O\ :math:`_2` + M                                                  |                     |
+------------------------------------------------------------------------------------+---------------------+
| CH\ :math:`_3`\ COO\ :math:`_2` + NO\ :math:`_2` + M :math:`\longrightarrow`       | D12                 |
+------------------------------------------------------------------------------------+---------------------+
| PAN + M                                                                            |                     |
+------------------------------------------------------------------------------------+---------------------+
| PAN + M :math:`\longrightarrow`                                                    | ROO\_15             |
+------------------------------------------------------------------------------------+---------------------+
| CH\ :math:`_3`\ COO\ :math:`_2` + NO\ :math:`_2` + M                               |                     |
+------------------------------------------------------------------------------------+---------------------+

| 

+--------------------------------------------------------------------------------------------------+--------------+
| Reaction                                                                                         | Ref.         |
+==================================================================================================+==============+
| **Ox**                                                                                           |              |
+--------------------------------------------------------------------------------------------------+--------------+
| O(\ :math:`^3`\ P) + O\ :math:`_2` + M :math:`\longrightarrow` O\ :math:`_3` + M                 | A1           |
+--------------------------------------------------------------------------------------------------+--------------+
| **HOx**                                                                                          |              |
+--------------------------------------------------------------------------------------------------+--------------+
| H + O\ :math:`_2` + M :math:`\longrightarrow` HO\ :math:`_2` + M                                 | B1           |
+--------------------------------------------------------------------------------------------------+--------------+
| OH + OH + M :math:`\longrightarrow`                                                              | B2           |
+--------------------------------------------------------------------------------------------------+--------------+
| H\ :math:`_2`\ O + O(\ :math:`^3`\ P) + M                                                        |              |
+--------------------------------------------------------------------------------------------------+--------------+
| **NOx**                                                                                          |              |
+--------------------------------------------------------------------------------------------------+--------------+
| O(\ :math:`^3`\ P) + NO\ :math:`_2` + M :math:`\longrightarrow` NO\ :math:`_3` + M               | C2           |
+--------------------------------------------------------------------------------------------------+--------------+
| OH + NO\ :math:`_2` + M :math:`\longrightarrow` HNO\ :math:`_3` + M                              | C4           |
+--------------------------------------------------------------------------------------------------+--------------+
| NO\ :math:`_2` + HO\ :math:`_2` + M :math:`\longrightarrow` HO\ :math:`_2`\ NO\ :math:`_2` + M   | C5           |
+--------------------------------------------------------------------------------------------------+--------------+
| NO\ :math:`_2` + NO\ :math:`_3` + M :math:`\longrightarrow` N\ :math:`_2`\ O\ :math:`_5` + M     | C6           |
+--------------------------------------------------------------------------------------------------+--------------+
| **Organic**                                                                                      |              |
+--------------------------------------------------------------------------------------------------+--------------+
| OH + CO + O\ :math:`_2` :math:`\longrightarrow`                                                  | HOx\_VOC10   |
+--------------------------------------------------------------------------------------------------+--------------+
| HO + CO\ :math:`_2`                                                                              |              |
+--------------------------------------------------------------------------------------------------+--------------+
| **ClOx**                                                                                         |              |
+--------------------------------------------------------------------------------------------------+--------------+
| Cl + O\ :math:`_2` + M :math:`\longrightarrow` ClOO + M                                          | F1           |
+--------------------------------------------------------------------------------------------------+--------------+
| ClO + NO\ :math:`_2` + M :math:`\longrightarrow` ClONO\ :math:`_2` + M                           | F8           |
+--------------------------------------------------------------------------------------------------+--------------+
| ClO + ClO + M :math:`\longrightarrow` Cl\ :math:`_2`\ O\ :math:`_2` + M                          | F10          |
+--------------------------------------------------------------------------------------------------+--------------+
| **BrOx**                                                                                         |              |
+--------------------------------------------------------------------------------------------------+--------------+
| BrO + NO\ :math:`_2` + M :math:`\longrightarrow` BrONO\ :math:`_2` + M                           | G2           |
+--------------------------------------------------------------------------------------------------+--------------+

Instantaneous reactions
-----------------------

| **Stratospheric chemistry module**

+------------------------------------------------------------------------------------------------+-------------------+
| Reaction                                                                                       | Ref.              |
+================================================================================================+===================+
| CH\ :math:`_3` + O\ :math:`_2` :math:`\longrightarrow` CH\ :math:`_3`\ O\ :math:`_2`           | D42/R\_Oxygen1    |
+------------------------------------------------------------------------------------------------+-------------------+
| HCO + O\ :math:`_2` :math:`\longrightarrow` CO + HO\ :math:`_2`                                | D44/R\_Oxygen10   |
+------------------------------------------------------------------------------------------------+-------------------+
| CH\ :math:`_3`\ O + O\ :math:`_2` :math:`\longrightarrow` CH\ :math:`_2`\ O + HO\ :math:`_2`   | D46/R\_O1         |
+------------------------------------------------------------------------------------------------+-------------------+

Thermal decomposition reactions
-------------------------------

| **Stratospheric chemistry module**

+------------------------------------------------------------------------------------------+------------+
| Reaction                                                                                 | JPL Ref.   |
+==========================================================================================+============+
| HO\ :math:`_2`\ NO\ :math:`_2` :math:`\longrightarrow` HO\ :math:`_2` + NO\ :math:`_2`   | T3-1.2     |
+------------------------------------------------------------------------------------------+------------+
| N\ :math:`_2`\ O\ :math:`_5` :math:`\longrightarrow` NO\ :math:`_2` + NO\ :math:`_3`     | T3-1.4     |
+------------------------------------------------------------------------------------------+------------+
| ClOO :math:`\longrightarrow` Cl + O\ :math:`_2`                                          | T3-1.11    |
+------------------------------------------------------------------------------------------+------------+
| Cl\ :math:`_2`\ O\ :math:`_2` :math:`\longrightarrow` ClO + ClO                          | T3-1.14    |
+------------------------------------------------------------------------------------------+------------+

Heterogeneous reactions
-----------------------

| **Tropospheric chemistry module**

+---------------------------------------------------------------------------------------------+--------------+
| Reaction                                                                                    | Ref./CTM     |
+=============================================================================================+==============+
| N\ :math:`_2`\ O\ :math:`_5` + aq(aerosol) :math:`\longrightarrow` 2HNO\ :math:`_3`\ (aq)   | NA/PR42HET   |
+---------------------------------------------------------------------------------------------+--------------+

| 
| See the code in *chem\_oslo\_rates.f90* for full references.

+---------------------------------------------------------------------------------------------------------+-----------+
| Reaction                                                                                                | CTM       |
+=========================================================================================================+===========+
| **On PSCs**                                                                                             |           |
+---------------------------------------------------------------------------------------------------------+-----------+
| N\ :math:`_2`\ O\ :math:`_5` + HCl (s) :math:`\longrightarrow` ClNO\ :math:`_2` + HNO\ :math:`_3` (s)   | ACPAR     |
+---------------------------------------------------------------------------------------------------------+-----------+
| N\ :math:`_2`\ O\ :math:`_5` + H\ :math:`_2`\ O (s) :math:`\longrightarrow` 2HNO\ :math:`_3` (s)        | ADPAR     |
+---------------------------------------------------------------------------------------------------------+-----------+
| ClONO\ :math:`_2` + HCl (s) :math:`\longrightarrow` Cl\ :math:`_2` + HNO\ :math:`_3` (s)                | BCPAR     |
+---------------------------------------------------------------------------------------------------------+-----------+
| ClONO\ :math:`_2` + H\ :math:`_2`\ O (s) :math:`\longrightarrow` HOCl + HNO\ :math:`_3` (s)             | BDPAR     |
+---------------------------------------------------------------------------------------------------------+-----------+
| HOCl + HCl (s) :math:`\longrightarrow` Cl\ :math:`_2` + H\ :math:`_2`\ O (s)                            | ECPAR     |
+---------------------------------------------------------------------------------------------------------+-----------+
| BrONO\ :math:`_2` + H\ :math:`_2`\ O (s) :math:`\longrightarrow` OHBr + HNO\ :math:`_3` (s)             | FDPAR     |
+---------------------------------------------------------------------------------------------------------+-----------+
| BrONO\ :math:`_2` + HCl (s) :math:`\longrightarrow` BrCl + HNO\ :math:`_3` (s)                          | FCPAR     |
+---------------------------------------------------------------------------------------------------------+-----------+
| OHBr + HCl (s) :math:`\longrightarrow` BrCl + H\ :math:`_2`\ O (s)                                      | GCPAR     |
+---------------------------------------------------------------------------------------------------------+-----------+
| ClONO\ :math:`_2` + HBr (s) :math:`\longrightarrow` BrCl + H\ :math:`_2`\ O (s)                         | BHPAR     |
+---------------------------------------------------------------------------------------------------------+-----------+
| **On sulfuric aerosols**                                                                                |           |
+---------------------------------------------------------------------------------------------------------+-----------+
| N\ :math:`_2`\ O\ :math:`_5` + HCl :math:`\longrightarrow` ClNO\ :math:`_2` + HNO\ :math:`_3`           | ACPARBK   |
+---------------------------------------------------------------------------------------------------------+-----------+
| N\ :math:`_2`\ O\ :math:`_5` + H\ :math:`_2`\ O :math:`\longrightarrow` 2HNO\ :math:`_3`                | ADPARBK   |
+---------------------------------------------------------------------------------------------------------+-----------+
| ClONO\ :math:`_2` + HCl :math:`\longrightarrow` Cl\ :math:`_2` + HNO\ :math:`_3`                        | BCPARBK   |
+---------------------------------------------------------------------------------------------------------+-----------+
| ClONO\ :math:`_2` + H\ :math:`_2`\ O :math:`\longrightarrow` HOCl + HNO\ :math:`_3`                     | BDPARBK   |
+---------------------------------------------------------------------------------------------------------+-----------+
| HOCl + HCl :math:`\longrightarrow` Cl\ :math:`_2` + H\ :math:`_2`\ O                                    | ECPARBK   |
+---------------------------------------------------------------------------------------------------------+-----------+
| BrONO\ :math:`_2` + H\ :math:`_2`\ O :math:`\longrightarrow` OHBr + HNO\ :math:`_3`                     | FDPARBK   |
+---------------------------------------------------------------------------------------------------------+-----------+
| BrONO\ :math:`_2` + HCl :math:`\longrightarrow` BrCl + HNO\ :math:`_3`                                  | FCPARBK   |
+---------------------------------------------------------------------------------------------------------+-----------+
| OHBr + HCl :math:`\longrightarrow` BrCl + H\ :math:`_2`\ O                                              | GCPARBK   |
+---------------------------------------------------------------------------------------------------------+-----------+

For reactions on PSCs, the species marked (s) stay on the particle.

Peer-reviewed papers
====================

Here you find the peer-reviewed papers of Oslo CTM3 and Oslo CTM2. and
a few older Oslo CTM1 papers at the end.

2017 – 6 papers
---------------

Regional temperature change potentials for short-lived climate forcing
based on radiative forcing from multiple models
:raw-latex:`\citep{AamaasEA2017}`.

Investigation of global particulate nitrate from the AeroCom phase III
experiment :raw-latex:`\citep{BianEA2017}`.

Emission metrics for quantifying regional climate impacts of aviation
:raw-latex:`\citep{LundEA2017b}`.

Sensitivity of black carbon concentrations and climate impact to aging
and scavenging in OsloCTM2-M7 :raw-latex:`\citep{LundEA2017a}`.

Multi-model simulations of aerosol and ozone radiative forcing due to
anthropogenic emission changes during the period 1990-2015
:raw-latex:`\citep{MyhreEA2017}`.

Aerosols at the poles: an AeroCom Phase II multi-model evaluation
:raw-latex:`\citep{SandEA2017}`.

2016 – 11 papers
----------------

Regional emission metrics for short-lived climate forcing from multiple
models :raw-latex:`\citep{AamaasEA2016}`.

Atmospheric methane evolution the last 40 years
:raw-latex:`\citep{DalsorenEA2016}`.

What controls the vertical distribution of aerosol? Relationships
between process sensitivity in HadGEM3-UKCA and inter-model variation
from AeroCom Phase II :raw-latex:`\citep{KiplingEA2016}`.

Evaluation of the aerosol vertical distribution in global aerosol models
through comparison against CALIOP measurements: AeroCom phase II results
:raw-latex:`\citep{KoffiEA2016}`.

Evaluation of observed and modelled aerosol lifetimes using radioactive
tracers of opportunity and an ensemble of 19 global models
:raw-latex:`\citep{KristiansenEA2016}`.

Extensive release of methane from Arctic seabed west of Svalbard during
summer 2014 does not influence the atmosphere
:raw-latex:`\citep{MyhreEA2016}`.

Comparison of aerosol optical properties above clouds between POLDER and
AeroCom models over the South East Atlantic Ocean during the fire season
:raw-latex:`\citep{PeersEA2016}`.

Radiative forcing from aircraft emissions of NOx: model calculations
with CH4 surface flux boundary condition
:raw-latex:`\citep{PitariEA2016}`.

Multi-model evaluation of short-lived pollutant distributions over East
Asia during summer 2008 :raw-latex:`\citep{QuennehenEA2016}`.

The effect of future ambient air pollution on human premature mortality
to 2100 using output from the ACCMIP model ensemble
:raw-latex:`\citep{SilvaEA2016}`.

Global and regional radiative forcing from 20% reductions in BC, OC and
SO4 - an HTAP2 multi-model study :raw-latex:`\citep{StjernEA2016}`.

2015 – 5 papers
---------------

AMAP Assessment 2015: Methane as an Arctic climate forcing
:raw-latex:`\citep{AMAP2015}`.

Current model capabilities for simulating black carbon and sulfate
concentrations in the Arctic atmosphere: a multi-model evaluation using
a comprehensive measurement data set
:raw-latex:`\citep{EckhardtEA2015}`.

Impact of coupled NOx-aerosol aircraft emissions on ozone photochemistry
and radiative forcing :raw-latex:`\citep{PitariEA2015}`.

Measuring and Modeling the Lifetime of Nitrous Oxide including its
Variability :raw-latex:`\citep{PratherEA2015}`.

Evaluating the climate and air quality impacts of short-lived pollutants
:raw-latex:`\citep{StohlEA2015}`.

2014 – 11 papers
----------------

Forty-seven years of weekly atmospheric black carbon measurements in the
Finnish Arctic: Decrease in black carbon with declining emissions
:raw-latex:`\citep{DutkiewiczEA2014}`.

Climate penalty for shifting shipping to the Arctic
:raw-latex:`\citep{FuglestvedtEA2014}`.

How shorter black carbon lifetime alters its climate effect
:raw-latex:`\citep{HodnebrogEA2014}`.

Atmospheric Ozone and Methane in a Changing Climate
:raw-latex:`\citep{IsaksenEA2014a}`.

An AeroCom assessment of black carbon in Arctic snow and sea ice
:raw-latex:`\citep{JiaoEA2014}`.

Climate impacts of short-lived climate forcers versus CO2 from
biodiesel: A case of the EU on-road sector
:raw-latex:`\citep{LundEA2014a}`.

Global and regional climate impacts of black carbon and co-emitted
species from the on-road diesel sector :raw-latex:`\citep{LundEA2014b}`.

Modelled black carbon radiative forcing and atmospheric lifetime in
AeroCom Phase II constrained by aircraft observations
:raw-latex:`\citep{SamsetEA2014}`.

Aircraft emission mitigation by changing route altitude: A multi-model
estimate of aircraft NOx emission impact on O3 photochemistry
:raw-latex:`\citep{SovdeEA2014}`.

The AeroCom evaluation and intercomparison of organic aerosol in global
models :raw-latex:`\citep{TsigaridisEA2014}`.

The influence of future non-mitigated road transport emissions on
regional ozone exceedences at global scale
:raw-latex:`\citep{WilliamsEA2014}`.

2013 – 20 papers
----------------

Evaluation of ACCMIP outgoing longwave radiation from tropospheric ozone
using TES satellite observations :raw-latex:`\citep{BowmanEA2013}`.

Environmental impacts of shipping in 2030 with a particular focus on the
Arctic region :raw-latex:`\citep{DalsorenEA2013}`.

Reducing CO2 from shipping - do non-CO2 effects matter?
:raw-latex:`\citep{EideEA2013}`.

Ozone Variations Derived by a Chemical Transport Model
:raw-latex:`\citep{EleftheratosEA2013}`.

Elemental carbon measurements in European Arctic snow packs
:raw-latex:`\citep{ForsstromEA2013}`.

Improvements to the retrieval of tropospheric NO2 from satellite
stratospheric correction using SCIAMACHY limb/nadir matching and
comparison to Oslo CTM2 simulations :raw-latex:`\citep{HilbollEA2013}`.

Global Warming Potentials and Radiative Efficiencies of Halocarbons and
Related Compounds: A Comprehensive Review
:raw-latex:`\citep{HodnebrogEA2013}`.

Future methane, hydroxyl, and their uncertainties: key climate and
emission parameters for future predictions
:raw-latex:`\citep{HolmesEA2013}`.

Multi-model mean nitrogen and sulfur deposition from the Atmospheric
Chemistry and Climate Model Intercomparison Project (ACCMIP): evaluation
of historical and projected future changes
:raw-latex:`\citep{LamarqueEA2013b}`.

The Atmospheric Chemistry and Climate Model Intercomparison Project
(ACCMIP): overview and description of models, simulations and climate
diagnostics :raw-latex:`\citep{LamarqueEA2013a}`.

Evaluation of preindustrial to present-day black carbon and its albedo
forcing from Atmospheric Chemistry and Climate Model Intercomparison
Project (ACCMIP) :raw-latex:`\citep{LeeEA2013}`.

Radiative forcing of the direct aerosol effect from AeroCom Phase II
simulations :raw-latex:`\citep{MyhreEA2013}`.

Preindustrial to present-day changes in tropospheric hydroxyl radical
and methane lifetime from the Atmospheric Chemistry and Climate Model
Intercomparison Project (ACCMIP) :raw-latex:`\citep{NaikEA2013}`.

Comparison of spheroidal carbonaceous particle (SCP) data with modelled
atmospheric black carbon concentration and deposition, and airmass
sources in northern Europe, 1850-2010 :raw-latex:`\citep{RuppelEA2013}`.

Black carbon vertical profiles strongly affect its radiative forcing
uncertainty :raw-latex:`\citep{SamsetEA2013}`.

Radiative forcing in the ACCMIP historical and future climate
simulations :raw-latex:`\citep{ShindellEA2013}`.

Global premature mortality due to anthropogenic outdoor air pollution
and the contribution of past climate change
:raw-latex:`\citep{SilvaEA2013}`.

Tropospheric ozone changes, radiative forcing and attribution to
emissions in the Atmospheric Chemistry and Climate Model
Inter-comparison Project (ACCMIP) :raw-latex:`\citep{StevensonEA2013}`.

Analysis of present day and future OH and methane lifetime in the ACCMIP
simulations :raw-latex:`\citep{VoulgarakisEA2013}`.

Pre-industrial to end 21st century projections of tropospheric ozone
from the Atmospheric Chemistry and Climate Model Intercomparison Project
(ACCMIP) :raw-latex:`\citep{YoungEA2013}`.

2012 – 8 papers
---------------

Future air quality in Europe: a multi-model assessment of projected
exposure to ozone :raw-latex:`\citep{ColetteEA2012}`.

Global air quality and climate :raw-latex:`\citep{FioreEA2012}`.

Future impact of traffic emissions on atmospheric ozone and OH based on
two scenarios :raw-latex:`\citep{HodnebrogEA2012}`.

Attribution of the Arctic ozone column deficit in March 2011
:raw-latex:`\citep{IsaksenEA2012}`.

Application of the CALIOP Layer Product to evaluate the vertical
distribution of aerosols estimated by global models: Part 1. AeroCom
phase I results :raw-latex:`\citep{KoffiEA2012}`.

Parameterization of black carbon aging in the OsloCTM2 and implications
for regional transport to the Arctic
:raw-latex:`\citep{LundBerntsen2012}`.

The chemical transport model Oslo CTM3 :raw-latex:`\citep{SovdeEA2012}`.

Short-lived climate forcers from current shipping and petroleum
activities in the Arctic :raw-latex:`\citep{OdemarkEA2012}`.

2011 – 16 papers
----------------

Inferring absorbing organic carbon content from AERONET data
:raw-latex:`\citep{ArolaEA2011}`.

Observed and Modelled record ozone decline over the Arctic during
winter/spring 2011 :raw-latex:`\citep{BalisEA2011}`.

Air quality trends in Europe over the past decade: a first multi-model
assessment :raw-latex:`\citep{ColetteEA2011}`.

A note on the comparison between total ozone from Oslo CTM2 and SBUV
satellite data :raw-latex:`\citep{EleftheratosEA2011}`.

Future impact of non-land based traffic emissions on atmospheric ozone
and OH - an optimistic scenario and a possible mitigation strategy
:raw-latex:`\citep{HodnebrogEA2011}`.

A review of the anthropogenic influence on biogenic secondary organic
aerosol :raw-latex:`\citep{HoyleEA2011b}`.

Representation of tropical deep convection in atmospheric models - Part
2: Tracer transport :raw-latex:`\citep{HoyleEA2011}`.

Global dust model intercomparison in AeroCom phase I
:raw-latex:`\citep{HuneeusEA2011}`.

Strong atmospheric chemistry feedback to climate warming from Arctic
methane emissions :raw-latex:`\citep{IsaksenEA2011}`.

Radiative forcing due to changes in ozone and methane caused by the
transport sector :raw-latex:`\citep{MyhreEA2011}`.

Representation of tropical deep convection in atmospheric models - Part
1: Meteorology and comparison with satellite observations
:raw-latex:`\citep{RussoEA2011}`.

Vertical dependence of black carbon, sulphate and biomass burning
aerosol radiative forcing :raw-latex:`\citep{SamsetMyhre2011}`.

Black carbon in the atmosphere and snow, from pre-industrial times until
present :raw-latex:`\citep{SkeieEA2011}`.

Anthropogenic radiative forcing time series from pre-industrial times
until 2010 :raw-latex:`\citep{SkeieEA2011b}`.

The HNO3 forming branch of the HO2 + NO reaction:
pre-industrial-to-present trends in atmospheric species and radiative
forcings :raw-latex:`\citep{SovdeEA2011b}`.

Estimation of Arctic Ozone Loss in the Winter 2006/07 using a Chemical
Transport Model and Data Assimilation :raw-latex:`\citep{SovdeEA2011}`.

2010 – 4 papers
---------------

Direct radiative effect of aerosols emitted by transport: from road,
shipping and aviation :raw-latex:`\citep{BalkanskiEA2010}`.

Impacts of the Large Increase in International Ship Traffic 2000-2007 on
Tropospheric Ozone and Methane :raw-latex:`\citep{DalsorenEA2010}`.

Transport impacts on atmosphere and climate: Aviation
:raw-latex:`\citep{LeeEA2010}`.

Transport impacts on atmosphere and climate: Land transport
:raw-latex:`\citep{UherekEA2010}`.

2009 – 13 papers
----------------

Effect of emission changes in southeast Asia on global hydroxyl and
methane levels :raw-latex:`\citep{DalsorenEA2009}`.

Update on emissions and environmental impacts from the international
fleet of ships: the contribution from major ship types and ports
:raw-latex:`\citep{DalsorenEA2009b}`.

Transport impacts on atmosphere and climate: Shipping
:raw-latex:`\citep{EyringEA2009}`.

The impact of traffic emissions on atmospheric ozone and OH: results
from QUANTIFY :raw-latex:`\citep{HoorEA2009}`.

Anthropogenic influence on SOA and the resulting radiative forcing
:raw-latex:`\citep{HoyleEA2009}`.

Present-day contribution of anthropogenic emissions from China to the
global burden and radiative forcing of aerosol and ozone
:raw-latex:`\citep{HoyleEA2009b}`.

Atmospheric composition change: Climate-Chemistry interactions
:raw-latex:`\citep{IsaksenEA2009}`.

Evaluation of black carbon estimations in global aerosol models
:raw-latex:`\citep{KochEA2009}`.

Modelled radiative forcing of the direct aerosol effect with
multi-observation evaluation :raw-latex:`\citep{MyhreEA2009}`.

Consistency Between Satellite-Derived and Modeled Estimates of the
Direct Aerosol Effect :raw-latex:`\citep{Myhre2009}`.

Modelling of chemical and physical aerosol properties during the ADRIEX
aerosol campaign :raw-latex:`\citep{MyhreEA2009b}`.

The influence of foreign vs. North American emissions on surface ozone
in the US :raw-latex:`\citep{ReidmillerEA2009}`.

Costs and global impacts of black carbon abatement strategies
:raw-latex:`\citep{RypdalEA2009}`.

2008 – 4 papers
---------------

Climate forcing from the transport sectors
:raw-latex:`\citep{FuglestvedtEA2009}`.

Modeling of the solar radiative impact of biomass burning aerosols
during the Dust and Biomass burning Experiment (DABEX)
:raw-latex:`\citep{MyhreEA2008}`.

European surface ozone in the extreme summer 2003
:raw-latex:`\citep{SolbergEA2008}`.

Evaluation of the chemical transport model Oslo CTM2 with focus on
Arctic winter ozone depletion :raw-latex:`\citep{SovdeEA2008}`.

2007 – 10 papers
----------------

Sulphate trends in Europe: are we able to model the recent observed
decrease? :raw-latex:`\citep{BerglenEA2007}`.

Environmental impacts of the expected increase in sea transportation,
with a particular focus on oil and gas scenarios for Norway and
Northwest Russia :raw-latex:`\citep{DalsorenEA2007}`.

Changes in Nitrogen Dioxide and Ozone over Southeast and East Asia
between Year 2000 and 2030 with Fixed Meteorology
:raw-latex:`\citep{GaussEA2007}`.

Climate impact of supersonic air traffic: An approach to optimize a
potential future supersonic fleet - Results from the EU-project SCENIC
:raw-latex:`\citep{GreweEA2007}`.

Secondary organic aerosol in the global aerosol - chemical transport
model Oslo CTM2 :raw-latex:`\citep{HoyleEA2007}`.

A Study of Tropospheric Ozone over China with a 3-D Global CTM Model
:raw-latex:`\citep{LiuEA2007}`.

Comparison of the radiative properties and direct radiative effect of
aerosols from a global aerosol model and remote sensing data over ocean
:raw-latex:`\citep{MyhreEA2007b}`.

Aerosol-cloud interaction inferred from MODIS satellite data and global
aerosol models :raw-latex:`\citep{MyhreEA2007c}`.

Aircraft pollution: A futuristic view :raw-latex:`\citep{SovdeEA2007}`.

The effect of harmonized emissions on aerosol properties in global
models - an AeroCom experiment :raw-latex:`\citep{TextorEA2007}`.

2006 – 13 papers
----------------

Abatement of Greenhouse Gases: Does Location Matter?
:raw-latex:`\citep{BerntsenEA2006}`.

CTM study of changes in tropospheric hydroxyl distribution 1990-2001 and
its impact on methane :raw-latex:`\citep{DalsorenIsaksen2006}`.

Nitrogen and sulfur deposition on regional and global scales: A
multimodel evaluation :raw-latex:`\citep{DentenerEA2006b}`.

The Global Atmospheric Environment for the Next Generation
:raw-latex:`\citep{DentenerEA2006a}`.

Impact of aircraft NOx emissions on the atmosphere - tradeoffs to reduce
the impact :raw-latex:`\citep{GaussEA2006}`.

Radiative forcing since preindustrial times due to ozone change in the
troposphere and the lower stratosphere
:raw-latex:`\citep{GaussEA2006b}`.

An AeroCom initial assessment - optical properties in aerosol component
modules of global models :raw-latex:`\citep{KinneEA2006}`.

Modelling of nitrate and ammonium-containing aerosols in presence of sea
salt :raw-latex:`\citep{MyhreEA2006a}`.

Multi-model ensemble simulations of tropospheric NO2 compared with GOME
retrievals for the year 2000 :raw-latex:`\citep{vanNoijeEA2006}`.

Radiative forcing by aerosols as derived from the AeroCom present-day
and pre-industrial simulations :raw-latex:`\citep{SchulzEA2006}`.

Multimodel simulations of carbon monoxide: Comparison with observations
and projected near-future changes :raw-latex:`\citep{ShindellEA2006}`.

Multimodel ensemble simulations of present-day and near-future
tropospheric ozone :raw-latex:`\citep{StevensonEA2006}`.

Analysis and quantification of the diversities of aerosol life cycles
within AeroCom :raw-latex:`\citep{TextorEA2006}`.

2005 – 5 papers
---------------

Response of climate to regional emissions of ozone precursors:
sensitivities and warming potentials
:raw-latex:`\citep{BerntsenEA2005}`.

An evaluation of the performance of chemistry transport models - Part 2:
Detailed comparison with two selected campaigns
:raw-latex:`\citep{BrunnerEA2005}`.

Model simulations of dust sources and transport in the global
atmosphere: Effects of soil erodibility and wind speed variability
:raw-latex:`\citep{GriniEA2005}`.

Tropospheric ozone changes at unpolluted and semipolluted regions
induced by stratospheric ozone changes
:raw-latex:`\citep{IsaksenEA2005}`.

Radiative effect of surface albedo change from biomass burning
:raw-latex:`\citep{MyhreEA2005}`.

2004 – 4 papers
---------------

A global model of the coupled sulfur/oxidant chemistry in the
troposphere: The sulfur cycle :raw-latex:`\citep{BerglenEA2004}`.

Roles of saltation, sandblasting, and wind speed variability on mineral
dust aerosol size distribution during the Puerto Rican Dust Experiment
(PRIDE) :raw-latex:`\citep{GriniZender2004}`.

NOx change over China and its influences :raw-latex:`\citep{LiuEA2004}`.

Uncertainties in the Radiative Forcing Due to Sulfate Aerosols
:raw-latex:`\citep{MyhreEA2004}`.

2003 – 10 papers
----------------

An evaluation of the performance of chemistry transport models by
comparison with research aircraft observations. Part 1: Concepts and
overall model performance :raw-latex:`\citep{BrunnerEA2003}`.

Emission from international sea transportation and environmental impact
:raw-latex:`\citep{EndresenEA2003}`.

Impact of H2O emissions from cryoplanes and kerosene aircraft on the
atmosphere :raw-latex:`\citep{GaussEA2003a}`.

Radiative forcing in the 21st century due to ozone changes in the
troposphere and the lower stratosphere
:raw-latex:`\citep{GaussEA2003c}`.

Impact of aircraft NOx emission on NOx and ozone over China
:raw-latex:`\citep{LiuEA2003a}`.

The possible influences of the increasing anthropogenic emissions in
India on tropospheric ozone and OH :raw-latex:`\citep{LiuEA2003b}`.

Modeling the radiative impact of mineral dust during the Saharan Dust
Experiment (SHADE) campaign :raw-latex:`\citep{MyhreEA2003b}`.

Modeling the solar radiative impact of aerosols from biomass burning
during the Southern African Regional Science Initiative (SAFARI-2000)
experiment :raw-latex:`\citep{MyhreEA2003a}`.

Fresh air in the 21st century? :raw-latex:`\citep{PratherEA2003}`.

Chemical transport model ozone simulations for spring 2001 over the
western Pacific: Comparisons with TRACE-P lidar, ozonesondes, and Total
Ozone Mapping Spectrometer columns :raw-latex:`\citep{WildEA2003}`.

2002 – 4 papers
---------------

Modeling the Annual Cycle of Sea Salt in the Global 3D Model Oslo CTM2:
Concentrations, Fluxes, and Radiative Impact
:raw-latex:`\citep{GriniEA2002}`.

Impacts of NOx emissions from subsonic aircraft in a global
three-dimensional chemistry transport model including plume processes
:raw-latex:`\citep{KraabolEA2002}`.

Model intercomparison of the transport of aircraft-like emissions from
sub- and supersonic aircraft :raw-latex:`\citep{RogersEA2002}`.

Photochemical Activity and Solar Ultraviolet Radiation (PAUR) Modulation
Factors: An overview of the project :raw-latex:`\citep{ZerefosEA2002}`.

2001 – 4 papers
---------------

Atmospheric degradation and global warming potentials of three
perfluoroalkenes :raw-latex:`\citep{AcerboniEA2001}`.

Chemistry-transport model comparison with ozone observations in the
midlatitude lowermost stratosphere :raw-latex:`\citep{BregmanEA2001}`.

Model calculations of present and future levels of ozone and ozone
precursors with a global and a regional model
:raw-latex:`\citep{JonsonEA2001}`.

Sulphate particles from subsonic aviation: impact on upper tropospheric
and lower stratospheric ozone :raw-latex:`\citep{PitariEA2001}`.

Oslo CTM1 – 12 papers
---------------------

NOx Emissions from Aircraft: Its Impact on the Global Distribution of
CH4 and O3 and on Radiative Forcing :raw-latex:`\citep{IsaksenEA2001}`.

Time evolution of tropospheric ozone and its radiative forcing
:raw-latex:`\citep{BerntsenEA2000}`.

Trend analysis of O3 and CO in the period 1980-1996: A three-dimensional
model study :raw-latex:`\citep{KarlsdottirEA2000}`.

Radiative forcing due to changes in tropospheric ozone in the period
1980 to 1996 :raw-latex:`\citep{MyhreEA2000}`.

Effects of lightning and convection on changes in tropospheric ozone due
to NOx emissions from aircraft :raw-latex:`\citep{BerntsenEA1999}`.

Influence of Asian emissions on the composition of air reaching the
north western United States :raw-latex:`\citep{BerntsenEA1999}`.

3-D global simulations of tropospheric CO distributions: Results of the
GIM/IGAC intercomparison 1997 exercise
:raw-latex:`\citep{KanakidouEA1999}`.

Estimation of the direct radiative forcing due to sulfate and soot
aerosols :raw-latex:`\citep{MyhreEA1998}`.

Global distribution of sulphate in the troposphere: A three-dimensional
model study :raw-latex:`\citep{RestadEA1998}`.

Effects of anthropogenic emissions on tropospheric ozone and its
radiative forcing :raw-latex:`\citep{BerntsenEA1997}`.

A global three-dimensional chemical transport model: 2. Nitrogen oxides
and nonmethane hydrocarbon results :raw-latex:`\citep{JaffeEA1997}`.

A global three-dimensional chemical transport model for the troposphere:
1. Model description and CO and Ozone results
:raw-latex:`\citep{BerntsenIsaksen1997}`.

Contact information
===================

| CICERO
| P.O. Box 1129 Blindern
| N-0318 Oslo
| Norway
| e-mail: post@cicero.oslo.no
| web: http://www.cicero.oslo.no/

The easiest way of getting in touch with us is to send an e-mail to
CICERO or to the Oslo CTM3 user mailing list.

| Oslo CTM3 mail list: ``oslo-ctm@geo.uio.no``

Acknowledgements
================

Parts of this manual was written during my time at the Department of
Geosciences at the University of Oslo, although considerable amount of
it, including revisions, have been written on my spare time or during
model development in projects at CICERO.

Thanks to Alf Grini for starting the Oslo CTM2 manual; some sections in
this manual are based on his work. Also thanks to EGU for
LaTeX bibliography style.

I (Amund) left CICERO and Oslo CTM3 in February 2018, and hope this
manual will be a good tool for future users of Oslo CTM3.

--------------

::

    Tracer       SOLU CHN TCHENA  TCHENB  TCKAQA TCKAQB   ISCVFR  IT
    O3           1.00  1  1.1d-02   2400     0      0     0.0d-00  0 
    OH           1.00  0  2.5d+01   5300     0      0     0.0d-00  0 
    HO2          1.00  1  4.0d+03   5900     0      0     0.0d-00  0 
    NO           1.00  0  1.9d-03   1400     0      0     0.0d-00  0 
    NO3          1.00  1  2.0d+00   2000     0      0     0.0d-00  0 
    NO2          1.00  0  1.2d-02   2500     0      0     0.0d-00  0 
    N2O5         1.00  1  2.1d+00   3400     0      0     0.0d-00  0 
    HONO         1.00  0  4.9d+01   4800     0      0     0.0d-00  0 
    HNO3         1.00  3  2.1d+05   8700     1      1     1.0d-00  1 
    HO2NO2       1.00  1  1.2d+04   6900     0      0     0.0d-00  0 
    H2O2         1.00  1  7.1d+04   6800     0      1     0.0d-00  0 
    CH4          1.00  0  1.4d-03   1600     0      0     0.0d-00  0 
    CH3O2        1.00  1  2.0d+03   6600     0      0     0.0d-00  0 
    CH3O2H       1.00  1  3.1d+02   5200     0      0     0.0d-00  0 
    CH2O         1.00  1  3.2d+03   6800     1      0     0.0d-00  0 
    CO           1.00  0  9.5d-04   1300     0      0     0.0d-00  0 
    O2           1.00  0  1.3d-03   1500     0      0     0.0d-00  0 
    N2           1.00  0  6.1d-04   1300     0      0     0.0d-00  0 
    C2H6         1.00  0  1.9d-03   2300     0      0     0.0d-00  0 
    C2H5O2       1.00  1  2.2d+03   7200     0      0     0.0d-00  0 
    EtOOH        1.00  0  3.4d+02   6000     0      0     0.0d-00  0 
    CH3CHO       1.00  1  1.4d+01   5600     0      0     0.0d-00  0 
    MeCO3        1.00  0  1.0d-01      0     0      0     0.0d-00  0 
    PANX         1.00  1  2.9d+00   5900     0      0     0.0d-00  0 
    Alkane       1.00  0  1.2d-03   3100     0      0     0.0d-00  0 
    ROHOO        1.00  0  0.0d+00      0     0      0     0.0d-00  0 
    Alkene       1.00  0  7.4d-03   3400     0      0     0.0d-00  0 
    Aromatic     1.00  0  1.5d-01   4000     0      0     0.0d-00  0 
    ISOPRENE     1.00  0  1.3d-02      0     0      0     0.0d-00  0 
    MVKMACR      1.00  0  2.1d+01   7800     0      0     0.0d-00  0 
    ISOK         1.00  1  1.3d+01   7800     0      0     0.0d-00  0 
    H2S          1.00  0  8.7d-02   2100     0      0     0.0d-00  0 
    DMS          1.00  0  4.8d-01   3100     0      0     0.0d-00  0 
    Me2SO        1.00  0  5.0d+04      0     0      0     0.0d-00  0 
    Me2SO2       1.00  0  5.0d+04      0     0      0     0.0d-00  0 
    MeSO3H       1.00  0  1.0d+08      0     0      0     0.0d-00  0 
    MSA          1.00  3  5.0d+04      0     0      0     0.0d-00  0 
    SO2          1.00  1  1.2d+00   3020     1      0     0.0d-00  0 
    SO3          1.00  0  1.0d+08      0     0      0     0.0d-00  0 
    SO4          1.00  3  1.0d+08      0     0      1     0.1d-00  0 
    SF6          1.00  0  2.4d-04   2400     0      0     0.0d-00  0 
    Rn           1.00  0  9.3d-03   2600     0      0     0.0d-00  0 
    omBB1fil     1.00  3  1.0d+08      0     0      1     0.1d-00  4 
    omFF1fil     1.00  3  1.0d+08      0     0      1     0.1d-00  4 
    omBF1fil     1.00  3  1.0d+08      0     0      1     0.1d-00  4 
    omOCNfil     1.00  3  1.0d+08      0     0      1     0.1d-00  4 
    omBB1fob     1.00  6  1.0d+08      0     0      1     0.2d-00  4 
    omFF1fob     1.00  6  1.0d+08      0     0      1     0.2d-00  4 
    omBF1fob     1.00  6  1.0d+08      0     0      1     0.2d-00  4 
    omOCNfob     1.00  6  1.0d+08      0     0      1     0.2d-00  4 
    bcBB1fil     1.00  3  1.0d+08      0     0      1     0.1d-00  4 
    bcFF1fil     1.00  3  1.0d+08      0     0      1     0.1d-00  4 
    bcBF1fil     1.00  3  1.0d+08      0     0      1     0.1d-00  4 
    bcBB1fob     1.00  6  1.0d+08      0     0      1     0.2d-00  4 
    bcFF1fob     1.00  6  1.0d+08      0     0      1     0.2d-00  4 
    bcBF1fob     1.00  6  1.0d+08      0     0      1     0.2d-00  4 

--------------

--------------

::

    Tracer       SOLU CHN TCHENA  TCHENB  TCKAQA TCKAQB   ISCVFR  IT
    SALT01       1.00  3  1.0d+08      0     0      1     0.1d-00  0 
    SALT02       1.00  3  1.0d+08      0     0      1     0.1d-00  0 
    SALT03       1.00  3  1.0d+08      0     0      1     0.1d-00  0 
    SALT04       1.00  3  1.0d+08      0     0      1     0.1d-00  0 
    SALT05       1.00  3  1.0d+08      0     0      1     0.1d-00  0 
    SALT06       1.00  3  1.0d+08      0     0      1     0.1d-00  0 
    SALT07       1.00  3  1.0d+08      0     0      1     0.1d-00  0 
    SALT08       1.00  3  1.0d+08      0     0      1     0.1d-00  0 
    DUST01       1.00  3  1.0d+08      0     0      1     0.5d-00  4 
    DUST02       1.00  3  1.0d+08      0     0      1     0.5d-00  4 
    DUST03       1.00  3  1.0d+08      0     0      1     0.5d-00  4 
    DUST04       1.00  3  1.0d+08      0     0      1     0.5d-00  4 
    DUST05       1.00  3  1.0d+08      0     0      1     0.5d-00  4 
    DUST06       1.00  3  1.0d+08      0     0      1     0.5d-00  4 
    DUST07       1.00  3  1.0d+08      0     0      1     0.5d-00  4 
    DUST08       1.00  3  1.0d+08      0     0      1     0.5d-00  4 
    HNO3s        1.00  3  2.1d+05   8700     1      1     1.0d-00  1 
    NH3          1.00  1  3.3d+06      0     0      0     0.0d-00  0 
    NH4fine      1.00  3  1.0d+08      0     0      1     0.1d-00  0 
    NH4coarse    1.00  3  1.0d+08      0     0      1     0.1d-00  0 
    NO3fine      1.00  3  1.0d+08      0     0      1     0.1d-00  0 
    NO3coarse    1.00  3  1.0d+08      0     0      1     0.1d-00  0 

--------------

--------------

::

    Tracer       SOLU CHN TCHENA  TCHENB  TCKAQA TCKAQB   ISCVFR  IT
    SOAGAS11     1.00  1  1.0d+05     12     0      0     1.0d-00  0 
    SOAGAS21     1.00  1  1.0d+05     12     0      0     1.0d-00  0 
    SOAGAS31     1.00  1  1.0d+05     12     0      0     1.0d-00  0 
    SOAGAS41     1.00  1  1.0d+05     12     0      0     1.0d-00  0 
    SOAGAS51     1.00  1  1.0d+05     12     0      0     1.0d-00  0 
    SOAGAS12     1.00  1  1.0d+05     12     0      0     1.0d-00  0 
    SOAGAS22     1.00  1  1.0d+05     12     0      0     1.0d-00  0 
    SOAGAS32     1.00  1  1.0d+05     12     0      0     1.0d-00  0 
    SOAGAS42     1.00  1  1.0d+05     12     0      0     1.0d-00  0 
    SOAGAS52     1.00  1  1.0d+05     12     0      0     1.0d-00  0 
    SOAGAS13     1.00  1  1.0d+05     12     0      0     1.0d-00  0 
    SOAGAS23     1.00  1  1.0d+05     12     0      0     1.0d-00  0 
    SOAGAS33     1.00  1  1.0d+05     12     0      0     1.0d-00  0 
    SOAGAS43     1.00  1  1.0d+05     12     0      0     1.0d-00  0 
    SOAGAS53     1.00  1  1.0d+05     12     0      0     1.0d-00  0 
    SOAGAS61     1.00  1  1.0d+05     12     0      0     1.0d-00  0 
    SOAGAS62     1.00  1  1.0d+05     12     0      0     1.0d-00  0 
    SOAGAS71     1.00  1  1.0d+04     12     0      0     1.0d-00  0 
    SOAGAS72     1.00  1  1.0d+03     12     0      0     1.0d-00  0 
    SOAGAS81     1.00  1  1.0d+03     12     0      0     1.0d-00  0 
    SOAGAS82     1.00  1  1.0d+03     12     0      0     1.0d-00  0 
    Apine        1.00  1  2.3d-02      0     0      0     1.0d-00  0 
    Bpine        1.00  1  2.3d-02      0     0      0     1.0d-00  0 
    Sabine       1.00  1  2.3d-02      0     0      0     1.0d-00  0 
    D3carene     1.00  1  2.3d-02      0     0      0     1.0d-00  0 
    Trp_Ket      1.00  1  2.3d-02      0     0      0     1.0d-00  0 
    Limon        1.00  1  7.0d-02      0     0      0     1.0d-00  0 
    Trpolene     1.00  1  6.7d-02      0     0      0     1.0d-00  0 
    Trpinene     1.00  1  6.7d-02      0     0      0     1.0d-00  0 
    Myrcene      1.00  1  5.4d+01      0     0      0     1.0d-00  0 
    Ocimene      1.00  1  5.4d+01      0     0      0     1.0d-00  0 
    TrpAlc       1.00  1  5.4d+01      0     0      0     1.0d-00  0 
    Sestrp       1.00  1  4.9d-02      0     0      0     1.0d-00  0 
    SOAAER11     0.80  3  1.0d+08      0     0      1     2.0d-01  4 
    SOAAER21     0.80  3  1.0d+08      0     0      1     2.0d-01  4 
    SOAAER31     0.80  3  1.0d+08      0     0      1     2.0d-01  4 
    SOAAER41     0.80  3  1.0d+08      0     0      1     2.0d-01  4 
    SOAAER51     0.80  3  1.0d+08      0     0      1     2.0d-01  4 
    SOAAER12     0.80  3  1.0d+08      0     0      1     2.0d-01  4 
    SOAAER22     0.80  3  1.0d+08      0     0      1     2.0d-01  4 
    SOAAER32     0.80  3  1.0d+08      0     0      1     2.0d-01  4 
    SOAAER42     0.80  3  1.0d+08      0     0      1     2.0d-01  4 
    SOAAER52     0.80  3  1.0d+08      0     0      1     2.0d-01  4 
    SOAAER13     0.80  3  1.0d+08      0     0      1     2.0d-01  4 
    SOAAER23     0.80  3  1.0d+08      0     0      1     2.0d-01  4 
    SOAAER33     0.80  3  1.0d+08      0     0      1     2.0d-01  4 
    SOAAER43     0.80  3  1.0d+08      0     0      1     2.0d-01  4 
    SOAAER53     0.80  3  1.0d+08      0     0      1     2.0d-01  4 
    SOAAER61     0.80  3  1.0d+08      0     0      1     2.0d-01  4 
    SOAAER62     0.80  3  1.0d+08      0     0      1     2.0d-01  4 
    SOAAER71     0.80  3  1.0d+08      0     0      1     2.0d-01  4 
    SOAAER72     0.80  3  1.0d+08      0     0      1     2.0d-01  4 
    SOAAER81     0.80  3  1.0d+08      0     0      1     2.0d-01  4 
    SOAAER82     0.80  3  1.0d+08      0     0      1     2.0d-01  4

--------------

| \|l\|l\|l\|l\|c\|c\|c\| Field & CTM & & & & UiO & UiO
| name & name &Description & Unit & I/A & ID & file
| lon & XDGRD\ :math:`^{\dagger}` & Longitude at grid center & degree
  East &—&—&—
| lat & YDGRD\ :math:`^{\dagger}` & Latitude at grid center & degree
  North &—&—&—
| ilon & XDEDG\ :math:`^{\dagger}` & Longitude at interstices & degree
  East &—&—&—
| ilat & YDEDG\ :math:`^{\dagger}` & Latitude at interstices & degree
  North &—&—&—
| hyai & ETAA\ :math:`^{\dagger}` & Hybrid sigma coordinate A at
  interstices & hPa &—&—&—
| hybi & ETAB\ :math:`^{\dagger}` & Hybrid sigma coordinate B at
  interstices & 0–1 &—&—&—
| data\_date & — & Date and UTC hour for data &—&—&—&—
| & & given as YYYYMMDDHH & & & &
| time\_span & — & Time span of accumulated period & s &—&—&—
| & & i.e. meteorological time step & & & &
| temperature & T & Temperature & K & I & 130 & SPE
| U & U & Horizontal wind at grid center\ :math:`^*` & m/s\*cos(lat)
  &I&—&—
| & & Unit is converted in CTM: &kg/s & & &
| V & V & Meridional wind at interstices\ :math:`^*` & m/s\*cos(lat)
  &I&—&—
| & & Unit is converted in CTM: &kg/s & & &
| & & Vorticity, relative\ :math:`^*`\ & s\ :math:`^{-1}` & I & 138 &
  SPE
| & & Divergence\ :math:`^*` & s\ :math:`^{-1}` & I & 155 & SPE
| qhum & Q &Specific humidity & kg/kg & I & 133 & GG
| lwc &CLDLWC& Cloud liquid water content & kg/kg & I & 246 & GG
| iwc &CLDIWC& Cloud ice water content & kg/kg & I & 247 & GG
| cfr &CLDFR & Cloud cover & (0–1) & I & 248 & GG
| cflxu&—& Mass flux into updrafts &
  kgm\ :math:`^{-2}`\ s\ :math:`^{-1}` &A&212&GG
| & & (1/2-levels starting first level above sfc) & & & &
| &CWETE& Unit is converted in CTM: &kgs\ :math:`^{-1}` & & &
| cflxd&—& Mass flux into downdrafts &
  kgm\ :math:`^{-2}`\ s\ :math:`^{-1}` &A&213&GG
| & & (1/2-levels starting first level above sfc) & & & &
| &CWETD& Unit is converted in CTM: &kgs\ :math:`^{-1}` & & &
| cdetu& & Mass flux detrainment into updrafts
  &kgm\ :math:`^{-3}`\ s\ :math:`^{-1}`\ &A&214&GG
| &CDETU& Unit is converted in CTM: &kgs\ :math:`^{-1}` & & &
| cdetd& & Mass flux detrainment into downdrafts
  &kgm\ :math:`^{-3}`\ s\ :math:`^{-1}`\ &A&215&GG
| &CDETD& Unit is converted in CTM: &kgs\ :math:`^{-1}` & & &
| convrain&— & Convective precipitation &
  kgm\ :math:`^{-2}`\ s\ :math:`^{-1}`\ &A& 217 & GG
| & & (half-levels starting at surface) & & & &
| &PRECCNV& Unit is converted in CTM: &kgs\ :math:`^{-1}` & & &
| lsrain&— & Large scale precipitation &
  kgm\ :math:`^{-2}`\ s\ :math:`^{-1}`\ &A& 218 & GG
| & & (half-levels starting at surface) & & & &
| &PRECLS& Unit is converted in CTM: &kgs\ :math:`^{-1}` & & &
| PV & — & Potential Vorticity &K
  m\ :math:`^2`\ kg\ :math:`^{-1}`\ s\ :math:`^{-1}` &I&60&GG
| & PVU & Unit is converted to PVU in CTM: &10\ :math:`^6`\ PV & & &
| — &CENTU & Mass flux entrainment into updrafts &kgs\ :math:`^{-1}` & &
  &
| && Calculated from CWETE and CDETU. & & & &
| — &CENTD & Mass flux entrainment into downdrafts &kgs\ :math:`^{-1}` &
  & &
| && Calculated from CWETD and CDETD. & & & &
| — &ZOFLE & Height of grid box interstices (bottom) & m & & &

+--------------------+-------------+-------------------------------------------------------+-----------------------------------+-------+-------+--------+
| Field              | CTM         |                                                       |                                   |       | UiO   | UiO    |
+--------------------+-------------+-------------------------------------------------------+-----------------------------------+-------+-------+--------+
| name               | name        | Description                                           | Unit                              | I/A   | ID    | file   |
+--------------------+-------------+-------------------------------------------------------+-----------------------------------+-------+-------+--------+
| U10M               | SFU         | 10 metre U wind component                             | ms\ :math:`^{-1}`                 | I     | 165   | SFC    |
+--------------------+-------------+-------------------------------------------------------+-----------------------------------+-------+-------+--------+
| V10M               | SFV         | 10 metre V wind component                             | ms\ :math:`^{-1}`                 | I     | 166   | SFC    |
+--------------------+-------------+-------------------------------------------------------+-----------------------------------+-------+-------+--------+
| U10G               | —           | 10 metre wind gust                                    | ms\ :math:`^{-1}`                 | I     | 49    | SFC    |
+--------------------+-------------+-------------------------------------------------------+-----------------------------------+-------+-------+--------+
| TDEW2M             | —           | 2 metre dewpoint temperature                          | K                                 | I     | 168   | SFC    |
+--------------------+-------------+-------------------------------------------------------+-----------------------------------+-------+-------+--------+
|                    |             | to calculate specific humidity SFQ                    |                                   |       |       |        |
+--------------------+-------------+-------------------------------------------------------+-----------------------------------+-------+-------+--------+
| T2M                | SFT         | 2 metre temperature                                   | K                                 | I     | 167   | SFC    |
+--------------------+-------------+-------------------------------------------------------+-----------------------------------+-------+-------+--------+
| BLD                | —           | Boundary layer dissipation                            | Wm\ :math:`^{-2}`                 | A     | 145   | SFC    |
+--------------------+-------------+-------------------------------------------------------+-----------------------------------+-------+-------+--------+
| BLH                | BLH\_CUR    | Boundary layer height                                 | m                                 | I     | 159   | SFC    |
+--------------------+-------------+-------------------------------------------------------+-----------------------------------+-------+-------+--------+
|                    |             | for current time step                                 |                                   |       |       |        |
+--------------------+-------------+-------------------------------------------------------+-----------------------------------+-------+-------+--------+
| BLH                | BLH\_NEXT   | Boundary layer height                                 | m                                 | I     | 159   | SFC    |
+--------------------+-------------+-------------------------------------------------------+-----------------------------------+-------+-------+--------+
|                    |             | for next time step                                    |                                   |       |       |        |
+--------------------+-------------+-------------------------------------------------------+-----------------------------------+-------+-------+--------+
|                    | BLH         | Boundary layer height                                 | m                                 | I     | 159   | SFC    |
+--------------------+-------------+-------------------------------------------------------+-----------------------------------+-------+-------+--------+
|                    |             | Set from BLH\_CUR and BLH\_NEXT                       |                                   |       |       |        |
+--------------------+-------------+-------------------------------------------------------+-----------------------------------+-------+-------+--------+
| CHNK               | —           | Charnock parameter                                    | —                                 | I     | 148   | SFC    |
+--------------------+-------------+-------------------------------------------------------+-----------------------------------+-------+-------+--------+
| CAPE               | —           | Convective available potential energy                 | Jkg\ :math:`^{-1}`                | I     | 59    | SFC    |
+--------------------+-------------+-------------------------------------------------------+-----------------------------------+-------+-------+--------+
| CONVPREC           | —           | Convective precipitation                              | m                                 | A     | 143   | SFC    |
+--------------------+-------------+-------------------------------------------------------+-----------------------------------+-------+-------+--------+
| DUVRS              | —           | Downward UV radiation at the surface                  | Wm\ :math:`^{-2}`                 | A     | 57    | SFC    |
+--------------------+-------------+-------------------------------------------------------+-----------------------------------+-------+-------+--------+
| EWSS               | EWSS        | East-West surface stress                              | Nm\ :math:`^{-2}`                 | A     | 180   | SFC    |
+--------------------+-------------+-------------------------------------------------------+-----------------------------------+-------+-------+--------+
| E                  | —           | Evaporation                                           | m of water                        | A     | 182   | SFC    |
+--------------------+-------------+-------------------------------------------------------+-----------------------------------+-------+-------+--------+
| SA                 | SA          | Forecast surface albedo                               | 0–1                               | I     | 243   | SFC    |
+--------------------+-------------+-------------------------------------------------------+-----------------------------------+-------+-------+--------+
| FLZOH              | —           | Forecast ln(surface roughness for heat)               | ln(m)                             | I     | 245   | SFC    |
+--------------------+-------------+-------------------------------------------------------+-----------------------------------+-------+-------+--------+
| FSR                | —           | Forecast surface roughness                            | m                                 | I     | 244   | SFC    |
+--------------------+-------------+-------------------------------------------------------+-----------------------------------+-------+-------+--------+
| ZSFC               | —           | Surface Geopotential                                  | m\ :math:`^2`\ s\ :math:`^{-2}`   | I     | 129   | SFC    |
+--------------------+-------------+-------------------------------------------------------+-----------------------------------+-------+-------+--------+
| GWD                | —           | Gravity wave dissipation                              | W m\ :math:`^{-2}` s              | A     | 197   | SFC    |
+--------------------+-------------+-------------------------------------------------------+-----------------------------------+-------+-------+--------+
| HCC                | —           | High cloud cover                                      | 0–1                               | I     | 188   | SFC    |
+--------------------+-------------+-------------------------------------------------------+-----------------------------------+-------+-------+--------+
| ISTL1              | —           | Ice surface temperature layer 1                       | K                                 | I     | 35    | SFC    |
+--------------------+-------------+-------------------------------------------------------+-----------------------------------+-------+-------+--------+
| ISTL2              | —           | Ice surface temperature layer 2                       | K                                 | I     | 36    | SFC    |
+--------------------+-------------+-------------------------------------------------------+-----------------------------------+-------+-------+--------+
| ISTL3              | —           | Ice surface temperature layer 3                       | K                                 | I     | 37    | SFC    |
+--------------------+-------------+-------------------------------------------------------+-----------------------------------+-------+-------+--------+
| ISTL4              | —           | Ice surface temperature layer 4                       | K                                 | I     | 38    | SFC    |
+--------------------+-------------+-------------------------------------------------------+-----------------------------------+-------+-------+--------+
| LSM                | —           | Land-sea mask                                         | 0–1                               | I     | 172   | SFC    |
+--------------------+-------------+-------------------------------------------------------+-----------------------------------+-------+-------+--------+
| LSPF               | —           | Large-scale precipitation fraction                    | 0–1                               | A     | 50    | SFC    |
+--------------------+-------------+-------------------------------------------------------+-----------------------------------+-------+-------+--------+
| LSPREC             | LSPREC      | Large-scale precipitation                             | m                                 | A     | 142   | SFC    |
+--------------------+-------------+-------------------------------------------------------+-----------------------------------+-------+-------+--------+
| LGWS               | —           | Latitudinal component of gravity wave stress          | Nm\ :math:`^{-2}`                 | A     | 195   | SFC    |
+--------------------+-------------+-------------------------------------------------------+-----------------------------------+-------+-------+--------+
| LZOH               | —           | ln(surface roughness length for heat)                 | ln(m)                             | I     | 234   | SFC    |
+--------------------+-------------+-------------------------------------------------------+-----------------------------------+-------+-------+--------+
| LCC                | —           | Low cloud cover                                       | 0–1                               | I     | 186   | SFC    |
+--------------------+-------------+-------------------------------------------------------+-----------------------------------+-------+-------+--------+
| MX2T               | —           | Maximum temperature at 2m                             | K                                 | I     | 201   | SFC    |
+--------------------+-------------+-------------------------------------------------------+-----------------------------------+-------+-------+--------+
| MSLP               | —           | Mean sea level pressure                               | Pa                                | I     | 151   | SFC    |
+--------------------+-------------+-------------------------------------------------------+-----------------------------------+-------+-------+--------+
| MCC                | —           | Medium cloud cover                                    | 0–1                               | I     | 187   | SFC    |
+--------------------+-------------+-------------------------------------------------------+-----------------------------------+-------+-------+--------+
| MGWS               | —           | Meridional component of gravity wave stress           | Nm\ :math:`^{-2}`                 | A     | 196   | SFC    |
+--------------------+-------------+-------------------------------------------------------+-----------------------------------+-------+-------+--------+
| MN2T               | —           | Minimum temperature at 2m                             | K                                 | I     | 202   | SFC    |
+--------------------+-------------+-------------------------------------------------------+-----------------------------------+-------+-------+--------+
| NSSS               | NSSS        | North-South surface stress                            | Nm\ :math:`^{-2}`                 | A     | 181   | SFC    |
+--------------------+-------------+-------------------------------------------------------+-----------------------------------+-------+-------+--------+
| PAR                | PAR         | Photosynthetically active radiation                   | Wm\ :math:`^{-2}`                 | A     | 58    | SFC    |
+--------------------+-------------+-------------------------------------------------------+-----------------------------------+-------+-------+--------+
|                    |             | at the surface                                        |                                   |       |       |        |
+--------------------+-------------+-------------------------------------------------------+-----------------------------------+-------+-------+--------+
| RO                 | —           | Runoff                                                | m                                 | A     | 205   | SFC    |
+--------------------+-------------+-------------------------------------------------------+-----------------------------------+-------+-------+--------+
| SSTK               | —           | Sea surface temperature-Kelvin                        | K                                 | I     | 34    | SFC    |
+--------------------+-------------+-------------------------------------------------------+-----------------------------------+-------+-------+--------+
| CI                 | CI          | Sea-ice cover                                         | 0–1                               | I     | 31    | SFC    |
+--------------------+-------------+-------------------------------------------------------+-----------------------------------+-------+-------+--------+
| SRC                | —           | Skin reservoir content                                | m of water                        | I     | 198   | SFC    |
+--------------------+-------------+-------------------------------------------------------+-----------------------------------+-------+-------+--------+
| SKT                | —           | Skin temperature                                      | K                                 | I     | 235   | SFC    |
+--------------------+-------------+-------------------------------------------------------+-----------------------------------+-------+-------+--------+
| ASN                | —           | Snow albedo                                           | 0–1                               | I     | 32    | SFC    |
+--------------------+-------------+-------------------------------------------------------+-----------------------------------+-------+-------+--------+
| RSN                | —           | Snow density                                          | kgm\ :math:`^{-3}`                | I     | 33    | SFC    |
+--------------------+-------------+-------------------------------------------------------+-----------------------------------+-------+-------+--------+
| SD                 | SD          | Snow depth                                            | m of water                        | I     | 141   | SFC    |
+--------------------+-------------+-------------------------------------------------------+-----------------------------------+-------+-------+--------+
|                    |             |                                                       | equivalent                        |       |       |        |
+--------------------+-------------+-------------------------------------------------------+-----------------------------------+-------+-------+--------+
| ES                 | ES          | Snow evaporation                                      | m of water                        | A     | 44    | SFC    |
+--------------------+-------------+-------------------------------------------------------+-----------------------------------+-------+-------+--------+
| SF                 | SF          | Snowfall (conv. + strat.)                             | m of water                        | A     | 144   | SFC    |
+--------------------+-------------+-------------------------------------------------------+-----------------------------------+-------+-------+--------+
|                    |             |                                                       | equivalent                        |       |       |        |
+--------------------+-------------+-------------------------------------------------------+-----------------------------------+-------+-------+--------+
| SMLT               | SMLT        | Snowmelt                                              | m of water                        | A     | 45    | SFC    |
+--------------------+-------------+-------------------------------------------------------+-----------------------------------+-------+-------+--------+
| STL1\ :math:`^*`   | —           | Soil temperature level 1                              | K                                 | I     | 139   | SFC    |
+--------------------+-------------+-------------------------------------------------------+-----------------------------------+-------+-------+--------+
|                    |             | :math:`^*`\ Erroneously called SLT1 on netCDF files   |                                   |       |       |        |
+--------------------+-------------+-------------------------------------------------------+-----------------------------------+-------+-------+--------+
| STL2               | —           | Soil temperature level 2                              | K                                 | I     | 170   | SFC    |
+--------------------+-------------+-------------------------------------------------------+-----------------------------------+-------+-------+--------+
| STL3               | —           | Soil temperature level 3                              | K                                 | I     | 183   | SFC    |
+--------------------+-------------+-------------------------------------------------------+-----------------------------------+-------+-------+--------+
| STL4               | —           | Soil temperature level 4                              | K                                 | I     | 236   | SFC    |
+--------------------+-------------+-------------------------------------------------------+-----------------------------------+-------+-------+--------+
| SUND               | —           | Sunshine duration                                     | 1                                 | A     | 189   | SFC    |
+--------------------+-------------+-------------------------------------------------------+-----------------------------------+-------+-------+--------+

| \|l\|l\|l\|l\|c\|c\|c\| Field & CTM & & & & UiO & UiO
| name & name &Description & Unit & I/A & ID & file
| SLHF & SLH & Surface latent heat flux & W m\ :math:`^{-2}` & A & 147 &
  SFC
| SNSRCS &—& Surface net solar radiation, clear sky & W m\ :math:`^{-2}`
  & A & 210 & SFC
| SNTRCS &—& Surface net thermal radiation, clear sky & W
  m\ :math:`^{-2}` & A & 211 & SFC
| pres\_sfc & P & Surface pressure & hPa & I & 152 & SPE
| & & UiO format: ln(Pa) & & & &
| SSHF & SHF & Surface sensible heat flux & W m\ :math:`^{-2}` & A & 146
  & SFC
| SSR &—& Surface solar radiation & W m\ :math:`^{-2}` & A & 176 & SFC
| SSRD & SSRD\ :math:`^{**}` & Surface solar radiation downwards & W
  m\ :math:`^{-2}` & A & 169 & SFC
| STR &—& Surface thermal radiation & W m\ :math:`^{-2}` & A & 177 & SFC
| STRD & STRD\ :math:`^{**}`\ & Surface thermal radiation downwards &
  Wm\ :math:`^{-2}` & A & 177 & SFC
| TSN &—& Temperature of snow layer & K & I & 238 & SFC
| TSRC &—& Top net solar radiation, clear sky & Wm\ :math:`^{-2}` & A &
  208 & SFC
| TTRC &—& Top net thermal radiation, clear sky & Wm\ :math:`^{-2}` & A
  & 209 & SFC
| TSR &—& Top solar radiation & Wm\ :math:`^{-2}` & A & 178 & SFC
| TTR &—& Top thermal radiation & Wm\ :math:`^{-2}` & A & 179 & SFC
| TCC &—& Total cloud cover & 0–1 & I & 164 & SFC
| TCIW &—& Total column ice water & kgm\ :math:`^{-2}` & I & 79 & SFC
| TCLW &—& Total column liquid water & kgm\ :math:`^{-2}` & I & 78 & SFC
| TCW &—& Total column water & kgm\ :math:`^{-2}` & I & 136 & SFC
| TCWV &—& Total column water vapor & kgm\ :math:`^{-2}` & I & 137 & SFC
| SWVL1 & SWVL1\ :math:`^{**}` & Volumetric soil water layer 1 & m
  m\ :math:`^{-3}` & I & 39 & SFC
| SWVL2 &—& Volumetric soil water layer 2 & m m\ :math:`^{-3}` & I & 40
  & SFC
| SWVL3 &—& Volumetric soil water layer 3 & m m\ :math:`^{-3}` & I & 41
  & SFC
| SWVL4 &—& Volumetric soil water layer 4 & m m\ :math:`^{-3}` & I & 42
  & SFC
| — & SFQ & Specific humidity at surface & kg/kg & I &—&—
| — & USTR & Friction velocity & m/s & I &—&—

.. [1]
   http://www.ess.uci.edu/\ :math:`\sim`\ prather/

.. [2]
   Nederlandse Organisatie voor Toegepast Natuurwetenschappelijk
   Onderzoek

.. [3]
   Nederlandse Organisatie voor Toegepast Natuurwetenschappelijk
   Onderzoek

.. [4]
   (CH:math:`_3`)\ :math:`_2`\ S

.. [5]
   CH\ :math:`_3`\ SO\ :math:`_3`\ H

.. [6]
   http://dust.ess.uci.edu/dead/

.. [7]
   Schedule instruction so that hardware is effectively used

.. [8]
   Makes the processor work all the time, avoids waiting

.. [9]
   E.g. guesses results of if-tests and starts calculations in advance

.. [10]
   Include subroutines directly into code

.. [11]
   Grabs code and starts calculating instead of waiting for the
   calculation to arrive

.. [12]
   Checks if any calculation can be replaced by constants
