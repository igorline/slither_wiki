Slither's plugin architecture lets you integrate new detectors that run from the command line.

# Detector Skeleton

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

# Integration

You can integrate your detector into Slither by:
- Adding it in [slither/detectors](https://github.com/trailofbits/slither/tree/20b8fdb7bc9227abe3c9c3a769f59eb5d1338849/slither/detectors) and in [__main__.py#L92-L108](https://github.com/trailofbits/slither/blob/20b8fdb7bc9227abe3c9c3a769f59eb5d1338849/slither/__main__.py#L92-L108)
- or, by creating a plugin package (see the [skeleton example](https://github.com/trailofbits/slither/tree/0d1bbbebad52affcc8f6ee5855ab16e3b6bbbc74/plugin_example)).

## Test the detector
If you want your detector to be added in [trailofbits/slither](https://github.com/trailofbits/slither), create a unit-test in [tests](https://github.com/trailofbits/slither/tree/master/tests) and update [scripts/travis_test.sh](https://github.com/trailofbits/slither/blob/master/scripts/travis_test.sh#L56) to run the unit-test automatically.

# Example
[backdoor.py](https://github.com/trailofbits/slither/blob/0d1bbbebad52affcc8f6ee5855ab16e3b6bbbc74/slither/detectors/examples/backdoor.py) will detect any function with `backdoor` in its name.
