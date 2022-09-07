# Licensing

## Background

Licensing of current (and future) model components:

| **Component** | **Licence** | **Comment**|
|--------------|-----------|-------------|
| CICE5        | Open source |[Bespoke licence](https://github.com/CICE-Consortium/CICE-svn-trunk/blob/main/LICENSE.pdf) allows redistribution and use with some restriction on making code publicly available  |
| MOM5     | GNU LGPLv3    | [Upgraded to LGPLv3](https://github.com/mom-ocean/MOM5/issues/370) |
| MOM6     | GNU LGPLv3    | Changed to LGPL in 2017 to allow use in a LGPLv3 coupled setup |
| FMS      | GNU LGPLv3    | GFDL internal coupler infrastructure used by MOM5 | 
| UM       | Restricted    | Australian research use licensed as part of [UM Partnership](https://www.metoffice.gov.uk/research/approach/collaboration/unified-model/partnership) |
| CABLE    | [CSIRO Open Source Software Licence Agreement](https://trac.nci.org.au/trac/cable/wiki/license) | Conditions: any redistribution must retain the copyright notice and list of conditions and disclaimers.|
| MCT      | Open source   | Bespoke licence. Allow redistribution if [source copyright notice retained](https://github.com/MCSclimate/MCT/blob/master/LICENSE) |
| OASIS-MCT| LGPL     | No version specified. Redistribution allowed   with reproduction of [licence](https://gitlab.com/cerfacs/oasis3-mct/-/blob/OASIS3-MCT_5.0/LICENSE) |

Licensing of current models:


|  **Model**     |  **Licence**   | **Comment** |
|---------------|-------------|-------------|
| ACCESS-OM2 | None | [Raised this as an issue](https://github.com/COSIMA/access-om2/issues/264) |
| ACCESS-CM2    | ???  |  |
| ACCESS-ESM1.5 | ???  |  |

An example of how to approach copyright and licensing with a complex
environment using many different codes is given in the [CESM Copyright
and Terms of Use](https://www.cesm.ucar.edu/models/cesm2/copyright.html).

## Decision Required

The first decision for new source code projects created by ACCESS-NRI is
to choose between a ["Copyleft" licenses](https://en.wikipedia.org/wiki/Copyleft) and ["Permissive" licenses](https://en.wikipedia.org/wiki/Permissive_software_license). Then a more focused discussion can choose the specific license.

A permissive licence leads to a lower burden on the community. Using a
copyleft license requires them to understand their obligations under
that licence if they use and redistribute NRI code. It may also align
better with Australian Government and NCRIS goals with respect to
commercial use.
