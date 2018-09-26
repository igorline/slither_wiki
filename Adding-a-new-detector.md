Slither's plugin architecture lets you integrate new detectors that run from the command line.

The skeleton for a detector is:

```python
from slither.detectors.abstract_detector import AbstractDetector, DetectorClassification


class Skeleton(AbstractDetector):
    """
    Documentation
    """

    ARGUMENT = 'mydetector' # slither will launch the detector with slither.py --mydetector
    HELP = 'Help printed by slither'
    IMPACT = DetectorClassification.HIGH
    CONFIDENCE = DetectorClassification.HIGH

    def detect(self):
        return []
```

- `ARGUMENT` lets you run the detector from the command line
- `HELP` is the information printed from the command line
- `IMPACT` indicates the impact of the issue. Allowed values are:
  - `DetectorClassification.INFORMATIONAL`: printed in green
  - `DetectorClassification.LOW`: printed in green
  - `DetectorClassification.MEDIUM`: printed in yellow
  - `DetectorClassification.HIGH`: printed in red
- `CONFIDENCE` indicates your confidence in the analysis. Allowed values are:
  - `DetectorClassification.LOW`
  - `DetectorClassification.MEDIUM`
  - `DetectorClassification.HIGH`

`detect()` needs to return a list of findings. To facilitate the automation of Slither, a finding is a dictionary containing a `vuln` key associated with the vulnerability name and additional information according to the vulnerability itself.

An `AbstractDetector` object has the `slither` attribute, which returns the current `Slither` object, and the `log(str)` function to print the result.

For example, [backdoor.py](https://github.com/trailofbits/slither/blob/67907575492b199d98282c2f05bbd941c26f780c/slither/detectors/examples/backdoor.py) will detect any function with `backdoor` in its name.

You will find a skeleton example to integrate your detector into slither in [slither/plugin_example](https://github.com/trailofbits/slither/tree/67907575492b199d98282c2f05bbd941c26f780c/plugin_example).
