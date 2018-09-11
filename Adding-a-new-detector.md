Slither's plugin architecture lets you integrate new detectors that run from the command line.

The skeleton for a detector is:

```python
from slither.detectors.abstract_detector import AbstractDetector, DetectorClassification


class Skeloton(AbstractDetector):
    """
    Documentation
    """

    ARGUMENT = 'mydetector' # slither will launch the detector with slither.py --mydetector
    HELP = 'Help printed by slither'
    CLASSIFICATION = DetectorClassification.HIGH

    def detect(self):
        return []
```

- `ARGUMENT` lets you run the detector from the command line
- `HELP` is the information printed from the command line
- `CLASSIFICATION` indicates your confidence in the impact of the issue. Allowed values are:
  - `DetectorClassification.LOW`: printed in green
  - `DetectorClassification.MEDIUM`: printed in yellow
  - `DetectorClassification.HIGH`: printed in red

`detect()` needs to return a list of findings. To facilitate the automation of Slither, a finding is a dictionary containing a `vuln` key associated with the vulnerability name and additional information according to the vulnerability itself.

An `AbstractDetector` object has the `slither` attribute, which returns the current `Slither` object, and the `log(str)` function to print the result.

For example, [backdoor.py](https://github.com/trailofbits/slither/blob/d3265490ea2d92033d83c8f3d9fc8fdb7f3d60f4/slither/detectors/examples/backdoor.py) will detect any function with `backdoor` in its name.

In addition, you need to load the module in Slither in [slither/__main.py](https://github.com/trailofbits/slither/blob/d3265490ea2d92033d83c8f3d9fc8fdb7f3d60f4/slither/__main__.py#L65-L71).
