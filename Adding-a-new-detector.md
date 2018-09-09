Slither's plugin architecture lets you integrate new detectors that run from the command line.

The skeleton for a detector is:

```python
from slither.detectors.abstractDetector import AbstractDetector
from slither.detectors.detectorClassification import DetectorClassification

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

- `ARGUMENT` allows to run the detector from the command line
- `HELP` is the information printed from the command line
- `CLASSIFICATION` indicates your confidence in the severity and precision of the issues. Allowed values are:
  - `DetectorClassification.LOW`: printed in green
  - `DetectorClassification.MEDIUM`: printed in yellow
  - `DetectorClassification.HIGH`: printed in red

`detect()` needs to return a list of findings. To facilitate the automation of Slither, a finding is a dictionary containing a `vuln` key associated to the vulnerability name and additional information according of the vulnerability itself.

An `AbstractDetector` object has the `slither` attribute, which returns the current `Slither` object, and the `log(str)` function to print the result.

For example, [backdoor.py](https://github.com/trailofbits/slither/blob/f47c4385db33c09d26e3dc67b20d58ec80995f91/slither/detectors/examples/backdoor.py) will detect any function with `backdoor` in its name.

In addition, you need to load the module in Slither in [slither/detectors/detectors.py](https://github.com/trailofbits/slither/blob/f47c4385db33c09d26e3dc67b20d58ec80995f91/slither/detectors/detectors.py). For example, `backdoor.py` is loaded [here](https://github.com/trailofbits/slither/blob/f47c4385db33c09d26e3dc67b20d58ec80995f91/slither/detectors/detectors.py#L9).
