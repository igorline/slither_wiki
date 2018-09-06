Slither has a detectors plugin architecture so that you can integrate your detector in slither, and run it by default from the command line.

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

- _ARGUMENT_ allows to run the detector from the command line
- _HELP_ is the information printed from the command line
- _CLASSIFICATION_  indicates your confidence in the severity and precision of the issues, it can be:
  - _DetectorClassification.LOW_
  - _DetectorClassification.MEDIUM_
  - _DetectorClassification.HIGH_

_LOW_ vulnerabilities will be printed in green, _MEDIUM_ in yellow and _HIGH_ in red.

_detect()_ needs to return a list of finding. To facilitate the automatization of slither, a finding is a dictionary containing a _vuln_ key associated to the vulnerability name, and additional information, according of the vulnerability itself.

An _AbstractDetector_ object has the _slither_ attribute, which return the current _Slither_ object, and the _log(str)_ function, allowing to print the result.

For example, [backdoor.py](https://github.com/trailofbits/slither/blob/f47c4385db33c09d26e3dc67b20d58ec80995f91/slither/detectors/examples/backdoor.py) will detect any function with `backdoor`in its name.

In addition, you need to load the module in slither in [slither/detectors/detectors.py](https://github.com/trailofbits/slither/blob/f47c4385db33c09d26e3dc67b20d58ec80995f91/slither/detectors/detectors.py). For example, `backdoor.py` is loaded [here](https://github.com/trailofbits/slither/blob/f47c4385db33c09d26e3dc67b20d58ec80995f91/slither/detectors/detectors.py#L9).
